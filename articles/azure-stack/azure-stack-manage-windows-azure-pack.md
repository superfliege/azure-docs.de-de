---
title: Verwalten von virtuellen Windows Azure Pack-Computern in Azure Stack | Microsoft-Dokumentation
description: "Erfahren Sie, wie Windows Azure Pack-VMs (WAP) über das Benutzerportal in Azure Stack verwaltet werden."
services: azure-stack
documentationcenter: 
author: walterov
manager: byronr
editor: 
ms.assetid: 213c2792-d404-4b44-8340-235adf3f8f0b
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: walterov
ms.openlocfilehash: b07a18055d149e20cd605a892063eccecf3df8a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-windows-azure-pack-virtual-machines-from-azure-stack"></a>Verwalten von virtuellen Windows Azure Pack-Computern in Azure Stack

*Gilt für: Azure Stack Development Kit*

In Azure Stack Development Kit können Sie über das Azure Stack-Benutzerportal den Zugriff auf virtuelle Mandantencomputer aktivieren, auf denen Windows Azure Pack ausgeführt wird. Benutzer können mithilfe des Azure Stack-Portals die vorhandenen virtuellen IaaS-Computer und virtuellen IaaS-Netzwerke verwalten. Diese Ressourcen werden durch die zugrunde liegenden Service Provider Foundation-Komponenten (SPF) und Virtual Machine Manager-Komponenten (VMM) in Windows Azure Pack zur Verfügung gestellt. Genauer gesagt können Benutzer folgende Aktionen ausführen:

* Durchsuchen von Ressourcen
* Überprüfen der Konfigurationswerte
* Beenden oder Starten eines virtuellen Computers
* Herstellen einer Verbindung mit einem virtuellen Computer über das Remotedesktopprotokoll (RDP)
* Erstellen und Verwalten von Prüfpunkten
* Löschen von virtuellen Computern und virtuellen Netzwerken

Diese Funktionalität wird vom Windows Azure Pack-Connector für Azure Stack (Vorschau) bereitgestellt. In diesem Artikel wird erläutert, wie der Connector für eine Azure Stack-Einzelknotenumgebung konfiguriert wird.

Beachten Sie bei dieser Vorschauversion des Connectors folgende Aspekte:
* Verwenden Sie den Windows Azure Pack-Connector nur in Testumgebungen (für Azure Stack sowie Windows Azure Pack) und nicht in Produktionsbereitstellungen.
* Sie müssen über Windows Azure Pack Update Rollup (UR) 9.1 oder höher und System Center SPF und VMM UR 9 oder höher verfügen.
* Bei den VMM- und SPF-Komponenten kann es sich entweder um System Center 2012 R2 oder System Center 2016 handeln.
* Sie müssen sowohl in Azure Stack als auch in Windows Azure Pack Konfigurationsschritte durchführen.
* Die Anweisungen gelten für Umgebungen außer CPS-Umgebungen (Cloud Platform System).
* Bekannte Probleme werden unter [Problembehandlung bei Microsoft Azure Stack](azure-stack-troubleshooting.md) erläutert.


## <a name="architecture"></a>Architektur
Die folgende Abbildung zeigt die Hauptkomponenten des Windows Azure Pack-Connectors:

![Komponenten des Windows Azure Pack-Connectors](media/azure-stack-manage-wap/image1.png)

Beachten Sie folgende Details:
* Das Azure Stack-Benutzerportal greift über beide Clouds (Azure Stack und Windows Azure Pack) auf die Ressourceninformationen zu.
* Der Benutzer muss ein Konto besitzen, das in beiden Umgebungen gültig ist.
* Das Azure Stack-Benutzerportal muss über Netzwerkzugriff auf die in Windows Azure Pack ausgeführten Komponenten verfügen.
* Im Bereich **WAP** der Abbildung können Sie die neuen Windows Azure Pack-Connectormodule (WAP-Erweiterung und Connector) und die vorhandene Windows Azure Pack-Mandanten-API mit SPF- und VMM-Komponenten sehen.


## <a name="identity-management"></a>Identitätsverwaltung
Die Windows Azure Pack-Mandanten-API muss dem Azure Stack-Sicherheitstokendienst (Security Token Service, STS) vertrauen.

