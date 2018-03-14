---
title: "Problembehandlung für Azure Cloud Shell | Microsoft-Dokumentation"
description: "Problembehandlung für Azure Cloud Shell"
services: azure
documentationcenter: 
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: damaerte
ms.openlocfilehash: 52ee832b643af573d8236b266df17d36e485ead2
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Problembehandlung und Einschränkungen bei Azure Cloud Shell

Zu den bekannten Lösungen für die Behandlung von Problemen in Azure Cloud Shell gehören Folgende:

## <a name="general-troubleshooting"></a>Allgemeine Problembehandlung

### <a name="early-timeouts-in-firefox"></a>Frühe Timeouts in Firefox
- **Details:** Cloud Shell nutzt einen offenen Websocket für die Weitergabe der Ein- und Ausgaben an den Browser. Firefox verfügt über vordefinierte Richtlinien, die den Websocket vorzeitig schließen und damit frühe Timeouts in Cloud Shell verursachen können.
- **Lösung:** Öffnen Sie Firefox, und navigieren Sie im Feld „URL“ zu „about:config“. Suchen Sie nach „network.websocket.timeout.ping.request“, und ändern Sie den Wert von 0 in 10.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Speicherdialogfeld – Fehler: 403 RequestDisallowedByPolicy
- **Details**: Das Erstellen eines Speicherkontos über Cloud Shell ist aufgrund einer von Ihrem Administrator platzierten Azure-Richtlinie nicht erfolgreich. Zu den Fehlermeldung zählt: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Auflösung**: Wenden Sie sich an Ihren Azure-Administrator, um die Azure-Richtlinie, die die Speichererstellung verweigert, zu entfernen oder zu aktualisieren.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Speicherdialogfeld – Fehler: 400 DisallowedOperation
 - **Details**: Mit einem Azure Active Directory-Abonnement können Sie keinen Speicher erstellen.
 - **Lösung**: Verwenden Sie ein Azure-Abonnement, das Speicherressourcen erstellen kann. Azure AD-Abonnements sind nicht in der Lage, Azure-Ressourcen zu erstellen.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminalausgabe – Fehler: Fehler beim Herstellen der Verbindung mit Terminal: Websocket kann nicht eingerichtet werden. Drücken Sie `Enter`, um erneut eine Verbindung herzustellen.
 - **Details**: Cloud Shell erfordert, dass eine Websocketverbindung mit der Cloud Shell-Infrastruktur hergestellt werden kann.
 - **Auflösung**: Überprüfen Sie, ob Sie Ihre Netzwerkeinstellungen so konfiguriert haben, dass HTTPS- und Websocketanforderungen an Domänen unter *.console.azure.com gesendet werden können.

## <a name="bash-troubleshooting"></a>Problembehandlung bei Bash

### <a name="cannot-run-az-login"></a>Ausführung von „az login“ ist nicht möglich.

- **Details**: Die Ausführung von `az login` funktioniert nicht, da Sie bereits unter dem Konto authentifiziert sind, das zur Anmeldung bei Cloud Shell oder Azure-Portal verwendet wird.
- **Auflösung**: Nutzen Sie Ihr Konto, das zum Anmelden oder Abmelden verwendet wird, und authentifizieren Sie sich erneut mit dem gewünschten Azure-Konto.

### <a name="cannot-run-the-docker-daemon"></a>Der Docker-Daemon kann nicht ausgeführt werden.

