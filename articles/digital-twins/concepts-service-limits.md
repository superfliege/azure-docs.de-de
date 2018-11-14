---
title: Diensteinschränkungen der öffentlichen Vorschauversion von Azure Digital Twins | Microsoft-Dokumentation
description: Grundlegendes zu den Diensteinschränkungen der Public Preview von Azure Digital Twins
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dwalthermsft
ms.openlocfilehash: 86ae75118dd1311ea2ae92fb718fe4c58b8e5673
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961754"
---
# <a name="public-preview-service-limits"></a>Diensteinschränkungen der öffentlichen Vorschauversion

Für die Public Preview von Azure Digital Twins gelten die nachfolgend beschriebenen temporären Abonnement-, Instanz- und Anzahlgrenzwerte.

Diese Einschränkungen gibt es, um das Kennenlernen des neuen Diensts und seiner zahlreichen Funktionen vereinfachen.

> [!NOTE]
> Die Grenzwerte werden gemäß der allgemeinen Verfügbarkeit erhöht oder entfernt.

## <a name="per-subscription-limits"></a>Grenzwerte pro Abonnement

In der Public Preview kann jedes Azure-Abonnement jeweils genau eine Azure Digital Twins-Instanz erstellen oder ausführen.

> [!TIP]
> Wenn Sie Ihre Instanz löschen, können Sie eine neue erstellen.

## <a name="per-instance-limits"></a>Grenzwerte pro Instanz

Der Reihe nach kann jede Azure Digital Twins-Instanz Folgendes haben:

- Eine **IoTHub**-Ressource
- Einen **EventHub**-Endpunkt für den Ereignistyp **DeviceMessage**
- Bis zu drei **EventHub**-, **ServiceBus**- oder **EventGrid**-Endpunkte der Ereignistypen **SensorChange**, **SpaceChange**, **TopologyOperation** oder **UdfCustom**

## <a name="management-api-limits"></a>Verwaltungs-API-Grenzwerte

Die Anforderungsanzahlgrenzwerte für eine Verwaltungs-API sind wie folgt:

- 100 Anforderungen pro Sekunde an die Verwaltungs-API
- Bis zu 1.000 Objekte, die von einer einzelnen Verwaltungs-API-Abfrage zurückgegeben werden 

> [!IMPORTANT]
> Beim Überschreiten des Grenzwerts von 1.000 Objekten tritt ein Fehler auf, und Sie müssen die Abfrage vereinfachen.

## <a name="udf-rate-limits"></a>Grenzwerte für die Aufrufanzahl von benutzerdefinierten Funktionen

Es gelten die folgenden Grenzwerte für die Gesamtanzahl der Aufrufe, die von benutzerdefinierten Funktionen an die Azure Digital Twins-Instanz gesendet werden:

- 400 Clientbibliotheksaufrufe pro Sekunde
- 100 **SendNotification**-Aufrufe pro Sekunde

> [!NOTE]
> Die folgenden Aktionen führen ggf. dazu, dass temporär zusätzliche Anzahlgrenzwerte angewendet werden:
> - Bearbeitungen an den Metadaten von Topologieobjekten
> - Updates von Definitionen benutzerdefinierter Funktionen
> - Erstmaliges Senden von Telemetriedaten von Geräten

## <a name="device-telemetry-limits"></a>Grenzwerte für Gerätetelemetrie

Die folgenden Grenzwerte deckeln die Gesamtanzahl aller Nachrichten, die Ihre Geräte an Ihre Azure Digital Twins Instanz senden können:

- 100 Nachrichten pro Sekunde

## <a name="next-steps"></a>Nächste Schritte

Um ein Azure Digital Twins-Beispiel auszuprobieren, wechseln Sie zu [Schnellstart: Suchen nach verfügbaren Räumen mithilfe von Azure Digital Twins](./quickstart-view-occupancy-dotnet.md).