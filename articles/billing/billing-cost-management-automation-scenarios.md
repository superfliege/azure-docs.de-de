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
ms.date: 6/07/2018
ms.author: erikre
ms.openlocfilehash: f84071577e9636e40d621093e2c57e3e9adf4913
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247665"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Automatisierungsszenarien zur Abrechnungs- und Kostenverwaltung

Im Folgenden werden gängige Szenarien für den Abrechnungs- und Kostenverwaltungsbereich identifiziert und verschiedenen APIs zugeordnet, die in diesen Szenarien verwendet werden können. Eine Zusammenfassung aller verfügbaren APIs und deren Funktionalität finden Sie unter der Zuordnung von Szenarien zu APIs. 

## <a name="common-scenarios"></a>Häufige Szenarien 

Sie können die APIs für Abrechnung und Kostenverwaltung in einer Vielzahl von Szenarien verwenden, um kosten- und nutzungsbezogene Fragen zu beantworten.  Ein Überblick über gängige Szenarien finden Sie unten.

- **Rechnungsabgleich**: Hat Microsoft den richtigen Betrag berechnet?  Wie sieht meine Rechnung aus, und kann ich den Betrag selbst berechnen?

- **Weiterbelastung**: Jetzt weiß ich, was mir in Rechnung gestellt wird. Wer in meiner Organisation muss diesen Rechnungsbetrag zahlen?

- **Kostenoptimierung**: Ich weiß, wie viel mir berechnet wurde, aber wie kann ich mehr aus dem Geld herausholen, das ich für Azure ausgebe?

- **Kostenverfolgung**: Ich möchte ermitteln, wie viel ich im Lauf der Zeit für Azure ausgebe und wie ich Azure nutze. Welche Trends gibt es? Was kann ich optimieren?

- **Azure-Ausgaben während des Monats**: Welche Ausgaben sind bisher im aktuellen Monat entstanden? Muss ich meine Ausgaben und/oder die Nutzung von Azure anpassen? Wann nutze ich im Lauf des Monats Azure am meisten?

- **Einrichten von Warnungen**: Ich möchte ressourcenbasierte Nutzungswarnungen oder kostenbasierte Warnungen einrichten.

## <a name="scenario-to-api-mappings"></a>Zuordnungen von Szenarien zu APIs

|         API/Szenario        | Rechnungsabgleich    | Weiterbelastung    | Kostenoptimierung    | Kostenverfolgung    | Ausgaben bis Mitte des Monats    | Warnungen    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Budgets                     |                           |                  |           X          |                  |                    |     X     |
| Marketplaces                |             X             |         X        |           X          |         X        |          X         |     X     |
| Preisblatt                 |             X             |         X        |           X          |         X        |          X         |           |
| Reservierungsempfehlungen |                           |                  |           X          |                  |                    |           |
| Reservierungsdetails         |                           |                  |           X          |         X        |                    |           |
| Reservierungszusammenfassungen       |                           |                  |           X          |         X        |                    |           |
| Nutzungsdetails               |             X             |         X        |           X          |         X        |          X         |     X     |
| Abrechnungszeiträume             |             X             |         X        |           X          |         X        |                    |           |
| Rechnungen                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Nicht bewertete Nutzung               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Die unten aufgeführten Zuordnungen von Szenarien zu APIs enthalten nicht die Enterprise-Nutzungs-APIs. Verwenden Sie nach Möglichkeit die allgemeinen Nutzungs-APIs, um zukünftige neue Entwicklungsszenarien abzudecken.

## <a name="api-summaries"></a>API-Zusammenfassungen

### <a name="consumption"></a>Nutzung
(*Web Direct- und Enterprise-Kunden für alle APIs mit Ausnahme der unten genannten*)

