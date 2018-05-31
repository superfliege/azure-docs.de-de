---
title: Integrieren von SIM-Daten in die Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Telefónica-SIM-Daten in die Remoteüberwachungslösung integrieren.
services: iot-suite
suite: iot-suite
author: hegate
manager: timlt
ms.author: hegate
ms.service: iot-suite
ms.date: 05/15/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 590a24113d66edacc6edcfe988330f643f1aa57a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34367552"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Integrieren von SIM-Daten in die Remoteüberwachungslösung

IoT-Geräte verwenden für die Cloudverbindung häufig eine SIM-Karte, um Datenströme von einem beliebigen Ort aus senden zu können. Die Azure IoT-Remoteüberwachungslösung ermöglicht die Integration von Verbindungsdaten, die über IoT verwaltet werden. Dadurch können Betreiber die Integrität des Geräts anhand der von der IoT SIM-Karte bereitgestellten Daten nachverfolgen.

Die Remoteüberwachung bietet eine vorkonfigurierte Integration in die Telefónica-IoT-Konnektivität, die es Kunden ermöglicht, die Konnektivitätsdaten der SIM-Karten ihrer Geräte unter Verwendung der IoT-Konnektivitätsplattform von Telefónica mit ihrer Lösung zu synchronisieren. Diese Lösung kann erweitert werden, um andere IoT-Konnektivitätsanbieter über das GitHub-[Repository](http://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) zu unterstützen.

In diesem Tutorial lernen Sie Folgendes:

* Integrieren von Telefónica-IoT-SIM-Daten in die Remoteüberwachungslösung
* Anzeigen von Echtzeit-Telemetriedaten
* Anzeigen der SIM-Daten

## <a name="telefnica-iot-integration-setup"></a>Einrichtung der Telefónica-IoT-Integration

### <a name="prerequisites"></a>Voraussetzungen

Diese zusätzliche Remoteüberwachungsfunktion ist derzeit als Vorschau verfügbar. Gehen Sie wie folgt vor, um Ihre Verbindungsdaten mit der Azure-Remoteüberwachungslösung zu synchronisieren:

1. Füllen Sie auf der [Website von Telefónica](https://iot.Telefónica.com/contact) einen Antrag aus. Wählen Sie dabei die Option **Azure Remote Monitoring** (Azure-Remoteüberwachung) aus, und geben Sie Ihre Kontaktdaten an.
2. Telefónica aktiviert Ihr Konto.
3. Falls Sie noch kein Telefónica-Kunde sind und diesen oder andere cloudfähige IoT-Konnektivitätsdienste nutzen möchten, besuchen Sie die [Website von Telefónica](https://iot.Telefónica.com/contact), und wählen Sie die Option **Connectivity** (Konnektivität) aus.

### <a name="telefnica-sim-setup"></a>Telefónica-SIM-Einrichtung
Die Zuordnung zwischen Telefónica-SIM und Azure-Gerätezwillings-ID basiert auf der Aliaseigenschaft der Telefónica-IoT-SIM. 

Navigieren Sie im [Portal der Konnektivitätsplattform von Telefónica IoT](https://m2m-movistar-es.telefonica.com/) zu „SIM Inventory“ (SIM-Bestand) > „Select your SIM“ (Eigene SIM auswählen), und aktualisieren Sie jeden SIM-„Alias“ mit der gewünschten Gerätezwillings-ID. Diese Aufgabe kann auch im Massenbearbeitungsmodus durchgeführt werden. (Weitere Informationen finden Sie in den Benutzerhandbüchern der Konnektivitätsplattform von Telefónica IoT.)

Diese Aufgabe kann auch im Massenbearbeitungsmodus durchgeführt werden. (Weitere Informationen finden Sie in den Benutzerhandbüchern der Konnektivitätsplattform von Telefónica IoT.)

![Telefónica-Update](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Zum Herstellen der Verbindung zwischen Ihrem Gerät und der Remoteüberwachung können Sie das [Tutorial für C](iot-accelerators-connecting-devices-linux.md) oder das [Tutorial für Node](iot-accelerators-connecting-devices-node.md) verwenden. 

## <a name="view-device-telemetry-and-sim-properties"></a>Anzeigen von Gerätetelemetriedaten und SIM-Eigenschaften

Nachdem Ihr Telefónica-Konto ordnungsgemäß konfiguriert und die Verbindung mit Ihrem Gerät hergestellt wurde, können Sie Gerätedetails und SIM-Daten anzeigen.

Folgende Konnektivitätsparameter werden veröffentlicht:

* ICCID
* IP
* Netzverfügbarkeit
* SIM-Status
* Netzbasierter Standort
* Datenverbrauch

![Dashboard](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich hier einen Überblick über die Integration von SIM-Daten in die Azure IoT-Remoteüberwachungslösung verschafft haben, können Sie sich als Nächstes mit Folgendem beschäftigen:

* [Erkunden der Funktionen der vorkonfigurierten Remoteüberwachungslösung](iot-accelerators-remote-monitoring-explore.md)
* [Ausführen der erweiterten Überwachung](iot-accelerators-remote-monitoring-monitor.md)
* [Verwalten von Geräten](iot-accelerators-remote-monitoring-manage.md)
* [Beheben von Geräteproblemen](iot-accelerators-remote-monitoring-maintain.md)

