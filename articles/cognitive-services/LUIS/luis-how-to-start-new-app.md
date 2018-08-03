---
title: Erstellen von Apps mit LUIS | Microsoft-Dokumentation
description: Erstellen und verwalten Sie Ihre Anwendungen auf der LUIS-Webseite (Language Understanding Intelligent Service).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: diberry
ms.openlocfilehash: 3adeecd4a4e2040a92689b7c92be9630c9a0d93b
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225413"
---
# <a name="create-an-app"></a>Erstellen einer App
Sie können eine neue App auf unterschiedliche Weise erstellen: 

* [Beginnen Sie](#create-new-app) mit einer leeren App, und erstellen Sie Absichten, Äußerungen und Entitäten.
* [Beginnen Sie](#create-new-app) mit einer leeren App, und fügen Sie eine [vordefinierte Domäne](luis-how-to-use-prebuilt-domains.md) hinzu.
* [Importieren Sie eine LUIS-App](#import-new-app) aus einer JSON-Datei, die bereits Absichten, Äußerungen und Entitäten enthält.

## <a name="what-is-an-app"></a>Was ist eine App?
Die App enthält [Versionen](luis-how-to-manage-versions.md) Ihres Modells sowie etwaige [Projektmitarbeiter](luis-how-to-collaborate.md) für die App. Wenn Sie die App erstellen, wählen Sie die Kultur ([Sprache](luis-supported-languages.md)) aus, die **später nicht mehr geändert werden kann**. 

Die Standardversion einer neuen App ist 0.1. 

Sie können Ihre Anwendungen auf der Seite **Meine Apps** erstellen und verwalten. Sie können immer auf diese Seite zugreifen, indem Sie **Meine Apps** auf der oberen Navigationsleiste der [LUIS](luis-reference-regions.md)-Website auswählen. 

[![](media/luis-create-new-app/apps-list.png "Screenshot der Liste der Apps")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Erstellen einer neuen App

1. Wählen Sie auf der Seite **Meine Apps** die Option **Neue App erstellen** aus.
2. Geben Sie im Dialogfeld der Anwendung den Namen „TravelAgent“.

    ![Dialogfeld „Neue App erstellen“](./media/luis-create-new-app/create-app.png)

3. Wählen Sie Ihre Anwendungskultur aus (wählen Sie für die TravelAgent-App „Englisch“ aus), und wählen Sie dann **Fertig** aus. 

    >[!NOTE]
    >Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden. 

## <a name="import-new-app"></a>Importieren einer neuen App
Sie können den Namen (max. 50 Zeichen), die Version (max. 10 Zeichen) und die Beschreibung einer App in der JSON-Datei festlegen. Beispiele für die JSON-Dateien einer Anwendung finden Sie in den [LUIS-Beispielen](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. Wählen Sie auf der Seite **Meine Apps** die Option **Import new app** (Neue App importieren) aus.
2. Wählen Sie im Dialogfeld **Import new app** (Neue App importieren) die JSON-Datei aus, in der die LUIS-App definiert wird.

    ![Dialogfeld „Import new app“ (Neue App importieren)](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>Exportieren einer App
1. Wählen Sie auf der Seite **Meine Apps** die Auslassungspunkte (***...***) am Ende der Zeile für die App aus.

    [![](media/luis-create-new-app/apps-list.png "Screenshot des Popupdialogfelds für Aktionen pro App")](media/luis-create-new-app/three-dots.png#lightbox)

2. Wählen Sie im Menü die Option **Export app** (App exportieren) aus. 

## <a name="rename-app"></a>Benennen der App

1. Wählen Sie auf der Seite **Meine Apps** die Auslassungspunkte (***...***) am Ende der Zeile für die App aus. 
2. Wählen Sie im Menü die Option **Umbenennen** aus.
3. Geben Sie den neuen Namen der App ein, und wählen Sie **Fertig** aus.

## <a name="delete-app"></a>Löschen einer App

> [!CAUTION]
> Sie löschen die App für alle Mitarbeiter und den Eigentümer. [Exportieren](#export-app) Sie die App, bevor Sie sie löschen. 

1. Wählen Sie auf der Seite **Meine Apps** die Auslassungspunkte (***...***) am Ende der Zeile für die App aus. 
2. Wählen Sie im Menü **Löschen** aus.
3. Wählen Sie im Bestätigungsfenster **OK** aus.

## <a name="export-endpoint-logs"></a>Exportieren von Endpunktprotokollen
Die Protokolle enthalten die Abfrage, die UTC-Zeit und die LUIS-JSON-Antwort.

1. Wählen Sie auf der Seite **Meine Apps** die Auslassungspunkte (***...***) am Ende der Zeile für die App aus. 
2. Wählen Sie im Menü **Export endpoint logs** (Endpunktprotokolle exportieren) aus.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Nächste Schritte

Ihre erste Aufgabe in der App besteht im [Hinzufügen von Absichten](luis-how-to-add-intents.md).