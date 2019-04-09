---
title: Erstellen einer neuen App
titleSuffix: Language Understanding - Azure Cognitive Services
description: Erstellen und verwalten Sie Ihre Anwendungen auf der LUIS-Webseite (Language Understanding Intelligent Service).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/28/2019
ms.author: diberry
ms.openlocfilehash: 72c4f23f47e0a2c6d9a96dbbe36716bc3ab665f1
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891428"
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

## <a name="import-an-app-from-file"></a>Importieren einer App aus einer Datei

1. Wählen Sie auf der Seite **Meine Apps** die Option **Import new app** (Neue App importieren) aus.
1. Wählen Sie im Popupdialogfeld eine gültige App-JSON-Datei und dann **Fertig** aus.

### <a name="import-errors"></a>Importfehler

Mögliche Fehler: 

* Eine App mit diesem Namen ist bereits vorhanden. Importieren Sie die App erneut, und legen Sie für **Optionaler Name** einen neuen Namen fest. 

## <a name="export-app"></a>Exportieren einer App

1. Wählen Sie auf der Seite **Meine Apps** die Option **Import new app** (Neue App importieren) aus.
1. Wählen Sie im Dialogfeld **Import new app** (Neue App importieren) die JSON-Datei aus, in der die LUIS-App definiert wird.

## <a name="delete-app"></a>Löschen einer App

1. Wählen Sie auf der Seite **Meine Apps** die drei Punkte („...“) am Ende der Zeile für die App aus.
1. Wählen Sie im Menü **Löschen** aus.
1. Wählen Sie im Bestätigungsfenster **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

Ihre erste Aufgabe in der App besteht im [Hinzufügen von Absichten](luis-how-to-add-intents.md).
