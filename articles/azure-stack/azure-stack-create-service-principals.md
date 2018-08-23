---
title: Erstellen eines Dienstprinzipals für Azure Stack | Microsoft-Dokumentation
description: Beschreibt das Erstellen eines neuen Dienstprinzipals, der mit der rollenbasierten Zugriffssteuerung in Azure Resource Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann.
services: azure-resource-manager
documentationcenter: na
author: sethmanheim
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2018
ms.author: sethm
ms.openlocfilehash: f7233d6a27b9ec3d58f33f7032bbec7a646d24f7
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42366118"
---
# <a name="provide-applications-access-to-azure-stack"></a>Bereitstellen des Anwendungszugriffs auf Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Wenn eine Anwendung Zugriff benötigt, um Ressourcen über Azure Resource Manager in Azure Stack bereitzustellen oder zu konfigurieren, erstellen Sie einen Dienstprinzipal. Dies ist ein Objekt mit Anmeldeinformationen für Ihre Anwendung.  Sie können für diesen Dienstprinzipal dann nur die erforderlichen Berechtigungen delegieren.  

Beispiel: Sie verfügen über ein Tool für die Konfigurationsverwaltung, das Azure Resource Manager zum Inventarisieren von Azure-Ressourcen einsetzt.  In diesem Szenario können Sie einen Dienstprinzipal erstellen, dem Dienstprinzipal die Rolle „Leser“ gewähren und das Tool für die Konfigurationsverwaltung auf den schreibgeschützten Zugriff beschränken. 

Der Ansatz mit Dienstprinzipalen ist dem Ausführen der App mit Ihren Anmeldeinformationen aus folgenden Gründen vorzuziehen:

* Sie können dem Dienstprinzipal Berechtigungen zuweisen, die sich von Ihren eigenen Kontoberechtigungen unterscheiden. In der Regel sind diese Berechtigungen genau auf die Aufgaben der App beschränkt.
* Sie müssen keine Anmeldeinformationen für die App ändern, wenn sich Ihre Zuständigkeiten ändern.
* Sie können ein Zertifikat verwenden, um die Authentifizierung beim Ausführen eines unbeaufsichtigten Skripts zu automatisieren.  

## <a name="getting-started"></a>Erste Schritte

