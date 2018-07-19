---
title: Includedatei
description: Includedatei
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/25/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: bacafdc8f7fd8e206335f3be0a086df1c54f1081
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38746708"
---
Erstellen Sie eine Geräteidentität für das simulierte Gerät, sodass es mit dem IoT Hub kommunizieren kann. Da IoT Edge-Geräte sich von typischen IoT-Geräten unterscheiden und auf unterschiedliche Weise verwaltet werden können, deklarieren Sie dieses Gerät von Anfang an als IoT Edge-Gerät. 

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.
1. Klicken Sie auf **IoT Edge** und dann auf **Add IoT Edge Device** (IoT Edge-Gerät hinzufügen).

   ![Hinzufügen eines IoT Edge-Geräts](./media/iot-edge-register-device/add-device.png)

1. Weisen Sie dem simulierten Gerät eine eindeutige Geräte-ID zu.
1. Wählen Sie **Speichern** aus, um das Gerät hinzuzufügen.
1. Wählen Sie das neue Gerät in der Liste der Geräte aus.
1. Kopieren Sie den Wert für **Verbindungszeichenfolge – Primärschlüssel**, und speichern Sie ihn. Sie verwenden diesen Wert, um im nächsten Abschnitt die IoT Edge-Runtime zu konfigurieren. 

