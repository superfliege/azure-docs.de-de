---
title: Beheben von Replikationsproblemen bei der Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Problembehandlung für häufige Replikationsprobleme bei der Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure mit Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/7/2019
ms.author: mayg
ms.openlocfilehash: 71c07d93d75ee372a50ec4ff5fc81e92926d329b
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964772"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Beheben von Problemen bei der Replikation von VMware-VMs und physischen Servern

Sie erhalten unter Umständen eine bestimmte Fehlermeldung, wenn Sie ihre virtuellen VMware-Computer oder Ihre physischen Server mithilfe von Azure Site Recovery schützen. Dieser Artikel beschreibt einige häufig auftretende Probleme bei der Replikation von lokalen VMware-VMs und physischen Servern in Azure mithilfe von [Site Recovery](site-recovery-overview.md).

## <a name="monitor-process-server-health-to-avoid-replication-issues"></a>Überwachen der Prozessserverintegrität zur Vermeidung von Replikationsproblemen

Es wird empfohlen, die Integrität des Prozessservers (PS) im Portal zu überwachen, um sicherzustellen, dass die Replikation für Ihre zugeordneten Quellcomputer durchgeführt wird. Navigieren Sie im Tresor zu „Verwalten“ > „Site Recovery-Infrastruktur“ > „Konfigurationsserver“. Klicken Sie auf dem Blatt „Konfigurationsserver“ unter „Zugeordnete Server“ auf den Prozessserver. Das Blatt „Prozessserver“ mit den Integritätsstatistiken wird geöffnet. Sie können die CPU-Auslastung, die Speicherauslastung, den Status der für die Replikation erforderlichen PS-Dienste, das Zertifikatablaufdatum und den verfügbaren freien Speicher nachverfolgen. Der Status aller Statistiken sollte grün angezeigt werden. 