Je nachdem, wie Sie Azure Stack bereitgestellt haben, beginnen Sie mit dem Erstellen eines Dienstprinzipals.  In diesem Dokument wird das Erstellen eines Dienstprinzipals für [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) und [Active Directory-Verbunddienste (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs) beschrieben.  Nachdem Sie den Dienstprinzipal erstellt haben, können Sie einige Schritte ausführen, die für AD FS und Azure Active Directory gleich sind, um für die Rolle [Berechtigungen zu delegieren](azure-stack-create-service-principals.md#assign-role-to-service-principal).     

## <a name="create-service-principal-for-azure-ad"></a>Erstellen des Dienstprinzipals für Azure AD

Wenn Sie Azure Stack mit Azure AD als Identitätsspeicher bereitgestellt haben, können Sie Dienstprinzipale genauso wie für Azure erstellen.  In diesem Abschnitt erfahren Sie, wie die Schritte über das Portal ausgeführt werden.  Vergewissern Sie sich vorher, ob Sie über die [erforderlichen Azure AD-Berechtigungen](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) verfügen.

### <a name="create-service-principal"></a>Erstellen eines Dienstprinzipals
In diesem Abschnitt erstellen Sie eine Anwendung (Dienstprinzipal) in Azure AD, die Ihre Anwendung repräsentiert.

1. Melden Sie sich über das [Azure-Portal](https://portal.azure.com) bei Ihrem Azure-Konto an.
2. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Hinzufügen**.   
3. Geben Sie einen Namen und eine URL für die Anwendung an. Wählen Sie als Typ für die zu erstellende Anwendung entweder **Web-App/API** oder **Nativ** aus. Wählen Sie nach dem Festlegen der Werte **Erstellen** aus.

Sie haben einen Dienstprinzipal für Ihre Anwendung erstellt.

### <a name="get-credentials"></a>Abrufen von Anmeldeinformationen
Beim programmgesteuerten Anmelden verwenden Sie die ID für Ihre Anwendung und einen Authentifizierungsschlüssel für eine Web-App/API. Führen Sie die folgenden Schritte aus, um diese Werte abzurufen:

1. Wählen Sie in Active Directory unter **App Registrierungen** Ihre Anwendung aus.

2. Kopieren Sie die **Anwendungs-ID**, und speichern Sie sie in Ihrem Anwendungscode. Die Anwendungen im Abschnitt [Beispielanwendungen](#sample-applications) verweisen auf diesen Wert als Client-ID.

     ![CLIENT-ID](./media/azure-stack-create-service-principal/image12.png)
3. Klicken Sie zum Generieren eines Authentifizierungsschlüssels für eine Web-App/API auf **Einstellungen** > **Schlüssel**. 

4. Geben Sie eine Beschreibung des Schlüssels und eine Dauer für den Schlüssel ein. Wählen Sie dann die Option **Schließen**.

Nach dem Speichern des Schlüssels wird der Wert des Schlüssels angezeigt. Kopieren Sie diesen Wert jetzt, da Sie ihn später nicht mehr abrufen können. Sie geben den Schlüsselwert zusammen mit der Anwendungs-ID ein, um die Anmeldung als Anwendung durchzuführen. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

![gespeicherter Schlüssel](./media/azure-stack-create-service-principal/image15.png)


Fahren Sie nach Abschluss des Vorgangs mit dem [Zuweisen einer Rolle für Ihre Anwendung](azure-stack-create-service-principals.md#assign-role-to-service-principal) fort.

## <a name="create-service-principal-for-ad-fs"></a>Erstellen eines Dienstprinzipals für AD FS
Wenn Sie Azure Stack mit AD FS bereitgestellt haben, können Sie PowerShell verwenden, um einen Dienstprinzipal zu erstellen, eine Rolle für den Zugriff zuzuweisen und die Anmeldung über PowerShell mit dieser Identität durchzuführen.

Das Skript wird über den privilegierten Endpunkt auf einem virtuellen ERCS-Computer ausgeführt.


Anforderungen:
- Ein Zertifikat ist erforderlich.

**Parameter**

Die folgenden Informationen sind als Eingabe für die Automatisierungsparameter erforderlich:


|Parameter|BESCHREIBUNG|Beispiel|
|---------|---------|---------|
|NAME|Name des SPN-Kontos|MyAPP|
|ClientCertificates|Array von Zertifikatobjekten|X.509-Zertifikat|
|ClientRedirectUris<br>(Optional)|Umleitungs-URI der Anwendung|         |

**Beispiel**

1. Öffnen Sie eine Windows PowerShell-Sitzung mit erhöhten Rechten, und führen Sie die folgenden Befehle aus:

   > [!NOTE]
   > In diesem Beispiel wird ein selbstsigniertes Zertifikat erstellt. Wenn Sie diese Befehle in einer Produktionsbereitstellung ausführen, rufen Sie mit „Get-Certificate“ das Zertifikatsobjekt für das Zertifikat ab, das Sie verwenden möchten.

   ```PowerShell  
    # Credential for accessing the ERCS PrivilegedEndpoint typically domain\cloudadmin
    $creds = Get-Credential

    # Creating a PSSession to the ERCS PrivilegedEndpoint
    $session = New-PSSession -ComputerName <ERCS IP> -ConfigurationName PrivilegedEndpoint -Credential $creds

    # This produces a self signed cert for testing purposes.  It is prefered to use a managed certificate for this.
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=<yourappname>" -KeySpec KeyExchange

    $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name '<yourappname>' -ClientCertificates $using:cert}
    $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }
    $session|remove-pssession

    # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

    # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

    # TenantID for the stamp. We will read this from the AzureStackStampInformation output of the ERCS VM.
    $TenantID = $AzureStackInfo.AADTenantID

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

    # Set the GraphEndpointResourceId value
    Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

    Add-AzureRmAccount -EnvironmentName "azurestackuser" `
    -ServicePrincipal `
    -CertificateThumbprint $ServicePrincipal.Thumbprint `
    -ApplicationId $ServicePrincipal.ClientId `
    -TenantId $TenantID

    # Output the SPN details
    $ServicePrincipal

   ```

2. Nach Abschluss der Automatisierung zeigt es die für die Verwendung des SPN erforderlichen Details an. 

   Beispiel: 

   ```
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
   ```

### <a name="assign-a-role"></a>Zuweisen einer Rolle
Nach dem Erstellen des Dienstprinzipals müssen Sie [ihn einer Rolle zuweisen](azure-stack-create-service-principals.md#assign-role-to-service-principal).

### <a name="sign-in-through-powershell"></a>Anmelden über PowerShell
Nachdem Sie eine Rolle zugewiesen haben, können Sie sich mit dem Dienstprinzipal an Azure Stack anmelden, indem Sie den folgenden Befehl verwenden:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ClientId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Zuweisen einer Rolle zum Dienstprinzipal
Um auf Ressourcen in Ihrem Abonnement zuzugreifen, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](../role-based-access-control/built-in-roles.md).

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1. Navigieren Sie im Azure Stack-Portal zur Bereichsebene, der Sie die Anwendung zuweisen möchten. Um z.B. einer Gruppe im Abonnementkontext eine Rolle zuzuweisen, wählen Sie **Abonnements** aus. Sie können stattdessen auch eine Ressourcengruppe oder Ressource auswählen.

2. Wählen Sie das entsprechende Abonnement (Ressourcengruppe oder Ressource) aus, dem die Anwendung zugewiesen werden soll.

     ![Abonnement für Zuweisung auswählen](./media/azure-stack-create-service-principal/image16.png)

3. Wählen Sie **Access Control (IAM)** aus.

     ![„Zugriff“ auswählen](./media/azure-stack-create-service-principal/image17.png)

4. Wählen Sie **Hinzufügen**.

5. Wählen Sie die Rolle aus, die Sie der Anwendung zuweisen möchten.

6. Suchen Sie nach Ihrer Anwendung, und wählen Sie sie aus.

7. Wählen Sie **OK** aus, um das Zuweisen der Rolle abzuschließen. Ihre Anwendung wird in der Liste der Benutzer angezeigt, die einer Rolle für diesen Kontext zugewiesen sind.

Nachdem Sie nun einen Dienstprinzipal erstellt und eine Rolle zugewiesen haben, können Sie diese Elemente in Ihrer Anwendung nutzen, um auf Azure Stack-Ressourcen zuzugreifen.  

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Benutzern für AD FS](azure-stack-add-users-adfs.md)
[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)
