---
title: Erstellen einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erstellen Sie eine neue Azure IoT Central-Anwendung für die Verwaltung gekühlter Verkaufsautomaten. Zeigen Sie die Telemetriedaten an, die von Ihren simulierten Geräten generiert wurden.
author: tbhagwat3
ms.author: tanmayb
ms.date: 10/12/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: c6ed1f0feaa9b8b20d291be7929228707281cf9b
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158090"
---
# <a name="create-an-azure-iot-central-application"></a>Erstellen einer Azure IoT Central-Anwendung

Als _Ersteller_ verwenden Sie die Benutzeroberfläche von Azure IoT Central, um Ihre Microsoft Azure IoT Central-Anwendung zu definieren. In dieser Schnellstartanleitung wird gezeigt, wie Sie eine Azure IoT Central-Anwendung mit einer exemplarischen _Gerätevorlage_ und simulierten _Geräten_ erstellen.

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
