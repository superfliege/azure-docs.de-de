---
title: Bereitstellen des Azure Stack Development Kit | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie das Azure Stack Development Kit vorbereiten und das PowerShell-Skript zum Bereitstellen von Azure Stack Development Kit ausführen."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/17/2017
ms.author: erikje
ms.openlocfilehash: b67cabf0ecdb48f137bfcfbce95eee568a1c298d
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Bereitstellen des Azure Stack Development Kit

*Gilt für: Azure Stack Development Kit*

Um das [Azure Stack Development Kit](azure-stack-poc.md) bereitstellen zu können, müssen Sie die folgenden Schritte ausführen:

1. [Laden Sie das Bereitstellungspaket herunter](https://azure.microsoft.com/overview/azure-stack/try/?v=try), um „Cloudbuilder.vhdx“ zu erhalten.
2. [Bereiten Sie „cloudbuilder.vhdx“ vor](#prepare-the-development-kit-host), indem Sie das Skript „asdk-installer.ps1“ ausführen, um den Computer (den Host des Development Kit) zu konfigurieren, auf dem Sie das Development Kit installieren möchten. Nach diesem Schritt wird der Development Kit-Host mit „cloudbuilder.vhdx“ gestartet.
3. [Stellen Sie das Development Kit](#deploy-the-development-kit) auf dem Development Kit-Host bereit.

> [!NOTE]
> Um auch dann optimale Ergebnisse zu erzielen, wenn Sie eine nicht verbundene Azure Stack-Umgebung verwenden möchten, sollten Sie die Bereitstellung durchführen, während Sie mit dem Internet verbunden sind. Auf diese Weise kann die Evaluierungsversion von Windows Server 2016 zum Zeitpunkt der Bereitstellung aktiviert werden.
> 
> 

## <a name="download-and-extract-the-development-kit"></a>Herunterladen und Extrahieren des Development Kit
1. Bevor Sie den Download starten, stellen Sie sicher, dass Ihr Computer die folgenden Voraussetzungen erfüllt:

   * Auf dem Computer müssen mindestens 60 GB freier Speicherplatz auf dem Datenträger verfügbar sein.
   * [.NET Framework 4.6 (oder eine höhere Version)](https://aka.ms/r6mkiy) muss installiert sein.

2. [Wechseln Sie zur Seite „Erste Schritte“](https://azure.microsoft.com/overview/azure-stack/try/?v=try), geben Sie Ihre Daten ein, und klicken Sie auf **Übermitteln**.
3. Klicken Sie unter **Software herunterladen** auf **Azure Stack Development Kit**.
4. Führen Sie die heruntergeladene Datei „AzureStackDownloader.exe“ aus.
5. Führen Sie im Fenster **Downloadprogramm für Azure Stack Development Kit** die Schritte 1 bis 5 aus.
6. Nachdem der Download abgeschlossen ist, klicken Sie auf **Ausführen**, um „MicrosoftAzureStackPOC.exe“ zu starten.
7. Lesen Sie die Lizenzbedingungen und die Informationen des Self-Extractor-Assistenten, und klicken Sie dann auf **Weiter**.
8. Überprüfen Sie den Datenschutzbestimmungen-Bildschirm und die Informationen des Self-Extractor-Assistenten, und klicken Sie dann auf **Weiter**.
9. Wählen Sie das Ziel für die zu extrahierenden Dateien aus, und klicken Sie auf **Weiter**.
   * Die Standardeinstellung ist: <drive letter>:\<aktueller Ordner>\Microsoft Azure Stack
10. Überprüfen Sie den Zielspeicherort, lesen Sie die Informationen des Self-Extractor-Assistenten, und klicken Sie dann auf **Extrahieren**, um die Dateien „CloudBuilder.vhdx“ (ca. 25 GB) und „ThirdPartyLicenses.rtf“ zu extrahieren. Dieser Vorgang nimmt einige Zeit in Anspruch.

> [!NOTE]
> Nachdem Sie die Dateien extrahiert haben, können Sie die EXE- und BIN-Dateien löschen, um auf dem Computer Speicherplatz wiederherzustellen. Alternativ können Sie diese Dateien zu einem anderen Speicherort verschieben, sodass Sie die Dateien nicht erneut herunterladen müssen, wenn eine erneute Bereitstellung erforderlich ist.
> 
> 

## <a name="prepare-the-development-kit-host"></a>Vorbereiten des Development Kit-Hosts
1. Stellen Sie sicher, dass Sie physisch eine Verbindung mit dem Development Kit-Host herstellen können oder physischen Konsolenzugriff (z.B. KVM) haben. Diesen Zugriff benötigen Sie, nachdem Sie den Development Kit-Host in Schritt 13 unten neu gestartet haben.
2. Stellen Sie sicher, dass der Development Kit-Host die [Mindestanforderungen](azure-stack-deploy.md) erfüllt. Mit dem [Deployment Checker for Azure Stack Development Kit](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) (Bereitstellungsprüfer für Azure Stack Development Kit) können Sie prüfen, ob Ihre Anforderungen erfüllt sind.
3. Melden Sie sich als lokaler Administrator bei Ihrem Development Kit-Host an.
4. Kopieren Sie die CloudBuilder.vhdx-Datei in das Stammverzeichnis von Laufwerk C:\ (C:\CloudBuilder.vhdx), oder verschieben Sie sie dorthin.
5. Führen Sie das folgende Skript zum Herunterladen der Development Kit-Installationsdatei („asdk-installer.ps1“) in den Ordner „C:\AzureStack_Installer“ auf Ihrem Development Kit-Host aus.
    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
    $LocalPath = 'c:\AzureStack_Installer'

    # Create folder
    New-Item $LocalPath -Type directory

    # Download file
    Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
    ```
6. Öffnen Sie eine PowerShell-Konsole mit erhöhten Rechten, führen Sie das Skript „C:\AzureStack_Installer\asdk-installer.ps1“ aus, und klicken Sie auf **Umgebung vorbereiten**.
7. Navigieren Sie auf der Seite **Cloudbuilder-VHDX erstellen** des Installationsprogramms zur zuvor heruntergeladenen Datei „cloudbuilder.vhdx“, und wählen Sie sie aus.
8. Optional: Aktivieren Sie das Kontrollkästchen **Treiber hinzufügen**, um einen Ordner mit weiteren Treibern anzugeben, die Sie auf dem Host installieren möchten.
9. Geben Sie auf der Seite **Optionale Einstellungen** das lokale Administratorkonto für den Development Kit-Host an. Wenn Sie diese Anmeldeinformationen nicht angeben, benötigen Sie während des folgenden Installationsvorgangs KVM-Zugriff auf den Host.
10. Außerdem haben Sie die Möglichkeit, auf der Seite **Optionale Einstellungen** Folgendes festzulegen:
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

## <a name="deploy-the-development-kit"></a>Bereitstellen des Development Kit
1. Melden Sie sich als lokaler Administrator beim Development Kit-Host an. Verwenden Sie die Anmeldeinformationen, die Sie in den vorherigen Schritten angegeben haben.

    > [!IMPORTANT]
    > Für Azure Active Directory-Bereitstellungen benötigt Azure Stack entweder direkten Zugriff auf das Internet oder Zugriff über einen transparenten Proxy. Die Bereitstellung unterstützt genau einen Netzwerkadapter für das Netzwerk. Wenn Sie mehrere Netzwerkadapter haben, stellen Sie vor dem Ausführen des Bereitstellungsskripts im nächsten Abschnitt sicher, dass nur einer aktiviert ist (und alle anderen deaktiviert sind).
    
2. Öffnen Sie eine PowerShell-Konsole mit erhöhten Rechten, führen Sie das Skript „\AzureStack_Installer\asdk-installer.ps1“ (dieses befindet sich möglicherweise auf einem anderen Laufwerk in „Cloudbuilder.vhdx“), und klicken Sie auf **Installieren**.
3. Wählen Sie im Feld **Typ** die Option **Azure Cloud** oder **AD FS**.
    - **Azure Cloud**: Azure Active Directory ist der Identitätsanbieter. Verwenden Sie diesen Parameter, um ein bestimmtes Verzeichnis anzugeben, in dem das AAD-Konto über globale Administratorberechtigungen verfügt. Vollständiger Name eines AAD-Verzeichnismandanten. Beispiel: .onmicrosoft.com. 
    - **AD FS**: Der standardmäßige Stampverzeichnisdienst ist der Identitätsanbieter, das Standardkonto für die Anmeldung ist azurestackadmin@azurestack.local, und das Kennwort ist dasjenige, das Sie als Teil des Setups angegeben haben.
4. Geben Sie unter **Lokales Administratorkennwort** im Feld **Kennwort** das lokale Administratorkennwort ein (das mit dem derzeit konfigurierten lokalen Administratorkennwort übereinstimmen muss), und klicken Sie dann auf **Weiter**.
5. Wählen Sie einen für das Development Kit zu verwendenden Netzwerkadapter aus, und klicken Sie dann auf **Weiter**.
6. Wählen Sie DHCP oder statische Netzwerkkonfiguration für den virtuellen Computer BGPNAT01 aus.
    - **DHCP** (Standard): Der virtuelle Computer ruft die IP-Netzwerkkonfiguration vom DHCP-Server ab.
    - **Statisch**: Verwenden Sie diese Option nur, wenn per DHCP keine gültige IP-Adresse für Azure Stack zugewiesen werden kann. Eine statische IP-Adresse muss mit der Länge der Subnetzmaske (z.B. 10.0.0.5/24) angegeben werden.
7. Legen Sie optional die folgenden Werte fest:
    - **VLAN-ID**: Legt die VLAN-ID fest. Verwenden Sie diese Option nur, wenn für Host und AzS-BGPNAT01 eine VLAN-ID für den Zugriff auf das physische Netzwerk (und das Internet) konfiguriert werden muss. 
    - **DNS-Weiterleitung**: Ein DNS-Server wird als Teil der Azure Stack-Bereitstellung erstellt. Damit Computer innerhalb der Lösung Namen außerhalb des Stamps auflösen können, stellen Sie Ihren vorhandenen Infrastruktur-DNS-Server bereit. Der DNS-Server innerhalb des Stamps leitet Auflösungsanforderungen, die unbekannte Namen betreffen, an diesen Server weiter.
    - **Zeitserver**: Dieses Pflichtfeld dient zum Festlegen des Zeitservers und muss eine IP-Adresse enthalten. Besuchen Sie zum Ermitteln der IP-Adresse eines Zeitservers [pool.ntp.org](http:\\pool.ntp.org), oder pingen Sie „time.windows.com“. 
8. Klicken Sie auf **Weiter**.
 
9. Auf der Seite **Überprüfen von Netzwerkadaptereigenschaften** sehen Sie eine Statusanzeige. 
    - Wenn sie **Ein Update kann nicht heruntergeladen werden** anzeigt, befolgen Sie die Anweisungen auf der Seite.
    - Wenn sie **Abgeschlossen** anzeigt, klicken Sie auf **Weiter**.
10. Klicken Sie auf der Seite **Zusammenfassung** auf **Bereitstellen**.
11. Wenn Sie eine Azure Active Directory-Bereitstellung verwenden, werden Sie zur Eingabe der Anmeldeinformationen Ihres globalen Administratorkontos für Azure Active Directory aufgefordert.
12. Der Bereitstellungsprozess kann einige Stunden dauern, wobei das System automatisch einmal neu startet.
   
   > [!IMPORTANT]
   > Wenn Sie den Bereitstellungsstatus überwachen möchten, melden Sie sich als „azurestack\AzureStackAdmin“ an. Wenn Sie sich als lokaler Administrator anmelden, nachdem der Computer der Domäne beigetreten ist, wird der Bereitstellungsstatus nicht angezeigt. Führen Sie die Bereitstellung nicht erneut aus, sondern melden Sie sich stattdessen als „azurestack\AzureStackAdmin“ an, um zu überprüfen, ob sie ausgeführt wird.
   > 
   > 
   
    Wenn die Bereitstellung erfolgreich verlaufen ist, zeigt die PowerShell-Konsole an: **ABGESCHLOSSEN: Aktion 'Bereitstellung'**.
   
Wenn die Bereitstellung nicht erfolgreich verlaufen ist, können Sie das folgende PowerShell-Skript zur erneuten Ausführung aus dem gleichen, mit erhöhten Rechten versehenen PowerShell-Fenster verwenden:

```powershell
cd c:\CloudDeployment\Setup
.\InstallAzureStackPOC.ps1 -Rerun
```

Dieses Skript startet die Bereitstellung vom letzten erfolgreichen Schritt ausgehend neu.

Alternativ können Sie von Grund auf [erneut bereitstellen](azure-stack-redeploy.md).


## <a name="reset-the-password-expiration-to-180-days"></a>Zurücksetzen des Kennwortablaufs auf 180 Tage

Um sicherzustellen, dass das Kennwort für den Development Kit-Host nicht zu früh abläuft, führen Sie nach der Bereitstellung folgende Schritte aus:

So ändern Sie die Kennwortablaufrichtlinie in Powershell:
1. Führen Sie im Powershell-Fenster den folgenden Befehl aus: Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local

So ändern Sie die Kennwortablaufrichtlinie manuell:
1. Öffnen Sie auf dem Development Kit-Host die **Gruppenrichtlinienverwaltung**, und navigieren Sie zu **Gruppenrichtlinienverwaltung** – **Gesamtstruktur: azurestack.local** – **Domänen** – **azurestack.local**.
2. Klicken Sie mit der rechten Maustaste auf **Standarddomänenrichtlinie**, und klicken Sie auf **Bearbeiten**.
3. Navigieren Sie im Gruppenrichtlinienverwaltungs-Editor zu **Computerkonfiguration** – **Richtlinien** – **Windows-Einstellungen** – **Sicherheitseinstellungen** – **Kontorichtlinien** – **Kennwortrichtlinie**.
4. Doppelklicken Sie im rechten Bereich auf **Maximales Kennwortalter**.
5. Ändern Sie im Dialogfeld **Maximales Kennwortalter – Eigenschaften** den Wert **Kennwort läuft ab in** in „180“, und klicken Sie auf **OK**.


## <a name="next-steps"></a>Nächste Schritte

[Installieren von PowerShell](azure-stack-powershell-configure-quickstart.md)

[Register Azure Stack with your Azure subscription](azure-stack-register.md) (Registrieren von Azure Stack in Ihrem Azure-Abonnement)

[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)

