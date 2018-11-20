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
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515898"
---
<!-- this tells how to get the connection string for your hub -->
<!-- This assumes the user is looking at his hub in the portal. -->

Rufen Sie nach der Erstellung Ihres Hubs die Verbindungszeichenfolge für den Hub ab. Diese wird zum Verbinden von Geräten und Anwendungen mit dem Hub verwendet. 

1. Klicken Sie auf Ihren Hub, um den IoT Hub-Bereich mit Einstellungen und weiteren Elementen anzuzeigen. Klicken Sie auf **SAS-Richtlinien**.
   
2. Wählen Sie unter **Freigegebene Zugriffsrichtlinien** die Richtlinie **iothubowner** aus. 

3. Kopieren Sie unter **Schlüssel für gemeinsamen Zugriff** den Wert von **Verbindungszeichenfolge – Primärschlüssel** zur späteren Verwendung.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-include-find-connection-string/iot-hub-get-connection-string.png)

    Weitere Informationen finden Sie unter [Zugriffssteuerung](../articles/iot-hub/iot-hub-devguide-security.md) im „Entwicklerhandbuch für Azure IoT Hub“.