Wenn ein Benutzer eine Aktion über das Azure Stack-Portal für Windows Azure Pack-Ressourcen durchführt, verwendet das Portal die Windows Azure Pack-Mandanten-API. Aus diesem Grund muss das bereitgestellte Benutzerauthentifizierungstoken von einem vertrauenswürdigen STS stammen. Sehen Sie sich die folgende Abbildung an:

![Abbildung zur Authentifizierung des Windows Azure Pack-Connectors](media/azure-stack-manage-wap/image2.png)

In der Development Kit-Umgebung weisen Windows Azure Pack und Azure Stack unabhängige Identitätsanbieter auf. Benutzer, die auf beide Umgebungen über das Azure Stack-Portal zugreifen, müssen bei beiden Identitätsanbietern denselben Benutzerprinzipalnamen (User Principal Name, UPN) verwenden. Das Konto *azurestackadmin@azurestack.local* sollte beispielsweise auch im STS für Windows Azure Pack vorhanden sein. Wenn die Konfiguration von AD FS keine ausgehenden Vertrauensstellungen unterstützt, müssen Sie Vertrauensstellung zwischen den Windows Azure Pack-Komponenten (Mandanten-API) und der Azure Stack-Instanz von AD FS herstellen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="download-the-windows-azure-pack-connector"></a>Herunterladen des Windows Azure Pack-Connectors
Laden Sie im [Microsoft Download Center](https://aka.ms/wapconnectorazurestackdlc) die EXE-Datei herunter, und extrahieren Sie sie auf dem lokalen Computer. Später kopieren Sie den Inhalt auf einen Computer, der auf Ihre Windows Azure Pack-Umgebung zugreifen kann.

### <a name="deployment-option-requirement"></a>Anforderung für die Bereitstellungsoption
Für die Integration in Windows Azure Pack können Sie mithilfe der AD FS- oder Azure Active Directory-Option Azure Stack bereitstellen.

### <a name="connectivity-requirements"></a>Konnektivitätsanforderungen
1. Stellen Sie sicher, dass Sie auf dem Computer, mit dem Sie auf das Azure Stack-Benutzerportal zugreifen, durch den Webbrowser auf das Windows Azure Pack-Mandantenportal zugreifen können.
2. Der virtuelle Computer „AzS-WASP01“ in Azure Stack muss eine Verbindung mit dem Computer herstellen können, auf dem das Windows Azure Pack-Mandantenportal ausgeführt wird. Überprüfen Sie die Netzwerkkonnektivität mithilfe der Datei „Ping.exe“. 
3.  Sie müssen über gültige Zertifikate für die neuen Connectordienste verfügen. Diese SSL-Zertifikate müssen von einer vertrauenswürdigen Zertifizierungsstelle (ZS) ausgestellt werden. Sie können keine selbstsignierten Zertifikate verwenden. Die SSL-Zertifikate müssen von Azure Stack (insbesondere der VM „AzS-WASP01“) und anderen Computern, die der Mandant möglicherweise für den Zugriff auf das Azure Stack-Benutzerportal verwendet, als vertrauenswürdig eingestuft werden.
 
    >[!NOTE]
    Da auf der VM „AzS-WASP01“ Windows Server mit der Server Core-Installationsoption ausgeführt wird, können Sie das Zertifikat mithilfe eines Befehlszeilentools wie „Certutil.ext“ importieren. Beispielsweise können Sie die CER-Datei in das Verzeichnis „c:\temp“ auf der VM „AzS-WASP01“ kopieren und dann den Befehl **certutil -addstore "CA" "c:\temp\certname.cer"** ausführen.

4.  Um eine RDP-Konnektivität mit virtuellen Computern des Windows Azure Pack-Mandanten über das Azure Stack-Portal herzustellen, muss die Windows Azure Pack-Umgebung Remotedesktopdatenverkehr an die Mandanten-VMs zulassen.
5.  Für die Konnektivität zwischen virtuellen Computern des Azure Stack-Mandanten und denen des Windows Azure Pack-Mandanten müssen externe IP-Adressen in beiden Umgebungen routingfähig sein. Diese Konnektivität kann zudem die Zuordnung eines DNS-Servers zur Auflösung von Namen virtueller Computer zwischen den Umgebungen umfassen.

### <a name="iis-requirements"></a>IIS-Anforderungen
Auf dem Computer, der das Windows Azure Pack-Mandantenportal hostet (bei dem es sich um einen physischen Host, einen virtuellen Computer oder mehrere virtuelle Computer handeln kann), muss die IIS-URL-Rewrite-Erweiterung installiert sein. Wenn sie noch nicht installiert ist, können Sie sie [hier](https://www.iis.net/downloads/microsoft/url-rewrite) herunterladen. Wenn das Mandantenportal von mehreren virtuellen Computern gehostet wird, installieren Sie die Erweiterung auf jedem virtuellen Computer.

## <a name="configure-azure-stack"></a>Konfigurieren von Azure Stack
Vor der Konfiguration des Windows Azure Pack-Connectors müssen Sie den Multicloudmodus im Azure Stack-Benutzerportal aktivieren. In diesem Modus können Benutzer auswählen, welche Cloud für den Zugriff auf Ressourcen verwendet werden soll.

Um den Multicloudmodus zu aktivieren, müssen Sie nach der Bereitstellung von Azure Stack das Skript „Add-AzurePackConnector.ps1“ ausführen. In der folgenden Tabelle werden die Skriptparameter beschrieben:


|  Parameter | Beschreibung | Beispiel |   
| -------- | ------------- | ------- |  
| AzurePackClouds | URIs der Windows Azure Pack-Connectors. Diese URIs sollten den Windows Azure Pack-Mandantenportalen entsprechen. | @{CloudName = "AzurePack1"; CloudEndpoint = "https://waptenantportal1:40005"},@{CloudName = "AzurePack2"; CloudEndpoint = "https://waptenantportal2:40005"}<br><br>  (Standardmäßig ist der Portwert auf 40005 festgelegt.) |  
| AzureStackCloudName | Bezeichnung für die lokale Azure Stack-Cloud.| "AzureStack" |
| DisableMultiCloud | Ein Schalter zur Deaktivierung des Multicloudmodus.| N/V |
| | |

Sie können sofort nach der Bereitstellung oder zu einem späteren Zeitpunkt das Skript „Add-AzurePackConnector.ps1“ ausführen. Um das Skript sofort nach der Bereitstellung auszuführen, verwenden Sie dieselbe Windows PowerShell-Sitzung, in der die Azure Stack-Bereitstellung durchgeführt wurde. Alternativ können Sie eine neue Windows PowerShell-Sitzung als (mit dem azurestackadmin-Konto angemeldeter) Administrator öffnen.

1. Führen Sie anhand der folgenden Befehle das Skript „Add-AzurePackConnector.ps1“ (mit den für Ihre Umgebung spezifischen Werten) aus. Beachten Sie, dass Sie mit dem Skript „Add-AzurePackConnector“ mehr als einen Windows Azure Pack-Connectorendpunkt hinzufügen können.
 
 ```powershell
    $cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local", `
    (ConvertTo-SecureString -String "<password>" -AsPlainText -Force))
    $session = New-PSSession -ComputerName 'azs-ercs01' `
     -Credential $cred `
     -ConfigurationName PrivilegedEndpoint `
     -Authentication Credssp

    # Enable Multicloud
    Invoke-Command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
    @{CloudName = "AzurePack_1"; CloudEndpoint = "https://waptenantportal1:40005"},`
    @{CloudName = "AzurePack_2"; CloudEndpoint = "https://waptenantportal2:40005" } `
    -AzureStackCloudName "AzureStack" }

 ```
> [!NOTE]
> Beim aktuellen Build liegt das Problem vor, dass das Skript „Add-AzurePackConnector“ nach der Beendigung für einen längeren Zeitraum (mehrere Minuten) in einer Abrufschleife verbleibt, bis es beendet wird. Wenn Sie die Meldung **VERBOSE: Step 'Configure Azure Pack Connector' status: 'Success'** sehen, können Sie das Skript beenden oder warten, bis es eigenständig beendet wird. Es macht keinen Unterschied, da die Konfiguration bereits erfolgreich durchgeführt wurde.

2. Notieren Sie sich die durch dieses Skript erzeugten Ausgabedateien – eine Datei für jede von Ihnen angegebene Windows Azure Pack-Umgebung. Die Dateien befinden sich unter: \\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput. Diese Dateien enthalten die Informationen, die zum Konfigurieren der Windows Azure Pack-Zielumgebungen erforderlich sind. In einem späteren Schritt dieser Anweisungen übergeben Sie diese Datei als Parameter an ein Skript. Diese Datei enthält folgende Informationen:

    * **AzurePackConnectorEndpoint**: Enthält den Endpunkt mit dem Windows Azure Pack-Connectordienst.
    * **AuthenticationIdentityProviderPartner**: Enthält das folgende Wertpaar:
        * Authentifizierungstoken-Signaturzertifikat, dem die Windows Azure Pack-Mandanten-API vertrauen muss, um Aufrufe von der Azure Stack-Portalerweiterung anzunehmen

        * Der dem Signaturzertifikat zugeordnete „Bereich“. Beispiel: https://adfs.local.azurestack.global.external/adfs/c1d72562-534e-4aa5-92aa-d65df289a107/.

3.  Navigieren Sie zum Ordner, der die Ausgabedateien enthält (\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput), und kopieren Sie die Dateien auf Ihren lokalen Computer. Die Dateinamen lauten ungefähr wie folgt: „AzurePack-06-27-15-50.txt“.

4.  Testen Sie die Konfiguration.

    a. Öffnen Sie Ihren Browser, und melden Sie sich beim Azure Stack-Benutzerportal (https://portal.local.azurestack.external/) an.
    
    b. Nachdem Sie sich als Mandant angemeldet haben und das Portal geladen wird, werden Fehler angezeigt, die besagen, dass keine Abonnements oder Erweiterungen von der Azure Pack-Cloud abgerufen werden können. Klicken Sie auf **OK**, um diese Meldungen zu schließen. (Diese Fehlermeldungen werden nach der Konfiguration von Windows Azure Pack nicht mehr angezeigt.)

    c. Beachten Sie die Dropdown-Liste **Cloud** in der oberen linken Ecke des Portals.

    ![Cloudselektor auf der Benutzeroberfläche von Azure Stack](media/azure-stack-manage-wap/image3.png)

## <a name="configure-windows-azure-pack"></a>Konfigurieren von Windows Azure Pack
Die manuelle Konfiguration von Windows Azure Pack ist nur bei dieser Vorschauversion des Connectors erforderlich.

>[!IMPORTANT]
Verwenden Sie bei dieser Vorschauversion den Windows Azure Pack-Connector nur in Testumgebungen und nicht in Produktionsbereitstellungen.

1.  Installieren Sie die MSI-Dateien des Connectors auf dem virtuellen Computer mit dem Windows Azure Pack-Mandantenportal, und installieren Sie die Zertifikate. (Wenn Sie mehrere virtuelle Computer mit dem Mandantenportal besitzen, müssen Sie diesen Schritt auf jedem virtuellen Computer durchführen.)

    a. Kopieren Sie im Datei-Explorer den (zuvor heruntergeladenen) Ordner **WAPConnector** auf dem virtuellen Computer mit dem Mandantenportal in einen Ordner namens **c:\temp**.

    b. Öffnen Sie eine Konsole oder RDP-Verbindung mit dem virtuellen Computer, auf dem sich das Mandantenportal befindet.

    c. Navigieren Sie zum Verzeichnis **c:\temp\wapconnector\setup\scripts**, und führen Sie das Skript **Installation-Connector.ps1** aus, um die drei MSI-Dateien zu installieren. Es müssen keine Parameter angegeben werden.

     ```powershell
    cd C:\temp\wapconnector\setup\scripts\

    .\Install-Connector.ps1
    ```
     d. Navigieren Sie zum Verzeichnis **c:\inetpub**, und stellen Sie sicher, dass die drei neuen Websites installiert sind:

       * MgmtSvc-Connector

       * MgmtSvc-ConnectorExtension

       * MgmtSvc-ConnectorController

    e. Führen Sie im selben Ordner **c:\temp\wapconnector\setup\scripts** das Skript **Configure-Certificates.ps1** aus, um Zertifikate zu installieren. Standardmäßig wird dasselbe Zertifikat verwendet, das für die Mandantenportalwebsite in Windows Azure Pack verfügbar ist. Stellen Sie sicher, dass es sich um ein gültiges Zertifikat handelt (das vom virtuellen Azure Stack-Computer „AzS-WASP01“ und von anderen Clientcomputern, die auf das Azure Stack-Portal zugreifen, als vertrauenswürdig eingestuft wird). Andernfalls funktioniert die Kommunikation nicht. (Alternativ können Sie mit dem Parameter „-Thumbprint“ explizit einen Zertifikatfingerabdruck als Parameter übergeben.)

     ```powershell
        cd C:\temp\wapconnector\setup\scripts\

        .\Configure-Certificates.ps1
    ```

    f. Um die Konfiguration dieser drei Dienste fertig zu stellen, führen Sie zum Aktualisieren der Parameter der Datei „Web.config“ das Skript **Configure-WapConnector.ps1** aus.

    |  Parameter | Beschreibung | Beispiel |   
    | -------- | ------------- | ------- |  
    | TenantPortalFQDN | Der FQDN des Windows Azure Pack-Mandantenportals. | tenant.contoso.com | 
    | TenantAPIFQDN | Der FQDN der Windows Azure Pack-Mandanten-API. | tenantapi.contoso.com  |
    | AzureStackPortalFQDN | Der FQDN des Azure Stack-Benutzerportals. | portal.local.azurestack.external |
    | | |
    
     ```powershell
    .\Configure-WapConnector.ps1 -TenantPortalFQDN "tenant.contoso.com" `
         -TenantAPIFQDN "tenantapi.contoso.com" `
         -AzureStackPortalFQDN "portal.local.azurestack.external"
    ```
    g. Wenn Sie über mehrere virtuelle Computer mit dem Mandantenportal verfügen, wiederholen Sie Schritt 1 für jeden dieser virtuellen Computer durch.

2. Installieren Sie die neue MSI-Datei der Mandanten-API auf jedem virtuellen Computer mit der Windows Azure Pack-Mandanten-API.

    a. Bei Verwendung eines Lastenausgleichsmoduls wird empfohlen, den virtuellen Computer als offline zu markieren.

    b. Kopieren Sie im Datei-Explorer den Ordner **WAPConnector** auf jedem Computer der Mandanten-API in den Ordner **c:\temp**.

    c. Kopieren Sie die zuvor gespeicherte Datei „AzurePackConnectorOutput.txt“ in den Ordner **c:\temp\WAPConnector**.

    d. Öffnen Sie eine Konsole oder RDP-Verbindung mit der VM der Mandanten-API, auf die Sie die Dateien kopiert haben.
    
    e. Navigieren Sie zum Verzeichnis **c:\temp\wapconnector\setup\scripts**, und führen Sie **Update-TenantAPI.ps1** aus. Bei dieser neuen Version der WAP-Mandanten-API wurde eine Änderung vorgenommen, damit nicht nur mit dem aktuellen STS eine Vertrauensstellung hergestellt werden kann, sondern auch mit der AD FS-Instanz in Azure Stack.

     ```powershell
    cd C:\temp\wapconnector\setup\packages\

    .\Update-TenantAPI.ps1
    ```

    f.  Wiederholen Sie Schritt 2 auf allen virtuellen Computern, auf denen die Mandanten-API ausgeführt wird.
3. Führen Sie auf **nur einer** der Mandanten-API-VMs das Skript „Configure-TrustAzureStack.ps1“ aus, um eine Vertrauensstellung zwischen der Mandanten-API und der AD FS-Instanz in Azure Stack herzustellen. Sie müssen ein Konto mit SysAdmin-Zugriff auf die Datenbank „Microsoft.MgmtSvc.Store“ verwenden. Dieses Skript weist die folgenden Parameter auf:

    | Parameter | Beschreibung | Beispiel |
    | --------- | ------------| ------- |
   | SqlServer | Der Name des SQL Servers, der die Datenbank „Microsoft.MgmtSvc.Store“ enthält. Dieser Parameter ist erforderlich. | SQLServer | 
   | DataFile | Die Ausgabedatei, die zuvor bei der Konfiguration des Multicloudmodus von Azure Stack generiert wurde. Dieser Parameter ist erforderlich. | AzurePack-06-27-15-50.txt | 
   | PromptForSqlCredential | Gibt an, dass Sie interaktiv durch das Skript zum Festlegen von Anmeldeinformationen für die SQL-Authentifizierung aufgefordert werden sollen, wenn eine Verbindung mit der SQL Server-Instanz hergestellt wird. Die angegebenen Anmeldeinformationen müssen mit ausreichenden Berechtigungen zum Deinstallieren von Datenbanken und Schemas sowie zum Löschen von Benutzeranmeldungen ausgestattet sein. Wenn keine dieser Informationen angegeben werden, geht das Skript davon aus, dass im aktuellen Benutzerkontext Zugriff auf diese besteht. | Es müssen keine Werte angegeben werden. |
   |  |  |

    Wenn Ihnen der zu verwendende SQL Server nicht bekannt ist, können Sie dies herausfinden. Stellen Sie eine Verbindung mit dem Computer mit der Mandanten-API her. Heben Sie dann mit dem Befehl „Unprotect-MgmtSvc“ den Schutz für die Datei „Web.config“ der Mandanten-API auf, und suchen Sie nach dem Servernamen in der Verbindungszeichenfolge. Denken Sie daran, erneut „Protect-MgmtSvc“ auszuführen, um die Datei „Web.config“ der Mandanten-API zu schützen.

  ```powershell
  cd C:\temp\wapconnector\setup\scripts\

 .\Configure-TrustAzureStack.ps1 -SqlServer "SQLServer" `
       -DataFile "C:\temp\wapconnector\AzurePackConnectorOutput.txt"
  ```

## <a name="example"></a>Beispiel
Das folgende Beispiel zeigt eine vollständige Bereitstellung des Windows Azure Pack-Connectors in einer Azure Stack-Einzelknotenkonfiguration und zwei Windows Azure Pack-Expressinstallationen. (Jede Expressinstallation wird auf einem einzelnen Computer mit den Beispielnamen *wapcomputer1* und *wapcomputer2* ausgeführt.)

```powershell
# Run the following script on the Azure Stack host
$cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local",`
     (ConvertTo-SecureString -String "p@ssw0rd" -AsPlainText -Force))
$session = New-PSSession -ComputerName 'azs-ercs01' -Credential $cred `
     -ConfigurationName PrivilegedEndpoint -Authentication Credssp
 
# Enable Multicloud
invoke-command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
     @{CloudName = "AzurePack_1"; CloudEndpoint = "https://wapcomputer1.contoso.com:40005"},`
     @{CloudName = "AzurePack_2"; CloudEndpoint = "https://wapcomputer2.contoso.com:40005"}`
     -AzureStackCloudName "AzureStack" }  

```
Laden Sie die EXE-Datei aus dem [Microsoft Download Center](https://aka.ms/wapconnectorazurestackdlc) herunter, extrahieren Sie sie, und kopieren Sie den Ordner „WAPConnector“ auf dem Windows Azure Pack-Computer in einen Ordner unter **c:\temp**. Kopieren Sie die Dateien, die als Ausgabe im vorherigen Skript generiert wurden (unter \\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput), in den Ordner **c:\temp\WAPConnector**. (Die Dateinamen lauten ungefähr wie folgt: „AzurePack-06-27-15-50.txt“.) Führen Sie dann für jede Instanz von Windows Azure Pack das folgende Skript aus:

 ```powershell
# Install Connector components
cd C:\temp\WAPConnector\Setup\Scripts
.\Install-Connector.ps1

# Configure Certificates for the new Connector services
.\Configure-Certificates.ps1

# Configure the Connector services
.\Configure-WapConnector.ps1 -TenantPortalFQDN "wapcomputer1.contoso.com" `
     -TenantAPIFQDN "wapcomputer1.contoso.com" `
     -AzureStackPortalFQDN "portal.local.azurestack.external"

# Install the updated TenantAPI
.\Update-TenantAPI.ps1

# Establish trust with the Azure Stack AD FS
.\Configure-TrustAzureStack.ps1 -SqlServer "wapcomputer1" `
     -DataFile "C:\temp\wapconnector\AzurePack-06-27-15-50.txt" 

```
## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung
1.  Stellen Sie sicher, dass Netzwerkkonnektivität zwischen Azure Stack und Windows Azure Pack besteht. Diese Konnektivität muss zwischen jedem Mandantencomputer, der auf das Azure Stack-Portal zugreift, und dem virtuellen Computer mit dem Windows Azure Pack-Mandantenportal bestehen, auf dem die neuen Connectordienste ausgeführt werden.
2.  Stellen Sie sicher, dass alle angegebenen FQDNs richtig sind.
3.  Stellen Sie sicher, dass die SSL-Zertifikate, die bei den neuen Connectordiensten verwendet werden, von Azure Stack (insbesondere der VM „AzS-WASP01“) und allen anderen Computern, die der Mandant möglicherweise für den Zugriff auf das Azure Stack-Benutzerportal verwendet, als vertrauenswürdig eingestuft werden.
4. Bekannte Probleme werden unter [Problembehandlung bei Microsoft Azure Stack](azure-stack-troubleshooting.md) erläutert.


## <a name="next-steps"></a>Nächste Schritte
[Verwenden des Administrator- und des Benutzerportals in Azure Stack](azure-stack-manage-portals.md)
