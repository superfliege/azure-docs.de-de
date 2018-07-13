---
title: Beheben von Problemen bei einem Failover nach Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt Möglichkeiten zum Beheben allgemeiner Probleme beim Failover nach Azure.
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: ad8b69bfe6f3261f00cd33846efc86ce3b198954
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919691"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Beheben von Problemen beim Ausführen eines Failovers eines virtuellen Computers nach Azure

Bei einem Failover eines virtuellen Computers nach Azure tritt eventuell einer der folgenden Fehler auf. Gehen Sie für die Problembehandlung gemäß den für den jeweiligen Fehlerzustand beschriebenen Schritten vor.

## <a name="failover-failed-with-error-id-28031"></a>Fehler beim Failover mit der Fehler-ID 28031

Site Recovery konnte einen virtuellen Computer, für den ein Failover ausgeführt wurde, nicht in Azure erstellen. Dies kann aus einem der folgenden Gründe auftreten:

* Das verfügbare Kontingent reicht nicht zum Erstellen des virtuellen Computers: Sie können das verfügbare Kontingent unter „Abonnement“ > „Nutzung + Kontingente“ prüfen. Sie können eine [neue Supportanfrage](http://aka.ms/getazuresupport) eröffnen, um das Kontingent zu erhöhen.

* Sie versuchen, ein Failover für virtuelle Computer unterschiedlicher Größensätze in derselben Verfügbarkeitsgruppe durchzuführen. Verwenden Sie unbedingt dieselben Größen Familien für alle virtuellen Computer in derselben Verfügbarkeitsgruppe. Sie können die Größe ändern, indem Sie zu den Einstellungen für Compute und Netzwerk des virtuellen Computers navigieren. Wiederholen Sie das Failover anschließend.

* Eine Richtlinie für das Abonnement verhindert die Erstellung eines virtuellen Computers. Ändern Sie die Richtlinie, um das Erstellen eines virtuellen Computers zuzulassen, und wiederholen dann das Failover.

## <a name="failover-failed-with-error-id-28092"></a>Fehler beim Failover mit der Fehler-ID 28092

Site Recovery konnte für den virtuellen Computer, für den ein Failover ausgeführt wurde, keine Netzwerkschnittstelle erstellen. Stellen Sie sicher, dass Sie in Ihrem Abonnement über ein ausreichendes Kontingent zum Erstellen von Netzwerkschnittstellen verfügen. Sie können das verfügbare Kontingent unter „Abonnement“ > „Nutzung + Kontingente“ prüfen. Sie können eine [neue Supportanfrage](http://aka.ms/getazuresupport) eröffnen, um das Kontingent zu erhöhen. Wenn Sie über ein ausreichendes Kontingent verfügen, ist dies möglicherweise nur ein vorübergehendes Problem. Wiederholen Sie den Vorgang. Wenn das Problem auch nach Wiederholungen weiter besteht, hinterlassen Sie am Ende dieses Dokuments einen Kommentar.  

## <a name="failover-failed-with-error-id-70038"></a>Fehler beim Failover mit der Fehler-ID 70038

Site Recovery konnte einen klassischen virtuellen Computer, für den ein Failover ausgeführt wurde, nicht in Azure erstellen. Dies kann mehrere Ursachen haben:

* Eine der Ressourcen – z.B. ein virtuelles Netzwerk, das für die Erstellung des virtuellen Computers erforderlich ist – ist nicht vorhanden. Erstellen Sie das virtuelle Netzwerk wie in den Einstellungen für Compute und Netzwerk des virtuellen Computers angegeben, oder ändern Sie die Einstellung in ein virtuelles Netzwerk, das bereits vorhanden ist. Wiederholen Sie dann das Failover.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Fehler beim Verbindungsaufbau über RDP/SSH mit dem virtuellen Computer (VM), für den ein Failover ausgeführt wurde, aufgrund ausgegrauter Schaltfläche „Verbinden“ auf der VM

Wenn die Schaltfläche „Verbinden“ ausgegraut ist und Sie über eine ExpressRoute- oder Standort-zu-Standort-VPN-Verbindung keine Verbindung mit Azure herstellen können, gehen Sie wie folgt vor:

1. Navigieren Sie zu **VM** > **Netzwerk**, und klicken Sie auf den Namen der jeweiligen Netzwerkschnittstelle.  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navigieren Sie zu **IP-Konfigurationen**, und klicken Sie dann auf das Namensfeld der gewünschten IP-Konfiguration. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Um die öffentliche IP-Adresse zu aktivieren, klicken Sie auf **Aktivieren**. ![Aktivieren der IP-Adresse](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Klicken Sie auf **Erforderliche Einstellungen konfigurieren** > **Neue erstellen**. ![Neue erstellen](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Geben Sie den Namen der öffentliche Adresse ein, wählen Sie die Standardoptionen für **SKU** und **Zuweisung**, und klicken Sie dann auf **OK**.
6. Um die Änderungen nun zu speichern, klicken Sie auf **Speichern**.
7. Schließen Sie die Bereiche, und navigieren Sie zum Abschnitt **Übersicht** des virtuellen Computers, mit dem per RDP eine Verbindung hergestellt werden soll.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>Fehler beim Verbindungsaufbau über RDP/SSH mit dem virtuellen Computer (VM), für den ein Failover ausgeführt wurde, trotz angezeigter Schaltfläche „Verbinden“ (nicht ausgegraut) auf der VM

Aktivieren Sie die **Startdiagnose** auf Ihrem virtuellen Computer, und prüfen Sie auf Fehler, die in diesem Artikel aufgeführt werden.

1. Falls der virtuelle Computer nicht gestartet wurde, führen Sie ein Failover auf einen früheren Wiederherstellungspunkt aus.
2. Wird die Anwendung auf dem virtuellen Computer nicht gestartet, führen Sie ein Failover auf einen anwendungskonsistenten Wiederherstellungspunkt aus.
3. Ist der virtuelle Computer Mitglied einer Domäne, stellen Sie sicher, dass der Domänencontroller ordnungsgemäß funktioniert. Dies kann durch Durchführen der unten angegebenen Schritte geschehen.
    a. Erstellen Sie einen neuen virtuellen Computer im selben Netzwerk.

    b.  Stellen Sie sicher, dass dieser der gleichen Domäne beitreten kann, in der der fehlerhafte virtuelle Computer aller Wahrscheinlichkeit nach eingesetzt wird.

    c. Funktioniert der Domänencontroller **nicht** ordnungsgemäß, melden Sie sich bei dem virtuellen Computer, für den ein Failover ausgeführt wurde, mit einem lokalen Administratorkonto an.
4. Vergewissern Sie sich bei Verwendung eines benutzerdefinierten DNS-Servers, dass dieser erreichbar ist. Dies kann durch Durchführen der unten angegebenen Schritte geschehen.
    a. Erstellen Sie einen neuen virtuellen Computer im selben Netzwerk. b. Überprüfen Sie, ob der virtuelle Computer Namen mit dem benutzerdefinierten DNS-Server auflösen kann.

>[!Note]
>Der Azure-VM-Agent muss vor dem Failover auf dem virtuellen Computer installiert werden, wenn Sie andere Einstellungen als die Startdiagnose aktivieren möchten.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihre Abfrage im [Site Recovery-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr), oder hinterlassen einen Kommentar am Ende dieses Dokuments. Wir verfügen über eine aktive Community, die Sie unterstützen kann.
