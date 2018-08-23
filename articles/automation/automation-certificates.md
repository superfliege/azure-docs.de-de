---
title: Zertifikatobjekte in Azure Automation
description: Zertifikate können sicher in Azure Automation gespeichert werden, sodass sie von Runbooks oder DSC-Konfigurationen zur Authentifizierung bei Azure und Drittanbieterressourcen verwendet werden können.  Dieser Artikel stellt eine ausführliche Beschreibung von Zertifikaten bereit und zeigt, wie diese in Textrunbooks und grafischen Runbooks eingesetzt werden.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c13da6ff7c864ffa365dbad33d6eb0cf2e35fa42
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42145834"
---
# <a name="certificate-assets-in-azure-automation"></a>Zertifikatobjekte in Azure Automation

Zertifikate können sicher in Azure Automation gespeichert werden, sodass Sie aus Runbooks oder DSC-Konfigurationen mithilfe der Aktivität **Get-AzureRmAutomationCertificate** für Azure Resource Manager-Ressourcen darauf zugreifen können. Mit dieser Funktion können Sie Runbooks und DSC-Konfigurationen erstellen, die Zertifikate für die Authentifizierung verwenden oder diese Azure oder Drittanbieterressourcen hinzufügen.

>[!NOTE]
>Zu den sicheren Objekten in Azure Automation gehören Anmeldeinformationen, Zertifikate, Verbindungen und verschlüsselte Variablen. Diese Objekte werden mithilfe eines eindeutigen Schlüssels verschlüsselt und in Azure Automation gespeichert, der für jedes Automation-Konto generiert wird. Dieser Schlüssel wird in Key Vault gespeichert. Vor dem Speichern eines sicheren Objekts wird der Schlüssel aus Key Vault geladen und dann zum Verschlüsseln des Objekts verwendet.

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell-Cmdlets
Die Cmdlets in der folgenden Tabelle werden für AzureRM zum Erstellen und Verwalten von Automation-Anmeldeinformationsobjekten mit Windows PowerShell verwendet. Sie gehören zum Lieferumfang des [AzureRM.Automation-Moduls](/powershell/azure/overview), das zur Verwendung in Automation-Runbooks und DSC-Konfigurationen verfügbar ist.

