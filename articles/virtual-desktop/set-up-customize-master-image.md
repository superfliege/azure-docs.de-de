---
title: Vorbereiten und Anpassen eines VHD-Masterimages – Azure
description: Es wird beschrieben, wie Sie ein Windows Virtual Desktop-Masterimage (Vorschauversion) vorbereiten, anpassen und in Azure hochladen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/03/2019
ms.author: helohr
ms.openlocfilehash: 58471dc539f72c49b041638e928dda751f4bf5a2
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410596"
---
# <a name="prepare-and-customize-a-master-vhd-image"></a>Vorbereiten und Anpassen eines VHD-Masterimages

In diesem Artikel wird beschrieben, wie Sie ein VHD-Masterimage (Virtual Hard Disk) für den Upload in Azure vorbereiten und virtuelle Computer (VMs) erstellen und darauf Software installieren und konfigurieren. Diese Anleitung gilt für eine Konfiguration, die für Windows Virtual Desktop (Vorschauversion) spezifisch ist und für die vorhandenen Prozesse Ihrer Organisation genutzt werden kann.

## <a name="create-a-vm"></a>Erstellen einer VM

Windows 10 Enterprise (mehrere Sitzungen) ist im Azure-Imagekatalog verfügbar. Es gibt zwei Möglichkeiten, dieses Image anzupassen.

