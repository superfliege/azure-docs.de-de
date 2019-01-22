---
title: Verwalten von IoT Central über das Azure-Portal | Microsoft-Dokumentation
description: Verwalten Sie IoT Central über das Azure-Portal.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/14/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 89109dec83342a8f4b5962778b1803eb36656e42
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352211"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Verwalten von IoT Central über das Azure-Portal

Statt IoT Central-Anwendungen über die IoT Central-Seite [Anwendungs-Manager](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie das [Azure-Portal](https://portal.azure.com) verwenden, um Ihre Anwendungen zu verwalten.

## <a name="create-iot-central-applications"></a>Erstellen von IoT Central-Anwendungen

Um eine Anwendung zu erstellen, navigieren Sie zum [Azure-Portal](https://ms.portal.azure.com), und klicken Sie im Hauptnavigationsmenü auf der linken Seite auf **Ressource erstellen**.

![Verwaltungsportal: Navigationsmenü](media/howto-manage-iot-central-from-portal/image0.png)

Geben Sie in der Suchleiste den Begriff **IoT Central** ein.

![Verwaltungsportal: Suchen](media/howto-manage-iot-central-from-portal/image0a.png)

Klicken Sie in den Suchergebnissen auf die Position **IoT Central-Anwendung**.

![Verwaltungsportal: Suchergebnisse](media/howto-manage-iot-central-from-portal/image0b.png)

Klicken Sie nun auf die Schaltfläche **Erstellen**.

![Verwaltungsportal: IoT Central-Ressource](media/howto-manage-iot-central-from-portal/image0c.png)

Füllen Sie alle Felder im Formular aus. Dieses Formular ist ähnlich wie das Formular, das Sie ausfüllen müssen, um Anwendungen auf der IoT Central-Seite [Anwendungs-Manager](https://aka.ms/iotcentral) zu erstellen. Weitere Informationen finden Sie unter dem Schnellstart [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

![Verwaltungsportal: IoT Central-Ressource erstellen](media/howto-manage-iot-central-from-portal/image1.png)  

Nachdem Sie alle Felder ausgefüllt haben, klicken Sie auf die Schaltfläche **Erstellen**.

## <a name="manage-existing-iot-central-applications"></a>Verwalten vorhandener IoT Central-Anwendungen

Wenn Sie bereits über eine Azure IoT Central-Anwendung verfügen, können Sie sie löschen oder aber im Azure-Portal in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben.

> [!NOTE]
> Testversionsanwendungen werden im Azure-Portal nicht angezeigt, da sie nicht mit Ihrem Abonnement verknüpft sind.

Klicken Sie zuerst im Hauptnavigationsmenü auf der linken Seite auf **Alle Ressourcen**. Geben Sie in das Suchfeld den Namen Ihrer Anwendung ein, um sie in Ihrer Liste von Ressourcen zu finden. Klicken Sie dann auf die IoT Central-Anwendung, die Sie verwalten möchten.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image2.png)

Um zur Anwendung zu navigieren, klicken Sie auf die URL der IoT Central-Anwendung.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image3.png)

Um die Anwendung in eine andere Ressourcengruppe zu verschieben, klicken Sie auf **Ändern** neben der Ressourcengruppe. Wählen Sie auf der Seite **Ressourcen verschieben** die Ressourcengruppe aus, in die Sie diese Anwendung migrieren möchten.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image4.png)

Um die Anwendung in ein anderes Abonnement zu verschieben, klicken Sie neben dem Abonnement auf den Link **Ändern**. Wählen Sie in dem daraufhin angezeigten Dialogfeld das Abonnement aus, in das Sie diese Anwendung migrieren möchten.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image5.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über das Azure-Portal verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)