|Cmdlets|BESCHREIBUNG|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Ruft Informationen über ein Zertifikat zur Verwendung in einem Runbook oder einer DSC-Konfiguration ab. Sie können mithilfe der Aktivität "Get-AutomationCertificate" nur das Zertifikat selbst abrufen.|
|[New-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Erstellt ein neues Zertifikat in Azure Automation.|
[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Entfernt ein Zertifikat aus Azure Automation.|Erstellt ein neues Zertifikat in Azure Automation.
|[Set-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Legt die Eigenschaften für ein vorhandenes Zertifikat fest, lädt die Zertifikatdatei hoch und legt das Kennwort für eine PFX-Datei fest.|
|[Add-AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Lädt ein Dienstzertifikat für den angegebenen Clouddienst hoch.|

## <a name="activities"></a>Aktivitäten
Die Aktivitäten in der folgenden Tabelle werden für den Zugriff auf Zertifikate in einem Runbook oder DSC-Konfigurationen verwendet.

| Aktivitäten | BESCHREIBUNG |
|:---|:---|
|Get-AutomationCertificate|Ruft ein Zertifikat zur Verwendung in einem Runbook oder einer DSC-Konfiguration ab. Gibt ein [System.Security.Cryptography.X509Certificates.X509Certificate2](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate2.aspx)-Objekt zurück.|

> [!NOTE] 
> Vermeiden Sie die Verwendung von Variablen im Parameter „–Name“ von **Get-AutomationCertificate** in einem Runbook oder einer DSC-Konfiguration, da dies die Ermittlung von Abhängigkeiten zwischen Runbooks bzw. DSC-Konfigurationen und Automation-Variablen zur Entwurfszeit erschwert.

## <a name="python2-functions"></a>Python2-Funktionen

Mit der Funktion in der folgenden Tabelle wird auf Zertifikate in einem Python2-Runbook zugegriffen.

| Funktion | BESCHREIBUNG |
|:---|:---|
| automationassets.get_automation_certificate | Ruft Informationen zu einem Zertifikatasset ab. |

> [!NOTE]
> Sie müssen das Modul **automationassets** am Anfang des Python-Runbooks importieren, um auf die Assetfunktionen zugreifen zu können.

## <a name="creating-a-new-certificate"></a>Erstellen eines neues Zertifikats

Wenn Sie ein neues Zertifikat erstellen, laden Sie eine CER- oder PFX-Datei in Azure Automation hoch. Wenn Sie das Zertifikat als exportierbar kennzeichnen, können Sie es aus dem Azure Automation-Zertifikatspeicher übertragen. Ist das Zertifikat nicht exportierbar, können Sie es nur zum Signieren innerhalb des Runbooks oder der DSC-Konfiguration verwenden. Für Azure Automation ist es erforderlich, dass das Zertifikat über den Anbieter **Microsoft Enhanced RSA und AES Cryptographic Provider** verfügt.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>So erstellen Sie ein neues Zertifikat mit dem Azure-Portal

1. Klicken Sie in Ihrem Automation-Konto auf die Kachel **Objekte**, um das Blatt **Objekte** zu öffnen.
1. Klicken Sie auf die Kachel **Zertifikate**, um das Blatt **Zertifikate** zu öffnen.
1. Klicken Sie oben im Blatt auf **Zertifikat hinzufügen** .
1. Geben Sie im Feld **Name** einen Namen für das Zertifikat ein.
1. Klicken Sie unterhalb von **Zertifikatdatei hochladen** auf **Datei auswählen**, um nach einer CER- oder PFX-Datei zu suchen. Wenn Sie eine PFX-Datei auswählen, geben Sie ein Kennwort an und legen fest, ob das Zertifikat exportiert werden kann.
1. Klicken Sie auf **Erstellen** , um das neue Zertifikatobjekt zu speichern.

### <a name="to-create-a-new-certificate-with-windows-powershell"></a>So erstellen Sie ein neues Zertifikat mit Windows PowerShell

Das folgende Beispiel zeigt, wie Sie ein neues Automation-Zertifikat erstellen und es als exportierbar kennzeichnen. In diesem Beispiel wird eine vorhandene PFX-Datei importiert.

```powershell-interactive
$certName = 'MyCertificate'
$certPath = '.\MyCert.pfx'
$certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup
```

## <a name="using-a-certificate"></a>Verwenden eines Zertifikats

Nutzen Sie die Aktivität **Get-AutomationCertificate**, um ein Zertifikat zu verwenden. Eine Verwendung des Cmdlets [Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate?view=azurermps-6.6.0) ist nicht möglich, da dieses Cmdlet Informationen zum Zertifikatobjekt, aber nicht zum Zertifikat selbst zurückgibt.

### <a name="textual-runbook-sample"></a>Beispiel für ein Textrunbook

Der folgende Beispielcode zeigt, wie Sie ein Zertifikat zu einem Clouddienst in einem Runbook hinzufügen. In diesem Beispiel wird das Kennwort aus einer verschlüsselten Automation-Variable abgerufen.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Beispiel für ein grafisches Runbook

Sie können einem grafischen Runbook **Get-AutomationCertificate** hinzufügen, indem Sie im Bibliotheksbereich des grafischen Editors mit der rechten Maustaste auf das Zertifikat klicken und **Zum Zeichenbereich hinzufügen** auswählen.

![Hinzufügen eines Zertifikats zum Zeichenbereich](media/automation-certificates/automation-certificate-add-to-canvas.png)

Die folgende Abbildung zeigt ein Beispiel für die Verwendung eines Zertifikats in einem grafischen Runbook. Dies entspricht dem obigen Beispiel zum Hinzufügen eines Zertifikats zu einem Clouddienst aus einem Textrunbook.

![Beispiel für grafische Inhaltserstellung ](media/automation-certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Python2-Beispiel
Das folgende Beispiel zeigt, wie Sie auf Zertifikate in Python2-Runbooks zugreifen:

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert 
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Arbeiten mit Verknüpfungen zum Steuern des logischen Ablaufs von Aktivitäten, die Ihr Runbook ausführen soll, finden Sie unter [Verknüpfungen bei der grafischen Erstellung](automation-graphical-authoring-intro.md#links-and-workflow). 
