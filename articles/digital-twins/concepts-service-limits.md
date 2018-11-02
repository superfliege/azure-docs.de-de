---
title: Diensteinschränkungen der öffentlichen Vorschauversion von Azure Digital Twins | Microsoft-Dokumentation
description: Grundlegendes zu den Diensteinschränkungen der öffentlichen Vorschauversion von Azure Digital Twins
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: dwalthermsft
ms.openlocfilehash: aa5f6053bf1c98d2b84c02617da30f5d856ed3fc
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323791"
---
# <a name="public-preview-service-limits"></a>Diensteinschränkungen der öffentlichen Vorschauversion

Während der öffentlichen Vorschau (Public Preview) gelten für Azure Digital Twins temporäre Abonnement-, Instanz- und Anzahlgrenzwerte, die nachstehend beschrieben sind.

Diese Einschränkungen gibt es, um das Kennenlernen des neuen Diensts und seiner zahlreichen Funktionen vereinfachen.

> [!NOTE]
> Diese Grenzwerte werden erhöht und/oder entfernt, wenn „Allgemeine Verfügbarkeit“ (General Availability, GA) gegeben ist.

## <a name="per-subscription-limits"></a>Grenzwerte pro Abonnement

Während Public Preview kann in jedem Azure-Abonnement nur jeweils genau eine Azure Digital Twins-Instanz erstellt oder ausgeführt werden.

> [!TIP]
> Wenn Sie Ihre Instanz gelöscht haben, können Sie eine neue Instanz erstellen.

## <a name="per-instance-limits"></a>Grenzwerte pro Instanz

Der Reihe nach kann jede Azure Digital Twins-Instanz Folgendes haben:

- Eine `IoTHub`-Ressource
- Einen `EventHub`-Endpunkt für den Ereignistyp „DeviceMessage“
- Bis zu drei `EventHub`-, `ServiceBus`- oder `EventGrid`-Endpunkte des Ereignistyps `SensorChange`, `SpaceChange`, `TopologyOperation` oder `UdfCustom`

## <a name="management-api-limits"></a>Verwaltungs-API-Grenzwerte

Die Anforderungsanzahlgrenzwerte für eine Verwaltungs-API sind wie folgt:

- 100 Anforderungen pro Sekunde an die Verwaltungs-API
- Eine einzelne Verwaltungs-API-Abfrage kann bis zu 1000 Objekte zurückgeben

> [!IMPORTANT]
> Wird der Grenzwert von 1000 Objekten überschritten, erhalten Sie einen Fehler, und Sie müssen die Abfrage vereinfachen.

## <a name="udf-rate-limits"></a>Grenzwerte für die Aufrufanzahl von benutzerdefinierten Funktionen

Es gelten die folgenden Grenzwerte für die Gesamtanzahl der Aufrufe, die von benutzerdefinierten Funktionen an die Azure Digital Twins-Instanz gesendet werden:

- 400 Clientbibliotheksaufrufe pro Sekunde
- 100 SendNotification-Aufrufe pro Sekunde

> [!NOTE]
> Die folgenden Aktionen können dazu führen, dass temporär zusätzliche Anzahlgrenzwerte wirksam werden:
> - Bearbeiten der Metadaten von Topologieobjekten
> - Aktualisierungen von benutzerdefinierten Funktionen
> - Geräte, die erstmalig Telemetriedaten senden

## <a name="device-telemetry-limits"></a>Grenzwerte für Gerätetelemetrie

Die folgenden Grenzwerte deckeln die Gesamtanzahl aller Nachrichten, die Ihre Geräte an Ihre Azure Digital Twins Instanz senden können:

- 100 Nachrichten pro Sekunde

## <a name="next-steps"></a>Nächste Schritte

Um ein Azure Digital Twins-Beispiel auszuprobieren, wechseln Sie zu [Schnellstart: Suchen nach verfügbaren Räumen mithilfe von Azure Digital Twins](./quickstart-view-occupancy-dotnet.md).