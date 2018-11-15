---
title: Support für die Azure IoT Edge-Plattform | Microsoft-Dokumentation
description: Von Azure IoT Edge unterstützte Plattformen
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 45c5c7245a2af3b0d0e328bfcc43112eaee406ee
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51565002"
---
# <a name="azure-iot-edge-support"></a>Azure IoT Edge-Support
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
| Raspbian-stretch | Nein  | JA|
| Ubuntu Server 16.04 | JA | Nein  |
| Ubuntu Server 18.04 | JA | Nein  |

Öffentliche Vorschau
| Betriebssystem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core (Update von April 2018) | JA | Nein  |
| Windows 10 IoT Enterprise (Update von April 2018) | JA | Nein  |
| Windows 10 Server 1803 | JA | Nein  |

### <a name="tier-2"></a>Ebene 2
Systeme der Ebene 2 können als mit Azure IoT Edge kompatibel betrachtet und relativ einfach verwendet werden. Dies bedeutet Folgendes:
* Microsoft hat Ad-hoc-Tests auf den Plattformen durchgeführt oder weiß, dass ein Partner Azure IoT Edge erfolgreich auf der Plattform ausführt.
* Installationspakete für andere Plattformen können ggf. auch für diese Plattformen verwendet werden.

| Betriebssystem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | JA | JA |
| Debian 8 | JA | JA |
| Debian 9 | JA | JA |
| RHEL 7.5 | JA | JA |
| Ubuntu 18.04 | JA | JA |
| Ubuntu 16.04 | JA | JA |
| Wind River 8 | JA | Nein  |
| Yocto | JA | Nein  |

## <a name="container-engines"></a>Container-Engines
Zum Starten von Modulen benötigt Azure IoT Edge eine Container-Engine (unabhängig vom verwendeten Betriebssystem). Mit „moby-engine“ stellt Microsoft eine Container-Engine bereit, um diese Anforderung zu erfüllen. Sie basiert auf dem Open-Source-Projekt „Moby“. Weitere gängige Container-Engines sind Docker CE und Docker EE. Sie basieren ebenfalls auf dem Open-Source-Projekt „Moby“ und sind mit Azure IoT Edge kompatibel. Microsoft bietet bestmöglichen Support für Systeme mit diesen Container-Engines, kann für sie aber keine Fehlerkorrekturen für interne Probleme bereitstellen. Aus diesem Grund empfiehlt Microsoft, für Produktionssysteme die Moby-Engine zu verwenden.

