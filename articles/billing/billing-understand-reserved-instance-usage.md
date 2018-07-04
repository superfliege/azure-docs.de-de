---
title: Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung | Microsoft Docs
description: In diesem Artikel erfahren Sie, wie die reservierte Azure-VM-Instanz für Ihr Abonnement mit nutzungsbasierter Bezahlung angewendet wird.
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
ms.openlocfilehash: 7e303f3e5ce0e618d941be4190f6fadb40f2e09d
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064328"
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung

Verschaffen Sie sich einen Überblick über die Verwendung einer reservierten Azure-VM-Instanz, indem Sie die ReservationId von der [Reservierungsseite](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) und die Verwendungsdatei aus dem [Azure-Kontoportal](https://account.azure.com) verwenden.


>[!NOTE]
>Dieser Artikel gilt nicht für Enterprise Agreement-Kunden (EA). Wenn Sie ein EA-Kunde sind, lesen Sie [Verstehen der Verwendung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md). In diesem Artikel wird davon ausgegangen, dass die reservierte Instanz auf ein einzelnes Abonnement angewendet wird. Wenn die reservierte Instanz auf mehrere Abonnements angewendet wird, kann sich der Vorteil für reservierte Instanzen über mehrere CSV-Verwendungsdateien erstrecken. 

Im folgenden Abschnitt wird davon ausgegangen, dass Sie einen virtuellen Windows-Computer des Typs „Standard_DS1_v2“ in der Region „USA, Osten“ ausführen und die Informationen Ihrer reservierten Instanz mit den Angaben in der folgenden Tabelle übereinstimmen:

| Feld | Wert |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Menge |1|
|SKU | Standard_DS1_v2|
|Region | eastus |

## <a name="reserved-instance-application"></a>Anwendung der reservierten Instanz

Der Hardwarebestandteil des virtuellen Computers ist abgedeckt, weil der bereitgestellte VM den Attributen der reservierten Instanz entspricht. Wenn Sie ermitteln möchten, welche Windows-Software nicht von der reservierten Instanz abgedeckt wird, navigieren Sie zu [Windows-Softwarekosten für reservierte Azure-VM-Instanzen.](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv"></a>Kostenaufstellungsabschnitt der CSV-Datei
Dieser Abschnitt Ihrer CSV-Datei zeigt die Gesamtnutzung für Ihre reservierte Instanz. Wenden Sie den Filter für das Unterkategoriefeld „Verbrauchseinheit“ an, das die Angabe „Reservierte“ enthält. Ihre Daten sollten wie im folgenden Screenshot aussehen: ![Screenshot der gefilterten Details zur Nutzung reservierter Instanzen und Kosten](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

Die Zeile „Reservierte Basis-VM-Instanzen“ enthält die Gesamtanzahl der Stunden, die durch die reservierte Instanz abgedeckt werden. Diese Zeile enthält den Wert 0,00 USD, weil diese Zeit durch die reservierte Instanz abgedeckt wird. Die Zeile „Reservierte Windows Server-Instanzen (1 Kern)“ deckt die Kosten der Windows-Software ab.

### <a name="daily-usage-section-of-csv"></a>Abschnitt zur täglichen Nutzung der CSV-Datei
Filtern Sie nach weiteren Informationen, und geben Sie Ihre **Reservierungs-ID** ein. Im folgenden Screenshot sehen Sie die Felder, die im Zusammenhang mit der reservierten Instanz stehen. 

![Screenshot der Details zu täglicher Nutzung und Kosten](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **ReservationId** im Feld „Zusätzliche Informationen“ stellt die reservierte Instanz dar, die verwendet wurde, um den Vorteil auf den VM anzuwenden.
2. „ConsumptionMeter“ ist die Verbrauchseinheit-ID für den virtuellen Computer.
3. Die Zeile „Unterkategorie der Verbrauchseinheit“ mit dem Wert „Reservierte Basis-VM-Instanzen“ stellt die Zeile mit den Kosten von 0,00 USD im Abschnitt „Kostenaufstellung“ dar. Die Kosten für die Ausführung dieses virtuellen Computers werden bereits durch die reservierte Instanz bezahlt.
4. Dies ist die Verbrauchseinheit-ID für die reservierte Instanz. Die Kosten für diese Verbrauchseinheit betragen 0,00 USD. Jeder virtuelle Computer, der sich für die reservierte Instanz qualifiziert, weist diese Verbrauchseinheit-ID in der CSV-Datei auf, um die Kosten zu berücksichtigen. 
5. Standard_DS1_v2 ist eine vCPU-VM, und der virtuelle Computer wird ohne Azure-Hybridvorteil bereitgestellt. Aus diesem Grund deckt diese Verbrauchseinheit die zusätzlichen Kosten der Windows-Software ab. Weitere Informationen finden Sie unter [Windows software costs not included with Reserved Instances](billing-reserved-instance-windows-software-costs.md) (Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten), wenn Sie die Verbrauchseinheit ermitteln möchten, die dem virtuellen Computer der D-Serie mit einem Kern entspricht. Wenn der Azure-Hybridvorteil verwendet wird, werden diese zusätzlichen Kosten nicht angewendet. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu reservierten Instanzen finden Sie in den folgenden Artikeln:

- [Was ist Azure Reserved VM Instances?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Instanzen in Azure](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
