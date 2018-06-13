---
title: Grundlegendes zur Nutzung reservierter Azure-Instanzen für Enterprise – Azure-Abrechnung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie die reservierte Azure-VM-Instanz für den Konzernbeitritt angewendet wird.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: a92fce33b194c5cb7b763930e7fd11135f9fbd4f
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34301313"
---
# <a name="understand-azure-reserved-instance-usage-for-your-enterprise-enrollment"></a>Grundlegendes zur Nutzung reservierter Azure-Instanzen für den Konzernbeitritt
Verschaffen Sie sich einen Überblick über die Nutzung der reservierten Instanz, indem Sie die **Reservierungs-ID** von der [Reservierungsseite](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) und die Verwendungsdatei aus dem [EA-Portal](https://ea.azure.com) verwenden. Darüber hinaus können Sie die Nutzung reservierter Instanzen im [EA-Portal](https://ea.azure.com) im Abschnitt mit der Nutzungsübersicht anzeigen.

>[!NOTE]
>Wenn Sie die reservierte Instanz in einem Abrechnungskontext mit nutzungsbasierter Bezahlung erworben haben, lesen Sie die Informationen unter [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md).

Im folgenden Abschnitt wird davon ausgegangen, dass Sie eine Windows-VM des Typs „Standard_D1_v2“ in der Region „USA, Osten“ ausführen und die Informationen Ihrer reservierten Instanz mit den Angaben in der folgenden Tabelle übereinstimmen:

| Feld | Wert |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Menge |1|
|SKU | Standard_D1|
|Region | eastus |

## <a name="reserved-instance-application"></a>Anwendung der reservierten Instanz

Der Hardwarebestandteil der VM ist abgedeckt, da die bereitgestellte VM den Attributen reservierter Instanzen entspricht. Wenn Sie ermitteln möchten, welche Windows-Software nicht von der reservierten Instanz abgedeckt wird, navigieren Sie zu [Nicht in Reserved Instances enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md).


### <a name="reserved-instance-usage-in-csv"></a>Nutzung reservierter Instanzen als CSV-Datei
Sie können die CSV-Datei zur EA-Nutzung über das EA-Portal herunterladen. Filtern Sie in der heruntergeladenen CSV-Datei nach weiteren Informationen, und geben Sie Ihre **Reservierungs-ID** ein. Der folgende Screenshot zeigt die Felder, die im Zusammenhang mit der reservierten Instanz stehen:

![Enterprise Agreement (EA) als CSV-Datei für reservierte Azure-Instanzen](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **ReservationId** im Feld „Zusätzliche Informationen“ stellt die reservierte Instanz dar, die verwendet wurde, um den Vorteil auf die VM anzuwenden.
2. „ConsumptionMeter“ ist die Verbrauchseinheit-ID für den virtuellen Computer.
3. Für die Reservierungsverbrauchseinheit werden als Kosten „$0“ angezeigt, da die Kosten für die ausgeführte VM bereits durch die reservierte Instanz bezahlt sind. 
4. „Standard_D1“ ist ein virtueller Computer mit einer vCPU, und der virtuelle Computer wird ohne Azure-Hybridvorteil bereitgestellt. Aus diesem Grund deckt diese Verbrauchseinheit die zusätzlichen Kosten der Windows-Software ab. Weitere Informationen finden Sie unter [Windows software costs not included with Reserved Instances](billing-reserved-instance-windows-software-costs.md) (Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten), wenn Sie die Verbrauchseinheit ermitteln möchten, die dem virtuellen Computer der D-Serie mit einem Kern entspricht. Wenn der Azure-Hybridvorteil verwendet wird, werden diese zusätzlichen Kosten nicht angewendet.

### <a name="reserved-instance-usage-in-usage-summary-page-in-ea-portal"></a>Nutzung reservierter Instanzen auf der Seite mit der Nutzungsübersicht im EA-Portal

Die Nutzung reservierter Instanzen wird darüber hinaus im Abschnitt zur Nutzungsübersicht des EA-Portals angezeigt: ![EA-Nutzungsübersicht (Enterprise Agreement)](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Hardwarekomponenten des virtuellen Computers werden nicht in Rechnung gestellt, da sie von der reservierten Instanz abgedeckt sind. 
2. Windows-Software wird in Rechnung gestellt, da der Azure-Hybridvorteil nicht verwendet wird. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu reservierten Azure-Instanzen finden Sie in den folgenden Artikeln:

- [Einsparung von Kosten für virtuelle Computer mit reservierten Azure-Instanzen](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit reservierten VM-Instanzen](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Instanzen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](billing-understand-reserved-instance-usage.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.