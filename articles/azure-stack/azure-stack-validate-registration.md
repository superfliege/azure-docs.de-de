---
title: Überprüfen der Azure-Registrierung für Azure Stack | Microsoft-Dokumentation
description: Verwenden Sie Azure Stack Readiness Checker, um eine Azure-Registrierung zu überprüfen.
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
ms.date: 12/04/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/04/2018
ms.openlocfilehash: 0281186be4fae71c56b65327e0c67da3f252a33c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239518"
---
# <a name="validate-azure-registration"></a>Überprüfen der Azure-Registrierung
 
Verwenden Sie das Tool Azure Stack Readiness Checker (AzsReadinessChecker), um zu überprüfen, ob Ihr Azure-Abonnement mit Azure Stack verwendet werden kann. Überprüfen Sie die Registrierung, bevor Sie mit einer Azure Stack-Bereitstellung beginnen. Bei der Überprüfung der Bereitschaft wird Folgendes geprüft:

- Bei dem Azure-Abonnement, das Sie verwenden, handelt es sich um einen unterstützten Typ. Bei Abonnements muss es sich um einen Cloud-Dienstanbieter (Cloud Service Provider, CSP) oder einen Enterprise Agreement (EA) handeln. 
- Das Konto, mit dem Sie Ihr Abonnement bei Azure registrieren, kann sich bei Azure anmelden und ist der Besitzer eines Abonnements. 

Weitere Informationen zur Azure Stack-Registrierung finden Sie unter [Registrieren von Azure Stack in Azure](azure-stack-registration.md). 

## <a name="get-the-readiness-checker-tool"></a>Beziehen des Tools zur Bereitschaftsüberprüfung

