---
title: Überwachen von Azure Media Services-Ereignissen mit Event Grid über das Portal | Microsoft-Dokumentation
description: In diesem Artikel wird gezeigt, wie Sie ein Abonnement für Event Grid abschließen können, um Azure Media Services-Ereignisse zu überwachen.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, streamen, übertragen, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 4e6527bf115f327635a0b0fe187094dafb320598
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49380604"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-portal"></a>Erstellen und Überwachen von Media Services-Ereignissen mit Event Grid über das Azure-Portal

Azure Event Grid ist ein Ereignisdienst für die Cloud. In diesem Artikel verwenden Sie das Azure-Portal, um Ereignisse für Ihr Azure Media Services-Konto zu abonnieren. Anschließend lösen Sie Ereignisse aus, um das Ergebnis anzuzeigen. Üblicherweise senden Sie Ereignisse an einen Endpunkt, der die Ereignisdaten verarbeitet und entsprechende Aktionen ausführt. In diesem Artikel senden wir Ereignisse an eine Web-App, die die Nachrichten sammelt und anzeigt.

Am Ende sehen Sie, dass die Ereignisdaten an die Web-App gesendet wurden.

## <a name="prerequisites"></a>Voraussetzungen 

* Ein aktives Azure-Abonnement
* Erstellen Sie ein Azure Media Services-Konto, wie in [dieser Schnellstartanleitung](create-account-cli-quickstart.md) beschrieben.

## <a name="create-a-message-endpoint"></a>Erstellen eines Nachrichtenendpunkts

Vor dem Abonnieren der Ereignisse für das Media Services-Konto erstellen wir zunächst den Endpunkt für die Ereignisnachricht. Der Endpunkt führt in der Regel Aktionen auf der Grundlage der Ereignisdaten aus. In diesem Artikel stellen Sie eine [vorgefertigte Web-App](https://github.com/Azure-Samples/azure-event-grid-viewer) bereit, die die Ereignismeldungen anzeigt. Die bereitgestellte Lösung umfasst einen App Service-Plan, eine App Service-Web-App und Quellcode von GitHub.

1. Wählen Sie **Deploy to Azure** (In Azure bereitstellen), um die Lösung für Ihr Abonnement bereitzustellen. Geben Sie im Azure-Portal Werte für die Parameter an.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Die Bereitstellung kann einige Minuten dauern. Nach erfolgreichem Abschluss der Bereitstellung können Sie Ihre Web-App anzeigen und sich vergewissern, dass sie ausgeführt wird. Navigieren Sie hierzu in einem Webbrowser zu `https://<your-site-name>.azurewebsites.net`.

Wenn Sie zur Seite „Azure Event Grid Viewer“ wechseln, werden Sie erkennen, dass sie noch nicht über Ereignisse verfügt.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="subscribe-to-media-services-events"></a>Abonnieren von Media Services-Ereignissen

Sie abonnieren ein Thema, um Event Grid mitzuteilen, welche Ereignisse Sie nachverfolgen möchten und wohin diese gesendet werden sollen.

1. Wählen Sie im Portal Ihr Media Services-Konto und dann **Ereignisse** aus.
1. Verwenden Sie zum Senden von Ereignissen an Ihre Viewer-App einen Webhook für den Endpunkt. 

   ![Auswählen von „Webhook“](./media/monitor-events-portal/select-web-hook.png)

1. In das Ereignisabonnement wurden die Werte für Ihr Media Services-Konto bereits eingefügt. 
1. Wählen Sie als **Endpunkttyp** die Option „Webhook“ aus.
1. Unter diesem Thema lassen wir das Kontrollkästchen **Alle Ereignistypen abonnieren** aktiviert. Sie können es jedoch deaktivieren und nach bestimmten Ereignistypen filtern. 
1. Klicken Sie auf den Link **Endpunkt auswählen**.

    Geben Sie für den Webhookendpunkt die URL Ihrer Web-App an, und fügen Sie `api/updates` der URL der Startseite hinzu. 

1. Drücken Sie auf **Auswahl bestätigen**.
1. Klicken Sie auf **Erstellen**.
1. Geben Sie Ihrem Abonnement einen Namen.

   ![Auswählen von „Protokolle“](./media/monitor-events-portal/create-subscription.png)

1. Zeigen Sie wieder Ihre Web-App an. Wie Sie sehen, wurde ein Abonnementüberprüfungsereignis an sie gesendet. 

    Event Grid sendet das Überprüfungsereignis, damit der Endpunkt bestätigen kann, dass er Ereignisdaten empfangen möchte. Der Endpunkt muss `validationResponse` auf `validationCode` festlegen. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](../../event-grid/security-authentication.md). Sie können den Web-App-Code anzeigen, um zu sehen, wie das Abonnement überprüft wird.

Anschließend werden Ereignisse ausgelöst, um zu sehen, wie Event Grid die Nachricht an Ihren Endpunkt weiterleitet.

## <a name="send-an-event-to-your-endpoint"></a>Senden eines Ereignisses an Ihren Endpunkt

Sie können Ereignisse für das Media Services-Konto auslösen, indem Sie einen Codierungsauftrag ausführen. Sie können [dieser Schnellstartanleitung](stream-files-dotnet-quickstart.md) folgen, um eine Datei zu codieren und mit dem Senden von Ereignissen zu beginnen. Wenn Sie alle Ereignisse abonniert haben, wird ein Bildschirm wie der folgende angezeigt:

> [!TIP]
> Klicken Sie auf das Augensymbol, um die Ereignisdaten zu erweitern. Aktualisieren Sie die Seite nicht, wenn Sie alle Ereignisse anzeigen möchten.

![Anzeigen des Abonnementereignisses](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Nächste Schritte

[Hochladen, Codieren und Streamen](stream-files-tutorial-with-api.md)
