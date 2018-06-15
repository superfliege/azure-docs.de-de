---
title: Microsoft Azure IoT-Optionen | Microsoft-Dokumentation
description: 'Wählen Sie, wie Ihre IoT-Lösung implementiert werden soll: per Azure IoT Solution Accelerators, Azure IoT Central oder Azure IoT Hub.'
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 56a19e9cbeb6e7d30171d23ff918a34ba423d2f2
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725580"
---
# <a name="compare-azure-iot-options"></a>Vergleich von Azure IoT-Optionen

Im Artikel [Azure und das Internet der Dinge](iot-accelerators-what-is-azure-iot.md) wird eine typische IoT-Lösungsarchitektur mit den folgenden Ebenen beschrieben:

* Gerätekonnektivität und -verwaltung
* Datenverarbeitung und Analysen
* Darstellung und Business-Integration

Zum Implementieren dieser Architektur stellt Azure IoT mehrere Optionen bereit, die jeweils für verschiedene Gruppen von Kundenanforderungen geeignet sind:

* [Azure IoT Solution Accelerators](../active-directory-domain-services/index.yml) sind eine für Unternehmen konzipierte Sammlung mit [Solution Accelerators](iot-accelerators-what-are-solution-accelerators.md) auf Basis von Azure-PaaS (Platform-as-a-Service), mit denen Sie die Entwicklung von benutzerdefinierten IoT-Lösungen beschleunigen können.

* [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions) ist eine SaaS-Lösung (Software-as-a-Service) mit einem modellbasierten Ansatz, der die Entwicklung von IoT-Lösungen für Unternehmen ermöglicht, ohne dass Sie dazu mit der Entwicklung von Cloudlösungen vertraut sein müssen.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub ist die Azure-PaaS-Kernanwendung, die sowohl von Azure IoT Central als auch von Azure IoT Solution Accelerators genutzt wird. IoT Hub ermöglicht die zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einer Cloudlösung. Mit IoT Hub können Sie beispielsweise folgende Anforderungen der IoT-Implementierung erfüllen:

* Gerätekonnektivität und -verwaltung mit hohem Volumen
* Telemetriedatenerfassung mit hohem Volumen
* Steuerung von Geräten mit Befehlen
* Durchsetzung der Gerätesicherheit

## <a name="compare-azure-iot-solution-accelerators-and-azure-iot-central"></a>Vergleich von Azure IoT Solution Accelerators und Azure IoT Central

Die Auswahl Ihres Azure IoT-Produkts ist ein wichtiger Teil der Planung Ihrer IoT-Lösung. IoT Hub ist ein individueller Azure-Dienst, von dem nicht selbst eine End-to-End-IoT-Lösung bereitgestellt wird. IoT Hub kann als Ausgangspunkt für jede IoT-Lösung verwendet werden, und Sie müssen nicht Azure IoT Solution Accelerators oder Azure IoT Central verwenden, um IoT Hub zu nutzen. Sowohl für Azure IoT Solution Accelerators als auch für Azure IoT Central wird IoT Hub zusammen mit anderen Azure-Diensten eingesetzt. In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen Azure IoT Solution Accelerators und Azure IoT Central zusammengefasst, damit Sie die richtige Auswahl für Ihre Anforderungen treffen können:

|                        | Azure IoT Solution Accelerators | Azure IoT Central |
| ---------------------- | --------- | ----------- |
| Primäre Nutzung | Beschleunigen der Entwicklung einer benutzerdefinierten IoT-Lösung, für die eine maximale Flexibilität erforderlich ist. | Verkürzen des Zeitraums bis zur Markteinführung für einfach aufgebaute IoT-Lösungen, für die keine eingehende Dienstanpassung erforderlich ist. |
| Zugriff auf zugrunde liegende PaaS-Dienste          | Sie haben Zugriff auf die zugrunde liegenden Azure-Dienste, um diese zu verwalten oder je nach Bedarf zu ersetzen. | SaaS: Vollständig verwaltete Lösung, und die zugrunde liegenden Dienste werden nicht verfügbar gemacht. |
| Flexibilität            | Hoch. Der Code für die Microservices ist Open Source-Code, den Sie auf beliebige Weise ändern können. Außerdem können Sie die Bereitstellungsinfrastruktur anpassen.| Mittel. Sie können die integrierte browserbasierte Benutzeroberfläche verwenden, um das Lösungsmodell und die Aspekte der Benutzeroberfläche anzupassen. Die Infrastruktur kann nicht angepasst werden, da die unterschiedlichen Komponenten nicht verfügbar gemacht werden.|
| Kenntnisstand                 | Mittelhoch. Sie benötigen Java- oder .NET-Kenntnisse, um das Lösungs-Back-End anzupassen. Sie benötigen JavaScript-Kenntnisse, um die Visualisierung anzupassen. | Niedrig. Sie benötigen Modellierungskenntnisse, um die Lösung anzupassen. Es sind keine Codierungskenntnisse erforderlich. |
| Einstieg | Solution Accelerators ermöglichen die Implementierung von häufigen IoT-Szenarien. Die Bereitstellung dauert nur wenige Minuten. | Mit Anwendungsvorlagen und Gerätevorlagen werden vorgefertigte Modelle bereitgestellt. Die Bereitstellung dauert nur wenige Minuten. |
| Preise                | Sie können die Dienste optimieren, um die Kosten zu kontrollieren. | Einfache vorhersagbare Preisstruktur. |

Die Entscheidung, welches Produkt zum Erstellen Ihrer IoT-Lösung gewählt werden sollte, richtet sich letztendlich nach den folgenden Faktoren:

* Den Anforderungen Ihres Unternehmens
* Dem Typ der zu erstellenden Lösung
* Der Fähigkeit Ihrer Organisation, die Lösung zu erstellen und langfristig zu warten

## <a name="next-steps"></a>Nächste Schritte

Je nach gewähltem Produkt und Ansatz sind die folgenden nächsten Schritte zu empfehlen:

* **Azure IoT Solution Accelerators**: [Was sind Azure IoT Solution Accelerators?](iot-accelerators-what-are-solution-accelerators.md)
* **Azure IoT Central**: [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions).
* **IoT Hub**: [Übersicht über den Azure IoT Hub-Dienst](../iot-hub/iot-hub-what-is-iot-hub.md)
