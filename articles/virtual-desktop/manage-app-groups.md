---
title: Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion) – Azure
description: Hier erfahren Sie, wie Sie in Azure Active Directory Mandanten mit Windows Virtual Desktop (Vorschauversion) einrichten.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: f0cdd28be8c6e7390aa26fdc2dfbf32ec5542c2d
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233910"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Tutorial: Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion)

Die Standard-App-Gruppe, die für einen neuen Hostpool für Windows Virtual Desktop (Vorschauversion) erstellt wird, veröffentlicht auch den vollständigen Desktop. Zusätzlich können Sie RemoteApp-Anwendungsgruppen für den Hostpool erstellen. In diesem Tutorial erfahren Sie, wie Sie eine RemoteApp-Gruppe erstellen und individuelle Startmenü-Apps veröffentlichen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer RemoteApp-Gruppe
> * Gewähren von Zugriff auf RemoteApps

Zur Vorbereitung müssen Sie ggf. zunächst das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

## <a name="create-a-remoteapp-group"></a>Erstellen einer RemoteApp-Gruppe

1. Führen Sie das folgende PowerShell-Cmdlet aus, um eine neue leere RemoteApp-App-Gruppe zu erstellen:

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. Optional: Vergewissern Sie sich, dass die App-Gruppe erstellt wurde. Führen Sie hierzu das folgende Cmdlet aus, um eine Liste mit allen App-Gruppen für den Hostpool anzuzeigen:

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Führen Sie das folgende Cmdlet aus, um eine Liste mit allen Startmenü-Apps im VM-Image des Hostpools abzurufen. Notieren Sie sich die Werte für **FilePath**, **IconPath** und **IconIndex** sowie weitere wichtige Informationen für die zu veröffentlichende Anwendung.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```
   
4. Führen Sie das folgende Cmdlet aus, um die Anwendung auf der Grundlage ihres App-Alias (appalias) zu installieren. „appalias“ ist in der Ausgabe von Schritt 3 enthalten.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -AppAlias <appalias>
   ```

5. Optional: Führen Sie das folgende Cmdlet aus, um eine neue RemoteApp in der in Schritt 1 erstellten Anwendungsgruppe zu veröffentlichen:

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

6. Führen Sie das folgende Cmdlet aus, um sich zu vergewissern, dass die App veröffentlicht wurde:

   ```powershell
   Get-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname>
   ```

7. Wiederholen Sie die Schritte 1 bis 5 für jede Anwendung, die Sie für diese App-Gruppe veröffentlichen möchten.
8. Führen Sie das folgende Cmdlet aus, um Benutzern Zugriff auf die RemoteApps in der App-Gruppe zu gewähren:

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname> -UserPrincipalName <userupn>
   ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie App-Gruppen erstellen, sie mit RemoteApps auffüllen und der App-Gruppe Benutzer hinzufügen. Fahren Sie mit den Anleitungsartikeln zur Verbindungsherstellung mit Windows Virtual Desktop fort, um mehr zur Anmeldung an Windows Virtual Desktop zu erfahren.

- [Connect to the Remote Desktop client on Windows 7 and Windows 10](connect-windows-7-and-10.md) (Herstellen einer Verbindung mit dem Remotedesktopclient unter Windows 7 und Windows 10)
- [Connect to the Windows Virtual Desktop Preview web client](connect-web.md) (Herstellen einer Verbindung mit dem Webclient von Windows Virtual Desktop (Vorschauversion))
