---
title: Rotieren von Geheimnissen in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Geheimnisse in Azure Stack rotieren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 418b23f0783341ff7e5aaf7e2bbb2e869eb7dc45
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466153"
---
# <a name="rotate-secrets-in-azure-stack"></a>Rotieren von Geheimnissen in Azure Stack

*Diese Anweisungen gelten nur für integrierte Azure Stack-Systeme ab Version 1803. Versuchen Sie nicht, die Geheimnisrotation mit älteren Azure Stack-Versionen (vor 1802) zu verwenden.*

Azure Stack verwendet verschiedene Geheimnisse, um eine sichere Kommunikation zwischen Azure Stack-Infrastrukturressourcen und -diensten zu gewährleisten.

- **Interne Geheimnisse**  
Alle Zertifikate, Kennwörter, sicheren Zeichenfolgen und Schlüssel, die von der Azure Stack-Infrastruktur ohne Beteiligung des Azure Stack-Betreibers verwendet werden. 

- **Externe Geheimnisse**  
Infrastrukturdienstzertifikate für extern ausgerichtete Dienste, die vom Azure Stack-Betreiber bereitgestellt werden. Dazu zählen Zertifikate für folgende Dienste: 
    - Administratorportal 
    - Öffentliches Portal 
    - Azure Resource Manager (Administrator) 
    - Azure Resource Manager (global) 
    - Key Vault (Administrator) 
    - KeyVault 
    - ACS (einschließlich Blob, Table und Queue Storage) 
    - ADFS<sup>*</sup>
    - Graph<sup>*</sup>

   <sup>*</sup>Nur relevant, wenn Active Directory-Verbunddienste (AD FS) als Identitätsanbieter der Umgebung verwendet wird.

> [!NOTE]
> Alle anderen sicheren Schlüssel und Zeichenfolgen (einschließlich BMC- und Wechselkennwörter sowie Kennwörter für Benutzer- und Administratorkonten) werden weiterhin manuell vom Administrator aktualisiert. 

Um die Integrität der Azure Stack-Infrastruktur zu erhalten, müssen Betreiber die Geheimnisse ihrer Infrastruktur regelmäßig und mit einer Häufigkeit rotieren können, die mit den Sicherheitsanforderungen ihrer Organisation in Einklang steht.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Rotieren von Geheimnissen mit externen Zertifikate von einer neuen Zertifizierungsstelle

Azure Stack unterstützt Geheimnisrotation mit externen Zertifikaten von einer neuen Zertifizierungsstelle (ZS) in den folgenden Kontexten:

|Installierte Zertifizierungsstelle|ZS, zu der rotiert werden soll|Unterstützt|Unterstützte Versionen von Azure Stack|
|-----|-----|-----|-----|
|Von „Selbstsigniert“|Zu „Enterprise“|Nicht unterstützt||
|Von „Selbstsigniert“|Zu „Selbstsigniert“|Nicht unterstützt||
|Von „Selbstsigniert“|Zu „Öffentlich“<sup>*</sup>|Unterstützt|1803 und höher|
|Von „Enterprise“|Zu „Enterprise“|Wird unterstützt, solange Kunden die GLEICHE Unternehmens-ZS wie bei der Bereitstellung verwenden.|1803 und höher|
|Von „Enterprise“|Zu „Selbstsigniert“|Nicht unterstützt||
|Von „Enterprise“|Zu „Öffentlich“<sup>*</sup>|Unterstützt|1803 und höher|
|Von „Öffentlich“<sup>*</sup>|Zu „Enterprise“|Nicht unterstützt|1803 und höher|
|Von „Öffentlich“<sup>*</sup>|Zu „Selbstsigniert“|Nicht unterstützt||
|Von „Öffentlich“<sup>*</sup>|Zu „Öffentlich“<sup>*</sup>|Unterstützt|1803 und höher|

