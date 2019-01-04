---
title: Automatisierungsszenarien zur Abrechnungs- und Kostenverwaltung in Azure| Microsoft Docs
description: Erfahren Sie, wie gängige Abrechnungs- und Kostenverwaltungsszenarien verschiedenen APIs zugeordnet werden.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: 204b15b2-6667-4b6c-8ea4-f32c06f287fd
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/13/2018
ms.author: erikre
ms.openlocfilehash: 064f141680e62d7102d7c3332e4d93efd6c28037
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "53258745"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Automatisierungsszenarien zur Abrechnungs- und Kostenverwaltung

Im Folgenden werden gängige Szenarien für die Abrechnungs- und Kostenverwaltung identifiziert und verschiedenen APIs zugeordnet, die in diesen Szenarien verwendet werden können. Eine Zusammenfassung aller verfügbaren APIs und deren Funktionalität finden Sie unter der Zuordnung von Szenarien zu APIs.

## <a name="common-scenarios"></a>Häufige Szenarios

Sie können die APIs für Abrechnung und Kostenverwaltung in mehreren Szenarios verwenden, um kosten- und nutzungsbezogene Fragen zu beantworten.  Ein Überblick über gängige Szenarien finden Sie unten.

- **Rechnungsabgleich**: Hat Microsoft den richtigen Betrag berechnet?  Wie sieht meine Rechnung aus, und kann ich den Betrag selbst berechnen?

- **Weiterbelastung**: Jetzt weiß ich, was mir in Rechnung gestellt wird. Wer in meiner Organisation muss diesen Rechnungsbetrag zahlen?

- **Kostenoptimierung**: Ich weiß, wie viel mir berechnet wurde. Aber wie kann ich mehr für das Geld bekommen, das ich für Azure ausgebe?

- **Kostenverfolgung**: Ich möchte ermitteln, wie viel ich im Lauf der Zeit für Azure ausgebe und wie ich Azure nutze. Welche Trends gibt es? Was kann ich optimieren?

- **Azure-Ausgaben während des Monats**: Welche Ausgaben sind bisher im aktuellen Monat entstanden? Muss ich Änderungen hinsichtlich meiner Ausgaben für und/oder Nutzung von Azure vornehmen? Wann nutze ich im Lauf des Monats Azure am meisten?

- **Einrichten von Warnungen**: Ich möchte ressourcenbasierte Nutzungswarnungen oder kostenbasierte Warnungen einrichten.

## <a name="scenario-to-api-mappings"></a>Zuordnungen von Szenarien zu APIs

|         API/Szenario        | Rechnungsabgleich    | Weiterbelastung    | Kostenoptimierung    | Kostenverfolgung    | Ausgaben bis Mitte des Monats    | Alerts    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Budgets                     |                           |                  |           X          |                  |                    |     X     |
| Marketplaces                |             X             |         X        |           X          |         X        |          X         |     X     |
| Preisblatt                 |             X             |         X        |           X          |         X        |          X         |           |
| Reservierungsempfehlungen |                           |                  |           X          |                  |                    |           |
| Reservierungsdetails         |                           |                  |           X          |         X        |                    |           |
| Reservierungszusammenfassungen       |                           |                  |           X          |         X        |                    |           |
| Nutzungsdetails               |             X             |         X        |           X          |         X        |          X         |     X     |
| Abrechnungszeiträume             |             X             |         X        |           X          |         X        |                    |           |
| Invoices                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Nicht bewertete Nutzung               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Die unten aufgeführten Zuordnungen von Szenarios zu APIs enthalten nicht die Enterprise-Nutzungs-APIs. Verwenden Sie für neue Entwicklungsszenarios möglichst die allgemeinen Nutzung-APIs.

## <a name="api-summaries"></a>API-Zusammenfassungen

### <a name="consumption"></a>Nutzung
(*Web Direct- und Enterprise-Kunden für alle APIs mit Ausnahme der unten genannten*)

