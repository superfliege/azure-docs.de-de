---
title: Moderationsworkflows – Content Moderator
titlesuffix: Azure Cognitive Services
description: Verwenden Sie Workflows mit den Auftragsvorgängen der Überprüfungs-API zum Automatisieren von Human-in-the-Loop-Überprüfungen auf Basis Ihrer Inhaltsrichtlinien und Schwellenwerte.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 4abe3a864b798552caa27d07a422a7d67b2e4018
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221384"
---
# <a name="automate-moderation-reviews-with-workflows"></a>Automatisieren von Moderationsüberprüfungen mit Workflows

Content Moderator umfasst Tools und APIs zum Verwalten von Workflows. Sie verwenden Workflows mit den [Auftragsvorgängen der Überprüfungs-API](review-api.md) zum automatischen Erstellen von Human-in-the-Loop-Überprüfungen auf Basis Ihrer Inhaltsrichtlinien und Schwellenwerte.

Die Überprüfungs-API bietet die folgenden Möglichkeiten, menschliche Aufsicht in Ihren Inhaltsmoderationsprozess einzubeziehen:

1. Die **Auftragsvorgänge**, um in einem Schritt die computergestützte Moderation zu starten und die manuelle Überprüfung zu erstellen.
1. Die **Überprüfungsvorgänge**, um die manuelle Überprüfung außerhalb des Moderationsschritts zu erstellen.
1. Die **Workflowvorgänge** zum Verwalten von Workflows, die die Überprüfung mit Schwellenwerten für das Erstellen der Überprüfung automatisieren.

Dieser Artikel behandelt die **Workflowvorgänge**. Lesen Sie die Übersicht [Moderationsaufträge und -überprüfungen](review-api.md), um sich über Inhaltsmoderationsaufträge und -überprüfungen zu informieren.

Sich mit dem **standardmäßigen** Workflow vertraut zu machen, ist der beste Weg, die Workflows im Content Moderator verstehen zu lernen.

## <a name="your-first-workflow"></a>Ihr erster Workflow

