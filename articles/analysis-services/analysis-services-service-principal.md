---
title: Automatisieren von Azure Analysis Services-Aufgaben mit Dienstprinzipalen | Microsoft Docs
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 931a45fdbb04d15f3080ee0a2c0546ab6e924b59
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595715"
---
# <a name="automation-with-service-principals"></a>Automatisierung mit Dienstprinzipalen

Dienstprinzipale sind eine Azure Active Directory-Anwendungsressource, die Sie in Ihrem Mandanten erstellen, um unbeaufsichtigte Ressourcen- und Servicelevelvorgänge auszuführen. Es handelt sich dabei um einen eindeutigen Typ von *Benutzeridentität* mit einer Anwendungs-ID und einem Kennwort oder Zertifikat. Ein Dienstprinzipal verfügt nur über die Berechtigungen, die zum Ausführen der Aufgaben erforderlich sind, die durch die ihm zugewiesenen Rollen und Berechtigungen definiert sind. 

In Analysis Services werden Dienstprinzipale mit Azure Automation, PowerShell im unbeaufsichtigten Modus, benutzerdefinierten Clientanwendungen und Web-Apps zum Automatisieren von allgemeinen Aufgaben verwendet. So können beispielsweise Aufgaben wie das Bereitstellen von Servern, das Bereitstellen von Modellen, das Aktualisieren von Daten, das zentrale Hoch-/Herunterskalieren sowie Anhalten/Fortsetzen mithilfe von Dienstprinzipalen automatisiert werden. Berechtigungen werden Dienstprinzipalen über Rollenmitgliedschaften zugewiesen, ähnlich wie bei Azure AD-UPN-Standardkonten.

## <a name="create-service-principals"></a>Erstellen von Dienstprinzipalen
 
Dienstprinzipale können im Azure-Portal oder mithilfe von PowerShell erstellt werden. Weitere Informationen finden Sie unter:

[Erstellen eines Dienstprinzipals – Azure-Portal](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Erstellen eines Dienstprinzipals – PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Speichern von Anmeldeinformations- und Zertifikatassets in Azure Automation

Anmeldeinformationen und Zertifikate von Dienstprinzipalen können in Azure Automation für Runbook-Vorgänge sicher gespeichert werden. Weitere Informationen finden Sie unter:

[Anmeldeinformationsobjekte in Azure Automation](../automation/automation-credentials.md)   
[Zertifikatobjekte in Azure Automation](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Hinzufügen von Dienstprinzipalen zur Serveradministratorrolle

Bevor Sie einen Dienstprinzipal für Analysis Services-Serververwaltungsvorgänge verwenden können, müssen Sie ihn zur Serveradministratorrolle hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen eines Dienstprinzipals zur Serveradministratorrolle](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Dienstprinzipale in Verbindungszeichenfolgen

Dienstprinzipal-AppID und ein Kennwort oder Zertifikat können in Verbindungszeichenfolgen ähnlich wie ein UPN verwendet werden.

### <a name="powershell"></a>PowerShell

Wenn Sie einen Dienstprinzipal für Ressourcenverwaltungsvorgänge mit dem [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)-Modul nutzen, verwenden Sie das Cmdlet `Login-AzureRmAccount`. Wenn Sie einen Dienstprinzipal für Servervorgänge mit dem [SQLServer](https://www.powershellgallery.com/packages/SqlServer)-Modul nutzen, verwenden Sie das Cmdlet `Add-AzureAnalysisServicesAccount`. 

Im folgenden Beispiel werden AppID und ein Kennwort verwendet, um einen Aktualisierungsvorgang für eine Modelldatenbank auszuführen:

```PowerShell
Param (

        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Add-AzureAnalysisServicesAccount -Credential $Credential -ServicePrincipal -TenantId $TenantId -RolloutEnvironment "westcentralus.asazure.windows.net"

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full"
```

### <a name="amo-and-adomd"></a>AMO und ADOMD 

Beim Herstellen von Verbindungen mit Clientanwendungen und Web-Apps, unterstützen [AMO- und ADOMD-Clientbibliotheken](analysis-services-data-providers.md) und installierbare NuGet-Pakete (Version 15.0.2 und höher) Dienstprinzipale in Verbindungszeichenfolgen mit der folgenden Syntax: `app:AppID` und Kennwort oder `cert:thumbprint`. 

Im folgenden Beispiel werden `appID` und ein `password` verwendet, um einen Aktualisierungsvorgang für eine Modelldatenbank auszuführen:

```C#
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>Nächste Schritte
[Anmelden mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Hinzufügen eines Dienstprinzipals zur Serveradministratorrolle](analysis-services-addservprinc-admins.md)   