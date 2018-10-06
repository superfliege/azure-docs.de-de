---
title: Moderationsaufträge und Human-in-the-Loop-Überprüfungen – Content Moderator
titlesuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie bei der computergestützten Moderation die menschliche Aufsicht anwenden, um optimale Ergebnisse zu erzielen.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 1/21/2018
ms.author: sajagtap
ms.openlocfilehash: b4a2f62b1c9cefb716cb217baf7389c3e7c790b8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223254"
---
# <a name="moderation-jobs-and-reviews"></a>Moderationsaufträge und -überprüfungen

Kombinieren Sie mithilfe der [Überprüfungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) von Azure Content Moderator die computergestützte Moderation mit Human-in-the-Loop-Funktionen, um die besten Ergebnisse für Ihr Unternehmen zu erhalten.

Die Überprüfungs-API bietet die folgenden Möglichkeiten, menschliche Aufsicht in Ihren Inhaltsmoderationsprozess einzubeziehen:

* Mit `Job`-Vorgängen wird in einem Schritt die computergestützte Moderation gestartet und die manuelle Überprüfung erstellt.
* `Review`-Vorgänge werden zur Erstellung der manuellen Überprüfung außerhalb des Moderationsschritts verwendet.
* `Workflow`-Vorgänge dienen zum Verwalten von Workflows, die die Überprüfung mit Schwellenwerten für das Erstellen der Überprüfung automatisieren.

Die Vorgänge `Job` und `Review` akzeptieren Ihre Rückrufendpunkte für den Empfang des Status und der Ergebnisse.

In diesem Artikel werden die Vorgänge `Job` und `Review` behandelt. Im Artikel [Übersicht über Workflows](workflow-api.md) finden Sie Informationen zum Erstellen, Bearbeiten und Abrufen von Workflowdefinitionen.

## <a name="job-operations"></a>Auftragsvorgänge

### <a name="start-a-job"></a>Starten eines Auftrags
Mit dem Vorgang `Job.Create` können Sie eine Moderation und den Auftrag zur Erstellung einer manuellen Überprüfung starten. Content Moderator überprüft den Inhalt und wertet den angegebenen Workflow aus. Basierend auf den Workflowergebnissen werden entweder Überprüfungen erstellt oder der Schritt wird übersprungen. Außerdem werden die Tags nach der Moderation und nach der Überprüfung an Ihren Rückrufendpunkt gesendet.

Die Eingaben enthalten folgende Informationen:

- Die ID des Überprüfungsteams
- Den zu moderierenden Inhalt
- Den Workflownamen (Standardmäßig lautet der Workflowname „default“.)
- Ihren API-Rückrufendpunkt für Benachrichtigungen
 
Die folgende Antwort zeigt die ID des gestarteten Auftrags. Mithilfe der Auftrags-ID können Sie den Auftragsstatus abrufen und detaillierte Informationen erhalten.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

### <a name="get-job-status"></a>Abrufen des Auftragsstatus

Verwenden Sie den Vorgang `Job.Get` und die Auftrags-ID, um die Details zu einem laufenden oder abgeschlossenen Auftrag abzurufen. Der Vorgang wird sofort zurückgegeben, während der Moderationsauftrag asynchron ausgeführt wird. Die Ergebnisse werden durch den Rückrufendpunkt zurückgegeben.

Ihre Eingaben enthalten folgende Informationen:

- Die ID des Überprüfungsteams: Die Auftrags-ID, die vom vorherigen Vorgang zurückgegeben wurde

Die Antwort enthält folgende Informationen:

