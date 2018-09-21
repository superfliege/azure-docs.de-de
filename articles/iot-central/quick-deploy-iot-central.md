---
title: Erstellen einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erstellen Sie eine neue Azure IoT Central-Anwendung für die Verwaltung gekühlter Verkaufsautomaten. Zeigen Sie die Telemetriedaten an, die von Ihren simulierten Geräten generiert wurden.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/15/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: af06766d89804b2f3d0aaf061494fb836f6ec262
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465604"
---
# <a name="create-an-azure-iot-central-application"></a>Erstellen einer Azure IoT Central-Anwendung

Als _Ersteller_ verwenden Sie die Benutzeroberfläche von Azure IoT Central, um Ihre Microsoft Azure IoT Central-Anwendung zu definieren. In dieser Schnellstartanleitung wird Folgendes beschrieben:

- Erstellen einer Azure IoT Central-Anwendung mit einer exemplarischen _Gerätevorlage_ und simulierten _Geräten_
- Anzeigen der Features der Gerätevorlage **Refrigerated Vending Machine** (Gekühlter Verkaufsautomat) in Ihrer Anwendung
- Anzeigen der Telemetriedaten und Analysen Ihrer simulierten **Kühlgeräte**

In dieser Schnellstartanleitung zeigen Sie ein simuliertes **Kühlgerät** an, das auf einer Gerätevorlage basiert. Für das simulierte Gerät gilt Folgendes:

* Es sendet Telemetriedaten wie Temperatur und Druck an Ihre Anwendung.
* Es meldet Geräteeigenschaftswerte (beispielsweise einen Bewegungsalarm) an Ihre Anwendung.
* Es verfügt über Geräteeinstellungen, die Sie in der Anwendung festlegen können (beispielsweise die Lüftergeschwindigkeit).

Wenn Sie auf der Grundlage einer Gerätevorlage in einer Azure IoT Central-Anwendung ein simuliertes Gerät erstellen, können Sie anhand des simulierten Geräts Ihre Anwendung testen, bevor Sie eine Verbindung mit einem echten Gerät herstellen.

## <a name="create-the-application"></a>Erstellen der Anwendung

Für diese Schnellstartanleitung müssen Sie eine Azure IoT Central-Anwendung auf der Grundlage der Anwendungsvorlage **Beispiel „Contoso“** erstellen.

Navigieren Sie zur Azure IoT Central-Seite [Application Manager](https://aka.ms/iotcentral) (Anwendungs-Manager). Geben Sie anschließend die E-Mail-Adresse und das Kennwort für den Zugriff auf Ihr Azure-Abonnement ein:

![Eingeben Ihres Organisationskontos](media/quick-deploy-iot-central/sign-in.png)

Klicken Sie auf **Neue Anwendung**, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen:

![Azure IoT Central-Seite „Application Manager“ (Anwendungs-Manager)](media/quick-deploy-iot-central/iotcentralhome.png)

So erstellen Sie eine neue Azure IoT Central-Anwendung:

1. Wählen Sie den Zahlungsplan **Free Trial Application** (Kostenlose Testanwendung) aus.
1. Wählen Sie einen Anzeigenamen für die Anwendung (beispielsweise **Contoso IoT**). Azure IoT Central generiert automatisch ein eindeutiges URL-Präfix. Dieses URL-Präfix kann in einen einprägsameren Wert geändert werden.
1. Wählen Sie die Anwendungsvorlage **Beispiel „Contoso“** aus.
1. Wählen Sie dann **Erstellen** aus.

![Azure IoT Central-Seite „Anwendung erstellen“](media/quick-deploy-iot-central/iotcentralcreate.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure IoT Central-Anwendung erstellt, die bereits die Gerätevorlage **Refrigerated Vending Machine** und simulierte Geräte enthält. Der Artikel [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type.md) richtet sich an Ersteller und erläutert, wie Sie eigene Gerätevorlagen definieren.
