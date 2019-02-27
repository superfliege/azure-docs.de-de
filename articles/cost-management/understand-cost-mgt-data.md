---
title: Verstehen der Daten in Azure Cost Management | Microsoft-Dokumentation
description: Dieser Artikel hilft Ihnen, besser zu verstehen, welche Daten in Azure Cost Management enthalten sind und wie häufig sie verarbeitet, gesammelt, angezeigt und geschlossen werden.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/07/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 1d7f58780819202eb02e3a48b4b5e0ac108a0432
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447462"
---
# <a name="understand-cost-management-data"></a>Grundlegendes zu Cost Management-Daten

Dieser Artikel hilft Ihnen, besser zu verstehen, welche Daten in Azure Cost Management enthalten sind. Und es wird erläutert, wie oft Daten verarbeitet, gesammelt, angezeigt und geschlossenen werden. Die Nutzung von Azure wird Ihnen monatlich in Rechnung gestellt. Ihre Azure-Abonnementart bestimmt jedoch, wann Ihr Abrechnungsmonat endet. Wie oft Cost Management Nutzungsdaten erhält, hängt von verschiedenen Faktoren ab. Zu diesen Faktoren gehört unter anderem, wie lange die Verarbeitung der Daten dauert und wie häufig Azure-Dienste die Nutzung an das Abrechnungssystem weitergeben.

## <a name="supported-microsoft-offers"></a>Unterstützte Microsoft-Angebote

Die folgenden Informationen zeigen die derzeit unterstützten [Microsoft Azure-Angebote](https://azure.microsoft.com/support/legal/offer-details/) im Azure Cost Management.  Ein Azure-Angebot ist der Typ von Azure-Abonnement, das Sie besitzen.

| Category (Kategorie)  | **Angebotsname** | **Angebotsnummer** |
| --- | --- | --- |
| **Azure Deutschland** | [Azure Deutschland – nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-de-0003p/) | MS-AZR-DE-0003P |
| **Azure Government** | Azure Government Enterprise | MS-AZR-USGOV-0017P |
| **Enterprise Agreement (EA)** | Enterprise Dev/Test | MS-AZR-0148P |
| **Enterprise Agreement (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade/) | MS-AZR-0017P |
| **Microsoft Developer Network (MSDN)** | [MSDN-Plattformen](https://azure.microsoft.com/offers/ms-azr-0062p/) | MS-AZR-0062P |
| **Nutzungsbasierte Bezahlung** | [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) | MS-AZR-0003P |
| **Nutzungsbasierte Bezahlung** | [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/) | MS-AZR-0023P |
| **Nutzungsbasierte Bezahlung** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/) | MS-AZR-0025P |
| **Nutzungsbasierte Bezahlung** | [Kostenlose Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/) | MS-AZR-0044P |
| **Nutzungsbasierte Bezahlung** | [Azure in Open](https://azure.microsoft.com/offers/ms-azr-0111p/) | MS-AZR-0111P |
| **Nutzungsbasierte Bezahlung** | [Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p/) | MS-AZR-0170P |
| **Nutzungsbasierte Bezahlung** | Azure Pass | MS-AZR-0120P, MS-AZR-0122P – MS-AZR-0125P, MS-AZR-0128P – MS-AZR-0130P |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p/) | MS-AZR-0029P |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/) | MS-AZR-0059P |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/) | MS-AZR-0060P |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) | MS-AZR-0063P |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR-0064P |

Die folgende Tabelle zeigt die nicht unterstützten Angebote:

| Category (Kategorie)  | **Angebotsname** | **Angebotsnummer** |
| --- | --- | --- |
| **Cloud Solution Provider (CSP)** | Microsoft Azure | MS-AZR-0145P |
| **Cloud Solution Provider (CSP)** | Azure Government CSP | MS-AZR-USGOV-0145P |
| **Cloud Solution Provider (CSP)** | Azure Deutschland in CSP für die Microsoft Cloud Deutschland | MS-AZR-DE-0145P |
| **Nutzungsbasierte Bezahlung** | Azure for Students Starter | MS-AZR-0144P |
| **Nutzungsbasierte Bezahlung** | [Microsoft Azure Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) | MS-AZR-0036P |
| **Supportpläne** | Standard-Support | MS-AZR-0041P |
| **Supportpläne** | Professional Direct-Support | MS-AZR-0042P |
| **Supportpläne** | Developer Support | MS-AZR-0043P |
| **Supportpläne** | Supportplan Deutschland | MS-AZR-DE-0043P |
| **Supportpläne** | Azure Government Standard Support | MS-AZR-USGOV-0041P |
| **Supportpläne** | Azure Government Pro-Direct Support | MS-AZR-USGOV-0042P |
| **Supportpläne** | Azure Government Developer Support | MS-AZR-USGOV-0043P |

Für Kunden mit Angeboten der Kategorie „Nutzungsbasierte Zahlung“, „MSDN“ und „Visual Studio“ stehen seit dem 02.10.2018 Daten in Cost Management zur Verfügung.

Wenn Sie keine Daten für ein Abonnement sehen und feststellen möchten, ob Ihr Abonnement zu den unterstützten Angeboten gehört, können Sie überprüfen, ob Ihr Abonnement unterstützt wird. Um zu überprüfen, ob ein Azure-Abonnement unterstützt wird, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie anschließend **Alle Dienste** im linken Menübereich aus. Wählen Sie in der Liste der Dienste **Abonnements** aus. Klicken Sie im Menü der Abonnentenliste auf das Abonnement, das Sie überprüfen möchten. Ihr Abonnement wird auf der Registerkarte „Übersicht“ angezeigt und Sie können das **Angebot** und **Angebots-ID** sehen. Die folgende Abbildung zeigt ein Beispiel.

