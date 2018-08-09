---
title: Informationen zum Bereitstellen optionaler Ansprüche für Ihre Azure AD-Anwendung | Microsoft-Dokumentation
description: Anleitung zum Hinzufügen benutzerdefinierter oder zusätzlicher Ansprüche zu den SAML 2.0- und JWT-Token (JSON Web Token), die von Azure Active Directory ausgestellt werden.
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 6e0b00117c35cd5222c69e72819afb37f9ec14dd
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265063"
---
# <a name="optional-claims-in-azure-ad-preview"></a>Optionale Ansprüche in Azure AD (Preview)

Dieses Feature wird von Anwendungsentwicklern verwendet, um anzugeben, welche Ansprüche in Token enthalten sein sollen, die an ihre Anwendung gesendet werden. Sie können optionale Ansprüche zu folgenden Zwecken verwenden:
-   Auswählen zusätzlicher Ansprüche, die in Token für Ihre Anwendung aufgenommen werden sollen
-   Ändern des Verhaltens bestimmter Ansprüche, die von Azure AD in Token zurückgegeben werden
-   Hinzufügen und Zugreifen auf benutzerdefinierte Ansprüche für Ihre Anwendung 

> [!Note]
> Diese Funktion befindet sich derzeit in der Public Preview. Seien Sie darauf vorbereitet, Änderungen zurückzusetzen bzw. zu löschen. Die Funktion ist während der Public Preview-Phase in allen Azure AD-Abonnements verfügbar. Sobald die Funktion aber allgemein verfügbar wird, ist für einige Aspekte des Features unter Umständen ein Azure AD Premium-Abonnement erforderlich.

Die Liste der Standardansprüche und Informationen zu deren Verwendung in Token finden Sie in den [Grundlagen zu den von Azure AD ausgestellten Token](active-directory-token-and-claims.md). 

Eines der Ziele des [v2.0 Azure AD-Endpunkts](active-directory-appmodel-v2-overview.md) ist eine geringere Tokengröße, um optimale Leistung von Clients zu gewährleisten.  Daher sind mehrere Ansprüche, die zuvor in den Zugriffs- und ID-Token enthalten waren, nicht mehr in v2.0-Token vorhanden und müssen für einzelne Anwendungen speziell angefordert werden.  

**Tabelle 1: Anwendbarkeit**

| Kontotyp | V1.0-Endpunkt                      | V2.0-Endpunkt  |
|--------------|------------------------------------|----------------|
| Persönliches Microsoft-Konto  | Nicht zutreffend – stattdessen werden RPS-Tickets verwendet | Unterstützung in Kürze verfügbar |
| Azure AD-Konto          | Unterstützt                          | Unterstützt      |

