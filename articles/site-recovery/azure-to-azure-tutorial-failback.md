---
title: Ausführen eines Failbacks von in einer sekundären Azure-Region replizierten virtuellen Azure IaaS-Computern zur Notfallwiederherstellung mit dem Azure Site Recovery-Dienst
description: Es wird beschrieben, wie Sie mit dem Azure Site Recovery-Dienst ein Failback für Azure-VMs durchführen.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: sideeksh
ms.custom: mvc
ms.openlocfilehash: d8721f313907f0e0519dca52f5565853f1c44110
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58089698"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Failback für Azure-VMs zwischen Azure-Regionen

Der Dienst [Azure Site Recovery](site-recovery-overview.md) verwaltet und orchestriert die Replikation, das Failover und das Failback von lokalen Computern sowie Azure-VMs und unterstützt dadurch Ihre Strategie zur Notfallwiederherstellung.

In diesem Tutorial wird beschrieben, wie Sie das Failback für eine einzelne Azure-VM durchführen. Nach dem Failover erfolgt ein Failback auf die primäre Region, wenn sie verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> 
> * Ausführen des Failbacks der sekundären VM
> * Erneutes Schützen der primären VM zurück zur sekundären Region
> 
> [!NOTE]
> 
> In diesem Tutorial werden für Benutzer die Schritte beschrieben, mit denen mit möglichst geringem Anpassungsaufwand ein Failover in eine Zielregion und zurück ausgeführt wird. Falls Sie mehr zu den verschiedenen Aspekten von Failovern erfahren möchten, z. B. zu Netzwerküberlegungen, Automatisierung oder Problembehandlung, helfen Ihnen die Gewusst wie-Dokumente für Azure-VMs weiter.

## <a name="prerequisites"></a>Voraussetzungen

> * Stellen Sie sicher, dass sich die VM im Status „Commit für Failover ausgeführt“ befindet, dass die primäre Region verfügbar ist und dass Sie neue Ressourcen darin erstellen und darauf zugreifen können.
> * Stellen Sie sicher, dass die Option für das erneute Schützen aktiviert ist.

## <a name="fail-back-to-the-primary-region"></a>Ausführen eines Failbacks auf die primäre Region

Wenn die VMs wieder geschützt sind, können Sie jederzeit ein Failback in die primäre Region durchführen, wenn Sie dies wünschen.

1. Navigieren Sie zu Ihrem Recovery Services-Tresor. Klicken Sie auf „Replizierte Elemente“, und wählen Sie die VM aus, die erneut geschützt wurde.

2. Folgendes sollte angezeigt werden. Beachten Sie, dass dies dem Blatt für das Testfailover und dem Failover aus der primären Region ähnelt.
![Failback in primäre Region](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Klicken Sie auf „Testfailover“, um ein Testfailover zurück in Ihre primäre Region durchzuführen. Wählen Sie den Wiederherstellungspunkt und das virtuelle Netzwerk für das Testfailover aus, und wählen Sie dann „OK“. Sie können den in der primären Region erstellten virtuellen Testcomputer anzeigen und darauf zugreifen und ihn untersuchen.

4. Nachdem das Testfailover wie gewünscht durchgeführt wurde, können Sie auf „Testfailover bereinigen“ klicken, um Ressourcen zu bereinigen, die in der Quellregion für das Testfailover erstellt wurden.

5. Wählen Sie unter „Replizierte Elemente“ die VM, für die ein Failover ausgeführt werden soll, und dann „Failover“ aus.

6. Wählen Sie unter „Failover“ einen Wiederherstellungspunkt für das Failover aus. Sie können eine der folgenden Optionen auswählen:
    1. Neuester (Standardeinstellung): Mit dieser Option werden alle Daten in Site Recovery verarbeitet, und sie verfügt über den niedrigsten RPO-Wert (Recovery Point Objective).
    2. Letzte Verarbeitung: Mit dieser Option wird für den virtuellen Computer der Zustand des letzten Wiederherstellungspunkts wiederhergestellt, der von Site Recovery verarbeitet wurde.
    3. Benutzerdefiniert: Verwenden Sie diese Option für ein Failover auf einen bestimmten Wiederherstellungspunkt. Diese Option eignet sich für ein Testfailover.

7. Klicken Sie auf „Der Computer wird vor Beginn des Failovers heruntergefahren“, wenn Site Recovery versuchen soll, virtuelle Quellcomputer herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Hinweis: Bei einer Sitewiederherstellung wird die Quelle nach dem Failover nicht bereinigt.

8. Der Fortschritt des Failovers wird auf der Seite „Aufträge“ angezeigt.

9. Überprüfen Sie den virtuellen Computer nach Abschluss des Failovers, indem Sie sich am Computer anmelden. Falls Sie einen anderen Wiederherstellungspunkt für den virtuellen Computer verwenden möchten, können Sie die Option „Wiederherstellungspunkt ändern“ wählen.

10. Nachdem Sie mit dem virtuellen Computer, für den das Failover durchgeführt wurde, zufrieden sind, können Sie für das Failover die Option „Commit“ wählen. Dadurch werden alle mit dem Dienst verfügbaren Wiederherstellungspunkte gelöscht. Die Option „Wiederherstellungspunkt ändern“ ist nicht mehr verfügbar.

![VM in primärer und sekundärer Region](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

Im vorhergehenden Screenshot wurde für den virtuellen Computer „ContosoWin2016“ ein Failover von der Region „USA, Mitte“ zur Region „USA, Osten“ und ein Failback von „USA, Osten“ zu „USA, Mitte“ ausgeführt.

Die virtuellen Computer für die Notfallwiederherstellung verbleiben im heruntergefahrenen Zustand mit aufgehobener Zuordnung. Dieses Verhalten ist beabsichtigt, da Azure Site Recovery die Informationen des virtuellen Computers speichert. Dies kann später beim Failover von der primären Region zur sekundären Region nützlich sein. Da für virtuelle Computer mit aufgehobener Zuordnung keine Gebühren anfallen, empfiehlt es sich, sie beizubehalten.

> [!NOTE]
> Weitere Informationen zum Workflow für den erneuten Schutz und dazu, was während des erneuten Schutzes passiert, finden Sie unter [Erneutes Schützen von virtuellen Azure-Computern, für die ein Failover zur primären Region durchgeführt wurde](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection).