Laden Sie die neueste Version des Tools Azure Stack Readiness Checker (AzsReadinessChecker) aus dem [PowerShell-Katalog](https://aka.ms/AzsReadinessChecker) herunter.  

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein:

**Auf dem Computer, auf dem das Tool ausgeführt wird, muss Folgendes installiert sein**:
 - Windows 10 oder Windows Server 2016 mit Internetkonnektivität
 - PowerShell 5.1 oder höher Um Ihre Version zu überprüfen, führen Sie das folgende PowerShell-Cmdlet aus, und überprüfen Sie dann die *Hauptversion* und die *Nebenversionen*:  

    ```powershell
    $PSVersionTable.PSVersion
    ``` 
 - Konfigurieren Sie [PowerShell für Azure Stack](azure-stack-powershell-install.md). 
 - Laden Sie die aktuelle Version des Tools [Microsoft Azure Stack Readiness Checker](https://aka.ms/AzsReadinessChecker) herunter.  

**Azure Active Directory-Umgebung**:
 - Sie müssen über den Benutzernamen und das Kennwort eines Kontos verfügen, das der Besitzer eines Azure-Abonnements ist, unter dem Sie Azure Stack verwenden.  
 - Ermitteln Sie die Abonnement-ID für das Azure-Abonnement, das Sie verwenden möchten. 
 - Legen Sie die Azure-Umgebung (**AzureEnvironment**) fest, die Sie verwenden möchten. Unterstützte Werte für den Umgebungsnamensparameter sind **AzureCloud**, **AzureChinaCloud** und **AzureUSGovernment** (abhängig Ihrem verwendeten Azure-Abonnement).

## <a name="validate-azure-registration"></a>Überprüfen der Azure-Registrierung

1. Öffnen Sie auf einem Computer, der die Voraussetzungen erfüllt, eine administrative PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus, um **AzsReadinessChecker** zu installieren:

    ```powershell
    Install-Module Microsoft.AzureStack.ReadinessChecker -Force
    ```

2. Führen Sie an der PowerShell-Eingabeaufforderung Folgendes aus, um `$registrationCredential` als Abonnementbesitzerkonto festzulegen. Ersetzen Sie `subscriptionowner@contoso.onmicrosoft.com` durch Ihr Konto und Ihren Mandanten: 
   ```powershell
   $registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"
   ```

3. Führen Sie an der PowerShell-Eingabeaufforderung Folgendes aus, um `$subscriptionID` als zu verwendendes Azure-Abonnement festzulegen. Ersetzen Sie `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` durch Ihre eigene Abonnement-ID:
   ```powershell
   $subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
   ``` 

4. Führen Sie an der PowerShell-Eingabeaufforderung Folgendes aus, um die Überprüfung Ihres Abonnements zu starten: 
   - Legen Sie als Wert für AzureEnvironment **AzureCloud**, **AzureGermanCloud** oder **AzureChinaCloud** fest.  
   - Geben Sie Ihren Azure Active Directory-Administrator und den Namen Ihres Azure Active Directory-Mandanten an. 

   ```powershell
   Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID
   ```

5. Wenn das Tool ausgeführt wird, überprüfen Sie die Ausgabe. Vergewissern Sie sich, dass der Status für die Anmelde- und Registrierungsanforderungen „OK“ lautet. Eine erfolgreiche Validierung sind in etwa wie folgt aus:
  
   ```shell
   Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
   Checking Registration Requirements: OK
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsRegistrationValidation Completed
   ```

## <a name="report-and-log-file"></a>Berichts- und Protokolldatei

Bei jeder Ausführung einer Überprüfung werden Ergebnisse in den Dateien **AzsReadinessChecker.log** und **AzsReadinessCheckerReport.json** protokolliert. Der Speicherort dieser Dateien wird mit den Überprüfungsergebnissen in PowerShell angezeigt. 

Anhand dieser Dateien können Sie den Überprüfungsstatus freigeben, bevor Sie Azure Stack bereitstellen oder Probleme mit der Überprüfung untersuchen. Die Ergebnisse aller nachfolgenden Überprüfungen werden in beiden Dateien gespeichert. Der Bericht enthält die Bestätigung Ihres Bereitstellungsteams über die Identitätskonfiguration. Mithilfe der Protokolldatei kann Ihr Bereitstellungs- oder Supportteam Probleme bei der Überprüfung untersuchen. 

Standardmäßig werden beide Dateien in das Verzeichnis *C:\Users\<Benutzername>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json* geschrieben.  
 - Verwenden Sie den Parameter **-OutputPath** ***&lt;Pfad&gt;*** am Ende der Befehlszeilenausführung, um einen anderen Berichtsspeicherort anzugeben.   
 - Verwenden Sie den Parameter **-CleanReport** am Ende der Befehlszeilenausführung, um Informationen von früheren Ausführungen des Tools aus *AzsReadinessCheckerReport.json*  zu löschen. Weitere Informationen hierzu finden Sie unter [Azure Stack-Überprüfungsbericht](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Fehler bei der Überprüfung
Wenn bei der Überprüfung Fehler auftreten, werden Details zum Fehler im PowerShell-Fenster angezeigt. Das Tool protokolliert Informationen außerdem in der Datei „AzsReadinessChecker.log“.

Die folgenden Beispiele zeigen, wie allgemeine Fehler bei der Überprüfung behoben werden:

### <a name="user-must-be-an-owner-of-the-subscription"></a>Der Benutzer muss der Besitzer eines Abonnements sein.   

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Ursache**: Das Dienstkonto gehört nicht einem Administrator des Azure-Abonnements.   

**Lösung**: Verwenden Sie ein Konto, das einem Administrator des Azure-Abonnements gehört und bei dem die Abrechnung für die Nutzung über die Azure Stack-Bereitstellung erfolgt.

### <a name="expired-or-temporary-password"></a>Abgelaufenes oder temporäres Kennwort
 
```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription [subscription ID] using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change P
assword.
Trace ID: [Trace ID]
Correlation ID: [Correlation ID]
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Ursache**: Das Konto kann sich nicht anmelden, da das Kennwort entweder abgelaufen oder temporär ist.     

**Lösung**: Führen Sie in PowerShell Folgendes aus, und befolgen Sie die Eingabeaufforderungen zum Zurücksetzen des Kennworts. 

```powershell
Login-AzureRMAccount
``` 

Melden Sie sich alternativ mit dem Konto unter https://portal.azure.com an. Der Benutzer wird dann zur Änderung des Kennworts aufgefordert.

### <a name="unknown-user-type"></a>Unbekannter Benutzertyp  

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription <subscription ID> using <account> failed with unknown_user_type: Unknown User Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Ursache**: Das Konto kann sich nicht bei der angegebenen Azure Active Directory-Umgebung anmelden. In diesem Beispiel ist *AzureChinaCloud* für *AzureEnvironment* angegeben.  

**Lösung**: Vergewissern Sie sich, dass das Konto für die angegebene Azure-Umgebung gültig ist. Führen Sie in PowerShell Folgendes aus, um zu überprüfen, ob das Konto für eine bestimmte Umgebung gültig ist:
     
```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen der Azure-Identität](azure-stack-validate-identity.md)
- [Anzeigen des Bereitschaftsberichts](azure-stack-validation-report.md)
- [Allgemeine Überlegungen zur Azure Stack-Integration](azure-stack-datacenter-integration.md)

