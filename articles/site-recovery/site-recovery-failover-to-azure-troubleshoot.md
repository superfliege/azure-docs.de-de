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
ms.date: 09/11/2018
ms.author: ponatara
ms.openlocfilehash: 6be71424e30c5783a03b157171b3f5acd0160e65
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391009"
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

## <a name="unable-to-connectrdpssh---vm-connect-button-grayed-out"></a>Verbindung nicht möglich/RDP/SSH – Schaltfläche zum Verbinden des virtuellen Computers abgeblendet

Wenn die Schaltfläche **Verbinden** auf dem fehlerhaften virtuellen Computer abgeblendet ist und Sie nicht über ExpressRoute oder Site-to-Site-VPN mit Azure verbunden sind, gehen Sie wie folgt vor:

1. Navigieren Sie zu **VM** > **Netzwerk**, und klicken Sie auf den Namen der jeweiligen Netzwerkschnittstelle.  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navigieren Sie zu **IP-Konfigurationen**, und klicken Sie dann auf das Namensfeld der gewünschten IP-Konfiguration. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Um die öffentliche IP-Adresse zu aktivieren, klicken Sie auf **Aktivieren**. ![Aktivieren der IP-Adresse](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Klicken Sie auf **Erforderliche Einstellungen konfigurieren** > **Neue erstellen**. ![Neue erstellen](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Geben Sie den Namen der öffentliche Adresse ein, wählen Sie die Standardoptionen für **SKU** und **Zuweisung**, und klicken Sie dann auf **OK**.
6. Um die Änderungen nun zu speichern, klicken Sie auf **Speichern**.
7. Schließen Sie die Bereiche, und navigieren Sie zum Abschnitt **Übersicht** des virtuellen Computers, mit dem per RDP eine Verbindung hergestellt werden soll.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>Verbindung nicht möglich/RDP/SSH – Schaltfläche zum Verbinden des virtuellen Computers verfügbar

Wenn die Schaltfläche **Verbinden** auf dem fehlerhaften virtuellen Computer in Azure verfügbar ist (nicht abgeblendet), überprüfen Sie **Startdiagnose** auf Ihrem virtuellen Computer, und suchen Sie nach den in [diesem Artikel](../virtual-machines/windows/boot-diagnostics.md) aufgeführten Fehlern.

1. Falls der virtuelle Computer nicht gestartet wurde, führen Sie ein Failover auf einen früheren Wiederherstellungspunkt aus.
2. Wird die Anwendung auf dem virtuellen Computer nicht gestartet, führen Sie ein Failover auf einen anwendungskonsistenten Wiederherstellungspunkt aus.
3. Ist der virtuelle Computer Mitglied einer Domäne, stellen Sie sicher, dass der Domänencontroller ordnungsgemäß funktioniert. Dies kann durch Ausführen der unten angegebenen Schritte geschehen:

    a. Erstellen Sie einen neuen virtuellen Computer im selben Netzwerk.

    b.  Stellen Sie sicher, dass dieser derselben Domäne beitreten kann, in der der fehlerhafte virtuelle Computer erwartet wird.

    c. Funktioniert der Domänencontroller **nicht** ordnungsgemäß, melden Sie sich bei dem virtuellen Computer, für den ein Failover ausgeführt wurde, mit einem lokalen Administratorkonto an.
4. Vergewissern Sie sich bei Verwendung eines benutzerdefinierten DNS-Servers, dass dieser erreichbar ist. Dies kann durch Ausführen der unten angegebenen Schritte geschehen:

    a. Erstellen Sie einen neuen virtuellen Computer im selben Netzwerk.

    b. Überprüfen Sie, ob der virtuelle Computer Namen mit dem benutzerdefinierten DNS-Server auflösen kann.

>[!Note]
>Der Azure-VM-Agent muss vor dem Failover auf dem virtuellen Computer installiert werden, wenn Sie andere Einstellungen als die Startdiagnose aktivieren möchten.

## <a name="unexpected-shutdown-message-event-id-6008"></a>Nachricht über unerwartetes Herunterfahren (Ereignis-ID 6008)

Wenn beim Starten eines virtuellen Windows-Computers nach dem Failover auf dem wiederhergestellten virtuellen Computer eine Nachricht über ein unerwartetes Herunterfahren ausgegeben wird, bedeutet dies, dass ein Status des Herunterfahrens des virtuellen Computers nicht in dem für das Failover verwendeten Wiederherstellungspunkt erfasst wurde. Dies geschieht, wenn Sie eine Wiederherstellung zu einem Zeitpunkt durchführen, zu dem der virtuelle Computer nicht vollständig heruntergefahren war.

Normalerweise ist das kein Anlass zur Sorge und kann in der Regel für ungeplante Failover ignoriert werden. Stellen Sie im Fall eines geplanten Failovers sicher, dass der virtuelle Computer vor dem Failover ordnungsgemäß heruntergefahren wird, und gewähren Sie ausreichend Zeit, damit ausstehende lokale Daten zur Replikation an Azure gesendet werden können. Verwenden Sie dann die Option **Neueste** auf dem Bildschirm [Failover](site-recovery-failover.md#run-a-failover), damit alle ausstehenden Daten in Azure in einem Wiederherstellungspunkt verarbeitet werden, der dann für VM-Failover verwendet wird.

## <a name="retaining-drive-letter-after-failover"></a>Beibehalten von Laufwerkbuchstaben nach einem Failover
Wenn Sie nach dem Failover den Laufwerkbuchstaben des virtuellen Computers beibehalten möchten, können Sie die **SAN-Richtlinie** für den virtuellen Computer lokal auf **OnlineAll** festlegen. [Weitere Informationen](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure)

## <a name="next-steps"></a>Nächste Schritte
- Beheben von Fehlern bei der [RDP-Verbindung mit einem virtuellen Windows-Computer](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Beheben von Fehlern bei der [SSH-Verbindung mit einem virtuellen Linux-Computer](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihre Abfrage im [Site Recovery-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr), oder hinterlassen einen Kommentar am Ende dieses Dokuments. Wir verfügen über eine aktive Community, die Sie unterstützen kann.
