---
title: Includedatei
description: Includedatei
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 8d7ac457041474f4e774414b1d5e6f9ed09dc856
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146533"
---
<!-- this tells how to get the connection string for your hub -->
<!-- This assumes the user is looking at his hub in the portal. -->

Rufen Sie nach der Erstellung Ihres Hubs die Verbindungszeichenfolge für den Hub ab. Diese wird zum Verbinden von Geräten und Anwendungen mit dem Hub verwendet. 

1. Klicken Sie auf Ihren Hub, um den IoT Hub-Bereich mit Einstellungen und weiteren Elementen anzuzeigen. Klicken Sie auf **SAS-Richtlinien**.
   
2. Wählen Sie unter **Freigegebene Zugriffsrichtlinien** die Richtlinie **iothubowner** aus. 

3. Kopieren Sie unter **Schlüssel für gemeinsamen Zugriff** den Wert von **Verbindungszeichenfolge – Primärschlüssel** zur späteren Verwendung.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-include-find-connection-string/iot-hub-get-connection-string.png)

    Weitere Informationen finden Sie unter [Zugriffssteuerung](../articles/iot-hub/iot-hub-devguide-security.md) im „Entwicklerhandbuch für Azure IoT Hub“.
