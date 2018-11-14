---
title: Includedatei
description: IoT Edge
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: kgremban
ms.openlocfilehash: be4d82577584e83e29f2511d51256fda0970e917
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264063"
---
## <a name="enabling-extended-offline-operation-preview"></a>Aktivieren des erweiterten Offlinebetriebs (Vorschau)
Mit [Release v1.0.4](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) der Edge-Runtime können das Edge-Gerät und Downstreamgeräte, die mit ihm verbunden sind, für einen erweiterten Offlinebetrieb konfiguriert werden. 

Mit dieser Funktion können lokale Module oder Downstreamgeräte bei Bedarf eine erneute Authentifizierung mit dem Edge-Gerät durchführen und über Nachrichten und Methoden miteinander kommunizieren, auch wenn sie vom IoT Hub getrennt sind. Weitere Details sowie Informationen zum Umfang dieser Funktion finden Sie in diesem [Blogbeitrag](https://aka.ms/iot-edge-offline) und in diesem [Konzeptartikel](../articles/iot-edge/offline-capabilities.md).

Für die Aktivierung der erweiterten Offlinefunktion in einem Gatewayszenario stellen Sie eine über-/untergeordnete Beziehung zwischen dem Edge-Gerät und Downstreamgeräten her, die eine Verbindung mit diesem herstellen.

1. Klicken Sie auf dem Blatt „Edge-Gerätedetails“ im IoT Hub-Portal in der oberen Befehlsleiste auf die Schaltfläche **Untergeordnete Geräte verwalten (Vorschau)**.

1. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.

1. Wählen Sie in der Geräteliste die untergeordneten Geräte aus, und verwenden Sie den Pfeil nach rechts, um die Geräte auszuwählen, die als untergeordnete Geräte hinzugefügt werden sollen.

1. Klicken Sie auf **OK** , um zu bestätigen.

Das Edge-Gerät und seine untergeordneten Geräte sind nun für den erweiterten Offlinebetrieb aktiviert.  