- Den Bezeichner (ID) der erstellten Überprüfung (Verwenden Sie diese ID, um die Ergebnisse der endgültigen Überprüfung abzurufen.)
- Den Status des (abgeschlossenen oder laufenden) Auftrags: Die zugewiesenen Moderationstags (Schlüssel-Wert-Paare)
- Den Bericht zur Auftragsausführung
 
 
        {
            "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
            "TeamName": "some team name",
            "Status": "Complete",
            "WorkflowId": "OCR",
            "Type": "Image",
            "CallBackEndpoint": "",
            "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
            "ResultMetaData":[
            {
            "Key": "hasText",
            "Value": "True"
            },
            {
            "Key": "ocrText",
            "Value": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
            }
            ],
            "JobExecutionReport": [
            {
                "Ts": "2018-01-07T00:38:29.3238715",
                "Msg": "Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
                },
                {
                "Ts": "2018-01-07T00:38:29.2928416",
                "Msg": "Job marked completed and job content has been removed"
                },
                {
                "Ts": "2018-01-07T00:38:29.0856472",
                "Msg": "Execution Complete"
                },
            {
                "Ts": "2018-01-07T00:38:26.7714671",
                "Msg": "Successfully got hasText response from Moderator"
                },
                {
                "Ts": "2018-01-07T00:38:26.4181346",
                "Msg": "Getting hasText from Moderator"
                },
                {
                "Ts": "2018-01-07T00:38:25.5122828",
                "Msg": "Starting Execution - Try 1"
                }
            ]
        }
 
![Bildüberprüfung für menschliche Moderatoren](images/ocr-sample-image.PNG)

## <a name="review-operations"></a>Überprüfen von Vorgängen

### <a name="create-reviews"></a>Erstellen von Überprüfungen

Verwenden Sie den Vorgang `Review.Create`, um die manuellen Überprüfungen zu erstellen. Sie moderieren diese entweder außerhalb oder verwenden eine benutzerdefinierte Logik, um die Moderationstags zuzuweisen.

Ihre Eingaben für diesen Vorgang enthalten Folgendes:

- Den zu überprüfenden Inhalt
- Die zugewiesenen Tags (Schlüssel-Wert-Paare) für die von menschlichen Moderatoren durchgeführte Überprüfung

Die folgende Antwort zeigt die Überprüfungs-ID:

    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]


### <a name="get-review-status"></a>Abrufen des Überprüfungsstatus
Verwenden Sie den Vorgang `Review.Get`, um die Ergebnisse nach einer manuellen Überprüfung des moderierte Bilds zu erhalten. Sie werden über Ihren bereitgestellten Rückrufendpunkt benachrichtigt. 

Der Vorgang gibt zwei Gruppen von Tags zurück: 

* Die vom Moderationsdienst zugewiesenen Tags
* Die Tags nach Abschluss der manuellen Überprüfung

Ihre Eingaben enthalten mindestens Folgendes:

- Den Namen des Überprüfungsteams
- Die Überprüfungs-ID, die vom vorherigen Vorgang zurückgegeben wurde

Die Antwort enthält folgende Informationen:

- Den Überprüfungsstatus
- Die Tags (Schlüssel-Wert-Paare), die vom Prüfer bestätigt wurden
- Die Tags (Schlüssel-Wert-Paare), die vom Moderationsdienst zugewiesen wurden

In der folgenden Beispielantwort sehen Sie sowohl die vom Prüfer zugewiesenen Tags (**reviewerResultTags**) als auch die Anfangstags (**metadata**):

    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="next-steps"></a>Nächste Schritte

* Testen Sie die [API-Konsole für Aufträge](try-review-api-job.md), und verwenden Sie die REST-API-Codebeispiele. Falls Sie mit Visual Studio und C# vertraut sind, können Sie sich auch die [.NET-Schnellstartanleitung für Aufträge](moderation-jobs-quickstart-dotnet.md) ansehen. 
* Machen Sie in Bezug auf Überprüfungen mit der [API-Konsole für Überprüfungen](try-review-api-review.md) vertraut, und verwenden Sie die REST-API-Codebeispiele. Gehen Sie anschließend den [.NET-Schnellstart für Überprüfungen](moderation-reviews-quickstart-dotnet.md) durch.
* Verwenden Sie für Videoüberprüfungen den [Schnellstart für Videoüberprüfungen](video-reviews-quickstart-dotnet.md), und erfahren Sie, wie Sie [Transkripte zur Videoüberprüfung hinzufügen](video-transcript-reviews-quickstart-dotnet.md).
