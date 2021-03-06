---
title: Azure Application Insights – Automatisches Sammeln von Abhängigkeiten | Microsoft-Dokumentation
description: Mit Application Insights können Abhängigkeiten automatisch gesammelt und visualisiert werden.
services: application-insights
documentationcenter: .net
author: nikmd23
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: reference
ms.date: 08/13/2018
ms.reviewer: mbullwin
ms.author: nimolnar
ms.openlocfilehash: f20963f030c9040b696f7d6a33b25bcee2dc517f
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "40130274"
---
# <a name="dependency-auto-collection"></a>Automatisches Sammeln von Abhängigkeiten

Unten ist die Liste mit den derzeit unterstützten Abhängigkeitsaufrufen angegeben, die automatisch als Abhängigkeiten erkannt werden, ohne dass am Code Ihrer Anwendung zusätzliche Änderungen erforderlich sind. Es sind ausgehende Aufrufe von Kommunikationsbibliotheken, Speicherclients, Protokollierungs- und Metrikbibliotheken sowie eingehende Aufrufe von Anwendungsframeworks und Servern aufgeführt. Diese Abhängigkeiten werden in Application Insights in den Ansichten [Anwendungsübersicht](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) und [Transaktionsdiagnose](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) visualisiert. Falls Ihre Abhängigkeit in der Liste unten nicht angegeben ist, können Sie sie per [TrackDependency-Aufruf](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency) trotzdem manuell nachverfolgen.

## <a name="net"></a>.NET

| App-Frameworks| Versionen |
| ------------------------|----------|
| ASP.NET-Webformulare | 4.5 und höher |
| ASP.NET MVC | 4 und höher |
| ASP.NET WebAPI | 4.5 und höher |
| ASP.NET Core | 1.1 und höher |
| <b>Kommunikationsbibliotheken</b> |
| [HttpClient](https://www.microsoft.com/net/) | 4.5 und höher, .NET Core 1.1 und höher |
| [SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) | .NET Core 1.0+, NuGet 4.3.0 |
| [EventHubs-Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 |
| [ServiceBus-Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) | 3.0.0 |
| <b>Speicherclients</b>|  |
| ADO.NET | 4.5 und höher |
| <b>Protokollierungsbibliotheken</b> |  |
| ILogger | 1.1 und höher |
| System.Diagnostics.Trace | 4.5 und höher |
| [nLog](https://www.nuget.org/packages/NLog/) | 4.4.12 und höher |
| [log4net](https://www.nuget.org/packages/log4net/) | 2.0.8 und höher unter NetStandard 1.3, 2.0.6 und höher unter .NET 4.5 und höher |

## <a name="java"></a>Java
| App-Server | Versionen |
|-------------|----------|
| [Tomcat](https://tomcat.apache.org/) | 7, 8 | 
| [JBoss EAP](https://developers.redhat.com/products/eap/download/) | 6, 7 |
| [Jetty](http://www.eclipse.org/jetty/) | 9 |
| <b>App-Frameworks</b> |  |
| [Spring](https://spring.io/) | 3.0 |
| [Spring Boot](https://spring.io/projects/spring-boot) | 1.5.9 und höher<sup>*</sup> |
| Java-Servlet | 3.1 und höher |
| <b>Kommunikationsbibliotheken</b> |  |
| [Apache-HTTP-Client](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) | 4.3 und höher<sup>†</sup> |
| <b>Speicherclients</b> | |
| [SQL Server]( https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc) | 1 und höher<sup>†</sup> |
| [Oracle]( http://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html) | 1 und höher<sup>†</sup> |
| [MySql]( https://mvnrepository.com/artifact/mysql/mysql-connector-java) | 1 und höher<sup>†</sup> |
| <b>Protokollierungsbibliotheken</b> | |
| [Logback](https://logback.qos.ch/) | 1 und höher |
| [Log4j](https://logging.apache.org/log4j/) | 1.2 und höher |
| <b>Metrikbibliotheken</b> |  |
| JMX | 1.0 und höher |

> [!NOTE]
> * Mit Ausnahme der Unterstützung für reaktive Programmierung.
> <br>†Installation des [JVM-Agents](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent#install-the-application-insights-agent-for-java) erforderlich.

## <a name="nodejs"></a>Node.js

| Kommunikationsbibliotheken | Versionen |
| ------------------------|----------|
| [HTTP](https://nodejs.org/api/http.html), [HTTPS](https://nodejs.org/api/https.html) | 0.10 und höher |
| <b>Speicherclients</b> | |
| [Redis](https://www.npmjs.com/package/redis) | 2.x |
| [MongoDb](https://www.npmjs.com/package/mongodb); [MongoDb Core](https://www.npmjs.com/package/mongodb-core) | 2.0.0 - 2.3.0 |
| [MySQL](https://www.npmjs.com/package/mysql) | 2.0.0 - 2.14.x |
| [PostgreSql](https://www.npmjs.com/package/pg); | 6.x |
| [pg-pool](https://www.npmjs.com/package/pg-pool) | 1.x |
| <b>Protokollierungsbibliotheken</b> | |
| [Konsole](https://nodejs.org/api/console.html) | 0.10 und höher |
| [Bunyan](https://www.npmjs.com/package/bunyan) | 1.x |
| [Winston](https://www.npmjs.com/package/winston) | 2.x |

## <a name="javascript"></a>JavaScript

| Kommunikationsbibliotheken | Versionen |
| ------------------------|----------|
| [XMLHttpRequest](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) | Alle |

## <a name="next-steps"></a>Nächste Schritte

- Einrichten der benutzerdefinierten Abhängigkeitsnachverfolgung für [.NET](app-insights-asp-net-dependencies.md)
- Einrichten der benutzerdefinierten Abhängigkeitsnachverfolgung für [Java](app-insights-java-agent.md)
- [Schreiben benutzerdefinierter Telemetriedaten zu Abhängigkeiten](app-insights-api-custom-events-metrics.md#trackdependency)
- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](application-insights-data-model.md).
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](app-insights-platforms.md).
