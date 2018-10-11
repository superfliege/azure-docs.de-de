---
title: Referenz zum Cmdlet „Start-AzsReadinessChecker“ | Microsoft-Dokumentation
description: Hilfe zum PowerShell-Cmdlet für das Azure Stack-Modul für die Bereitschaftsüberprüfung.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 60e9a790a9b74bce7ccbdd58b320ad969c0932f3
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079281"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Referenz zum Cmdlet „Start-AzsReadinessChecker“

Modul: Microsoft.AzureStack.ReadinessChecker

Dieses Modul enthält nur ein einzelnes Cmdlet.  Dieses Cmdlet führt eine oder mehrere Funktionen vor der Bereitstellung oder Wartung für Azure Stack durch.

## <a name="syntax"></a>Syntax
```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       -Subject <OrderedDictionary>
       -RequestType <String>
       [-IncludePaaS]
       -OutputRequestPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```


```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -AADDirectoryTenantName <String>
       -IdentitySystem <String>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -ReportPath <String>
       [-ReportSections <String>]
       [-Summary]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```





 ## <a name="description"></a>BESCHREIBUNG
Das Cmdlet **Start-AzsReadinessChecker** überprüft Zertifikate, Azure-Konten, Azure-Abonnements und Azure Active Directory-Instanzen. Führen Sie vor der Bereitstellung von Azure Stack oder vor Azure Stack-Wartungsaktionen wie der Geheimnisrotation eine Überprüfung aus. Das Cmdlet kann auch zum Generieren von Zertifikatsignieranforderungen für Infrastruktur- und optional PaaS-Zertifikate verwendet werden.  Zu guter Letzt kann das Cmdlet PFX-Zertifikate neu packen, um häufige Probleme mit der Paketerstellung zu beheben.

## <a name="examples"></a>Beispiele
**Beispiel: Generieren einer Zertifikatsignieranforderung**

```PowerShell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subject $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

In diesem Beispiel generiert Start-AzsReadinessChecker mehrere Zertifikatsignieranforderungen (Certificate Signing Requests, CSR) für Zertifikate, die für eine AD FS-Azure Stack-Bereitstellung mit dem Regionsnamen „Osten“ und dem externen FQDN „azurestack.contoso.com“ geeignet sind.

**Beispiel: Überprüfen von Zertifikaten**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

In diesem Beispiel wird die Angabe des PFX-Kennworts für eine größere Sicherheit angefordert, und Start-AzsReadinessChecker überprüft den relativen Ordner „Zertifikate“ für Zertifikate, die für eine AAD-Bereitstellung mit dem Regionsnamen „Osten“ und dem externen FQDN „azurestack.contoso.com“ gültig sind. 

**Beispiel: Überprüfen von Zertifikaten mit Bereitstellungsdaten (Bereitstellung und Support)**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```
In diesem Beispiel zur Bereitstellung und zum Support wird das PFX-Kennwort für größere Sicherheit angefordert, und Start-AzsReadinessChecker überprüft den relativen Ordner „Zertifikate“ auf Zertifikate, die für eine Bereitstellung geeignet sind, bei der Identität, Region und externer FQDN aus der für die Bereitstellung generierten JSON-Datei mit Bereitstellungsdaten gelesen werden. 

**Beispiel: Überprüfen von PaaS-Zertifikaten**
```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates – RegionName east -FQDN azurestack.contoso.com
```

In diesem Beispiel wird eine Hashtabelle mit Pfaden und Kennwörtern für jedes PaaS-Zertifikat erstellt. Zertifikate können weggelassen werden. Start-AzsReadinessChecker überprüft, ob jeder PFX-Pfad vorhanden ist, und überprüft die Region „Osten“ und den externen FQDN „azurestack.contoso.com“.

