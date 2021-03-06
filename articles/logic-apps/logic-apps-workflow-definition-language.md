---
title: Schemareferenz zur Definitionssprache für Workflows – Azure Logic Apps | Microsoft-Dokumentation
description: Schreiben benutzerdefinierter Workflowdefinitionen für Azure Logic Apps mit der Definitionssprache für Workflows
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 04/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: d5dadd054f95e61626942a1cab7d95ba8c9182e1
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42140166"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Schemareferenz zur Definitionssprache für Workflows in Azure Logic Apps

Beim Erstellen eines Logic-App-Workflows mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) beschreibt die Ihrem Workflow zugrunde liegende Definition die eigentliche Logik, die für Ihre Logik-App ausgeführt wird. Diese Beschreibung folgt einer Struktur, die vom Schema der Definitionssprache für Workflows definiert und überprüft wird und [JavaScript Object Notation (JSON)](https://www.json.org/) verwendet. 
  
## <a name="workflow-definition-structure"></a>Struktur der Workflowdefinition

Eine Workflowdefinition verfügt über mindestens einen Trigger, der Ihre Logik-App instanziiert, sowie über mindestens eine Aktion, die Ihre Logik-App ausführt. 

So sieht die allgemeine Struktur einer Workflowdefinition aus:  
  
```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "contentVersion": "<workflow-definition-version-number>",
  "parameters": { "<workflow-parameter-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" },
  "actions": { "<workflow-action-definitions>" },
  "outputs": { "<workflow-output-definitions>" }
}
```
  
| Element | Erforderlich | BESCHREIBUNG | 
|---------|----------|-------------| 
| Definition | JA | Das Startelement für Ihre Workflowdefinition | 
| $schema | Nur bei externem Verweis auf eine Workflowdefinition | Der Speicherort der JSON-Schemadatei, welche die Version der Workflowdefinitionssprache beschreibt. Der Speicherort lautet wie folgt: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| contentVersion | Nein  | Die Versionsnummer für Ihre Workflowdefinition, die standardmäßig „1.0.0.0“ lautet. Geben Sie einen zu verwendenden Wert an, um bei der Bereitstellung eines Workflows die richtige Definition besser ermitteln und bestätigen zu können. | 
| Parameter | Nein  | Die Definitionen für mindestens einen Parameter, der Daten an Ihren Workflow übergibt <p><p>Maximale Anzahl von Parametern: 50 | 
| Trigger | Nein  | Die Definitionen für mindestens einen Trigger, der Ihren Workflow instanziiert. Sie können mehrere Trigger definieren, jedoch nur mit der Definitionssprache für Workflows, nicht visuell mit dem Designer für Logik-Apps. <p><p>Maximale Anzahl von Triggern: 10 | 
| Aktionen | Nein  | Die Definitionen für mindestens eine Aktion, die zur Workflowlaufzeit ausgeführt werden soll <p><p>Maximale Anzahl von Aktionen: 250 | 
| outputs | Nein  | Die Definitionen für die Ausgaben, die von einer Workflowausführung zurückgegeben werden <p><p>Maximale Anzahl von Ausgaben: 10 |  
|||| 

## <a name="parameters"></a>Parameter

Im Abschnitt `parameters` werden alle Workflowparameter definiert, die Ihre Logik-App bei der Bereitstellung zur Annahme von Eingaben verwendet. Bei der Bereitstellung sind sowohl Parameterdeklarationen als auch Parameterwerte erforderlich. Bevor Sie diese Parameter in anderen Workflowabschnitten verwenden können, müssen Sie sicherstellen, dass Sie alle Parameter in diesen Abschnitten deklarieren. 

So sieht die allgemeine Struktur einer Parameterdefinition aus:  

```json
"parameters": {
  "<parameter-name>": {
    "type": "<parameter-type>",
    "defaultValue": "<default-parameter-value>",
    "allowedValues": [ <array-with-permitted-parameter-values> ],
    "metadata": { 
      "key": { 
        "name": "<key-value>"
      } 
    }
  }
},
```

| Element | Erforderlich | Typ | BESCHREIBUNG |  
|---------|----------|------|-------------|  
| type | JA | int, float, string, securestring, bool, array, JSON-Objekt, secureobject <p><p>**Hinweis**: Verwenden Sie für sämtliche Kennwörter, Schlüssel und geheimen Schlüssel die Typen `securestring` und `secureobject`, da diese Typen beim `GET`-Vorgang nicht zurückgegeben werden. | Der Typ des Parameters |
| defaultValue | Nein  | Identisch mit `type` | Der Standardparameterwert, wenn bei der Instanziierung des Workflows kein Wert angegeben wird | 
| allowedValues | Nein  | Identisch mit `type` | Ein Array mit Werten, die vom Parameter akzeptiert werden können |  
| metadata | Nein  | JSON-Objekt | Alle anderen von Visual Studio oder anderen Tools verwendeten Parameterdetails, wie z.B. der Name, eine lesbare Beschreibung Ihrer Logik-App oder Entwurfszeitdaten |  
||||

## <a name="triggers-and-actions"></a>Trigger und Aktionen  

In einer Workflowdefinition werden in den Abschnitten `triggers` und `actions` sämtliche Aufrufe definiert, die während der Ausführung Ihres Workflows auftreten. Informationen zur Syntax sowie weitere Informationen zu diesen Abschnitten finden Sie unter [Workflowtrigger und -aktionen](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Ausgaben 

Definieren Sie im Abschnitt `outputs` die Daten, die Ihr Workflow nach Abschluss der Ausführung zurückgeben kann. Wenn Sie beispielsweise einen bestimmten Status oder Wert aus den einzelnen Ausführungen nachverfolgen möchten, müssen Sie angeben, dass diese Daten bei der Workflowausgabe zurückgegeben werden. 

> [!NOTE]
> Verwenden Sie nicht `outputs`, wenn Sie über die REST-API eines Diensts auf eingehende Anforderungen reagieren. Verwenden Sie stattdessen den Aktionstyp `Response`. Weitere Informationen finden Sie unter [Workflowtrigger und -aktionen](../logic-apps/logic-apps-workflow-actions-triggers.md).

So sieht die allgemeine Struktur einer Ausgabedefinition aus: 

```json
"outputs": {
  "<key-name>": {  
    "type": "<key-type>",  
    "value": "<key-value>"  
  }
} 
```

| Element | Erforderlich | Typ | BESCHREIBUNG | 
|---------|----------|------|-------------| 
| <*key-name*> | JA | Zeichenfolge | Der Schlüsselname des Rückgabewerts der Ausgabe |  
| type | JA | int, float, string, securestring, bool, array, JSON-Objekt | Der Typ des Rückgabewerts der Ausgabe | 
| value | JA | Identisch mit `type` | Der Rückgabewert der Ausgabe |  
||||| 

Zum Abrufen der Ausgabe aus einer Workflowausführung müssen Sie im Azure-Portal den Ausführungsverlauf der Logik-App sowie Details überprüfen oder die [REST-API des Workflows](https://docs.microsoft.com/rest/api/logic/workflows) verwenden. Sie können die Ausgabe auch an externe Systeme übergeben, z.B. Power BI, um Dashboards erstellen zu können. 

<a name="expressions"></a>

## <a name="expressions"></a>Ausdrücke

Mit JSON können Sie über Literalwerte verfügen, die zur Entwurfszeit vorhanden sind. Beispiel:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

Sie können auch über Werte verfügen, die erst zur Laufzeit vorhanden sind. Zur Darstellung dieser Werte können Sie *Ausdrücke* verwenden, die zur Laufzeit ausgewertet werden. Ein Ausdruck ist eine Sequenz, die mindestens eine [Funktion](#functions), einen [Operator](#operators), eine Variable, einen expliziten Wert oder eine Konstante enthalten kann. In Ihrer Workflowdefinition können Sie einen Ausdruck an einer beliebigen Stelle in einem JSON-Zeichenfolgenwert verwenden, indem Sie dem Ausdruck das at-Zeichen voranstellen (\@). Beim Auswerten eines Ausdrucks, der einen JSON-Wert darstellt, wird der Ausdruckskörper durch Entfernen des \@-Zeichens extrahiert. Dies führt immer zu einem anderen JSON-Wert. 

Für die zuvor definierte Eigenschaft `customerName` können Sie den Eigenschaftswert in einem Ausdruck beispielsweise über die Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) abrufen und diesen Wert der Eigenschaft `accountName` zuweisen:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

Durch die *Zeichenfolgeninterpolation* können Sie auch mehrere Ausdrücke in Zeichenfolgen verwenden, die vom \@-Zeichen und geschweiften Klammern ({}) umschlossen sind. Die Syntax sieht wie folgt aus:

```json
@{ "<expression1>", "<expression2>" }
```

Das Ergebnis ist immer eine Zeichenfolge, wodurch diese Funktion der Funktion `concat()` ähnelt. Beispiel: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Wenn Sie über eine Zeichenfolgenliteral verfügen, das mit dem \@-Zeichen beginnt, stellen Sie dem \@-Zeichen ein weiteres \@-Zeichen als Escapezeichen voran: \@\@.

Diese Beispiele veranschaulichen die Auswertung von Ausdrücken:

| JSON-Wert | Ergebnis |
|------------|--------| 
| "Sophia Owen" | Folgende Zeichen werden zurückgegeben: „Sophia Owen“ |
| "array[1]" | Folgende Zeichen werden zurückgegeben: „array[1]“ |
| „\@\@“ | Folgende Zeichen werden als Zeichenfolge mit einem Zeichen zurückgegeben: „\@“ |   
| " \@" | Folgende Zeichen werden als Zeichenfolge mit zwei Zeichen zurückgegeben: „\@“ |
|||

Angenommen, für "myBirthMonth" ist der Monat "January" und für "myAge" die Zahl 42 definiert:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Diese Beispiele veranschaulichen, wie die folgenden Ausdrücke ausgewertet werden:

| JSON-Ausdruck | Ergebnis |
|-----------------|--------| 
| „\@parameters('myBirthMonth')“ | Folgende Zeichenfolge wird zurückgeben: „January“ |  
| „\@{parameters('myBirthMonth')}“ | Folgende Zeichenfolge wird zurückgeben: „January“ |  
| „\@parameters('myAge')“ | Folgende Zahl wird zurückgeben: 42 |  
| „\@{parameters('myAge')}“ | Folgende Zahl wird als Zeichenfolge zurückgegeben: „42“ |  
| „My age is \@{parameters('myAge')}“ | Folgende Zeichenfolge wird zurückgegeben: „My age is 42“ |  
| „\@concat('My age is ', string(parameters('myAge')))“ | Folgende Zeichenfolge wird zurückgegeben: „My age is 42“ |  
| „My age is \@\@{parameters('myAge')}“ | Folgende Zeichenfolge, die den Ausdruck enthält, wird zurückgegeben: „My age is \@{parameters('myAge')}“ | 
||| 

Wenn Sie im Designer für Logik-Apps visuell arbeiten, können Sie über den Ausdrucks-Generator Ausdrücke erstellen. Beispiel: 

![Designer für Logik-Apps > Ausdrucks-Generator](./media/logic-apps-workflow-definition-language/expression-builder.png)

Wenn Sie fertig sind, wird der Ausdruck für die entsprechende Eigenschaft in Ihrer Workflowdefinition angezeigt, wie im folgenden Beispiel für die Eigenschaft `searchQuery`:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
       "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="operators"></a>

## <a name="operators"></a>Operatoren

In [Ausdrücken](#expressions) und [Funktionen](#functions) führen Operatoren bestimmte Tasks aus. Sie verweisen z.B. auf eine Eigenschaft oder einen Wert in einem Array. 

| Operator | Aufgabe | 
|----------|------|
| ' | Wenn Sie ein Zeichenfolgenliteral als Eingabe oder in Ausdrücken und Funktionen verwenden möchten, umschließen Sie die Zeichenfolge nur mit einfachen Anführungszeichen. Beispiel: `'<myString>'`. Verwenden Sie keine doppelten Anführungszeichen (""). Dies könnte zu einem Konflikt mit der JSON-Formatierung eines ganzen Ausdrucks führen. Beispiel:  <p>**Ja**: length('Hello') </br>**Nein**: length("Hello") <p>Wenn Sie Arrays oder Zahlen übergeben, sind keine umschließenden Satzzeichen erforderlich. Beispiel:  <p>**Ja**: length([1, 2, 3]) </br>**Nein**: length("[1, 2, 3]") | 
| [] | Verwenden Sie eckige Klammern, um auf einen Wert an einer bestimmten Position (Index) in einem Array zu verweisen. So können Sie z.B. das zweite Element in einem Array abrufen: <p>`myArray[1]` | 
| . | Verwenden Sie den Punktoperator, um auf eine Eigenschaft in einem Objekt zu verweisen. So können Sie z.B. die Eigenschaft `name` für das JSON-Objekt `customer` abrufen: <p>`"@parameters('customer').name"` | 
| ? | Mit dem Fragezeichenoperator können Sie ohne Laufzeitfehler auf NULL-Eigenschaften in einem Objekt verweisen. Mithilfe des folgenden Ausdrucks können Sie beispielsweise NULL-Ausgaben eines Triggers verarbeiten: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>Functions

Einige Ausdrücke erhalten ihre Werte von Laufzeitaktionen, die zu Beginn der Ausführung einer Logik-App möglicherweise noch nicht vorhanden sind. Sie können mithilfe von [*Funktionen*](../logic-apps/workflow-definition-language-functions-reference.md), die von der Definitionssprache für Workflows bereitgestellt werden, auf diese Werte in Ausdrücken verweisen oder mit diesen Werten arbeiten. 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Aktionen und Triggern für die Definitionssprache für Workflows](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Weitere Informationen zum programmgesteuerten Erstellen und Verwalten von Logik-Apps mit der [Workflow-REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
