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
ms.openlocfilehash: a8160e677fa99d8cb691db39d7f29ba6eddbd261
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004678"
---
## <a name="enabling-extended-offline-operation-preview"></a>Aktivieren des erweiterten Offlinebetriebs (Vorschau)
Ab Release [v1.0.2](https://aka.ms/edge102) der Edge-Runtime können das Edge-Gerät und Downstreamgeräte, die mit ihm verbunden sind, für einen erweiterten Offlinebetrieb konfiguriert werden. 

Mit dieser Funktion können lokale Module oder Downstreamgeräte bei Bedarf eine erneute Authentifizierung mit dem Edge-Gerät durchführen und über Nachrichten und Methoden miteinander kommunizieren, auch wenn sie vom IoT Hub getrennt sind. Weitere Details sowie Informationen zum Umfang dieser Funktion finden Sie in diesem [Blogbeitrag](https://aka.ms/iot-edge-offline) und in diesem [Konzeptartikel](../articles/iot-edge/offline-capabilities.md).

Für die Aktivierung der erweiterten Offlinefunktion in einem Gatewayszenario stellen Sie eine über-/untergeordnete Beziehung zwischen dem Edge-Gerät und Downstreamgeräten her, die eine Verbindung mit diesem herstellen.

1. Klicken Sie auf dem Blatt „Edge-Gerätedetails“ im IoT Hub-Portal in der oberen Befehlsleiste auf die Schaltfläche **Untergeordnete Geräte verwalten (Vorschau)**.

1. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.

1. Wählen Sie in der Geräteliste die untergeordneten Geräte aus, und verwenden Sie den Pfeil nach rechts, um die Geräte auszuwählen, die als untergeordnete Geräte hinzugefügt werden sollen.

1. Klicken Sie auf **OK** , um zu bestätigen.

1. Klicken Sie im Bildschirm **Module festlegen** der Details des Edge-Geräts auf **Erweiterte Einstellungen der Edge-Runtime konfigurieren**, und fügen Sie unter **Edge Hub**-Umgebungsvariablen einen Eintrag **UpstreamProtocol** mit dem Wert **MQTT** hinzu. Fügen Sie die gleiche Umgebungsvariable und den Wert auch für den **Edge-Agent** hinzu. 

1. Klicken Sie auf **Speichern**, und stellen Sie sicher, dass die Änderungen **übermittelt** werden, nachdem Sie zwei Mal auf **Weiter** geklickt haben.

Das Edge-Gerät und seine untergeordneten Geräte sind nun für den erweiterten Offlinebetrieb aktiviert.  