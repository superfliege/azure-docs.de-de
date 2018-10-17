---
title: 'Tutorial: Abrufen von Daten per Azure AD-Berichterstellungs-API mit Zertifikaten | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie die Azure AD-Berichterstellungs-API mit Zertifikatanmeldeinformationen verwenden, um Daten ohne Benutzereingriff aus Verzeichnissen abzurufen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 5c54af76fc1e145ea062c6bcb37f354a7de94781
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364175"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Tutorial: Abrufen von Daten per Berichterstellungs-API von Azure Active Directory mit Zertifikaten

Die [Berichtserstellungs-APIs von Azure Active Directory (Azure AD)](concept-reporting-api.md) bieten Ihnen über eine Gruppe von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über verschiedene Programmiersprachen und Tools aufrufen. Wenn Sie ohne Benutzereingriff auf die Azure AD-Berichterstellungs-API zugreifen möchten, müssen Sie für Ihren Zugriff die Verwendung von Zertifikaten konfigurieren.

In diesem Tutorial erfahren Sie, wie Sie ein Testzertifikat erstellen und verwenden, um zu Berichterstellungszwecken auf die MS Graph-API zuzugreifen. Wir empfehlen nicht, das Testzertifikat in einer Produktionsumgebung zu verwenden. 

## <a name="prerequisites"></a>Voraussetzungen

1. Schließen Sie zuerst die [Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API](howto-configure-prerequisites-for-reporting-api.md) ab. 

2. Laden Sie [Azure AD PowerShell V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md) herunter, und installieren Sie die Software.

3. Installieren Sie [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Dieses Modul enthält mehrere Hilfsprogramm-Cmdlets, z.B.:
    - Die für die Authentifizierung benötigten ADAL-Bibliotheken
    - Zugriffstoken für Benutzer, Anwendungsschlüssel und Zertifikate mit Verwendung von ADAL
    - Graph-API zur Verarbeitung von auf Seiten aufgeteilten Ergebnissen

4. Wenn Sie das Modul erstmalig verwenden, führen Sie **Install-MSCloudIdUtilsModule** aus. Ansonsten können Sie es einfach mit dem PowerShell-Befehl **Import-Module** importieren.

Ihre Sitzung sollte etwa wie folgt aussehen:

  ![Windows PowerShell](./media/tutorial-access-api-with-certificates/module-install.png)
  
## <a name="create-a-test-certificate"></a>Erstellen eines Testzertifikats

1. Verwenden Sie das PowerShell-Cmdlet **New-SelfSignedCertificate**, um ein Testzertifikat zu erstellen.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

2. Verwenden Sie das Cmdlet **Export-Certificate**, um es in eine Zertifikatdatei zu exportieren.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="register-the-certificate-in-your-app"></a>Registrieren des Zertifikats in Ihrer App

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), wählen Sie **Azure Active Directory** aus und dann **App-Registrierungen**, und wählen Sie Ihre Anwendung aus der Liste aus. 

2. Wählen Sie **Einstellungen** > **Schlüssel** und dann **Öffentlichen Schlüssel** hochladen aus.

3. Wählen Sie die Zertifikatdatei aus dem vorherigen Schritt aus, und wählen Sie dann **Speichern** aus. 

4. Notieren Sie die Anwendungs-ID und den Fingerabdruck des Zertifikats, das Sie gerade in Ihrer Anwendung registriert haben. Um den Fingerabdruck zu finden, wechseln Sie auf Ihrer Anwendungsseite im Portal zu **Einstellungen**, und klicken Sie auf **Schlüssel**. Der Fingerabdruck befindet sich unter der Liste **Öffentliche Schlüssel**.

5. Öffnen Sie das Anwendungsmanifest im Inline-Manifest-Editor, und ersetzen Sie die Eigenschaft *keyCredentials* durch Ihre neuen Zertifikatinformationen, indem Sie das folgende Schema verwenden. 

   ```
   "keyCredentials": [
        {
            "customKeyIdentifier": "$base64Thumbprint", //base64 encoding of the certificate hash
            "keyId": "$keyid", //GUID to identify the key in the manifest
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value":  "$base64Value" //base64 encoding of the certificate raw data
        }
    ]
   ```

6. Speichern Sie das Manifest. 
  
## <a name="get-an-access-token-for-ms-graph-api"></a>Abrufen eines Zugriffstokens für die MS Graph-API

1. Verwenden Sie das Cmdlet **Get-MSCloudIdMSGraphAccessTokenFromCert** aus dem PowerShell-Modul „MSCloudIdUtils“, und übergeben Sie dabei als Eingabe die Anwendungs-ID und den Fingerabdruck, die Sie im vorherigen Schritt erhalten haben. 

 ![Azure-Portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Verwenden des Zugriffstokens zum Aufrufen der Graph-API

1. Nun können Sie das Zugriffstoken in Ihrem PowerShell-Skript zum Abfragen der Graph-API verwenden. Verwenden Sie das Cmdlet **Invoke-MSCloudIdMSGraphQuery** aus „MSCloudIDUtils“, um die Endpunkte „signins“ und „directoryAudits“ aufzuzählen. Mit diesem Cmdlet werden mehrseitige Ergebnisse verarbeitet, die an die PowerShell-Pipeline gesendet werden.

2. Abfragen des Endpunkts „directoryAudits“, um die Überwachungsprotokolle abzurufen. 
 ![Azure-Portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

3. Abfragen des Endpunkts „signins“, um die Anmeldeprotokolle abzurufen.
 ![Azure-Portal](./media/tutorial-access-api-with-certificates/query-signins.png)

4. Sie können nun diese Daten in eine CSV-Datei exportieren und Datei in einem SIEM-System speichern. Außerdem können Sie Ihr Skript mit einer geplanten Aufgabe umschließen, mit der Azure AD-Daten regelmäßig aus Ihrem Mandanten abgerufen werden, ohne dass Anwendungsschlüssel im Quellcode gespeichert werden müssen. 

## <a name="next-steps"></a>Nächste Schritte

* [Verschaffen eines ersten Eindrucks über die Berichterstellungs-APIs](concept-reporting-api.md)
* [Referenz zur Überwachungs-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referenz zur Anmeldeaktivitätsbericht-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



