---
title: Hinzufügen von OpenID Connect-Identitätsanbietern in integrierten Richtlinien in Azure Active Directory B2C | Microsoft-Dokumentation
description: Übersichtshandbuch zum Hinzufügen von OpenID Connect-Anbietern in integrierten Richtlinien in Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e4325a7c87ac9975e819b22536838ec438fa1bcd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709562"
---
# <a name="azure-active-directory-b2c-add-a-custom-openid-connect-identity-provider-in-built-in-policies"></a>Azure Active Directory B2C: Hinzufügen eines benutzerdefinierten OpenID Connect-Identitätsanbieters in integrierten Richtlinien

>[!NOTE]
> Dieses Feature befindet sich in der Phase der öffentlichen Vorschau. Verwenden Sie dieses Feature nicht in Produktionsumgebungen.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher angemeldet werden können. Die meisten Identitätsanbieter, die dieses Protokoll verwenden, z.B. [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md), werden in Azure AD B2C unterstützt. In diesem Artikel wird erläutert, wie Sie benutzerdefinierte OpenID Connect-Identitätsanbieter in Ihren integrierten Richtlinien hinzufügen können.

## <a name="configuring-a-custom-openid-connect-identity-provider"></a>Konfigurieren eines benutzerdefinierten OpenID Connect-Identitätsanbieters

So fügen Sie einen benutzerdefinierten OpenID Connect-Identitätsanbieter hinzu:

1. Führen Sie die folgenden Schritte aus, um im Azure-Portal zu den [Azure AD B2C-Einstellungen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) zu wechseln.
1. Klicken Sie auf **Identitätsanbieter**.
1. Klicken Sie auf **Hinzufügen**.
1. Wählen Sie für den **Identitätsanbietertyp** die Option **OpenID Connect** aus.

### <a name="setting-up-the-openid-connect-identity-provider"></a>Einrichten des OpenID Connect-Identitätsanbieters

#### <a name="metadata-url"></a>Metadaten-URL

Laut Spezifikation beschreibt jeder OpenID Connect-Identitätsanbieter ein Metadatendokument, das den Großteil der erforderlichen Informationen für die Anmeldung enthält. Hierzu gehören Informationen wie z.B. die zu verwendenden URLs und der Speicherort der öffentlichen Signaturschlüssel des Dienstes. Das OpenID Connect-Metadatendokument befindet sich immer an einem Endpunkt, der in `.well-known\openid-configuration` endet.

Geben Sie für den OpenID Connect-Identitätsanbieter, den Sie hinzufügen möchten, die Metadaten-URL ein.

#### <a name="client-id-and-secret"></a>Client-ID und Geheimnis

Damit Benutzer sich anmelden können, setzt der Identitätsanbieter voraus, dass Entwickler eine Anwendung in ihrem Dienst registrieren. Diese Anwendung hat eine ID (als **Client-ID** bezeichnet) und ein **Clientgeheimnis**. Kopieren Sie diese Werte vom Identitätsanbieter aus, und geben Sie sie in die entsprechenden Felder ein.

> [!NOTE]
> Das Clientgeheimnis ist optional. Allerdings müssen Sie ein Clientgeheimnis eingeben, wenn Sie den [Autorisierungscodefluss](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth) verwenden möchten, der das Geheimnis für den Austausch des Codes für das Token verwendet.

#### <a name="scope"></a>Umfang

Bereiche definieren die Informationen und Berechtigungen, die Sie bei Ihrem benutzerdefinierten Identitätsanbieter sammeln möchten. OpenID Connect-Anforderungen müssen den `openid`-Bereichswert enthalten, um das ID-Token vom Identitätsanbieter zu empfangen. Ohne das ID-Token können Benutzer sich nicht mit dem benutzerdefinierten Identitätsanbieter bei Azure AD B2C anmelden.

Andere Bereiche können (durch Leerzeichen getrennt) angefügt werden. Entnehmen Sie der Dokumentation des benutzerdefinierten Identitätsanbieters, welche anderen Bereiche möglicherweise zur Verfügung stehen.

#### <a name="response-type"></a>Antworttyp

Der Antworttyp beschreibt, Informationen welcher Art beim ersten Aufruf von `authorization_endpoint` des benutzerdefinierten Identitätsanbieters zurückgesendet werden. 

* `code`: Gemäß dem [Autorisierungscodefluss](http://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth) wird ein Code an Azure AD B2C zurückgegeben. Azure AD B2C fährt dann damit fort, den `token_endpoint` für den Austausch des Codes für das Token aufzurufen.
* `token`: Ein Zugriffstoken wird vom benutzerdefinierten Identitätsanbieter an Azure AD B2C zurückgegeben.
* `id_token`: Ein ID-Token wird vom benutzerdefinierten Identitätsanbieter an Azure AD B2C zurückgegeben.


#### <a name="response-mode"></a>Antwortmodus

Der Antwortmodus definiert die Methode, die zum Senden der Daten vom benutzerdefinierten Identitätsanbieter zurück zu Azure AD B2C verwendet werden sollte.

* `form_post`: Dieser Antwortmodus wird für die optimale Sicherheit empfohlen. Die Antwort wird mit der HTTP-`POST`-Methode übertragen, wobei Code oder Token im `application/x-www-form-urlencoded`-Format codiert wird.
* `query`: Code oder Token wird als Abfrageparameter zurückgegeben.


#### <a name="domain-hint"></a>Domänenhinweis

Der Domänenhinweis kann zum direkten Wechsel zur Anmeldeseite des angegebenen Identitätsanbieters verwendet werden, anstatt den Benutzer eine Auswahl in der Liste der verfügbaren Identitätsanbieter treffen zu lassen. Um diese Art von Verhalten zu ermöglichen, geben Sie einen Wert für den Domänenhinweis ein.

Um direkt zum benutzerdefinierten Identitätsanbieter zu wechseln, fügen Sie den Parameter `domain_hint=<domain hint value>` beim Aufrufen von Azure AD B2C für die Anmeldung am Ende Ihrer Anforderung an.


### <a name="mapping-the-claims-from-the-openid-connect-identity-provider"></a>Zuordnung der Ansprüche vom OpenID Connect-Identitätsanbieter aus

Nachdem der benutzerdefinierte Identitätsanbieter ein ID-Token an Azure AD B2C zurückgesendet hat, muss Azure AD B2C in der Lage sein, die Ansprüche aus dem empfangenen Token den Ansprüchen zuzuordnen, die Azure AD B2C erkennt und verwendet. 

Ziehen Sie für jede der folgenden Zuordnungen die Dokumentation des benutzerdefinierten Identitätsanbieters zu Rate, um die Ansprüche zu verstehen, die in den Token des Identitätsanbieters zurückgegeben werden.

* `User ID`: Geben Sie den Anspruch ein, der den eindeutigen Bezeichner für den angemeldeten Benutzer bereitstellt.
* `Display Name`: Geben Sie den Anspruch ein, der den Anzeigenamen oder den vollständigen Namen des Benutzers bereitstellt.
* `Given Name`: Geben Sie den Anspruch ein, der den Vornamen des Benutzers bereitstellt.
* `Surname`: Geben Sie den Anspruch ein, der den Nachnamen des Benutzers bereitstellt.
* `Email`: Geben Sie den Anspruch ein, der die E-Mail-Adresse des Benutzers bereitstellt.

## <a name="next-steps"></a>Nächste Schritte

Fügen Sie den benutzerdefinierten OpenID Connect-Identitätsanbieter Ihrer [integrierten Richtlinie](active-directory-b2c-reference-policies.md) hinzu.
