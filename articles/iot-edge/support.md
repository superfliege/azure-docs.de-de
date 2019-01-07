---
title: Unterstützte Betriebssystem, Containerengines – Azure IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie, welche Betriebssysteme zur Ausführung von Azure IoT Edge-Daemon und -Runtime verwendet werden können, und welche Containerengines für Ihre Produktionsgeräte unterstützt werden.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 3c75f50e40f83a4dc6765ae422fbb2458f40d770
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100179"
---
# <a name="azure-iot-edge-supported-systems"></a>Von Azure IoT Edge unterstützte Systeme

Für Azure IoT Edge stehen verschiedene Supportoptionen zur Verfügung:

**Melden von Fehlern:** Der Großteil der Entwicklungsarbeiten für Azure IoT Edge findet im Rahmen des entsprechenden Open-Source-Projekts statt. Fehler können auf der [Problemseite](https://github.com/azure/iotedge/issues) des Projekts gemeldet werden. Fehlerbehebungen aus dem Projekt werden zeitnah in Produktupdates integriert.

**Microsoft-Kundendienstteam:** Benutzer mit einem [Supportplan](https://azure.microsoft.com/support/plans/) können direkt über das [Azure-Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac) ein Supportticket erstellen, um sich an das Microsoft-Kundendienstteam zu wenden.

**Funktionsanfragen:** Angefragte Funktionen werden von Azure IoT Edge über die [User Voice-Seite](https://feedback.azure.com/forums/907045-azure-iot-edge) des Produkts verfolgt.

## <a name="operating-systems"></a>Betriebssysteme
Azure IoT Edge ist für die meisten containerfähigen Betriebssysteme geeignet. Allerdings werden diese nicht alle gleich unterstützt. Betriebssysteme sind in Ebenen unterteilt, die jeweils den Grad der zu erwartenden Unterstützung angeben.

### <a name="tier-1"></a>Ebene 1
Systeme der Ebene 1 können als offiziell unterstützt betrachtet werden. Das bedeutet Folgendes:
* Microsoft führt für diese Betriebssysteme automatisierte Tests durch.
* Microsoft stellt Installationspakete für sie bereit.

Allgemein verfügbar
| Betriebssystem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Raspbian-stretch | Nein  | Ja|
| Ubuntu Server 16.04 | Ja | Nein  |
| Ubuntu Server 18.04 | Ja | Nein  |

Öffentliche Vorschau
| Betriebssystem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core (Update von April 2018) | Ja | Nein  |
| Windows 10 IoT Enterprise (Update von April 2018) | Ja | Nein  |
| Windows 10 Server 1803 | Ja | Nein  |

### <a name="tier-2"></a>Ebene 2
Systeme der Ebene 2 können als mit Azure IoT Edge kompatibel betrachtet und relativ einfach verwendet werden. Dies bedeutet Folgendes:
* Microsoft hat Ad-hoc-Tests auf den Plattformen durchgeführt oder weiß, dass ein Partner Azure IoT Edge erfolgreich auf der Plattform ausführt.
* Installationspakete für andere Plattformen können ggf. auch für diese Plattformen verwendet werden.

| Betriebssystem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Ja | Ja |
| Debian 8 | Ja | Ja |
| Debian 9 | Ja | Ja |
| RHEL 7.5 | Ja | Ja |
| Ubuntu 18.04 | Ja | Ja |
| Ubuntu 16.04 | Ja | Ja |
| Wind River 8 | Ja | Nein  |
| Yocto | Ja | Nein  |

## <a name="container-engines"></a>Containerengines
Zum Starten von Modulen benötigt Azure IoT Edge eine Containerengine (unabhängig vom verwendeten Betriebssystem). Mit „moby-engine“ stellt Microsoft eine Containerengine bereit, um diese Anforderung zu erfüllen. Sie basiert auf dem Open-Source-Projekt „Moby“. Weitere gängige Containerengines sind Docker CE und Docker EE. Sie basieren ebenfalls auf dem Open-Source-Projekt „Moby“ und sind mit Azure IoT Edge kompatibel. Microsoft bietet bestmöglichen Support für Systeme mit diesen Containerengines, kann für sie aber keine Fehlerkorrekturen für interne Probleme bereitstellen. Aus diesem Grund empfiehlt Microsoft, für Produktionssysteme die Moby-Engine zu verwenden.