![Beispiel für die Registerkarte „Abonnementübersicht“ mit Anzeige von Angebot und Angebots-ID](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>In Cost Management enthaltene Kosten

Die folgenden Tabellen zeigen Daten, die in Cost Management enthalten sind oder nicht.

**Kosten und Nutzungsdaten**

| **Enthalten** | **Nicht enthalten** |
| --- | --- |
| Nutzung des Azure-Diensts<sup>1</sup> | Reservierungskäufe – Weitere Informationen finden Sie unter [APIs für die Automatisierung von Azure-Reservierungen](../billing/billing-reservation-apis.md). |
| Nutzung von Marketplaceangeboten | Marketplacekäufe – Weitere Informationen finden Sie unter [Gebühren für Drittanbieterdienste](../billing/billing-understand-your-azure-marketplace-charges.md). |
|   | Supportgebühren – Weitere Informationen finden Sie unter [Erläuterung der Rechnungsbedingungen](../billing/billing-understand-your-invoice.md). |
|   | Steuern – Weitere Informationen finden Sie unter [Erläuterung der Rechnungsbedingungen](../billing/billing-understand-your-invoice.md). |
|   | Gutschriften – Weitere Informationen finden Sie unter [Erläuterung der Rechnungsbedingungen](../billing/billing-understand-your-invoice.md). |

<sup>1</sup> Die Nutzung des Azure-Dienstes basiert auf Reservierungen und ausgehandelten Preisen.

**Metadaten**

| **Enthalten** | **Nicht enthalten** |
| --- | --- |
| Ressourcentags<sup>2</sup> | Ressourcengruppen-Tags |

<sup>2</sup> Ressourcentags werden verwendet, wenn die Nutzung von jedem Dienst ausgegeben wird, und sind nicht rückwirkend für die historische Nutzung verfügbar.

## <a name="rated-usage-data-refresh-schedule"></a>Zeitplan zur Datenaktualisierung der bewerteten Nutzung

Kosten- und Nutzungsdaten sind i Cost Management + Abrechnung im Azure-Portal und in den [unterstützenden APIs](https://aka.ms/costmgmt/docs) verfügbar. Beachten Sie beim Überprüfen der Kosten folgende Punkte:

- Die geschätzten Gebühren für den aktuellen Abrechnungszeitraum werden sechsmal pro Tag aktualisiert.
- Die geschätzten Gebühren für den aktuellen Abrechnungszeitraum können sich mit zunehmender Nutzung ändern.
- Jede Aktualisierung ist kumulativ und enthält alle Einzelposten und Informationen der vorherigen Aktualisierung.
- Azure finalisiert oder _schließt_ den aktuellen Abrechnungszeitraum bis zu 72 Stunden (drei Kalendertage) nach Ablauf des Abrechnungszeitraums.

Die folgenden Beispiele veranschaulichen, wie Abrechnungszeiträume enden können.

Abonnements mit Enterprise Agreement (EA) – Wenn der Abrechnungsmonat am 31. März endet, werden die geschätzten Gebühren bis zu 72 Stunden später aktualisiert. In diesem Beispiel bis Mitternacht am (UTC) 4. April.

Abonnements mit nutzungsbasierter Bezahlung – Wenn der Abrechnungsmonat am 15. Mai endet, können die geschätzten Gebühren bis zu 72 Stunden später aktualisiert werden. In diesem Beispiel bis Mitternacht am (UTC) 19. April.

### <a name="rerated-data"></a>Neu bewertete Daten

Unabhängig davon, ob Sie die [Cost Management-APIs](https://aka.ms/costmgmt/docs), PowerBI oder das Azure-Portal zum Abrufen von Daten verwenden, ist zu erwarten, dass die Gebühren des aktuellen Abrechnungszeitraums neu bewertet werden und sich somit ändern, bis die Rechnung geschlossen ist.

## <a name="usage-data-update-frequency-varies"></a>Die Häufigkeit der Aktualisierung der Nutzungsdaten variiert.

Die Verfügbarkeit Ihrer anfallenden Nutzungsdaten in Cost Management hängt von einer Reihe von Faktoren ab, darunter:

- Wie häufig Azure-Dienste (wie z.B. Speicher, Compute, CDN und SQL) die Nutzung ausgeben.
- Die Zeit, die für die Verarbeitung der Nutzungsdaten durch die Rating-Engine und die Cost Management-Pipelines benötigt wird.

Einige Dienste geben die Nutzung häufiger als andere ab. Daher können Sie für einige Dienste Daten in Cost Management früher sehen als für andere Dienste, die seltener Daten ausgeben. In der Regel dauert es ach bis 24 Stunden bis die Nutzung von Diensten in Cost Management angezeigt wird. Beachten Sie, dass die Daten für einen offenen Monat aktualisiert werden, wenn Sie den Dienst häufiger nutzen, da Aktualisierungen kumulativ sind.

## <a name="see-also"></a>Weitere Informationen

- Falls Sie den ersten Schnellstart für Cost Management noch nicht abgeschlossen haben, lesen Sie ihn unter [Erste Schritte in der Analyse von Kosten](quick-acm-cost-analysis.md).
