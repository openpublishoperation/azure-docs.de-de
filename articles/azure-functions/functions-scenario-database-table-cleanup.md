---
title: Verwenden von Azure Functions zum Ausführen eines Datenbank-Bereinigungstasks| Microsoft-Dokumentation
description: Verwenden Sie Azure Functions, um eine Aufgabe zu planen, die eine Verbindung mit Azure SQL-Datenbank herstellt, um regelmäßig Zeilen zu bereinigen.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: a257948c97437d6045f705acb02054928d22ff89
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092868"
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Verwenden von Azure Functions zum Herstellen einer Verbindung mit einer Azure SQL-Datenbank-Instanz
In diesem Thema wird gezeigt, wie Sie mit Azure Functions einen geplanten Auftrag erstellen, der Zeilen in einer Tabelle in einer Azure SQL-Datenbank-Instanz bereinigt. Die neue C#-Skriptfunktion wird basierend auf einer vordefinierten Vorlage für einen Timertrigger im Azure-Portal erstellt. Zur Unterstützung dieses Szenarios müssen Sie auch eine Datenbank-Verbindungszeichenfolge als App-Einstellung in der Funktions-App festlegen. In diesem Szenario wird ein Massenvorgang auf die Datenbank angewendet. 

Damit Ihre Funktion einzelne Vorgänge wie das Erstellen, Lesen, Aktualisieren und Löschen in einer Mobile Apps-Tabelle verarbeiten kann, sollten Sie stattdessen die [Mobile Apps-Bindungen](functions-bindings-mobile-apps.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

+ In diesem Thema wird eine Funktion mit Auslösung per Timer verwendet. Führen Sie die Schritte im Thema [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](functions-create-scheduled-function.md) durch, um eine C#-Version dieser Funktion zu erstellen.   

+ In diesem Thema wird ein Transact-SQL-Befehl veranschaulicht, der einen Massenbereinigungsvorgang in der Tabelle **SalesOrderHeader** in der Beispieldatenbank „AdventureWorksLT“ ausführt. Um die Beispieldatenbank „AdventureWorksLT“ zu erstellen, führen Sie die Schritte im Thema [Erstellen einer Azure SQL-Datenbank-Instanz im Azure-Portal](../sql-database/sql-database-get-started-portal.md) aus. 

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen

Sie müssen die Verbindungszeichenfolge für die Datenbank abrufen, die Sie in [Erstellen einer Azure SQL-Datenbank-Instanz im Azure-Portal](../sql-database/sql-database-get-started-portal.md) erstellt haben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
 
3. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken** und anschließend auf der Seite **SQL-Datenbanken** Ihre Datenbank aus.

4. Wählen Sie **Datenbank-Verbindungszeichenfolgen anzeigen** aus, und kopieren Sie die vollständige **ADO.NET**-Verbindungszeichenfolge. 

    ![Kopieren Sie die ADO.NET-Verbindungszeichenfolge.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Festlegen der Verbindungszeichenfolge 

Eine Funktions-App hostet die Ausführung Ihrer Funktionen in Azure. Es ist eine bewährte Methode, Verbindungszeichenfolgen und andere geheime Schlüssel in den Einstellungen Ihrer Funktionen-App zu speichern. Die Verwendung von Anwendungseinstellungen verhindert eine versehentliche Offenlegung der Verbindungszeichenfolge mit dem Code. 

1. Navigieren Sie zu Ihrer Funktions-App, die Sie in [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](functions-create-scheduled-function.md) erstellt haben.

2. Wählen Sie **Plattformfeatures** > **Anwendungseinstellungen** aus.
   
    ![Anwendungseinstellungen für Funktions-App](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Scrollen Sie nach unten bis zu **Verbindungszeichenfolgen**, und fügen Sie eine Verbindungszeichenfolge mit den Einstellungen in der Tabelle hinzu.
   
    ![Fügen Sie den Funktions-App-Einstellungen eine Verbindungszeichenfolge hinzu.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Einstellung       | Empfohlener Wert | Beschreibung             | 
    | ------------ | ------------------ | --------------------- | 
    | **Name**  |  sqldb_connection  | Wird verwendet, um auf die gespeicherte Verbindungszeichenfolge im Funktionscode zuzugreifen    |
    | **Wert** | Kopierte Zeichenfolge  | Fügen Sie die Verbindungszeichenfolge, die Sie im vorherigen Abschnitt kopiert haben, ein, und ersetzen Sie die Platzhalter `{your_username}` und `{your_password}` durch die tatsächlichen Werte. |
    | **Typ** | SQL-Datenbank | Verwenden Sie die Standardverbindung mit SQL-Datenbank. |   

3. Klicken Sie auf **Speichern**.

Nun können Sie den C#-Funktionscode hinzufügen, der eine Verbindung mit Ihrer SQL-Datenbank herstellt.

## <a name="update-your-function-code"></a>Aktualisieren Ihres Funktionscodes

1. Wählen Sie im Portal in Ihrer Funktions-App die Funktion mit Timertrigger aus.
 
3. Fügen Sie am Anfang des vorhandenen C#-Skriptfunktionscodes die folgenden Assemblyverweise hinzu:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```
    >[!NOTE]
    >Beim Code in diesen Beispielen handelt es sich um C#-Skripts aus dem Portal. Wenn Sie eine vorkompilierte C#-Funktion lokal entwickeln, müssen Sie stattdessen Verweise auf diese Assemblys in Ihrem lokalen Projekt hinzufügen.  

3. Fügen Sie der Funktion die folgenden `using` -Anweisungen hinzu:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Ersetzen Sie die vorhandene `Run`-Funktion durch den folgenden Code:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    Dieser Beispielbefehl aktualisiert die `Status`-Spalte basierend auf dem Versanddatum. Es sollten 32 Zeilen mit Daten aktualisiert werden.

5. Klicken Sie auf **Speichern**, überwachen Sie das Fenster **Protokolle** auf die nächste Funktionsausführung, und beachten Sie die Anzahl der Zeilen, die in der Tabelle **SalesOrderHeader** aktualisiert wurden.

    ![Zeigen Sie die Funktionsprotokolle an.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich als Nächstes darüber, wie Sie Functions mit Logic Apps für die Integration in andere Dienste verwenden.

> [!div class="nextstepaction"] 
> [Erstellen einer Funktion, die in Logic Apps integriert ist](functions-twitter-email.md)

Weitere Informationen zu Functions finden Sie in den folgenden Themen:

* [Azure Functions developer reference (Azure Functions-Entwicklerreferenz) (Azure Functions-Entwicklerreferenz)](functions-reference.md)  
  Referenz zum Programmieren von Funktionen sowie zum Festlegen von Triggern und Bindungen.
* [Testing Azure Functions (Testen von Azure Functions) (Testen von Azure Functions)](functions-test-a-function.md)  
  Beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen  