**Beispiel: Überprüfen von PaaS-Zertifikaten mit Bereitstellungsdaten**
```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

In diesem Beispiel wird eine Hashtabelle mit Pfaden und Kennwörtern für jedes PaaS-Zertifikat erstellt. Zertifikate können weggelassen werden. Start-AzsReadinessChecker überprüft, ob jeder PFX-Pfad vorhanden ist, und überprüft diese anhand der Region und dem externen FQDN, die aus der für die Bereitstellung generierten JSON-Datei mit Bereitstellungsdaten gelesen werden. 

**Beispiel: Überprüfen der Azure-Identität**
```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AzureDirectoryTenantName azurestack.contoso.com
```

In diesem Beispiel werden die Kontoanmeldeinformationen des Dienstadministrators für größere Sicherheit angefordert, und Start-AzsReadinessChecker überprüft, ob das Azure-Konto und Azure Active Directory für eine AAD-Bereitstellung mit dem Mandantenverzeichnisnamen „azurestack.contoso.com“ gültig sind.


**Beispiel: Überprüfen der Azure-Identität mit Bereitstellungsdaten (Support für die Bereitstellung)**
```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

In diesem Beispiel werden die Kontoanmeldeinformationen des Dienstadministrators für größere Sicherheit angefordert, und Start-AzsReadinessChecker überprüft, ob das Azure-Konto und Azure Active Directory für eine AAD-Bereitstellung gültig sind, wobei AzureCloud und TenantName aus der für die Bereitstellung generierten JSON-Datei mit Bereitstellungsdaten gelesen werden.


