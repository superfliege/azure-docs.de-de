---
title: Ausführen von Inhaltsmoderationsaufträgen in Azure Content Moderator – | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Inhaltsmoderationsaufträge in der API-Konsole ausführen.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/03/2017
ms.author: sajagtap
ms.openlocfilehash: 6f741be1001ae70d5fdbf6f374204aaad1601abe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373018"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>Starten eines Moderationsauftrags über die API-Konsole

Verwenden Sie die [Auftragsvorgänge](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) der Überprüfungs-API, um End-to-End-Inhaltsmoderationsaufträge für Bild- und Textinhalt in Azure Content Moderator zu initiieren. 

Der Moderationsauftrag durchsucht Ihre Inhalte mithilfe der Bildmoderations-API oder der Textmoderations-API in Content Moderator. Der Moderationsauftrag verwendet dann Workflows (im Prüfungstool definiert), um Überprüfungen im Prüfungstool zu generieren. 

Nachdem ein menschlicher Moderator die automatisch zugewiesenen Tags und Vorhersagedaten überprüft und eine finale Moderationsentscheidung getroffen hat, sendet die Überprüfungs-API alle Informationen an Ihren API-Endpunkt.

## <a name="prerequisites"></a>Voraussetzungen

Navigieren Sie zum [Prüfungstool](https://contentmoderator.cognitive.microsoft.com/). Registrieren Sie sich, wenn Sie es nicht bereits getan haben. [Definieren Sie einen benutzerdefinierten Workflow](Review-Tool-User-Guide/Workflows.md) innerhalb des Prüfungstools, um diesen in diesem `Job`-Vorgang zu verwenden.

## <a name="use-the-api-console"></a>Verwenden der API-Konsole
Um die API über die Online-Konsole zu testen, benötigen Sie einige Werte, die Sie in die Konsole eingeben müssen:
    
- `teamName`: Verwenden Sie das `Id`-Feld aus dem Bildschirm mit den Anmeldeinformationen des Prüfungstools. 
- `ContentId`: Diese Zeichenfolge wird an die API übergeben und durch den Rückruf zurückgegeben. **ContentId** ist nützlich, um interne Bezeichner oder Metadaten mit den Ergebnissen eines Moderationsauftrags zu verknüpfen.- `Workflowname`: der Name des [Workflows, den Sie im vorherigen Abschnitt erstellt haben](Review-Tool-User-Guide/Workflows.md).
- `Ocp-Apim-Subscription-Key`: Befindet sich auf der Registerkarte **Einstellungen**. Weitere Informationen finden Sie in der [Übersicht](overview.md).

Greifen Sie auf die API-Konsole über das Fenster **Anmeldeinformationen** zu.

### <a name="navigate-to-the-api-reference"></a>Navigieren zur API-Referenz
Wählen Sie im Fenster **Anmeldeinformationen** die Option [API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  Die `Job.Create`-Seite wird geöffnet.

### <a name="select-your-region"></a>Auswählen Ihrer Region
Wählen Sie für **API-Testkonsole öffnen** die Region aus, die Ihrem Standort am ehesten nahekommt.
  ![Regionsauswahl auf der Seite „Auftrag – Erstellen“](images/test-drive-job-1.png)

  Die `Job.Create`-API-Konsole wird geöffnet. 

### <a name="enter-parameters"></a>Parameter eingeben

Geben Sie Werte für die gewünschten Abfrageparameter und Ihren Abonnementschlüssel ein. Das Feld **Anforderungstext** gibt den Speicherort der Informationen an, die Sie durchsuchen möchten. Verwenden wir dieses [Beispielbild](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Abfrageparameter, Header und Feld „Anforderungstext“ in der Konsole „Auftrag – Erstellen“](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Senden der Anforderung
Wählen Sie **Senden** aus. Es wird eine Auftrags-ID erstellt. Kopieren Sie diese Option, um in den nächsten Schritten zu verwenden.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Kopieren Sie diese ID für die folgenden Schritte.
Wählen Sie **Abrufen**, und öffnen Sie die API durch Auswählen der Schaltfläche, die Ihrer Region entspricht.

  ![Abrufergebnisse in der Konsole „Auftrag – Erstellen“](images/test-drive-job-4.png)

### <a name="review-the-response"></a>Antwort überprüfen

Geben Sie Werte für **teamName** und **JobID** ein. Geben Sie Ihren Abonnementschlüssel ein, und wählen Sie dann **Senden** aus. Die folgende Antwort zeigt Auftragsstatus und Details für das Beispiel an.

```
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": 
        [
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
```

## <a name="navigate-to-the-review-tool"></a>Navigieren zum Prüfungstool
Wählen Sie auf dem Content Moderator-Dashboard **Überprüfung** > **Bild**. Das durchsuchte Bild wird angezeigt und kann für die Überprüfung durch Personen verwendet werden.

  ![Prüfungstool-Bild von drei Radfahrern](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die REST-API in Ihrem Code, oder beginnen Sie mit dem [.NET-Schnellstart für Aufträge](moderation-jobs-quickstart-dotnet.md), die in Ihre Anwendung integriert werden sollen.
