---
title: Abrechnungsoptionen im Azure Marketplace | Azure
description: Abrechnungsoptionen im Azure Marketplace für Herausgeber.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: d0dbf603c600639679e121723556a7f3ceb17e37
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158980"
---
# <a name="billing-options-in-the-azure-marketplace"></a>Abrechnungsoptionen im Azure Marketplace

In diesem Artikel werden Abrechnungsoptionen beschrieben, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) verfügbar sind.

## <a name="commercial-considerations-in-the-marketplace"></a>Kommerzielle Aspekte im Marketplace
Im Marketplace erfolgt keine Aufteilung des Umsatzes für die folgenden Arten von Listings: 
*   Auflisten
*   Testversion
*   Transaktionen über das BYOL-Abrechnungsmodell (Bring Your Own License)

Für die Teilnahme an den Storefronts im Marketplace werden Ihnen keine zusätzlichen Gebühren berechnet.

Weitere Informationen finden Sie in den [Microsoft Azure Marketplace-Teilnahmerichtlinien](https://azure.microsoft.com/support/legal/marketplace/participation-policies).  

## <a name="pay-as-you-go-and-byol-billing-options"></a>Nutzungsbasierte und BYOL-Abrechnungsoptionen
Wenn Sie ein Abrechnungsmodell mit nutzungsbasierter Bezahlung als Veröffentlichungsoption verwenden, wird Ihr nutzungsbasierter Softwarelizenzierungsumsatz im Verhältnis 80%/20% zwischen Ihnen und Microsoft aufgeteilt. Zur Preisgestaltung für ein einzelnes Angebot können Sie sowohl das Abrechnungsmodell mit nutzungsbasierter Bezahlung als auch das BYOL-Modell verwenden. Beide Abrechnungsmodelle stehen auf Angebotsebene als separate SKUs zur Verfügung. Sie konfigurieren die Abrechnungsmodelle in Ihrem Angebot im Cloud-Partnerportal. 

Betrachten Sie die folgenden Beispiele:
*   Wenn Sie die Option mit nutzungsbasierter Bezahlung aktivieren, sieht das Ergebnis folgendermaßen aus:
    | Ihre Lizenzkosten | 1,00 USD pro Stunde |
    |:--- |:--- |
    | Azure-Nutzungskosten (D1/1-Kern) | 0,14 USD pro Stunde |
    | *Microsoft führt die Abrechnung mit dem Kunden durch* | *1,14 USD pro Stunde* |

    In diesem Szenario berechnet Microsoft 1,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images:
    | Berechnung durch Microsoft | 1,14 USD pro Stunde |
    |:--- |:--- |
    | Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten | 0,80 USD pro Stunde |
    | Microsoft behält 20% Ihrer Lizenzkosten ein | 0,20 USD pro Stunde |
    | Microsoft behält die Kosten der Azure-Nutzung ein | 0,14 USD pro Stunde |

*   Wenn Sie die BYOL-Option aktivieren, sieht das Ergebnis folgendermaßen aus:
    | Ihre Lizenzkosten | Ausgehandelte und von Ihnen in Rechnung gestellte Lizenzgebühr |
    |:--- |:--- |
    | Azure-Nutzungskosten (D1/1-Kern) | 0,14 USD pro Stunde |
    | *Microsoft führt die Abrechnung mit dem Kunden durch* | *0,14 USD pro Stunde* |

    In diesem Szenario berechnet Microsoft 0,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images: 
    | Berechnung durch Microsoft | 0,14 USD pro Stunde |
    |:--- |:--- |
    | Microsoft behält die Kosten der Azure-Nutzung ein | 0,14 USD pro Stunde |
    | Microsoft behält 0% Ihrer Lizenzkosten ein | 0,00 USD pro Stunde |

## <a name="single-billing-and-payment-methods"></a>Einzelabrechnung und Zahlungsmethoden
Ein wesentlicher Vorteil der Veröffentlichungsoption „Transaktion“ im Marketplace besteht darin, dass Ihre Lizenzkosten und Azure-Nutzung Ihrem Kunden direkt als Einzelposten berechnet werden.

In diesem Szenario wickelt Microsoft Abrechnung und Zahlungseingänge in Ihrem Auftrag ab. Bei der Abrechnung durch Microsoft entfällt für Sie die Notwendigkeit, eine eigene Zahlungsvereinbarung mit Ihren Kunden zu treffen. Durch Einzelabrechnungen können Sie Zeit und Ressourcen sparen. Außerdem können Sie sich so vielleicht eher auf das Erzielen von Verkäufen statt auf das Abkassieren konzentrieren. 

## <a name="enterprise-agreement"></a>Enterprise Agreement  
Wenn Sie ein Microsoft Enterprise Agreement-Kunde sind, können Sie Ihr Enterprise Agreement verwenden, um Microsoft-Produkte zu bezahlen. Ihnen können Produkte in Rechnung gestellt werden, Ihre Azure-Nutzung inbegriffen. Die Verwendung des Enterprise Agreements ist für Organisationen gedacht, die Software und Clouddienste für mindestens drei Jahre lizenzieren möchten. Sie können Zahlungen verteilen statt eine große Zahlung zu leisten. Vorauszahlung. Wenn Sie eine Veröffentlichungsoption mit nutzungsbasierter Bezahlung verwenden, folgt die Abrechnung für Ihre Softwarelizenzkosten dem vierteljährlichen Enterprise Agreement-Abrechnungszyklus für Überschreitungen.  

### <a name="monetary-commitment"></a>Finanzielle Verpflichtung
Wenn Sie ein Enterprise Agreement-Kunde sind, können Sie Azure Ihrer Vereinbarung hinzufügen. Dazu vereinbaren Sie im Rahmen der EA einen verbindlichen Betrag für Azure. Dieser verbindliche Betrag wird im Laufe des Jahres verbraucht. Der Betrag deckt jede Kombination der Nutzung von Azure-Diensten ab.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den [Veröffentlichungsleitfaden für Azure Marketplace und AppSource](./marketplace-publishers-guide.md).
