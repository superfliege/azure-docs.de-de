---
title: Schema der Definitionssprache für Workflows – Azure Logic Apps | Microsoft-Dokumentation
description: Schreiben benutzerdefinierter Workflowdefinitionen für Azure Logic Apps mit der Definitionssprache für Workflows
services: logic-apps
author: ecfan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 04/30/2018
ms.author: estfan
ms.openlocfilehash: 6a4e113c6816540e303210c3f1c96d81146cf5db
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300180"
---
# <a name="logic-apps-workflow-definitions-with-the-workflow-definition-language-schema"></a>Logic Apps-Workflowdefinitionen mit dem Schema der Definitionssprache für Workflows

Beim Erstellen eines Logic-App-Workflows mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) beschreibt die Ihrem Workflow zugrunde liegende Definition die eigentliche Logik, die für Ihre Logik-App ausgeführt wird. Diese Beschreibung folgt einer Struktur, die vom Schema der Definitionssprache für Workflows definiert und überprüft wird und das Format [JavaScript Object Notation (JSON)](https://www.json.org/) verwendet. 
  
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
| Definition | Ja | Das Startelement für Ihre Workflowdefinition | 
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
| type | Ja | int, float, string, securestring, bool, array, JSON-Objekt, secureobject <p><p>**Hinweis**: Verwenden Sie für sämtliche Kennwörter, Schlüssel und geheimen Schlüssel die Typen `securestring` und `secureobject`, da diese Typen beim `GET`-Vorgang nicht zurückgegeben werden. | Der Typ des Parameters |
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
| <*key-name*> | Ja | Zeichenfolge | Der Schlüsselname des Rückgabewerts der Ausgabe |  
| type | Ja | int, float, string, securestring, bool, array, JSON-Objekt | Der Typ des Rückgabewerts der Ausgabe | 
| value | Ja | Identisch mit `type` | Der Rückgabewert der Ausgabe |  
||||| 

Zum Abrufen der Ausgabe aus einer Workflowausführung müssen Sie im Azure-Portal den Ausführungsverlauf der Logik-App sowie Details überprüfen oder die [REST-API des Workflows](https://docs.microsoft.com/rest/api/logic/workflows) verwenden. Sie können die Ausgabe auch an externe Systeme übergeben, wie z.B. PowerBI, um Dashboards erstellen zu können. 

<a name="expressions"></a>

## <a name="expressions"></a>Ausdrücke

Mit JSON können Sie über Literalwerte verfügen, die zur Entwurfszeit vorhanden sind. Beispiel:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

Sie können auch über Werte verfügen, die erst zur Laufzeit vorhanden sind. Zur Darstellung dieser Werte können Sie *Ausdrücke* verwenden, die zur Laufzeit ausgewertet werden. Ein Ausdruck ist eine Sequenz, die mindestens eine [Funktion](#functions), einen [Operator](#operators), eine Variable, einen expliziten Wert oder eine Konstante enthalten kann. In Ihrer Workflowdefinition können Sie einen Ausdruck an einer beliebigen Stelle in einem JSON-Zeichenfolgenwert verwenden, indem Sie dem Ausdruck das at-Zeichen voranstellen (@). Beim Auswerten eines Ausdrucks, der einen JSON-Wert darstellt, wird der Ausdruckskörper durch Entfernen des @-Zeichens extrahiert. Dies führt immer zu einem anderen JSON-Wert. 

Für die zuvor definierte Eigenschaft `customerName` können Sie den Eigenschaftswert in einem Ausdruck beispielsweise über die Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) abrufen und diesen Wert der Eigenschaft `accountName` zuweisen:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

Durch die *Zeichenfolgeninterpolation* können Sie auch mehrere Ausdrücke in Zeichenfolgen verwenden, die vom @-Zeichen und geschweiften Klammern ({}) umschlossen sind. Die Syntax sieht wie folgt aus:

```json
@{ "<expression1>", "<expression2>" }
```

Das Ergebnis ist immer eine Zeichenfolge, wodurch diese Funktion der Funktion `concat()` ähnelt. Beispiel: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Wenn Sie über eine Zeichenfolgenliteral verfügen, das mit dem @-Zeichen beginnt, stellen Sie dem @-Zeichen ein weiteres @-Zeichen als Escapezeichen voran: @@.

Diese Beispiele veranschaulichen die Auswertung von Ausdrücken:

| JSON-Wert | Ergebnis |
|------------|--------| 
| "Sophia Owen" | Folgende Zeichen werden zurückgegeben: „Sophia Owen“ |
| "array[1]" | Folgende Zeichen werden zurückgegeben: „array[1]“ |
| "\@@\" | Folgende Zeichen werden als Zeichenfolge mit einem Zeichen zurückgegeben: „@“ |   
| \" \@\" | Folgende Zeichen werden als Zeichenfolge mit zwei Zeichen zurückgegeben: „@“ |
|||

Angenommen, für "myBirthMonth" ist der Monat "January" und für "myAge" die Zahl 42 definiert:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Diese Beispiele veranschaulichen, wie die folgenden Ausdrücke ausgewertet werden:

| JSON-Ausdruck | Ergebnis |
|-----------------|--------| 
| "@parameters('myBirthMonth')" | Folgende Zeichenfolge wird zurückgeben: „January“ |  
| "@{parameters('myBirthMonth')}" | Folgende Zeichenfolge wird zurückgeben: „January“ |  
| "@parameters('myAge')" | Folgende Zahl wird zurückgeben: 42 |  
| "@{parameters('myAge')}" | Folgende Zahl wird als Zeichenfolge zurückgegeben: „42“ |  
| "My age is @{parameters('myAge')}" | Folgende Zeichenfolge wird zurückgegeben: „My age is 42“ |  
| "@concat('My age is ', string(parameters('myAge')))" | Folgende Zeichenfolge wird zurückgegeben: „My age is 42“ |  
| "My age is @@{parameters('myAge')}" | Folgende Zeichenfolge, die den Ausdruck enthält, wird zurückgegeben: „My age is @{parameters('myAge')}“ | 
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
| zu erstellen und zu verwalten. | Verwenden Sie den Punktoperator, um auf eine Eigenschaft in einem Objekt zu verweisen. So können Sie z.B. die Eigenschaft `name` für das JSON-Objekt `customer` abrufen: <p>`"@parameters('customer').name"` | 
| ? | Mit dem Fragezeichenoperator können Sie ohne Laufzeitfehler auf NULL-Eigenschaften in einem Objekt verweisen. Mithilfe des folgenden Ausdrucks können Sie beispielsweise NULL-Ausgaben eines Triggers verarbeiten: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>Functions

Einige Ausdrücke erhalten ihre Werte von Laufzeitaktionen, die zu Beginn der Ausführung einer Logik-App möglicherweise noch nicht vorhanden sind. Sie können mithilfe von [*Funktionen*](../logic-apps/workflow-definition-language-functions-reference.md) auf diese Werte in Ausdrücken verweisen oder mit diesen Werten arbeiten. So können Sie beispielsweise mathematische Funktionen für Berechnungen verwenden, wie z.B. die Funktion [add()](../logic-apps/workflow-definition-language-functions-reference.md#add), welche die Summe aus ganzen Zahlen oder Gleitkommazahlen zurückgibt. Ausführliche Informationen zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).
Alternativ können Sie mehr über Funktionen und deren allgemeinen Zweck erfahren.

Im Folgenden finden Sie nur einige Beispielaufgaben, die mithilfe von Funktionen ausgeführt werden können: 

| Aufgabe | Funktionssyntax | Ergebnis | 
| ---- | --------------- | -------------- | 
| Gibt eine Zeichenfolge in Kleinbuchstaben zurück. | toLower('<*text*>') <p>Beispiel: toLower('Hello') | "hello" | 
| Gibt einen global eindeutigen Bezeichner (Globally Unique Identifier, GUID) zurück. | guid() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

Dieses Beispiel zeigt, wie Sie den Wert über den Parameter `customerName` abrufen und ihn mithilfe der Funktion [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) in einem Ausdruck der Eigenschaft `accountName` zuweisen können:

```json
"accountName": "@parameters('customerName')"
```

Im Folgenden werden einige weitere allgemeine Möglichkeiten für die Verwendung von Funktionen in Ausdrücken aufgeführt:

| Aufgabe | Funktionssyntax in einem Ausdruck | 
| ---- | -------------------------------- | 
| Führen Sie Aufgaben mit einem Element durch, indem Sie dieses Element an eine Funktion übergeben. | "@<*functionName*>(<*item*>)" | 
| 1. Rufen Sie den Wert von *parameterName* mit der geschachtelten Funktion `parameters()` ab. </br>2. Führen Sie Aufgaben mit dem Ergebnis durch, indem Sie diesen Wert an *functionName* übergeben. | "@<*functionName*>(parameters('<*parameterName*>'))" | 
| 1. Rufen Sie das Ergebnis der geschachtelten inneren Funktion *functionName* ab. </br>2. Übergeben Sie das Ergebnis an die äußere Funktion *functionName2*. | "@<*functionName2*>(<*functionName*>(<*item*>))" | 
| 1. Abrufen des Ergebnisses aus *functionName*. </br>2. Da das Ergebnis ein Objekt mit der Eigenschaft *propertyName* ist, rufen Sie den Eigenschaftswert ab. | "@<*functionName*>(<*item*>).<*propertyName*>" | 
||| 

Die Funktion `concat()` kann z.B. mindestens zwei Zeichenfolgenwerte als Parameter aufweisen. In dieser Funktion werden diese Zeichenfolgen in einer Zeichenfolge kombiniert. Sie können Zeichenfolgenliterale eingeben, wie z.B. „Sophia“ und „Owen“, um eine kombinierte Zeichenfolge („SophiaOwen“) zu erhalten:

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Alternativ können Sie Zeichenfolgenwerte von Parametern abrufen. In diesem Beispiel wird die Funktion `parameters()` in den einzelnen `concat()`-Parametern und den Parametern `firstName` und `lastName` verwendet. Anschließend übergeben Sie die resultierenden Zeichenfolgen an die Funktion `concat()`, um eine kombinierte Zeichenfolge zu erhalten, wie z.B. „SophiaOwen“:

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

Das Ergebnis wird in beiden Fällen und Beispielen der Eigenschaft `customerName` zugewiesen. 

Ausführliche Informationen zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).
Alternativ können Sie basierend auf dem allgemeinen Zweck der Funktionen mehr über diese erfahren.

<a name="string-functions"></a>

### <a name="string-functions"></a>Zeichenfolgenfunktionen

Für die Arbeit mit Zeichenfolgen können Sie folgende Zeichenfolgenfunktionen und auch einige [Sammlungsfunktionen](#collection-functions) verwenden. Zeichenfolgenfunktionen können nur in Zeichenfolgen verwendet werden. 

| Zeichenfolgenfunktion | Aufgabe | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Kombiniert mindestens zwei Zeichenfolgen miteinander und gibt die kombinierte Zeichenfolge zurück. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Überprüft, ob eine Zeichenfolge mit der angegebenen Teilzeichenfolge endet. | 
| [guid](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Generiert einen global eindeutigen Bezeichner (Globally Unique Identifier, GUID) als Zeichenfolge. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Gibt die Anfangsposition für eine Teilzeichenfolge zurück. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Gibt die Endposition für eine Teilzeichenfolge zurück. | 
| [replace](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Ersetzt eine Teilzeichenfolge durch die angegebene Zeichenfolge und gibt die aktualisierte Zeichenfolge zurück. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Gibt ein Array zurück, das sämtliche Zeichen aus einer Zeichenfolge enthält und in dem die einzelnen Zeichen mit einem bestimmten Trennzeichen getrennt werden. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Überprüft, ob eine Zeichenfolge mit einer bestimmten Teilzeichenfolge beginnt. | 
| [substring](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Gibt Zeichen aus einer Zeichenfolge zurück, beginnend mit der angegebenen Position. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Gibt eine Zeichenfolge in Kleinbuchstaben zurück. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Gibt eine Zeichenfolge in Großbuchstaben zurück. | 
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Entfernt führende und nachfolgende Leerzeichen aus einer Zeichenfolge und gibt die aktualisierte Zeichenfolge zurück. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Auflistungsfunktionen

Für die Arbeit mit Sammlungen (in der Regel Arrays, Zeichenfolgen und manchmal auch Bibliotheken) können Sie folgende Sammlungsfunktionen verwenden. 

| Sammlungsfunktion | Aufgabe | 
| ------------------- | ---- | 
| [contains](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Überprüft, ob eine Sammlung ein bestimmtes Element enthält. |
| [empty](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Überprüft, ob eine Sammlung leer ist. | 
| [first](../logic-apps/workflow-definition-language-functions-reference.md#first) | Gibt das erste Element aus einer Sammlung zurück. | 
| [intersection](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Gibt eine Sammlung zurück, die *nur* die gängigen Elemente aus den angegebenen Sammlungen enthält. | 
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | Gibt eine Zeichenfolge zurück, die *alle* Elemente aus einem Array enthält, getrennt durch das angegebene Zeichen. | 
| [last](../logic-apps/workflow-definition-language-functions-reference.md#last) | Gibt das letzte Element aus einer Sammlung zurück. | 
| [Länge](../logic-apps/workflow-definition-language-functions-reference.md#length) | Gibt die Anzahl der Elemente in einer Zeichenfolge oder einem Array zurück. | 
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Entfernt Elemente vom Anfang einer Sammlung und gibt *alle anderen* Elemente zurück. | 
| [take](../logic-apps/workflow-definition-language-functions-reference.md#take) | Gibt Elemente vom Anfang einer Sammlung zurück. | 
| [union](../logic-apps/workflow-definition-language-functions-reference.md#union) | Gibt eine Sammlung zurück, die *sämtliche* Elemente aus den angegebenen Sammlungen enthält. | 
||| 

<a name="comparison-functions"></a>

### <a name="comparison-functions"></a>Vergleichsfunktionen

Sie können für die Arbeit mit Bedingungen, das Vergleichen von Werten und Ausdrucksergebnissen oder das Auswerten verschiedener Logiken folgende Vergleichsfunktionen verwenden. Die vollständige Referenz zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).

| Vergleichsfunktion | Aufgabe | 
| ------------------- | ---- | 
| [and](../logic-apps/workflow-definition-language-functions-reference.md#and) | Überprüft, ob für sämtliche Ausdrücke der Wert „TRUE“ festgelegt ist. | 
| [equals](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Überprüft, ob beide Werte identisch sind. | 
| [greater](../logic-apps/workflow-definition-language-functions-reference.md#greater) | Überprüft, ob der erste Wert größer als der zweite ist. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | Überprüft, ob der erste Wert größer als oder gleich dem zweiten ist. | 
| [if](../logic-apps/workflow-definition-language-functions-reference.md#if) | Überprüft, ob ein Ausdruck gleich „true“ oder „false“ ist. Gibt abhängig vom Ergebnis einen angegebenen Wert zurück. | 
| [less](../logic-apps/workflow-definition-language-functions-reference.md#less) | Überprüft, ob der erste Wert kleiner als der zweite ist. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | Überprüft, ob der erste Wert kleiner als oder gleich dem zweiten ist. | 
| [not](../logic-apps/workflow-definition-language-functions-reference.md#not) | Überprüft, ob ein Ausdruck gleich „false“ ist. | 
| [or](../logic-apps/workflow-definition-language-functions-reference.md#or) | Überprüft, ob mindestens ein Ausdruck gleich „true“ ist. |
||| 

<a name="conversion-functions"></a>

### <a name="conversion-functions"></a>Konvertierungsfunktionen

Sie können mit den folgenden Konvertierungsfunktionen den Typ oder das Format eines Werts ändern. So können Sie beispielsweise einen Wert von einem booleschen Wert in einen ganzzahligen Wert ändern. Weitere Informationen darüber, wie Logic Apps Inhaltstypen während des Wechsels behandelt, finden Sie unter [Behandeln von Inhaltstypen](../logic-apps/logic-apps-content-type.md). Die vollständige Referenz zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).

| Konvertierungsfunktion | Aufgabe | 
| ------------------- | ---- | 
| [array](../logic-apps/workflow-definition-language-functions-reference.md#array) | Gibt ein Array aus einer einzelnen angegebenen Eingabe zurück. Für mehrere Eingaben siehe [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Gibt die base64-codierte Version für eine Zeichenfolge zurück. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | Gibt die Binärversion für eine base64-codierte Zeichenfolge zurück. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Gibt die Zeichenfolgenversion für eine base64-codierte Zeichenfolge zurück. | 
| [binary](../logic-apps/workflow-definition-language-functions-reference.md#binary) | Gibt die Binärversion für einen Eingabewert zurück. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | Gibt die Version des booleschen Werts für einen Eingabewert zurück. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Gibt ein Array aus mehreren Eingaben zurück. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | Gibt den Daten-URI für einen Eingabewert zurück. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Gibt die Binärversion für einen Daten-URI zurück. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | Gibt die Zeichenfolgenversion für einen Daten-URI zurück. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Gibt die Zeichenfolgenversion für eine base64-codierte Zeichenfolge zurück. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Gibt die Binärversion für einen Daten-URI zurück. | 
| [decodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Gibt eine Zeichenfolge zurück, die Escapezeichen durch decodierte Versionen ersetzt. | 
| [encodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Gibt eine Zeichenfolge zurück, die URL-unsichere Zeichen durch Escapezeichen ersetzt. | 
| [float](../logic-apps/workflow-definition-language-functions-reference.md#float) | Gibt eine Gleitkommazahl für einen Eingabewert zurück. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Gibt die Ganzzahlversion für eine Zeichenfolge zurück. | 
| [json](../logic-apps/workflow-definition-language-functions-reference.md#json) | Gibt den JSON-Typwert oder das JSON-Objekt (JSON = JavaScript Object Notation) für eine Zeichenfolge oder XML zurück. | 
| [string](../logic-apps/workflow-definition-language-functions-reference.md#string) | Gibt die Zeichenfolgenversion für einen Eingabewert zurück. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | Gibt die URI-codierte Version für einen Eingabewert zurück, indem URL-unsichere Zeichen durch Escapezeichen ersetzt werden. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | Gibt die Binärversion für eine URI-codierte Zeichenfolge zurück. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Gibt die Zeichenfolgenversion für eine URI-codierte Zeichenfolge zurück. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | Gibt die XML-Version für eine Zeichenfolge zurück. | 
||| 

<a name="math-functions"></a>

### <a name="math-functions"></a>Mathematische Funktionen

Sie können mithilfe der folgenden mathematischen Funktionen mit ganzen Zahlen und Gleitkommazahlen arbeiten. Die vollständige Referenz zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).

| Mathematische Funktion | Aufgabe | 
| ------------- | ---- | 
| [Hinzufügen](../logic-apps/workflow-definition-language-functions-reference.md#add) | Gibt das Ergebnis der Addition zweier Zahlen zurück. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Gibt das Ergebnis der Division zweier Zahlen zurück. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | Gibt den höchsten Wert aus einer Reihe von Zahlen oder einem Array zurück. | 
| [min](../logic-apps/workflow-definition-language-functions-reference.md#min) | Gibt den niedrigsten Wert aus einer Reihe von Zahlen oder einem Array zurück. | 
| [mod](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Gibt den Restwert aus der Division zweier Zahlen zurück. | 
| [mul](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Gibt das Produkt aus der Multiplikation zweier Zahlen zurück. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Gibt eine beliebige ganze Zahl aus einem angegebenen Bereich zurück. | 
| [range](../logic-apps/workflow-definition-language-functions-reference.md#range) | Gibt ein Array mit ganzen Zahlen zurück, das mit einer angegebenen ganzen Zahl beginnt. | 
| [sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | Gibt das Ergebnis aus der Subtraktion der zweiten Zahl von der ersten Zahl zurück. | 
||| 

<a name="date-time-functions"></a>

### <a name="date-and-time-functions"></a>Datums- und Uhrzeitfunktionen

Sie können für die Arbeit mit Datums- und Uhrzeitangaben folgende Datums- und Uhrzeitfunktionen verwenden.
Die vollständige Referenz zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).

| Datums- oder Zeitfunktion | Aufgabe | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Fügt eine Anzahl von Tagen zu einem Zeitstempel hinzu. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Fügt eine Anzahl von Stunden zu einem Zeitstempel hinzu. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Fügt eine Anzahl von Minuten zu einem Zeitstempel hinzu. | 
| [addSeconds](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Fügt eine Anzahl von Sekunden zu einem Zeitstempel hinzu. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Fügt eine Anzahl von Zeiteinheiten zu einem Zeitstempel hinzu. Siehe auch [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Konvertiert einen Zeitstempel von der UTC-Zeitzone (UTC = Universal Time, Coordinated) in die Zielzeitzone. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Konvertiert einen Zeitstempel von der Quellzeitzone in die Zielzeitzone. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Konvertiert einen Zeitstempel von der Quellzeitzone in die UTC-Zeitzone (UTC = Universal Time, Coordinated). | 
| [dayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | Gibt den Tag der Monatskomponente eines Zeitstempels zurück. | 
| [dayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | Gibt den Tag der Wochenkomponente eines Zeitstempels zurück. | 
| [dayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | Gibt den Tag der Jahreskomponente eines Zeitstempels zurück. | 
| [formatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Gibt das Datum eines Zeitstempels zurück. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Gibt den aktuellen Zeitstempel plus der angegebenen Zeiteinheiten zurück. Siehe auch [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Gibt den aktuellen Zeitstempel abzüglich der angegebenen Zeiteinheiten zurück. Siehe auch [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Gibt den Beginn des Tages für einen Zeitstempel zurück. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Gibt den Beginn der Stunde für einen Zeitstempel zurück. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Gibt den Beginn des Monats für einen Zeitstempel zurück. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Subtrahiert eine Anzahl von Zeiteinheiten von einem Zeitstempel. Siehe auch [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [ticks](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Gibt den Eigenschaftswert `ticks` für einen angegebenen Zeitstempel zurück. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Gibt den aktuellen Zeitstempel als Zeichenfolge zurück. | 
||| 

<a name="workflow-functions"></a>

### <a name="workflow-functions"></a>Workflowfunktionen

Diese Workflowfunktionen können für folgende Aktionen hilfreich sein:

* Abrufen von Details zu einer Workflowinstanz zur Laufzeit. 
* Arbeiten mit den Eingaben für die Instanziierung von Logik-Apps.
* Verweisen auf die Ausgaben von Triggern und Aktionen.

Sie können beispielsweise auf die Ausgaben einer Aktion verweisen und die Daten in einer späteren Aktion verwenden. Die vollständige Referenz zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).

| Workflowfunktion | Aufgabe | 
| ----------------- | ---- | 
| [action](../logic-apps/workflow-definition-language-functions-reference.md#action) | Gibt die Ausgabe der aktuellen Aktion zur Laufzeit oder Werte aus anderen Name/Wert-Paaren im JSON-Format zurück. Siehe auch [actions](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Gibt die `body`-Ausgabe einer Aktion zur Laufzeit zurück. Siehe auch [body](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Gibt die Ausgabe einer Aktion zur Laufzeit zurück. Siehe [actions](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actions](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Gibt die Ausgabe einer Aktion zur Laufzeit oder Werte aus anderen Name/Wert-Paaren im JSON-Format zurück. Siehe auch [action](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [body](#body) | Gibt die `body`-Ausgabe einer Aktion zur Laufzeit zurück. Siehe auch [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Erstellt ein Array mit den Werten, die mit einem Schlüsselnamen in Aktionsausgaben vom Typ *form-data* oder *form-encoded* übereinstimmen. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Gibt einen einzelnen Wert zurück, der mit einem Schlüsselnamen in einer Aktionsausgabe vom Typ *form-data* oder *form-encoded* übereinstimmt. | 
| [item](../logic-apps/workflow-definition-language-functions-reference.md#item) | Gibt in einer wiederholten Aktion für ein Array das aktuelle Element im Array während der aktuellen Iteration der Aktion zurück. | 
| [items](../logic-apps/workflow-definition-language-functions-reference.md#items) | Gibt in einer „for-each“- oder „do-until“-Schleife das aktuelle Element aus der angegebenen Schleife zurück.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | Gibt die „Rückruf-URL“ zurück, die einen Trigger oder eine Aktion aufruft. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Gibt den Textteil für einen bestimmten Teil einer Aktionsausgabe zurück, die mehrere Teile hat. | 
| [parameters](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Gibt den Wert für einen Parameter zurück, der in der Definition Ihrer Logik-App beschrieben wird. | 
| [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Gibt die Ausgabe eines Triggers zur Laufzeit oder aus anderen Name/Wert-Paaren im JSON-Format zurück. Siehe auch [triggerOutputs](#triggerOutputs) und [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Gibt die `body`-Ausgabe eines Triggers zur Laufzeit zurück. Siehe [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Gibt einen einzelnen Wert zurück, der mit einem Schlüsselnamen in Triggerausgaben vom Typ *form-data* oder *form-encoded* übereinstimmt. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | Gibt einen bestimmten Textteil in der mehrteiligen Ausgabe eines Triggers zurück. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Erstellt ein Array, dessen Werte mit einem Schlüsselnamen in Triggerausgaben vom Typ *form-data* oder *form-encoded* übereinstimmen. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Gibt eine Triggerausgabe zur Laufzeit oder Werte aus anderen Name/Wert-Paaren im JSON-Format zurück. Siehe [trigger](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [Variablen](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Gibt den Wert für eine angegebene Variable zurück. | 
| [workflow](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | Gibt sämtliche Details zum Workflow selbst zur Laufzeit zurück. | 
||| 

<a name="uri-parsing-functions"></a>

### <a name="uri-parsing-functions"></a>URI-Analysefunktionen

Sie können für die Arbeit mit URIs (Uniform Resource Identifiers) und das Abrufen verschiedener Eigenschaftswerte für diese URIs folgende URI-Analysefunktionen verwenden. Die vollständige Referenz zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).

| URI-Analysefunktion | Aufgabe | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Gibt den Wert `host` für einen Uniform Resource Identifier (URI) zurück. | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Gibt den Wert `path` für einen Uniform Resource Identifier (URI) zurück. | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Gibt die Werte `path` und `query` für einen Uniform Resource Identifier (URI) zurück. | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Gibt den Wert `port` für einen Uniform Resource Identifier (URI) zurück. | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Gibt den Wert `query` für einen Uniform Resource Identifier (URI) zurück. | 
| [uriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Gibt den Wert `scheme` für einen Uniform Resource Identifier (URI) zurück. | 
||| 

<a name="manipulation-functions"></a>

### <a name="json-and-xml-functions"></a>JSON- und XML-Funktionen

Für die Arbeit mit JSON-Objekten und XML-Knoten können Sie folgende Bearbeitungsfunktionen verwenden. Die vollständige Referenz zu den einzelnen Funktionen finden Sie unter [Workflow Definition Language functions reference for Azure Logic Apps (Funktionsreferenz für die Workflowdefinitionssprache für Azure Logic Apps)](../logic-apps/workflow-definition-language-functions-reference.md).

| Bearbeitungsfunktion | Aufgabe | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Fügt eine Eigenschaft und den zugehörigen Wert, oder ein Name/Wert-Paar zu einem JSON-Objekt hinzu und gibt das aktualisierte Objekt zurück. | 
| [coalesce](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Gibt den ersten Wert ungleich NULL aus mindestens einem Parameter zurück. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Entfernt eine Eigenschaft aus einem JSON-Objekt und gibt das aktualisierte Objekt zurück. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Legt den Wert für die Eigenschaft eines JSON-Objekts fest und gibt das aktualisierte Objekt zurück. | 
| [xpath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Überprüft die XML auf Knoten oder Werte, die mit einem XPath-Ausdruck (XML Path Language) übereinstimmen, und gibt die übereinstimmenden Knoten oder Werte zurück. | 
||| 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Aktionen und Triggern für die Definitionssprache für Workflows](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Weitere Informationen zum programmgesteuerten Erstellen und Verwalten von Logik-Apps mit der [Workflow-REST-API](https://docs.microsoft.com/rest/api/logic/workflows)
