---
title: Prüfpunkte und Wiedergabe in Durable Functions – Azure
description: Es wird beschrieben, wie das Setzen von Prüfpunkten und die Wiedergabe in der Erweiterung „Durable Functions“ für Azure Functions funktioniert.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 5336c0a510f0be7d548d1d549b5b763c12e700b2
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987714"
---
# <a name="checkpoints-and-replay-in-durable-functions-azure-functions"></a>Prüfpunkte und Wiedergabe in Durable Functions (Azure Functions)

Ein Schlüsselattribut von Durable Functions ist die **zuverlässige Ausführung**. Orchestratorfunktionen und Aktivitätsfunktionen können in einem Rechenzentrum auf unterschiedlichen VMs ausgeführt werden, und diese VMs bzw. die zugrunde liegende Netzwerkinfrastruktur sind nicht hundertprozentig zuverlässig.

Trotzdem wird mit Durable Functions die zuverlässige Ausführung von Orchestrierungen sichergestellt. Hierfür werden Speicherwarteschlangen für den Funktionsaufruf verwendet und regelmäßig Prüfpunkte für den Ausführungsverlauf in Speichertabellen gesetzt (mit einem Cloudentwurfsmuster, das als [Ereignisherkunftsermittlung](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) bezeichnet wird). Dieser Verlauf kann dann wiedergegeben werden, um den In-Memory-Status einer Orchestratorfunktion automatisch wiederherzustellen.

## <a name="orchestration-history"></a>Orchestrierungsverlauf

Angenommen, Sie verfügen über die folgende Orchestratorfunktion:

