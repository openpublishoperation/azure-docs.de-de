---
title: Includedatei
description: Includedatei
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/30/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 19352df7abff23ed44521a11e7907c84c8c0327f
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "33835832"
---
In diesem Abschnitt senden Sie Neuigkeiten als Vorlagenbenachrichtigungen mit Tags über eine .NET-Konsolen-App. 

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolenanwendung:
   
      ![Link „Konsolenanwendung“][13]

2. Wählen Sie im Hauptmenü von Visual Studio die Optionen **Extras** > **Bibliothekspaket-Manager** > **Paket-Manager-Konsole**, und geben Sie im Konsolenfenster die folgende Zeichenfolge ein:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
3. Drücken Sie die **EINGABETASTE**.  
    Dadurch wird mithilfe des [Microsoft.Azure.NotificationHubs-NuGet-Pakets] ein Verweis auf das Azure Notification Hubs-SDK hinzugefügt.

4. Öffnen Sie die Datei „Program.cs“, und fügen Sie die folgende `using`-Anweisung hinzu:
   
    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```

5. Fügen Sie in der `Program` -Klasse die folgende Methode hinzu, oder ersetzen Sie sie, falls sie bereits vorhanden ist:
   
    ```csharp
    private static async void SendTemplateNotificationAsync()
    {
        // Define the notification hub.
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");

        // Create an array of breaking news categories.
        var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};

        // Send the notification as a template notification. All template registrations that contain
        // "messageParam" and the proper tags will receive the notifications.
        // This includes APNS, GCM, WNS, and MPNS template registrations.

        Dictionary<string, string> templateParams = new Dictionary<string, string>();

        foreach (var category in categories)
        {
            templateParams["messageParam"] = "Breaking " + category + " News!";
            await hub.SendTemplateNotificationAsync(templateParams, category);
        }
    }
    ```   
   
    Dieser Code sendet eine Vorlagenbenachrichtigung für jedes der sechs Tags im Zeichenfolgenarray. Durch die Verwendung von Tags wird sichergestellt, dass Geräte nur Benachrichtigungen für die registrierten Kategorien erhalten.

5. Ersetzen Sie im obigen Code die Platzhalter `<hub name>` und `<connection string with full access>` durch den Namen Ihres Notification Hubs und die Verbindungszeichenfolge für *DefaultFullSharedAccessSignature* aus dem Dashboard für Ihren Notification Hub.

6. Fügen Sie in der **Main**-Methode die folgenden Zeilen hinzu:
   
    ```csharp
    SendTemplateNotificationAsync();
    Console.ReadLine();
    ```

7. Erstellen Sie die Konsolenanwendung.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Get started with Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs REST interface]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Add push notifications for Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[How to use Notification Hubs from Java or PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.NotificationHubs-NuGet-Pakets]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