Die erste Möglichkeit ist die Bereitstellung eines virtuellen Computers (VM) in Azure, indem Sie die Anleitung unter [Erstellen eines virtuellen Computers aus einem verwalteten Image](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed) befolgen und dann mit [Software: Vorbereitung und Installation](set-up-customize-master-image.md#software-preparation-and-installation) fortfahren.

Die zweite Möglichkeit ist die lokale Erstellung des Images, indem das Image heruntergeladen wird und eine Hyper-V-VM bereitgestellt und dann an Ihre Anforderungen angepasst wird. Dies wird im folgenden Abschnitt beschrieben.

### <a name="local-image-creation"></a>Lokales Erstellen des Images

Nachdem Sie das Image an einen lokalen Speicherort heruntergeladen haben, öffnen Sie **Hyper-V-Manager**, um eine VM mit der gerade kopierten VHD zu erstellen. Unten ist eine einfache Version angegeben. Eine ausführlichere Anleitung finden Sie unter [Create a virtual machine in Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v) (Erstellen eines virtuellen Computers in Hyper-V).

Erstellen Sie wie folgt eine VM mit der kopierten VHD:

1. Öffnen Sie den **Assistenten „Neuer virtueller Computer“**.

2. Wählen Sie auf der Seite „Generation angeben“ die Option **Generation 1** aus.

    ![Screenshot: Seite „Generation angeben“. Die Option „Generation 1“ ist ausgewählt.](media/a41174fd41302a181e46385e1e701975.png)

3. Deaktivieren Sie unter „Prüfpunkttyp“ die Prüfpunkte, indem Sie das Kontrollkästchen deaktivieren.

    ![Screenshot: Abschnitt „Prüfpunkttyp“ auf der Seite „Prüfpunkte“](media/20c6dda51d7cafef33251188ae1c0c6a.png)

Sie können auch das folgende Cmdlet in PowerShell ausführen, um Prüfpunkte zu deaktivieren.

```powershell
Set-VM -Name <VMNAME> -CheckpointType Disabled
```

### <a name="fixed-disk"></a>Datenträger mit fester Größe

Wenn Sie eine VM aus einer vorhandenen VHD erstellen, wird standardmäßig ein dynamischer Datenträger erstellt. Sie können stattdessen einen Datenträger mit fester Größe erstellen, indem Sie wie in der folgenden Abbildung gezeigt die Option **Datenträger bearbeiten...** wählen. Eine ausführlichere Anleitung finden Sie unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

![Screenshot: Option „Datenträger bearbeiten“](media/35772414b5a0f81f06f54065561d1414.png)

Sie können auch das folgende PowerShell-Cmdlet ausführen, um den Datenträger in einen Datenträger mit fester Größe zu ändern.

```powershell
Convert-VHD –Path c:\\test\\MY-VM.vhdx –DestinationPath c:\\test\\MY-NEW-VM.vhd -VHDType Fixed
```

## <a name="software-preparation-and-installation"></a>Software: Vorbereitung und Installation

In diesem Abschnitt wird beschrieben, wie Sie Office 365 ProPlus, OneDrive, FSLogix, Windows Defender und andere gängige Anwendungen vorbereiten und installieren. Wenn Ihre Benutzer auf bestimmte Branchenanwendungen zugreifen müssen, empfehlen wir Ihnen, die Installation durchzuführen, nachdem Sie die Anleitung in diesem Abschnitt befolgt haben.

In diesem Abschnitt wird davon ausgegangen, dass Sie über erhöhte Rechte für die VM verfügen – unabhängig davon, ob sie in Azure oder in Hyper-V-Manager bereitgestellt wird.

### <a name="install-office-in-shared-computer-activation-mode"></a>Installieren von Office im Modus „Aktivierung gemeinsam genutzter Computer“

Verwenden Sie das [Office-Bereitstellungstool](https://www.microsoft.com/download/details.aspx?id=49117), um Office zu installieren. Windows 10 Enterprise (mehrere Sitzungen) unterstützt nur Office 365 ProPlus (nicht Office 2019 (unbefristet).

Für das Office-Bereitstellungstool wird eine XML-Konfigurationsdatei benötigt. Informationen zum Anpassen des folgenden Beispiels finden Sie unter [Konfigurationsoptionen für das Office-Bereitstellungstool](https://docs.microsoft.com/deployoffice/configuration-options-for-the-office-2016-deployment-tool).

Mit dieser von uns bereitgestellten XML-Beispieldatei für die Konfiguration wird Folgendes durchgeführt:

- Installieren von Office über den Insiderkanal und Bereitstellen von Updates über den Insiderkanal bei deren Ausführung
- Verwenden der X64-Architektur
- Deaktivieren automatischer Updates
- Installieren von Visio und Project
- Entfernen aller vorhandenen Installationen von Office und Migrieren der zugehörigen Einstellungen
- Aktivieren der Lizenzierung gemeinsam genutzter Computer für den Betrieb in einer Terminalserverumgebung

Folgendes wird dagegen von dieser XML-Beispielkonfigurationsdatei nicht erledigt:

- Installieren von Skype for Business
- Installieren von OneDrive im Modus „Pro Benutzer“ Weitere Informationen finden Sie unter [Installieren von OneDrive im Modus „Pro Computer“](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Die Lizenzierung gemeinsam genutzter Computer kann über Gruppenrichtlinienobjekte (GPOs) oder Registrierungseinstellungen eingerichtet werden. Das Gruppenrichtlinienobjekt befindet sich unter **Computerkonfiguration\\Richtlinien\\Administrative Vorlagen\\Microsoft Office 2016 (Computer)\\Lizenzierungseinstellungen**.

Das Office-Bereitstellungstool enthält die Datei „setup.exe“. Führen Sie den folgenden Befehl auf einer Befehlszeile aus, um Office zu installieren:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Beispieldatei „configuration.xml“

Mit der folgenden XML-Beispieldatei wird das Insiders-Release installiert (auch als „Insiders Fast“ oder „Insiders Main“ bezeichnet).

```xml
<Configuration>
    <Add OfficeClientEdition="64" SourcePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f">
        <Product ID="O365ProPlusRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Groove" />
            <ExcludeApp ID="Lync" />
            <ExcludeApp ID="OneDrive" />
            <ExcludeApp ID="Teams" />
        </Product>
        <Product ID="VisioProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" /> 
        </Product>
        <Product ID="ProjectProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" />
        </Product>
    </Add>
    <RemoveMSI All="True" />
    <Updates Enabled="FALSE" UpdatePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f" />
    <Display Level="None" AcceptEULA="TRUE" />
    <Logging Level="Verbose" Path="%temp%\WVDOfficeInstall" />
    <Property Value="TRUE" Name="FORCEAPPSHUTDOWN"/>
    <Property Value="1" Name="SharedComputerLicensing"/>
    <Property Value="TRUE" Name="PinIconsToTaskbar"/>
</Configuration>
```

>[!NOTE]
>Das Office-Team empfiehlt die Verwendung der 64-Bit-Installation für den Parameter **OfficeClientEdition**.

Nach der Installation von Office können Sie das Standardverhalten von Office aktualisieren. Führen Sie die folgenden Befehle einzeln oder in einer Batchdatei aus, um das Verhalten zu aktualisieren.

```batch
rem Mount the default user registry hive
reg load HKU\TempDefault C:\Users\Default\NTUSER.DAT
rem Must be executed with default registry hive mounted.
reg add HKU\TempDefault\SOFTWARE\Policies\Microsoft\office\16.0\common /v InsiderSlabBehavior /t REG_DWORD /d 2 /f
rem Set Outlook's Cached Exchange Mode behavior
rem Must be executed with default registry hive mounted.
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v enable /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v syncwindowsetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSettingMonths  /t REG_DWORD /d 1 /f
rem Unmount the default user registry hive
reg unload HKU\TempDefault

rem Set the Office Update UI behavior.
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideupdatenotifications /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideenabledisableupdates /t REG_DWORD /d 1 /f
```

### <a name="disable-automatic-updates"></a>Deaktivieren automatischer Updates

So deaktivieren Sie „Automatische Updates“ über lokale Gruppenrichtlinien:

1. Öffnen Sie **Editor für lokale Gruppenrichtlinien\\Administrative Vorlagen\\Windows-Komponenten\\Windows Update**.
2. Klicken Sie mit der rechten Maustaste auf **Automatische Updates konfigurieren**, und legen Sie die Option auf **Deaktiviert** fest.

Sie können auch den folgenden Befehl an einer Eingabeaufforderung ausführen, um automatische Updates zu deaktivieren.

```batch
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v NoAutoUpdate /t REG_DWORD /d 1 /f
```

### <a name="specify-start-layout-for-windows-10-pcs-optional"></a>Festlegen des Startlayouts für Windows 10-PCs (optional)

Führen Sie diesen Befehl aus, um ein Startlayout für Windows 10-PCs anzugeben.

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SpecialRoamingOverrideAllowed /t REG_DWORD /d 1 /f
```

### <a name="install-onedrive-in-per-machine-mode"></a>Installieren von OneDrive im Modus „Pro Computer“

Normalerweise wird OneDrive im Modus „Pro Benutzer“ installiert. In dieser Umgebung sollte die Anwendung pro Computer installiert werden.

Hier ist beschrieben, wie Sie OneDrive im Modus „Pro Computer“ installieren:

1. Erstellen Sie zunächst einen Speicherort zum Bereitstellen des OneDrive-Installationsprogramms. Hierfür ist ein lokaler Datenträgerordner oder ein Speicherort vom Typ [\\\\unc](file://unc) geeignet.

2. Laden Sie die Datei „OneDriveSetup.exe“ mit diesem Link an Ihren bereitgestellten Speicherort herunter: <https://aka.ms/OneDriveWVD-Installer>.

3. Wenn Sie Office mit OneDrive installiert und **\<ExcludeApp ID="OneDrive" /\>** weggelassen haben, sollten Sie alle vorhandenen OneDrive-Installationen im Modus „Pro Benutzer“ entfernen, indem Sie an einer Eingabeaufforderung mit erhöhten Rechten den folgenden Befehl ausführen:
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. Führen Sie diesen Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus, um den Registrierungswert **AllUsersInstall** festzulegen:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. Führen Sie diesen Befehl aus, um OneDrive im Modus „Pro Computer“ zu installieren:

    ```batch
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. Führen Sie diesen Befehl aus, um OneDrive mit der Anmeldung für alle Benutzer zu starten:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Aktivieren Sie die Option **Silently configure user account** (Benutzerkonto im Hintergrund konfigurieren), indem Sie den folgenden Befehl ausführen.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Sie können bekannte Windows-Ordner umleiten und in OneDrive verschieben, indem Sie den folgenden Befehl ausführen.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

### <a name="teams-and-skype"></a>Teams und Skype

Windows Virtual Desktop unterstützt Skype for Business und Teams offiziell nicht.

### <a name="set-up-user-profile-container-fslogix"></a>Einrichten des Benutzerprofilcontainers (FSLogix)

Befolgen Sie die Anleitung unter [Set up a user profile share for a host pool](create-host-pools-user-profile.md#configure-the-fslogix-profile-container) (Einrichten einer Benutzerprofilfreigabe für einen Hostpool), um den FSLogix-Container als Teil des Images einzubeziehen. Sie können die Funktionalität des FSLogix-Containers testen, indem Sie [diese Schnellstartanleitung](https://docs.fslogix.com/display/20170529/Profile+Containers+-+Quick+Start) verwenden.

### <a name="configure-windows-defender"></a>Konfigurieren von Windows Defender

Wenn Windows Defender auf dem virtuellen Computer konfiguriert ist, sollten Sie sicherstellen, dass beim Anfügen von VHD- und VHDX-Dateien nicht der gesamte Inhalt gescannt wird.

Bei dieser Konfiguration wird nur das Scannen von VHD- und VHDX-Dateien während des Anfügens entfernt, aber es ergibt sich keine Auswirkung auf die Echtzeitüberprüfung.

Eine ausführlichere Anleitung zur Konfiguration von Windows Defender unter Windows Server finden Sie unter [Konfigurieren von Windows Defender Antivirus-Ausschlüssen auf Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus).

Weitere Informationen zum Konfigurieren von Windows Defender, um bestimmte Dateien vom Scannen auszuschließen, finden Sie unter [Konfigurieren und Validieren von Ausschlüssen basierend auf Dateierweiterung und Speicherort des Ordners](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-extension-file-exclusions-windows-defender-antivirus).

### <a name="configure-session-timeout-policies"></a>Konfigurieren von Richtlinien für Sitzungstimeouts

Richtlinien für Remotesitzungen können auf Gruppenrichtlinienebene erzwungen werden, da alle VMs in einem Hostpool Teil derselben Sicherheitsgruppe sind.

Konfigurieren Sie die Richtlinien für Remotesitzungen wie folgt:

1. Navigieren Sie zu **Administrative Vorlagen** > **Windows-Komponenten** > **Remotedesktopdienste** > **Remotedesktop-Sitzungshost** > **Sitzungszeitlimits**.
2. Wählen Sie im Bereich auf der rechten Seite die Richtlinie **Zeitlimit für aktive, aber im Leerlauf befindliche Remotedesktopdienste-Sitzungen festlegen** aus.
3. Ändern Sie im angezeigten modalen Fenster die Richtlinienoption von **Nicht konfiguriert** in **Aktiviert**, um die Richtlinie zu aktivieren.
4. Legen Sie im Dropdownmenü unter der Richtlinienoption den Zeitraum auf **4 Stunden** fest.

Sie können die Richtlinien für Remotesitzungen auch manuell konfigurieren, indem Sie die folgenden Befehle ausführen:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fResetBroken /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxConnectionTime /t REG_DWORD /d 10800000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxDisconnectionTime /t REG_DWORD /d 5000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxIdleTime /t REG_DWORD /d 7200000 /f
```

### <a name="set-up-time-zone-redirection"></a>Einrichten der Zeitzonenumleitung

Die Zeitzonenumleitung kann auf Gruppenrichtlinienebene erzwungen werden, da alle VMs in einem Hostpool Teil derselben Sicherheitsgruppe sind.

Führen Sie die Umleitung für Zeitzonen wie folgt durch:

1. Öffnen Sie auf dem Active Directory-Server die **Gruppenrichtlinien-Verwaltungskonsole**.
2. Erweitern Sie Ihre Domäne und die Gruppenrichtlinienobjekte.
3. Klicken Sie mit der rechten Maustaste auf das **Gruppenrichtlinienobjekt**, das Sie für die Gruppenrichtlinieneinstellungen erstellt haben, und wählen Sie **Bearbeiten**.
4. Navigieren Sie im **Gruppenrichtlinienverwaltungs-Editor** zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Remotedesktopdienste** > **Remotedesktop-Sitzungshost** > **Geräte- und Ressourcenumleitung**.
5. Aktivieren Sie die Einstellung **Zeitzonenumleitung zulassen**.

Sie können diesen Befehl auch auf dem Masterimage ausführen, um Zeitzonen umzuleiten:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fEnableTimeZoneRedirection /t REG_DWORD /d 1 /f
```

### <a name="disable-storage-sense"></a>Deaktivieren der Speicheroptimierung

Für Windows Virtual Desktop-Sitzungshosts, für die Windows 10 Enterprise oder Windows 10 Enterprise (mehrere Sitzungen) verwendet wird, empfehlen wir die Deaktivierung der Speicheroptimierung. Sie können die Speicheroptimierung im Menü „Einstellungen“ unter **Speicher** deaktivieren, wie im folgenden Screenshot gezeigt:

![Screenshot: Menü „Speicher“ unter „Einstellungen“. Die Option „Speicheroptimierung“ ist deaktiviert.](media/storagesense.png)

Sie können die Einstellung auch über die Registrierung ändern, indem Sie den folgenden Befehl ausführen:

```batch
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy /v 01 /t REG_DWORD /d 0 /f
```

### <a name="include-additional-language-support"></a>Einbinden der zusätzlichen Sprachunterstützung

In diesem Artikel wird nicht beschrieben, wie Sie die Unterstützung von Sprachen und Regionen konfigurieren. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Add languages to Windows images](https://docs.microsoft.com/windows-hardware/manufacture/desktop/add-language-packs-to-windows) (Hinzufügen von Sprachen zu Windows-Images)
- [Features on demand](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) (On-Demand-Features)
- [Language and region Features on Demand (FOD)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-language-fod) (Sprachen und Regionen: On-Demand-Features)

### <a name="other-applications-and-registry-configuration"></a>Konfiguration anderer Anwendungen und der Registrierung

In diesem Abschnitt wird die Konfiguration der Anwendungen und Betriebssysteme behandelt. Alle Konfigurationen in diesem Abschnitt erfolgen über Registrierungseinträge mit Befehlszeilen- und Registrierungstools.

>[!NOTE]
>Sie können die bewährten Methoden für die Konfiguration implementieren, indem Sie entweder Gruppenrichtlinienobjekte (GPOs) oder Registrierungsimporte verwenden. Der Administrator kann je nach den Anforderungen der Organisation eine der beiden Optionen wählen.

Führen Sie diesen Befehl aus, um unter Windows 10 Enterprise (mehrere Sitzungen) über den Feedback-Hub Telemetriedaten zu erfassen:

```batch
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection "AllowTelemetry"=dword:00000003
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection /v AllowTelemetry /d 3
```

Führen Sie den folgenden Befehl aus, um Watson-Abstürze zu beheben:

```batch
remove CorporateWerServer* from Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
```

Geben Sie die folgenden Befehle in den Registrierungs-Editor ein, um Probleme mit der Unterstützung der 5k-Auflösung zu beheben. Sie müssen die Befehle ausführen, bevor Sie den parallelen Stapel aktivieren können.

```batch
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40
```

## <a name="prepare-the-image-for-upload-to-azure"></a>Vorbereiten des Images für den Upload in Azure

Nachdem Sie die Konfiguration abgeschlossen und alle Anwendungen installiert haben, können Sie die Anleitung unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) befolgen, um das Image vorzubereiten.

Stellen Sie nach dem Vorbereiten des Images für den Upload sicher, dass die VM im Zustand „Aus“ oder „Zuordnung aufgehoben“ verbleibt.

## <a name="upload-master-image-to-a-storage-account-in-azure"></a>Hochladen eines Masterimages in ein Speicherkonto in Azure

Dieser Abschnitt gilt nur, wenn das Masterimage lokal erstellt wurde.

In der folgenden Anleitung wird beschrieben, wie Sie Ihr Masterimage in ein Azure-Speicherkonto hochladen. Falls Sie nicht bereits über ein Azure-Speicherkonto verfügen, können Sie die Anleitung in [diesem Artikel](https://code.visualstudio.com/tutorials/static-website/create-storage) verwenden, um ein Konto zu erstellen.

1. Konvertieren Sie das VM-Image (VHD) in „Fixed“ (Fest), falls Sie dies noch nicht getan haben. Wenn Sie das Image nicht in „Fixed“ (Fest) konvertieren, ist die Imageerstellung nicht erfolgreich.

2. Laden Sie die VHD in einen Blobcontainer in Ihrem Speicherkonto hoch. Sie können den Upload mit dem [Storage-Explorer-Tool](https://azure.microsoft.com/features/storage-explorer/) schnell durchführen. Weitere Informationen zum Storage-Explorer-Tool finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

    ![Screenshot: Fenster „Suchen“ im Tool Microsoft Azure Storage-Explorer Das Kontrollkästchen „Upload .vhd or vhdx files as page blobs (recommended)“ (VHD-/VHDX-Dateien als Seitenblobs hochladen (empfohlen))“ ist aktiviert.](media/897aa9a9b6acc0aa775c31e7fd82df02.png)

3. Navigieren Sie anschließend in Ihrem Browser zum Azure-Portal, und suchen Sie nach „Images“. Ihre Suche sollte Sie zur Seite **Image erstellen** führen, wie im folgenden Screenshot dargestellt:

    ![Screenshot: Seite „Image erstellen“ im Azure-Portal mit Beispielwerten für das Image](media/d3c840fe3e2430c8b9b1f44b27d2bf4f.png)

4. Nachdem Sie das Image erstellt haben, sollte eine Benachrichtigung wie im folgenden Screenshot angezeigt werden:

    ![Screenshot: Benachrichtigung „Image erfolgreich erstellt“](media/1f41b7192824a2950718a2b7bb9e9d69.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über ein Image verfügen, können Sie Hostpools erstellen oder aktualisieren. Weitere Informationen zum Erstellen und Aktualisieren von Hostpools finden Sie in den folgenden Artikeln:

- [Erstellen eines Hostpools mit einer Azure Resource Manager-Vorlage](create-host-pools-arm-template.md)
- [Tutorial: Erstellen eines Hostpools mit Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell.md)
- [Einrichten einer Benutzerprofilfreigabe für einen Hostpool](create-host-pools-user-profile.md)
- [Konfigurieren der Lastenausgleichsmethode für Windows Virtual Desktop](configure-host-pool-load-balancing.md)
