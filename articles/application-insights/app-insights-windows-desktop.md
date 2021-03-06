---
title: Überwachen der Nutzung und Leistung bei Windows-Desktop-Apps
description: Analysieren Sie die Nutzung und Leistung Ihrer Windows-Desktop-App mit Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: mbullwin
ms.openlocfilehash: 755cf2e9b99cbb443157d1aead2e3391eb4ccdba
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094591"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Überwachen der Nutzung und Leistung von klassischen Windows-Desktop-Apps

Lokal, in Azure oder in anderen Clouds gehostete Anwendungen können Application Insights nutzen. Die einzige Einschränkung besteht darin, dass die [Kommunikation](app-insights-ip-addresses.md) mit dem Application Insights-Dienst zugelassen werden muss. Für die Überwachung von UWP-Anwendungen (universelle Windows-Plattform) wird [Visual Studio App Center](app-insights-mobile-center-quickstart.md) empfohlen.

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>So senden Sie Telemetriedaten aus einer klassischen Windows-Anwendung an Application Insights
1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) [eine Application Insights-Ressource](app-insights-create-new-resource.md). Wählen Sie als Anwendungstyp "ASP.NET-App" aus.
2. Erstellen Sie eine Kopie des Instrumentierungsschlüssels. Diesen finden Sie in der Dropdownliste „Essentials“ der neuen Ressource, die Sie gerade erstellt haben. 
3. Bearbeiten Sie die NuGet-Pakete Ihres App-Projekts in Visual Studio, und fügen Sie ihnen „Microsoft.ApplicationInsights.WindowsServer“ hinzu. (Oder wählen Sie „Microsoft.ApplicationInsights“ aus, wenn Sie nur die reine API ohne die Standardmodule zur Telemetrieerfassung verwenden möchten.)
4. Legen Sie den Instrumentierungsschlüssel im Code fest:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "`*Ihr Schlüssel*`";`
   
    Oder legen Sie ihn in der Datei „ApplicationInsights.config“ fest (wenn Sie eines der Standardtelemetriepakete installiert haben):
   
    `<InstrumentationKey>`*Ihr Schlüssel*`</InstrumentationKey>` 
   
    Wenn Sie die Datei „ApplicationInsights.config“ verwenden, stellen Sie sicher, dass ihre Eigenschaften im Projektmappen-Explorer auf **Buildvorgang = Inhalt, In Ausgabeverzeichnis kopieren = Kopieren**festgelegt sind.
5. [Verwenden Sie die API](app-insights-api-custom-events-metrics.md) , um Telemetriedaten zu senden.
6. Führen Sie die App aus, und schauen Sie sich die Telemetriedaten in der Ressource an, die Sie im Azure-Portal erstellt haben.

## <a name="telemetry"></a>Beispielcode
```csharp

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Dashboards](app-insights-dashboards.md)
* [Diagnosesuche](app-insights-diagnostic-search.md)
* [Untersuchen von Metriken](app-insights-metrics-explorer.md)
* [Schreiben von Analytics-Abfragen](app-insights-analytics.md)

