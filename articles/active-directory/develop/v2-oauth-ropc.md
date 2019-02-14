---
title: Verwenden von Azure AD v2.0 zum Anmelden von Benutzern mit ROPC | Microsoft-Dokumentation
description: Support für nicht browserbasierte Authentifizierungsflows mit der Gewährung für Kennwortanmeldeinformationen des Ressourcenbesitzers
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 716f48a5db437ef1dc865aca66fbac1c1d51aec3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207431"
---
# <a name="azure-active-directory-v20-and-the-oauth-20-resource-owner-password-credential"></a>Azure Active Directory v2.0 und OAuth 2.0: Kennwortanmeldeinformationen des Ressourcenbesitzers

Azure Active Directory (Azure AD) unterstützt die [Gewährung für Kennwortanmeldeinformationen des Ressourcenbesitzers (ROPC)](https://tools.ietf.org/html/rfc6749#section-4.3). So kann eine Anwendung Benutzer anmelden, indem sie ihr Kennwort direkt verarbeitet. Der ROPC-Flow erfordert ein hohes Maß an Vertrauen und die Freigabe von Benutzeranmeldeinformationen. Entwickler sollten diesen Flow nur dann verwenden, wenn der andere, sicherere Flow nicht genutzt werden kann.

> [!Important]
> * Der Azure AD v2.0-Endpunkt unterstützt nur ROPC für Azure AD-Mandanten – nicht für persönliche Konten. Das bedeutet, Sie müssen einen mandantenspezifischen Endpunkt (`https://login.microsoftonline.com/{TenantId_or_Name}`) oder den Endpunkt `organizations` verwenden.
> * Persönliche Konten, die zu einem Mandanten von Azure AD eingeladen werden, können ROPC nicht verwenden.
> * Konten ohne Kennwörter können sich nicht über ROPC anmelden. Für dieses Szenario wird empfohlen, für Ihre App einen anderen Flow zu verwenden.
> * Wenn Benutzer sich über die mehrstufige Authentifizierung (MFA) in der Anwendung anmelden müssen, werden sie blockiert.

## <a name="protocol-diagram"></a>Protokolldiagramm

Dieses Diagramm zeigt den ROPC-Flow:

![ROPC-Flow](media/v2-oauth2-ropc/v2-oauth-ropc.png)

## <a name="authorization-request"></a>Authorization request (Autorisierungsanforderung)

Der ROPC-Flow ist eine einzelne Anforderung &mdash; die Clientidentifikation und die Anmeldeinformationen des Benutzers werden an den ausstellenden Verteilungspunkt gesendet und im Gegenzug wird ein Token zurückgegeben. Zuvor muss der Client die E-Mail-Adresse des Benutzers und das Kennwort anfordern. Unmittelbar nach einer erfolgreichen Anforderung muss der Client die Anmeldeinformationen des Benutzers sicher aus dem Arbeitsspeicher freigeben. Er muss sie nie speichern.

```
// Line breaks and spaces are for legibility only.

POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token?

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=user.read%20openid%20profile%20offline_access
&client_secret=wkubdywbc2894u
&username=MyUsername@myTenant.com
&password=SuperS3cret
&grant_type=password
```

| Parameter | Bedingung | BESCHREIBUNG |
| --- | --- | --- |
| `tenant` | Erforderlich | Der Verzeichnismandant, bei dem Sie den Benutzer anmelden möchten. Kann als GUID oder als Anzeigename bereitgestellt werden. Dieser Parameter kann nicht auf `common` oder `consumers`, sondern nur auf `organizations` festgelegt werden. |
| `grant_type` | Erforderlich | Muss auf `password` festgelegt sein. |
| `username` | Erforderlich | Die E-Mail-Adresse des Benutzers. |
| `password` | Erforderlich | Das Kennwort des Benutzers. |
| `scope` | Empfohlen | Eine durch Leerzeichen getrennte Liste von [Bereichen](v2-permissions-and-consent.md) oder Berechtigungen, die die App benötigt. Diese Bereiche müssen vorab entweder von einem Administrator oder vom Benutzer in einem interaktiven Flow genehmigt werden. |

### <a name="successful-authentication-response"></a>Erfolgreiche Authentifizierungsantwort

Im Folgenden sehen Sie ein Beispiel für eine erfolgreiche Tokenantwort:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parameter | Format | BESCHREIBUNG |
| --------- | ------ | ----------- |
| `token_type` | Zeichenfolge | Immer auf `Bearer` festgelegt. |
| `scope` | Durch Leerzeichen getrennte Zeichenfolgen | Wenn ein Zugriffstoken zurückgegeben wurde, führt dieser Parameter die Bereiche auf, für die das Zugriffstoken gültig ist. |
| `expires_in`| int | Die Anzahl von Sekunden, die das enthaltene Zugriffstoken gültig ist. |
| `access_token`| Nicht transparente Zeichenfolge | Ausgestellt für die [Bereiche](v2-permissions-and-consent.md), die angefordert wurden. |
| `id_token` | JWT | Ausgestellt, wenn der ursprüngliche `scope`-Parameter den `openid`-Bereich enthalten hat. |
| `refresh_token` | Nicht transparente Zeichenfolge | Ausgestellt, wenn der ursprüngliche `scope`-Parameter `offline_access` enthalten hat. |

Sie können mit Aktualisierungstoken neue Zugriffstoken und Aktualisierungstoken abrufen. Verwenden Sie den in der [Dokumentation für den OAuth-Code](v2-oauth2-auth-code-flow.md#refresh-the-access-token) beschriebenen Flow.

### <a name="error-response"></a>Fehlerantwort

Wenn der Benutzer nicht den richtigen Benutzernamen bzw. das richtige Kennwort angegeben hat oder der Client nicht über die erforderliche Berechtigung verfügt, schlägt die Authentifizierung fehl.

| Error | BESCHREIBUNG | Clientaktion |
|------ | ----------- | -------------|
| `invalid_grant` | Fehler bei der Authentifizierung | Die Anmeldeinformationen waren falsch oder dem Client fehlt die Berechtigung für die angeforderten Bereiche. Wenn die Bereiche nicht gewährt werden, wird ein Unterfehler vom Typ `consent_required` zurückgegeben. In diesem Fall sollte der Client den Benutzer über eine Webansicht oder einen Browser zu einer interaktiven Eingabeaufforderung weiterleiten. |
| `invalid_request` | Anforderung war nicht ordnungsgemäß konstruiert | Der Gewährungstyp wird in den Authentifizierungskontexten `/common` oder `/consumers` nicht unterstützt.  Verwenden Sie stattdessen `/organizations`. |
| `invalid_client` | App wurde nicht ordnungsgemäß eingerichtet | Das kann passieren, wenn die `allowPublicClient`-Eigenschaft im [Anwendungsmanifest](reference-app-manifest.md) nicht auf TRUE gesetzt ist. Die Eigenschaft `allowPublicClient` ist erforderlich, da die ROPC-Gewährung keinen Weiterleitungs-URI hat. Azure AD kann nur feststellen, ob es sich bei der App um eine öffentliche oder eine vertrauliche Clientanwendung handelt, wenn die Eigenschaft festgelegt ist. Beachten Sie, dass ROPC nur für öffentliche Client-Apps unterstützt wird. |

## <a name="learn-more"></a>Weitere Informationen

* Testen Sie ROPC mit der [Beispielkonsolenanwendung](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2).
* Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
