---
title: Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion) – Azure
description: Hier erfahren Sie, wie Sie in Azure Active Directory Mandanten mit Windows Virtual Desktop (Vorschauversion) einrichten.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 2baabe6837d25cce5e9f5d9e1071af8417fe5f4d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401861"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Tutorial: Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion)

Die Standard-App-Gruppe, die für einen neuen Hostpool für Windows Virtual Desktop (Vorschauversion) erstellt wird, veröffentlicht auch den vollständigen Desktop. Zusätzlich können Sie RemoteApp-Anwendungsgruppen für den Hostpool erstellen. In diesem Tutorial erfahren Sie, wie Sie eine RemoteApp-Gruppe erstellen und individuelle Startmenü-Apps veröffentlichen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer RemoteApp-Gruppe
> * Gewähren von Zugriff auf RemoteApps

Zur Vorbereitung müssen Sie ggf. zunächst das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

## <a name="create-a-remoteapp-group"></a>Erstellen einer RemoteApp-Gruppe

1. Führen Sie das folgende PowerShell-Cmdlet aus, um eine neue leere RemoteApp-Gruppe zu erstellen:

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. Optional: Vergewissern Sie sich, dass die Anwendungsgruppe erstellt wurde. Führen Sie hierzu das folgende Cmdlet aus, um eine Liste mit allen Anwendungsgruppen für den Hostpool anzuzeigen:

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Führen Sie das folgende Cmdlet aus, um eine Liste mit allen Startmenü-Apps im VM-Image des Hostpools abzurufen. Notieren Sie sich die Werte für **FilePath**, **IconPath** und **IconIndex** sowie weitere wichtige Informationen für die zu veröffentlichende Anwendung.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```

4. Führen Sie das folgende Cmdlet aus, um eine neue RemoteApp in der in Schritt 1 erstellten Anwendungsgruppe zu veröffentlichen:

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

5. Optional: Führen Sie das folgende Cmdlet aus, um die Anwendung auf der Grundlage von „appalias“ zu installieren. „appalias“ ist in der Ausgabe von Schritt 3 enthalten.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> <remoteappname> -AppAlias <appalias>
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

Nach Erstellung der App-Gruppen können Sie Dienstprinzipale erstellen und Ihren Benutzern Rollen zuweisen. Eine entsprechende Anleitung finden Sie im Tutorial zur Erstellung von Dienstprinzipalen und Rollenzuweisungen mit PowerShell.

> [!div class="nextstepaction"]
> [Erstellen von Dienstprinzipalen und Rollenzuweisungen mit PowerShell](create-service-principal-role-powershell.md)
