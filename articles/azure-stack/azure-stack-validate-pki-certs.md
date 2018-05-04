---
title: Überprüfen von PKI-Zertifikaten für Azure Stack für die Bereitstellung von in Azure Stack integrierten Systemen | Microsoft-Dokumentation
description: Beschreibt die Vorgehensweise zum Überprüfen von Azure Stack-PKI-Zertifikaten für in Azure Stack integrierte Systeme. Enthält Informationen zur Verwendung des Zertifikatüberprüfungstools von Azure Stack.
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
ms.date: 04/11/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: cd917165804314f6ee4ee006e3f29263d8d4b4c5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="validate-azure-stack-pki-certificates"></a>Überprüfen von Azure Stack-PKI-Zertifikaten

Das in diesem Artikel beschriebene Tool Azure Stack Readiness Checker steht [im PowerShell-Katalog](https://aka.ms/AzsReadinessChecker) zur Verfügung. Mit diesem Tool können Sie sich vergewissern, dass die [generierten PKI-Zertifikate](azure-stack-get-pki-certs.md) für die Vorabbereitstellung geeignet sind. Planen Sie bei der Zertifikatüberprüfung genügend Zeit ein, um die Zertifikate testen und ggf. neu ausstellen zu können.

Das Readiness Checker-Tool führt folgende Zertifikatüberprüfungen durch:

- **Lesen der PFX-Datei**  
    Sucht nach einer gültigen PFX-Datei und einem korrekten Kennwort und gibt eine Warnung aus, falls öffentliche Informationen nicht durch das Kennwort geschützt sind. 
- **Signaturalgorithmus**  
    Überprüft, ob SHA1 als Signaturalgorithmus verwendet wird (was nicht der Fall sein darf).
- **Privater Schlüssel**  
    Überprüft, ob der private Schlüssel vorhanden ist und mit dem Attribut „Lokaler Computer“ exportiert wird. 
- **Vertrauenskette**  
    Überprüft, ob die Vertrauenskette intakt ist (auch für selbstsignierte Zertifikate).
- **DNS-Namen**  
    Überprüft, ob das SAN relevante DNS-Namen für die einzelnen Endpunkte enthält oder ob ein unterstützender Platzhalter vorhanden ist.
- **Schlüsselverwendung**  
    Überprüft, ob die Schlüsselverwendung eine digitale Signatur und eine Schlüsselchiffrierung enthält und ob die erweiterte Schlüsselverwendung eine Serverauthentifizierung und eine Clientauthentifizierung enthält.
- **Schlüsselgröße**  
    Überprüft, ob die Schlüsselgröße mindestens 2.048 beträgt.
- **Kettenreihenfolge**  
    Überprüft, ob die Reihenfolge der anderen Zertifikate, aus denen sich die Kette zusammensetzt, korrekt ist.
- **Andere Zertifikate**  
    Stellt sicher, dass die PFX-Datei nur das relevante Blattzertifikat und dessen Kette enthält und keine anderen Zertifikate in die Datei gepackt wurden.

> [!IMPORTANT]  
> Das PKI-Zertifikat ist eine PFX-Datei, und das Kennwort muss vertraulich behandelt werden.

## <a name="prerequisites"></a>Voraussetzungen

Zur Überprüfung von PKI-Zertifikaten für eine Azure Stack-Bereitstellung muss Ihr System über Folgendes verfügen:

- Microsoft Azure Stack Readiness Checker
- Mindestens ein SSL-Zertifikat, das gemäß den [Vorbereitungsanweisungen](azure-stack-prepare-pki-certs.md) exportiert wurde
- DeploymentData.json
- Windows 10 oder Windows Server 2016

## <a name="perform-certificate-validation"></a>Durchführen der Zertifikatüberprüfung

Gehen Sie wie folgt vor, um die Azure Stack-PKI-Zertifikate vorzubereiten und zu überprüfen:

1. Führen Sie an einer PowerShell-Eingabeaufforderung (5.1 oder höher) das folgende Cmdlet aus, um AzsReadinessChecker zu installieren:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker 
    ````

2. Erstellen Sie die Zertifikatverzeichnisstruktur. Im folgenden Beispiel können Sie für `<c:\certificates>` einen neuen Verzeichnispfad Ihrer Wahl angeben.

    ````PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','ADFS','Admin Portal','ARM Admin','ARM Public','Graph','KeyVault','KeyVaultInternal','Public Portal' 

    $destination = 'c:\certificates' 

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
    ````

 - Platzieren Sie Ihre Zertifikate in den entsprechenden Verzeichnissen aus dem vorherigen Schritt. Beispiele:  
    - C:\certificates\ACSBlob\CustomerCertificate.pfx 
    - C:\certificates\Certs\Admin Portal\CustomerCertificate.pfx 
    - C:\certificates\Certs\ARM Admin\CustomerCertificate.pfx 
    - Und so weiter. 

3. Führen Sie im PowerShell-Fenster Folgendes aus:

    ````PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ````

4. Vergewissern Sie sich anhand der Ausgabe, dass alle Zertifikate die Tests bestanden haben. Beispiel: 

    ````PowerShell
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Validation

    Starting Azure Stack Certificate Validation 1.1803.405.3
    Testing: ARM Public\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: ACSBlob\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessReport.json
    AzsReadinessChecker Completed
    ````

### <a name="known-issues"></a>Bekannte Probleme

**Symptom:** Tests werden übersprungen.

**Ursache:** AzsReadinessChecker überspringt bestimmte Tests, wenn eine Abhängigkeit nicht erfüllt ist:

 - „Andere Zertifikate“ wird übersprungen, wenn „Vertrauenskette“ nicht erfolgreich ist.

    ````PowerShell  
    Testing: ACSBlob\singlewildcard.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: Fail
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: Skipped
    Details:
    The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
    The Other Certificates check was skipped because Cert Chain and/or DNS Names failed. Follow the guidance to remediate those issues and recheck. 
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

**Lösung:** Folgen Sie den Toolanweisungen im Detailabschnitt unter den Tests des jeweiligen Zertifikats.

## <a name="using-validated-certificates"></a>Verwenden überprüfter Zertifikate

Nach der Überprüfung durch AzsReadinessChecker können Sie die Zertifikate in Ihrer Azure Stack-Bereitstellung oder für die Geheimnisrotation von Azure Stack verwenden. 

 - Bereitstellung: Übertragen Sie Ihre Zertifikate auf sichere Weise an Ihren Bereitstellungstechniker, damit dieser sie auf den Bereitstellungshost kopieren kann (wie unter [Azure Stack-PKI-Zertifikatanforderungen](azure-stack-pki-certs.md) beschrieben).
 - Geheimnisrotation: Sie können die Zertifikate verwenden, um ältere Zertifikate für die Endpunkte der öffentlichen Infrastruktur von Azure Stack zu aktualisieren. Eine entsprechende Anleitung finden Sie unter [Rotieren von Geheimnissen in Azure Stack](azure-stack-rotate-secrets.md).

## <a name="next-steps"></a>Nächste Schritte

[Azure Stack-Datencenterintegration: Identität](azure-stack-integrate-identity.md)
