---
title: "Problembehandlung für Azure Cloud Shell (Vorschau) | Microsoft-Dokumentation"
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
ms.date: 11/2/2017
ms.author: damaerte
ms.openlocfilehash: 89d5d8df9327c6136fbd00078f6a34f78d85032e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="troubleshooting-azure-cloud-shell"></a>Problembehandlung für Azure Cloud Shell

Zu den bekannten Lösungen für Probleme in Azure Cloud Shell gehören:

## <a name="general-resolutions"></a>Allgemeine Lösungen

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Speicherdialogfeld – Fehler: 403 RequestDisallowedByPolicy
- **Details**: Das Erstellen eines Speicherkontos über Cloud Shell ist aufgrund einer von Ihrem Administrator platzierten Azure-Richtlinie nicht erfolgreich. Zu den Fehlermeldung zählt: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Auflösung**: Wenden Sie sich an Ihren Azure-Administrator, um die Azure-Richtlinie, die die Speichererstellung verweigert, zu entfernen oder zu aktualisieren.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Speicherdialogfeld – Fehler: 400 DisallowedOperation
 - **Details**: Mit einem Azure Active Directory-Abonnement können Sie keinen Speicher erstellen.
 - **Lösung**: Verwenden Sie ein Azure-Abonnement, das Speicherressourcen erstellen kann. Azure AD-Abonnements sind nicht in der Lage, Azure-Ressourcen zu erstellen.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminalausgabe – Fehler: Fehler beim Herstellen der Verbindung mit Terminal: Websocket kann nicht eingerichtet werden. Drücken Sie `Enter`, um erneut eine Verbindung herzustellen.
 - **Details**: Cloud Shell erfordert, dass eine Websocketverbindung mit der Cloud Shell-Infrastruktur hergestellt werden kann.
 - **Auflösung**: Überprüfen Sie, ob Sie Ihre Netzwerkeinstellungen so konfiguriert haben, dass HTTPS- und Websocketanforderungen an Domänen unter *.console.azure.com gesendet werden können.

## <a name="bash-resolutions"></a>Bashlösungen

### <a name="cannot-run-az-login"></a>Ausführung von „az login“ ist nicht möglich.

- **Details**: Die Ausführung von `az login` funktioniert nicht, da Sie bereits unter dem Konto authentifiziert sind, das zur Anmeldung bei Cloud Shell oder Azure-Portal verwendet wird.
- **Auflösung**: Nutzen Sie Ihr Konto, das zum Anmelden oder Abmelden verwendet wird, und authentifizieren Sie sich erneut mit dem gewünschten Azure-Konto.

### <a name="cannot-run-the-docker-daemon"></a>Der Docker-Daemon kann nicht ausgeführt werden.

- **Details**: Cloud Shell nutzt einen Container zum Hosten Ihrer Shellumgebung, daher ist die Ausführung des Daemons nicht zulässig.
- **Auflösung**: Nutzen Sie das standardmäßig installierte [Docker Machine](https://docs.docker.com/machine/overview/), um Docker-Container von einem Docker-Remotehost aus zu verwalten.

## <a name="powershell-resolutions"></a>PowerShell-Lösungen

### <a name="no-home-directory-persistence"></a>Keine $Home-Verzeichnispersistenz

- **Details**: Daten, die eine Anwendung (z.B. git, vim usw.) in `$Home` schreibt, bleiben zwischen den PowerShell-Sitzungen nicht erhalten.
- **Lösung**: Erstellen Sie in Ihrem PowerShell-Profil einen symbolischen Link zu einem anwendungsbezogenen Ordner in `clouddrive` zu „$Home“.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>Das Drücken von STRG+C führt nicht zum Verlassen einer Cmdlet-Eingabeaufforderung.

- **Details**: Beim Versuch, eine Cmdlet-Eingabeaufforderung zu verlassen, führt `Ctrl+C` nicht zum Verlassen der Eingabeaufforderung.
- **Lösung**: Drücken Sie `Ctrl+C` und dann `Enter`, um die Eingabeaufforderung zu verlassen.

### <a name="gui-applications-are-not-supported"></a>GUI-Anwendungen werden nicht unterstützt.

- **Details**: Wenn ein Benutzer eine GUI-App startet, kehrt die Eingabeaufforderung nicht zurück. Wenn ein Benutzer z.B. ein privates GitHub-Repository klont, für das die zweistufige Authentifizierung aktiviert ist, wird ein Dialogfeld zur Ausführung der zweistufigen Authentifizierung angezeigt.
- **Lösung**: Drücken Sie `Ctrl+C`, um den Befehl zu beenden.

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
