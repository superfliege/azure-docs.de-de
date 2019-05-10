---
title: Erstellen von Dienstprinzipalen und Rollenzuweisungen unter dem Windows Virtual Desktop-Dienst (Vorschauversion) mit PowerShell – Azure
description: Es wird beschrieben, wie Sie mit PowerShell in Windows Virtual Desktop (Vorschauversion) Dienstprinzipale erstellen und Rollen zuweisen.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/12/2019
ms.author: helohr
ms.openlocfilehash: 93725fc9d77552d779378d0c14294a5bbb11c926
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65146142"
---
# <a name="tutorial-create-service-principals-and-role-assignments-with-powershell"></a>Tutorial: Erstellen von Dienstprinzipalen und Rollenzuweisungen mit PowerShell

Dienstprinzipale sind Identitäten, die Sie in Azure Active Directory erstellen können, um Rollen und Berechtigungen für einen bestimmten Zweck zuzuweisen. In Windows Virtual Desktop (Vorschauversion) können Sie einen Dienstprinzipal für folgende Zwecke erstellen:

- Automatisieren von bestimmten Verwaltungsaufgaben für Windows Virtual Desktop
- Verwenden als Anmeldeinformationen anstelle von MFA-Benutzern beim Ausführen einer Azure Resource Manager-Vorlage für Windows Virtual Desktop

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Dienstprinzipals in Azure Active Directory
> * Erstellen einer Rollenzuweisung in Windows Virtual Desktop
> * Anmelden an Windows Virtual Desktop mit dem Dienstprinzipal

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Dienstprinzipale und Rollenzuweisungen erstellen können, müssen Sie drei Schritte ausführen:

1. Installieren Sie das Azure AD-Modul. Führen Sie zum Installieren des Moduls PowerShell als Administrator und dann den folgenden Befehl aus:

    ```powershell
    Install-Module AzureAD
    ```

2. Führen Sie die folgenden Cmdlets aus, indem Sie die Werte in Anführungszeichen durch die für Ihre Sitzung relevanten Werte ersetzen.

    ```powershell
    $myTenantGroupName = "<my-tenant-group-name>"
    $myTenantName = "<my-tenant-name>"
    ```

3. Befolgen Sie alle Anweisungen in diesem Artikel in ein und derselben PowerShell-Sitzung. Der Vorgang ist ggf. nicht erfolgreich, wenn Sie das Fenster schließen und später wieder öffnen.

## <a name="create-a-service-principal-in-azure-active-directory"></a>Erstellen eines Dienstprinzipals in Azure Active Directory

Nachdem Sie die Voraussetzungen in Ihrer PowerShell-Sitzung erfüllt haben, können Sie die folgenden PowerShell-Cmdlets ausführen, um in Azure einen mehrinstanzenfähigen Dienstprinzipal zu erstellen.

```powershell
Import-Module AzureAD
$aadContext = Connect-AzureAD
$svcPrincipal = New-AzureADApplication -AvailableToOtherTenants $true -DisplayName "Windows Virtual Desktop Svc Principal"
$svcPrincipalCreds = New-AzureADApplicationPasswordCredential -ObjectId $svcPrincipal.ObjectId
```

## <a name="create-a-role-assignment-in-windows-virtual-desktop-preview"></a>Erstellen einer Rollenzuweisung in Windows Virtual Desktop (Vorschauversion)

Nachdem Sie nun einen Dienstprinzipal erstellt haben, können Sie ihn verwenden, um sich an Windows Virtual Desktop anzumelden. Achten Sie darauf, dass Sie sich mit einem Konto anmelden, das über Berechtigungen zum Erstellen der Rollenzuweisung verfügt.

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

Führen Sie die folgenden PowerShell-Cmdlets aus, um eine Verbindung mit Windows Virtual Desktop herzustellen und eine Rollenzuweisung für den Dienstprinzipal zu erstellen.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsContext -TenantGroupName $myTenantGroupName
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantGroupName $myTenantGroupName -TenantName $myTenantName
```

## <a name="sign-in-with-the-service-principal"></a>Anmelden mit dem Dienstprinzipal

Nachdem Sie eine Rollenzuweisung für den Dienstprinzipal erstellt haben, sollten Sie jetzt sicherstellen, dass die Anmeldung an Windows Virtual Desktop mit dem Dienstprinzipal möglich ist, indem Sie das folgende Cmdlet ausführen:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Vergewissern Sie sich nach dem Anmelden, dass alles funktioniert, indem Sie einige Windows Virtual Desktop-PowerShell-Cmdlets mit dem Dienstprinzipal testen.

## <a name="view-your-credentials-in-powershell"></a>Anzeigen Ihrer Anmeldeinformationen in PowerShell

Bevor Sie Ihre PowerShell-Sitzung beenden, sollten Sie Ihre Anmeldeinformationen anzeigen und zur späteren Verwendung notieren. Das Kennwort ist besonders wichtig, da Sie es nicht mehr abrufen können, nachdem Sie diese PowerShell-Sitzung geschlossen haben.

Hier sind die drei Anmeldeinformationen, die Sie sich notieren sollten, und die Cmdlets angegeben, die Sie zum Abrufen ausführen müssen:

- Password (Kennwort):

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Mandanten-ID:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Anwendungs-ID:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie einen Dienstprinzipal erstellen und ihn für die Anmeldung an Windows Virtual Desktop verwenden. Fahren Sie mit den Anleitungsartikeln zur Verbindungsherstellung mit Windows Virtual Desktop fort, um mehr zur Anmeldung an Windows Virtual Desktop zu erfahren.

- [Herstellen einer Verbindung über Windows 10 oder Windows 7](connect-windows-7-and-10.md)
- [Herstellen einer Verbindung über einen Webbrowser](connect-web.md)
