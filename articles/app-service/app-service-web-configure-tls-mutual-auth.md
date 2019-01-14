---
title: Konfigurieren der gegenseitigen TLS-Authentifizierung – Azure App Service
description: Erfahren Sie, wie Sie Ihre App zur Verwendung der Clientzertifikatauthentifizierung für TLS konfigurieren.
services: app-service
documentationcenter: ''
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.custom: seodec18
ms.openlocfilehash: d441329bc3f279e95b2ee302db53d78f786c3470
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2018
ms.locfileid: "53650396"
---
# <a name="how-to-configure-tls-mutual-authentication-for-azure-app-service"></a>Konfigurieren der gegenseitigen TLS-Authentifizierung für Azure App Service
## <a name="overview"></a>Übersicht
Sie können den Zugriff auf Ihre Azure App Service-App einschränken, indem Sie verschiedene Authentifizierungstypen für sie aktivieren. Eine Möglichkeit hierzu ist die Authentifizierung mithilfe eines Clientzertifikats, wenn die Anforderung über TLS/SSL erfolgt. Dieser Mechanismus wird als gegenseitige TLS-Authentifizierung oder Clientzertifikatauthentifizierung bezeichnet. In diesem Artikel wird das Einrichten Ihrer App für die Clientzertifikatauthentifizierung ausführlich beschrieben.

> **Hinweis:** Wenn Sie über HTTP und nicht HTTPS auf Ihre Website zugreifen, erhalten Sie kein Clientzertifikat. Wenn Ihre Anwendung also Clientzertifikate erfordert, sollten Sie keine Anfragen an Ihre Anwendung über HTTP zulassen.
> 
> 

## <a name="configure-app-service-for-client-certificate-authentication"></a>Konfigurieren von App Service für die Clientzertifikatauthentifizierung
Zum Einrichten Ihrer App für die Erzwingung der Nutzung von Clientzertifikaten müssen Sie Ihrer App die Website-Einstellung „clientCertEnabled“ hinzufügen und diese auf „true“ festlegen. Diese Einstellung kann auch im Azure-Portal auf dem Blatt „SSL-Zertifikate“ konfiguriert werden.

Sie können das Tool [ARMClient](https://github.com/projectkudu/ARMClient) zum Vereinfachen des REST-API-Aufrufs verwenden. Nachdem Sie sich mit dem Tool angemeldet haben, müssen Sie den folgenden Befehl ausführen:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

Ersetzen Sie alle Daten in {} durch Informationen für Ihre App, und erstellen Sie die Datei „enableclientcert.json“ mit dem folgenden JSON-Inhalt:

    {
        "location": "My App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Passen Sie den Wert von „location“ an die Region Ihrer App an, z.B. „USA, Norden-Mitte“, „USA, Westen“ usw.

Sie können auch https://resources.azure.com verwenden, um die `clientCertEnabled`-Eigenschaft in `true` zu ändern.

> **Hinweis:** Wenn Sie ARMClient von PowerShell aus ausführen, müssen Sie das \@-Symbol für die JSON-Datei mit einem Graviszeichen „`“ als Escapezeichen versehen.
> 
> 

## <a name="accessing-the-client-certificate-from-app-service"></a>Zugreifen auf das Clientzertifikat aus App Service
Wenn Sie ASP.NET verwenden und Ihre App für die Authentifizierung mit Clientzertifikaten konfigurieren, ist das Zertifikat über die **HttpRequest.ClientCertificate** -Eigenschaft verfügbar. Für andere Anwendungsstapel steht das Clientzertifikat Ihrer App über einen mit Base64 codierten Wert im Anforderungsheader „X-ARR-ClientCert“ zur Verfügung. Ihre Anwendung kann anhand dieses Werts ein Zertifikat erstellen und es für Authentifizierungs- und Autorisierungszwecke in Ihrer Anwendung verwenden.

## <a name="special-considerations-for-certificate-validation"></a>Besondere Aspekte bei der Zertifikatüberprüfung
Das Clientzertifikat, das an die Anwendung gesendet wird, durchläuft keinerlei Überprüfung seitens der Azure App Service-Plattform. Für das Überprüfen dieses Zertifikats ist die App zuständig. Hier ist ASP.NET-Beispielcode, mit dem die Zertifikateigenschaften zum Zweck der Authentifizierung überprüft werden.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
