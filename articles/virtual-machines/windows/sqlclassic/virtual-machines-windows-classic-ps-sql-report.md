---
title: Verwenden von PowerShell zum Erstellen eines virtuellen Computers mit einem Berichtsserver im einheitlichen Modus | Microsoft Docs
description: "Dieses Thema beschreibt und führt Sie schrittweise durch die Bereitstellung und Konfiguration von einem Berichtsserver von SQL Server Reporting Services im einheitlichen Modus auf einem virtuellen Azure-Computer. "
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: 5e5c11251cd316e8161dbe362b300be76927ac01
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>Verwenden von PowerShell zum Erstellen einer Azure VM mit einem Berichtsserver im einheitlichen Modus
> [!IMPORTANT] 
> Azure gibt es zwei verschiedene Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Ressourcen-Manager und klassischen](../../../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel umfasst, mit dem klassischen Bereitstellungsmodell. Microsoft empfiehlt, dass die meisten neue Bereitstellungen, die Ressourcen-Manager-Modell verwenden.

Dieses Thema beschreibt und führt Sie schrittweise durch die Bereitstellung und Konfiguration von einem Berichtsserver von SQL Server Reporting Services im einheitlichen Modus auf einem virtuellen Azure-Computer. Die Schritte in diesem Dokument verwenden eine Kombination von manuellen Schritten ausführen, um die virtuelle Maschine und eine Windows PowerShell-Skript zum Konfigurieren von Reporting Services auf dem virtuellen Computer zu erstellen. Das Konfigurationsskript beinhaltet das Öffnen eines Firewallports für HTTP oder HTTPs.

> [!NOTE]
> Wenn Sie nicht benötigen **HTTPS** auf dem Berichtsserver **überspringen Sie Schritt 2**.
> 
> Nach dem Erstellen des virtuellen Computers in Schritt 1, finden Sie im Abschnitt Skript so konfigurieren Sie den Berichtsserver und HTTP verwenden. Nachdem Sie das Skript ausführen wird der Berichtsserver einsatzbereit.

## <a name="prerequisites-and-assumptions"></a>Voraussetzungen und Annahmen
* **Azure-Abonnement**: Überprüfen Sie die Anzahl der Kerne in Ihrem Azure-Abonnement verfügbar. Wenn Sie die empfohlene Größe des virtuellen Computers erstellen **A3**, müssen Sie **4** verfügbare Kerne. Bei Verwendung von VM-Größe **A2**, müssen Sie **2** verfügbare Kerne.
  
  * Zum Überprüfen des kernlimits Ihres Abonnements im klassischen Azure-Portal, klicken Sie auf EINSTELLUNGEN in den linken Bereich, und klicken Sie auf der Nutzung im oberen Menü.
  * Zum Erhöhen des kernkontingents wenden Sie sich an [Azure-Support](https://azure.microsoft.com/support/options/). Informationen des VM-Größe, finden Sie unter [Größe virtueller Computer unter Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* **Windows PowerShell-Skripts**: das Thema wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von Windows PowerShell verfügen. Weitere Informationen zur Verwendung von Windows PowerShell finden Sie in der folgenden:
  
  * [Starten von WindowsPowerShell unter WindowsServer](https://technet.microsoft.com/library/hh847814.aspx)
  * [Erste Schritte mit WindowsPowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Schritt 1: Bereitstellen von virtuellen Azure-Computer
1. Navigieren Sie zum klassischen Azure-Portal.
2. Klicken Sie auf **VMs** im linken Bereich.
   
    ![Microsoft Azure-VMs](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. Klicken Sie auf **Neu**.
   
    ![Schaltfläche "Neu"](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. Klicken Sie auf **aus Galerie**.
   
    ![Neuer virtueller Computer aus Galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. Klicken Sie auf **SQL Server 2014 RTM Standard – Windows Server 2012 R2** , und klicken Sie dann auf den Pfeil, um den Vorgang fortzusetzen.
   
    ![Weiter](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    Wenn Sie die Reporting Services für datengesteuerte Abonnements benötigen, wählen Sie **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Weitere Informationen zu SQL Server-Editionen und Feature-Unterstützung finden Sie unter [von den SQL Server 2012-Editionen unterstützte Funktionen](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).
6. Auf der **Konfiguration des virtuellen Computers** bearbeiten Sie die folgenden Felder:
   
   * Wenn es mehr als eine **VERÖFFENTLICHUNGSDATUM der VERSION**, wählen Sie die neueste Version.
   * **Name des virtuellen Computers**: Name des Computers wird auch auf der nächsten Konfigurationsseite als den DNS des Cloud-Service-Standardnamen verwendet. Der DNS-Name muss im gesamten Azure-Dienst eindeutig sein. Berücksichtigen Sie die Konfiguration des virtuellen Computers mit einem Computernamen, der beschreibt, was für der virtuellen Computer verwendet wird. Z. B. Ssrsnativecloud.
   * **Ebene**: Standard
   * **Größe: A3** ist die empfohlene VM-Größe für SQL Server-arbeitsauslastungen. Wenn Sie ein virtuellen Computer nur als Berichtsserver verwendet wird, ist eine VM-Größe von A2 ausreichend, wenn der Berichtsserver keine großen arbeitsauslastungen auftreten. Virtuellen Computer Informationen zu den Preisen finden Sie unter [Preisangaben zu virtuellen Computern](https://azure.microsoft.com/pricing/details/virtual-machines/).
   * **Neuer Benutzername**: die von Ihnen angegebene Name wird als ein Administrator auf dem virtuellen Computer erstellt.
   * **Neues Kennwort** und **bestätigen**. Dieses Kennwort für das neue Administratorkonto verwendet wird, und es wird empfohlen, dass Sie ein sicheres Kennwort verwenden.
   * Klicken Sie auf **Weiter**. ![Weiter](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
7. Bearbeiten Sie auf der nächsten Seite die folgenden Felder ein:
   
   * **Cloud-Dienst**: Wählen Sie **Erstellen eines neuen Cloud-Diensts**.
   * **Cloud-Dienst-DNS-Namen**: Dies ist der öffentliche DNS-Name des Cloud-Diensts, der dem virtuellen Computer zugeordnet ist. Der Standardname ist der Name, den Sie für den VM-Namen eingegeben haben. Wenn in späteren Schritten dieses Themas Sie ein vertrauenswürdiges SSL-Zertifikat erstellen, und klicken Sie dann der DNS-Namen für den Wert des dient der "**ausgestellt für**" des Zertifikats.
   * **Region/Affinitätsgruppe/virtuelles Netzwerk**: Wählen Sie die Region, die Ihre Endbenutzer am nächsten.
   * **Speicherkonto**: ein automatisch generiertes Speicherkonto verwenden.
   * **Verfügbarkeitsgruppe**: None.
   * **ENDPUNKTE** behalten die **Remotedesktop** und **PowerShell** Endpunkte und fügen Sie entweder einen HTTP- oder HTTPS-Endpunkt, abhängig von Ihrer Umgebung hinzu.
     
     * **HTTP**: die öffentlichen und privaten Standardports sind **80**. Beachten Sie, die ändern, wenn Sie einen anderen privaten Port als 80 verwenden **$HTTPport = 80** im HTTP-Skript.
     * **HTTPS**: die öffentlichen und privaten Standardports sind **443**. Eine bewährte Sicherheitsmethode ist den privaten Port ändern und Konfigurieren der Firewall und Berichtsserver her, um den privaten Port zu verwenden. Weitere Informationen zu Endpunkten finden Sie unter [zum Einrichten der Kommunikation mit einem virtuellen Computer](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Beachten Sie, die, wenn Sie einen anderen Port als 443 verwenden, ändern Sie den Parameter **$HTTPsport = 443** im HTTPS-Skript.
   * Klicken Sie auf Weiter. ![Weiter](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. Behalten Sie auf der letzten Seite des Assistenten, den Standardwert **installieren Sie den VM-Agent** ausgewählten. Die Schritte in diesem Thema führen Sie den VM-Agent nicht nutzen, aber wenn Sie diesen virtuellen Computer behalten möchten, die VM-Agent und Erweiterungen können Sie zur Verbesserung der er CM.  Weitere Informationen zu den VM-Agent, finden Sie unter [VM-Agent und Erweiterungen – Teil 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Keines der standardmäßig installierten Erweiterungen Ad ausgeführt handelt es sich um die "BGINFO"-Erweiterung, die Systeminformationen wie z. B. interne IP-Adresse und der freie Speicherplatz auf dem Desktop des virtuellen Computers angezeigt.
9. Klicken Sie auf "abgeschlossen". ![Okay](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. Die **Status** des virtuellen Computers wird als **gestartet (Bereitstellung)** während des Bereitstellungsvorgangs und wird als **ausführen** Wenn der virtuelle Computer bereitgestellt und sind verwendungsbereit ist.

## <a name="step-2-create-a-server-certificate"></a>Schritt 2: Erstellen eines Serverzertifikats
> [!NOTE]
> Wenn Sie nicht über HTTPS auf dem Berichtsserver benötigen, können Sie **überspringen Sie Schritt 2** und navigieren Sie zum Abschnitt **verwenden Sie Skripts so konfigurieren Sie den Berichtsserver und HTTP-**. Verwenden Sie das HTTP-Skript auf den Berichtsserver schnell konfigurieren und der Berichtsserver verwendet werden soll.

Um die Verwendung von HTTPS auf dem virtuellen Computer benötigen Sie ein vertrauenswürdiges SSL-Zertifikat. Abhängig von Ihrem Szenario können Sie eine der beiden folgenden Methoden verwenden:

* Ein gültiges SSL-Zertifikat von einer Zertifizierungsstelle (Certification Authority, CA) ausgestellt und von Microsoft vertraut wird. Die CA-Stammzertifikate müssen über das Microsoft Root Certificate Program verteilt werden. Weitere Informationen zu diesem Programm finden Sie unter [Windows- und Windows Phone 8 SSL-Programms für Stammzertifikate (Mitglieder-CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) und [Einführung in das Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).
* Ein selbst signiertes Zertifikat. Selbstsignierte Zertifikate werden nicht für produktionsumgebungen empfohlen.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Zur Verwendung eines Zertifikats von einer vertrauenswürdigen Zertifizierungsstelle (CA) erstellt
1. **Ein Serverzertifikat für die Website von einer Zertifizierungsstelle anfordern**. 
   
    Sie können den Assistenten für Webserverzertifikate verwenden, um eine Zertifikatanforderungsdatei (Certreq.txt) zu generieren, die Sie an eine Zertifizierungsstelle senden oder auf eine Anforderung für eine Onlinezertifizierungsstelle generieren. Z. B. Microsoft Certificate Services in Windows Server 2012. Je nach der Kennung Zusicherung, die von Ihr Serverzertifikat angeboten wird ist es mehrere Tage bis Monate für die Zertifizierungsstelle Ihre Anforderung genehmigt und Ihnen eine Zertifikatdatei sendet. 
   
    Weitere Informationen zum Anfordern eines Serverzertifikats finden Sie hier: 
   
   * Verwendung [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
   * Sicherheitstools zum Verwalten von WindowsServer 2012.
     
     [Sicherheitstools zum Verwalten von WindowsServer 2012](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > Die **ausgestellt für** Feld des vertrauenswürdigen SSL-Zertifikats sollten identisch sein der **DNS-NAME für Cloud-Diensts** Sie für den neuen virtuellen Computer verwendet.

2. **Installieren Sie das Serverzertifikat auf dem Webserver**. Der Webserver ist in diesem Fall die virtuellen Computer, der den Berichtsserver hostet, und die Website wird in späteren Schritten erstellt, wenn Sie Reporting Services konfigurieren. Weitere Informationen zum Installieren des Serverzertifikats auf dem Webserver mithilfe der Zertifikat-MMC-Snap-in finden Sie unter [Installieren eines Serverzertifikats](https://technet.microsoft.com/library/cc740068).
   
    Wenn Sie das in diesem Thema enthaltene Skript, die zum Konfigurieren des Berichtsservers, den Wert der Zertifikate verwenden möchten **Fingerabdruck** als Parameter des Skripts erforderlich ist. Finden Sie im nächsten Abschnitt Weitere Informationen zum Abrufen des Fingerabdrucks des Zertifikats an.
3. Weisen Sie das Serverzertifikat mit dem Berichtsserver her. Die Zuweisung wird im nächsten Abschnitt abgeschlossen, wenn Sie den Berichtsserver konfigurieren.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Der virtuelle Computer selbstsigniertes Zertifikat verwenden
Ein selbst signiertes Zertifikat wurde auf dem virtuellen Computer erstellt, wenn der virtuelle Computer bereitgestellt wurde. Das Zertifikat weist den gleichen Namen wie das VM-DNS-Namen an. Zum Vermeiden von Zertifikatfehlern ist es erforderlich, dass das Zertifikat auf dem virtuellen Computer selbst und von allen Benutzern der Website vertrauenswürdig ist.

1. Fügen Sie das Zertifikat zum Vertrauen der Stammzertifizierungsstelle des Zertifikats auf dem lokalen virtuellen Computer, die **Trusted Root Certification Authorities**. Im folgenden wird ein Überblick über die erforderlichen Schritte. Detaillierte Schritte zur Verwendung von der Zertifizierungsstelle vertrauen, finden Sie unter [Installieren eines Serverzertifikats](https://technet.microsoft.com/library/cc740068).
   
   1. Vom klassischen Azure-Portal wählen Sie den virtuellen Computer aus, und klicken Sie auf Verbinden. Abhängig von Ihrer Browserkonfiguration werden Sie möglicherweise aufgefordert, eine RDP-Datei zum Herstellen einer Verbindung mit dem virtuellen Computer zu speichern.
      
       ![Herstellen einer Verbindung mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Verwenden Sie die VM-Benutzername, Benutzernamen und Kennwort an, das Sie bei der Erstellung des virtuellen Computers konfiguriert. 
      
       In der folgenden Abbildung ist der Name des VM **Ssrsnativecloud** und der Benutzername ist **Testuser**.
      
       ![Anmeldung enthält den Namen des virtuellen Computers](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. "Mmc.exe" ausführen. Weitere Informationen finden Sie unter [Vorgehensweise: Anzeigen von Zertifikaten mit dem MMC-Snap-in](https://msdn.microsoft.com/library/ms788967.aspx).
   3. In der Konsolenanwendung **Datei** im Menü hinzufügen die **Zertifikate** -Snap-in auf **Computerkonto** ein, und klicken Sie dann auf **Weiter**.
   4. Wählen Sie **Sicherheitszertifikate** verwalten, und klicken Sie dann auf **Fertig stellen**.
   5. Klicken Sie auf **Ok** und erweitern Sie dann die **Zertifikate - persönliche** Knoten und klicken Sie dann auf **Zertifikate**. Das Zertifikat wird nach dem DNS-Namen des virtuellen Computers mit dem Namen und endet mit **cloudapp.net**. Mit der rechten Maustaste des Zertifikatnamen, und klicken Sie auf **Kopie**.
   6. Erweitern Sie die **Trusted Root Certification Authorities** Knoten, und klicken Sie dann mit der rechten Maustaste **Zertifikate** , und klicken Sie dann auf **einfügen**.
   7. Um zu überprüfen, klicken Sie mit der Doppelklicken auf den Zertifikatsnamen unter **Trusted Root Certification Authorities** und stellen Sie sicher, dass keine Fehler vorliegen, und Sie Ihre Zertifikate sehen. Wenn Sie in diesem Thema enthaltene HTTPS-Skript zum Konfigurieren des Berichtsservers, den Wert der Zertifikate verwenden möchten **Fingerabdruck** als Parameter des Skripts erforderlich ist. **Zum Abrufen des Fingerabdruckwerts**, führen Sie die folgenden Schritte. Es gibt auch eine PowerShell-Beispiel zum Abrufen des Fingerabdrucks in Abschnitt [Skript so konfigurieren Sie den Berichtsserver und HTTPS verwenden](#use-script-to-configure-the-report-server-and-HTTPS).
      
      1. Doppelklicken Sie auf den Namen des Zertifikats, z. B. ssrsnativecloud.cloudapp.net.
      2. Klicken Sie auf die Registerkarte **Details** .
      3. Klicken Sie auf **Fingerabdruck**. Der Wert des Fingerabdrucks wird im Detailfeld, z. B. a6 angezeigt 08 3c df f9 0 b f7 e3 7c 25 Ed a4 Ed 7e Ac 91 9c 2c fb 2f.
      4. Kopieren Sie den Fingerabdruck und speichern Sie den Wert für die spätere hinzu oder bearbeiten Sie das Skript jetzt.
      5. (*) Entfernen Sie bevor Sie das Skript ausführen die Leerzeichen zwischen den Wertpaaren. Beispielsweise würde die zuvor notierte Fingerabdruck jetzt a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f sein.
      6. Weisen Sie das Serverzertifikat mit dem Berichtsserver her. Die Zuweisung wird im nächsten Abschnitt abgeschlossen, wenn Sie den Berichtsserver konfigurieren.

Wenn Sie ein selbstsigniertes SSL-Zertifikat verwenden, entspricht der Name für das Zertifikat bereits den Hostnamen des virtuellen Computers. Deshalb wird der DNS des Computers bereits global registriert und von jedem beliebigen Client zugegriffen werden kann.

## <a name="step-3-configure-the-report-server"></a>Schritt 3: Konfigurieren des Berichtsservers
Dieser Abschnitt führt Sie durch die Konfiguration des virtuellen Computers als einen Berichtsserver für Reporting Services im einheitlichen Modus. Sie können eine der folgenden Methoden verwenden, auf um den Berichtsserver zu konfigurieren:

* Verwenden Sie das Skript zum Konfigurieren des Berichtsservers
* Verwenden Sie Configuration Manager, um den Berichtsserver konfigurieren.

Ausführliche Schritte finden Sie im Abschnitt [Herstellen einer Verbindung mit dem virtuellen Computer, und starten Sie den Konfigurations-Manager für Reporting Services](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Hinweis zur Authentifizierung:** Windows-Authentifizierung ist die empfohlene Authentifizierungsmethode und die Standardauthentifizierung für Reporting Services. Nur Benutzer, die auf dem virtuellen Computer konfiguriert sind, können Zugriff auf Reporting Services und Reporting Services-Rollen zugewiesen.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Verwenden Sie so konfigurieren Sie den Berichtsserver und HTTP-Skript
Um das Windows PowerShell-Skript verwenden, um den Berichtsserver konfigurieren, müssen führen Sie die folgenden Schritte aus. Die Konfiguration umfasst HTTP, nicht HTTPS:

1. Vom klassischen Azure-Portal wählen Sie den virtuellen Computer aus, und klicken Sie auf Verbinden. Abhängig von Ihrer Browserkonfiguration werden Sie möglicherweise aufgefordert, eine RDP-Datei zum Herstellen einer Verbindung mit dem virtuellen Computer zu speichern.
   
    ![Herstellen einer Verbindung mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Verwenden Sie die VM-Benutzername, Benutzernamen und Kennwort an, das Sie bei der Erstellung des virtuellen Computers konfiguriert. 
   
    In der folgenden Abbildung ist der Name des VM **Ssrsnativecloud** und der Benutzername ist **Testuser**.
   
    ![Anmeldung enthält den Namen des virtuellen Computers](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Öffnen Sie auf dem virtuellen Computer **Windows PowerShell ISE** mit Administratorrechten aus. Der PowerShell ISE ist unter WindowsServer 2012 standardmäßig installiert. Es wird empfohlen, ISE anstelle einer Windows PowerShell-Standardfenster zu verwenden, damit Sie das Skript in die ISE einfügen, ändern Sie das Skript und führen Sie das Skript.
3. Klicken Sie in Windows PowerShell ISE auf das **Ansicht** Menü, und klicken Sie dann auf **Skriptbereich anzeigen**.
4. Kopieren Sie das folgende Skript aus, und fügen Sie das Skript in den Windows PowerShell ISE-Skriptbereich.
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
   
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. Wenn Sie den virtuellen Computer mit einem HTTP-Port als 80 erstellt haben, ändern Sie den Parameter $HTTPport = 80.
6. Das Skript wird zurzeit für Reporting Services konfiguriert. Wenn Sie das Skript für Reporting Services ausführen möchten, ändern Sie den versionsanteil des Pfads für den Namespace "v11" für die Get-WmiObject-Anweisung.
7. Führen Sie das Skript ein.

**Überprüfung**: um sicherzustellen, dass die grundlegenden berichtsserverfunktionen ordnungsgemäß funktionieren, ist, finden Sie unter der [überprüfen Sie die Konfiguration](#verify-the-configuration) weiter unten in diesem Thema.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Verwenden Sie Skripts so konfigurieren Sie den Berichtsserver und HTTPS
Um Windows PowerShell verwenden, um den Berichtsserver konfigurieren, müssen führen Sie die folgenden Schritte aus. Die Konfiguration umfasst HTTPS, nicht HTTP.

1. Vom klassischen Azure-Portal wählen Sie den virtuellen Computer aus, und klicken Sie auf Verbinden. Abhängig von Ihrer Browserkonfiguration werden Sie möglicherweise aufgefordert, eine RDP-Datei zum Herstellen einer Verbindung mit dem virtuellen Computer zu speichern.
   
    ![Herstellen einer Verbindung mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Verwenden Sie die VM-Benutzername, Benutzernamen und Kennwort an, das Sie bei der Erstellung des virtuellen Computers konfiguriert. 
   
    In der folgenden Abbildung ist der Name des VM **Ssrsnativecloud** und der Benutzername ist **Testuser**.
   
    ![Anmeldung enthält den Namen des virtuellen Computers](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Öffnen Sie auf dem virtuellen Computer **Windows PowerShell ISE** mit Administratorrechten aus. Der PowerShell ISE ist unter WindowsServer 2012 standardmäßig installiert. Es wird empfohlen, ISE anstelle einer Windows PowerShell-Standardfenster zu verwenden, damit Sie das Skript in die ISE einfügen, ändern Sie das Skript und führen Sie das Skript.
3. Zum Ausführen von Skripts zu aktivieren, führen Sie den folgenden Windows PowerShell-Befehl ein:
   
        Set-ExecutionPolicy RemoteSigned
   
    Anschließend können Sie Folgendes ein, um zu überprüfen, ob die Richtlinie ausführen:
   
        Get-ExecutionPolicy
4. In **Windows PowerShell ISE**, klicken Sie auf die **Ansicht** Menü, und klicken Sie dann auf **Skriptbereich anzeigen**.
5. Kopieren Sie das folgende Skript aus, und fügen Sie ihn in den Windows PowerShell ISE-Skriptbereich.
   
        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
   
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. Ändern der **$certificatehash** Parameters im Skript:
   
   * Dies ist eine **erforderlichen** Parameter. Wenn Sie nicht den Zertifikatwert in den vorherigen Schritten gespeichert haben, verwenden Sie eine der folgenden Methoden den Zertifikathashwert aus dem Zertifikatfingerabdruck kopieren.:
     
       Öffnen Sie auf dem virtuellen Computer Windows PowerShell ISE, und führen Sie den folgenden Befehl:
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       Die Ausgabe sieht etwa wie folgt. Wenn das Skript eine leere Zeile zurückgibt, verfügt nicht über z. B. konfigurierten Zertifikats finden Sie im Abschnitt [VMs selbstsigniertes Zertifikat verwenden](#to-use-the-virtual-machines-self-signed-certificate).
     
     ODER
   * Auf dem virtuellen Computer führen Sie mmc.exe, und fügen Sie dann die **Zertifikate** -Snap-in.
   * Klicken Sie unter der **vertrauenswürdige Stammzertifizierungsstellen** Knoten doppelklicken Sie auf den Namen Ihres Zertifikats. Wenn Sie das selbstsignierte Zertifikat des virtuellen Computers verwenden, wird das Zertifikat wird nach dem DNS-Namen des virtuellen Computers mit dem Namen und endet mit **cloudapp.net**.
   * Klicken Sie auf die Registerkarte **Details** .
   * Klicken Sie auf **Fingerabdruck**. Der Wert des Fingerabdrucks wird im Detailfeld, z. B. af 11 60 b6 4 b 28 8 d 89 0a 82 12 ff 6 a9 c3 66 4f 31 90 48
   * **Vor dem Ausführen des Skripts**, entfernen Sie die Leerzeichen zwischen den Wertpaaren. Z. B. af1160b64b288d890a8212ff6ba9c3664f319048
7. Ändern der **$httpsport** Parameter: 
   
   * Wenn Sie Port 443 für HTTPS-Endpunkt verwendet, müssen Sie nicht diesen Parameter im Skript zu aktualisieren. Verwenden Sie andernfalls den Portwert, den Sie ausgewählt werden, wenn Sie den privaten HTTPS-Endpunkt auf dem virtuellen Computer konfiguriert.
8. Ändern der **$DNSName** Parameter: 
   
   * Das Skript ist ein Platzhalter-Zertifikat $DNSName konfiguriert = "+". Stimmen Sie keine möchten für eine Platzhalterzertifikat-Bindung konfigurieren, kommentieren Sie $DNSName ="+"und aktivieren die folgende Zeile, die vollständige $DNSNAme-Verweis, ## $DNSName="$server.cloudapp.net".
     
       Ändern Sie den $DNSName-Wert, wenn Sie nicht die virtuellen Computer DNS-Name für Reporting Services verwenden möchten. Wenn Sie den Parameter verwenden, wird das Zertifikat muss außerdem verwenden Sie diesen Namen, und der Name Global bei einem DNS-Server registrieren.
9. Das Skript wird zurzeit für Reporting Services konfiguriert. Wenn Sie das Skript für Reporting Services ausführen möchten, ändern Sie den versionsanteil des Pfads für den Namespace "v11" für die Get-WmiObject-Anweisung.
10. Führen Sie das Skript ein.

**Überprüfung**: um sicherzustellen, dass die grundlegenden berichtsserverfunktionen ordnungsgemäß funktionieren, ist, finden Sie unter der [überprüfen Sie die Konfiguration](#verify-the-connection) weiter unten in diesem Thema. Um zu überprüfen, ob das Zertifikat Bindung öffnen eine Eingabeaufforderung mit Administratorrechten, und führen Sie den folgenden Befehl:

    netsh http show sslcert

Das Ergebnis enthält Folgendes:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Verwenden des Konfigurations-Manager zum Konfigurieren des Berichtsservers
Wenn Sie nicht die PowerShell-Skript zum Konfigurieren des Berichtsservers ausführen möchten, führen Sie die Schritte in diesem Abschnitt des Konfigurations-Managers für Reporting Services im einheitlichen Modus zu verwenden, um den Berichtsserver konfigurieren.

1. Vom klassischen Azure-Portal wählen Sie den virtuellen Computer aus, und klicken Sie auf Verbinden. Verwenden Sie den Benutzernamen und das Kennwort, die Sie bei der Erstellung des virtuellen Computers konfiguriert.
   
    ![Herstellen einer Verbindung mit virtuellen Azure-Computer](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. Führen Sie Windows Update aus, und Installieren von Updates für den virtuellen Computer. Wenn ein Neustart des virtuellen Computers erforderlich ist, den virtuellen Computer neu, und das erneute Verbinden mit dem virtuellen Computer vom klassischen Azure-Portal.
3. Geben Sie im Menü Start auf dem virtuellen Computer **Reporting Services** , und öffnen Sie **Konfigurations-Manager für Reporting Services**.
4. Lassen Sie die Standardwerte für **Servernamen** und **Berichtsserverinstanz**. Klicken Sie auf **Verbinden**.
5. Klicken Sie im linken Bereich auf **Webdienst-URL**.
6. Standardmäßig ist die RS für HTTP-Port 80 mit IP-Adresse "Alle zugewiesenen" konfiguriert. So fügen Sie HTTPS hinzu:
   
   1. In **SSL-Zertifikat**: Wählen Sie das Zertifikat, das Sie verwenden z. B. [VM-Name, möchten]. cloudapp.net. Wenn keine Zertifikate aufgelistet sind, finden Sie im Abschnitt **Schritt2: Erstellen eines Serverzertifikats** Informationen zum Installieren und das Zertifikat auf dem virtuellen Computer als vertrauenswürdig ansehen.
   2. Klicken Sie unter **SSL-Port**: Wählen Sie 443. Wenn Sie den privaten HTTPS-Endpunkt auf dem virtuellen Computer mit einem anderen privaten Port konfiguriert haben, verwenden Sie diesen Wert hier an.
   3. Klicken Sie auf **übernehmen** und warten Sie, bis der Vorgang abgeschlossen.
7. Klicken Sie im linken Bereich auf **Datenbank**.
   
   1. Klicken Sie auf **ändern Databas**e.
   2. Klicken Sie auf **erstellen eine neuen Berichtsserver-Datenbank** , und klicken Sie dann auf **Weiter**.
   3. Übernehmen Sie den Standardport **Servernamen**: als Namen der VM, und übernehmen Sie den Standardport **Authentifizierungstyp** als **Aktueller Benutzer** – **integrierte Sicherheit von**. Klicken Sie auf **Weiter**.
   4. Übernehmen Sie den Standardport **Datenbankname** als **ReportServer** , und klicken Sie auf **Weiter**.
   5. Übernehmen Sie den Standardport **Authentifizierungstyp** als **Dienstanmeldeinformationen** , und klicken Sie auf **Weiter**.
   6. Klicken Sie auf **Weiter** auf die **Zusammenfassung** Seite.
   7. Wenn die Konfiguration abgeschlossen ist, klicken Sie auf **Fertig stellen**.
8. Klicken Sie im linken Bereich auf **Report Manager URL**. Übernehmen Sie den Standardport **virtuelles Verzeichnis** als **Berichte** , und klicken Sie auf **übernehmen**.
9. Klicken Sie auf **beenden** der Reporting Services-Konfigurations-Manager zu schließen.

## <a name="step-4-open-windows-firewall-port"></a>Schritt 4: Öffnen eines Windows-Firewall-Ports
> [!NOTE]
> Wenn Sie eines der Skripts zum Konfigurieren des Berichtsservers verwendet, können Sie diesen Abschnitt überspringen. Das Skript umfasst einen Schritt zum Öffnen des Firewallports. Der Standardwert war Port 80 für HTTP und Port 443 für HTTPS.
> 
> 

Um Berichts-Manager oder dem Berichtsserver auf dem virtuellen Computer herstellen, wird ein TCP-Endpunkt auf dem virtuellen Computer erforderlich. Es ist erforderlich, um den gleichen Port in der Firewall des virtuellen Computers zu öffnen. Der Endpunkt wurde erstellt, wenn der virtuelle Computer bereitgestellt wurde.

Dieser Abschnitt enthält grundlegenden Informationen zum Öffnen des Firewallports. Weitere Informationen finden Sie unter [Konfigurieren einer Firewall für den Berichtsserverzugriff](https://technet.microsoft.com/library/bb934283.aspx)

> [!NOTE]
> Wenn Sie das Skript verwendet, um den Berichtsserver konfigurieren, können Sie diesen Abschnitt überspringen. Das Skript umfasst einen Schritt zum Öffnen des Firewallports.
> 
> 

Wenn Sie einen privaten Port als 443 für HTTPS konfiguriert haben, ändern Sie das folgende Skript entsprechend. So öffnen Port **443** führen Sie auf der Windows-Firewall Folgendes aus:

1. Öffnen Sie ein Windows PowerShell-Fenster mit Administratorrechten aus.
2. Wenn Sie einen anderen Port als 443 verwendet, wenn Sie den HTTPS-Endpunkt auf dem virtuellen Computer konfiguriert ist, aktualisieren Sie den Port in den folgenden Befehl ein, und führen Sie den Befehl:
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. Wenn der Befehl abgeschlossen ist, **Ok** an der Eingabeaufforderung angezeigt wird.

Um sicherzustellen, dass der Port geöffnet ist, öffnen Sie ein Windows PowerShell-Fenster, und führen Sie den folgenden Befehl aus:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Überprüfen der Konfiguration
Um sicherzustellen, dass die grundlegenden berichtsserverfunktionen jetzt ordnungsgemäß funktionieren, öffnen Sie den Browser mit Administratorprivilegien, und suchen Sie dann den folgenden Bericht Berichts-Manager-URLS:

* Navigieren Sie auf dem virtuellen Computer auf die Berichtsserver-URL:
  
        http://localhost/reportserver
* Navigieren Sie auf dem virtuellen Computer auf die URL des Berichts-Managers:
  
        http://localhost/Reports
* Navigieren Sie von Ihrem lokalen Computer zu der **remote** Berichts-Manager auf dem virtuellen Computer. Aktualisieren Sie den DNS-Namen im folgenden Beispiel nach Bedarf. Aufforderung zur Kennworteingabe verwenden Sie die Administratoranmeldeinformationen, die Sie erstellt, wenn der virtuelle Computer bereitgestellt wurde. Der Benutzername ist in der [Domain]\[Benutzername] Format, wobei die Domäne der VM-Computername, z. B. Ssrsnativecloud\testuser ist. Wenn Sie nicht auf HTTP verwenden,**S**, entfernen Sie die **s** in der URL. Finden Sie im nächsten Abschnitt Informationen zum Erstellen zusätzlicher Benutzer auf dem virtuellen Computer an.
  
        https://ssrsnativecloud.cloudapp.net/Reports
* Navigieren Sie von Ihrem lokalen Computer an den remote-Berichtsserver-URL. Aktualisieren Sie den DNS-Namen im folgenden Beispiel nach Bedarf. Wenn Sie HTTPS nicht verwenden, entfernen Sie die s in der URL.
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Erstellen von Benutzern und Zuweisen von Rollen
Eine häufig anfallende Verwaltungsaufgabe werden nach dem Konfigurieren und Überprüfen des Berichtsservers, aus einem oder mehreren Benutzern zu erstellen und Zuweisen von Benutzern zu Reporting Services-Rollen. Weitere Informationen finden Sie unter:

* [Erstellen eines lokalen Benutzerkontos](https://technet.microsoft.com/library/cc770642.aspx)
* [Gewähren von Benutzerzugriff auf einem Berichtsserver (Berichts-Manager)](https://msdn.microsoft.com/library/ms156034.aspx))
* [Erstellen und Verwalten von Rollenzuweisungen](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Zum Erstellen und Veröffentlichen von Berichten auf virtuellen Azure-Computer
Die folgende Tabelle fasst einige der Optionen für die Veröffentlichung von vorhandener Berichten aus einem lokalen Computer mit dem Berichtsserver her, die auf Microsoft Azure Virtual Machines gehostet verfügbar:

* **RS.exe-Skript**: verwenden RS.exe-Skript, um Berichtselemente aus und vorhandenen Berichtsserver auf Ihre Microsoft Azure Virtual Machines kopieren. Weitere Informationen finden Sie im Abschnitt "Einheitlichen Modus zu einheitlichem Modus – Microsoft Azure Virtual Machines" in [Sample Reporting Services rs.exe Script to Migrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).
* **Berichts-Generator**: der virtuelle Computer umfasst, klicken Sie auf-einmal Version von Microsoft SQL Server-Berichts-Generator. So starten Sie Berichts-Generator das erste Mal auf der virtuellen Maschine
  
  1. Starten Sie den Browser mit Administratorprivilegien an.
  2. Navigieren Sie zum Berichts-Manager auf dem virtuellen Computer, und klicken Sie auf **Berichts-Generator** im Menüband.
     
     Weitere Informationen finden Sie unter [installieren, deinstallieren und unterstützen von Berichts-Generator](https://technet.microsoft.com/library/dd207038.aspx).
* **SQL Server Datatools: VM**: Wenn Sie den virtuellen Computer erstellt, mit SQL Server 2012, SQL Server Data Tools auf dem virtuellen Computer installiert ist und dienen zum Erstellen **Berichtsserverprojekte** und Berichte auf dem virtuellen Computer. SQL Server Data Tools, um die Berichte auf dem Berichtsserver auf dem virtuellen Computer zu veröffentlichen.
  
    Wenn Sie den virtuellen Computer mit SQLServer 2014 erstellt haben, können Sie SQL Server Data Tools – BI für visual Studio installieren. Weitere Informationen finden Sie unter:
  
  * [Microsoft SQL Server Data Tools - Business Intelligence für Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)
  * [Microsoft SQL Server Data Tools - Business Intelligence für Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)
  * [SQL Server Datatools und SQL Server Business Intelligence (SSDT-BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* **SQL Server Datatools: Remote**: Erstellen Sie auf dem lokalen Computer ein Reporting Services-Projekt in SQL Server Data Tools, die Reporting Services-Berichte enthält. Konfigurieren Sie das Projekt für die Webdienst-URL die Verbindung an.
  
    ![SSDT-Projekteigenschaften für SSRS-Projekt](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* **Verwenden Sie Skript**: Skript verwenden, um berichtsserverinhalte zu kopieren. Weitere Informationen finden Sie unter [Sample Reporting Services rs.exe Script to Migrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Kosten zu minimieren, wenn Sie den virtuellen Computer nicht verwenden
> [!NOTE]
> Um die Gebühren für Ihr Azure-virtuellen Computer nicht in Gebrauch zu minimieren, sollten Sie den virtuellen Computer vom klassischen Azure-Portal Herunterfahren. Wenn Sie die Windows-Energieoptionen auf einem virtuellen Computer Herunterfahren des virtuellen Computers verwenden, werden weiterhin die gleiche Menge für den virtuellen Computer Gebühren an. Zum Senken der Gebühren müssen Sie den virtuellen Computer im klassischen Azure-Portal zu schließen. Wenn Sie den virtuellen Computer nicht mehr benötigen, denken Sie daran, löschen Sie den virtuellen Computer und die zugehörigen VHD-Dateien, um speichergebühren zu vermeiden. Weitere Informationen finden Sie in den FAQ-Abschnitt auf [Preisangaben zu virtuellen Computern](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Weitere Informationen
### <a name="resources"></a>Ressourcen
* Vergleichbaren im Zusammenhang mit einer Einzelserver-Bereitstellung von SQL Server Business Intelligence und SharePoint 2013, finden Sie unter [Verwenden von Windows PowerShell zum Erstellen eines Azure-Computers mit SQL Server BI und SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).
* Ähnliche Inhalte zu einer multiserverbereitstellung der SQL Server Business Intelligence und SharePoint 2013, finden Sie unter [Bereitstellen von SQL Server Business Intelligence auf Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).
* Allgemeine Informationen im Zusammenhang mit Bereitstellungen von SQL Server Business Intelligence auf Azure Virtual Machines finden Sie unter [SQL Server Business Intelligence auf Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).
* Weitere Informationen zu den Kosten der Azure-Nutzung finden Sie unter der Registerkarte virtuelle Computer der [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Community-Inhalt
* Schritt-für-Schritt-Anleitungen zum Erstellen eines einheitlicher Reporting Services-Modus-Berichtsservers, ohne Verwendung eines Skripts finden Sie unter [Hosting SQL Reporting-Dienst auf Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Links zu anderen Ressourcen für SQL Server auf virtuellen Azure-Computern
[SQLServer auf Azure Virtual Machines (Übersicht)](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

