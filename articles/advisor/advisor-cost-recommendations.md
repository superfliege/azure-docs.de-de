---
title: Kostenempfehlungen von Azure Advisor | Microsoft Docs
description: Nutzen Sie Azure Advisor, um die Kosten Ihrer Azure-Bereitstellungen zu optimieren.
services: advisor
documentationcenter: NA
author: kasparks
manager: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: a4de50e3dc17d9c9543bf1ec8cabb04c5d60070b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52832886"
---
# <a name="advisor-cost-recommendations"></a>Advisor-Empfehlungen zu Kosten

Advisor hilft Ihnen beim Optimieren und Senken Ihrer Gesamtausgaben für Azure, indem Ressourcen im Leerlauf bzw. zu gering ausgelastete Ressourcen ermittelt werden. Kostenempfehlungen finden Sie auf dem Advisor-Dashboard auf der Registerkarte **Kosten**.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimieren der Kosten für virtuelle Computer durch Ändern der Größe oder Herunterfahren von zu gering ausgelasteten Instanzen 
Obwohl bestimmte Anwendungsszenarien zielgerichtet eine geringe Auslastung bewirken, können Sie häufig Kosten sparen, indem Sie Größe und Anzahl Ihrer virtuellen Computer steuern. Der Advisor überwacht die Verwendung Ihrer virtuellen Computer 14 Tage lang und ermittelt virtuelle Computer mit geringer Auslastung. Virtuelle Computer, bei denen an mindestens vier Tagen die CPU-Auslastung unter 5 Prozent und die Netzwerklast unter 7 MB lag, gelten als virtuelle Computer mit geringer Auslastung.

Der Advisor gibt die geschätzten Kosten für die weitere Ausführung des virtuellen Computers an, sodass Sie entscheiden können, ob Sie diesen herunterfahren oder seine Größe ändern.

Wenn Sie konsequenter zu gering ausgelastete virtuelle Computer ermitteln möchten, können Sie die Regel für durchschnittliche CPU-Auslastung auf Abonnementbasis anpassen.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Kostensenkung durch die Beseitigung nicht bereitgestellter ExpressRoute-Verbindungen
Der Advisor identifiziert ExpressRoute-Verbindungen, die sich für mehr als einen Monat im Anbieterstatus *Nicht bereitgestellt* befunden haben, und empfiehlt, die Verbindung zu löschen, wenn Sie nicht beabsichtigen, die Verbindung durch Ihren Dienstanbieter bereitzustellen.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Verringern der Kosten durch Löschen oder Neukonfigurieren von Gateways für virtuelle Netzwerke im Leerlauf
Der Advisor identifiziert virtuelle Netzwerkgates, die mehr als 90 Tage lang im Leerlauf waren. Da diese Gateways pro Stunde in Rechnung gestellt werden, sollten Sie sie neu konfigurieren oder löschen, wenn Sie nicht mehr beabsichtigen, diese zu verwenden. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Erwerben reservierter VM-Instanzen, um gegenüber dem nutzungsbasierten Modell Kosten einzusparen
Der Advisor überprüft Ihre VM-Nutzung der letzten 30 Tage und ermittelt, ob der Erwerb einer Azure-Reservierung für Sie günstiger wäre. Der Advisor zeigt die Regionen und Größen mit dem größten Einsparpotenzial sowie die voraussichtlichen Einsparungen an, die sich durch den Erwerb von Reservierung erzielen lassen. 

Mit Azure-Reservierungen können Sie die Grundkosten für Ihre virtuellen Computer vorab begleichen. Rabatte gelten automatisch für neue oder vorhandene virtuelle Computer mit der gleichen Größe und Region wie Ihre Reservierungen. Weitere Informationen zu Azure Reserved Virtual Machine Instances finden Sie [hier](https://azure.microsoft.com/pricing/reserved-vm-instances/).

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Zugreifen auf Kostenempfehlungen im Azure Advisor

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klicken Sie auf dem Advisor-Dashboard auf die Registerkarte **Kosten**.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Advisor:
* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte](advisor-get-started.md)
* [Advisor-Empfehlungen zur Leistung](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen für Hochverfügbarkeit](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-cost-recommendations.md)
