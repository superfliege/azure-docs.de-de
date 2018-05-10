---
title: Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung | Microsoft Docs
description: Erfahren Sie, wie Sie Ihre Nutzung interpretieren können, um die Anwendung der reservierten Instanz für ein nutzungsbasiertes Abonnement zu verstehen.
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
ms.openlocfilehash: 142e172b3f4ab4b88bb3733f70d5e0fb252854ca
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung

Verstehen Sie die Nutzung der reservierten Instanz, indem Sie die ReservationId von der [Reservierungsseite](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade ) und die Verwendungsdatei aus dem [Azure-Kontoportal](https://account.azure.com) verwenden.


>[!NOTE]
>Dieser Artikel gilt nicht für Enterprise Agreement-Kunden (EA). Wenn Sie ein EA-Kunde sind, lesen Sie [Verstehen der Verwendung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md). In diesem Artikel wird davon ausgegangen, dass die Reservierung auf ein einzelnes Abonnement angewendet wird. Wenn die Reservierung auf mehrere Abonnements angewendet wird, kann sich der Reservierungsvorteil über mehrere CSV-Verwendungsdateien erstrecken. 

Im folgenden Abschnitt wird davon ausgegangen, dass Sie einen virtuellen Windows-Computer des Typs Standard_DS1_v2 in der Region „USA, Osten“ ausführen und Ihre Reservierungsinformationen mit den Angaben in der folgenden Tabelle übereinstimmen:

| Feld | Wert |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Menge |1|
|SKU | Standard_DS1_v2|
|Region | eastus |

## <a name="reservation-application"></a>Reservierungsanwendung

Der Hardwarebestandteil des virtuellen Computers wird abgedeckt, da der bereitgestellte virtuelle Computer den Reservierungsattributen entspricht. Wenn Sie ermitteln möchten, welche Windows-Software nicht von der reservierten Instanz abgedeckt wird, navigieren Sie zu [Windows-Softwarekosten für reservierte Azure-VM-Instanzen.](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv"></a>Kostenaufstellungsabschnitt der CSV-Datei
Dieser Abschnitt Ihrer CSV-Datei zeigt die Gesamtverwendung für Ihre Reservierung. Wenden Sie den Filter für das Unterkategoriefeld „Verbrauchseinheit“ an, das die Angabe „Reservierung“ enthält. Ihre Daten sollten wie im folgenden Screenshot aussehen: ![Direkte Kostenaufstellung Reservierung](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

Die Zeile „Reservierte Basis-VM-Instanzen“ enthält die Gesamtzahl der Stunden, die durch die Reservierung abgedeckt werden. Diese Zeile weist den Wert 0,00 USD aus, weil diese Zeit durch die reservierte Instanz abgedeckt wird. Die Zeile „Reservierte Windows Server-Instanzen (1 Kern)“ deckt die Kosten der Windows-Software ab.

### <a name="daily-usage-section-of-csv"></a>Abschnitt zur täglichen Nutzung der CSV-Datei
Filtern Sie nach weiteren Informationen, und geben Sie in Ihre Reservierungs-ID ein. Der folgende Screenshot zeigt die Felder, die im Zusammenhang mit der Reservierung stehen. 

![Tägliche Nutzungsgebühren](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. Die „ReservationId“ im Feld „Zusätzliche Informationen“ ist die Reservierung, die verwendet wurde, um den Vorteil auf den virtuellen Computer anzuwenden.
2. „ConsumptionMeter“ ist die Verbrauchseinheit-ID für den virtuellen Computer.
3. Die Unterkategoriezeile „ Verbrauchseinheit reservierte Basis-VM-Instanzen“ stellt die Zeile mit den Kosten von 0,00 USD im Abschnitt „Kostenaufstellung“ dar. Die Kosten für die Ausführung dieses virtuellen Computers werden bereits durch die Reservierung bezahlt.
4. Dies ist die Verbrauchseinheit-ID für die Reservierung. Die Kosten für diese Verbrauchseinheit betragen 0,00 USD. Jeder virtuelle Computer, der sich für die reservierte Instanz qualifiziert, weist diese „MeterId“ in der CSV-Datei auf, um die Kosten zu berücksichtigen. 
5. Standard_DS1_v2 ist eine vCPU-VM, und der virtuelle Computer wird ohne Azure-Hybridvorteil bereitgestellt. Aus diesem Grund deckt diese Verbrauchseinheit die zusätzlichen Kosten der Windows-Software ab. Weitere Informationen finden Sie unter [Windows software costs not included with Reserved Instances](billing-reserved-instance-windows-software-costs.md) (Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten), wenn Sie die Verbrauchseinheit ermitteln möchten, die dem virtuellen Computer der D-Serie mit einem Kern entspricht. Wenn der Azure-Hybridvorteil verwendet wird, werden diese zusätzlichen Kosten nicht angewendet. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu reservierten VM-Instanzen finden Sie in den folgenden Artikeln.

- [Vorauszahlen für virtuelle Computer mit reservierten VM-Instanzen](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Azure-VM-Instanzen](billing-manage-reserved-vm-instance.md)
- [Mit reservierten VM-Instanzen Kosten für virtuelle Computer sparen](billing-save-compute-costs-reservations.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.