Ihr erster Workflow wird zusammen mit Ihrem [Überprüfungstoolteam](https://contentmoderator.cognitive.microsoft.com/) ausgeliefert. Registrieren Sie sich, wenn Sie es nicht bereits getan haben.

Navigieren Sie zum [Workflowsbildschirm des Überprüfungstools](Review-Tool-User-Guide/Workflows.md) auf der Registerkarte „Einstellungen“. Sie sehen einen **Standardworkflow** wie in der folgenden Abbildung gezeigt:

![Content Moderator-Workflows](Review-Tool-User-Guide/images/2-workflows-1.png)

### <a name="open-the-default-workflow"></a>Öffnen des Standardworkflows

Öffnen Sie die Workflowbearbeitungsseite mit der Option **Bearbeiten**, wie in der folgenden Abbildung gezeigt: ![Content Moderator-Standardworkflow](images/default-workflow-listed.PNG)

### <a name="the-designer-view"></a>Die Designeransicht

Sie sehen die Registerkarte **Designer** für den Workflow. Die Designeransicht ist in folgende Schritte unterteilt:

1. Die **Bedingung** für den Workflow, der ausgewertet werden soll. In diesem Fall ruft der Workflow die Bild-API von Content Moderator auf und überprüft, ob die `isAdult`-Ausgabe gleich `true` ist.
1. Die auszuführende **Aktion**, wenn die Bedingung erfüllt ist. In diesem Fall erstellt der Workflow eine Überprüfung im Überprüfungstool, wenn die `isAdult`-Ausgabe gleich `true` ist.

![Content Moderator-Standardworkflow – Designer](images/default-workflow-designer.png)

### <a name="the-json-view"></a>Die JSON-Ansicht

Wählen Sie die Registerkarte **JSON** aus, um die JSON-Definition des Workflows anzuzeigen.

    {
        "Type": "Logic",
        If": {
            "ConnectorName": "moderator",
            "OutputName": "isAdult",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": []
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="key-learning"></a>Wesentliche Punkte

Die Workflows in Content Moderator sind einfach zu konfigurieren und flexibel. Wenn der integrierte Designer nicht Ihren Anforderungen entspricht, schreiben Sie die Workflowdefinition im **JSON**-Format. Verwenden Sie dann die JSON-Definition mit der [Workflow-API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) zum Erstellen und Verwalten der Workflows aus Ihrer Anwendung.

## <a name="define-a-custom-workflow"></a>Definieren eines benutzerdefinierten Workflows

Mit den Workflowfunktionen von Content Moderator können Sie benutzerdefinierte Workflows definieren und verwenden. Definieren Sie mithilfe des Artikels [Definieren, Testen und Verwenden von Workflows](Review-Tool-User-Guide/Workflows.md) einen benutzerdefinierten Workflow. Dieser Workflow nutzt die OCR-Funktion von Content Moderator zum Extrahieren von Text aus einem Beispielbild. Er erstellt dann eine Überprüfung im Überprüfungstool.

### <a name="the-sample-image"></a>Das Beispielbild

Speichern Sie das [Beispielbild](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) auf Ihrem lokalen Laufwerk. Sie benötigen dieses Bild für Ihre Übung.

### <a name="the-designer-view"></a>Die Designeransicht

Wählen Sie die Registerkarte **Designer** und das [Tutorial zum Erstellen des Workflows](Review-Tool-User-Guide/Workflows.md) aus, um einen benutzerdefinierten Workflow zu definieren. Die folgende Abbildung zeigt die Ansicht **Bedingung** des Designers. Bitte entnehmen Sie die restlichen Schritte dem Tutorial.

![Content Moderator – Workflowbedingung](Review-Tool-User-Guide/images/ocr-workflow-step-2-condition.PNG)

### <a name="the-json-view"></a>Die JSON-Ansicht

Wählen Sie die Registerkarte **JSON** aus, um die folgende JSON-Definition Ihres benutzerdefinierten Workflows anzuzeigen. Beachten Sie, dass die **If-Then**-Anweisungen in der JSON-Definition den Schritten entsprechen, die Sie mithilfe der Designer-Ansicht definiert haben.

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="workflow-result"></a>Workflowergebnis

Nachdem Sie den Workflow aus dem Workflowsbildschirm getestet haben, wird die folgende Überprüfung erstellt. Navigieren Sie zur Registerkarte **Bild** unter **Überprüfen**, um Ihre Überprüfung anzuzeigen.
Der Workflow erstellte die Überprüfung, da die primäre Bedingung für das Vorhandensein von Text ein positives Testergebnis erbracht hat. In der Bildüberprüfung wurde auch das **`a`**-Tag hervorgehoben.

![Content Moderator – einfache Workflowausgabe](images/ocr-sample-image-workflow1.PNG)


## <a name="advanced-workflow-with-combination"></a>Erweiterter Workflow mit Kombination

### <a name="the-sample-image"></a>Das Beispielbild

Verwenden Sie das gleiche [Beispielbild](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) wie im vorherigen Abschnitt.

Ändern Sie allerdings nun Ihre primäre Bedingung in eine Kombination aus zwei Überprüfungen. Überprüfen Sie nicht nur, ob Text vorhanden ist, sondern auch, ob der ggf. vorhandene Text anstößige Ausdrücke enthält. Der Workflow erstellt eine Überprüfung, wenn er Text findet **und** darin anstößige Ausdrücke erkennt.

### <a name="the-designer-view"></a>Die Designeransicht

Um die **Bedingung** in eine **Kombination** zu ändern, ändern Sie den Workflow. Die folgende Abbildung zeigt die neue Ansicht im Designer.

![Content Moderator – Geänderte Workflowbedingung](images/ocr-workflow-2-designer.PNG)

### <a name="the-json-view"></a>Die JSON-Ansicht

Wählen Sie die Registerkarte **JSON** aus, um die folgende JSON-Definition Ihres geänderten benutzerdefinierten Workflows anzuzeigen. Beachten Sie, dass die **If-Then**-Anweisungen in der JSON-Definition den neuen Schritten entsprechen, die Sie dem Workflow hinzugefügt haben.

    {
        "Type": "Logic",
        "If": {
        "Left": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Right": {
            "ConnectorName": "moderator",
            "OutputName": "text.HasProfanity",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition",
            "AlternateInput": "moderator.ocrText"
            },
        "Combine": "AND",
        "Type": "Combine"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

    
### <a name="workflow-result"></a>Workflowergebnis

Nachdem Sie den Workflow erneut getestet haben, werden Sie feststellen, dass keine Überprüfung erstellt wurde. Um das Fehlen jeglicher Überprüfung zu bestätigen, navigieren Sie zur Registerkarte **Bild** unter **Überprüfen**.
Der Workflow hat keine Überprüfung erstellt, weil er im extrahierten Text keine anstößigen Ausdrücke erkennen konnte.

![Content Moderator – Geänderte Workflowbedingung](images/ocr-workflow-2-result.PNG)


## <a name="the-workflow-api"></a>Der Workflow-API

Die [Workflowvorgänge](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) stellen die Programmierschnittstelle zu den Workflowfunktionen bereit. Mithilfe der Workflow-API erstellen Sie Workflows, rufen Workflowdetails ab und aktualisieren Workflowdefinitionen.

### <a name="get-all-workflow-details"></a>Abrufen [aller] Workflowdetails

Der **Workflow-Get**-Vorgang akzeptiert die folgenden Eingaben:

- **team**: Die Team-ID, die Sie beim Einrichten Ihres [Prüfungstoolkontos](https://contentmoderator.cognitive.microsoft.com/) erstellt haben. 
- **workflowname**: Der Name Ihres Workflows. Beginnen Sie mit `default`.
- **Ocp-Apim-Subscription-Key**: Befindet sich auf der Registerkarte **Einstellungen**. Weitere Informationen finden Sie in der [Übersicht](overview.md).

Wenn der Vorgang erfolgreich ist, lautet der **Antwortstatus** `200 OK`, und im Feld **Antwortinhalt** wird die Workflowdefinition im JSON-Format angezeigt.
Weitere Informationen finden Sie unter [Starten eines Moderationsauftrags in der API-Konsole](try-review-api-job.md).

### <a name="create-or-update-workflow"></a>Erstellen oder Aktualisieren eines Workflows

Der Erstellungs- und Aktualisierungsvorgang ermöglicht das Erstellen von Workflows über die API.

Der **„Workflow-Create“- bzw. „Update“**-Vorgang akzeptiert die folgenden Eingaben:

- **team**: Die Team-ID, die Sie beim Einrichten Ihres [Prüfungstoolkontos](https://contentmoderator.cognitive.microsoft.com/) erstellt haben. 
- **workflowname**: Der Name Ihres Workflows. Beginnen Sie mit `default`.
- **Ocp-Apim-Subscription-Key**: Befindet sich auf der Registerkarte **Einstellungen**. Weitere Informationen finden Sie in der [Übersicht](overview.md).

Wenn der Vorgang erfolgreich ist, lautet der **Antwortstatus** `200 OK`, und im Feld **Antwortinhalt** wird `true` angezeigt. Um mehr zu erfahren, [testen Sie den `Create`-Vorgang](try-review-api-job.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen von benutzerdefinierten Workflows finden Sie unter [Definieren, Testen und Verwenden von Workflows](Review-Tool-User-Guide/Workflows.md). 

Testen Sie die [Workflow-API-Konsole](try-review-api-job.md), und verwenden Sie die REST-API-Codebeispiele. 

Verwenden Sie abschließend Ihre benutzerdefinierten Workflows mit den **Auftragsvorgängen**, wie in [Starten eines Moderationsauftrags in der API-Konsole](try-review-api-job.md) und [Starten von Moderationsaufträgen mit .NET](moderation-jobs-quickstart-dotnet.md) gezeigt.
