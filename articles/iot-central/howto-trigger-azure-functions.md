---
title: Auslösen von Azure Functions mithilfe von Webhooks in Azure IoT Central
description: Erstellen Sie eine Funktions-App, die immer dann ausgeführt wird, wenn eine Regel in Azure IoT Central ausgelöst wird.
author: viv-liu
ms.author: viviali
ms.date: 09/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 694c259472bf7e18f0ae3d424acf5b5cfb7ea7ab
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294253"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Auslösen von Azure Functions mithilfe von Webhooks in Azure IoT Central

*Dieses Thema gilt für Generatoren und Administratoren.*

Verwenden Sie Azure Functions, um serverlosen Code bei der Webhookausgabe aus IoT Central-Regeln auszuführen. Sie müssen keine VM bereitstellen oder eine Web-App veröffentlichen, um Azure Functions verwenden zu können. Stattdessen können Sie diesen Code serverlos ausführen. Verwenden Sie Azure Functions, um die Webhook-Nutzlast zu transformieren, bevor Sie sie an ihr endgültiges Ziel senden, z. B. eine SQL-Datenbank oder ein Event Grid. 

## <a name="prerequisites"></a>Voraussetzungen
+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="how-to-connect-azure-functions"></a>Gewusst wie: Verbinden von Azure Functions

1. [Erstellen Sie eine neue Funktions-App im Azure-Portal.](https://ms.portal.azure.com/#create/Microsoft.FunctionApp)

    ![Erstellen einer neuen Funktions-App im Azure-Portal](media/howto-trigger-azure-functions/createfunction.png)

2. Erweitern Sie die Funktions-App, und klicken Sie neben „Functions“auf die **Schaltfläche +**. Wenn dies die erste Funktion in Ihrer Funktions-App ist, wählen Sie **Benutzerdefinierte Funktion** aus. Hiermit wird der vollständige Satz von Funktionsvorlagen angezeigt.
    
    ![Auswählen einer benutzerdefinierten Funktion in der Funktions-App](media/howto-trigger-azure-functions/customfunction.png)

3. Geben Sie im Suchfeld **„generisch“** ein, und wählen Sie dann Ihre gewünschte Sprache für die generische Webhook-Triggervorlage aus. In diesem Thema wird eine C#-Funktion verwendet. 

    ![Auswählen eines generischen Webhook-Triggers](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. Klicken Sie in der neuen Funktion auf **</> Funktions-URL abrufen**, kopieren Sie den Wert, und speichern Sie ihn. Sie werden diesen Wert zum Konfigurieren des Webhooks verwenden. 

    ![Abrufen der URL für die Funktion](media/howto-trigger-azure-functions/getfunctionurl.png)
4. Navigieren Sie in IoT Central zu der Regel, die Sie mit Ihrer Funktions-App verbinden möchten.

5. Fügen Sie eine Webhookaktion hinzu. Geben Sie einen **Anzeigenamen** ein, und fügen Sie die zuvor kopierte Funktions-URL in **Rückruf-URL** ein.

    ![Die Funktions-URL im Feld „Rückruf-URL“ eingeben](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. Speichern Sie die Regel. Beim Auslösen der Regel wird der Webhook jetzt die Funktions-App zum Ausführen aufrufen. In Ihrer Funktions-App können Sie auf **Überwachen** klicken, um den Aufrufverlauf der Funktion anzuzeigen. Zum Anzeigen des Verlaufs können Sie App Insights oder die klassische Ansicht verwenden.

    ![Überwachen des Aufrufverlaufs der Funktion](media/howto-trigger-azure-functions/monitorfunction.PNG)

Weitere Informationen finden Sie im Azure Functions-Artikel zum [Erstellen einer Funktion, die durch einen generischen Webhook ausgelöst wird](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function). 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun gelernt haben, wie man Webhooks einrichtet und verwendet, erkunden Sie idealerweise als nächsten Schritt die [Erstellung von Workflows in Microsoft Flow](howto-add-microsoft-flow.md).