- **Details**: Cloud Shell nutzt einen Container zum Hosten Ihrer Shellumgebung, daher ist die Ausführung des Daemons nicht zulässig.
- **Auflösung**: Nutzen Sie das standardmäßig installierte [Docker Machine](https://docs.docker.com/machine/overview/), um Docker-Container von einem Docker-Remotehost aus zu verwalten.

## <a name="powershell-troubleshooting"></a>Problembehandlung bei PowerShell

### <a name="no-home-directory-persistence"></a>Keine $Home-Verzeichnispersistenz

- **Details**: Daten, die eine Anwendung (z.B. git, vim usw.) in `$Home` schreibt, bleiben zwischen den PowerShell-Sitzungen nicht erhalten.
- **Lösung**: Erstellen Sie in Ihrem PowerShell-Profil einen symbolischen Link zu einem anwendungsbezogenen Ordner in `clouddrive` zu „$Home“.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>Das Drücken von STRG+C führt nicht zum Verlassen einer Cmdlet-Eingabeaufforderung.

- **Details**: Beim Versuch, eine Cmdlet-Eingabeaufforderung zu verlassen, führt `Ctrl+C` nicht zum Verlassen der Eingabeaufforderung.
- **Lösung**: Drücken Sie `Ctrl+C` und dann `Enter`, um die Eingabeaufforderung zu verlassen.

### <a name="gui-applications-are-not-supported"></a>GUI-Anwendungen werden nicht unterstützt.

- **Details**: Wenn ein Benutzer eine GUI-App startet, kehrt die Eingabeaufforderung nicht zurück. Wenn ein Benutzer z.B. ein privates GitHub-Repository klont, für das die zweistufige Authentifizierung aktiviert ist, wird ein Dialogfeld zur Ausführung der zweistufigen Authentifizierung angezeigt.  
- **Lösung**: Schließen Sie die Shell, und öffnen Sie sie wieder.

### <a name="get-help--online-does-not-open-the-help-page"></a>„Get-Help -online“ öffnet nicht die Hilfeseite.

- **Details**: Wenn ein Benutzer `Get-Help Find-Module -online` eingibt, wird eine Fehlermeldung wie die Folgende angezeigt: `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Lösung**: Kopieren Sie die URL, und öffnen Sie sie manuell in Ihrem Browser.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Problembehandlung bei der Remoteverwaltung von virtuellen Azure-Computern

- **Details**: Aufgrund der Standardeinstellungen der Windows-Firewall für WinRM wird dem Benutzer möglicherweise der folgende Fehler angezeigt: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Lösung**: Stellen Sie sicher, dass Ihr virtueller Computer ausgeführt wird. Sie können `Get-AzureRmVM -Status` ausführen, um den Status des virtuellen Computers zu ermitteln.  Als Nächstes fügen Sie eine neue Firewallregel zum virtuellen Remotecomputer hinzu, um WinRM-Verbindungen aus beliebigen Subnetzen zu ermöglichen. Beispiel:

 ``` Powershell
 New-NetFirewallRule -Name 'WINRM-HTTP-In-TCP-PSCloudShell' -Group 'Windows Remote Management' -Enabled True -Protocol TCP -LocalPort 5985 -Direction Inbound -Action Allow -DisplayName 'Windows Remote Management - PSCloud (HTTP-In)' -Profile Public
 ```
 Sie können die [benutzerdefinierte Azure-Skripterweiterung](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript) verwenden, um die Anmeldung am virtuellen Remotecomputer zum Hinzufügen der neuen Firewallregel zu vermeiden.
 Sie können das obige Skript in einer Datei speichern, z. B. `addfirerule.ps1`, und diese in Ihren Azure-Speichercontainer hochladen.
 Versuchen Sie dann den folgenden Befehl:

 ``` Powershell
 Get-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup | Set-AzureRmVMCustomScriptExtension -VMName MyVM1 -FileUri https://mystorageaccount.blob.core.windows.net/mycontainer/addfirerule.ps1 -Run 'addfirerule.ps1' -Name myextension
 ```

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir` speichert das Ergebnis auf dem Azure-Laufwerk zwischen.

- **Details**: Das Ergebnis von `dir` wird auf dem Azure-Laufwerk zwischengespeichert.
- **Lösung**: Führen Sie nach dem Erstellen oder Entfernen einer Ressourcen in der Azure-Laufwerkansicht zum Aktualisieren `dir -force` aus.

## <a name="general-limitations"></a>Allgemeine Einschränkungen
Für Azure Cloud Shell gelten die folgenden bekannten Einschränkungen:

### <a name="system-state-and-persistence"></a>Systemstatus und Persistenz

Der Computer mit Ihrer Cloud Shell-Sitzung besteht nur vorübergehend und wird recycelt, wenn die Sitzung 20 Minuten lang inaktiv ist. Für Cloud Shell muss eine Azure-Dateifreigabe bereitgestellt werden. Daher muss Ihr Abonnement Speicherressourcen für den Zugriff auf Cloud Shell einrichten können. Weitere Überlegungen:

* Mit eingebundenem Speicher werden nur Änderungen im `clouddrive`-Verzeichnis gespeichert. In Bash bleibt Ihr `$Home`-Verzeichnis ebenfalls erhalten.
* Azure-Dateifreigaben können nur innerhalb Ihrer [zugewiesenen Region](persisting-shell-storage.md#mount-a-new-clouddrive) eingebunden werden.
  * Führen Sie in Bash `env` aus, um für Ihre Region die Einstellung `ACC_LOCATION` zu ermitteln.
* Azure Files unterstützt nur Konten mit lokal redundantem Speicher und georedundantem Speicher.

### <a name="browser-support"></a>Browserunterstützung

Cloud Shell unterstützt die aktuellen Versionen von Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox und Apple Safari. Safari im privaten Modus wird nicht unterstützt.

### <a name="copy-and-paste"></a>Kopieren und Einfügen

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Für einen bestimmten Benutzer kann nur eine Shell aktiv sein.

Benutzer können jeweils nur eine Art von Shell zu einem bestimmten Zeitpunkt starten, entweder **Bash** oder **PowerShell**. Möglicherweise führen Sie jedoch mehrere Instanzen von Bash oder PowerShell gleichzeitig aus. Der Wechsel zwischen Bash oder PowerShell führt dazu, dass Cloud Shell neu gestartet wird, wodurch bestehende Sitzungen beendet werden.

### <a name="usage-limits"></a>Usage limits (Nutzungseinschränkungen)

Cloud Shell ist für interaktive Anwendungsfälle konzipiert. Daher werden lange Sitzungen ohne Interaktion ohne Vorwarnung beendet.

## <a name="bash-limitations"></a>Bash-Einschränkungen

### <a name="user-permissions"></a>Benutzerberechtigungen

Berechtigungen werden als reguläre Benutzer ohne sudo-Zugriff festgelegt. Installationen außerhalb des Verzeichnisses `$Home` werden nicht gespeichert.

### <a name="editing-bashrc"></a>Bearbeiten von „.bashrc“

Gehen Sie bei der Bearbeitung von „.bashrc“ vorsichtig vor, da sonst unerwartete Fehler in Cloud Shell auftreten können.

## <a name="powershell-limitations"></a>PowerShell-Einschränkungen

### <a name="slow-startup-time"></a>Langsame Startzeit

Die Initialisierung von PowerShell in Azure Cloud Shell (Vorschauversion) kann in der Vorschauphase bis zu 60 Sekunden dauern.

### <a name="default-file-location-when-created-from-azure-drive"></a>Standard-Dateispeicherort beim Erstellen vom Azure-Laufwerk:

Mithilfe von PowerShell-Cmdlets können Benutzer keine Dateien unter dem Azure-Laufwerk erstellen. Wenn Benutzer neue Dateien mit anderen Tools wie vim oder nano erstellen, werden die Dateien standardmäßig im Ordner „C:\Users“ gespeichert. 

### <a name="gui-applications-are-not-supported"></a>GUI-Anwendungen werden nicht unterstützt.

Wenn der Benutzer einen Befehl (etwa `Connect-AzureAD` oder `Login-AzureRMAccount`) ausführt, mit dem ein Windows-Dialogfeld erstellt wird, wird etwa folgende Fehlermeldung angezeigt: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.