**Speicher- und CPU-Auslastung sollten unter 70 Prozent liegen, und es sollte 25 Prozent freier Speicherplatz verfügbar sein.** Freier Speicherplatz bezieht sich auf den Speicherplatz des Cachedatenträgers auf dem Prozessserver, in dem die Replikationsdaten von Quellcomputern vor dem Hochladen in Azure gespeichert werden. Stehen weniger als 20 Prozent Speicherplatz zur Verfügung, wird die Replikation für alle zugeordneten Quellcomputer gedrosselt. Sehen Sie sich die [Kapazitätsrichtlinien](./site-recovery-plan-capacity-vmware.md#capacity-considerations) an, um die erforderliche Konfiguration für die Replikation der Quellcomputer nachvollziehen zu können.

Stellen Sie sicher, das die unten aufgeführten Dienste auf dem PS-Computer ausgeführt werden. Starten Sie jeden der Dienste, der nicht ausgeführt wird, bzw. starten Sie ihn neu.

**Integrierter Prozessserver**

* cxprocessserver
* InMage PushInstall
* Dienst für den Protokollupload (LogUpload)
* InMage Scout-Anwendungsdienst
* Microsoft Azure Recovery Services-Agent (OBEngine)
* InMage Scout VX Agent – Sentinel/Outpost (svagents)
* tmansvc
* WWW-Publishingdienst (W3SVC)
* MySQL
* Microsoft Azure Site Recovery-Dienst (dra)

**Horizontal skalierter Prozessserver**

* cxprocessserver
* InMage PushInstall
* Dienst für den Protokollupload (LogUpload)
* InMage Scout-Anwendungsdienst
* Microsoft Azure Recovery Services-Agent (OBEngine)
* InMage Scout VX Agent – Sentinel/Outpost (svagents)
* tmansvc

**Prozessserver für das Failback in Azure**

* cxprocessserver
* InMage PushInstall
* Dienst für den Protokollupload (LogUpload)

Stellen Sie sicher, dass „StartType“ aller Dienste auf **„Automatisch“ oder „Automatisch (Verzögerter Start)“** festgelegt ist. Für den Dienst für den Microsoft Azure Recovery Services-Agent (OBEngine) muss „StartType“ nicht wie oben angegeben festgelegt werden.

## <a name="initial-replication-issues"></a>Probleme bei der Erstreplikation

Fehler bei der ersten Replikation werden häufig von Konnektivitätsproblemen zwischen dem Quellserver und dem Prozessserver oder zwischen dem Prozessserver und Azure verursacht. In den meisten Fällen können Sie diese Probleme beheben, indem Sie die Schritte in den folgenden Abschnitten ausführen.

### <a name="check-the-source-machine"></a>Überprüfen des Quellcomputers

In der folgenden Liste finden Sie mögliche Wege zum Überprüfen des Quellcomputers:

*  Verwenden Sie in der Befehlszeile auf dem Quellserver Telnet, um den Prozessserver über den HTTPS-Port zu pingen, indem Sie den folgenden Befehl ausführen. HTTPS-Port 9443 ist der Standardport, der vom Prozessserver zum Senden und Empfangen von Replikationsdatenverkehr verwendet wird. Sie können diesen Port bei der Registrierung ändern. Der folgende Befehl überprüft auf Probleme mit der Netzwerkkonnektivität sowie Probleme, durch die der Firewallport blockiert wird.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Verwenden Sie Telnet, um die Konnektivität zu testen. Verwenden Sie nicht `ping`. Wenn Telnet nicht installiert ist, führen Sie die unter [Installieren des Telnet-Clients](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) aufgeführten Schritte aus.

   Wenn Telnet eine Verbindung mit dem PS-Port herstellen kann, wird ein leerer Bildschirm angezeigt.

   Wenn Sie keine Verbindung mit dem Prozessserver herstellen können, öffnen Sie den eingehenden Port 9443 auf dem Prozessserver. Beispielsweise müssen Sie möglicherweise den eingehenden Port 9443 auf dem Prozessserver öffnen, wenn Ihr Netzwerk über ein Umkreisnetzwerk verfügt. Überprüfen Sie dann, ob das Problem weiterhin auftritt.

*  Wenn die Telnet-Verbindung hergestellt wurde, der Quellcomputer aber meldet, dass der PS nicht erreichbar ist, öffnen Sie auf dem Quellcomputer den Webbrowser, und überprüfen Sie ob die Adresse „https://<PS_IP>:<PS_Data_Port>/“ erreichbar ist.

    Beim Aufrufen dieser Adresse wird ein HTTPS-Zertifikatfehler erwartet. Wenn Sie den Zertifikatfehler ignorieren und den Vorgang fortsetzen, wird in der Regel der Fehler „400 – Ungültige Anforderung“ angezeigt. Das bedeutet, dass der Server die Anforderung des Browsers nicht verarbeiten kann und dass die HTTPS-Standardverbindung mit dem Server ordnungsgemäß funktioniert.

    War der Vorgang nicht erfolgreich, enthält die Fehlermeldung im Browser ausführlichere Informationen. Beispiel: War die Proxyauthentifizierung fehlerhaft, gibt der Proxyserver „407 – Proxyauthentifizierung erforderlich“ sowie die erforderlichen Aktionen in der Fehlermeldung zurück. 

*  Suchen Sie in den folgenden Protokollen auf dem virtuellen Quellcomputer nach Fehlern im Zusammenhang mit den Netzwerkuploadfehlern:

       C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\svagents*.log 

### <a name="check-the-process-server"></a>Überprüfen des Prozessservers

In der folgenden Liste finden Sie mögliche Wege zum Überprüfen des Prozessservers:

> [!NOTE]
> Für den Prozessserver muss eine statische IPv4-Adresse festgelegt sein, und NAT-IP darf nicht konfiguriert sein.

* **Überprüfen der Konnektivität zwischen den Quellcomputern und dem Prozessserver**
1. Wenn Sie eine Telnet-Verbindung vom Quellcomputer herstellen können, der PS über den Quellcomputer jedoch nicht erreichbar ist, überprüfen Sie die End-to-End-Verbindung zwischen „cxprocessserver“ und dem virtuellen Quellcomputer. Führen Sie dazu das Tool „cxpsclient“ auf dem virtuellen Quellcomputer aus:

       <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>

    Überprüfen Sie die generierten Protokolle auf dem PS in den folgenden Verzeichnissen auf Details zu entsprechenden Fehlern:

       C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err
       and
       C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer
2. Überprüfen Sie die folgenden Protokolle auf dem PS, falls kein Heartbeat vom PS vorhanden ist:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

*  **Überprüfen Sie, ob der Prozessserver aktiv Daten mithilfe von Push an Azure überträgt**.

   1. Öffnen Sie auf dem Prozessserver den Task-Manager (drücken Sie STRG+UMSCHALT+ESC).
   2. Wählen Sie die Registerkarte **Leistung** aus, und wählen Sie dann den Link **Ressourcenmonitor öffnen** aus. 
   3. Wählen Sie auf der Seite **Ressourcenmonitor** die Registerkarte **Netzwerk** aus. Überprüfen Sie unter **Prozess mit Netzwerkaktivität**, ob **cbengine.exe** aktiv große Datenvolumen sendet.

        ![Screenshot der Volumen unter „Prozesse mit Netzwerkaktivität“](./media/vmware-azure-troubleshoot-replication/cbengine.png)

   Wenn „cbengine.exe“ keine großen Datenmengen sendet, führen Sie die Schritte in den folgenden Abschnitten aus.

*  **Überprüfen Sie, ob der Prozessserver eine Verbindung mit dem Azure-Blob-Speicher herstellen kann**.

   Wählen Sie **cbengine.exe** aus. Überprüfen Sie unter **TCP-Verbindungen**, ob eine Verbindung vom Prozessserver ausgehend mit der URL des Azure-Blob-Speichers besteht.

   ![Screenshot der Konnektivität zwischen „cbengine.exe“ und der URL des Azure-Blob-Speichers](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

   Wenn keine Verbindung vom Prozessserver mit der URL des Azure-Blob-Speichers besteht, wählen Sie in der Systemsteuerung **Dienste** aus. Überprüfen Sie, ob die folgenden Dienste ausgeführt werden:

   *  cxprocessserver
   *  InMage Scout VX Agent – Sentinel/Outpost
   *  Microsoft Azure Recovery Services-Agent
   *  Microsoft Azure Site Recovery-Dienst
   *  tmansvc

   Starten Sie jeden der Dienste, der nicht ausgeführt wird, bzw. starten Sie ihn neu. Überprüfen Sie, ob das Problem weiterhin auftritt.

*  **Überprüfen Sie, ob der Prozessserver über Port 443 eine Verbindung mit der öffentlichen Azure-IP-Adresse herstellen kann**.

   Öffnen Sie in „%programfiles%\Microsoft Azure Recovery Services Agent\Temp“ die neueste Datei „CBEngineCurr.errlog“. Suchen Sie in der Datei nach **443** oder nach der Zeichenfolge **connection attempt failed** (Fehler beim Verbindungsversuch).

   ![Screenshot, der die Fehlerprotokolle im Ordner „Temp“ zeigt](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

   Wenn Probleme angezeigt werden, verwenden Sie in der Befehlszeile auf dem Prozessserver Telnet, um Ihre öffentliche Azure-IP-Adresse zu pingen (in der vorangehenden Abbildung ist die IP-Adresse maskiert). Ihre öffentliche Azure-IP-Adresse finden Sie in der Datei „CBEngineCurr.currLog“ mithilfe von Port 443:

   `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

   Wenn Sie keine Verbindung herstellen können, überprüfen Sie, ob das Zugriffsproblem von Firewall- oder Proxyeinstellungen verursacht wird, wie im nächsten Schritt beschrieben.

*  **Überprüfen Sie, ob die IP-Adressenbasierte Firewall auf dem Prozessserver den Zugriff blockiert**.

   Wenn Sie IP-Adressenbasierte Firewallregeln auf dem Server verwenden, laden Sie die vollständige Liste der [IP-Adressbereiche für Microsoft Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) herunter. Fügen Sie die IP-Adressbereiche Ihrer Firewallkonfiguration hinzu, um sicherzustellen, dass die Firewall die Kommunikation in Azure (und mit dem HTTPS-Standardport 443) zulässt. Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Azure-Region „USA, Westen“ (die für die Zugriffsteuerung und Identitätsverwaltung verwendet werden) zu.

*  **Überprüfen Sie, ob eine URL-basierte Firewall auf dem Prozessserver den Zugriff blockiert**.

   Wenn Sie eine URL-basierte Firewallregel auf dem Server verwenden, fügen Sie die in der folgenden Tabelle aufgeführten URLs der Firewallkonfiguration hinzu:

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

*  **Überprüfen Sie, ob Proxyeinstellungen auf dem Prozessserver den Zugriff blockieren**.

   Wenn Sie einen Proxyserver verwenden, stellen Sie sicher, dass der Proxyservername durch den DNS-Server aufgelöst wird. Um den von Ihnen bei der Einrichtung des Konfigurationsservers angegebenen Wert zu überprüfen, wechseln Sie zum Registrierungsschlüssel **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.

   Stellen Sie nun sicher, dass dieselben Einstellungen vom Azure Site Recovery-Agent zum Senden von Daten verwendet werden: 
      
   1. Suchen Sie nach **Microsoft Azure Backup**. 
   2. Öffnen Sie **Microsoft Azure Backup**, und wählen Sie dann **Aktion** > **Eigenschaften ändern** aus. 
   3. Auf der Registerkarte **Proxykonfiguration** sollte die Proxyadresse angezeigt werden. Die Proxyadresse sollte mit der Proxyadresse übereinstimmen, die in den Registrierungseinstellungen angezeigt wird. Wenn dies nicht der Fall ist, ändern Sie diese in dieselbe Adresse.

*  **Überprüfen Sie, ob die Drosselungsbandbreite auf dem Prozessserver eingeschränkt ist**.

   Erhöhen Sie die Bandbreite, und überprüfen Sie dann, ob das Problem weiterhin auftritt.

## <a name="source-machine-isnt-listed-in-the-azure-portal"></a>Quellcomputer wird im Azure-Portal nicht aufgeführt

Wenn Sie versuchen, den Quellcomputer auszuwählen, um Replikation mithilfe von Site Recovery zu aktivieren, steht der Computer möglicherweise aus einem der folgenden Gründe nicht zur Verfügung:

* **Zwei virtuelle Computer mit derselben Instanz-UUID**: Wenn zwei virtuelle Computer im vCenter dieselbe Instanz-UUID haben, wird der erste vom Konfigurationsserver ermittelte virtuelle Computer im Azure-Portal angezeigt. Um dieses Problem zu beheben, stellen Sie sicher, dass keine zwei virtuellen Computer über dieselbe Instanz-UUID verfügen. Dieses Szenario tritt häufig auf, wenn ein virtueller Sicherungscomputer aktiv wird und in unseren Ermittlungsdatensätzen protokolliert wird. Informationen zum Beheben dieses Problems finden Sie unter [Azure Site Recovery VMware-zu-Azure: Bereinigen von doppelten oder veralteten Einträgen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).
* **Falsche vCenter-Benutzeranmeldeinformationen**: Stellen Sie sicher, dass Sie während der Einrichtung des Konfigurationsservers mithilfe der OVF-Vorlage oder des einheitlichen Setups die richtigen vCenter-Anmeldeinformationen hinzugefügt haben. Informationen zum Überprüfen der Anmeldeinformationen, die Sie während der Einrichtung hinzugefügt haben, finden Sie unter [Ändern der Anmeldeinformationen für die automatische Ermittlung](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **Unzureichende vCenter-Berechtigungen**: Wenn die für den Zugriff auf vCenter bereitgestellten Berechtigungen nicht die erforderlichen Berechtigungen umfassen, kann die Ermittlung von virtuellen Computern möglicherweise fehlschlagen. Stellen Sie sicher, dass die unter [Vorbereiten eines Kontos für die automatische Ermittlung](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) beschriebenen Berechtigungen dem vCenter-Benutzerkonto hinzugefügt werden.
* **Azure Site Recovery-Verwaltungsserver**: Wenn der virtuelle Computer als Verwaltungsserver unter mindestens einer Rolle aus Konfigurationsserver, horizontal skalierter Prozessserver oder Masterzielserver verwendet wird, können Sie den virtuellen Computer im Portal nicht auswählen. Verwaltungsserver können nicht repliziert werden.
* **Bereits geschützt/Failover durch Azure Site Recovery-Dienste ausgeführt**: Wenn der virtuelle Computer bereits mithilfe von Site Recovery geschützt wird bzw. ein Failover dafür ausgeführt wurde, steht er nicht mehr zur Auswahl für den Schutz im Portal zur Verfügung. Stellen Sie sicher, dass der gesuchte virtuelle Computer im Portal noch nicht von einem anderen Benutzer oder unter einem anderen Abonnement geschützt wurde.
* **vCenter nicht verbunden**: Überprüfen Sie, ob sich vCenter im verbundenen Zustand befindet. Dazu wechseln Sie zu Recovery Services-Tresor > Site Recovery-Infrastruktur > Konfigurationsserver > Auf den jeweiligen Konfigurationsserver klicken > auf der rechten Seite wird ein Blatt mit Details zu zugeordneten Servern geöffnet. Überprüfen Sie, ob vCenter verbunden ist. Falls es sich im Zustand „Nicht verbunden“ befindet, beheben Sie das Problem, und [aktualisieren Sie den Konfigurationsserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) im Portal. Danach wird der virtuelle Computer im Portal aufgeführt.
* **ESXi ausgeschaltet**: Wenn der ESXi-Host, auf dem sich der virtuelle Computer befindet, ausgeschaltet ist, wird der virtuelle Computer im Azure-Portal nicht aufgeführt und kann nicht ausgewählt werden. Schalten Sie den ESXi-Host ein, und [aktualisieren Sie den Konfigurationsserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) im Portal. Danach wird der virtuelle Computer im Portal aufgeführt.
* **Ausstehender Neustart**: Wenn auf dem virtuellen Computer ein Neustart aussteht, können Sie den Computer im Azure-Portal nicht auswählen. Schließen Sie die ausstehenden Neustartaktivitäten ab, und [aktualisieren Sie den Konfigurationsserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Danach wird der virtuelle Computer im Portal aufgeführt.
* **IP nicht gefunden**: Wenn dem virtuellen Computer keine gültige IP-Adresse zugeordnet ist, können Sie ihn im Azure-Portal nicht auswählen. Weisen Sie dem virtuellen Computer eine gültige IP-Adresse zu, und [aktualisieren Sie den Konfigurationsserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Danach wird der virtuelle Computer im Portal aufgeführt.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Geschützte virtuelle Computer werden im Portal grau dargestellt

Virtuelle Computer, die unter Site Recovery repliziert werden, sind im Azure-Portal nicht verfügbar, wenn doppelte Einträge im System vorhanden sind. Informationen zum Löschen veralteter Einträge sowie zum Beheben des Problems finden Sie unter [Azure Site Recovery VMware-zu-Azure: Gewusst wie: Bereinigen von doppelten oder veralteten Einträgen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Hilfe benötigen, können Sie Ihre Frage im [Azure Site Recovery-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veröffentlichen. Unsere Community ist sehr aktiv, und einer unserer Techniker kann Sie bei Ihrem Problem unterstützen.
