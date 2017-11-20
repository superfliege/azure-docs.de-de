---
title: Was ist Azure IoT Edge? | Microsoft-Dokumentation
description: "Übersicht über den Azure IoT Edge-Dienst"
services: iot-Edge
documentationcenter: 
author: kgremban
manager: timlt
editor: chipalost
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: kgremban
ms.custom: 
ms.openlocfilehash: 5f69041572729d1458a22a855128639056d61586
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="what-is-azure-iot-edge---preview"></a>Was ist Azure IoT Edge? – Vorschauversion

Azure IoT Edge verschiebt Cloudanalysen und benutzerdefinierte Geschäftslogik auf Geräte, damit sich Ihre Organisation auf geschäftliche Erkenntnisse konzentrieren kann und sich nicht mit der Datenverwaltung befassen muss. Ermöglichen Sie für Ihre Lösung das richtige Skalieren, indem Sie Ihre IoT-Software konfigurieren und auf Geräten über Standardcontainer bereitstellen und alles über die Cloud überwachen.

Analysen (Analytics) sind die Grundlage des geschäftlichen Nutzens von IoT-Lösungen, aber nicht alle Analysen müssen sich in der Cloud befinden. Wenn Sie möchten, dass ein Gerät so schnell wie möglich auf Notfälle reagiert, können Sie die Anomalieerkennung auf dem Gerät selbst durchführen. Ebenso können Sie die Datenbereinigung und -aggregierung lokal durchführen, falls Sie die Bandbreitenkosten reduzieren und die Übertragung von Rohdaten im Terabyte-Bereich vermeiden möchten. Anschließend können Sie die Erkenntnisse in die Cloud senden. 

Azure IoT Edge besteht aus drei Komponenten:
* IoT Edge-Module sind Container, die Azure-Dienste, Drittanbieterdienste oder Ihren eigenen Code ausführen. Sie werden auf IoT Edge-Geräten bereitgestellt und lokal auf diesen Geräten ausgeführt. 
* Die IoT Edge-Laufzeit wird auf jedem IoT Edge-Gerät ausgeführt und dient zum Verwalten der Module, die auf einem Gerät jeweils bereitgestellt wurden. 
* Mit einer cloudbasierten Schnittstelle können Sie für IoT Edge-Geräte die Remoteüberwachung und -verwaltung durchführen.

## <a name="iot-edge-modules"></a>IoT Edge-Module

IoT Edge-Module sind Ausführungseinheiten, die derzeit als Docker-kompatible Container implementiert sind, über die Ihre Geschäftslogik im Edge-Bereich ausgeführt wird. Es können mehrere Module konfiguriert werden, die miteinander kommunizieren und eine Pipeline für die Datenverarbeitung bilden. Sie können benutzerdefinierte Module entwickeln oder bestimmte Azure-Dienste in Modulen verpacken, die offline und im Edge-Bereich Erkenntnisse liefern. 

### <a name="artificial-intelligence-on-the-edge"></a>Künstliche Intelligenz im Edge-Bereich

Mit Azure IoT Edge können Sie eine komplexe Ereignisverarbeitung, Machine Learning, Bilderkennung und andere KI-Komponenten mit hohem Nutzen bereitstellen, ohne diese intern schreiben zu müssen. Azure-Dienste wie Azure Functions, Azure Stream Analytics und Azure Machine Learning können allesamt lokal per Azure IoT Edge ausgeführt werden, aber Sie sind nicht auf Azure-Dienste beschränkt. Alle Benutzer können KI-Module erstellen und für die Community zur Verfügung stellen. 

### <a name="bring-your-own-code"></a>Verwenden von eigenem Code

Wenn Sie Ihren eigenen Code auf Ihren Geräten bereitstellen möchten, wird dies von Azure IoT Edge ebenfalls unterstützt. Azure IoT Edge enthält das gleiche Programmiermodell wie die anderen Azure IoT-Dienste. Derselbe Code kann auf einem Gerät oder in der Cloud ausgeführt werden. Azure IoT Edge unterstützt sowohl Linux als auch Windows, damit Sie für die Codierung die Plattform Ihrer Wahl nutzen können. Java, .NET Core 2.0, Node.js, C und Python werden unterstützt, damit Ihre Entwickler das Codieren in einer vertrauten Sprache durchführen und vorhandene Geschäftslogik verwenden können, anstatt sie völlig neu erstellen zu müssen.

