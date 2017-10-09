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
ms.date: 09/25/2017
ms.author: damaerte
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 32d4d4d10e5d8986e2dfe94430f52db8f038e245
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="troubleshooting-azure-cloud-shell"></a>Problembehandlung für Azure Cloud Shell
Zu den bekannten Lösungen für Probleme in Azure Cloud Shell gehören:

## <a name="error-400-disallowedoperation"></a>Fehler: 400 DisallowedOperation
 - **Details**: Mit einem Azure Active Directory-Abonnement können Sie keinen Speicher erstellen.
 - **Lösung**: Verwenden Sie ein Azure-Abonnement, das Speicherressourcen erstellen kann. Azure AD-Abonnements sind nicht in der Lage, Azure-Ressourcen zu erstellen.

## <a name="powershell-resolutions"></a>PowerShell-Lösungen

### <a name="no-home-directory-persistence"></a>Keine $Home-Verzeichnispersistenz
  - **Details**: Jede Anwendung (z. B.: git, vim usw.), die Daten in „$Home“ schreibt, bleibt zwischen den PowerShell-Sitzungen nicht erhalten.
  - **Lösung**: Erstellen Sie in Ihrem PowerShell-Profil einen symbolischen Link zu einem anwendungsbezogenen Ordner in `clouddrive` zu „$Home“.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>Das Drücken von STRG+C führt nicht zum Verlassen einer Cmdlet-Eingabeaufforderung.
 - **Details**: Beim Versuch, eine Cmdlet-Eingabeaufforderung zu verlassen, führt `Ctrl+C` nicht zum Verlassen der Eingabeaufforderung.
 - **Lösung**: Drücken Sie `Ctrl+C` und dann `Enter`, um die Eingabeaufforderung zu verlassen.

### <a name="gui-applications-are-not-supported"></a>GUI-Anwendungen werden nicht unterstützt.
  - **Details**: Wenn ein Benutzer eine GUI-App startet, kehrt die Eingabeaufforderung nicht zurück.  Wenn ein Benutzer z. B. `git` verwendet, um ein privates Repository zu schließen, für das die zweistufige Authentifizierung aktiviert ist, wird ein Dialogfeld für die beiden Authentifizierungscodes angezeigt.
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
 Sie können [Benutzerdefinierte Azure-Skripterweiterung][Customex] verwenden, um die Anmeldung am virtuellen Remotecomputer zum Hinzufügen der neuen Firewallregel zu vermeiden.
 Sie können das obige Skript in einer Datei speichern, z. B. `addfirerule.ps1`, und diese in Ihren Azure-Speichercontainer hochladen.
 Versuchen Sie dann den folgenden Befehl:

 ``` Powershell
 Get-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup | Set-AzureRmVMCustomScriptExtension -VMName MyVM1 -FileUri https://mystorageaccount.blob.core.windows.net/mycontainer/addfirerule.ps1 -Run 'addfirerule.ps1' -Name myextension
 ```
