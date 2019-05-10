---
title: Die Qualifikation „Bedingt“ der kognitiven Suche (Azure Search) | Microsoft-Dokumentation
description: Die Qualifikation „Bedingt“ ermöglicht das Filtern, das Erstellen von Standardwerten und das Zusammenführen von Werten.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: luisca
ms.openlocfilehash: 6a203a38437ccb6a9c325e6594289744e0148c84
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027686"
---
#   <a name="conditional-skill"></a>Qualifikation „Bedingt“

Die Qualifikation **Bedingt** ermöglicht eine Vielzahl von Szenarien, die eine boolesche Operation erfordern, um zu entscheiden, welche Daten einer Ausgabe zugewiesen werden sollen. Zu diesen Szenarien gehören: Filterung, Zuweisung eines Standardwerts und Zusammenführung von Daten auf der Grundlage einer Bedingung.

Der folgende Pseudocode erläutert, wofür die Qualifikation „Bedingt“ dient:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Dieser Skill ist nicht an eine Cognitive Services-API gebunden, und Ihnen entstehen für seine Nutzung keine Kosten. Es wird jedoch empfohlen, dennoch eine [Cognitive Services-Ressource anzufügen](cognitive-search-attach-cognitive-services.md), um die Ressourcenoption **Free** außer Kraft zu setzen, durch die Sie auf eine geringe Anzahl von Anreicherungen pro Tag beschränkt werden.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Ausgewertete Felder

Diese Qualifikation stellt einen Sonderfall dar, da ihre Eingaben ausgewertete Felder sind.

Die folgenden sind gültige Werte eines Ausdrucks:

-   Anmerkungspfade (Pfade in Ausdrücken müssen mit „$(“ und „)“ abgegrenzt werden) <br/>
    Beispiele:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Literale (Zeichenfolgen, Zahlen, TRUE, FALSE, NULL) <br/>
    Beispiele:
    ```
       "= 'this is a string'"   // string, note the single quotes
       "= 34"                   // number
       "= true"                 // boolean
       "= null"                 // null value
    ```

-  Ausdrücke, die einen Vergleichsoperator verwenden (==, !=, >=, >, <=, <) <br/>
    Beispiele:
    ```
        "= $(/document/language) == 'en'"
        "= $(/document/sentiment) >= 0.5"
    ```

-   Ausdrücke, die boolesche Operatoren verwenden (&&, ||, !, ^) <br/>
    Beispiele:
    ```
        "= $(/document/language) == 'en' && $(/document/sentiment) > 0.5"
        "= !true"
    ```

