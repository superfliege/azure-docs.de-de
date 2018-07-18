---
title: Verwenden eines hochgeladenen SSL-Zertifikats in Ihrem Anwendungscode in Azure App Service | Microsoft-Dokumentation
description: ''
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cephalin
ms.openlocfilehash: 6800bf766deb2044d400f92dbe370fa15bdd5f00
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
ms.locfileid: "26047601"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Verwenden eines SSL-Zertifikats in Ihrem Anwendungscode in Azure App Service

Diese Anleitung zeigt, wie Sie ein SSL-Zertifikat, das Sie hochgeladen oder in Ihrem Anwendungscode in Ihre App Service-App importiert haben, verwenden. Ein Beispiel eines Anwendungsfalls ist der Zugriff Ihrer App auf einen externen Dienst, der eine Zertifikatauthentifizierung erfordert. 

Dieser Ansatz für die Verwendung von SSL-Zertifikaten in Ihrem Code nutzt die SSL-Funktionalität in App Service. Dazu muss Ihre App mindestens im Tarif **Basic** ausgeführt werden. Als Alternative können Sie die Zertifikatsdatei in Ihr Anwendungsverzeichnis einschließen und direkt laden (siehe [Alternative: Laden von Zertifikaten als Datei](#file)). Bei dieser Alternative können Sie jedoch den privaten Schlüssel im Zertifikat nicht vor dem Anwendungscode oder dem Entwickler verbergen. Wenn Ihr Anwendungscode sich darüber hinaus in einem Open-Source-Repository befindet, ist das Verwalten eines Zertifikats mit einem privaten Schlüssel im Repository keine Option.

Wenn Sie Ihre SSL-Zertifikate über App Service verwalten, können Sie die Zertifikate und Ihren Anwendungscode separat verwalten und Ihre sensiblen Daten schützen.

## <a name="prerequisites"></a>Voraussetzungen

So schließen Sie diese Anleitung ab

- [Erstellen einer App Service-App](/azure/app-service/)
- [Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer Web-App](app-service-web-tutorial-custom-domain.md)
- [Hochladen eines SSL-Zertifikats](app-service-web-tutorial-custom-ssl.md) oder [Importieren eines App Service-Zertifikats](web-sites-purchase-ssl-web-site.md) in Ihre Web-App


## <a name="load-your-certificates"></a>Laden Ihrer Zertifikate

Um ein Zertifikat zu verwenden, das in App Service hochgeladen oder importiert wurde, machen Sie es zunächst für Ihren Anwendungscode zugänglich. Dies erledigen Sie über die App-Einstellung `WEBSITE_LOAD_CERTIFICATES`.

Öffnen Sie im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> die Seite Ihrer Web-App.

Klicken Sie im linken Navigationsbereich auf **SSL-Zertifikate**.

![Zertifikat hochgeladen](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

Hier werden alle Ihre hochgeladenen und importierten SSL-Zertifikate für diese Web-App mit den entsprechenden Fingerabdrücken angezeigt. Kopieren Sie den Fingerabdruck des Zertifikats, das Sie verwenden möchten.

Klicken Sie im linken Navigationsbereich auf **Anwendungseinstellungen**.

Fügen Sie eine App-Einstellung mit dem Namen `WEBSITE_LOAD_CERTIFICATES` hinzu, und legen Sie als Wert den Fingerabdruck des Zertifikats fest. Um mehrere Zertifikate zugänglich zu machen, verwenden Sie durch Trennzeichen getrennte Fingerabdruckwerte. Um alle Zertifikate zugänglich zu machen, legen Sie den Wert auf `*` fest. 

![Konfigurieren von App-Einstellungen](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Klicken Sie abschließend auf **Speichern**.

Das konfigurierte Zertifikat kann nun in Ihrem Code verwendet werden.

## <a name="use-certificate-in-c-code"></a>Verwenden von Zertifikaten in C#-Code

Nachdem Ihr Zertifikat verfügbar ist, greifen Sie in C#-Code über den Fingerabdruck des Zertifikats auf dieses zu. Der folgende Code lädt ein Zertifikat mit dem Fingerabdruck `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="alternative-load-certificate-as-a-file"></a>Alternative: Laden des Zertifikats als Datei

In diesem Abschnitt wird gezeigt, wie Sie eine Zertifikatsdatei laden, die sich in Ihrem Anwendungsverzeichnis befindet. 

Im folgenden C#-Beispiel wird ein Zertifikat namens `mycert.pfx` aus dem Verzeichnis `certs` des Repositorys Ihrer App geladen.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

