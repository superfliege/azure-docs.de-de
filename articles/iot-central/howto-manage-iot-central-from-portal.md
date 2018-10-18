---
title: Verwalten von IoT Central über das Azure-Portal | Microsoft-Dokumentation
description: Verwalten Sie IoT Central über das Azure-Portal.
services: iot-central
ms.service: iot-central
author: tbhagwat3
ms.author: tanmayb
ms.date: 08/30/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: ff6978ddbf8718ad45a4265898109b7ac799d70c
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46468556"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Verwalten von IoT Central über das Azure-Portal 
Zusätzlich zum Erstellen und Verwalten von IoT Central-Anwendungen über die IoT Central-Website, können Sie IoT Central auch über das Azure-Portal verwalten. Dieser Artikel führt Sie durch Ihre Möglichkeiten und die dazu erforderlichen Vorgehensweisen.

## <a name="create-iot-central-applications"></a>Erstellen von IoT Central-Anwendungen
Um eine neue Anwendung zu erstellen, navigieren Sie zum [Azure-Portal](https://ms.portal.azure.com), und klicken Sie im Hauptnavigationsmenü links auf "Ressource erstellen". 

![Verwaltungsportal: Navigationsmenü](media\howto-manage-iot-central-from-portal\image0.png)

Geben Sie in der Suchleiste den Begriff „IoT Central“ ein.

![Verwaltungsportal: Suchen](media\howto-manage-iot-central-from-portal\image0a.png)

Klicken Sie in den Suchergebnissen auf die Position „IoT Central-Anwendung“.

![Verwaltungsportal: Suchergebnisse](media\howto-manage-iot-central-from-portal\image0b.png)

Klicken Sie jetzt auf die Schaltfläche „Erstellen“, um das Formular anzuzeigen, das Sie ausfüllen müssen.

![Verwaltungsportal: IoT Central-Ressource](media\howto-manage-iot-central-from-portal\image0c.png)

Füllen Sie alle Felder im Formular aus. Dieses Formular ist ähnlich wie das Formular, das Sie ausfüllen müssen, um Anwendungen über die IoT Central-Website zu erstellen. Weitere Informationen zum Ausfüllen der einzelnen Felder finden Sie im Dokument [Erstellen einer IoT Central-Anwendung](https://docs.microsoft.com/ azure/iot-central/howto-create-application). 

![Verwaltungsportal: IoT Central-Ressource erstellen](media\howto-manage-iot-central-from-portal\image1.png)  

Nachdem Sie alle Felder ausgefüllt haben, klicken Sie auf die Schaltfläche „Erstellen“.

## <a name="manage-existing-iot-central-applications"></a>Verwalten vorhandener IoT Central-Anwendungen
Wenn Sie bereits über eine Azure IoT Central-Anwendung verfügen, können Sie sie löschen oder aber im Azure-Portal in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben. Im Azure-Portal werden keine Anwendungen auf Basis einer 7-Tage-Testversion angezeigt, weil kein Abonnement diese Testversionen unterstützt.

Klicken Sie zuerst im Hauptnavigationsmenü links auf "Alle Ressourcen". Geben Sie im Suchfeld den Namen Ihrer Anwendung ein, um sie in Ihrer Liste von Ressourcen zu finden. Klicken Sie dann auf die IoT Central-Anwendung, die Sie verwalten möchten.

![Verwaltungsportal: Ressourcenverwaltung](media\howto-manage-iot-central-from-portal\image2.png)

Um zur Anwendung zu navigieren, klicken Sie auf die URL der IoT Central-Anwendung.

![Verwaltungsportal: Ressourcenverwaltung](media\howto-manage-iot-central-from-portal\image3.png)

Um die Anwendung in eine andere Ressourcengruppe zu verschieben, klicken Sie neben der Gruppe auf den Link **Ändern**. Wählen Sie in dem daraufhin angezeigten Dialogfeld die Ressourcengruppe aus, in die Sie diese Anwendung migrieren möchten.

![Verwaltungsportal: Ressourcenverwaltung](media\howto-manage-iot-central-from-portal\image4.png)

Um die Anwendung in ein anderes Abonnement zu verschieben, klicken Sie neben dem Abonnement auf den Link **Ändern**. Wählen Sie in dem daraufhin angezeigten Dialogfeld das Abonnement aus, in das Sie diese Anwendung migrieren möchten.

![Verwaltungsportal: Ressourcenverwaltung](media\howto-manage-iot-central-from-portal\image5.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über das Azure-Portal verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)