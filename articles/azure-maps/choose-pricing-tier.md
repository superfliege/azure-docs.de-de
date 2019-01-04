---
title: Auswählen des richtigen Tarifs für Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie mehr über die von Azure Maps angebotenen Tarife
author: walsehgal
ms.author: v-musehg
ms.date: 12/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: ee277867f449afddeb89c3fd73b5b577a68a4497
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998379"
---
# <a name="choosing-the-right-pricing-tier-in-azure-maps"></a>Auswählen des richtigen Tarifs in Azure Maps

Azure Maps bietet zwei Tarife. Dieser Artikel soll Ihnen bei der Auswahl des richtigen Tarifs für Ihre Ansprüche helfen. Um den richtigen Tarif auszuwählen, sollten Sie sich zwei Fragen stellen:

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Welche Geofunktionen planen Sie, zu verwenden?
Wenn Sie meinen, dass Ihre Dienstanforderungen von den Kerngeofunktions-APIs erfüllt werden, ist der Tarif S0 der richtige für Sie. Wenn Sie erweiterte Funktionen für Ihre Anwendung benötigen, z. B. Raum- und Hybridbildgebung, Abrufen von Routenbereichen, Batchgeocodierung usw., sollten Sie den Tarif S1 in Erwägung ziehen. Die Tabelle unten mit **Tarifen und Funktionen** vermittelt Ihnen einen besseren Eindruck von den Anforderungen Ihrer Anwendung und hilft Ihnen außerdem bei der Auswahl eines Tarifs, der für Ihre Anwendung am besten geeignet ist.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Wie viele gleichzeitige Benutzer planen Sie, zu unterstützen? 
Die Tarife S0 und S1 können unterschiedliche Volumen von Datendurchsatz verarbeiten. Bevor Sie einen Azure Maps-Tarif auswählen, sollten Sie sich Fragen stellen wie „Wie viele gleichzeitige Benutzer möchten Sie unterstützen?“. Der Tarif S0 kann bis zu **50 Abfragen pro Sekunde** verarbeiten, während der Tarif S1 **mehr als 50 Abfragen pro Sekunde** verarbeiten kann.

### <a name="pricing-tier-capabilities"></a>Tarife und Funktionen

| Funktion                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Suchen,                                  |        ✓           |     ✓    |
| Routing                                 |        ✓           |     ✓    |
| Rendern                                  |        ✓           |     ✓    |
| Verkehr                                 |        ✓           |     ✓    |
| Zeitzonen                              |        ✓           |     ✓    |
| *Bildgebung und Hybridbildgebung (Preview)     |        ✓           |     ✓    |
| *Routenbereich (Preview)                  |        ✓           |     ✓    |
| *IP 2-Standort (Preview)                |        ✓           |     ✓    |
| *Polygone aus Suche (Preview)         |        ✓           |     ✓    |
| *Batchgeocodierung (Preview)              |        ✓           |     ✓    |
| *Batchrouting (Preview)                |        ✓           |     ✓    |
| *Matrixrouting (Preview)               |        ✓           |     ✓    |

> [!Note]
> Zugriff auf diese Funktionen aus dem Tarif S0 wird nach dem 4. Februar 2019 eingestellt.

Einige zusätzliche Datenpunkte, die in Betracht gezogen werden sollten, sind die Art von Unternehmen, das Sie haben, oder wie kritisch die zu erstellende Anwendung ist.

Einen besseren Eindruck von den Tarifen S0 und S1 verschafft Ihnen die Tabelle mit **Tarifen und Kundenzielgruppen**. Weitere Informationen zu den Tarifen von Azure Maps finden Sie unter [Azure Maps-Tarife](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Tarife und Kundenzielgruppen

| Tarif  |        Tarife und Kundenzielgruppen                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>Der Tarif S0 richtet sich an Kunden mit kleinen oder mittelgroßen Unternehmen. Dies ist der richtige Tarif für Sie, wenn Sie keine große Anzahl gleichzeitiger Benutzer erwarten und Ihre Dienstanforderungen von den Kerngeofunktions-APIs gemäß den Angaben in der folgenden Tabelle erfüllt werden. Dieser Tarif ist allgemein verfügbar und anwendbar auf Anwendungen in allen Phasen der Produktion, von der Entwicklung einer Machbarkeitsstudie und frühen Testphasen bis hin zur Produktion und Bereitstellung.<p>|
| S1            |    <p>Der Tarif S1 richtet sich an Kunden, die Unterstützung für große Unternehmen, unternehmenskritische Anwendungen, eine große Anzahl gleichzeitiger Benutzer benötigen oder komplexe Geodienste benötigen.</p>|


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Anzeigen und Ändern von Tarifen:

> [!div class="nextstepaction"]
> [Verwalten des Tarifs](how-to-manage-pricing-tier.md)
