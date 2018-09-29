---
title: Grundlegendes zum Rabatt für reservierte Azure-VM-Instanzen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie der Rabatt für reservierte Azure-VM-Instanzen auf ausgeführte virtuelle Computer angewendet wird.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: cwatson
ms.openlocfilehash: 460cd3a5b05680563661e2f6519eaa3506eee03b
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392079"
---
# <a name="understand-how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Grundlegendes zur Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer

Nachdem Sie eine reservierte Azure-VM-Instanz erworben haben, wird der Reservierungsrabatt automatisch auf virtuelle Computer angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt die Computekosten Ihrer virtuellen Computer ab.

Informationen zu reservierten SQL-Datenbank-Kapazitäten finden Sie unter [Grundlegendes zur Anwendung des Rabatts für Azure-Reservierungen auf SQL-Datenbank-Instanzen](billing-understand-reservation-charges.md).

In der folgenden Tabelle sind die Kosten für virtuelle Computer nach dem Kauf einer reservierten VM-Instanz veranschaulicht. In jedem Fall werden Ihnen die regulären Gebühren für Speicher und Netzwerke berechnet.

| Typ des virtuellen Computers  | Gebühren bei einer reservierten VM-Instanz |
|-----------------------|--------------------------------------------|
|Virtuelle Linux-Computer ohne zusätzliche Software | Die Reservierung deckt die Kosten der VM-Infrastruktur ab.|
|Virtuelle Linux-Computer mit Softwaregebühren (z.B. Red Hat) | Die Reservierung deckt die Kosten der Infrastruktur ab. Zusätzliche Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer ohne zusätzliche Software |Die Reservierung deckt die Kosten der Infrastruktur ab. Windows-Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer mit zusätzlicher Software (z.B. SQL Server) | Die Reservierung deckt die Kosten der Infrastruktur ab. Windows-Software und zusätzliche Software werden in Rechnung gestellt.|
|Virtuelle Windows-Computer mit [Azure-Hybridvorteil](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Die Reservierung deckt die Kosten der Infrastruktur ab. Die Kosten für die Windows-Software werden durch den Azure-Hybridvorteil abgedeckt. Zusätzliche Software wird separat in Rechnung gestellt.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Anwendung des Reservierungsrabatts auf virtuelle Nicht-Windows-Computer

 Der Rabatt für Azure-Reservierungen wird auf Stundenbasis auf ausgeführte VM-Instanzen angewendet. Die erworbenen Reservierungen werden zur Anwendung des Reservierungsrabatts auf die von den ausgeführten virtuellen Computern ausgegebene Nutzung abgestimmt. Bei virtuellen Computern, die keine volle Stunde lang ausgeführt werden, wird die Reservierung von anderen virtuellen Computern aufgefüllt, die keine Reservierung verwenden. Dazu gehören auch gleichzeitig ausgeführte virtuelle Computer. Am Ende der Stunde wird die Anwendung der Reservierung für VMs für diese Stunde gesperrt. Wenn eine VM nicht eine ganze Stunde lang ausgeführt wird oder gleichzeitige VMs diese Reservierungsstunde nicht auffüllen, ist die Reservierung für diese Stunde nicht vollständig ausgelastet. Das folgende Diagramm veranschaulicht die Anwendung einer Reservierung auf die abrechenbare VM-Nutzung. Die Abbildung basiert auf einem Reservierungskauf und zwei entsprechenden VM-Instanzen.

![Screenshot einer angewendeten Reservierung und zweier entsprechender VM-Instanzen](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Jegliche Nutzung, die über der Grenze für die Reservierung liegt, wird zu den regulären Preisen der nutzungsbasierten Bezahlung in Rechnung gestellt. Die Nutzung unterhalb der Grenze für Reservierungen wird nicht in Rechnung gestellt, da dafür bereits im Rahmen eines Reservierungskaufs bezahlt wurde.
2. In der ersten Stunde wird Instanz 1 für 0,75 Stunden und Instanz 2 für 0,5 Stunden ausgeführt. Die Gesamtnutzung für die erste Stunde beträgt 1,25 Stunden. Ihnen wird der Preis für die nutzungsbasierte Bezahlung für die verbleibenden 0,25 Stunden in Rechnung gestellt.
3. In der zweiten und dritten Stunde wurden beide Instanzen jeweils eine Stunde lang ausgeführt. Eine Instanz wird durch die Reservierung abgedeckt, die andere wird zu den Preisen für die nutzungsbasierte Zahlung in Rechnung gestellt.
4. In der vierten Stunde wird Instanz 1 für 0,5 Stunden und Instanz 2 für 1 Stunde ausgeführt. Instanz 1 wird vollständig durch die Reservierung abgedeckt. Außerdem sind 0,5 Stunden von Instanz 2 abgedeckt. Ihnen wird der Preis für die nutzungsbasierte Zahlung für die verbleibenden 0,5 Stunden in Rechnung gestellt.

Wenn Sie grundlegende Informationen wünschen und die Anwendung Ihrer Azure-Reservierungen in Abrechnungsnutzungsberichten anzeigen möchten, lesen Sie [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Anwendung des Reservierungsrabatts auf virtuelle Windows-Computer

Wenn Sie Windows-VM-Instanzen ausführen, wird die Reservierung angewendet, um die Infrastrukturkosten abzudecken. Die Anwendung der Reservierung auf die VM-Infrastrukturkosten ist bei virtuellen Windows-Computer und virtuellen Nicht-Windows-Computern identisch. Windows-Software wird separat basierend auf vCPU-Basis in Rechnung gestellt. Informationen finden Sie unter [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](https://go.microsoft.com/fwlink/?linkid=862756). Sie können Ihre Windows-Lizenzierungskosten mit dem [Azure-Hybridvorteil für Windows Server] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) decken.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md)
- [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
- [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)
- [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