## <a name="standard-optional-claims-set"></a>Standardmäßige optionale Ansprüche
Die Gruppe optionaler Ansprüche, die standardmäßig zur Verwendung in Anwendungen bereitstehen, sind nachfolgend aufgeführt.  Informationen zum Hinzufügen benutzerdefinierter optionaler Ansprüche für Ihre Anwendung finden Sie unter [Verzeichniserweiterungen](active-directory-optional-claims.md#Configuring-custom-claims-via-directory-extensions) weiter unten. 

> [!Note]
>Die Mehrzahl dieser Ansprüche kann in JWTs für v1.0- und v2.0-Token, jedoch nicht in SAML-Token eingeschlossen werden. Ausnahmen stellen die Ansprüche dar, die einen entsprechenden Hinweis in der Spalte „Tokentyp“ enthalten.  Obwohl optionale Ansprüche derzeit nur für AAD-Benutzer unterstützt werden, wird MSA-Unterstützung hinzugefügt.  Wenn MSA optionale Ansprüche am v2.0-Endpunkt unterstützt, ist in der Spalte „Benutzertyp“ angegeben, ob ein Anspruch für einen AAD- oder MSA-Benutzer verfügbar ist.  

**Tabelle 2: Standardmäßige optionale Ansprüche**

| NAME                        | BESCHREIBUNG   | Tokentyp | Benutzertyp | Notizen  |
|-----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Zeitpunkt der letzten Authentifizierung des Benutzers.  Siehe OpenID Connect-Spezifikation.| JWT        |           |  |
| `tenant_region_scope`      | Region des Ressourcenmandanten | JWT        |           | |
| `signin_state`             | Anspruch für Anmeldestatus   | JWT        |           | 6 Rückgabewerte als Flags:<br> „dvc_mngd“: Gerät wird verwaltet.<br> „dvc_cmp“: Gerät ist kompatibel.<br> „dvc_dmjd“: Gerät ist in die Domäne eingebunden.<br> „dvc_mngd_app“: Gerät wird über MDM verwaltet.<br> „inknownntwk“: Gerät befindet sich in einem bekannten Netzwerk.<br> „kmsi“: „Angemeldet bleiben“ wurde verwendet. <br> |
| `controls`                 | Mehrwertiger Anspruch, der die durch Richtlinien für bedingten Zugriff erzwungenen Sitzungssteuerelemente enthält.  | JWT        |           | 3 Werte:<br> „app_res“: Die App muss präzisere Einschränkungen erzwingen. <br> „ca_enf“: Die Durchsetzung des bedingten Zugriffs wurde verzögert und ist immer noch erforderlich. <br> „no_cookie“: Dieses Token ist für den Austausch gegen ein Cookie im Browser nicht ausreichend. <br>  |
| `home_oid`                 | Bei Gastbenutzern: Die Objekt-ID des Benutzers im Home-Mandanten des Benutzers.| JWT        |           | |
| `sid`                      | Sitzungs-ID, die zur sitzungsbezogenen Abmeldung des Benutzers verwendet wird. | JWT        |           |         |
| `platf`                    | Geräteplattform    | JWT        |           | Beschränkt auf verwaltete Geräte, die den Gerätetyp überprüfen können.|
| `verified_primary_email`   | Stammt von „PrimaryAuthoritativeEmail“ des Benutzers.      | JWT        |           |         |
| `verified_secondary_email` | Stammt von „SecondaryAuthoritativeEmail“ des Benutzers.   | JWT        |           |        |
| `enfpolids`                | IDs erzwungener Richtlinien. Eine Liste der Richtlinien-IDs, die für den aktuellen Benutzer ausgewertet wurden.  | JWT |  |  |
| `vnet`                     | Informationen zum VNET-Spezifizierer    | JWT        |           |      |
| `fwd`                      | IP-Adresse.| JWT    |   | Fügt die ursprüngliche IPv4-Adresse des anfordernden Clients hinzu (wenn innerhalb eines VNET). |
| `ctry`                     | Land des Benutzers | JWT |           | Azure AD gibt den optionalen Anspruch `ctry` zurück, wenn er vorhanden ist und der Wert des Anspruchs aus einem standardmäßigen Ländercode mit zwei Buchstaben besteht, z.B. FR, JP oder SZ. |
| `tenant_ctry`              | Land des Ressourcenmandanten | JWT | | |
| `xms_pdl`          | Bevorzugter Datenspeicherort   | JWT | | Für Multi-Geo-Mandanten ist dies ein aus drei Buchstaben bestehender Code, der anzeigt, in welcher geografischen Region sich der Benutzer befindet.  Weitere Informationen finden Sie unter [Azure Active Directory Connect-Synchronisierung: Konfigurieren des bevorzugten Datenspeicherorts für Office 365-Ressourcen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation). <br> Zum Beispiel: `APC` für Asien-Pazifik. |
| `xms_pl`                   | Bevorzugte Benutzersprache  | JWT ||Die bevorzugte Sprache des Benutzers, falls festgelegt.  Wird in Szenarios mit Gastzugriff aus dem Basismandanten abgerufen.  Format: Sprachraum-Land (z.B.: en-us) |
| `xms_tpl`                  | Bevorzugte Mandantensprache| JWT | | Die bevorzugte Sprache des Ressourcenmandanten, falls festgelegt.  Format: Sprachraum (z.B.: en) |
| `ztdid`                    | ID der Bereitstellung ohne manuelles Eingreifen | JWT | | Die für [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) verwendete Geräteidentität |
| `acct`             | Benutzerkontostatus im Mandanten.   | JWT, SAML | | Wenn der Benutzer dem Mandanten angehört, lautet der Wert `0`.  Bei einem Gastbenutzer lautet der Wert `1`.  |
| `upn`                      | Anspruch „UserPrincipalName“.  | JWT, SAML  |           | Obwohl dieser Anspruch automatisch hinzugefügt wird, können Sie ihn als einen optionalen Anspruch angeben, um zusätzliche Eigenschaften zum Ändern des Verhaltens im Fall eines Gastbenutzer anzufügen.  <br> Zusätzliche Eigenschaften: <br> `include_externally_authenticated_upn` <br> `include_externally_authenticated_upn_without_hash` |

### <a name="v20-optional-claims"></a>Optionale Ansprüche in v2.0
Diese Ansprüche sind in v1.0-Token immer enthalten, jedoch nie in v2.0-Token, sofern nicht angefordert.  Diese Ansprüche gelten nur für JWTs (ID-Token und Zugriffstoken).  

**Tabelle 3: Nur in v2.0 enthaltene optionale Ansprüche**

| JWT-Anspruch     | NAME                            | BESCHREIBUNG                                                                                                                    | Notizen |
|---------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|
| `ipaddr`      | IP-Adresse                      | Die IP-Adresse, von der aus sich der Client angemeldet hat.                                                                                      |       |
| `onprem_sid`  | Lokale Sicherheits-ID |                                                                                                                                |       |
| `pwd_exp`     | Kennwortablaufzeit        | Datum und Uhrzeit, zu der das Kennwort abläuft.                                                                                    |       |
| `pwd_url`     | Kennwortänderungs-URL             | Eine URL, die der Benutzer besuchen kann, um sein Kennwort zu ändern.                                                                        |       |
| `in_corp`     | Innerhalb des Unternehmensnetzwerks        | Signalisiert, ob sich der Client aus dem Unternehmensnetzwerk anmeldet. Andernfalls ist der Anspruch nicht enthalten.                     |       |
| `nickname`    | Spitzname                        | Ein zusätzlicher Name für den Benutzer, der vom Vor- oder Nachnamen abweicht.                                                             |       |                                                                                                                |       |
| `family_name` | Last Name (Nachname)                       | Gibt den Nachnamen des Benutzers entsprechend der Definition im Azure AD-Benutzerobjekt an. <br>„family_name“: „Miller“ |       |
| `given_name`  | Vorname                      | Gibt den Vornamen des Benutzers entsprechend der Festlegung im Azure AD-Benutzerobjekt an.<br>„given_name“: „Frank“                   |       |

### <a name="additional-properties-of-optional-claims"></a>Zusätzliche Eigenschaften optionaler Ansprüche

Einige optionale Ansprüche können so konfiguriert werden, dass sie auf andere Art zurückgegeben werden.  Diese zusätzlichen Eigenschaften unterstützen in erster Linie die Migration lokaler Anwendungen mit unterschiedlichen Datenerwartungen. So ist `include_externally_authenticated_upn_without_hash` beispielsweise hilfreich bei Clients, die keine Hashmarks (`#`) im UPN verarbeiten können.

**Tabelle 4: Werte zum Konfigurieren standardmäßiger optionaler Ansprüche**

| Eigenschaftenname                                     | Name der zusätzlichen Eigenschaft                                                                                                             | BESCHREIBUNG |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `upn`                                                 |                                                                                                                                      |  Kann für sowohl für SAML- als auch für JWT-Antworten verwendet werden            |
| | `include_externally_authenticated_upn`              | Bezieht den Gast-UPN ein, wie er im Ressourcenmandanten gespeichert ist.  Zum Beispiel, `foo_hometenant.com#EXT#@resourcetenant.com`                            |             
| | `include_externally_authenticated_upn_without_hash` | Wie oben, außer dass die Rautenzeichen (`#`) durch Unterstriche (`_`) ersetzt werden. Beispiel: `foo_hometenant.com_EXT_@resourcetenant.com` |             

> [!Note]
>Durch Angabe des optionalen Anspruchs „upn“ ohne eine zusätzliche Eigenschaft wird kein Verhalten geändert. Damit ein neuer Anspruch im Token ausgestellt wird, muss mindestens eine der zusätzlichen Eigenschaften hinzugefügt werden. 


#### <a name="additional-properties-example"></a>Beispiel für zusätzliche Eigenschaften:

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

Dieses OptionalClaims-Objekt bewirkt, dass das an den Client zurückgegebene ID-Token einen weiteren UPN mit den zusätzlichen Informationen zum Home- und zum Ressourcenmandanten enthält.  Dies ändert den `upn`-Anspruch im Token nur dann, wenn der Benutzer ein Gastbenutzer im Mandanten ist (und einen anderen Identitätsanbieter für die Authentifizierung verwendet). 

## <a name="configuring-optional-claims"></a>Konfigurieren optionaler Ansprüche

Sie können optionale Ansprüche für Ihre Anwendung konfigurieren, indem Sie das Anwendungsmanifest ändern (siehe Beispiel unten). Weitere Informationen finden Sie in dem Artikel [Grundlegendes zum Azure AD-Anwendungsmanifest](active-directory-application-manifest.md).

**Beispielschema:**

```json
"optionalClaims":  
   {
       "idToken": [
             { 
                   "name": "auth_time", 
                   "essential": false
              }
        ],
 "accessToken": [ 
             {
                    "name": "ipaddr", 
                    "essential": false
              }
        ],
"saml2Token": [ 
              { 
                    "name": "upn", 
                    "essential": true
               },
               { 
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": true
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>„OptionalClaims“-Typ

Deklariert die von einer Anwendung angeforderten optionalen Ansprüche. Eine Anwendung kann optionale Ansprüche so konfigurieren, dass sie in jedem der drei Tokentypen (ID-Token, Zugriffstoken, SAML 2-Token) zurückgegeben werden, die vom Sicherheitstokendienst empfangen werden können. Die Anwendung kann einen anderen Satz optionaler Ansprüche für die Rückgabe im jeweiligen Tokentyp konfigurieren. Die „OptionalClaims“-Eigenschaft der Anwendungsentität ist ein „OptionalClaims“-Objekt.

**Tabelle 5: Eigenschaften des „OptionalClaims“-Typs**

| NAME        | Typ                       | BESCHREIBUNG                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Sammlung (OptionalClaim) | Die optionalen Ansprüche, die im JWT-ID-Token zurückgegeben werden.     |
| `accessToken` | Sammlung (OptionalClaim) | Die optionalen Ansprüche, die im JWT-Zugriffstoken zurückgegeben werden. |
| `saml2Token`  | Sammlung (OptionalClaim) | Die optionalen Ansprüche, die im SAML-Token zurückgegeben werden.       |


### <a name="optionalclaim-type"></a>„OptionalClaim“-Typ

Enthält einen optionalen Anspruch, der einer Anwendung oder einem Dienstprinzipal zugeordnet ist. Die „idToken“-, „accessToken“- und „saml2Token“-Eigenschaften des [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type)-Typs ist eine Sammlung von „OptionalClaim“.
Wenn durch einen bestimmten Anspruch unterstützt, können Sie auch das Verhalten von „OptionalClaim“ mithilfe des Felds „AdditionalProperties“ ändern.

**Tabelle 6: Eigenschaften des „OptionalClaim“-Typs**

| NAME                 | Typ                    | BESCHREIBUNG                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | Der Name des optionalen Anspruchs.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | Die Quelle (Verzeichnisobjekt) des Anspruchs. Es gibt vordefinierte Ansprüche und benutzerdefinierte Ansprüche aus Erweiterungseigenschaften. Wenn der Quellwert „null“ ist, ist der Anspruch ein vordefinierter optionaler Anspruch. Wenn der Quellwert „user“ ist, ist der Wert in der „name“-Eigenschaft die Erweiterungseigenschaft aus dem Benutzerobjekt. |
| `essential`            | Edm.Boolean             | Wenn der Wert „true“ ist, ist der vom Client angegebene Anspruch erforderlich, um eine reibungslose Autorisierung für die jeweilige vom Endbenutzer angeforderte Aufgabe sicherzustellen. Der Standardwert ist „false“.                                                                                                                 |
| `additionalProperties` | Sammlung (Edm.String) | Zusätzliche Eigenschaften des Anspruchs. Wenn eine Eigenschaft in dieser Sammlung vorhanden ist, ändert sie das Verhalten des optionalen Anspruchs, der in der „name“-Eigenschaft angegeben ist.                                                                                                                                                   |

## <a name="configuring-custom-claims-via-directory-extensions"></a>Konfigurieren benutzerdefinierter Ansprüche über Verzeichniserweiterungen

Zusätzlich zu den standardmäßigen optionalen Ansprüchen können Token auch so konfiguriert werden, dass sie Verzeichnisschemaerweiterungen enthalten (weitere Informationen finden Sie im Artikel [Verzeichnisschemaerweiterungen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)).  Diese Funktion ist nützlich, um zusätzliche Benutzerinformationen anzufügen, die von Ihrer App verwendet werden können, z. B. einen zusätzlichen Bezeichner oder eine wichtige Konfigurationsoption, die vom Benutzer festgelegt wurde. 

> [!Note]
> Verzeichnisschemaerweiterungen sind eine auf AAD beschränkte Funktion, d. h., wenn Ihr Anwendungsmanifest eine benutzerdefinierte Erweiterung erfordert und sich ein MSA-Benutzer bei Ihrer App anmeldet, werden diese Erweiterungen nicht zurückgegeben. 

### <a name="values-for-configuring-additional-optional-claims"></a>Werte zum Konfigurieren zusätzlicher optionaler Ansprüche 

Verwenden Sie bei Erweiterungsattributen den vollständigen Namen der Erweiterung (im Format `extension_<appid>_<attributename>`) im Anwendungsmanifest. Die `<appid>` muss mit der ID der Anwendung übereinstimmen, die den Anspruch anfordert. 

Innerhalb des JWT werden diese Ansprüche mit dem folgenden Namensformat ausgegeben: `extn.<attributename>`

Innerhalb der SAML-Token werden diese Ansprüche mit dem folgenden URI-Format ausgegeben: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="optional-claims-example"></a>Beispiel für optionale Ansprüche

In diesem Abschnitt können Sie ein Szenario durchgehen, um zu erfahren, wie Sie optionale Ansprüche für Ihre Anwendung verwenden können.
Es sind mehrere Optionen für das Aktualisieren der Eigenschaften in der Identitätskonfiguration einer Anwendung zum Aktivieren und Konfigurieren optionaler Ansprüche verfügbar:
-   Sie können das Anwendungsmanifest ändern. Im folgenden Beispiel wird diese Methode zum Ausführen der Konfiguration verwendet. Lesen Sie zuerst das Dokument [Grundlegendes zum Azure AD-Anwendungsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest), um eine Einführung in das Manifest zu erhalten.
-   Sie können zum Aktualisieren Ihrer Anwendung auch eine Anwendung schreiben, die die [Graph-API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) nutzt. Die [Referenz für Entitäten und komplexe Typen](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) im Graph-API-Referenzhandbuch kann beim Konfigurieren der optionalen Ansprüche hilfreich sein.

**Beispiel:** Im folgenden Beispiel ändern Sie das Manifest einer Anwendung, um Ansprüche zu Zugriffs-, ID- und SAML-Token hinzuzufügen, die für die Anwendung vorgesehen sind.
1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2.  Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der oberen rechten Ecke der Seite auswählen.
3.  Wählen Sie im Navigationsbereich auf der linken Seite die **Azure AD**-Erweiterung aus, und klicken Sie auf **App-Registrierungen**.
4.  Suchen Sie in der Liste nach der Anwendung, für die Sie optionale Ansprüche konfigurieren möchten, und klicken Sie darauf.
5.  Klicken Sie auf der Anwendungsseite auf **Manifest**, um den Inline-Manifest-Editor zu öffnen. 
6.  Das Manifest kann mit diesem Editor direkt bearbeitet werden. Das Manifest folgt dem Schema für die [Anwendungsentität](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity) und wird nach dem Speichern automatisch formatiert. Neue Elemente werden der `OptionalClaims`-Eigenschaft hinzugefügt.

      ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
      "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
      "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }
      ```
      In diesem Fall wurden verschiedene optionale Ansprüche den einzelnen Tokentypen hinzugefügt, die von der Anwendung empfangen werden können. Die ID-Token enthalten jetzt den UPN für Verbundbenutzer in vollständiger Form (`<upn>_<homedomain>#EXT#@<resourcedomain>`). Die Zugriffstoken empfangen nun den Anspruch „auth_time“. Die SAML-Token enthalten jetzt die Verzeichnisschemaerweiterung „skypeId“ (in diesem Beispiel lautet die App-ID für diese App „ab603c56068041afb2f6832e2a17e237“).  Die SAML-Token stellen die Skype-ID als `extension_skypeId` bereit.

7.  Wenn Sie das Manifest aktualisiert haben, klicken Sie auf **Speichern**, um das Manifest zu speichern.


## <a name="related-content"></a>Verwandte Inhalte
* Erfahren Sie mehr über die [Standardansprüche](active-directory-token-and-claims.md), die von Azure AD bereitgestellt werden. 
