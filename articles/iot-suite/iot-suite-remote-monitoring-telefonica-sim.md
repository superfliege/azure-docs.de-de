---
title: Integrieren von SIM-Daten in die Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Telefónica-SIM-Daten in die Remoteüberwachungslösung integrieren.
services: iot-suite
suite: iot-suite
author: hegate
manager: corywink
ms.author: hegate
ms.service: iot-suite
ms.date: 04/30/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: c82bb8ff3d0f903e1de627f13337ae5fc633458c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Integrieren von SIM-Daten in die Remoteüberwachungslösung

## <a name="overview"></a>Übersicht
IoT-Geräte verwenden für die Cloudverbindung häufig eine SIM-Karte, um Datenströme von einem beliebigen Ort aus senden zu können. Die Azure IoT-Remoteüberwachungslösung ermöglicht die Integration von SIM-Verwaltungsdaten. Dadurch können Betreiber die Integrität des Geräts anhand der von der SIM-Karte bereitgestellten Daten nachverfolgen. Die Remoteüberwachung bietet eine vorkonfigurierte Integration für Telefónica IoT, die es Kunden ermöglicht, die Konnektivitätsdaten der SIM-Karten ihrer Geräte unter Verwendung der IoT-Konnektivitätsplattform von Telefónica mit der Lösung zu synchronisieren. Diese Lösung kann erweitert werden, um andere Telefonunternehmen über das GitHub-Repository zu unterstützen.
In diesem Tutorial lernen Sie Folgendes:
* Integrieren von SIM-Daten in die Remoteüberwachungslösung
* Anzeigen von Echtzeit-Telemetriedaten
* Anzeigen der SIM-Daten 

## <a name="telefonica-iot-integration-setup"></a>Einrichtung der Telefónica IoT-Integration

### <a name="prerequisites"></a>Voraussetzungen
Gehen Sie wie folgt vor, um Ihre Verbindungsdaten mit der Azure-Remoteüberwachungslösung zu synchronisieren:

1.  Füllen Sie auf der [Website von Telefónica](https://iot.telefonica.com/contact) einen Antrag aus. Wählen Sie dabei die Option **Azure Remote Monitoring** (Azure-Remoteüberwachung) aus, und geben Sie Ihre Kontaktdaten an.
2.  Daraufhin aktiviert Telefónica Ihr Konto. 
3.  Falls Sie noch kein Telefónica-Kunde sind und diesen oder andere cloudfähige IoT-Konnektivitätsdienste nutzen möchten, besuchen Sie die [Website von Telefónica](https://iot.telefonica.com/contact), und wählen Sie die Option **Connectivity** (Konnektivität) aus.

### <a name="telefonica-sim-setup"></a>Telefónica-SIM-Einrichtung
Die Zuordnung zwischen Telefónica-SIM und Azure-Gerätezwillings-ID basiert auf der Aliaseigenschaft der Telefónica IoT-SIM. 

Navigieren Sie im [Portal der Konnektivitätsplattform von Telefónica IoT](https://m2m-movistar-es.telefonica.com/) zu „SIM Inventory“ (SIM-Bestand) > „Select your SIM“ (Eigene SIM auswählen), und aktualisieren Sie jeden SIM-Alias mit der gewünschten Gerätezwillings-ID. 

Diese Aufgabe kann auch im Massenbearbeitungsmodus durchgeführt werden. (Weitere Informationen finden Sie in den Benutzerhandbüchern der Konnektivitätsplattform von Telefónica IoT.)

![Telefónica-Update](media/iot-suite-remote-monitoring-telefonica/telefonica_site.png)

Zum Herstellen der Verbindung zwischen Ihrem Gerät und der Remoteüberwachung können Sie das [Tutorial für C](iot-suite-connecting-devices-linux.md) oder das [Tutorial für Node](iot-suite-connecting-devices-node.md) verwenden. 

## <a name="view-device-telemetry-and-sim-properties"></a>Anzeigen von Gerätetelemetriedaten und SIM-Eigenschaften
Nachdem Ihr Telefónica-Konto ordnungsgemäß konfiguriert und die Verbindung mit Ihrem Gerät hergestellt wurde, können Sie Gerätedetails und SIM-Daten anzeigen.
Folgende Konnektivitätsparameter können veröffentlicht werden:
* ICCID
* IP
* Netzverfügbarkeit
* SIM-Status
* Netzbasierter Standort
* Datenverbrauch

![Dashboard](media/iot-suite-remote-monitoring-telefonica/dashboard.png)
 
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich hier einen Überblick über die Integration von SIM-Daten in die Azure IoT-Remoteüberwachungslösung verschafft haben, können Sie sich als Nächstes mit Folgendem beschäftigen:

* [Erkunden der Funktionen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-explore.md)
* [Ausführen der erweiterten Überwachung](iot-suite-remote-monitoring-monitor.md)
* [Verwalten von Geräten](iot-suite-remote-monitoring-manage.md)
* [Beheben von Geräteproblemen](iot-suite-remote-monitoring-maintain.md)

