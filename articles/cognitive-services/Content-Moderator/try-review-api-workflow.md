---
title: Inhaltsmoderationsworkflows über die API-Konsole – Content Moderator
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Inhaltsmoderationsworkflows über die API-Konsole verwenden.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: sajagtap
ms.openlocfilehash: 4ef8951b30fa7aede08a1af3c834192b5ed18649
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223849"
---
# <a name="workflows-from-the-api-console"></a>Workflows über die API-Konsole

Verwenden Sie die [Workflowvorgänge](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) in Azure Content Moderator, um mithilfe der API für die Überprüfung einen Workflow zu erstellen oder aktualisieren oder Workflowdetails abzurufen. Mit dieser API können Sie einfache, komplexe und sogar geschachtelte Ausdrücke für Ihre Workflows definieren. Die Workflows werden im Prüfungstool angezeigt, die Ihr Team verwenden kann. Die Workflows werden auch bei den Auftragsvorgängen der Überprüfung-API verwendet.

## <a name="prerequisites"></a>Voraussetzungen

1. Navigieren Sie zum [Prüfungstool](https://contentmoderator.cognitive.microsoft.com/). Registrieren Sie sich, sofern noch nicht geschehen. 
2. Klicken Sie im Prüfungstool unter **Einstellungen** auf die Registerkarte **Workflows**, wie im [Workflowtutorial](Review-Tool-User-Guide/Workflows.md) des Prüfungstools gezeigt wird.

### <a name="browse-to-the-workflows-screen"></a>Aufrufen der Ansicht „Workflows“

Wählen Sie auf dem Content Moderator-Dashboard **Überprüfung** > **Einstellungen** > **Workflows**. Ein Standardworkflow wird angezeigt.

  ![Standardworkflow](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>Abrufen der JSON-Definition des Standardworkflows

Wählen Sie für Ihren Workflow die Option **Bearbeiten** aus, und klicken Sie dann auf die Registerkarte **JSON**. Der folgende JSON-Ausdruck wird angezeigt:

    {
        "Type": "Logic",
        "If": {
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

## <a name="get-workflow-details"></a>Abrufen von Workflowdetails

Verwenden Sie den Vorgang **Workflow – Abrufen**, um Details zu Ihrem bestehenden Standardworkflow abzurufen.

Navigieren Sie im Prüfungstool zum Abschnitt [Anmeldeinformationen](Review-Tool-User-Guide/credentials.md#the-review-tool).

### <a name="browse-to-the-api-reference"></a>Navigieren zur API-Referenz

1. Wählen Sie in der Ansicht **Anmeldeinformationen** die Option [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) aus. 
2. Wenn die Seite **Workflow – Erstellen oder aktualisieren** geöffnet wird, navigieren Sie zum Verweis [Workflow – Abrufen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58).

### <a name="select-your-region"></a>Auswählen Ihrer Region

Wählen Sie für die **OpenAPI-Testkonsole** die Region aus, die Ihrem Standort am ehesten nahekommt.

  ![Regionsauswahl auf der Seite „Workflow – Abrufen“](images/test-drive-region.png)

  Die API-Konsole **Workflow – Abrufen** wird geöffnet.

### <a name="enter-parameters"></a>Parameter eingeben

Geben Sie Werte für **team**, **workflowname** und **Ocp-Apim-Subscription-Key** (Ihr Abonnementschlüssel) ein:

- **team**: Die Team-ID, die Sie beim Einrichten Ihres [Prüfungstoolkontos](https://contentmoderator.cognitive.microsoft.com/) erstellt haben. 
- **workflowname**: Der Name Ihres Workflows. Verwenden Sie `default`.
- **Ocp-Apim-Subscription-Key**: Befindet sich auf der Registerkarte **Einstellungen**. Weitere Informationen finden Sie in der [Übersicht](overview.md).

  ![Abrufen allgemeiner Parameter und Header](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>Senden der Anforderung
  
Wählen Sie **Senden** aus. Wenn der Vorgang erfolgreich ist, lautet der **Antwortstatus** `200 OK`, und im Feld **Antwortinhalt** wird der folgende JSON-Workflow angezeigt:

    {
        "Name": "default",
        "Description": "Default",
        "Type": "Image",
        "Expression": {
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isadult",
            "Operator": "eq",
            "Value": "true",
            "AlternateInput": null,
            "Type": "Condition"
            },
        "Then": {
            "Perform": [{
                "Name": "createreview",
                "Subteam": null,
                "CallbackEndpoint": null,
                "Tags": []
            }],
            "Type": "Actions"
            },
            "Else": null,
            "Type": "Logic"
            }
    }


## <a name="create-a-workflow"></a>Erstellen eines Workflows

Navigieren Sie im Prüfungstool zum Abschnitt [Anmeldeinformationen](Review-Tool-User-Guide/credentials.md#the-review-tool).

### <a name="browse-to-the-api-reference"></a>Navigieren zur API-Referenz

Wählen Sie in der Ansicht **Anmeldeinformationen** die Option [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) aus. Die Seite **Workflow – Erstellen oder aktualisieren** wird geöffnet.

### <a name="select-your-region"></a>Auswählen Ihrer Region

Wählen Sie für die **OpenAPI-Testkonsole** die Region aus, die Ihrem Standort am ehesten nahekommt.

  ![Regionsauswahl auf der Seite „Workflow – Erstellen oder aktualisieren“](images/test-drive-region.png)

  Die API-Konsole **Workflow – Erstellen oder aktualisieren** wird geöffnet.

### <a name="enter-parameters"></a>Parameter eingeben

Geben Sie Werte für **team**, **workflowname** und **Ocp-Apim-Subscription-Key** (Ihr Abonnementschlüssel) ein:

- **team**: Die Team-ID, die Sie beim Einrichten Ihres [Prüfungstoolkontos](https://contentmoderator.cognitive.microsoft.com/) erstellt haben. 
- **workflowname**: Der Name Ihres neuen Workflows.
- **Ocp-Apim-Subscription-Key**: Befindet sich auf der Registerkarte **Einstellungen**. Weitere Informationen finden Sie in der [Übersicht](overview.md).

  ![Abfrageparameter und Header in der Konsole „Workflow – Erstellen oder aktualisieren“](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>Eingeben der Workflowdefinition

1. Bearbeiten Sie das Feld **Anforderungstext**, indem Sie die JSON-Anforderung mit Angaben zur **Description** und zum **Type** („Image“ oder „Text“) eingeben. 
2. Kopieren Sie für **Expression** den Standardworkflowausdruck aus dem vorherigen Abschnitt, wie im Folgenden gezeigt wird:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    Der Anforderungstext sieht wie in der folgenden JSON-Anforderung aus:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": {
                "Type": "Logic",
                "If": {
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
                    "Tags": [ ]
                }
                ],
                "Type": "Actions"
                }
            }
        }
 
### <a name="submit-your-request"></a>Senden der Anforderung
  
Wählen Sie **Senden** aus. Wenn der Vorgang erfolgreich ist, lautet der **Antwortstatus** `200 OK`, und im Feld **Antwortinhalt** wird `true` angezeigt.

### <a name="check-out-the-new-workflow"></a>Verwenden des neuen Workflows

Wählen Sie im Prüfungstool **Überprüfung** > **Einstellungen** > **Workflows** aus. Der neue Workflow wird angezeigt und kann verwendet werden.

  ![Liste von Workflows im Prüfungstool](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Überprüfen der Details des neuen Workflows

1. Wählen Sie für Ihren Workflow die Option **Bearbeiten** aus, und klicken Sie dann auf die Registerkarten **Designer** und **JSON**.

   ![Registerkarte „Designer“ für einen ausgewählten Workflow](images/workflow-console-new-workflow-designer.PNG)

2. Um die JSON-Ansicht des Workflows anzuzeigen, wählen Sie die Registerkarte **JSON** aus.

## <a name="next-steps"></a>Nächste Schritte

* Beispiele für komplexere Workflows finden Sie in der [Übersicht über die Workflows](workflow-api.md).
* Erfahren Sie, wie Workflows mit [Inhaltsmoderationsaufträgen](try-review-api-job.md) verwendet werden.