#### <a name="c"></a>C#

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (nur Functions v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const output = [];
    output.push(yield context.df.callActivity("E1_SayHello", "Tokyo"));
    output.push(yield context.df.callActivity("E1_SayHello", "Seattle"));
    output.push(yield context.df.callActivity("E1_SayHello", "London"));

    return output;
});
```

Bei jeder `await`-Anweisung (C#) oder `yield`-Anweisung (JavaScript) setzt das Durable Task Framework für den Ausführungszustand der Funktion im Tabellenspeicher einen Prüfpunkt. Dieser Zustand wird als *Orchestrierungsverlauf* bezeichnet.

## <a name="history-table"></a>Verlaufstabelle

Im Allgemeinen führt das Durable Task Framework an jedem Prüfpunkt Folgendes durch:

1. Der Ausführungsverlauf wird in Azure Storage-Tabellen gespeichert.
2. Die Nachrichten für Funktionen, die der Orchestrator aufrufen möchte, werden in die Warteschlange eingereiht.
3. Nachrichten, die für den Orchestrator selbst bestimmt sind – z.B. Nachrichten des permanenten Timers –, werden in die Warteschlange eingereiht.

Nachdem der Prüfpunktvorgang abgeschlossen ist, kann die Orchestratorfunktion aus dem Arbeitsspeicher entfernt werden, bis weitere Arbeitsschritte damit ausgeführt werden müssen.

> [!NOTE]
> Azure Storage gibt keine Transaktionsgarantie für das Speichern von Daten in Tabellenspeicher und Warteschlangen. Zur Behandlung von Ausfällen nutzt der Anbieter von Durable Functions-Speicher Muster vom Typ *Letztliche Konsistenz*. Mit diesen Mustern wird sichergestellt, dass keine Daten verloren gehen, wenn es zu einem Absturz kommt oder während des Prüfpunktprozesses die Verbindung getrennt wird.

Nach Abschluss des Vorgangs sieht der Verlauf der obigen Funktion in Azure Table Storage etwa wie folgt aus (zu Darstellungszwecken gekürzt):

| PartitionKey (InstanceId)                     | EventType             | Zeitstempel               | Eingabe | NAME             | Ergebnis                                                    | Status | 
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|---------------------| 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362Z |       |                  |                                                           |                     | 
| eaee885b | ExecutionStarted      | 2017-05-05T18:45:28.852Z | null  | E1_HelloSequence |                                                           |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:32.670Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:32.670Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.201Z |       |                  | """Hello Tokyo!"""                                        |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.435Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.435Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.763Z |       |                  | """Hello Seattle!"""                                      |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.857Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.919Z |       |                  | """Hello London!"""                                       |                     | 
| eaee885b | ExecutionCompleted    | 2017-05-05T18:45:35.044Z |       |                  | "[""Hello Tokyo!"",""Hello Seattle!"",""Hello London!""]" | Abgeschlossen           | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:35.044Z |       |                  |                                                           |                     | 

Einige Hinweise zu den Spaltenwerten:
* **PartitionKey**: Enthält die Instanz-ID der Orchestrierung.
* **EventType**: Steht für den Typ des Ereignisses. Es kann sich um einen der folgenden Typen handeln:
    * **OrchestrationStarted**: Die Orchestratorfunktion wurde aus einem Wartezustand fortgesetzt oder wird zum ersten Mal ausgeführt. Die Spalte `Timestamp` wird verwendet, um den deterministischen Wert für die [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime)-API aufzufüllen.
    * **ExecutionStarted**: Die Ausführung der Orchestratorfunktion wurde zum ersten Mal gestartet. Dieses Ereignis enthält auch die Funktionseingabe in der Spalte `Input`.
    * **TaskScheduled**: Eine Aktivitätsfunktion wurde geplant. Der Name der Aktivitätsfunktion wird in der Spalte `Name` erfasst.
    * **TaskCompleted**: Eine Aktivitätsfunktion wurde abgeschlossen. Das Ergebnis der Funktion ist in der Spalte `Result` enthalten.
    * **TimerCreated**: Ein permanenter Timer wurde erstellt. Die Spalte `FireAt` enthält die geplante UTC-Uhrzeit, zu der der Timer abläuft.
    * **TimerFired**: Ein permanenter Timer wurde ausgelöst.
    * **EventRaised**: Ein externes Ereignis wurde an die Orchestrierungsinstanz gesendet. In der Spalte `Name` wird der Name des Ereignisses und in der Spalte `Input` die Nutzlast des Ereignisses erfasst.
    * **OrchestratorCompleted**: Die Orchestratorfunktion befindet sich im Wartezustand.
    * **ContinueAsNew**: Die Orchestratorfunktion wurde abgeschlossen und mit einem neuen Zustand neu gestartet. Die Spalte `Result` enthält den Wert, der als Eingabe in der neu gestarteten Instanz verwendet wird.
    * **ExecutionCompleted**: Die Orchestratorfunktion wurde bis zum Abschluss (oder Fehler) ausgeführt. Die Ausgaben der Funktion bzw. die Fehlerdetails werden in der Spalte `Result` gespeichert.
* **Timestamp**: Der UTC-Zeitstempel des Verlaufsereignisses.
* **Name**: Der Name der Funktion, die aufgerufen wurde.
* **Input**: Die Eingabe der Funktion im JSON-Format.
* **Result**: Die Ausgabe der Funktion, also ihr Rückgabewert.

> [!WARNING]
> Die Tabelle ist zwar als Debugtool nützlich, aber Sie sollten keine Abhängigkeiten dafür einrichten. Dies kann sich im Rahmen der Weiterentwicklung der Erweiterung Durable Functions ändern.

Jedes Mal, wenn die Funktion aus einem `await`-Zustand fortgesetzt wird, führt das Durable Task Framework die Orchestratorfunktion von Grund auf neu aus. Bei jeder erneuten Ausführung wird der Ausführungsverlauf herangezogen, um zu ermitteln, ob der aktuelle asynchrone Vorgang durchgeführt wurde.  Wenn ja, gibt das Framework die Ausgabe dieses Vorgangs sofort wieder und fährt mit dem nächsten `await`-Element fort. Dieser Prozess wird fortgesetzt, bis der gesamte Verlauf wiedergegeben wurde. An diesem Punkt werden alle lokalen Variablen in der Orchestratorfunktion auf ihre vorherigen Werte wiederhergestellt.

## <a name="orchestrator-code-constraints"></a>Einschränkungen des Orchestratorcodes

Mit dem Wiedergabeverhalten sind Einschränkungen des Codetyps verbunden, der in einer Orchestratorfunktion geschrieben werden kann:

* Der Orchestratorcode muss **deterministisch** sein. Er wird mehrfach wiedergegeben und muss jedes Mal zum gleichen Ergebnis führen. Beispielsweise keine direkten Aufrufe zum Abrufen des aktuellen Datums bzw. der Uhrzeit, Abrufen von Zufallszahlen, Generieren von zufälligen GUIDs oder Aufrufen von Remoteendpunkten.

  Wenn der Orchestratorcode das aktuelle Datum bzw. die Uhrzeit abrufen muss, sollte hierfür die [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime)-API genutzt werden, mit der die Wiedergabe sicher ist.

  Nicht deterministische Vorgänge müssen in Aktivitätsfunktionen durchgeführt werden. Dies schließt alle Interaktionen mit anderen Eingabe- oder Ausgabebindungen ein. So wird sichergestellt, dass alle nicht deterministischen Werte bei der ersten Ausführung einmal generiert und im Ausführungsverlauf gespeichert werden. Bei den nachfolgenden Ausführungen wird der gespeicherte Wert dann automatisch verwendet.

* Der Orchestratorcode sollte **nicht blockierend** sein. Das bedeutet beispielsweise keine E/A und keine Aufrufe an `Thread.Sleep` oder entsprechende APIs.

  Wenn für einen Orchestrator eine Verzögerung erforderlich ist, kann die [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_)-API genutzt werden.

* Orchestratorcode darf **niemals einen asynchronen Vorgang initiieren**, sofern nicht die [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)-API verwendet wird. `Task.Run`, `Task.Delay` oder `HttpClient.SendAsync` sind beispielsweise nicht zulässig. Das Durable Task Framework führt Orchestratorcode über einen einzelnen Thread aus und kann nicht mit anderen Threads interagieren, die von anderen asynchronen APIs geplant werden können.

* **Endlosschleifen sollten im Orchestratorcode vermieden werden**. Da das Durable Task Framework den Ausführungsverlauf speichert, während die Orchestrierungsfunktion voranschreitet, kann eine Endlosschleife unter Umständen dazu führen, dass für eine Orchestratorinstanz der Arbeitsspeicher nicht mehr ausreicht. Verwenden Sie für Szenarien mit Endlosschleifen APIs wie beispielsweise [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_), um die Funktionsausführung neu zu starten und den vorherigen Ausführungsverlauf zu verwerfen.

Diese Einschränkungen können zuerst etwas abschreckend wirken, aber in der Praxis ist ihre Einhaltung nicht sehr schwierig. Das Durable Task Framework versucht, Verstöße gegen die obigen Regeln zu erkennen, und löst eine `NonDeterministicOrchestrationException` aus. Dieses Erkennungsverhalten wird aber jeweils nur auf bestmögliche Weise durchgeführt, sodass Sie sich nicht darauf verlassen sollten.

> [!NOTE]
> Diese Regeln gelten jeweils nur für Funktionen, die von der Bindung `orchestrationTrigger` ausgelöst werden. Für Aktivitätsfunktionen, die von der Bindung `activityTrigger` ausgelöst werden, und Funktionen, für die die Bindung `orchestrationClient` verwendet wird, gelten diese Einschränkungen nicht.

## <a name="durable-tasks"></a>Durable Tasks (Permanente Vorgänge)

> [!NOTE]
> In diesem Abschnitt werden Details der internen Implementierung für das Durable Task Framework beschrieben. Sie können Durable Functions nutzen, ohne diese Informationen zu kennen. Sie sollen Ihnen nur als Hilfe beim Verständnis des Wiedergabeverhaltens dienen.

Aufgaben, die in Orchestratorfunktionen auf sichere Weise erwartet werden können, werden auch als *Durable Tasks* (Permanente Aufgaben) bezeichnet. Dies sind Aufgaben, die mit dem Durable Task Framework erstellt und verwaltet werden. Beispiele hierfür sind die Aufgaben, die von `CallActivityAsync`, `WaitForExternalEvent` und `CreateTimer` zurückgegeben werden können.

Diese *permanenten Aufgaben* werden intern anhand einer Liste mit `TaskCompletionSource`-Objekten verwaltet. Während der Wiedergabe werden diese Aufgaben im Rahmen der Ausführung des Orchestratorcodes erstellt und durchgeführt, wenn der Verteiler die entsprechenden Verlaufsereignisse auflistet. Dies erfolgt synchron mit einem einzelnen Thread, bis der gesamte Verlauf wiedergegeben wurde. Für alle permanenten Aufgaben, die am Ende der Verlaufswiedergabe nicht abgeschlossen wurden, werden entsprechende Aktionen durchgeführt. Beispielsweise kann eine Nachricht in die Warteschlange eingereiht werden, um eine Aktivitätsfunktion aufzurufen.

Das hier beschriebene Ausführungsverhalten soll Ihnen verdeutlichen, warum Orchestratorfunktionscode keinen `await`-Vorgang für eine nicht permanente Aufgabe durchführen darf: Der Verteilerthread kann nicht auf den Abschluss der Durchführung warten, und alle Rückrufe dieser Aufgabe können ggf. eine Beschädigung des Nachverfolgungsstatus der Orchestratorfunktion nach sich ziehen. Es können Laufzeitüberprüfungen verwendet werden, um zu versuchen, dies zu verhindern.

Falls Sie weitere Informationen dazu benötigen, wie Orchestratorfunktionen im Durable Task Framework ausgeführt werden, hilft Innen der Artikel zum [Durable Task-Quellcode auf GitHub](https://github.com/Azure/durabletask) weiter. Sehen Sie sich vor allem [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) und [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs) an.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Instanzverwaltung](durable-functions-instance-management.md)
