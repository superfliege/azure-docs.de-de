---
title: Erstellen eines Dienstprinzipals für Azure Stack | Microsoft-Dokumentation
description: Beschreibt das Erstellen eines Dienstprinzipals, der mit der rollenbasierten Zugriffssteuerung in Azure Resource Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2018
ms.author: mabrigg
ms.openlocfilehash: de5712fd7b48a759b366f5b9808bbbefc6e305cd
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "34010493"
---
# <a name="give-applications-access-to-azure-stack-resources-by-creating-service-principals"></a>Gewähren des Anwendungszugriffs auf Azure Stack-Ressourcen durch Erstellen von Dienstprinzipalen

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können einer Anwendung Zugriff auf Azure Stack-Ressourcen gewähren, indem Sie einen Dienstprinzipal erstellen, der Azure Resource Manager verwendet. Mit einem Dienstprinzipal können Sie mithilfe der [rollenbasierten Zugriffssteuerung](azure-stack-manage-permissions.md) bestimmte Berechtigungen delegieren.

Es wird empfohlen, Dienstprinzipale für Ihre Anwendungen zu verwenden. Der Ansatz mit Dienstprinzipalen ist dem Ausführen einer App mit Ihren Anmeldeinformationen aus folgenden Gründen vorzuziehen:

* Sie können dem Dienstprinzipal Berechtigungen zuweisen, die sich von Ihren eigenen Kontoberechtigungen unterscheiden. In der Regel sind die Berechtigungen eines Dienstprinzipals genau auf die Aufgaben der App beschränkt.
* Sie müssen keine Anmeldeinformationen für die App ändern, wenn sich Ihre Rolle oder Zuständigkeiten ändern.
* Sie können ein Zertifikat verwenden, um die Authentifizierung beim Ausführen eines unbeaufsichtigten Skripts zu automatisieren.

## <a name="example-scenario"></a>Beispielszenario

Sie verfügen über eine App für die Konfigurationsverwaltung, die Azure-Ressourcen mithilfe von Azure Resource Manager inventarisieren muss. Sie können einen Dienstprinzipal erstellen und ihm die Rolle „Leser“ zuweisen. Mit dieser Rolle erhält die App schreibgeschützten Zugriff auf Azure-Ressourcen.

## <a name="getting-started"></a>Erste Schritte

Führen Sie die Schritte in diesem Artikel für folgende Aufgaben aus:

* Erstellen eines Dienstprinzipals für Ihre App
* Registrieren Ihrer App und Erstellen eines Authentifizierungsschlüssels
* Zuweisen Ihrer App zu einer Rolle

Die Konfiguration von Active Directory für Azure Stack bestimmt die Vorgehensweise zur Erstellung des Dienstprinzipals. Wählen Sie eine der folgenden Optionen:

