---
title: Grundlegendes zur Nutzung reservierter Azure Instanzen für Enterprise | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Ihre Nutzung interpretieren können, um die Anwendung der reservierten Instanz für die Enterprise-Registrierung zu verstehen.
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
ms.date: 11/03/2017
ms.author: manshuk
ms.openlocfilehash: cf79926e6497c50156f2a0191997ca06bc605c16
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
---
# <a name="understand--reserved-instance-usage-for-your-enterprise-enrollment"></a>Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung
Verstehen Sie die Nutzung der reservierten Instanz, indem Sie die Reservierungs-ID von der [Reservierungsseite](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade ) und die Verwendungsdatei aus dem [EA-Portal](https://ea.azure.com) verwenden. Darüber hinaus können Sie die Reservierungsnutzung im [EA-Portal](https://ea.azure.com) im Abschnitt mit der Nutzungsübersicht anzeigen.

>[!NOTE]
>Wenn Sie die Reservierung in einem Abrechnungskontext mit nutzungsbasierter Bezahlung erworben haben, lesen Sie die Informationen unter [Understand Reserved Instance usage for your Pay-As-You-Go subscription](billing-understand-reserved-instance-usage.md) (Grundlegendes zur Nutzung reservierter Instanzen für Ihr Abonnement mit nutzungsbasierter Bezahlung).

Im folgenden Abschnitt wird davon ausgegangen, dass Sie einen virtuellen Windows-Computer des Typs „Standard_D1_v2“ in der Region „USA, Osten“ ausführen und Ihre Reservierungsinformationen mit den Angaben in der folgenden Tabelle übereinstimmen:

| Feld | Wert |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Menge |1|
|SKU | Standard_D1|
|Region | eastus |

## <a name="reservation-application"></a>Reservierungsanwendung

Der Hardwarebestandteil des virtuellen Computers ist abgedeckt, da der bereitgestellte virtuelle Computer den Reservierungsattributen entspricht. Wenn Sie ermitteln möchten, welche Windows-Software nicht von der reservierten Instanz abgedeckt wird, navigieren Sie zu [Nicht in Reserved Instances enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md).


### <a name="reservation-usage-in-csv"></a>Reservierungsnutzung in CSV
Sie können die CSV-Datei zur EA-Nutzung über das EA-Portal herunterladen. Filtern Sie in der heruntergeladenen CSV-Datei nach weiteren Informationen, und geben Sie in Ihre Reservierungs-ID ein. Der folgende Screenshot zeigt die Felder, die im Zusammenhang mit der Reservierung stehen.

![EA-CSV-Datei für reservierte Instanzen](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. „ReservationId“ im Feld „Zusätzliche Informationen“ stellt die Reservierung dar, die verwendet wurde, um den Vorteil auf den virtuellen Computer anzuwenden.
2. „ConsumptionMeter“ ist die Verbrauchseinheit-ID für den virtuellen Computer.
3. Für „ReservationMeter“ werden als Kosten „$0“ angezeigt, da die Kosten für den ausgeführten virtuellen Computer bereits durch die Reservierung bezahlt sind. 
4. „Standard_D1“ ist ein virtueller Computer mit einer vCPU, und der virtuelle Computer wird ohne Azure-Hybridvorteil bereitgestellt. Aus diesem Grund deckt diese Verbrauchseinheit die zusätzlichen Kosten der Windows-Software ab. Weitere Informationen finden Sie unter [Windows software costs not included with Reserved Instances](billing-reserved-instance-windows-software-costs.md) (Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten), wenn Sie die Verbrauchseinheit ermitteln möchten, die dem virtuellen Computer der D-Serie mit einem Kern entspricht. Wenn der Azure-Hybridvorteil verwendet wird, werden diese zusätzlichen Kosten nicht angewendet.

### <a name="reservation-usage-in-usage-summary-page-in-ea-portal"></a>Reservierungsnutzung auf der Seite mit der Nutzungsübersicht im EA-Portal

Die Nutzung reservierter Instanzen wird darüber hinaus im Abschnitt zur Nutzungsübersicht des EA-Portals angezeigt: ![EA-Nutzungsübersicht](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Hardwarekomponenten des virtuellen Computers werden nicht in Rechnung gestellt, da sie von der reservierten Instanz abgedeckt sind. 
2. Windows-Software wird in Rechnung gestellt, da der Azure-Hybridvorteil nicht verwendet wird. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu reservierten VM-Instanzen finden Sie in den folgenden Artikeln.

- [Vorauszahlen für virtuelle Computer mit reservierten VM-Instanzen](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Azure-VM-Instanzen](billing-manage-reserved-vm-instance.md)
- [Mit reservierten VM-Instanzen Kosten für virtuelle Computer sparen](billing-save-compute-costs-reservations.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](billing-understand-reserved-instance-usage.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.