-   **Budgets** (*NUR Enterprise-Kunden*): Verwenden Sie die [Budgets-API](https://docs.microsoft.com/rest/api/consumption/budgets), um Kosten- oder Nutzungsbudgets für Ressourcen, Ressourcengruppen oder Abrechnungszähler zu erstellen. Nachdem Sie Budgets erstellt haben, können Sie Warnungen so konfigurieren, dass Sie benachrichtigt werden, wenn benutzerdefinierte Budgetschwellenwerte überschritten werden. Es können auch Aktionen konfiguriert werden, die auftreten, wenn Budgetwerte erreicht werden.
-   **Marketplaces**: Verwenden Sie die [API für Marketplace-Gebühren](https://docs.microsoft.com/rest/api/consumption/marketplaces), um Kosten- und Nutzungsdaten für alle Marketplace-Ressourcen abzurufen (Azure-Angebote von Drittanbietern). Diese Daten können verwendet werden, um Kosten für alle Marketplace-Ressourcen zu addieren oder die Kosten/Nutzung für bestimmte Ressourcen zu untersuchen.
-   **Preisblatt** (*NUR Enterprise-Kunden*): Enterprise-Kunden können mithilfe der [Preisblatt-API](https://docs.microsoft.com/rest/api/consumption/pricesheet) ihre benutzerdefinierten Preise für alle Zähler (Verbrauchseinheiten) abrufen. Unternehmen können diese Daten in Kombination mit Nutzungsdetails und Marketplace-Nutzungsinformationen verwenden, um Kostenberechnungen anhand von Nutzungs- und Marketplace-Daten auszuführen. 
-   **Reservierungsempfehlungen**: Verwenden Sie die [Reservierungsempfehlungs-API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations), um Empfehlungen für den Erwerb reservierter VM-Instanzen zu erhalten. Die Empfehlungen sind so konzipiert, dass die Kunden die zu erwartenden Kosteneinsparungen und Kaufbeträge analysieren können. Weitere Informationen finden Sie unter [APIs für die Automatisierung von Azure-Reservierungen](billing-reservation-apis.md).
-   **Reservierungsdetails**: Verwenden Sie die [API für Reservierungsdetails](https://docs.microsoft.com/rest/api/consumption/reservationsdetails), um Informationen zu bereits erworbenen VM-Reservierungen anzuzeigen, z. B. wie viel Nutzung ist reserviert im Vergleich zu wie viel wird genutzt. Sie können die Daten auf VM-Ebene im Detail anzeigen. Weitere Informationen finden Sie unter [APIs für die Automatisierung von Azure-Reservierungen](billing-reservation-apis.md).
-   **Reservierungszusammenfassungen**: Verwenden Sie die [Reservierungszusammenfassungen-API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries), um aggregierte Informationen zu VM-Reservierungen anzuzeigen, die Ihre Organisation erworben hat, z. B. wie viel Nutzung reserviert ist im Vergleich zur tatsächlichen Nutzung im Aggregat. Weitere Informationen finden Sie unter [APIs für die Automatisierung von Azure-Reservierungen](billing-reservation-apis.md).
-   **Nutzungsdetails**: Verwenden Sie die [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/consumption/usagedetails), um Kosten und Nutzung für alle Azure-Erstanbieterressourcen abzurufen. Die Informationen liegen in Form von Nutzungsdetail-Datensätzen vor, die zurzeit einmal pro Verbrauchszähler und Tag ausgegeben werden. Diese Informationen können verwendet werden, um Kosten für alle Ressourcen zu addieren oder die Kosten/Nutzung für bestimmte Ressourcen zu untersuchen.
-   **RateCard**: Web Direct-Kunden können die [RateCard-API](https://msdn.microsoft.com/library/azure/mt219005.aspx) verwenden, um ihre Verbrauchszählerraten abzurufen. Sie können dann die zurückgegebenen Informationen mit ihren Ressourcenverbrauchsinformationen verwenden, um die zu erwartende Rechnung manuell zu berechnen. 
-   **Nicht bewertete Nutzung**: Sie können die [API für nicht bewertete Nutzung](https://msdn.microsoft.com/library/azure/mt219003.aspx) verwenden, um Nutzungsrohinformationen vor jeglicher Verbrauchseinheitenzählung/Verrechnung abzurufen, die in Azure erfolgt.

### <a name="billing"></a>Abrechnung
-   **Abrechnungszeiträume**: Verwenden Sie die [API für Abrechnungszeiträume](https://docs.microsoft.com/rest/api/billing/billingperiods), um einen zu analysierenden Abrechnungszeitraum zusammen mit den Rechnungs-IDs für diesen Zeitraum zu bestimmen. Rechnungs-IDs können mit der folgenden Rechnungs-API verwendet werden. 
-   **Rechnungen**: Verwenden Sie die [Rechnungen-API](https://docs.microsoft.com/rest/api/billing/invoices), um die Download-URL für eine Rechnung für einen bestimmten Abrechnungszeitraum im PDF-Format abzurufen.

### <a name="enterprise-consumption"></a>Enterprise-Verbrauch
*(Alle APIs NUR Enterprise)*

-   **Saldozusammenfassung**: Verwenden Sie die [Saldozusammenfassungs-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary), um eine monatliche Übersicht über Informationen zu Salden, neuen Käufen, Gebühren für den Azure Marketplace-Dienst, Korrekturen und Überschreitungsgebühren abzurufen. Sie können diese Informationen für den aktuellen Abrechnungszeitraum oder einen beliebigen Zeitraum in der Vergangenheit abrufen. Unternehmen können diese Daten mit manuell berechneten Summengebühren vergleichen. Diese API stellt keine ressourcenspezifische Informationen und keine Aggregatansicht der Kosten bereit.
-   **Nutzungsdetails**: Verwenden Sie die [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail), um Azure-Erstanbieter-Nutzungsdetailinformationen für den aktuellen Monat, einen bestimmten Abrechnungszeitraum oder einen benutzerdefinierten Datumszeitraum abzurufen. Unternehmen können diese Daten zur manuellen Berechnung der Rechnung auf der Grundlage von Tarif und Verbrauch verwenden und auch die vorhandenen Abteilungs-/Organisationsinformationen nutzen, um Kosten organisationsübergreifend zuzuordnen. Die Daten stellen eine ressourcenspezifische Ansicht von Nutzung/Kosten bereit.
-   **Gebühr für den Marketplace Store**: Verwenden Sie die [API für die Gebühr für den Marketplace Store](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge), um Azure-Drittanbieter-Nutzungsdetailinformationen für den aktuellen Monat, einen bestimmten Abrechnungszeitraum oder einen benutzerdefinierten Datumsbereich abzurufen. Unternehmen können diese Daten zur manuellen Berechnung der Rechnung auf der Grundlage von Tarif und Verbrauch verwenden und auch die vorhandenen Abteilungs-/Organisationsinformationen nutzen, um Kosten organisationsübergreifend zuzuordnen. Die API für die Gebühr für den Marketplace Store bietet einen ressourcenspezifischen Überblick über Nutzung/Kosten.
-   **Preisblatt**: Die [Preisblatt-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) stellt den zutreffenden Tarif für jede Verbrauchseinheit für den angegebenen Registrierungs- und Abrechnungszeitraum bereit. Diese Tarifinformationen können in Kombination mit Nutzungsdetails und Marketplace-Nutzungsinformationen verwendet werden, um die zu erwartende Rechnung manuell zu berechnen.
-   **Abrechnungszeiträume**: Verwenden Sie die [API für Abrechnungszeiträume](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods), um eine Liste der Abrechnungszeiträume zusammen mit einer Eigenschaft abzurufen, die auf die API-Route für die vier Sätze von Enterprise-API-Daten verweist, die zu diesem Abrechnungszeitraum gehören: BalanceSummary, UsageDetails, Marketplace-Gebühren und PriceSheet.
-   **Empfehlungen für Azure-Reservierungen**: Die [API für Empfehlungen für reservierte Instanzen](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) untersucht die Nutzung virtueller Computer des Kunden innerhalb von 7 Tagen, 30 Tagen oder 60 Tagen und bietet Empfehlungen für Einzel- und gemeinsame Käufe. Die API für reservierte Instanz ist so konzipiert, dass die Kunden die zu erwartenden Kosteneinsparungen und empfohlene Kaufbeträge analysieren können. Weitere Informationen finden Sie unter [APIs für die Automatisierung von Azure-Reservierungen](billing-reservation-apis.md).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="what-is-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Worin besteht der Unterschied zwischen den Enterprise-Berichterstellungs-APIs und den Verbrauchs-APIs? Wann sollte ich diese jeweils verwenden?
Diese APIs weisen ähnliche Funktionen auf und können die gleiche Bandbreite an Fragen im Abrechnungs- und Kostenverwaltungsbereich beantworten. Die APIs zielen allerdings auf verschiedene Zielgruppen ab: 

- **Enterprise-Berichterstellungs-APIs**: Diese APIs stehen für Kunden zur Verfügung, die ein Enterprise Agreement mit Microsoft geschlossen haben, das ihnen Zugang zu ausgehandelten finanziellen Verpflichtungen und benutzerdefinierten Preisen gewährt. Für die APIs ist ein Schlüssel erforderlich, der aus dem [Enterprise Portal](https://ea.azure.com) abgerufen werden kann. Eine Beschreibung dieser APIs finden Sie unter [Überblick über Berichterstellungs-APIs für Enterprise-Kunden](billing-enterprise-api.md).

- **Verbrauchs-APIs**: Diese APIs sind mit einigen Ausnahmen für alle Kunden verfügbar. Weitere Informationen finden Sie unter [Azure-Nutzungs-API: Übersicht](billing-consumption-api-overview.md) und [Azure-Verbrauch](https://docs.microsoft.com/rest/api/consumption/). Die bereitgestellten APIs stellen die empfohlene Lösung für die aktuellen Entwicklungsszenarien dar. 

### <a name="what-is-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Worin besteht der Unterschied zwischen der Nutzungsdetails-API und der Nutzungs-API?
Diese APIs stellen im Grundsatz verschiedene Daten bereit:

- **Nutzungsdetails**: Die [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/consumption/usagedetails) bietet Informationen zur Nutzung und den Kosten von Azure auf Basis von Verbrauchseinheiten. Die bereitgestellten Daten haben bereits das System zur Kostenverbrauchseinheitenzählung von Azure durchlaufen. Kosten sowie andere mögliche Änderungen wurden auf diese angewendet:

    - Änderungen am Konto für die Verwendung der im Voraus bezahlten finanziellen Verpflichtungen
    - Änderungen am Konto für die Verwendung der von Azure ermittelten Abweichungen

- **Verwendung**: Die [Nutzungs-API](https://msdn.microsoft.com/library/Mt219003.aspx) bietet Informationen zu Azure-Nutzungsrohdaten, bevor diese das Kostenerfassungssystem von Azure durchlaufen. Diese Daten weisen möglicherweise keine Korrelation mit dem Nutzungsumfang oder dem Gebührenbetrag auf, der nach dem Kostenerfassungssystem von Azure zu sehen ist.

### <a name="what-is-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Worin besteht der Unterschied zwischen der Rechnungs-API und der Nutzungsdetails-API?
Diese APIs bieten unterschiedliche Ansichten derselben Daten. Die [Rechnungs-API](https://docs.microsoft.com/rest/api/billing/invoices) gilt nur für Web Direct-Kunden und bietet ein monatliches Rollup Ihrer Rechnung basierend auf den aggregierten Kosten für jeden Verbrauchseinheitentyp. Die [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/consumption/usagedetails) bietet eine präzise Ansicht der Nutzungs-/Kostendatensätze für jeden Tag und kann von Enterprise- und Web Direct-Kunden gleichermaßen verwendet werden.

### <a name="what-is-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Worin besteht der Unterschied zwischen der Preisblatt-API und der RateCard-API?
Diese APIs stellen ähnliche Datasets bereit, sind jedoch auf unterschiedliche Zielgruppen ausgerichtet. Weitere Informationen finden Sie unten.

- Preisblatt-API: Die [Preisblatt-API](https://docs.microsoft.com/rest/api/consumption/pricesheet) stellt die benutzerdefinierten Preise bereit, die für einen Enterprise-Kunden ausgehandelt wurden.
- RateCard-API: Die [RateCard-API](https://msdn.microsoft.com/library/mt219005.aspx) stellt die öffentlichen Preise für Web Direct-Kunden bereit.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Verwendung der Azure-APIs, um programmgesteuert Einblick in Ihre Azure-Nutzung zu erhalten, finden Sie unter [Azure-Nutzungs-API: Übersicht](billing-consumption-api-overview.md) und [Programmgesteuertes Abrufen von Informationen zu Ihrer Azure-Nutzung mit Azure-Abrechnungs-APIs](billing-usage-rate-card-overview.md).
- Um Ihre Rechnung mit der Datei mit den ausführlichen Daten zur täglichen Nutzung und den Kostenverwaltungsberichten im Azure-Portal zu vergleichen, lesen Sie [Grundlegendes zu Ihrer Rechnung für Microsoft Azure](billing-understand-your-bill.md).
- Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
