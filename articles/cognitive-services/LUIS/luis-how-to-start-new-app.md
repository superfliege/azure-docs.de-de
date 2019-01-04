---
title: Erstellen einer neuen App
titleSuffix: Language Understanding - Azure Cognitive Services
description: Erstellen und verwalten Sie Ihre Anwendungen auf der LUIS-Webseite (Language Understanding Intelligent Service).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8275965e84021c41a3d0b3d13a4fb71d22090757
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139910"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Erstellen einer LUIS-App im LUIS-Portal
Es gibt zwei Verfahren zum Erstellen einer LUIS-App. Sie können eine LUIS-App im [LUIS](https://www.luis.ai)-Portal oder mithilfe der LUIS-Erstellungs-[APIs](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) erstellen.

## <a name="using-the-luis-portal"></a>Mithilfe des LUIS-Portals
Sie können eine neue App auf verschiedene Arten im LUIS-Portal erstellen:

* Beginnen Sie mit einer leeren App, und erstellen Sie Absichten, Äußerungen und Entitäten.
* Beginnen Sie mit einer leeren App, und fügen Sie eine [vordefinierte Domäne](luis-how-to-use-prebuilt-domains.md) hinzu.
* Importieren Sie eine LUIS-App aus einer JSON-Datei, die bereits Absichten, Äußerungen und Entitäten enthält.

## <a name="using-the-authoring-apis"></a>Mithilfe der Erstellungs-APIs
Sie können eine neue App mit den Erstellungs-APIs auf zwei Arten erstellen:

* [Beginnen Sie](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) mit einer leeren App, und erstellen Sie Absichten, Äußerungen und Entitäten.
* [Beginnen Sie](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) mit einer vordefinierten Domäne.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Erstellen einer neuen App in LUIS

1. Wählen Sie auf der Seite **Meine Apps** die Option **Neue App erstellen** aus.

    ![Liste der LUIS-Apps](./media/luis-create-new-app/apps-list.png)


2. Geben Sie im Dialogfeld der Anwendung den Namen „TravelAgent“.

    ![Dialogfeld „Neue App erstellen“](./media/luis-create-new-app/create-app.png)

3. Wählen Sie Ihre Anwendungskultur aus (wählen Sie für die TravelAgent-App „Englisch“ aus), und wählen Sie dann **Fertig** aus. 

    > [!NOTE]
    > Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden. 

    

<!--

## Import new app
You can set the name (50 char max), version (10 char max), and description of an app in the JSON file. Examples of application JSON files are available at [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. On **My Apps** page, select **Import new app**.
2. In the **Import new app** dialog, select the JSON file defining the LUIS app.

    ![Import a new app dialog](./media/luis-create-new-app/import-app.png)

## Export app
1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row.

    [![Screenshot of pop-up dialog of per-app actions](media/luis-create-new-app/apps-list.png "Screenshot of pop-up dialog of per-app actions")](media/luis-create-new-app/three-dots.png#lightbox)

2. Select **Export app** from the menu. 

## Rename app

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Rename** from the menu.
3. Enter the new name of the app and select **Done**.

## Delete app

> [!CAUTION]
> You are deleting the app for all collaborators and the owner. [Export](#export-app) the app before deleting it. 

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Delete** from the menu.
3. Select **Ok** in the confirmation window.

## Export endpoint logs
The logs contain the Query, UTC time, and LUIS JSON response.

1. On **My Apps** page, select the ellipsis (***...***) at the end of the app row. 
2. Select **Export endpoint logs** from the menu.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```
-->
## <a name="next-steps"></a>Nächste Schritte

Ihre erste Aufgabe in der App besteht im [Hinzufügen von Absichten](luis-how-to-add-intents.md).