<sup>*</sup> Öffentliche Zertifizierungsstellen sind hier diejenigen, die Teil des „Vertrauenswürdiger Stamm“-Programms von Windows sind. Sie finden die vollständige Liste unter [Microsoft Trusted Root Certificate Program: Participants (as of June 27, 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca) („Vertrauenswürdiges Stammzertifikat“-Programm von Microsoft [mit Stand vom 27. Juni 2017]).

## <a name="alert-remediation"></a>Behandeln von Warnungen

Für Geheimnisse, die in 30 Tagen ablaufen, werden im Administratorportal folgende Warnungen generiert: 

- Pending service account password expiration (Bevorstehender Ablauf eines Dienstkontokennworts) 
- Pending internal certificate expiration (Bevorstehender Ablauf eines internen Zertifikats) 
- Pending external certificate expiration (Bevorstehender Ablauf eines externen Zertifikats) 

Diese Warnungen können durch Ausführen der Geheimnisrotation behandelt werden. Führen Sie hierzu die im Anschluss beschriebenen Schritte aus.

## <a name="pre-steps-for-secret-rotation"></a>Vorbereiten der Geheimnisrotation

   > [!IMPORTANT]  
   > Vergewissern Sie sich, dass die Geheimnisrotation in Ihrer Umgebung nicht erfolgreich durchgeführt wurde. Wenn die Geheimnisrotation bereits durchgeführt wurde, aktualisieren Sie Azure Stack auf Version 1807 oder höher, bevor Sie die Geheimnisrotation ausführen. 

1.  Operatoren wird auffallen, dass während der Rotation der Azure Stack-Geheimnisse Warnungen geöffnet und automatisch wieder geschlossen werden.  Dieses Verhalten ist zu erwarten, und die Warnungen können ignoriert werden.  Operatoren können die Gültigkeit dieser Warnungen durch Ausführen von Test-AzureStack überprüfen.  Wenn Operatoren Azure Stack-Systeme mithilfe von SCOM überwachen, wird durch Versetzen eines Systems in den Wartungsmodus verhindert, dass diese Warnungen ihre ITSM-Systeme erreichen, es werden jedoch weiterhin Warnungen ausgegeben, wenn das Azure Stack-System nicht mehr erreichbar ist. 
2. Informieren Sie Ihre Benutzer über sämtliche Wartungsmaßnahmen. Planen Sie normale Wartungszeitfenster möglichst außerhalb der Geschäftszeiten. Wartungsvorgänge können sowohl Benutzerworkloads als auch Portalvorgänge beeinträchtigen.
    > [!note]  
    > Die nächsten Schritte gelten nur für die Rotation externer Azure Stack-Geheimnisse.

3. Führen Sie **[Test-AzureStack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test)** aus, und bestätigen Sie, dass alle Testausgaben fehlerfrei sind, bevor Sie Geheimnisse rotieren.
4. Bereiten Sie eine neue Gruppe externer Ersatzzertifikate vor. Die neue Gruppe entspricht den Zertifikatspezifikationen aus den [Azure Stack-PKI-Zertifikatanforderungen](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs).
5.  Speichern Sie eine Sicherung der für die Rotation verwendeten Zertifikate an einem sicheren Sicherungsspeicherort. Sollte die Rotation nicht erfolgreich sein, ersetzen Sie die Zertifikate in der Dateifreigabe durch die Sicherungskopien, und wiederholen Sie dann die Rotation. Hinweis: Bewahren Sie Sicherungskopien am sicheren Sicherungsspeicherort auf.
6.  Erstellen Sie eine Dateifreigabe, auf die Sie über die virtuellen ERCS-Computer zugreifen können. Die Identität **CloudAdmin** muss über Lese- und Schreibzugriff für die Dateifreigabe verfügen.
7.  Öffnen Sie eine PowerShell ISE-Konsole auf einem Computer, auf dem Sie Zugriff auf die Dateifreigabe haben. Navigieren Sie zu Ihrer Dateifreigabe. 
8.  Führen Sie **[CertDirectoryMaker.ps1](http://www.aka.ms/azssecretrotationhelper)** aus, um die erforderlichen Verzeichnisse für Ihre externen Zertifikate zu erstellen.

## <a name="rotating-external-and-internal-secrets"></a>Rotieren externer und interner Geheimnisse

Gehen Sie zum Rotieren externer und interner Geheimnisse wie folgt vor:

1. Platzieren Sie die neue Gruppe externer Ersatzzertifikate in der Verzeichnisstruktur des Verzeichnisses **/Certificates**, das Sie im Rahmen der Vorbereitungsschritte erstellt haben. Berücksichtigen Sie dabei das Format, wie unter [Azure Stack-PKI-Zertifikatanforderungen](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates) im Abschnitt „Erforderliche Zertifikate“ erläutert.
2. Erstellen Sie unter Verwendung des Kontos **CloudAdmin** eine PowerShell-Sitzung mit dem [privilegierten Endpunkt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint), und speichern Sie die Sitzung als Variable. Diese Variable wird im nächsten Schritt als Parameter verwendet.

    > [!IMPORTANT]  
    > Geben Sie die Sitzung nicht ein; speichern Sie sie als Variable.
    
3. Führen Sie **[invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)** aus. Übergeben Sie die PowerShell-Sitzungsvariable für den privilegierten Endpunkt als Parameter **Session**. 
4. Führen Sie **Start-SecretRotation** mit den folgenden Parametern aus:
    - **PfxFilesPath**  
    Geben Sie den Netzwerkpfad des zuvor erstellten Verzeichnisses „Certificates“ an.  
    - **PathAccessCredential**  
    Ein PSCredential-Objekt mit Anmeldeinformationen für die Freigabe. 
    - **CertificatePassword**  
    Eine sichere Zeichenfolge des Kennworts, das für alle erstellten PFX-Zertifikatdateien verwendet wird.
4. Warten Sie, bis Ihre Geheimnisse rotiert wurden.  
Wenn die Geheimnisrotation erfolgreich abgeschlossen wurde, wird in der Konsole **Overall action status: Success** (Allgemeiner Aktionsstatus: Erfolgreich) angezeigt. 
    > [!note]  
    > Wenn bei der Geheimnisrotation ein Fehler auftritt, befolgen Sie die Anweisungen in der Fehlermeldung, und führen Sie „start-secretrotation“ mit dem Parameter **-Rerun** erneut aus. Wenden Sie sich an den Support, wenn wiederholt Fehler bei der Geheimnisrotation auftreten. 
5. Entfernen Sie nach erfolgreichem Abschluss der Geheimnisrotation Ihre Zertifikate aus der Freigabe, die Sie im Rahmen der Vorbereitung erstellt haben, und speichern Sie sie an ihrem sicheren Sicherungsspeicherort. 

## <a name="walkthrough-of-secret-rotation"></a>Exemplarische Vorgehensweise für die Geheimnisrotation

Das folgende PowerShell-Beispiel veranschaulicht die zu verwendenden Cmdlets und Parameter für die Geheimnisrotation.

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = ConvertTo-SecureString "Certpasswordhere" -AsPlainText -Force
$CertShareCred = Get-Credential 
$CertSharePath = "<NetworkPathofCertShare>"
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```
## <a name="rotating-only-internal-secrets"></a>Nur Rotieren interner Geheimnisse

So rotieren Sie nur die internen Geheimnisse von Azure Stack:

1. Erstellen Sie eine PowerShell-Sitzung mit dem [privilegierten Endpunkt](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Führen Sie in der Sitzung des privilegierten Endpunkts **Start-SecretRotation** ohne Argumente aus.
3. Warten Sie, bis Ihre Geheimnisse rotiert wurden.  
Wenn die Geheimnisrotation erfolgreich abgeschlossen wurde, wird in der Konsole **Overall action status: Success** (Allgemeiner Aktionsstatus: Erfolgreich) angezeigt. 
    > [!note]  
    > Wenn bei der Geheimnisrotation ein Fehler auftritt, befolgen Sie die Anweisungen in der Fehlermeldung, und führen Sie „start-secretrotation“ mit dem Parameter **-Rerun** erneut aus. Wenden Sie sich an den Support, wenn wiederholt Fehler bei der Geheimnisrotation auftreten. 

## <a name="start-secretrotation-reference"></a>Referenz für „Start-SecretRotation“

Rotiert die Geheimnisse eines Azure Stack-Systems. Wird nur für den privilegierten Azure Stack-Endpunkt ausgeführt.

### <a name="syntax"></a>Syntax

Pfad (Standard)

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>BESCHREIBUNG

Das Cmdlet „Start-SecretRotation“ rotiert die Infrastrukturgeheimnisse eines Azure Stack-Systems. Standardmäßig werden alle Geheimnisse rotiert, die für das interne Infrastrukturnetzwerk verfügbar gemacht wurden. Mit „user-input“ werden auch die Zertifikate aller externen Netzwerkinfrastruktur-Endpunkte rotiert. Beim Rotieren externer Netzwerkinfrastruktur-Endpunkte muss „Start-SecretRotation“ über einen Invoke-Command-Skriptblock ausgeführt werden, und die Sitzung des privilegierten Endpunkts der Azure Stack-Umgebung muss als Sitzungsparameter übergeben werden.
 
### <a name="parameters"></a>Parameter

| Parameter | Typ | Erforderlich | Position | Standard | BESCHREIBUNG |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Zeichenfolge  | False  | benannt  | Keine  | Der Dateifreigabepfad des Verzeichnisses **\Certificates** mit allen externen Netzwerkendpunkt-Zertifikaten. Nur beim Rotieren externer oder aller Geheimnisse erforderlich. Das Endverzeichnis muss **\Certificates** sein. |
| CertificatePassword | SecureString | False  | benannt  | Keine  | Das Kennwort für alle Zertifikate in „-PfXFilesPath“. Erforderlich, wenn „PfxFilesPath“ beim gleichzeitigen Rotieren interner und externer Geheimnisse angegeben wird. |
| PathAccessCredential | PSCredential | False  | benannt  | Keine  | Die PowerShell-Anmeldeinformationen für die Dateifreigabe des Verzeichnisses **\Certificates** mit allen externen Netzwerkendpunkt-Zertifikaten. Nur beim Rotieren externer oder aller Geheimnisse erforderlich.  |
| Rerun | SwitchParameter | False  | benannt  | Keine  | Die erneute Ausführung muss immer dann erfolgen, wenn die Geheimnisrotation nach einem fehlgeschlagenen Versuch erneut versucht wird. |

### <a name="examples"></a>Beispiele
 
**Nur Rotieren interner Infrastrukturgeheimnisse**

```powershell  
PS C:\> Start-SecretRotation  
```

Dieser Befehl rotiert alle Infrastrukturgeheimnisse, die für das interne Azure Stack-Netzwerk verfügbar gemacht wurden. „Start-SecretRotation“ rotiert alle von Stack generierten Geheimnisse. Da aber keine bereitgestellten Zertifikate vorhanden sind, werden keine externen Endpunktzertifikate rotiert.  

**Rotieren interner und externer Infrastrukturgeheimnisse**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Dieser Befehl rotiert alle Infrastrukturgeheimnisse, die für das interne Azure Stack-Netzwerk verfügbar gemacht wurden, sowie die TLS-Zertifikate, die für die externen Netzwerkinfrastruktur-Endpunkte von Azure Stack verwendet werden. „Start-SecretRotation“ rotiert alle von Stack generierten Geheimnisse, und da bereitgestellte Zertifikate vorhanden sind, werden auch externe Endpunktzertifikate rotiert.  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>Aktualisieren des BMC-Kennworts (Baseboard Management Controller, Baseboard-Verwaltungscontroller)

Der Baseboard-Verwaltungscontroller (Baseboard Management Controller, BMC) überwacht den physischen Zustand Ihrer Server. Die Spezifikationen und Anweisungen zur Aktualisierung des Kennworts des BMC variieren je nach OEM (Original Equipment Manufacturer, Originalgerätehersteller). Die Kennwörter für Azure Stack-Komponenten sollten in regelmäßigen Abständen aktualisiert werden.

1. Aktualisieren Sie den BMC auf den physischen Azure Stack-Server gemäß den Anweisungen des OEM. Das Kennwort für jeden BMC in Ihrer Umgebung muss identisch sein.
2. Öffnen Sie in Azure Stack einen privilegierten Endpunkt. Anweisungen finden Sie unter [Verwenden des privilegierten Endpunkts in Azure Stack](azure-stack-privileged-endpoint.md).
3. Nachdem die PowerShell-Eingabeaufforderung sich je nach Umgebung in **[IP-Adresse oder ERCS-VM-Name]: PS>** oder in **[azs-ercs01]: PS>** geändert hat, führen Sie `Set-BmcPassword` aus, indem Sie `invoke-command` ausführen. Übergeben Sie die Sitzungsvariable des privilegierten Endpunkts als Parameter. Beispiel: 

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    Sie können auch die statische PowerShell-Version mit den Kennwörtern als Codezeilen verwenden:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Nächste Schritte

[Informieren Sie sich ausführlicher über die Sicherheit von Azure Stack.](azure-stack-security-foundations.md)
