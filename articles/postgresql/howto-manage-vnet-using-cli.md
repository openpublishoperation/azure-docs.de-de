---
title: Erstellen und Verwalten von VNet-Dienstendpunkten und -regeln für Azure Database for PostgreSQL mithilfe der Azure CLI | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI-Befehlszeile VNet-Dienstendpunkte und -regeln für Azure Database for PostgreSQL erstellen und verwalten.
services: postgresql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 08/15/2018
ms.openlocfilehash: 6f69c412aa7322326205e32b5df35aed2faa4649
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971517"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-using-azure-cli"></a>Erstellen und Verwalten von VNet-Dienstendpunkten für Azure Database for PostgreSQL mithilfe der Azure CLI
VNet-Dienstendpunkte und -regeln (Virtual Network) erweitern den privaten Adressraum eines virtuellen Netzwerks auf Ihren Azure Database for PostgreSQL-Server. Dank praktischer Azure Command Line Interface-Befehle (CLI) können Sie zum Verwalten Ihres Servers VNet-Dienstendpunkte und -regeln erstellen, aktualisieren, löschen, auflisten und anzeigen. Eine Übersicht über die VNet-Dienstendpunkte für Azure Database for PostgreSQL, einschließlich Einschränkungen, finden Sie unter [Azure Database for PostgreSQL Server VNet service endpoints (VNet-Dienstendpunkte für Azure Database for PostgreSQL Server)](concepts-data-access-and-security-vnet.md). VNET-Dienstendpunkte sind in allen unterstützten Regionen für Azure Database for PostgreSQL verfügbar.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli) oder verwenden Sie alternativ Azure Cloud Shell im Browser.
- Einen [Server und eine Datenbank für Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).

> [!NOTE]
> VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>Konfigurieren von Vnet-Dienstendpunkten für Azure Database for PostgreSQL
Die [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest)-Befehle werden zum Konfigurieren von virtuellen Netzwerken verwendet.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie den Befehl `az --version` aus, um die installierte Version anzuzeigen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). 

Wenn Sie die CLI lokal ausführen, müssen Sie sich mit dem Befehl [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) an Ihrem Konto anmelden. Beachten Sie die Eigenschaft **id** aus der Befehlsausgabe für den entsprechenden Abonnementnamen.
```azurecli-interactive
az login
```

Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource fakturiert sein sollte. Wählen Sie mithilfe des Befehls [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set) die Abonnement-ID unter Ihrem Konto aus. Ersetzen Sie den Platzhalter für die Abonnement-ID durch die Eigenschaft **id** aus der Ausgabe von **az login** für Ihr Abonnement.

- Das Konto muss über die erforderlichen Berechtigungen zum Erstellen eines virtuellen Netzwerks und eines Dienstendpunkts verfügen.

Dienstendpunkte können unabhängig für virtuelle Netzwerke konfiguriert werden, der Benutzer benötigt hierzu Schreibzugriff auf das virtuelle Netzwerk.

Zum Schützen der Ressourcen von Azure-Diensten in einem VNET muss der Benutzer die Berechtigung „Microsoft.Network/JoinServicetoaSubnet“ für die hinzuzufügenden Subnetze haben. Diese Berechtigung ist standardmäßig in die integrierten Dienstadministratorrollen integriert und kann durch die Erstellung von benutzerdefinierten Rollen geändert werden.

Erfahren Sie mehr über [integrierte Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

VNETs und Ressourcen von Azure-Diensten können sich in demselben oder in unterschiedlichen Abonnements befinden. Wenn das VNET und die Ressourcen von Azure-Diensten in unterschiedlichen Abonnements enthalten sind, sollten sich die Ressourcen unter demselben Active Directory-Mandanten (AD) befinden.

> [!IMPORTANT]
> Es wird dringend empfohlen, diesen Artikel zu Dienstendpunktkonfigurationen und -überlegungen zu lesen, bevor Sie das unten aufgeführte Beispielskript ausführen oder Dienstendpunkte konfigurieren. **Dienstendpunkte im virtuellen Netzwerk:** Ein [Dienstendpunkt im virtuellen Netzwerk](../virtual-network/virtual-network-service-endpoints-overview.md) ist ein Subnetz, dessen Eigenschaftswerte mindestens einen formalen Azure-Diensttypnamen enthalten. VNET-Dienstendpunkte verwenden den Diensttypnamen **Microsoft.Sql**, der auf den Azure-Dienst „SQL-Datenbank“ verweist. Dieses Diensttag gilt auch für die Dienste Azure SQL-Datenbank, Azure Database for PostgreSQL und Azure Database for MySQL. Bitte beachten Sie bei der Anwendung des **Microsoft.Sql**-Diensttags auf einem VNet-Dienstendpunkt die Konfiguration des Dienstendpunktverkehrs für alle Azure-Datenbankdienste, einschließlich Azure SQL-Datenbank, Azure Database for PostgreSQL und Azure Database for MySQL-Server im Subnetz. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Beispielskript für die Erstellung einer Azure Database for PostgreSQL-Datenbank, zur Erstellung eines VNets, eines VNet-Dienstendpunkts und für die Sicherung des Servers mit einer VNet-Regel im Subnetz
Ändern Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen. Ersetzen Sie die SubscriptionID im Befehl `az account set --subscription` durch Ihre eigene Abonnement-ID.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]