-   Ausdrücke, die einen numerischen Operator verwenden (+, -, \*, /, %) <br/>
    Beispiele: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Aufgrund der unterstützten Auswertung kann die Qualifikation „Bedingt“ auch für kleinere Transformationsszenarien verwendet werden. Ein Beispiel finden Sie in der [Qualifikationsdefinition 4](#transformation-examples).

## <a name="skill-inputs"></a>Skilleingaben
Bei Eingaben muss die Groß-/Kleinschreibung beachtet werden.

| Eingaben      | BESCHREIBUNG |
|-------------|-------------|
| condition   | Diese Eingabe ist ein [ausgewertetes Feld](#evaluated-fields), das die auszuwertende Bedingung darstellt. Diese Bedingung sollte in einen booleschen Wert ausgewertet werden (TRUE oder FALSE).   <br/>  Beispiele: <br/> "= true" <br/> "= $(/document/language) =='fr'" <br/> "= $(/document/pages/\*/language) == $(/document/expectedLanguage)" <br/> |
| whenTrue    | Diese Eingabe ist ein [ausgewertetes Feld](#evaluated-fields). Der Wert für die Rückgabe, wenn die Bedingung mit TRUE ausgewertet wird. Konstante Zeichenfolgen sollten in einfachen Anführungszeichen (' ') zurückgegeben werden. <br/>Beispielwerte: <br/> "= 'contract'"<br/>"= $(/document/contractType)" <br/> "= $(/document/entities/\*)" <br/> |
| whenFalse   | Diese Eingabe ist ein [ausgewertetes Feld](#evaluated-fields). Der Wert für die Rückgabe, wenn die Bedingung mit FALSE ausgewertet wird.  <br/>Beispielwerte: <br/> "= 'contract'"<br/>"= $(/document/contractType)" <br/> "= $(/document/entities/\*)" <br/>

## <a name="skill-outputs"></a>Skillausgaben
Es gibt eine einzelne Ausgabe mit der Bezeichnung „output“. Sie gibt den Wert von whenFalse zurück, wenn die Bedingung FALSE ist, oder von whenTrue, wenn die Bedingung TRUE ist.

## <a name="examples"></a>Beispiele

### <a name="sample-skill-definition-1-filtering-documents-to-return-only-french-documents"></a>Beispieldefinition 1 einer Qualifikation: Filtern von Dokumenten, um nur „französische“ Dokumente zurückzugeben

Die folgende Ausgabe gibt ein Array von Sätzen („/document/frenchSentences“) zurück, wenn die Sprache des Dokuments Französisch ist. Wenn die Sprache nicht Französisch ist, wird dieser Wert auf NULL festgelegt.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'fr'" },
        { "name": "whenTrue", "source": "/document/sentences" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "frenchSentences" } ]
}
```
Wenn „/document/frenchSentences“ als *Kontext* einer anderen Qualifikation verwendet wird, wird diese nur ausgeführt, wenn „/document/frenchSentences“ nicht auf NULL festgelegt ist.


### <a name="sample-skill-definition-2-setting-a-default-value-when-it-does-not-exist"></a>Beispieldefinition 2 einer Qualifikation: Festlegen eines Standardwerts, wenn noch keiner vorhanden ist

Die folgende Ausgabe wird mit eine Anmerkung („/document/languageWithDefault“) erstellt, die entweder auf die Sprache des Dokuments oder auf „es“ bei nicht festgelegter Sprache festgelegt wird.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'es'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

### <a name="sample-skill-definition-3-merging-values-from-two-different-fields-into-a-single-field"></a>Beispieldefinition 3 einer Qualifikation: Zusammenführen von Werten aus zwei unterschiedlichen Feldern in einem einzigen Feld

In diesem Beispiel haben einige Sätze eine *frenchSentiment*-Eigenschaft. Wenn die *frenchSentiment*-Eigenschaft NULL ist, soll der Wert *englishSentiment* verwendet werden. Wir weisen die Ausgabe einfach dem Member *sentiment* zu („/document/sentiment/*/sentiment“).

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= $(/document/sentences/*/frenchSentiment) == null" },
        { "name": "whenTrue", "source": "/document/sentences/*/englishSentiment" },
        { "name": "whenFalse", "source": "/document/sentences/*/frenchSentiment" }
    ],
    "outputs": [ { "name": "output", "targetName": "sentiment" } ]
}
```

## <a name="transformation-examples"></a>Transformationsbeispiele
### <a name="sample-skill-definition-4-performing-data-transformations-on-a-single-field"></a>Beispieldefinition 4 einer Qualifikation: Ausführen von Datentransformationen auf ein einzelnes Feld

In diesem Beispiel erhalten wir eine Stimmung zwischen 0 und 1 und möchten diese so transformieren, dass sie zwischen -1 und 1 liegt. Dies ist eine kleine mathematische Transformation, die wir mit der Qualifikation „Bedingt“ ausführen können.

In diesem spezifischen Beispiel verwenden wir nie den bedingten Aspekt der Qualifikation, da die Bedingung immer TRUE ist. 

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= true" },
        { "name": "whenTrue", "source": "= $(/document/sentences/*/sentiment) * 2 - 1" },
        { "name": "whenFalse", "source": "= 0" }
    ],
    "outputs": [ { "name": "output", "targetName": "normalizedSentiment" } ]
}
```


## <a name="special-considerations"></a>Besondere Überlegungen
Beachten Sie, dass einige der Parameter ausgewertet werden. Sie müssen daher das dokumentierte Muster besonders sorgfältig befolgen. Ausdrücke müssen mit einem Gleichheitszeichen (=) beginnen, und Pfade müssen durch „$(“ und „)“ abgegrenzt werden. Schließen Sie Zeichenfolgen unbedingt in Apostrophe ein, da dies die Unterscheidung zwischen Zeichenfolgen und tatsächlichen Pfaden und Operatoren bei der Auswertung vereinfacht. Fügen Sie außerdem ein Leerzeichen um Operatoren ein (ein Sternchen „*“ hat in einem Pfad z. B. eine andere Bedeutung als den Multiplikationsoperator).


## <a name="next-steps"></a>Nächste Schritte

+ [Vordefinierte Skills](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