**Beispiel: Überprüfen der Azure-Registrierung**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment AzureCloud
```

In diesem Beispiel werden die Anmeldeinformationen des Abonnementbesitzers für größere Sicherheit angefordert, und Start-AzsReadinessChecker führt dann eine Überprüfung anhand des angegebenen Kontos und Abonnements aus, um sicherzustellen, dass diese für die Azure Stack-Registrierung verwendet werden können. 


**Beispiel: Überprüfen der Azure-Registrierung mit Bereitstellungsdaten (Bereitstellungsteam)**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

In diesem Beispiel werden die Anmeldeinformationen des Abonnementbesitzers für größere Sicherheit angefordert, und Start-AzsReadinessChecker führt dann eine Überprüfung anhand des angegebenes Kontos und Abonnements aus, um sicherzustellen, dass diese für die Azure Stack-Registrierung verwendet werden können. Zusätzliche Details werden dabei aus der für die Bereitstellung generierten JSON-Datei mit Bereitstellungsdaten gelesen.

**Beispiel: Importieren bzw. Exportieren von PFX-Paketen**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

In diesem Beispiel wird das PFX-Kennwort für größere Sicherheit angefordert. Die Datei „ssl.pfx“ wird in den Zertifikatspeicher des lokalen Computers importiert, mit dem gleichen Kennwort wieder exportiert und als „ssl_new.pfx“ gespeichert.  Diese Vorgehensweise wird verwendet, wenn die Zertifikatüberprüfung gekennzeichnet hat, dass ein privater Schlüssel nicht über den Attributsatz eines lokalen Computers verfügt, die Zertifikatkette unterbrochen ist, irrelevante Zertifikate in der PFX-Datei enthalten sind oder die Zertifikatkette die falsche Reihenfolge aufweist.


**Beispiel: Anzeigen des Überprüfungsberichts (Support für die Bereitstellung)**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

In diesem Beispiel erhält das Bereitstellungs- oder Supportteam den Bereitschaftsbericht vom Kunden (Contoso), und Start-AzsReadinessChecker wird verwendet, um den Status der von Contoso ausgeführten Überprüfungsausführungen anzuzeigen.

**Beispiel: Anzeigen der Zusammenfassung des Überprüfungsberichts ausschließlich zur Zertifikatüberprüfung (Bereitstellung und Support)**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

In diesem Beispiel erhält das Bereitstellungs- oder Supportteam den Bereitschaftsbericht vom Kunden Contoso, und Start-AzsReadinessChecker wird verwendet, um einen zusammengefassten Status der von Contoso ausgeführten Zertifikatüberprüfungsausführungen anzuzeigen.



## <a name="required-parameters"></a>Erforderliche Parameter
> -RegionName

Gibt den Regionsnamen der Azure Stack-Bereitstellung an.
|  |  |
|----------------------------|--------------|
|Geben Sie Folgendes ein:                        |Zeichenfolge        |
|Position:                   |benannt         |
|Standardwert:              |Keine          |
|Pipelineeingabe akzeptieren:      |False         |
|Platzhalterzeichen akzeptieren: |False         |

> -FQDN    

Gibt den externen FQDN der Azure Stack-Bereitstellung an, der auch als Alias für ExternalFQDN und ExternalDomainName verwendet wird.
|  |  |
|----------------------------|--------------|
|Geben Sie Folgendes ein:                        |Zeichenfolge        |
|Position:                   |benannt         |
|Standardwert:              |ExternalFQDN, ExternalDomainName |
|Pipelineeingabe akzeptieren:      |False         |
|Platzhalterzeichen akzeptieren: |False         |

 

> -IdentitySystem    

Gibt die gültigen Werte des Identitätssystems der Azure Stack-Bereitstellung an, d.h. AAD oder ADFS für Azure Active Directory bzw. Active Directory-Verbunddienste (AD FS).
|  |  |
|----------------------------|--------------|
|Geben Sie Folgendes ein:                        |Zeichenfolge        |
|Position:                   |benannt         |
|Standardwert:              |Keine          |
|Gültige Werte:                |'AAD','ADFS'  |
|Pipelineeingabe akzeptieren:      |False         |
|Platzhalterzeichen akzeptieren: |False         |

> -PfxPassword    

Gibt das den PFX-Zertifikatdateien zugeordnete Kennwort an.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |SecureString |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -PaaSCertificates

Gibt die Hashtabelle an, die Pfade und Kennwörter für PaaS-Zertifikate enthält.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Hashtable |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -DeploymentDataJSONPath

Gibt die JSON-Konfigurationsdatei mit Azure Stack-Bereitstellungsdaten an. Diese Datei wird für die Bereitstellung generiert.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -PfxPath

Gibt den Pfad zu einem problematischen Zertifikat an, für das die Import-/Exportroutine, die für die Zertifikatüberprüfung in diesem Tool angegeben ist, behandelt werden muss.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -ExportPFXPath  

Gibt den Zielpfad für die resultierende PFX-Datei von der Import-/Exportroutine an.  
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -Subject

Gibt ein sortiertes Wörterbuch des Fachgebiets für die Generierung von Zertifikatanforderungen an.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |OrderedDictionary   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -RequestType

Gibt den SAN-Typ der Zertifikatanforderung an. Gültige Werte: MultipleCSR, SingleCSR.
- *MultipleCSR* generiert mehrere Zertifikatanforderungen, eine für jeden Dienst.
- *SingleCSR* generiert eine Zertifikatanforderung für alle Dienste.   

|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Gültige Werte:                |'MultipleCSR','SingleCSR' |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -OutputRequestPath

Gibt den Zielpfad für die Zertifikatanforderungsdateien an (das Verzeichnis muss bereits vorhanden sein).
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -AADServiceAdministrator

Gibt den für die Azure Stack-Bereitstellung zu verwendenden Azure Active Directory-Dienstadministrator an.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |PSCredential   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -AADDirectoryTenantName

Gibt den für die Azure Stack-Bereitstellung zu verwendenden Azure Active Directory-Namen an.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -AzureEnvironment

Gibt die Instanz der Azure-Dienste mit den Konten, Verzeichnissen und Abonnements an, die für die Azure Stack-Bereitstellung und -Registrierung verwendet werden sollen.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Gültige Werte:                |'AzureCloud','AzureChinaCloud','AzureGermanCloud' |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -RegistrationAccount

Gibt das Registrierungskonto an, das für die Azure Stack-Registrierung verwendet werden soll.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -RegistrationSubscriptionID

Gibt die Registrierungsabonnement-ID an, die für die Azure Stack-Registrierung verwendet werden soll.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Guid     |
|Position:                   |benannt    |
|Standardwert:              |Keine     |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |

> -ReportPath

Gibt den Pfad für den Bereitschaftsbericht an, für den standardmäßig das aktuelle Verzeichnis und der Standardberichtsname festgelegt sind.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Alle      |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |



## <a name="optional-parameters"></a>Optionale Parameter
> -CertificatePath     

Gibt den Pfad an, unter dem nur die für das Zertifikat erforderlichen Zertifikatordner enthalten sind.

Zu den erforderlichen Ordnern für die Azure Stack-Bereitstellung mit dem Azure Active Directory-Identitätssystem zählen Folgende:

ACSBlob, ACSQueue, ACSTable, Admin Portal, ARM Admin, ARM Public, KeyVault, KeyVaultInternal, Public Portal

Zu den erforderlichen Ordnern für die Azure Stack-Bereitstellung mit dem Identitätssystem der Active Directory-Verbunddienste (AD FS) zählen Folgende:

ACSBlob, ACSQueue, ACSTable, ADFS, Admin Portal, ARM Admin, ARM Public, Graph, KeyVault, KeyVaultInternal, Public Portal


|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |.\Certificates |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |


> -IncludePaaS  

Gibt an, ob PaaS-Dienste/-Hostnamen zu Zertifikatanforderungen hinzugefügt werden sollen.


|  |  |
|----------------------------|------------------|
|Geben Sie Folgendes ein:                        |SwitchParameter   |
|Position:                   |benannt             |
|Standardwert:              |False             |
|Pipelineeingabe akzeptieren:      |False             |
|Platzhalterzeichen akzeptieren: |False             |


> -ReportSections        

Gibt an, ob nur die Berichtszusammenfassung angezeigt werden soll und Details ausgelassen werden sollen.
|  |  |
|----------------------------|---------|
|Geben Sie Folgendes ein:                        |Zeichenfolge   |
|Position:                   |benannt    |
|Standardwert:              |Alle      |
|Gültige Werte:                |'Certificate','AzureRegistration','AzureIdentity','Jobs','All' |
|Pipelineeingabe akzeptieren:      |False    |
|Platzhalterzeichen akzeptieren: |False    |


> -Summary 

Gibt an, ob nur die Berichtszusammenfassung angezeigt werden soll und Details ausgelassen werden sollen.
|  |  |
|----------------------------|------------------|
|Geben Sie Folgendes ein:                        |SwitchParameter   |
|Position:                   |benannt             |
|Standardwert:              |False             |
|Pipelineeingabe akzeptieren:      |False             |
|Platzhalterzeichen akzeptieren: |False             |


> -CleanReport  

Entfernt die vorherige Ausführung und den Überprüfungsverlauf und schreibt Überprüfungen in einen neuen Bericht.
|  |  |
|----------------------------|------------------|
|Geben Sie Folgendes ein:                        |SwitchParameter   |
|Aliase:                    |cf                |
|Position:                   |benannt             |
|Standardwert:              |False             |
|Pipelineeingabe akzeptieren:      |False             |
|Platzhalterzeichen akzeptieren: |False             |


> -OutputPath    

Gibt den benutzerdefinierte Pfad zum Speichern des Bereitschaft-JSON-Berichts und der ausführlichen Protokolldatei an.  Wenn der Pfad noch nicht vorhanden ist, versucht das Tool, das Verzeichnis zu erstellen.
|  |  |
|----------------------------|------------------|
|Geben Sie Folgendes ein:                        |Zeichenfolge            |
|Position:                   |benannt             |
|Standardwert:              |$ENV:TEMP\AzsReadinessChecker  |
|Pipelineeingabe akzeptieren:      |False             |
|Platzhalterzeichen akzeptieren: |False             |


> -Confirm  

Fordert vor der Ausführung des Cmdlets zur Bestätigung auf.
|  |  |
|----------------------------|------------------|
|Geben Sie Folgendes ein:                        |SwitchParameter   |
|Aliase:                    |cf                |
|Position:                   |benannt             |
|Standardwert:              |False             |
|Pipelineeingabe akzeptieren:      |False             |
|Platzhalterzeichen akzeptieren: |False             |


> -WhatIf  

Zeigt, was geschieht, wenn das Cmdlet ausgeführt wird. Das Cmdlet wird nicht ausgeführt.
|  |  |
|----------------------------|------------------|
|Geben Sie Folgendes ein:                        |SwitchParameter   |
|Aliase:                    |wi                |
|Position:                   |benannt             |
|Standardwert:              |False             |
|Pipelineeingabe akzeptieren:      |False             |
|Platzhalterzeichen akzeptieren: |False             |

 
