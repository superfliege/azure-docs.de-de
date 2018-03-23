---
title: Problembehandlung von Fehlern während des Failbacks von virtuellen Azure-Computern auf lokale VMware-Systeme mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Möglichkeiten zur Problembehandlung bei allgemeinen Fehlern beim Failback und erneuten Schützen während des Failbacks auf VMware aus Azure mit Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/09/2018
ms.author: rajanaki
ms.openlocfilehash: 6dcecce78de3caaefb40cb3fe4853d5d550163b4
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Problembehandlung des Failbacks aus Azure auf VMware

Dieser Artikel beschreibt, wie Sie Probleme behandeln, die auftreten können, wenn Sie für virtuelle Azure-Computer ein Failback auf Ihre lokale VMware-Infrastruktur nach einem Failover in Azure mit [Azure Site Recovery](site-recovery-overview.md) ausgeführt haben.

Ein Failback umfasst im Wesentlichen zwei Schritte. Nach dem Failover müssen Sie virtuelle Azure-Computer lokal erneut schützen, damit sie mit der Replikation beginnen. Der zweite Schritt besteht darin, ein Failover aus Azure auszuführen, um ein Failback auf Ihren lokalen Standort zu erreichen.

## <a name="troubleshoot-reprotection-errors"></a>Problembehandlung von Fehlern beim erneuten Schützen

In diesem Abschnitt werden häufig auftretende Fehler beim erneuten Schützen und deren Behandlung beschrieben.

### <a name="error-code-95226"></a>Fehlercode 95226

**Fehler beim erneuten Anwenden des Schutzes, weil der virtuelle Azure-Computer den lokalen Konfigurationsserver nicht erreichen konnte.**

Dieser Fehler tritt unter den folgenden Umständen auf:

1. Der virtuelle Azure-Computer kann den lokalen Konfigurationsserver nicht erreichen. Der virtuelle Computer kann nicht ermittelt und beim Konfigurationsserver registriert werden.
2. Der InMage Scout-Anwendungsdienst wird nach dem Failover nicht auf dem virtuellen Azure-Computer ausgeführt. Der Dienst ist für die Kommunikation mit dem lokalen Konfigurationsserver erforderlich.

So lösen Sie dieses Problem:

1. Überprüfen Sie, ob das Azure-VM-Netzwerk es dem virtuellen Azure-Computer ermöglicht, mit dem lokalen Konfigurationsserver zu kommunizieren. Richten Sie zu diesem Zweck ein Site-to-Site-VPN mit Ihrem lokalen Datencenter ein, oder konfigurieren Sie eine ExpressRoute-Verbindung mit privatem Peering für das virtuelle Netzwerk des virtuellen Azure-Computers.
2. Wenn der virtuelle Computer mit dem lokalen Konfigurationsserver kommunizieren kann, melden Sie sich beim virtuellen Computer an, und überprüfen Sie den „InMage Scout-Anwendungsdienst“. Wenn Sie sehen, dass dieser nicht ausgeführt wird, starten Sie den Dienst manuell, und überprüfen Sie, ob der Starttyp des Diensts auf „Automatisch“ festgelegt ist.

### <a name="error-code-78052"></a>Fehlercode 78052

**Der Schutz konnte für den virtuellen Computer nicht abgeschlossen werden.**

Dies kann passieren, wenn sich bereits ein virtueller Computer mit dem gleichen Namen auf dem Masterzielserver befindet, auf den das Failback stattfindet.

Führen Sie folgende Schritte aus, um dieses Problem zu beheben:
1. Wählen Sie einen anderen Masterzielserver auf einem anderen Host aus, sodass der Computer beim erneuten Schützen auf einem anderen Host erstellt und der Namenskonflikt somit vermieden wird.
2. Sie können das Masterziel auch mithilfe von vMotion auf einen anderen Host übertragen, auf dem der Namenskonflikt nicht auftritt. Wenn es sich bei dem vorhandenen virtuellen Computer um einen vereinzelten Computer handelt, benennen Sie ihn um, sodass der neue virtuelle Computer auf demselben ESXi-Host erstellt werden kann.

### <a name="error-code-78093"></a>Fehlercode 78093

**Die VM wird nicht ausgeführt, reagiert nicht oder ist nicht erreichbar.**

Um einen virtuellen Computer erneut schützen zu können, für den ein Failover ausgeführt wurde, muss der virtuelle Azure-Computer ausgeführt werden. Dies ist erforderlich, damit sich der Mobilitätsdienst beim Konfigurationsserver lokal registrieren und mit der Replikation beginnen kann, indem er mit dem Prozessserver kommuniziert. Wenn sich der Computer in einem falschen Netzwerk befindet oder nicht ausgeführt wird (also nicht reagiert oder heruntergefahren ist), kann der Konfigurationsserver den Mobilitätsdienst auf dem virtuellen Computer nicht erreichen und den Schutz nicht erneut aktivieren.

1. Starten Sie den virtuellen Computer neu, damit er wieder mit der lokalen Umgebung kommunizieren kann.
2. Starten Sie nach dem Neustart des virtuellen Azure-Computers den Auftrag für erneutes Schützen neu.

### <a name="error-code-8061"></a>Fehlercode 8061

**Der Datenspeicher ist über den ESXi-Host nicht zugänglich.**

Informationen zum Failback finden Sie unter [Reprotect machines from Azure to an on-premises site](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) (Erneutes Schützen von Computern aus Azure an einem lokalen Standort).


## <a name="troubleshoot-failback-errors"></a>Problembehandlung von Failbackfehlern

In diesem Abschnitt werden häufige Fehler beschrieben, die beim Failback auftreten können.

### <a name="error-code-8038"></a>Fehlercode 8038

**Fehler beim Aktivieren des lokalen virtuellen Computers aufgrund des Fehlers**

Dies passiert, wenn der lokale virtuelle Computer auf einem Host aktiviert wird, auf dem nicht genügend Arbeitsspeicher verfügbar ist. So lösen Sie dieses Problem:

1. Stellen Sie mehr Speicher auf dem ESXi-Host bereit.
2. Außerdem können Sie den virtuellen Computer mithilfe von vMotion zu einem anderen ESXi-Host migrieren, auf dem ausreichend Speicher zum Starten des virtuellen Computers zur Verfügung steht.
