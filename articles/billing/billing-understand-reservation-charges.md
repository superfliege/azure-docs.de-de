---
title: Grundlegendes zum Rabatt für Azure-Reservierungen | Microsoft-Dokumentation
description: Erfahren Sie, wie ein Reservierungsrabatt auf ausgeführte SQL-Datenbank-Instanzen angewendet wird.
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
ms.author: yashar
ms.openlocfilehash: 3a5339200309a50eace5b48dfbd9127b7eb1dae6
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39632333"
---
# <a name="understand-how-an-azure-reservation-discount-is-applied-to-sql-databases"></a>Grundlegendes zur Anwendung eines Rabatts für Azure-Reservierungen auf SQL-Datenbank-Instanzen

Nachdem Sie eine reservierte Azure SQL-Datenbank-Kapazität erworben haben, wird der Reservierungsrabatt automatisch auf SQL-Datenbank-Instanzen angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt die Computekosten Ihrer SQL-Datenbank-Instanz ab. Ihnen werden die regulären Gebühren für Software, Speicher und Netzwerke berechnet. Die Lizenzkosten für SQL-Datenbank-Instanzen können Sie mit dem [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) abdecken.

Weitere Informationen zu reservierten VM-Instanzen finden Sie unter [Grundlegendes zum Rabatt für Azure-Reservierungen auf virtuelle Computer](billing-understand-vm-reservation-charges.md).

## <a name="reservation-discount-applied-to-sql-databases"></a>Anwendung des Reservierungsrabatts auf SQL-Datenbank-Instanzen

 Der Rabatt für reservierte SQL-Datenbank-Kapazitäten wird auf ausgeführte SQL-Datenbank-Instanzen, die auf Stundenbasis abgerechnet werden, angewendet. Die Reservierung, die Sie kaufen, wird der Computenutzung zugeordnet, die von den ausgeführten SQL-Datenbank-Instanzen ausgegeben wird. Bei SQL-Datenbank-Instanzen, die nicht über die gesamte Stunde hinweg ausgeführt werden, wird die Reservierung automatisch auf andere SQL-Datenbank-Instanzen angewendet, die den Reservierungsattributen entsprechen. Der Rabatt kann auf SQL-Datenbank-Instanzen angewendet werden, die gleichzeitig ausgeführt werden. Wenn die den Reservierungsattributen entsprechenden SQL-Datenbank-Instanzen nicht über die gesamte Stunde hinweg ausgeführt werden, profitieren Sie nicht im vollen Umfang vom Reservierungsrabatt für diese Stunde.

Die folgenden Beispiele zeigen, wie der Rabatt für reservierte SQL-Datenbank-Kapazitäten in Abhängigkeit von der Anzahl der Kerne, die Sie erworben haben, und der Dauer ihrer Ausführung angewendet wird.

- Szenario 1: Sie erwerben eine reservierte SQL-Datenbank-Kapazität für eine SQL-Datenbank-Instanz mit 8 Kernen. Sie führen eine SQL-Datenbank-Instanz mit 16 Kernen aus, die den restlichen Attributen der Reservierung entspricht. Ihnen wird der Preis für die nutzungsbasierte Bezahlung für 8 Kerne der SQL-Datenbank-Computenutzung in Rechnung gestellt. Sie erhalten den Reservierungsrabatt für eine Stunde der SQL-Datenbank-Computenutzung (8 Kerne).

Bei den übrigen Beispielen wird davon ausgegangen, dass die reservierte SQL-Datenbank-Kapazität, die Sie erwerben, für eine SQL-Datenbank-Instanz mit 16 Kernen gilt und die restlichen Reservierungsattribute den ausgeführten SQL-Datenbank-Instanzen entsprechen.

- Szenario 2: Sie führen zwei SQL-Datenbank-Instanzen mit 8 Kernen jeweils eine Stunde lang aus. Der Reservierungsrabatt für 16 Kerne wird auf die Computenutzung für beide SQL-Datenbank-Instanzen mit 8 Kernen angewendet.
- Szenario 3: Sie führen von 13 bis 13:30 Uhr eine SQL-Datenbank-Instanz mit 16 Kernen aus. Sie führen von 13:30 bis 14 Uhr eine weitere SQL-Datenbank-Instanz mit 16 Kernen aus. Beide Instanzen sind durch den Reservierungsrabatt abgedeckt.
- Szenario 3: Sie führen von 13 bis 13:45 Uhr eine SQL-Datenbank-Instanz mit 16 Kernen aus. Sie führen von 13:30 bis 14 Uhr eine weitere SQL-Datenbank-Instanz mit 16 Kernen aus. Die zusätzlichen 15 Minuten werden Ihnen zu den Preisen der nutzungsbasierten Bezahlung in Rechnung gestellt. Für den restlichen Zeitraum wird der Reservierungsrabatt auf die Computenutzung angewendet.

Wenn Sie grundlegende Informationen wünschen und die Anwendung Ihrer Azure-Reservierungen in Abrechnungsnutzungsberichten anzeigen möchten, lesen Sie [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md)
- [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
- [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)


## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
