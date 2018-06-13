---
title: Behandeln von Problemen während eines Failbacks von Azure zu einem lokalen Standort und anschließendes erneutes Schützen in Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Möglichkeiten zum Beheben allgemeiner Fehler bei Failbacks von Azure zu einem lokalen Standort und während des erneuten Schützens
services: site-recovery
documentationcenter: ''
author: rajani-janaki-ram
manager: gauravd
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/19/2017
ms.author: rajanaki
ms.openlocfilehash: d487c1fcf7fb6444c2b8489df839a6450715ae0a
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
ms.locfileid: "29677682"
---
# <a name="troubleshoot-errors-reported-during-the-process-of-failback"></a>Beheben von Fehlern, die während des Failbackprozesses gemeldet werden
Ein Failback umfasst im Wesentlichen zwei Schritte. Der erste Schritt ist das erneute Schützen von virtuellen Computern am lokalen Standort, der zweite ist der eigentliche Failbackvorgang von Azure zu einem lokalen Standort.

## <a name="troubleshoot-errors-when-reprotecting-a-virtual-machine-back-to-on-premises-after-failover"></a>Beheben von Fehlern beim erneuten Schützen eines virtuellen Computers am lokalen Standort nach dem Failover
Beim erneuten Schützen eines virtuellen Computers in Azure erhalten Sie möglicherweise eine der folgenden Fehlermeldungen. Gehen Sie für die Problembehandlung gemäß den für den jeweiligen Fehlerzustand beschriebenen Schritten vor.


### <a name="error-code-95226"></a>Fehlercode 95226

*Fehler beim erneuten Anwenden des Schutzes, weil der virtuelle Azure-Computer den lokalen Konfigurationsserver nicht erreichen konnte.*

Ursache 
1. Der virtuelle Azure-Computer konnte den lokalen Konfigurationsserver nicht erreichen und daher nicht ermittelt und nicht beim Konfigurationsserver registriert werden. 
2. Der Dienst „InMage Scout Application“, der für die Kommunikation mit dem lokalen Konfigurationsserver auf dem virtuellen Azure-Computer ausgeführt werden muss, wird im Anschluss an ein Failover unter Umständen nicht ausgeführt.

Lösung
1. Stellen Sie sicher, dass das Netzwerk des virtuellen Azure-Computers so konfiguriert ist, dass der virtuelle Computer mit dem lokalen Konfigurationsserver kommunizieren kann. Richten Sie hierzu entweder ein Site-to-Site-VPN mit Ihrem lokalen Datencenter ein, oder konfigurieren Sie eine ExpressRoute-Verbindung mit privatem Peering für das virtuelle Netzwerk des virtuellen Azure-Computers. 
2. Wenn Sie bereits über ein ordnungsgemäß konfiguriertes Netzwerk verfügen, sodass der virtuelle Azure-Computer mit dem lokalen Konfigurationsserver kommunizieren kann, melden Sie sich bei dem virtuellen Computer an, und überprüfen Sie den Dienst „InMage Scout Application“. Sollte der Dienst „InMage Scout Application“ nicht ausgeführt werden, starten Sie den Dienst manuell, und vergewissern Sie sich, dass der Starttyp des Diensts auf „Automatisch“ festgelegt ist.

### <a name="error-code-78052"></a>Fehlercode 78052
Beim erneuten Schützen tritt der folgende Fehler auf: *Der Schutz konnte für den virtuellen Computer nicht abgeschlossen werden.*

Das kann zwei Ursachen haben:
1. Der virtuelle Computer, den Sie erneut schützen möchten, verwendet Windows Server 2016. Dieses Betriebssystem wird erst in Kürze für Failbacks unterstützt.
2. Auf dem Masterzielserver für das Failback ist bereits ein virtueller Computer mit dem gleichen Namen vorhanden.

In diesem Fall können Sie einen anderen Masterzielserver auf einem anderen Host auswählen, sodass der Computer beim erneuten Schützen auf einem anderen Host erstellt und der Namenskonflikt somit vermieden wird. Sie können das Masterziel auch mithilfe von vMotion auf einen anderen Host übertragen, auf dem der Namenskonflikt nicht auftritt. Wenn der vorhandene virtuelle Computer ein vereinzelter Computer ist, können Sie ihn einfach umbenennen, damit der neue virtuelle Computer auf demselben ESXi-Host erstellt werden kann.

### <a name="error-code-78093"></a>Fehlercode 78093

*Die VM wird nicht ausgeführt, reagiert nicht oder ist nicht erreichbar.*

Um einen virtuellen Computer nach einem Failback auf die lokale Umgebung erneut schützen zu können, muss der virtuelle Azure-Computer ausgeführt werden. Dies ist erforderlich, damit sich der Mobility Service beim Konfigurationsserver lokal registriert und mit der Replikation beginnen kann, indem er mit dem Prozessserver kommuniziert. Wenn sich der Computer in einem falschen Netzwerk befindet oder nicht ausgeführt wird (also nicht reagiert oder heruntergefahren ist), kann der Konfigurationsserver den Mobility Service auf dem virtuellen Computer nicht erreichen und den Schutz nicht erneut aktivieren. Sie können den virtuellen Computer neu starten, damit er wieder mit der lokalen Umgebung kommunizieren kann. Starten Sie nach dem Neustart des virtuellen Azure-Computers den Auftrag für erneutes Schützen neu.

### <a name="error-code-8061"></a>Fehlercode 8061

*Der Datenspeicher ist über den ESXi-Host nicht zugänglich.*

Informationen zum Failback finden Sie in den [Voraussetzungen des Masterziels](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) und in den [unterstützen Datenspeichern](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback).


## <a name="troubleshoot-errors-when-performing-a-failback-of-an-azure-virtual-machine-back-to-on-premises"></a>Beheben von Fehlern beim Durchführen eines Failbacks eines virtuellen Azure-Computers zum lokalen Standort
Beim Durchführen des Failbacks eines virtuellen Azure-Computers zum lokalen Standort erhalten Sie möglicherweise eine der folgenden Fehlermeldungen. Gehen Sie für die Problembehandlung gemäß den für den jeweiligen Fehlerzustand beschriebenen Schritten vor.

### <a name="error-code-8038"></a>Fehlercode 8038

*Fehler beim Aktivieren des lokalen virtuellen Computers aufgrund des Fehlers*

Dies passiert, wenn der lokale virtuelle Computer auf einem Host aktiviert wird, auf dem nicht genügend Arbeitsspeicher verfügbar ist.

Lösung

1. Sie können mehr Speicher auf dem ESXi-Host bereitstellen.
1. Migrieren Sie den virtuellen Computer mithilfe von vMotion zu einem anderen ESXi-Host, auf dem ausreichend Speicher zum Starten des virtuellen Computers zur Verfügung steht.