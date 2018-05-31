---
title: Microsoft Azure IoT-Optionen | Microsoft-Dokumentation
description: 'Entscheiden Sie, wie Ihre Azure IoT-Lösung implementiert werden soll: per Azure IoT Central, IoT Suite oder IoT Hub.'
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 8ed524e87f820f6c1e2e05da0bcbc7241bdd1ec1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201457"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Vergleich der Optionen von Azure IoT Central und Azure IoT

Zur Implementierung einer IoT-Lösung bieten Microsoft Azure IoT Central und Azure IoT mehrere Optionen, die jeweils für verschiedene Gruppen von Kundenanforderungen geeignet sind:

* [Azure IoT Central](overview-iot-central.md) ist eine SaaS-Lösung (Software-as-a-Service) mit einem modellbasierten Ansatz, der die Entwicklung von IoT-Lösungen für Unternehmen ermöglicht, ohne dass Sie dazu mit der Entwicklung von Cloudlösungen vertraut sein müssen.

* [Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/) ist eine für Unternehmen konzipierte Sammlung mit [vorkonfigurierten Lösungen](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions) auf Basis von Azure-PaaS (Platform-as-a-Service), mit denen Sie die Entwicklung von benutzerdefinierten IoT-Lösungen beschleunigen können.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub ist die Azure PaaS-Kernanwendung, die sowohl von Azure IoT Central als auch von Azure IoT Suite genutzt wird. IoT Hub ermöglicht die zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einer Cloudlösung. Mit IoT Hub können Sie beispielsweise folgende Anforderungen der IoT-Implementierung erfüllen:

* Gerätekonnektivität und -verwaltung mit hohem Volumen
* Telemetriedatenerfassung mit hohem Volumen
* Steuerung von Geräten mit Befehlen
* Durchsetzung der Gerätesicherheit

## <a name="compare-azure-iot-central-and-azure-iot-suite"></a>Vergleich von Azure IoT Central und Azure IoT Suite

Die Auswahl Ihres Azure IoT-Produkts ist ein wichtiger Teil der Planung Ihrer IoT-Lösung. IoT Hub ist ein individueller Azure-Dienst, von dem nicht selbst eine End-to-End-IoT-Lösung bereitgestellt wird. IoT Hub kann als Ausgangspunkt für jede IoT-Lösung verwendet werden, und Sie müssen nicht Azure IoT Suite oder Azure IoT Central verwenden, um IoT Hub zu nutzen. Sowohl bei IoT Suite als auch bei Azure IoT Central wird eine Kombination aus IoT Hub und anderen Azure-Diensten genutzt. In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen IoT Suite und Azure IoT Central zusammengefasst, damit Sie die richtige Wahl für Ihre Anforderungen treffen können:

|     | Azure IoT Central | Azure IoT Suite |
| --- | ----------- | --------- |
| Primäre Nutzung                      | Verkürzen des Zeitraums bis zur Markteinführung für einfach aufgebaute IoT-Lösungen, für die keine eingehende Dienstanpassung erforderlich ist.                                                    | Beschleunigen der Entwicklung einer benutzerdefinierten IoT-Lösung, für die eine maximale Flexibilität erforderlich ist.                                                                                                                             |
| Zugriff auf zugrunde liegende PaaS-Dienste | SaaS: Vollständig verwaltete Lösung, und die zugrunde liegenden Dienste werden nicht verfügbar gemacht.                                                                                            | Sie haben Zugriff auf die zugrunde liegenden Azure-Dienste, um diese zu verwalten oder je nach Bedarf zu ersetzen.                                                                                                                    |
| Flexibilität                        | Mittel. Sie können die integrierte browserbasierte Benutzeroberfläche verwenden, um das Lösungsmodell und die Aspekte der Benutzeroberfläche anzupassen. Die Infrastruktur kann nicht angepasst werden, da die unterschiedlichen Komponenten nicht verfügbar gemacht werden. | Hoch. Der Code für die Microservices ist Open Source-Code, den Sie auf beliebige Weise ändern können. Außerdem können Sie die Bereitstellungsinfrastruktur anpassen.                                               |
| Kenntnisstand                        | Niedrig. Sie benötigen Modellierungskenntnisse, um die Lösung anzupassen. Es sind keine Codierungskenntnisse erforderlich.                                                                          | Mittelhoch. Sie benötigen Java- oder .NET-Kenntnisse, um das Lösungs-Back-End anzupassen. Sie benötigen JavaScript-Kenntnisse, um die Visualisierung anzupassen.                                                                       |
| Einstieg             | Mit Anwendungsvorlagen und Gerätevorlagen werden vorgefertigte Modelle bereitgestellt. Die Bereitstellung dauert nur wenige Minuten.                                                                                                  | Vorkonfigurierte Lösungen ermöglichen die Implementierung von häufigen IoT-Szenarien. Die Bereitstellung dauert nur wenige Minuten.                                                                                                                            |
| Preise                            | Einfache vorhersagbare Preisstruktur.                                                                                                                           | Sie können die Dienste optimieren, um die Kosten zu kontrollieren.                                                                                                                                                            |

Die Entscheidung, welches Produkt zum Erstellen Ihrer IoT-Lösung gewählt werden sollte, richtet sich letztendlich nach den folgenden Faktoren:

* Den Anforderungen Ihres Unternehmens
* Dem Typ der zu erstellenden Lösung
* Der Fähigkeit Ihrer Organisation, die Lösung zu erstellen und langfristig zu warten

## <a name="next-steps"></a>Nächste Schritte

Je nach gewähltem Produkt und Ansatz sind die folgenden nächsten Schritte zu empfehlen:

* **Azure IoT Central**: [Azure IoT Central](overview-iot-central.md)
* **IoT Suite**: [Was sind vorkonfigurierte Azure IoT-Lösungen?](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions)
* **IoT Hub**: [Übersicht über den Azure IoT Hub-Dienst](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)