---
title: 'Gewusst wie: Verwenden von PowerShell zum Erstellen von X.509-Zertifikaten | Microsoft-Dokumentation'
description: Verwenden von PowerShell zum lokalen Erstellen von X.509-Zertifikate und Aktivieren der X.509-basierten Sicherheit in Ihrem Azure IoT Hub in einer simulierten Umgebung.
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: dkshir
ms.openlocfilehash: 31f94686fed376fbeda2ccdcbc5ed001bcda8126
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="powershell-scripts-to-manage-ca-signed-x509-certificates"></a>PowerShell-Skripts zum Verwalten von X.509-Zertifikaten, die von einer Zertifizierungsstelle signiert sind

Die auf dem X.509-Zertifikat basierende Sicherheit in IoT Hub erfordert, dass Sie mit einer [X.509-Zertifikatkette](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification) beginnen, die sowohl das Stammzertifizierungsstellen-Zertifikat als auch alle Zwischenzertifikate bis zum untergeordneten Zertifikat enthält. Diese *Gewusst wie*-Anleitung führt Sie durch PowerShell-Beispielskripts, in denen [OpenSSL](https://www.openssl.org/) zum Erstellen und Signieren von X.509-Zertifikaten verwendet werden. Sie sollten dieses Handbuch nur für Experimente verwenden, da viele dieser Schritte bei Fertigungsprozessen in der realen Welt durchgeführt werden. Sie können diese Zertifikate zum Simulieren von Sicherheit in Ihrem Azure IoT Hub mit der *X.509-Zertifikatauthentifizierung* verwenden. Mit den Schritten in diesem Handbuch werden Zertifikate lokal auf Ihrem Windows-Computer erstellt. 

## <a name="prerequisites"></a>Voraussetzungen
In diesem Tutorial wird vorausgesetzt, dass Sie die OpenSSL-Binärdateien abgerufen haben. Sie können entweder
    - den OpenSSL-Quellcode herunterladen und die Binärdateien auf dem Computer erstellen, oder 
    - beliebige [Binärdateien von OpenSSL-Drittanbietern](https://wiki.openssl.org/index.php/Binaries) herunterladen und installieren, z.B. von [diesem Projekt auf SourceForge](https://sourceforge.net/projects/openssl/).

<a id="createcerts"></a>

## <a name="create-x509-certificates"></a>Erstellen von X.509-Zertifikaten
Die folgenden Schritte zeigen ein Beispiel für das lokale Erstellen der X.509-Zertifikate der Stammzertifizierungsstelle. 

1. Öffnen Sie ein PowerShell-Fenster als *Administrator*. 
2. Navigieren Sie zu Ihrem Arbeitsverzeichnis. Führen Sie das folgende Skript aus, um die globalen Variablen festzulegen. 
    ```PowerShell
    $openSSLBinSource = "<full_path_to_the_binaries>\OpenSSL\bin"
    $errorActionPreference    = "stop"

    # Note that these values are for test purpose only
    $_rootCertSubject         = "CN=Azure IoT Root CA"
    $_intermediateCertSubject = "CN=Azure IoT Intermediate {0} CA"
    $_privateKeyPassword      = "123"

    $rootCACerFileName          = "./RootCA.cer"
    $rootCAPemFileName          = "./RootCA.pem"
    $intermediate1CAPemFileName = "./Intermediate1.pem"
    $intermediate2CAPemFileName = "./Intermediate2.pem"
    $intermediate3CAPemFileName = "./Intermediate3.pem"

    $openSSLBinDir              = Join-Path $ENV:TEMP "openssl-bin"

    # Whether to use ECC or RSA.
    $useEcc                     = $true
    ```
3. Führen Sie das folgende Skript aus, das die OpenSSL-Binärdateien in das Arbeitsverzeichnis kopiert und die Umgebungsvariablen festlegt:

    ```PowerShell
    function Initialize-CAOpenSSL()
    {
        Write-Host ("Beginning copy of openssl binaries to {0} (and setting up env variables...)" -f $openSSLBinDir)
        if (-not (Test-Path $openSSLBinDir))
        {
            mkdir $openSSLBinDir | Out-Null
        }

        robocopy $openSSLBinSource $openSSLBinDir * /s 
        robocopy $openSSLBinSource . * /s 

        Write-Host "Setting up PATH and other environment variables."
        $ENV:PATH += "; $openSSLBinDir"
        $ENV:OPENSSL_CONF = Join-Path $openSSLBinDir "openssl.cnf"

        Write-Host "Success"
    }
    Initialize-CAOpenSSL
    ```
4. Führen Sie als Nächstes das folgende Skript aus, das prüft, ob ein Zertifikat mit dem angegebenen *Antragstellernamen* bereits installiert ist, und ob OpenSSL auf dem Computer richtig konfiguriert ist:
    ```PowerShell
    function Get-CACertBySubjectName([string]$subjectName)
    {
        $certificates = gci -Recurse Cert:\LocalMachine\ |? { $_.gettype().name -eq "X509Certificate2" }
        $cert = $certificates |? { $_.subject -eq $subjectName -and $_.PSParentPath -eq "Microsoft.PowerShell.Security\Certificate::LocalMachine\My" }
        if ($NULL -eq $cert)
        {
            throw ("Unable to find certificate with subjectName {0}" -f $subjectName)
        }
    
        write $cert
    }
    function Test-CAPrerequisites()
    {
        $certInstalled = $null
        try
        {
            $certInstalled = Get-CACertBySubjectName $_rootCertSubject
        }
        catch {}

        if ($NULL -ne $certInstalled)
        {
            throw ("Certificate {0} already installed.  Cleanup CA certs 1st" -f $_rootCertSubject)
        }

        if ($NULL -eq $ENV:OPENSSL_CONF)
        {
            throw ("OpenSSL not configured on this system.  Run 'Initialize-CAOpenSSL' (even if you've already done so) to set everything up.")
        }
        Write-Host "Success"
    }
    Test-CAPrerequisites
    ```
    Wenn alles richtig konfiguriert ist, sollten Sie die „Erfolgreich“-Meldung sehen. 

<a id="createcertchain"></a>

## <a name="create-x509-certificate-chain"></a>Erstellen einer X.509-Zertifikatkette
Erstellen Sie eine Zertifikatkette mit einer Stammzertifizierungsstelle, z.B. „CN=Azure IoT Root CA“, die dieses Beispiel verwendet, indem Sie das folgende PowerShell-Skript ausführen. Dieses Skript aktualisiert auch den Zertifikatsspeicher Ihres Windows-Betriebssystems und erstellt Zertifikatsdateien in Ihrem Arbeitsverzeichnis. 
    1. Das folgende Skript erstellt eine PowerShell-Funktion zum Erstellen eines selbstsignierten Zertifikats für einen bestimmten *Antragstellernamen* und eine Signaturstelle. 
    ```PowerShell
    function New-CASelfsignedCertificate([string]$subjectName, [object]$signingCert, [bool]$isASigner=$true)
    {
        # Build up argument list
        $selfSignedArgs =@{"-DnsName"=$subjectName; 
                           "-CertStoreLocation"="cert:\LocalMachine\My";
                           "-NotAfter"=(get-date).AddDays(30); 
                          }

        if ($isASigner -eq $true)
        {
            $selfSignedArgs += @{"-KeyUsage"="CertSign"; }
            $selfSignedArgs += @{"-TextExtension"= @(("2.5.29.19={text}ca=TRUE&pathlength=12")); }
        }
        else
        {
            $selfSignedArgs += @{"-TextExtension"= @("2.5.29.37={text}1.3.6.1.5.5.7.3.2,1.3.6.1.5.5.7.3.1", "2.5.29.19={text}ca=FALSE&pathlength=0")  }
        }

        if ($signingCert -ne $null)
        {
            $selfSignedArgs += @{"-Signer"=$signingCert }
        }

        if ($useEcc -eq $true)
        {
            $selfSignedArgs += @{"-KeyAlgorithm"="ECDSA_nistP256";
                             "-CurveExport"="CurveName" }
        }

        # Now use splatting to process this
        Write-Host ("Generating certificate {0} which is for prototyping, NOT PRODUCTION.  It will expire in 30 days." -f $subjectName)
        write (New-SelfSignedCertificate @selfSignedArgs)
    }
    ``` 
    2. Die folgende PowerShell-Funktion erstellt X.509-Zwischenzertifikate sowohl unter Verwendung der vorhergehenden Funktion als auch der OpenSSL-Binärdateien. 
    ```PowerShell
    function New-CAIntermediateCert([string]$subjectName, [Microsoft.CertificateServices.Commands.Certificate]$signingCert, [string]$pemFileName)
    {
        $certFileName = ($subjectName + ".cer")
        $newCert = New-CASelfsignedCertificate $subjectName $signingCert
        Export-Certificate -Cert $newCert -FilePath $certFileName -Type CERT | Out-Null
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\CA" -FilePath $certFileName | Out-Null

        # Store public PEM for later chaining
        openssl x509 -inform deer -in $certFileName -out $pemFileName

        del $certFileName
   
        write $newCert
    }  
    ```
    3. Die folgende PowerShell-Funktion erstellt die X.509-Zertifikatkette. Weitere Informationen finden Sie unter [Certificate chains and cross-certification](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification) (Zertifikatketten und Kreuzzertifizierung).
    ```PowerShell
    function New-CACertChain()
    {
        Write-Host "Beginning to install certificate chain to your LocalMachine\My store"
        $rootCACert =  New-CASelfsignedCertificate $_rootCertSubject $null
    
        Export-Certificate -Cert $rootCACert -FilePath $rootCACerFileName  -Type CERT
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\Root" -FilePath $rootCACerFileName

        openssl x509 -inform der -in $rootCACerFileName -out $rootCAPemFileName

        $intermediateCert1 = New-CAIntermediateCert ($_intermediateCertSubject -f "1") $rootCACert $intermediate1CAPemFileName
        $intermediateCert2 = New-CAIntermediateCert ($_intermediateCertSubject -f "2") $intermediateCert1 $intermediate2CAPemFileName
        $intermediateCert3 = New-CAIntermediateCert ($_intermediateCertSubject -f "3") $intermediateCert2 $intermediate3CAPemFileName
        Write-Host "Success"
    }    
    ```
    Dieses Skript erstellt eine Datei namens *RootCA.cer* in Ihrem Arbeitsverzeichnis. 
    4. Verwenden Sie abschließend die oben beschriebenen PowerShell-Funktionen, um die X.509-Zertifikatkette zu erstellen, indem Sie den Befehl `New-CACertChain` in Ihrem PowerShell-Fenster ausführen. 


<a id="signverificationcode"></a>

## <a name="proof-of-possession-of-your-x509-ca-certificate"></a>Nachweis des rechtmäßigen Besitzes Ihres X.509-Zertifikats

Dieses Skript führt den *Nachweis des rechtmäßigen Besitzes* für Ihr X.509-Zertifikat aus. 

Führen Sie im PowerShell-Fenster auf Ihrem Desktop folgenden Code aus:
    ```PowerShell
    function New-CAVerificationCert([string]$requestedSubjectName)
    {
        $cnRequestedSubjectName = ("CN={0}" -f $requestedSubjectName)
        $verifyRequestedFileName = ".\verifyCert4.cer"
        $rootCACert = Get-CACertBySubjectName $_rootCertSubject
        Write-Host "Using Signing Cert:::" 
        Write-Host $rootCACert
    
        $verifyCert = New-CASelfsignedCertificate $cnRequestedSubjectName $rootCACert $false

        Export-Certificate -cert $verifyCert -filePath $verifyRequestedFileName -Type Cert
        if (-not (Test-Path $verifyRequestedFileName))
        {
            throw ("Error: CERT file {0} doesn't exist" -f $verifyRequestedFileName)
        }
    
        Write-Host ("Certificate with subject {0} has been output to {1}" -f $cnRequestedSubjectName, (Join-Path (get-location).path $verifyRequestedFileName)) 
    }
    New-CAVerificationCert "<your verification code>"
    ```
   Dadurch wird ein Zertifikat mit dem angegebenen Antragstellernamen, das von der Zertifizierungsstelle signiert wurde, als eine Datei namens *VerifyCert4.cer* in Ihrem Arbeitsverzeichnis erstellt. Diese Zertifikatsdatei unterstützt die Überprüfung auf Ihrem IoT Hub, ob Sie die Signierungsberechtigung (d.h. den privaten Schlüssel) dieser Zertifizierungsstelle besitzen.


<a id="createx509device"></a>

## <a name="create-leaf-x509-certificate-for-your-device"></a>Erstellen eines Blattknoten-X.509-Zertifikats für Ihr Gerät

In diesem Abschnitt wird gezeigt, dass Sie ein PowerShell-Skript verwenden können, das ein Blattknotengeräte-Zertfikat und die entsprechende Zertifikatkette erstellt. 

Führen Sie auf dem lokalen Computer im PowerShell-Fenster das folgende Skript aus, um ein von einer Zertifizierungsstelle signiertes X.509-Zertifikat für dieses Gerät zu erstellen:
    ```PowerShell
    function New-CADevice([string]$deviceName, [string]$signingCertSubject=$_rootCertSubject)
    {
        $cnNewDeviceSubjectName = ("CN={0}" -f $deviceName)
        $newDevicePfxFileName = ("./{0}.pfx" -f $deviceName)
        $newDevicePemAllFileName      = ("./{0}-all.pem" -f $deviceName)
        $newDevicePemPrivateFileName  = ("./{0}-private.pem" -f $deviceName)
        $newDevicePemPublicFileName   = ("./{0}-public.pem" -f $deviceName)
    
        $signingCert = Get-CACertBySubjectName $signingCertSubject ## "CN=Azure IoT CA Intermediate 1 CA"

        $newDeviceCertPfx = New-CASelfSignedCertificate $cnNewDeviceSubjectName $signingCert $false
    
        $certSecureStringPwd = ConvertTo-SecureString -String $_privateKeyPassword -Force -AsPlainText

        # Export the PFX of the cert we've just created.  The PFX is a format that contains both public and private keys.
        Export-PFXCertificate -cert $newDeviceCertPfx -filePath $newDevicePfxFileName -password $certSecureStringPwd
        if (-not (Test-Path $newDevicePfxFileName))
        {
            throw ("Error: CERT file {0} doesn't exist" -f $newDevicePfxFileName)
        }

        # Begin the massaging.  First, turn the PFX into a PEM file which contains public key, private key, and other attributes.
        Write-Host ("When prompted for password by openssl, enter the password as {0}" -f $_privateKeyPassword)
        openssl pkcs12 -in $newDevicePfxFileName -out $newDevicePemAllFileName -nodes

        # Convert the PEM to get formats we can process
        if ($useEcc -eq $true)
        {
            openssl ec -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
        }
        else
        {
            openssl rsa -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
        }
        openssl x509 -in $newDevicePemAllFileName -out $newDevicePemPublicFileName
 
        Write-Host ("Certificate with subject {0} has been output to {1}" -f $cnNewDeviceSubjectName, (Join-Path (get-location).path $newDevicePemPublicFileName)) 
    }
    ```
   Führen Sie `New-CADevice "<yourTestDevice>"` unter Verwendung des Anzeigenamens, mit dem Sie Ihr Gerät erstellt haben, in Ihrem PowerShell-Fenster aus. Wenn Sie zur Eingabe des Kennworts für den privaten Schlüssel der Zertifizierungsstelle aufgefordert werden, geben Sie „123“ ein. Dies erstellt eine _<yourTestDevice>PFX_-Datei in Ihrem Arbeitsverzeichnis.

