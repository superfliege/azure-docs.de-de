---
title: Verwenden eines vorhandenen IoT Hubs mit dem Solution Accelerator für Gerätesimulation – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie ein benutzerdefiniertes Gerätemodell im Solution Accelerator für Gerätesimulation konfigurieren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: ee96173ca5f36dee0f08c38e4b6e29da6fee804e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967488"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Verwenden eines vorhandenen IoT Hubs mit dem Solution Accelerator für Gerätesimulation

Wenn Sie den Solution Accelerator für Gerätesimulation bereitstellen, können Sie einen IoT Hub in der Ressourcengruppe des Solution Accelerators für die Verwendung in der Simulation bereitstellen.

Wenn Sie nicht den optionalen IoT-Hub bereitstellen möchten, müssen Sie Ihren eigenen Hub für alle ausgeführten Simulationen verwenden. Wenn Sie den optionalen IoT Hub bereitstellen, können Sie diesen optionalen Hub oder Ihren eigenen Hub verwenden.

Wenn Sie keinen IoT Hub besitzen, können Sie jederzeit im [Azure-Portal](https://portal.azure.com) einen erstellen.

Um einen bereits vorhandenen IoT Hub verwenden zu können, benötigen Sie eine Verbindungszeichenfolge für die SAS-Richtlinie **iothubowner**. Sie können diese Verbindungszeichenfolge aus dem [Azure-Portal](https://portal.azure.com) abrufen:

1. Klicken Sie auf der Konfigurationsseite des Hubs im Portal auf **Freigegebene Zugriffsrichtlinien**.
1. Klicken Sie auf **iothubowner**.
1. Kopieren Sie die primäre oder die sekundäre Verbindungszeichenfolge.

[![Abrufen der Verbindungszeichenfolge](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Verwenden Sie beim Konfigurieren der Simulation die Verbindungszeichenfolge, die Sie kopiert haben:

[![Konfigurieren der Simulation](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-expanded.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden haben Sie gelernt, wie Sie einen vorhandenen IoT Hub in einer Simulation verwenden. Als Nächstes sollten Sie lernen, wie Sie [ein benutzerdefiniertes Gerätemodell für eine Simulation konfigurieren](iot-accelerators-device-simulation-custom-model.md).