-   **Budgets** (*NUR Enterprise-Kunden*): Verwenden Sie die [Budgets-API](https://docs.microsoft.com/rest/api/consumption/budgets), um Kosten- oder Nutzungsbudgets für Ressourcen, Ressourcengruppen oder Abrechnungszähler zu erstellen.  Sobald Sie Budgets erstellt haben, können Sie Warnungen so konfigurieren, dass Sie benachrichtigt werden, wenn benutzerdefinierte Budgetschwellenwerte überschritten werden. Es können auch Aktionen konfiguriert werden, die auftreten, wenn Budgetwerte erreicht werden.
-   **Marketplaces**: Verwenden Sie die [API für Marketplace-Gebühren](https://docs.microsoft.com/rest/api/consumption/marketplaces), um Kosten- und Nutzungsdaten für alle Marketplace-Ressourcen abzurufen (Azure-Angebote von Drittanbietern). Diese Daten können verwendet werden, um Kosten für alle Marketplace-Ressourcen zu addieren oder die Kosten/Nutzung für bestimmte Ressourcen zu untersuchen.
-   **Preisblatt** (*NUR Enterprise-Kunden*): Enterprise-Kunden können mithilfe der [Preisblatt-API](https://docs.microsoft.com/rest/api/consumption/pricesheet) ihre benutzerdefinierten Preise für alle Nutzungsdaten abrufen. Unternehmen können diese Daten in Kombination mit Nutzungsdetails und Marketplace-Nutzungsinformationen verwenden, um Kostenberechnungen anhand von Nutzungs- und Marketplace-Daten auszuführen. 
-   **Reservierungsempfehlungen**: Verwenden Sie die [Reservierungsempfehlungs-API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations), um Empfehlungen für den Erwerb reservierter VM-Instanzen zu erhalten. Die Empfehlungen sind so konzipiert, dass die Kunden die zu erwartenden Kosteneinsparungen und Kaufbeträge analysieren können.
-   **Reservierungsdetails**: Verwenden Sie die [Reservierungsdetails-API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails), um Informationen zu bereits erworbenen VM-Reservierungen anzuzeigen, z.B. wie viel Nutzung reserviert wurde und wie viel tatsächlich genutzt wird. Sie können die Daten auf VM-Ebene im Detail anzeigen.
-   **Reservierungszusammenfassungen**: Verwenden Sie die [Reservierungszusammenfassungen-API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries), um aggregierte Informationen zu zuvor erworbenen VM-Reservierungen anzuzeigen, z.B. wie viel Nutzung reserviert wurde und wie viel tatsächlich im Aggregat verwendet wird. 
-   **Nutzungsdetails**: Verwenden Sie die [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/consumption/usagedetails) zum Abrufen von Kosten und Nutzung für alle Azure-Erstparteiressourcen. Die Informationen liegen in Form von Nutzungsdetail-Datensätzen vor, die zurzeit einmal pro Verbrauchszähler und Tag ausgegeben werden. Diese Informationen können verwendet werden, um Kosten für alle Ressourcen zu addieren oder die Kosten/Nutzung für bestimmte Ressourcen zu untersuchen.
-   **RateCard**: Web Direct-Kunden können die [RateCard-API](https://msdn.microsoft.com/library/azure/mt219005.aspx) zum Abrufen ihrer Verbrauchszählerraten verwenden. Sie können dann die zurückgegebenen Informationen mit ihren Ressourcenverbrauchsinformationen verwenden, um die zu erwartende Rechnung manuell zu berechnen. 
-   **Nicht bewertete Nutzung**: Sie können die [API für nicht bewertete Nutzung](https://msdn.microsoft.com/library/azure/mt219003.aspx) verwenden, um Nutzungsrohinformationen vor der Verbrauchszählung/Belastung zu ermitteln, die durch Azure erfolgt.

### <a name="billing"></a>Abrechnung
-   **Abrechnungszeiträume**: Verwenden Sie die [API für Abrechnungszeiträume](https://docs.microsoft.com/rest/api/billing/billingperiods), um einen zu analysierenden Abrechnungszeitraum zusammen mit den Rechnungs-IDs für diesen Zeitraum zu ermitteln. Rechnungs-IDs können mit der folgenden Rechnungs-API verwendet werden. 
-   **Rechnungen**: Verwenden Sie die [Rechnungen-API](https://docs.microsoft.com/rest/api/billing/invoices), um die Download-URL für eine Rechnung für einen bestimmten Abrechnungszeitraum im PDF-Format abzurufen.

### <a name="enterprise-consumption"></a>Enterprise-Verbrauch
*(Alle APIs NUR Enterprise)*

-   **Bilanzzusammenfassung**: Verwenden Sie die [API für Bilanz und Zusammenfassung](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary), um eine monatliche Übersicht über Informationen zu Bilanzen, neuen Käufen, Gebühren für den Azure Marketplace, Korrekturen und Überschreitungsgebühren zu erhalten. Sie können diese Informationen für den aktuellen Abrechnungszeitraum oder einen beliebigen Zeitraum in der Vergangenheit abrufen. Unternehmen können diese Daten verwenden, um einen Vergleich mit manuell berechneten Summengebühren vorzunehmen. Diese API stellt keine ressourcenspezifische Informationen und keine Aggregatansicht der Kosten bereit.
-   **Nutzungsdetails**: Verwenden Sie die [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) zum Abrufen von Azure-Erstpartei-Nutzungsdetailinformationen für den aktuellen Monat, einen bestimmten Abrechnungszeitraum oder einen benutzerdefinierten Datumszeitraum. Unternehmen können diese Daten zur manuellen Berechnung der Rechnung auf der Grundlage von Tarif und Verbrauch verwenden und auch die vorhandenen Abteilungs-/Organisationsinformationen nutzen, um Kosten organisationsübergreifend zuzuordnen. Die Daten stellen eine ressourcenspezifische Ansicht von Nutzung/Kosten bereit.
-   **Gebühr für den Marketplace Store**: Verwenden Sie die [API für die Gebühr für den Marketplace Store](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) zum Abrufen von Azure-Drittanbieter-Nutzungsdetailinformationen für den aktuellen Monat, einen bestimmten Abrechnungszeitraum oder einen benutzerdefinierten Datumszeitraum. Unternehmen können diese Daten zur manuellen Berechnung der Rechnung auf der Grundlage von Tarif und Verbrauch verwenden und auch die vorhandenen Abteilungs-/Organisationsinformationen nutzen, um Kosten organisationsübergreifend zuzuordnen. Die API für die Gebühr für den Marketplace Store bietet einen ressourcenspezifischen Überblick über Nutzung/Kosten.
-   **Preisblatt**: Die [Preisblatt-API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) stellt den zutreffenden Tarif für jede Verbrauchseinheit für den angegebenen Registrierungs- und Abrechnungszeitraum bereit. Diese Tarifinformationen können in Kombination mit Nutzungsdetails und Marketplace-Nutzungsinformationen verwendet werden, um die zu erwartende Rechnung manuell zu berechnen.
-   **Abrechnungszeiträume**: Verwenden Sie die [API für Abrechnungszeiträume](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods), um eine Liste der Abrechnungszeiträume zusammen mit einer Eigenschaft abzurufen, die auf die API-Route für die vier Sätze von Enterprise-API-Daten verweist, die zu diesem Abrechnungszeitraum gehören: BalanceSummary, UsageDetails, Marketplace-Gebühren und PriceSheet.
-   **Empfehlungen für reservierte Instanz**: Die [API für Empfehlungen für reservierte Instanz](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) untersucht die Nutzung virtueller Computer des Kunden innerhalb von 7 Tagen, 30 Tagen oder 60 Tagen und bietet Empfehlungen für Einzel- und gemeinsame Käufe. Die API für reservierte Instanz ist so konzipiert, dass die Kunden die zu erwartenden Kosteneinsparungen und empfohlene Kaufbeträge analysieren können.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Verwendung der Azure-Abrechnungs-APIs, um programmgesteuert Einblick in Ihre Azure-Nutzung zu erhalten, finden Sie unter [Azure-Abrechnungs-API: Übersicht](billing-usage-rate-card-overview.md).
