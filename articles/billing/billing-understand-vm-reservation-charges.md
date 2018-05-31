---
title: Grundlegendes zur Anwendung des Rabatts für Azure Reserved Virtual Machine Instances | Microsoft-Dokumentation
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
ms.date: 11/10/2017
ms.author: yashar
ms.openlocfilehash: ddf6e6a869fb2961db6b626878f9433fdc00a55f
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34056651"
---
# <a name="understand-how-the-reserved-virtual-machine-instance-discount-is-applied"></a>Grundlegendes zur Anwendung des Rabatts für reservierte VM-Instanzen
Nachdem Sie eine reservierte VM-Instanz erworben haben, wird der Reservierungsrabatt automatisch auf die virtuellen Computer angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt die Infrastrukturkosten Ihrer virtuellen Computer ab. In der folgenden Tabelle sind die Kosten für virtuelle Computer nach dem Kauf einer Reservierung veranschaulicht. In jedem Fall werden Ihnen die regulären Gebühren für Speicher und Netzwerke berechnet.

| Typ des virtuellen Computers  | Gebühren mit Reservierung |    
|-----------------------|--------------------------------------------|
|Virtuelle Linux-Computer ohne zusätzliche Software | Die Reservierung deckt die Kosten der VM-Infrastruktur ab.|
|Virtuelle Linux-Computer mit Softwaregebühren (z.B. Red Hat) | Die Reservierung deckt die Kosten der Infrastruktur ab. Zusätzliche Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer ohne zusätzliche Software |Die Reservierung deckt die Kosten der Infrastruktur ab. Windows-Software wird in Rechnung gestellt.|
|Virtuelle Windows-Computer mit zusätzlicher Software (z.B. SQL Server) | Die Reservierung deckt die Kosten der Infrastruktur ab. Windows-Software und zusätzliche Software werden in Rechnung gestellt.|
|Virtuelle Windows-Computer mit [Azure-Hybridvorteil](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Die Reservierung deckt die Kosten der Infrastruktur ab. Die Kosten für die Windows-Software werden durch den Azure-Hybridvorteil abgedeckt. Zusätzliche Software wird separat in Rechnung gestellt.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Anwendung des Reservierungsrabatts auf virtuelle Nicht-Windows-Computer
 Der Reservierungsrabatt wird auf Stundenbasis auf ausgeführte VM-Instanzen angewendet. Die erworbenen Reservierungen werden zur Anwendung des Reservierungsrabatts auf die von den ausgeführten virtuellen Computern ausgegebene Nutzung abgestimmt. Bei virtuellen Computern, die keine volle Stunde lang ausgeführt werden, wird die Reservierung von anderen virtuellen Computern aufgefüllt, die keine Reservierung verwenden. Dazu gehören auch gleichzeitig ausgeführte virtuelle Computer. Am Ende der Stunde wird die Anwendung der Reservierung für virtuelle Computer für diese Stunde gesperrt. Wenn ein virtueller Computer nicht eine ganze Stunde lang ausgeführt wird oder gleichzeitige virtuelle Computer diese Reservierungsstunde nicht auffüllen, ist die Reservierung für diese Stunde nicht vollständig ausgelastet. Das folgende Diagramm veranschaulicht die Anwendung einer Reservierung auf die abrechenbare VM-Nutzung. Die Abbildung basiert auf einem Reservierungskauf und zwei entsprechenden VM-Instanzen.

![Anwendung der reservierten VM-Instanz](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Jegliche Nutzung, die über der Grenze für reservierte VM-Instanzen liegt, wird zu den regulären Preisen für nutzungsbasierte Zahlung in Rechnung gestellt. Die Nutzung unterhalb der Grenze für reservierte VM-Instanzen werden nicht in Rechnung gestellt, da dafür bereits im Rahmen eines Reservierungskaufs bezahlt wurde.
2.  In der ersten Stunde wird Instanz 1 für 0,75 Stunden und Instanz 2 für 0,5 Stunden ausgeführt. Die Gesamtnutzung für die erste Stunde beträgt 1,25 Stunden. Ihnen wird der Preis für die nutzungsbasierte Zahlung für die verbleibenden 0,25 Stunden in Rechnung gestellt.
3.  In der zweiten und dritten Stunde wurden beide Instanzen jeweils eine Stunde lang ausgeführt. Eine Instanz wird durch die Reservierung abgedeckt, die andere wird zu den Preisen für die nutzungsbasierte Zahlung in Rechnung gestellt.
4.  In der vierten Stunde wird Instanz 1 für 0,5 Stunden und Instanz 2 für 1 Stunde ausgeführt. Instanz 1 wird vollständig durch die Reservierung abgedeckt. Außerdem sind 0,5 Stunden von Instanz 2 abgedeckt. Ihnen wird der Preis für die nutzungsbasierte Zahlung für die verbleibenden 0,5 Stunden in Rechnung gestellt.

Wenn Sie grundlegende Informationen wünschen und die Anwendung Ihrer Reservierungen in Abrechnungsnutzungsberichten anzeigen möchten, lesen Sie [Understand Reserved Instance usage for your Enterprise enrollment](https://go.microsoft.com/fwlink/?linkid=862757) (Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Anwendung des Reservierungsrabatts auf virtuelle Windows-Computer
Wenn Sie Windows-VM-Instanzen ausführen, wird die Reservierung angewendet, um die Infrastrukturkosten abzudecken. Die Anwendung der Reservierung auf die VM-Infrastrukturkosten ist bei virtuellen Windows-Computer und virtuellen Nicht-Windows-Computern identisch. Windows-Software wird separat basierend auf den vCPUs in Rechnung gestellt. Informationen finden Sie unter [Windows software costs not included with Reserved Instances](https://go.microsoft.com/fwlink/?linkid=862756) (Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten). Sie können Ihre Windows-Lizenzierungskosten mit dem [Azure-Hybridvorteil für Windows Server] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) decken.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu reservierten VM-Instanzen finden Sie in den folgenden Artikeln.

- [Vorauszahlen für virtuelle Computer mit reservierten VM-Instanzen](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von reservierten Azure-VM-Instanzen](billing-manage-reserved-vm-instance.md)
- [Mit reservierten VM-Instanzen Kosten für virtuelle Computer sparen](billing-save-compute-costs-reservations.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)


## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
