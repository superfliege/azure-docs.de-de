---
title: "Azure Site Recovery – Problembehandlung für „VMware zu Azure“ | Microsoft-Dokumentation"
description: Beheben von Fehlern beim Replizieren von virtuellen Azure-Computern
services: site-recovery
documentationcenter: 
author: asgang
manager: srinathv
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/28/2017
ms.author: asgang
ms.openlocfilehash: b7b03442ba815c86e5defa1018b66f56c0b379df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Beheben von Problemen bei Pushinstallationen von Mobility Service

In diesem Artikel werden häufige Probleme beschrieben, die beim Installieren von Azure Site Recovery Mobility Service auf dem Quellserver zum Aktivieren des Schutzes auftreten können.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Fehler 78007 – Der angeforderte Vorgang konnte nicht abgeschlossen werden.
Dieser Fehler kann aus mehreren Gründen vom Dienst ausgelöst werden. Wählen Sie den entsprechenden Anbieterfehler aus, um das Problem weiter einzugrenzen.

* [Fehler 95103](#error-95103---protection-could-not-be-enabled-ep0854) 
* [Fehler 95105](#error-95105---protection-could-not-be-enabled-ep0856) 
* [Fehler 95107](#error-95107---protection-could-not-be-enabled-ep0858) 
* [Fehler 95108](#error-95108---protection-could-not-be-enabled-ep0859) 
* [Fehler 95117](#error-95117---protection-could-not-be-enabled-ep0865) 
* [Fehler 95213](#error-95213---protection-could-not-be-enabled-ep0874) 
* [Fehler 95224](#error-95224---protection-could-not-be-enabled-ep0883) 
* [Fehler 95265](#error-95265---protection-could-not-be-enabled-ep0902) 


## <a name="error-95105---protection-could-not-be-enabled-ep0856"></a>Fehler 95105 – Der Schutz konnte nicht aktiviert werden (EP0856).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95105 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0856**. <br> Entweder ist die **Datei- und Druckerfreigabe** auf dem Quellcomputer nicht zulässig, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer.| Die **Datei- und Druckerfreigabe** ist nicht aktiviert. | Lassen Sie die **Datei- und Druckerfreigabe** auf dem Quellcomputer in der Windows-Firewall zu. Wählen Sie auf dem Quellcomputer unter **Windows-Firewall** > **Eine App oder ein Feature durch die Windows-Firewall zulassen** die Option **Datei- und Druckerfreigabe für alle Profile** aus. </br> Überprüfen Sie außerdem, ob die unten aufgeführten Voraussetzungen erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.<br> Weitere Informationen zur [Problembehandlung bei WMI-Problemen](#troubleshoot-wmi-issues).


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Fehler 95107 – Der Schutz konnte nicht aktiviert werden (EP0858).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95107 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0858**. <br> Entweder sind die Anmeldeinformationen für das Installieren von Mobility Service falsch, oder das Benutzerkonto verfügt nicht über ausreichende Berechtigungen. | Die auf dem Quellcomputer angegebenen Benutzeranmeldeinformationen zum Installieren von Mobility Service sind falsch. | Stellen Sie sicher, dass die für den Quellcomputer auf dem Konfigurationsserver angegebenen Anmeldeinformationen richtig sind. <br> Navigieren Sie zum Hinzufügen oder Bearbeiten von Benutzeranmeldeinformationen zum Konfigurationsserver, und wählen Sie **Cspsconfigtool** > **Konto verwalten** aus. </br> Überprüfen Sie außerdem, ob die unten aufgeführten [Voraussetzungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Fehler 95117 – Der Schutz konnte nicht aktiviert werden (EP0865).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95117 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0865**. <br> Entweder wird der Quellcomputer nicht ausgeführt, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer. | Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellserver. | Überprüfen Sie die Verbindung zwischen dem Prozessserver und dem Quellserver. </br> Überprüfen Sie außerdem, ob die unten aufgeführten [Voraussetzungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Fehler 95103 – Der Schutz konnte nicht aktiviert werden (EP0854).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95103 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0854**. <br> Entweder ist die Windows-Verwaltungsinstrumentation (WMI) auf dem Quellcomputer nicht zulässig, oder es bestehen Netzwerkverbindungsprobleme zwischen dem Prozessserver und dem Quellcomputer.| WMI ist in der Windows-Firewall blockiert. | Lassen Sie WMI in der Windows-Firewall zu. Wählen Sie unter **Windows-Firewall** > **Eine App oder ein Feature durch die Windows-Firewall zulassen** die Option **WMI für alle Profile** aus. </br> Überprüfen Sie außerdem, ob die unten aufgeführten [Voraussetzungen](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Fehler 95213 – Der Schutz konnte nicht aktiviert werden (EP0874).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95213 </br>**Meldung:** Fehler beim Installieren des Mobility Service auf dem Quellcomputer „%SourceIP“, mit dem Fehlercode **EP0874**. <br> | Die Version des Betriebssystems wird auf dem Quellcomputer nicht unterstützt. <br>| Stellen Sie sicher, dass die Version des Betriebssystems auf dem Quellcomputer unterstützt wird. Weitere Informationen finden Sie in der [Supportmatrix](https://aka.ms/asr-os-support). </br> Überprüfen Sie außerdem, ob die unten aufgeführten [Voraussetzungen](https://aka.ms/pushinstallerror) erfüllt sind, um die Pushinstallation erfolgreich abzuschließen.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Fehler 95108 – Der Schutz konnte nicht aktiviert werden (EP0859).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95108 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer mit dem Fehlercode **EP0859**. <br>| Entweder sind die Anmeldeinformationen für das Installieren von Mobility Service falsch, oder das Benutzerkonto verfügt nicht über ausreichende Berechtigungen. <br>| Stellen Sie sicher, dass die bereitgestellten Anmeldeinformationen die Anmeldeinformationen für das **root**-Konto sind. Navigieren Sie zum [Hinzufügen oder Bearbeiten von Benutzeranmeldeinformationen](site-recovery-vmware-to-azure-manage-configuration-server.md#modify-user-accounts-and-passwords) zum Konfigurationsserver, und klicken Sie auf dem Desktop auf das Verknüpfungssymbol „Cspsconfigtool“. Klicken Sie auf „Konto verwalten“, um Anmeldeinformationen hinzuzufügen bzw. zu bearbeiten.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Fehler 95265 – Der Schutz konnte nicht aktiviert werden (EP0902).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95265 </br>**Meldung:** Die Pushinstallation des Mobility Service auf dem Quellcomputer wurde erfolgreich durchgeführt. Der Quellcomputer muss jedoch neu gestartet werden, damit einige Systemänderungen wirksam werden. <br>| Eine ältere Version des Mobility Service wurde bereits auf dem Server installiert.| Die Replikation des virtuellen Computers wird nahtlos fortgesetzt.<br> Starten Sie den Server während des nächsten Wartungsfensters neu, um die Vorteile der neuen Mobility Service-Erweiterungen zu erhalten.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Fehler 95224 – Der Schutz konnte nicht aktiviert werden (EP0883).

**Fehlercode** | **Mögliche Ursachen** | **Fehlerspezifische Empfehlungen**
--- | --- | ---
95224 </br>**Meldung:** Fehler bei der Pushinstallation des Mobility Service auf dem Quellcomputer „%SourceIP“, mit dem Fehlercode EP0883. Ein Neustart des Systems aufgrund einer vorherigen Installation bzw. eines Updates steht an.| Das System wurde nach der Deinstallation einer älteren bzw. inkompatiblen Version des Mobility Service nicht neu gestartet.| Stellen Sie sicher, dass keine Version des Mobility Service auf dem Server vorhanden ist. <br> Starten Sie den Server neu, und führen Sie den Auftrag zur Schutzaktivierung erneut aus.|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Ressource zur Behandlung von Problemen bei der Pushinstallation

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Behandeln von Problemen bei der Datei- und Druckerfreigabe
*  [Aktivieren oder Deaktivieren der Dateifreigabe mit der Gruppenrichtlinie](https://technet.microsoft.com/en-us/library/cc754359(v=ws.10).aspx)
* [Aktivieren der Datei- und Druckfreigabe über die Windows-Firewall](https://technet.microsoft.com/en-us/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>Behandeln von WMI-Problemen
* [Grundlegende WMI-Tests](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI-Problembehandlung](https://msdn.microsoft.com/en-us/library/aa394603(v=vs.85).aspx)
* [Problembehandlung bei WMI-Skripts und WMI-Diensten](https://technet.microsoft.com/en-us/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Nächste Schritte
- [Aktivieren der Replikation für virtuelle VMware-VMs](vmware-walkthrough-enable-replication.md)
