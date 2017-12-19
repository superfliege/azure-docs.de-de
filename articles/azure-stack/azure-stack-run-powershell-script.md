---
title: Bereitstellen des Azure Stack Development Kit | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie das Azure Stack Development Kit vorbereiten und das PowerShell-Skript zum Bereitstellen von Azure Stack Development Kit ausführen."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 7c320c6ba51ae0800407aab7aee92c42b2b441a7
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Bereitstellen des Azure Stack Development Kit

*Gilt für: Azure Stack Development Kit*

Um das [Azure Stack Development Kit](azure-stack-poc.md) bereitstellen zu können, müssen Sie die folgenden Schritte ausführen:

1. [Laden Sie das Bereitstellungspaket herunter](https://azure.microsoft.com/overview/azure-stack/try/?v=try), um „Cloudbuilder.vhdx“ zu erhalten.
2. Bereiten Sie „cloudbuilder.vhdx“ vor, um den Computer (den Development Kit-Host) zu konfigurieren, auf dem Sie das Development Kit installieren möchten. Nach diesem Schritt wird der Development Kit-Host mit „cloudbuilder.vhdx“ gestartet.
3. Stellen Sie das Development Kit auf dem Development Kit-Host bereit.

> [!NOTE]
> Um auch dann optimale Ergebnisse zu erzielen, wenn Sie eine nicht verbundene Azure Stack-Umgebung verwenden möchten, sollten Sie die Bereitstellung durchführen, während Sie mit dem Internet verbunden sind. Auf diese Weise kann die Evaluierungsversion von Windows Server 2016, die Teil der Development Kit-Installation ist, während der Bereitstellung aktiviert werden.

## <a name="download-and-extract-the-development-kit"></a>Herunterladen und Extrahieren des Development Kit
1. Bevor Sie den Download starten, stellen Sie sicher, dass Ihr Computer die folgenden Voraussetzungen erfüllt:

  - Der Computer muss zusätzlich zum Betriebssystem-Datenträger mindestens über 60 GB freien Speicherplatz auf vier separaten, identischen logischen Festplatten verfügen.
  - [.NET Framework 4.6 (oder eine höhere Version)](https://aka.ms/r6mkiy) muss installiert sein.

2. [Navigieren Sie zur Seite mit den ersten Schritten](https://azure.microsoft.com/overview/azure-stack/try/?v=try), auf der Sie das Azure Stack Development Kit herunterladen und Ihre Details angeben können. Klicken Sie anschließend auf **Übermitteln**.
3. Laden Sie den [Deployment Checker for Azure Stack Development Kit](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) (Bereitstellungsprüfer für Azure Stack Development Kit) herunter, und führen Sie dieses Skript zur Überprüfung der Voraussetzungen aus. Mit diesem eigenständigen Skript werden die Voraussetzungsüberprüfungen des Setups für das Azure Stack Development Kit durchlaufen. Hiermit können Sie sicherstellen, dass die Hardware- und Softwareanforderungen erfüllt sind, bevor Sie das größere Paket für das Azure Stack Development Kit herunterladen.
4. Klicken Sie unter **Software herunterladen** auf **Azure Stack Development Kit**.

  > [!NOTE]
  > Der ASDK-Download (AzureStackDevelopmentKit.exe) selbst umfasst ca. 10 GB. Wenn Sie für den Download auch die ISO-Datei mit der Evaluierungsversion von Windows Server 2016 auswählen, erhöht sich die Downloadgröße auf ca. 17 GB. Sie können diese ISO-Datei verwenden, um nach Abschluss der ASDK-Installation ein Windows Server 2016-VM-Image zu erstellen und dem Azure Stack Marketplace hinzuzufügen. Beachten Sie, dass dieses Image mit der Windows Server 2016-Evaluierungsversion nur zusammen mit dem ASDK verwendet werden kann und den ASDK-Lizenzbedingungen unterliegt.

5. Klicken Sie nach Abschluss des Downloads auf **Ausführen**, um die selbstextrahierende ASDK-Datei (AzureStackDevelopmentKit.exe) zu starten.
6. Prüfen und akzeptieren Sie den angezeigten Lizenzvertrag auf der Seite **License Agreement** (Lizenzvertrag) des Self-Extractor-Assistenten, und klicken Sie anschließend auf **Weiter**.
7. Überprüfen Sie die Informationen in der Datenschutzerklärung, die auf der Seite **Important Notice** (Wichtiger Hinweis) des Self-Extractor-Assistenten angezeigt werden, und klicken Sie anschließend auf **Weiter**.
8. Wählen Sie den Speicherort für die Azure Stack-Setupdateien, die extrahiert werden sollen, auf der Seite **Select Destination Location** (Zielspeicherort auswählen) des Self-Extractor-Assistenten aus, und klicken Sie anschließend auf **Weiter**. Der Standardspeicherort lautet „*Aktueller Ordner*\Azure Stack Development Kit“. 
9. Sehen Sie sich die zusammengefassten Informationen zum Speicherort auf der Seite **Ready to Extract** (Bereit für Extraktion) des Self-Extractor-Assistenten an, und klicken Sie anschließend auf **Extract** (Extrahieren), um die Dateien „CloudBuilder.vhdx“ (ca. 25 GB) und „ThirdPartyLicenses.rtf“ zu extrahieren. Dieser Vorgang nimmt einige Zeit in Anspruch.
10. Kopieren bzw. verschieben Sie die Datei „CloudBuilder.vhdx“ in den Stammordner des Laufwerks C:\ (C:\CloudBuilder.vhdx) auf dem ASDK-Hostcomputer.

> [!NOTE]
> Nachdem Sie die Dateien extrahiert haben, können Sie die EXE- und BIN-Dateien löschen, um Platz auf der Festplatte zu schaffen. Oder Sie können diese Dateien sichern, damit Sie sie nicht erneut herunterladen müssen, wenn Sie das ASDK erneut bereitstellen müssen.

## <a name="deploy-the-asdk-using-a-guided-experience"></a>Bereitstellen des ASDK per Anleitungsoberfläche
Das ASDK kann mit einer grafischen Benutzeroberfläche (GUI) bereitgestellt werden, die Sie bereitstellen können, indem Sie das PowerShell-Skript „asdk-installer.ps1“ herunterladen und ausführen.

> [!NOTE]
> Die Installer-Benutzeroberfläche für das Azure Stack Development Kit ist ein Open Source-Skript, das auf WCF und PowerShell basiert.

### <a name="prepare-the-development-kit-host-using-a-guided-user-experience"></a>Vorbereiten des Development Kit-Hosts per Anleitungsoberfläche
Bevor Sie das ASDK auf dem Hostcomputer installieren können, muss die ASDK-Umgebung vorbereitet werden.
1. Melden Sie sich als lokaler Administrator auf Ihrem ASDK-Hostcomputer an.
2. Stellen Sie sicher, dass die Datei „CloudBuilder.vhdx“ in den Stammordner des Laufwerks C:\ verschoben wurde (C:\CloudBuilder.vhdx).
3. Führen Sie das folgende Skript zum Herunterladen der Development Kit-Installationsdatei („asdk-installer.ps1“) aus dem [GitHub-Repository mit den Azure Stack-Tools](https://github.com/Azure/AzureStack-Tools) in den Ordner **C:\AzureStack_Installer** auf Ihrem Development Kit-Hostcomputer aus:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Führen Sie in einer PowerShell-Konsole mit erhöhten Rechten das Skript **C:\AzureStack_Installer\asdk-installer.ps1** aus, und klicken Sie anschließend auf **Umgebung vorbereiten**.
5. Navigieren Sie auf der Seite **Select Cloudbuilder vhdx** ("Cloudbuilder.vhdx" auswählen) des Installationsprogramms zur zuvor heruntergeladenen und extrahierten Datei **cloudbuilder.vhdx**, und wählen Sie sie aus. Auf dieser Seite können Sie optional auch das Kontrollkästchen **Treiber hinzufügen** aktivieren, wenn Sie dem Development Kit-Hostcomputer zusätzliche Treiber hinzufügen müssen.  
6. Geben Sie auf der Seite **Optionale Einstellungen** das lokale Administratorkonto für den Development Kit-Hostcomputer an. 

  > [!IMPORTANT]
  > Wenn Sie diese Anmeldeinformationen nicht angeben, benötigen Sie direkten Zugriff oder KVM-Zugriff auf den Host, nachdem der Computer während der Development Kit-Einrichtung neu gestartet wird.

7. Sie können diese optionalen Einstellungen auch auf der Seite **Optionale Einstellungen** angeben:
    - **Computername**: Diese Option legt den Namen für den Development Kit-Host fest. Der Name muss die FQDN-Anforderungen erfüllen und darf höchstens 15 Zeichen umfassen. Der Standardwert ist ein von Windows generierter zufälliger Computername.
    - **Zeitzone**: Legt die Zeitzone für den Development Kit-Host fest. Der Standardwert ist (UTC-8:00) Pacific Time (USA und Kanada).
    - **Statische IP-Konfiguration**: Legt für Ihre Bereitstellung die Verwendung einer statischen IP-Adresse fest. Andernfalls werden beim Neustart des Installationsprogramms in „cloudbuilder.vhdx“ die Netzwerkschnittstellen mit DHCP konfiguriert.
11. Klicken Sie auf **Weiter**.

12. Wenn Sie im vorherigen Schritt eine statische IP-Konfiguration ausgewählt haben, müssen Sie jetzt Folgendes tun:
    - Wählen Sie einen Netzwerkadapter aus. Stellen Sie sicher, dass Sie eine Verbindung mit dem Adapter herstellen können, bevor Sie auf **Weiter** klicken.
    - Stellen Sie sicher, dass die Werte für **IP-Adresse**, **Gateway** und **DNS** richtig sind, und klicken Sie dann auf **Weiter**.
13. Klicken Sie auf **Weiter**, um den Vorbereitungsprozess zu starten.
14. Wenn die Vorbereitung **Abgeschlossen** anzeigt, klicken Sie auf **Weiter**.
15. Klicken Sie auf **Jetzt neu starten**, um in „cloudbuilder.vhdx“ zu starten und den Bereitstellungsprozess fortzusetzen.


### <a name="deploy-the-development-kit-using-a-guided-experience"></a>Bereitstellen des Development Kit per Anleitungsoberfläche
Nachdem Sie den ASDK-Hostcomputer vorbereitet haben, kann das ASDK mit dem folgenden Schritten im Image „CloudBuilder.vhdx“ bereitgestellt werden. 
1. Nachdem der Hostcomputer im Image „CloudBuilder.vhdx“ erfolgreich gestartet wurde, können Sie sich mit den Administratoranmeldeinformationen anmelden, die in den vorherigen Schritten angegeben wurden. 
2. Öffnen Sie eine PowerShell-Konsole mit erhöhten Rechten, und führen Sie das Skript **\AzureStack_Installer\asdk-installer.ps1** aus (dieses befindet sich jetzt im Image „Cloudbuilder.vhdx“ ggf. auf einem anderen Laufwerk). Klicken Sie auf **Installieren**.
3. Wählen Sie im Dropdownfeld **Typ** die Option **Azure Cloud** oder **AD FS**.
    - **Azure Cloud**: Konfiguriert Azure Active Directory (Azure AD) als Identitätsanbieter. Für die Verwendung dieser Option benötigen Sie eine Internetverbindung, den vollständigen Namen eines Azure AD-Verzeichnismandanten der Form „*Domänenname*.onmicrosoft.com“ und globale Administratoranmeldeinformationen für das angegebene Verzeichnis. 
    - **AD FS**: Der standardmäßige Stampverzeichnisdienst wird als Identitätsanbieter verwendet. Das Standardkonto für die Anmeldung lautet azurestackadmin@azurestack.local, und das dazugehörige Kennwort haben Sie im Rahmen des Setupvorgangs angegeben.
4. Geben Sie unter **Lokales Administratorkennwort** im Feld **Kennwort** das lokale Administratorkennwort ein (das mit dem derzeit konfigurierten lokalen Administratorkennwort übereinstimmen muss), und klicken Sie dann auf **Weiter**.
5. Wählen Sie einen für das Development Kit zu verwendenden Netzwerkadapter aus, und klicken Sie dann auf **Weiter**.
6. Wählen Sie DHCP oder statische Netzwerkkonfiguration für den virtuellen Computer BGPNAT01 aus.
    - **DHCP** (Standard): Der virtuelle Computer ruft die IP-Netzwerkkonfiguration vom DHCP-Server ab.
    - **Statisch**: Verwenden Sie diese Option nur, wenn per DHCP keine gültige IP-Adresse für Azure Stack zugewiesen werden kann. Eine statische IP-Adresse muss mit der Länge der Subnetzmaske im CIDR-Format (z.B. 10.0.0.5/24) angegeben werden.
7. Legen Sie optional die folgenden Werte fest:
    - **VLAN-ID**: Legt die VLAN-ID fest. Verwenden Sie diese Option nur, wenn für Host und AzS-BGPNAT01 eine VLAN-ID für den Zugriff auf das physische Netzwerk (und das Internet) konfiguriert werden muss. 
    - **DNS-Weiterleitung**: Ein DNS-Server wird als Teil der Azure Stack-Bereitstellung erstellt. Damit Computer innerhalb der Lösung Namen außerhalb des Stamps auflösen können, stellen Sie Ihren vorhandenen Infrastruktur-DNS-Server bereit. Der DNS-Server innerhalb des Stamps leitet Auflösungsanforderungen, die unbekannte Namen betreffen, an diesen Server weiter.
    - **Zeitserver**: In diesem erforderlichen Feld wird der Zeitserver festgelegt, der vom Development Kit verwendet wird. Dieser Parameter muss als gültige Zeitserver-IP-Adresse angegeben werden. Servernamen werden nicht unterstützt.
  
  > [!TIP]
  > Besuchen Sie zum Ermitteln der IP-Adresse eines Zeitservers [pool.ntp.org](http:\\pool.ntp.org), oder pingen Sie „time.windows.com“. 
  
8. Klicken Sie auf **Weiter**.
 
9. Auf der Seite **Überprüfen von Netzwerkadaptereigenschaften** sehen Sie eine Statusanzeige. 
    - Wenn sie **Ein Update kann nicht heruntergeladen werden** anzeigt, befolgen Sie die Anweisungen auf der Seite.
    - Wenn sie **Abgeschlossen** anzeigt, klicken Sie auf **Weiter**.
10. Klicken Sie auf der Seite **Zusammenfassung** auf **Bereitstellen**. Hier können Sie auch die PowerShell-Setupbefehle kopieren, die zum Installieren des Development Kit verwendet werden.
11. Wenn Sie eine Azure AD-Bereitstellung nutzen, werden Sie einige Minuten nach dem Start des Setupvorgangs zum Eingeben der Anmeldeinformationen Ihres globalen Administratorkontos für Azure AD aufgefordert.
12. Der Bereitstellungsprozess kann einige Stunden dauern, wobei das System automatisch einmal neu startet. Wenn die Bereitstellung erfolgreich verlaufen ist, zeigt die PowerShell-Konsole an: **ABGESCHLOSSEN: Aktion 'Bereitstellung'**. Wenn die Bereitstellung nicht erfolgreich ist, können Sie eine ganz [neue Bereitstellung](azure-stack-redeploy.md) durchführen oder die folgenden PowerShell-Befehle in demselben PowerShell-Fenster mit erhöhten Rechten verwenden, um den Bereitstellungsvorgang ab dem letzten erfolgreichen Schritt neu zu starten:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

   > [!IMPORTANT]
   > Falls Sie den Status der Bereitstellung überwachen möchten, können Sie sich als „azurestack\AzureStackAdmin“ anmelden, wenn der Development Kit-Host während des Setups neu gestartet wird. Wenn Sie sich als lokaler Administrator anmelden, nachdem der Computer der Domäne beigetreten ist, wird der Bereitstellungsstatus nicht angezeigt. Führen Sie die Bereitstellung nicht erneut aus, sondern melden Sie sich stattdessen als „azurestack\AzureStackAdmin“ an, um zu überprüfen, ob sie ausgeführt wird.
   

## <a name="deploy-the-asdk-using-powershell"></a>Bereitstellen des ASDK per PowerShell
In den obigen Schritten wurde Schritt für Schritt beschrieben, wie Sie das ASDK über eine Anleitungsoberfläche bereitstellen. Alternativ hierzu können Sie PowerShell nutzen, um das ASDK auf dem Development Kit-Host bereitzustellen, indem Sie die Schritte in diesem Abschnitt ausführen.

### <a name="configure-the-asdk-host-computer-to-boot-from-cloudbuildervhdx"></a>Konfigurieren des ASDK-Hostcomputers für das Starten mit „CloudBuilder.vhdx“
Mit diesen Befehlen wird Ihr ASDK-Hostcomputer für das Starten über die heruntergeladene und extrahierte virtuelle Azure Stack-Festplatte (CloudBuilder.vhdx) konfiguriert. Starten Sie den ASDK-Hostcomputer neu, nachdem Sie diese Schritte ausgeführt haben.

1. Starten Sie eine Eingabeaufforderung als Administrator.
2. Führen Sie `bcdedit /copy {current} /d "Azure Stack"` aus.
3. Kopieren Sie (mit STRG+C) den zurückgegebenen CLSID-Wert, einschließlich der erforderlichen geschweiften Klammern ({}). Dieser Wert hat die Bezeichnung {CLSID} und muss (mit STRG+V oder Rechtsklick) in den verbleibenden Schritten eingefügt werden.
4. Führen Sie `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` aus. 
5. Führen Sie `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` aus. 
6. Führen Sie `bcdedit /set {CLSID} detecthal on` aus. 
7. Führen Sie `bcdedit /default {CLSID}` aus.
8. Führen Sie `bcdedit` aus, um die Starteinstellungen zu überprüfen. 
9. Stellen Sie sicher, dass die Datei „CloudBuilder.vhdx“ in den Stammordner des Laufwerks C:\ verschoben wurde (C:\CloudBuilder.vhdx), und starten Sie den Development Kit-Hostcomputer neu. Nachdem der ASDK-Host neu gestartet wurde, sollte das Starten jetzt standardmäßig über die VM „CloudBuilder.vhdx“ erfolgen. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>Vorbereiten des Development Kit-Hosts mit PowerShell 
Nachdem der Development Kit-Hostcomputer für das Image „CloudBuilder.vhdx“ erfolgreich gestartet wurde, können Sie eine PowerShell-Konsole mit erhöhten Rechten öffnen und die Befehle in diesem Abschnitt ausführen, um das ASDK auf dem Development Kit-Host bereitzustellen.

> [!IMPORTANT] 
> Für die ASDK-Installation wird genau eine Netzwerkschnittstellenkarte (NIC) für das Netzwerk unterstützt. Wenn Sie mehrere Netzwerkadapter nutzen, sollten Sie vor dem Ausführen des Bereitstellungsskripts sicherstellen, dass nur einer aktiviert ist (und alle anderen deaktiviert).

Sie können Azure Stack mit Azure AD oder AD FS als Identitätsanbieter bereitstellen. Azure Stack, Ressourcenanbieter und andere Anwendungen funktionieren in beiden Fällen. Weitere Informationen zu den Komponenten, die für AD FS in Azure Stack unterstützt werden, finden Sie im Artikel [Wichtige Features und Konzepte](.\azure-stack-key-features.md).

> [!TIP]
> Wenn Sie keine Setupparameter angeben (siehe „InstallAzureStackPOC.ps1“ mit optionalen Parametern und Beispielen unten), werden Sie zum Eingeben der erforderlichen Parameter aufgefordert.

### <a name="deploy-azure-stack-using-azure-ad"></a>Bereitstellen von Azure Stack mit Azure AD 
Zum Bereitstellen von Azure Stack **mit Azure AD als Identitätsanbieter** müssen Sie entweder direkt oder über einen transparenten Proxy über eine Internetverbindung verfügen. Führen Sie die folgenden PowerShell-Befehle aus, um das Development Kit mit Azure AD bereitzustellen:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

  Einige Minuten nach Beginn der ASDK-Installation werden Sie zum Eingeben der Azure AD-Anmeldeinformationen aufgefordert. Geben Sie die Anmeldeinformationen des globalen Administrators für Ihren Azure AD-Mandanten an. 

### <a name="deploy-azure-stack-using-ad-fs"></a>Bereitstellen von Azure Stack mit AD FS 
Führen Sie die folgenden PowerShell-Befehle aus (indem Sie einfach den Parameter „-UseADFS“ hinzufügen), um das Development Kit **mit AD FS als Identitätsanbieter** bereitzustellen: 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

Bei AD FS-Bereitstellungen wird der standardmäßige Stampverzeichnisdienst als Identitätsanbieter verwendet. Das Standardkonto für die Anmeldung ist azurestackadmin@azurestack.local, und das Kennwort ist auf den Wert festgelegt, den sie für die PowerShell-Setupbefehle angegeben haben.

Der Bereitstellungsprozess kann einige Stunden dauern, und während dieses Zeitraums wird das System automatisch einmal neu gestartet. Wenn die Bereitstellung erfolgreich verlaufen ist, zeigt die PowerShell-Konsole an: **ABGESCHLOSSEN: Aktion 'Bereitstellung'**. Wenn die Bereitstellung nicht erfolgreich ist, können Sie versuchen, das Skript erneut auszuführen, indem Sie den Parameter „-rerun“ verwenden. Alternativ dazu können Sie [ASDK auch ganz neu bereitstellen](.\azure-stack-redeploy.md).
> [!IMPORTANT]
> Wenn Sie den Bereitstellungsstatus nach dem Neustart des ASDK-Hosts überwachen möchten, müssen Sie sich als „AzureStack\AzureStackAdmin“ anmelden. Der Bereitstellungsstatus wird nicht angezeigt, wenn Sie sich nach dem Neustart des Hostcomputers (und dem Beitritt zur Domäne „azurestack.local“) als lokaler Administrator anmelden. Führen Sie die Bereitstellung nicht erneut aus, sondern melden Sie sich stattdessen als „azurestack“ an, um zu überprüfen, ob sie ausgeführt wird.
>

#### <a name="azure-ad-deployment-script-examples"></a>Beispiele für Azure AD-Bereitstellungsskripts
Sie können Skripts für die gesamte Azure AD-Bereitstellung erstellen. Hier sind einige kommentierte Beispiele mit einigen optionalen Parametern angegeben.

Wenn Ihre Azure AD-Identität nur **einem** Azure AD-Verzeichnis zugeordnet ist:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Wenn Ihre Azure AD-Identität **mehr als einem** Azure AD-Verzeichnis zugeordnet ist:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<specific Azure AD directory in the form of domainname.onmicrosoft.com>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Wenn für Ihre Umgebung DHCP **nicht** aktiviert ist, müssen Sie die folgenden zusätzlichen Parameter in eine der obigen Optionen einfügen (Verwendungsbeispiel vorhanden): 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1 -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>ASDK „InstallAzureStackPOC.ps1“ – Optionale Parameter
|Parameter|Erforderlich/Optional|Beschreibung|
|-----|-----|-----|
|AdminPassword|Erforderlich|Legt das lokale Administratorkonto und alle anderen Benutzerkonten auf allen virtuellen Computern fest, die im Rahmen der Development Kit-Bereitstellung erstellt werden. Dieses Kennwort muss mit dem Kennwort des lokalen Administrators auf dem Host übereinstimmen.|
|InfraAzureDirectoryTenantName|Erforderlich|Legt das Mandantenverzeichnis fest. Verwenden Sie diesen Parameter, um ein bestimmtes Verzeichnis anzugeben, für das das AAD-Konto über Berechtigungen zum Verwalten mehrerer Verzeichnisse verfügt. Vollständiger Name eines AAD-Verzeichnismandanten im Format „onmicrosoft.com“.|
|TimeServer|Erforderlich|Verwenden Sie diesen Parameter, um einen bestimmten Zeitserver anzugeben. Dieser Parameter muss als gültige Zeitserver-IP-Adresse angegeben werden. Servernamen werden nicht unterstützt.|
|InfraAzureDirectoryTenantAdminCredential|Optional|Legt den Benutzernamen und das Kennwort für Azure Active Directory fest. Bei diesen Azure-Anmeldeinformationen muss es sich um eine Organisations-ID handeln.|
|InfraAzureEnvironment|Optional|Wählen Sie die Azure-Umgebung aus, für die Sie diese Azure Stack-Bereitstellung registrieren möchten. Die verfügbaren Optionen sind „Public Azure“, „Azure – China“ und „Azure – US Government“.|
|DNSForwarder|Optional|Ein DNS-Server wird als Teil der Azure Stack-Bereitstellung erstellt. Damit Computer innerhalb der Lösung Namen außerhalb des Stamps auflösen können, stellen Sie Ihren vorhandenen Infrastruktur-DNS-Server bereit. Der DNS-Server innerhalb des Stamps leitet Auflösungsanforderungen, die unbekannte Namen betreffen, an diesen Server weiter.|
|NatIPv4Address|Für DHCP-NAT-Unterstützung erforderlich|Legt eine statische IP-Adresse für MAS-BGPNAT01 fest. Verwenden Sie diesen Parameter nur, wenn per DHCP keine gültige IP-Adresse für den Internetzugang zugewiesen werden kann.|
|NatIPv4Subnet|Für DHCP-NAT-Unterstützung erforderlich|IP-Subnetzpräfix, das für die Unterstützung von DHCP über NAT verwendet wird. Verwenden Sie diesen Parameter nur, wenn per DHCP keine gültige IP-Adresse für den Internetzugang zugewiesen werden kann.|
|PublicVlanId|Optional|Legt die VLAN-ID fest. Verwenden Sie diesen Parameter nur, wenn für Host und MAS-BGPNAT01 eine VLAN-ID für den Zugriff auf das physische Netzwerk (und das Internet) konfiguriert werden muss. Beispiel: .\InstallAzureStackPOC.ps1 -Verbose -PublicVLan 305|
|Rerun|Optional|Verwenden Sie dieses Flag, um die Bereitstellung erneut durchzuführen. Alle vorherigen Eingaben werden verwendet. Das erneute Eingeben von zuvor angegebenen Daten wird nicht unterstützt, da mehrere eindeutige Werte generiert und für die Bereitstellung verwendet werden.|

## <a name="activate-the-administrator-and-tenant-portals"></a>Aktivieren des Administrator- und Mandantenportals
Nach Bereitstellungen, für die Azure AD verwendet wird, müssen Sie sowohl das Azure Stack-Administratorportal als auch das Mandantenportal aktivieren. Mit dieser Aktivierung wird die Zustimmung erteilt, dass dem Azure Stack-Portal und Azure Resource Manager die richtigen Berechtigungen (auf der Zustimmungsseite aufgeführt) für alle Benutzer des Verzeichnisses gewährt werden können.

- Navigieren Sie für das Administratorportal zu „https://adminportal.local.azurestack.external/guest/signup“, lesen Sie sich die Informationen durch, und klicken Sie dann auf **Akzeptieren**. Nach dem Akzeptieren können Sie Dienstadministratoren hinzufügen, die nicht gleichzeitig Verzeichnismandantenadministratoren sind.

- Navigieren Sie für das Mandantenportal zu „https://portal.local.azurestack.external/guest/signup“, lesen Sie sich die Informationen durch, und klicken Sie dann auf **Akzeptieren**. Nach dem Akzeptieren können sich Benutzer im Verzeichnis am Mandantenportal anmelden. 

> [!NOTE] 
> Wenn die Portale nicht aktiviert sind, kann sich nur der Verzeichnisadministrator anmelden und die Portale verwenden. Anderen Benutzern, die sich anmelden, wird ein Fehler mit dem Hinweis angezeigt, dass der Administrator keine Berechtigungen für andere Benutzer erteilt hat. Falls der Administrator nicht nativ dem Verzeichnis angehört, für das Azure Stack registriert ist, muss das Azure Stack-Verzeichnis an die Aktivierungs-URL angefügt werden. Wenn Azure Stack beispielsweise für „fabrikam.onmicrosoft.com“ registriert ist und der Administratorbenutzer admin@contoso.com lautet, navigieren Sie zu „https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com“, um das Portal zu aktivieren. 

## <a name="reset-the-password-expiration-policy"></a>Zurücksetzen der Kennwortablaufrichtlinie 
Führen Sie nach dem Bereitstellen des ASDK die unten angegebenen Schritte aus, um sicherzustellen, dass das Kennwort für den Development Kit-Host nicht vor dem Ende des Evaluierungszeitraums abläuft.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>So ändern Sie die Kennwortablaufrichtlinie in PowerShell:
Führen Sie diesen Befehl über eine PowerShell-Konsole mit erhöhten Rechten aus:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>So ändern Sie die Kennwortablaufrichtlinie manuell:
1. Öffnen Sie auf dem Development Kit-Host die **Gruppenrichtlinienverwaltung**, und navigieren Sie zu **Gruppenrichtlinienverwaltung** – **Gesamtstruktur: azurestack.local** – **Domänen** – **azurestack.local**.
2. Klicken Sie mit der rechten Maustaste auf **Standarddomänenrichtlinie**, und klicken Sie auf **Bearbeiten**.
3. Navigieren Sie im Gruppenrichtlinienverwaltungs-Editor zu **Computerkonfiguration** – **Richtlinien** – **Windows-Einstellungen** – **Sicherheitseinstellungen** – **Kontorichtlinien** – **Kennwortrichtlinie**.
4. Doppelklicken Sie im rechten Bereich auf **Maximales Kennwortalter**.
5. Ändern Sie im Dialogfeld **Maximales Kennwortalter – Eigenschaften** den Wert **Kennwort läuft ab in** in „180“, und klicken Sie auf **OK**.


## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)

[Einrichten von PowerShell in Azure Stack](azure-stack-powershell-configure-quickstart.md)

[Register Azure Stack with your Azure subscription](azure-stack-register.md) (Registrieren von Azure Stack in Ihrem Azure-Abonnement)



