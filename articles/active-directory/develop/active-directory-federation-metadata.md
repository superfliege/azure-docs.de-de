---
title: Azure AD-Verbundmetadaten | Microsoft Docs
description: "Dieser Artikel beschreibt das Verbundmetadaten-Dokument, das Azure Active Directory für Dienste veröffentlicht, die Azure Active Directory-Token akzeptiert werden."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ecafb02a6ac13d1c3cd1fe77ef710cd8525e32b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="federation-metadata"></a>Verbundmetadaten
Azure Active Directory (Azure AD) veröffentlicht ein verbundmetadatendokument für Dienste, die die Sicherheitstoken akzeptiert, die Azure AD ausstellt. Die Verbund-Metadaten-Dokumentformat ist beschrieben, in der [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), die eine Erweiterung [Metadaten für die OASIS Security Assertion Markup Language (SAML) v2. 0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Mandantenspezifische und mandantenunabhängige Metadatenendpunkte
Azure AD veröffentlicht mandantenspezifische und mandantenunabhängige Endpunkte.

Mandantenspezifische Endpunkte sind für einen bestimmten Mandanten ausgelegt. Die mandantenspezifischen Verbundmetadaten enthält Informationen über den Mandanten, einschließlich mandantenspezifischer Aussteller- und Endpunktinformationen Informationen. Anwendungen, die Zugriff auf einen einzelnen Mandanten beschränken, verwenden mandantenspezifische Endpunkte.

Mandantenunabhängige Endpunkte bieten Informationen, die für alle Azure AD-Mandanten gemein sind. Diese Informationen betreffen Mandanten an gehostete *login.microsoftonline.com* und mehreren Mandanten gemeinsam verwendet. Mandantenunabhängige Endpunkte werden für Anwendungen mit mehreren Mandanten, empfohlen, da es keine keinem bestimmten Mandanten zugeordnet sind.

## <a name="federation-metadata-endpoints"></a>Verbundmetadatenendpunkte
Azure AD veröffentlicht Verbundmetadaten unter `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Für **mandantenspezifische Endpunkte**die `TenantDomainName` kann eine der folgenden Typen:

* Ein registrierter Domänenname eines Azure AD-Mandanten, z. B.: `contoso.onmicrosoft.com`.
* Die unveränderliche Mandanten-ID der Domäne sind, z. B. `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Für **mandantenunabhängige Endpunkte**, `TenantDomainName` ist `common`. Dieses Dokument Listet nur die Verbundmetadaten-Elemente, die für alle Azure AD-Mandanten gemeinsam verwendet werden, die am login.microsoftonline.com gehostet werden.

Ein mandantenspezifischer Endpunkt kann z. B., `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Der mandantenunabhängige Endpunkt ist [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Sie können das Verbundmetadaten-Dokument anzeigen, indem Sie diese URL in einem Browser eingeben.

## <a name="contents-of-federation-metadata"></a>Inhalt der Verbund-Metadaten
Der folgende Abschnitt enthält Informationen, die von Diensten, die die von Azure AD ausgegebenen Token nutzen benötigt werden.

### <a name="entity-id"></a>Entitäts-ID
Die `EntityDescriptor` Element enthält ein `EntityID` Attribut. Der Wert, der die `EntityID` Attribut stellt die Aussteller, d. h. der Sicherheitstokendienst (STS), die das Token ausgestellt hat. Es ist wichtig, den Aussteller zu überprüfen, wenn Sie ein Token erhalten.

Die folgenden Metadaten zeigen ein Beispiel für mandantenspezifische `EntityDescriptor` Element mit einem `EntityID` Element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Sie können die Mandanten-ID im mandantenunabhängigen Endpunkt durch Ihre Mandanten-ID zum Erstellen eines mandantenspezifisches ersetzen `EntityID` Wert. Der resultierende Wert wird mit dem Aussteller des Tokens identisch sein. Die Strategie ermöglicht eine mehrinstanzenfähige Anwendung um den Aussteller für einen bestimmten Mandanten zu überprüfen.

Die folgenden Metadaten zeigen ein Beispiel für ein mandantenunabhängiges `EntityID` Element. Bitte beachten Sie, dass die `{tenant}` ist ein Literal, kein Platzhalter.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Tokensignaturzertifikate
Wenn ein Dienst ein Token, die von Azure AD-Mandanten ausgestellt wird empfängt, muss die Signatur des Tokens anhand eines Signaturschlüssels überprüft werden, die in das Verbundmetadaten-Dokument veröffentlicht wird. Die Verbundmetadaten enthalten den öffentlichen Teil der Zertifikate, die die Mandanten für die Tokensignatur verwenden. Die Rohdatenbytes Zertifikat angezeigt wird, dem `KeyDescriptor` Element. Das Tokensignaturzertifikat ist gültig für die Signierung nur, wenn der Wert der `use` -Attribut ist `signing`.

Ein von Azure AD veröffentlichtes verbundmetadatendokument kann mehrere Signaturschlüssel, z. B. wenn Azure AD Aktualisierung des Signaturzertifikats vorbereitet haben. Wenn ein verbundmetadatendokument mehr als ein Zertifikat enthält, sollte ein Dienst, der das Token überprüft alle Zertifikate im Dokument unterstützen.

Die folgenden Metadaten zeigen ein Beispiel `KeyDescriptor` Element mit einem Signaturschlüssel.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Die `KeyDescriptor` -Element tritt an zwei Stellen im verbundmetadatendokument; in der WS-Verbund-spezifischen Abschnitt und im SAML-spezifischen Abschnitt. In den beiden Abschnitten veröffentlichten Zertifikate werden identisch sein.

In der WS-Verbund-spezifischen Abschnitt liest ein WS-verbundmetadatenleser die Zertifikate von einer `RoleDescriptor` Element mit der `SecurityTokenServiceType` Typ.

Die folgenden Metadaten zeigen ein Beispiel `RoleDescriptor` Element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

In der SAML-spezifischen Abschnitt liest ein WS-verbundmetadatenleser die Zertifikate von einer `IDPSSODescriptor` Element.

Die folgenden Metadaten zeigen ein Beispiel `IDPSSODescriptor` Element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Es gibt keine Unterschiede im Format von mandantenspezifischen und mandantenunabhängigen Zertifikaten.

### <a name="ws-federation-endpoint-url"></a>WS-Verbund-Endpunkt-URL
Die Verbundmetadaten enthalten die URL, die Azure AD verwendet für das einmalige Anmelden und Abmelden beim WS-Verbund-Protokoll ist. Dieser Endpunkt tritt im die `PassiveRequestorEndpoint` Element.

Die folgenden Metadaten zeigen ein Beispiel `PassiveRequestorEndpoint` -Element für einen mandantenspezifischen Endpunkt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Für den mandantenunabhängigen Endpunkt wird in der WS-Verbund-Endpunkt der WS-Verbund-URL angezeigt, wie im folgenden Beispiel gezeigt.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML-Protokollendpunkt-URL
Die Verbundmetadaten enthalten die URL, die Azure AD für einmaliges Anmelden und Abmelden beim SAML 2.0-Protokoll verwendet. Diese Endpunkte werden in der `IDPSSODescriptor` Element.

Die URLs für an- und Abmeldung angezeigt, der `SingleSignOnService` und `SingleLogoutService` Elemente.

Die folgenden Metadaten zeigen ein Beispiel `PassiveResistorEndpoint` für einen mandantenspezifischen Endpunkt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Auf ähnliche Weise werden die Endpunkte für die allgemeinen SAML 2.0-protokollendpunkte in den mandantenunabhängigen Verbundmetadaten veröffentlicht, wie im folgenden Beispiel gezeigt.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