* Erstellen eines Dienstprinzipals für [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad)
* Erstellen eines Dienstprinzipals für [Active Directory-Verbunddienste (Azure Directory Federation Services, AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs)

Die Schritte zum Zuweisen eines Dienstprinzipals zu einer Rolle sind für Azure AD und AD FS identisch. Nach der Erstellung des Dienstprinzipals können Sie durch Zuweisung zu einer Rolle [Berechtigungen delegieren](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-a-service-principal-for-azure-ad"></a>Erstellen eines Dienstprinzipals für Azure AD

Wenn Azure Stack Azure AD als Identitätsspeicher nutzt, können Sie mit dem Azure-Portal anhand der gleichen Schritte wie in Azure einen Dienstprinzipal erstellen.

>[!NOTE]
Stellen Sie vor dem Erstellen eines Dienstprinzipals sicher, dass Sie über die [erforderlichen Azure AD-Berechtigungen](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) verfügen.

### <a name="create-service-principal"></a>Erstellen eines Dienstprinzipals

So erstellen Sie einen Dienstprinzipal für Ihre Anwendung

1. Melden Sie sich über das [Azure-Portal](https://portal.azure.com) bei Ihrem Azure-Konto an.
2. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Hinzufügen**.
3. Geben Sie einen Namen und eine URL für die Anwendung an. Wählen Sie als Typ für die zu erstellende Anwendung entweder **Web-App/API** oder **Nativ** aus. Wählen Sie nach dem Festlegen der Werte **Erstellen** aus.

### <a name="get-credentials"></a>Abrufen von Anmeldeinformationen

Beim programmgesteuerten Anmelden verwenden Sie die ID für Ihre Anwendung und einen Authentifizierungsschlüssel. Diese Werte erhalten Sie wie folgt:

1. Wählen Sie in Active Directory unter **App Registrierungen** Ihre Anwendung aus.

2. Kopieren Sie die **Anwendungs-ID**, und speichern Sie sie in Ihrem Anwendungscode. In den [Beispielanwendungen](#sample-applications) wird für **Anwendungs-ID** der Begriff **Client-ID** verwendet.

     ![Anwendungs-ID für die Anwendung](./media/azure-stack-create-service-principal/image12.png)
3. Wählen Sie zum Generieren eines Authentifizierungsschlüssels die Option **Schlüssel** aus.

4. Geben Sie eine Beschreibung des Schlüssels und eine Dauer für den Schlüssel ein. Wählen Sie dann die Option **Schließen**.

>[!IMPORTANT]
Wenn Sie den Schlüssel gespeichert haben, wird unter **WERT** der Schlüssel angezeigt. Notieren Sie diesen Wert, da Sie ihn später nicht mehr abrufen können. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

![Schlüsselwertwarnung für den gespeicherten Schlüssel](./media/azure-stack-create-service-principal/image15.png)

Der letzte Schritt besteht im [Zuweisen einer Rolle zu Ihrer Anwendung](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>Erstellen eines Dienstprinzipals für AD FS

Wenn Sie Azure Stack mit AD FS als Identitätsspeicher bereitgestellt haben, können Sie PowerShell für die folgenden Aufgaben verwenden:

* Erstellen eines Dienstprinzipals
* Zuweisen eines Dienstprinzipals zu einer Rolle
* Anmelden mithilfe der Identität des Dienstprinzipals

### <a name="before-you-begin"></a>Voraussetzungen

[Laden Sie die erforderlichen Azure Stack-Tools auf Ihren lokalen Computer herunter.](azure-stack-powershell-download.md)

### <a name="import-the-identity-powershell-module"></a>Importieren des PowerShell-Moduls „Identity“

Navigieren Sie zum Downloadordner für die Azure Stack-Tools, und importieren Sie das PowerShell-Modul „Identity“ mithilfe des folgenden Befehls:

```PowerShell
Import-Module .\Identity\AzureStack.Identity.psm1
```

Beim Importieren des Moduls „Identity“ erhalten Sie ggf. den folgenden Fehler: „AzureStack.Connect.psm1 is not digitally signed. The script will not execute on the system“ (AzureStack.Connect.psm1 ist nicht digital signiert. Das Skript wird auf dem System nicht ausgeführt.)

Sie müssen die Ausführungsrichtlinie zum Zulassen der Skriptausführung konfigurieren, um dieses Problem zu beheben. Um die Ausführungsrichtlinie festzulegen, führen Sie den folgenden Befehl in einer PowerShell-Sitzung mit erhöhten Rechten aus:

```PowerShell
Set-ExecutionPolicy Unrestricted
```

### <a name="create-the-service-principal"></a>Erstellen des Dienstprinzipals

Sie können einen Dienstprinzipal erstellen, indem Sie den folgenden Befehl ausführen. Achten Sie hierbei darauf, dass Sie den Parameter **DisplayName** aktualisieren:

```powershell
$servicePrincipal = New-AzSADGraphServicePrincipal `
 -DisplayName "<YourServicePrincipalName>" `
 -AdminCredential $(Get-Credential) `
 -AdfsMachineName "AZS-ADFS01" `
 -Verbose

```

### <a name="assign-a-role"></a>Zuweisen einer Rolle

Nach dem Erstellen des Dienstprinzipals müssen Sie [ihn einer Rolle zuweisen](azure-stack-create-service-principals.md#assign-role-to-service-principal).

### <a name="sign-in-using-powershell"></a>Anmelden mithilfe von PowerShell

Sie können sich mit dem folgenden Befehl bei Azure Stack anmelden. Aktualisieren Sie dabei den Parameter **EnvironmentName** mit dem Namen Ihrer App:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ApplicationId `
 -TenantId $directoryTenantId
```

## <a name="assign-the-service-principal-to-a-role"></a>Zuweisen des Dienstprinzipals zu einer Rolle

Um auf Ressourcen in Ihrem Abonnement zuzugreifen, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](../../role-based-access-control/built-in-roles.md).

>[!NOTE]
Sie können den Bereich der Rolle auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Beispiel: Eine App mit der Rolle „Leser“ für eine Ressourcengruppe kann alle in einer Ressourcengruppe enthaltenen Ressourcen lesen.

Verwenden Sie die folgenden Schritte als Leitfaden für das Zuweisen einer Rolle zu einem Dienstprinzipal.

1. Navigieren Sie im Azure Stack-Portal zur Bereichsebene, der Sie die Anwendung zuweisen möchten. Um z.B. einer Gruppe im Abonnementkontext eine Rolle zuzuweisen, wählen Sie **Abonnements** aus.

2. Wählen Sie das Abonnement aus, dem die Anwendung zugewiesen werden soll. In diesem Beispiel wird das Abonnement für Visual Studio Enterprise verwendet.

     ![Auswählen des Visual Studio Enterprise-Abonnements für die Zuweisung](./media/azure-stack-create-service-principal/image16.png)

3. Wählen Sie für das Abonnement die Option **Zugriffssteuerung (IAM)**.

     ![Auswählen der Zugriffssteuerung](./media/azure-stack-create-service-principal/image17.png)

4. Wählen Sie **Hinzufügen**.

5. Wählen Sie die Rolle aus, die Sie der Anwendung zuweisen möchten.

6. Suchen Sie nach Ihrer Anwendung, und wählen Sie sie aus.

7. Wählen Sie **OK** aus, um das Zuweisen der Rolle abzuschließen. Ihre Anwendung wird in der Liste der Benutzer angezeigt, die einer Rolle für diesen Bereich zugewiesen sind.

Nachdem Sie nun einen Dienstprinzipal erstellt und eine Rolle zugewiesen haben, kann Ihre Anwendung auf Azure Stack-Ressourcen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)