## <a name="iot-edge-runtime"></a>IoT Edge-Laufzeit

Mit der Azure IoT Edge-Laufzeit wird die benutzerdefinierte Logik und Cloudlogik auf IoT Edge-Geräten aktiviert. Sie befindet sich auf dem IoT Edge-Gerät und ist für die Durchführung von Verwaltungs- und Kommunikationsvorgängen bestimmt. Mit der Laufzeit werden mehrere Funktionen durchgeführt:

* Installieren und Aktualisieren von Workloads auf dem Gerät
* Aufrechterhalten von Azure IoT Edge-Sicherheitsstandards auf dem Gerät
* Sicherstellen, dass die IoT Edge-Module immer ausgeführt werden
* Melden der Modulintegrität an die Cloud für die Remoteüberwachung
* Ermöglichen der Kommunikation zwischen nachgeschalteten Blattknotengeräten und dem IoT Edge-Gerät
* Ermöglichen der Kommunikation zwischen Modulen auf dem IoT Edge-Gerät
* Ermöglichen der Kommunikation zwischen dem IoT Edge-Gerät und der Cloud

![IoT Edge-Laufzeit sendet Erkenntnisse und Berichtsdaten an IoT Hub][1]

Sie können völlig frei entscheiden, wie Sie ein Azure IoT Edge-Gerät einsetzen. Die Laufzeit wird häufig zum Bereitstellen von KI-Komponenten für Gateways verwendet, mit denen Daten mehrerer anderer lokaler Geräte aggregiert und verarbeitet werden. Dies ist aber nur eine mögliche Einsatzoption. Bei Blattknotengeräten kann es sich auch um Azure IoT Edge-Geräte handeln. Dabei spielt es keine Rolle, ob sie mit einem Gateway oder direkt mit der Cloud verbunden sind.

Die Azure IoT Edge-Laufzeit kann auf einem umfangreichen IoT-Gerätesatz ausgeführt werden, damit die Laufzeit auf viele verschiedene Arten genutzt werden kann. Es werden sowohl Linux- als auch Windows-Betriebssysteme unterstützt, und Hardwaredetails werden abstrahiert. Verwenden Sie ein kleineres Gerät als einen Raspberry Pi 3, wenn Sie nicht viele Daten verarbeiten oder zentral auf einen Industrieserver hochskalieren, um ressourcenintensive Workloads auszuführen.

## <a name="iot-edge-cloud-interface"></a>IoT Edge-Cloudschnittstelle

Das Verwalten des Softwarelebenszyklus für Unternehmensgeräte ist kompliziert. Die Verwaltung des Softwarelebenszyklus für Millionen von heterogenen IoT-Geräten ist sogar noch schwieriger. Workloads müssen für einen bestimmten Typ von Gerät erstellt und konfiguriert, bedarfsabhängig auf Millionen von Geräten Ihrer Lösung bereitgestellt und dann überwacht werden, um Geräte mit Fehlfunktionen zu ermitteln. Diese Aktivitäten können nicht pro Gerät durchgeführt werden und müssen bedarfsabhängig erfolgen.

Azure IoT Edge kann nahtlos in Azure IoT Suite integriert werden, um als Steuerungsebene für die Anforderungen Ihrer Lösung zu dienen. Mit Clouddiensten haben Benutzer folgende Möglichkeiten:

* Erstellen und konfigurieren Sie eine Workload, die auf einem bestimmten Typ von Gerät ausgeführt wird.
* Senden Sie eine Workload an eine Gruppe von Geräten.
* Überwachen Sie Workloads, die auf Geräten im Feld ausgeführt werden.

![Telemetrie, Erkenntnisse und Aktionen von Geräten werden über die Cloud koordiniert.][2]

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie diese Konzepte aus, indem Sie [IoT Edge auf einem simulierten Gerät bereitstellen][lnk-quickstart].

<!-- Images -->
[1]: ./media/how-iot-edge-works/runtime.png
[2]: ./media/how-iot-edge-works/cloud-interface.png

<!-- Links -->
[lnk-quickstart]: quickstart.md