---
title: Beheben von Replikationsproblemen bei der Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Problembehandlung für häufige Replikationsprobleme bei der Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure mit Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: ae2f32a02005bc015d2521e576ea5625bef2d377
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846010"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Beheben von Problemen bei der Replikation von VMware-VMs und physischen Servern

Sie erhalten unter Umständen eine bestimmte Fehlermeldung, wenn Sie ihre virtuellen VMware-Computer oder Ihre physischen Server mithilfe von Azure Site Recovery schützen. Dieser Artikel beschreibt einige häufig auftretende Probleme bei der Replikation von lokalen VMware-VMs und physischen Servern in Azure mithilfe von [Azure Site Recovery](site-recovery-overview.md).

## <a name="initial-replication-issues"></a>Probleme bei der Erstreplikation

In vielen Fällen treten bei der ersten Replikation aufgrund von Konnektivitätsproblemen zwischen dem Quellserver und dem Prozessserver oder zwischen dem Prozessserver und Azure Fehler auf. In den meisten Fällen können Sie diese Probleme beheben, indem Sie die folgenden Schritte ausführen.

### <a name="verify-the-source-machine"></a>Überprüfen des Quellcomputers
* Verwenden Sie in der Befehlszeile des Quellservercomputers wie unten gezeigt Telnet zum Pingen des Prozessservers mit dem https-Port (Standard: 9443), um festzustellen, ob Netzwerkkonnektivitätsprobleme bestehen oder der Port durch die Firewall blockiert wird.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Testen Sie die Konnektivität mit Telnet und nicht mit PING.  Wenn Telnet nicht installiert ist, führen Sie die [hier](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) angegebenen Schritte aus.

Wenn keine Verbindung hergestellt werden kann, lassen Sie den eingehenden Port 9443 auf dem Prozessserver zu, und überprüfen Sie, ob das Problem weiterhin besteht. In der Vergangenheit wurde dieses Problem bereits dadurch hervorgerufen, dass sich der Prozessserver hinter DMZ befunden hat.

* Prüfen Sie den Status des Diensts `InMage Scout VX Agent – Sentinel/OutpostStart`, wenn er nicht ausgeführt wird, und überprüfen Sie, ob das Problem weiterhin besteht.   

## <a name="verify-the-process-server"></a>Überprüfen des Prozessservers

* **Überprüfen Sie, ob der Prozessserver aktiv Daten mithilfe von Push an Azure überträgt.**

Öffnen Sie den Task-Manager auf dem Prozessservercomputer (drücken Sie STRG+UMSCHALT+ESC). Wechseln Sie zur Registerkarte „Leistung“, und klicken Sie auf den Link „Ressourcenmonitor öffnen“. Wechseln Sie ausgehend vom Resource Manager zur Registerkarte „Netzwerk“. Prüfen Sie ob „cbengine.exe“ unter „Prozesse mit Netzwerkaktivität“ aktiv große Datenmengen (in MB) sendet.

![Aktivieren der Replikation](./media/vmware-azure-troubleshoot-replication/cbengine.png)

Führen Sie andernfalls die folgenden Schritte aus:

* **Überprüfen Sie, ob der Prozessserver eine Verbindung mit dem Azure-Blob herstellen kann:** Wählen Sie „cbengine.exe“ aus, und überprüfen Sie unter „TCP-Verbindungen“, ob zwischen dem Prozessserver und der Azure Storage-Blob-URL eine Verbindung besteht.

![Aktivieren der Replikation](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

Navigieren Sie andernfalls zur Systemsteuerung > „Dienste“, und prüfen Sie, ob die folgenden Dienste ausgeführt werden:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
Starten Sie alle nicht ausgeführten Dienste (bzw. starten Sie sie neu), und überprüfen Sie, ob das Problem weiterhin besteht.

* **Prüfen Sie, ob der Prozessserver mithilfe von Port 443 eine Verbindung mit der öffentlichen Azure-IP-Adresse herstellen kann.**

Öffnen Sie das aktuelle Fehlerprotokoll („CBEngineCurr.errlog“) unter `%programfiles%\Microsoft Azure Recovery Services Agent\Temp`, und suchen Sie nach „443“ und „connection attempt failed“.

![Aktivieren der Replikation](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

Wenn Probleme auftreten, verwenden Sie in der Befehlszeile des Prozessservers Telnet zum Pingen Ihrer öffentlichen Azure-IP-Adresse (im der Abbildung oben unleserlich gemacht), die Sie mithilfe von Port 443 in „CBEngineCurr.currLog“ gefunden haben.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Wenn Sie keine Verbindung herstellen können, prüfen Sie, ob das Zugriffsproblem durch die Firewall oder den Proxy verursacht wird.


* **Vergewissern Sie sich, dass die auf IP-Adressen basierende Firewall auf dem Prozessserver den Zugriff nicht blockiert:** Wenn Sie auf IP-Adressen basierende Firewallregeln auf dem Server verwenden, laden Sie [hier](https://www.microsoft.com/download/details.aspx?id=41653) die vollständige Liste der IP-Bereiche für das Microsoft Azure-Rechenzentrum herunter, und fügen Sie diese Ihrer Firewallkonfiguration hinzu, um sicherzustellen, dass eine Kommunikation mit Azure (und dem HTTPS-Port 443) zugelassen wird.  Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.

* **Vergewissern Sie sich, dass die auf URLs basierende Firewall auf dem Prozessserver den Zugriff nicht blockiert:**  Wenn Sie URL-basierte Firewallregeln auf dem Server verwenden, stellen Sie sicher, dass der Firewallkonfiguration die folgenden URLs hinzugefügt wurden.

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

* **Prüfen Sie, ob die Proxyeinstellungen auf dem Prozessserver den Zugriff blockieren**.  Wenn Sie einen Proxyserver verwenden, stellen Sie sicher, dass der Proxyservername durch den DNS-Server aufgelöst wird.
So überprüfen Sie, was Sie bei der Einrichtung des Konfigurationsservers angegeben haben: Navigieren Sie zum Registrierungsschlüssel:

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Stellen Sie nun sicher, dass die gleichen Einstellungen vom Azure Site Recovery-Agent zum Senden von Daten verwendet werden.
Suchen Sie nach „Microsoft Azure Backup“.

Klicken Sie nach dem Öffnen auf „Aktion“ > „Eigenschaften ändern“. Auf der Registerkarte „Proxykonfiguration“ wird Ihnen die Proxyadresse angezeigt, die mit der unter den Registrierungseinstellungen angezeigten Adresse übereinstimmen muss. Wenn dies nicht der Fall ist, passen Sie die Adresse an.


* **Vergewissern Sie sich, dass die Bandbreite auf dem Prozessserver nicht eingeschränkt wird:**  Erhöhen Sie die Bandbreite, und überprüfen Sie, ob das Problem weiterhin besteht.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Unterstützung benötigen, können Sie Ihre Frage im [Azure Site Recovery-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veröffentlichen. Unsere Community ist sehr aktiv, und einer unserer Techniker wird Sie bei Ihrem Problem unterstützen.
