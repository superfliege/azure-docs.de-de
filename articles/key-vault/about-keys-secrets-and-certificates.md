---
title: Informationen zu Schlüsseln, Geheimnissen und Zertifikaten
description: Übersicht über REST-Schnittstelle und KV-Entwicklerdetails
services: key-vault
documentationcenter: ''
author: BryanLa
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: abd1b743-1d58-413f-afc1-d08ebf93828a
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2018
ms.author: bryanla
ms.openlocfilehash: 962f9be53ce0b2022e8e5490bdeb04b2eefb4d7c
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "42140958"
---
# <a name="about-keys-secrets-and-certificates"></a>Informationen zu Schlüsseln, Geheimnissen und Zertifikaten
Mit Azure Key Vault können Benutzer kryptografische Schlüssel in der Microsoft Azure-Umgebung speichern und verwenden. Key Vault unterstützt mehrere Schlüsseltypen und Algorithmen und ermöglicht die Verwendung von Hardwaresicherheitsmodulen (Hardware Security Modules, HSM) für Schlüssel von hohem Wert. Darüber hinaus ermöglicht Key Vault Benutzern das sichere Speichern von geheimen Schlüsseln. Geheime Schlüssel sind Oktettobjekte von begrenzter Größe ohne spezifische Semantik. Key Vault unterstützt auch Zertifikate, die auf Schlüsseln und Geheimnissen aufbauen und ein automatisiertes Verlängerungsfeature hinzufügen.

Weitere Informationen zu Azure Key Vault finden Sie unter [Was ist der Azure-Schlüsseltresor?](/azure/key-vault/key-vault-whatis)

**Allgemeine Informationen zu Key Vault**

-   [Unterstützung von Standards](#BKMK_Standards)
-   [Datentypen](#BKMK_DataTypes)  
-   [Objekte, Bezeichner und Versionsverwaltung](#BKMK_ObjId)  

**Informationen zu Schlüsseln**

-   [Schlüssel und Schlüsseltypen](#BKMK_KeyTypes)  
-   [RSA-Algorithmen](#BKMK_RSAAlgorithms)  
-   [RSA-HSM-Algorithmen](#BKMK_RSA-HSMAlgorithms)  
-   [Kryptografischer Schutz](#BKMK_Cryptographic)
-   [Schlüsselvorgänge](#BKMK_KeyOperations)  
-   [Schlüsselattribute](#BKMK_KeyAttributes)  
-   [Schlüsseltags](#BKMK_Keytags)  

**Informationen zu Geheimnissen** 

-   [Arbeiten mit Geheimnissen](#BKMK_WorkingWithSecrets)  
-   [Geheimnisattribute](#BKMK_SecretAttrs)  
-   [Geheimnistags](#BKMK_SecretTags)  
-   [Geheimniszugriffssteuerung](#BKMK_SecretAccessControl)  

**Informationen zu Zertifikaten**

-   [Zusammensetzung eines Zertifikats](#BKMK_CompositionOfCertificate)  
-   [Zertifikatattribute und Tags](#BKMK_CertificateAttributesAndTags)  
-   [Zertifikatrichtlinie](#BKMK_CertificatePolicy)  
-   [Zertifikataussteller](#BKMK_CertificateIssuer)  
-   [Zertifikatkontakte](#BKMK_CertificateContacts)  
-   [Zertifikatzugriffssteuerung](#BKMK_CertificateAccessControl)  

--

## <a name="key-vault-general-details"></a>Allgemeine Informationen zu Key Vault

Die folgenden Abschnitte enthalten allgemeine Informationen über die Implementierung des Azure Key Vault-Diensts.

###  <a name="BKMK_Standards"></a> Unterstützung von Standards

Die Spezifikationen von JavaScript Object Notation (JSON) und JavaScript Object Signing and Encryption (JOSE) sind wichtige Hintergrundinformationen.  

-   [JSON Web Key (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web Encryption (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web Algorithms (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web Signature (JWS)](http://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="BKMK_DataTypes"></a> Datentypen

In den [JOSE-Spezifikationen](#BKMK_Standards) finden Sie relevante Datentypen für Schlüssel, Verschlüsselung und Signatur.  

-   **algorithm**: ein unterstützter Algorithmus für einen Schlüsselvorgang, z.B. RSA1_5.  
-   **ciphertext-value**: Verschlüsselungstextoktette, codiert mit Base64URL.  
-   **digest-value**: die Ausgabe eines Hashalgorithmus, codiert mit Base64URL.  
-   **key-type**: einer der unterstützten Schlüsseltypen, z.B. RSA.  
-   **plaintext-value**: Klartextoktette, codiert mit Base64URL.  
-   **signature-value**: die Ausgabe eines Signaturalgorithmus, codiert mit Base64URL.  
-   **base64URL**: ein mit Base64URL [RFC4648] codierter Binärwert.  
-   **boolean**: entweder TRUE oder FALSE.  
-   **Identity**: eine Identität in Azure Active Directory (AAD).  
-   **IntDate**: ein dezimaler JSON-Wert, der die Anzahl von Sekunden von 1970-01-01T0:0:0Z UTC bis zum angegebenen UTC-Datum / zur angegebenen UTC-Uhrzeit darstellt. Details in Bezug auf Datum/Uhrzeit im Allgemeinen und UTC im Besonderen finden Sie unter [RFC3339].  

###  <a name="BKMK_ObjId"></a> Objekte, Bezeichner und Versionsverwaltung

In Azure Key Vault gespeicherte Objekte behalten ihre Versionen bei, wenn eine neue Instanz eines Objekts erstellt wird, und jede Version verfügt über einen eindeutigen Bezeichner und eine URL. Wenn ein Objekt erstellt wird, erhält es einen eindeutigen Versionsbezeichner und wird als aktuelle Version des Objekts gekennzeichnet. Beim Erstellen einer neuen Instanz mit dem gleichen Objektnamen erhält das neue Objekt einen eindeutigen Versionsbezeichner und wird zur aktuellen Version.  

Objekte in Azure Key Vault können mit dem aktuellen Bezeichner oder einem versionsspezifischen Bezeichner adressiert werden. Im Beispiel liegt ein Schlüssel mit dem Namen MasterKey vor. Das Ausführen von Vorgängen mit dem aktuellen Bezeichner veranlasst das System, die neueste verfügbare Version zu verwenden. Das Ausführen von Vorgängen mit dem versionsspezifischen Bezeichner veranlasst das System, die spezifische Version des Objekts zu verwenden.  

Objekte werden innerhalb von Azure Key Vault mithilfe einer URL eindeutig identifiziert, sodass nie zwei Objekte im System, unabhängig vom geografischen Standort, die gleiche URL haben. Die vollständige URL zu einem Objekt, der Objektbezeichner, besteht aus einem Präfixteil, der den Schlüsseltresor identifiziert, dem Objekttyp, einem vom Benutzer bereitgestellten Objektnamen und einer Objektversion. Beim Objektnamen wird Groß-/Kleinschreibung nicht unterschieden, und er ist unveränderlich. Bezeichner, die nicht die Objektversion enthalten, sind Basisbezeichner.  

Weitere Informationen finden Sie unter [Authentifizierung, Anforderungen und Antworten](authentication-requests-and-responses.md).

Ein Objektbezeichner hat das folgende allgemeine Format:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Hinweis:  

|||  
|-|-|  
|`keyvault-name`|Der Name eines Schlüsseltresors im Microsoft Azure Key Vault-Dienst.<br /><br /> Schlüsseltresornamen werden vom Benutzer ausgewählt und sind global eindeutig.<br /><br /> Der Schlüsseltresornamen muss eine Zeichenfolge sein, die zwischen 3 und 24 Zeichen lang ist, und darf nur die Zeichen 0-9, a-z, A-Z und - enthalten.|  
|`object-type`|Der Typ des Objekts, entweder „Schlüssel“ oder „Geheimnisse“.|  
|`object-name`|Ein `object-name` ist ein vom Benutzer bereitgestellter Name und muss innerhalb eines Schlüsseltresors eindeutig sein. Der Name muss eine Zeichenfolge sein, die zwischen 1 und 127 Zeichen lang ist, und darf nur die Zeichen 0-9, a-z, A-Z und - enthalten.|  
|`object-version`|Ein `object-version` ist ein vom System generierter und 32 Zeichen langer Zeichenfolgenbezeichner, der optional verwendet wird, um eine eindeutige Version eines Objekts zu adressieren.|  

## <a name="key-vault-keys"></a>Key Vault-Schlüssel

###  <a name="BKMK_KeyTypes"></a> Schlüssel und Schlüsseltypen

Kryptografische Schlüssel in Azure Key Vault werden als JSON Web Key-Objekte (JWK) dargestellt. Die grundlegenden Spezifikationen von JWK/JWA sind ebenfalls so erweitert, dass für die Azure Key Vault-Implementierung eindeutige Schlüsseltypen möglich sind, z.B. der Import von Schlüsseln in Azure Key Vault mit Verwendung der für den HSM-Anbieter (Thales) spezifischen Verpackung, um den sicheren Transport von Schlüsseln zu ermöglichen, derart, dass sie nur in den Azure Key Vault-HSMs verwendet werden können.  

- **„Soft“-Schlüssel:** Schlüssel, die in der Software von Key Vault verarbeitet, aber im Ruhezustand unter Verwendung eines Systemschlüssels, der sich in einem HSM befindet, verschlüsselt werden. Clients können einen vorhandenen RSA- oder EC-Schlüssel importieren oder anfordern, dass Azure Key Vault einen generiert.
- **„Hard“-Schlüssel:** Schlüssel, die in einem HSM (Hardwaresicherheitsmodul) verarbeitet werden. Diese Schlüssel werden in einer der HSM Security Worlds von Azure Key Vault geschützt (es gibt in jeder geografischen Region eine Security World, um die Isolation aufrechtzuerhalten). Clients können einen RSA- oder EC-Schlüssel importieren, entweder in „Soft“-Form oder durch Exportieren von einem kompatiblen HSM-Gerät, oder anfordern, dass Azure Key Vault einen generiert. Dieser Schlüsseltyp fügt dem JWK das T-Attribut hinzu, um das HSM-Schlüsselmaterial zu tragen.

     Weitere Informationen zu geografischen Grenzen finden Sie unter [Datenschutz](https://azure.microsoft.com/support/trust-center/privacy/).  

Azure Key Vault unterstützt nur RSA- und EC-Schlüssel (Elliptic Curve). Zukünftige Releases unterstützen möglicherweise andere Schlüsseltypen, z.B. symmetrische.

-   **EC:** „Soft“-Elliptic Curve-Schlüssel
-   **EC-HSM:** „Hard“-Elliptic Curve-Schlüssel
-   **RSA:** „Soft“-RSA-Schlüssel
-   **RSA-HSM:** „Hard“-RSA-Schlüssel

Azure Key Vault unterstützt RSA-Schlüssel der Größen 2.048, 3.072 und 4.096 sowie EC-Schlüssel (Elliptic Curve) der Typen P-256, P-384, P-521 und P-256K.

### <a name="BKMK_Cryptographic"></a> Kryptografischer Schutz

Die kryptografischen Module, die Azure Key Vault verwendet, ob HSM oder Software, werden von FIPS überprüft. Für die Ausführung im FIPS-Modus müssen Sie keine besonderen Maßnahmen ergreifen. Wenn Sie Schlüssel als HSM-geschützt **erstellen** oder **importieren**, ist sichergestellt, dass sie in HSMs verarbeitet werden, die auf FIPS 140-2 Level 2 oder höher überprüft sind. Wenn Sie Schlüssel als softwaregeschützt **erstellen** oder **importieren**, werden sie in kryptografischen Modulen verarbeitet, die auf FIPS 140-2 Level 1 oder höher überprüft sind. Weitere Informationen finden Sie unter [Schlüssel und Schlüsseltypen](#BKMK_KeyTypes).

###  <a name="BKMK_ECAlgorithms"></a> EC-Algorithmen
 Die folgenden Algorithmusbezeichner werden mit EC- und EC-HSM-Schlüsseln in Azure Key Vault unterstützt. 

#### <a name="signverify"></a>SIGN/VERIFY

-   **ES256** – ECDSA für SHA-256-Hashs und -Schlüssel, erstellt mit P-256-Kurve. Dieser Algorithmus wird im RFC 7518 beschrieben.
-   **ES256K** – ECDSA für SHA-256-Hashs und -Schlüssel, erstellt mit P-256K-Kurve. Bei diesem Algorithmus steht die Standardisierung noch aus.
-   **ES384** – ECDSA für SHA-384-Hashs und -Schlüssel, erstellt mit P-384-Kurve. Dieser Algorithmus wird im RFC 7518 beschrieben.
-   **ES512** – ECDSA für SHA-512-Hashs und -Schlüssel, erstellt mit P-521-Kurve. Dieser Algorithmus wird im RFC 7518 beschrieben.

###  <a name="BKMK_RSAAlgorithms"></a> RSA-Algorithmen  
 Die folgenden Algorithmusbezeichner werden mit RSA- und RSA-HSM-Schlüsseln in Azure Key Vault unterstützt.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, ENCRYPT/DECRYPT

-   **RSA1_5**: RSAES-PKCS1-V1_5 [RFC3447] Schlüsselverschlüsselung  
-   **RSA-OAEP**: RSAES unter Verwendung von Optimal Asymmetric Encryption Padding (OAEP) [RFC3447], wobei die Standardparameter durch RFC 3447 in Abschnitt A.2.1 angegeben werden. Diese Standardparameter verwenden eine Hashfunktion von SHA-1 und eine Maskengenerierungsfunktion von MGF1 mit SHA-1.  

#### <a name="signverify"></a>SIGN/VERIFY

-   **RS256**: RSASSA-PKCS-v1_5 mithilfe von SHA-256. Der von der Anwendung bereitgestellte Zusammenfassungswert muss mithilfe von SHA-256 berechnet werden und 32 Byte lang sein.  
-   **RS384**: RSASSA-PKCS-v1_5 mithilfe von SHA-384. Der von der Anwendung bereitgestellte Zusammenfassungswert muss mithilfe von SHA-384 berechnet werden und 48 Byte lang sein.  
-   **RS512**: RSASSA-PKCS-v1_5 mithilfe von SHA-512. Der von der Anwendung bereitgestellte Zusammenfassungswert muss mithilfe von SHA-512 berechnet werden und 64 Byte lang sein.  
-   **RSNULL**: Siehe [RFC2437], ein spezialisierter Anwendungsfall, um bestimmte TLS-Szenarios zu ermöglichen.  

###  <a name="BKMK_KeyOperations"></a> Schlüsselvorgänge

Azure Key Vault unterstützt die folgenden Vorgänge bei Schlüsselobjekten:  

-   **Erstellen**: Ermöglicht einem Client, einen Schlüssel in Azure Key Vault zu erstellen. Der Wert des Schlüssels wird von Azure Key Vault generiert und gespeichert und nicht für den Client freigegeben. Asymmetrische Schlüssel (und in Zukunft Schlüssel mit elliptischen Kurven und symmetrische) können in Azure Key Vault erstellt werden.  
-   **Importieren**: Ermöglicht einem Client, einen vorhandenen Schlüssel in Azure Key Vault zu importieren. Asymmetrische Schlüssel (und in Zukunft Schlüssel mit elliptischen Kurven und symmetrische) können mithilfe einer Anzahl unterschiedlicher Verpackungsmethoden in einem JWK-Konstrukt in Azure Key Vault importiert werden.  
-   **Aktualisieren**: Ermöglicht einem Client mit ausreichenden Berechtigungen, die Metadaten (Schlüsselattribute) zu ändern, die einem zuvor in Azure Key Vault gespeicherten Schlüssel zugeordnet sind.  
-   **Löschen**: Ermöglicht einem Client mit ausreichenden Berechtigungen das Löschen eines Schlüssels aus Azure Key Vault.  
-   **Auflisten**: Ermöglicht einem Client das Auflisten aller Schlüssel in einer bestimmten Azure Key Vault-Instanz.  
-   **Auflisten von Versionen**: Ermöglicht einem Client das Auflisten aller Versionen eines bestimmten Schlüssels in einer bestimmten Azure Key Vault-Instanz.  
-   **Abrufen**: Ermöglicht einem Client das Abrufen des öffentlichen Teils eines bestimmten Schlüssels in einer Azure Key Vault-Instanz.  
-   **Sichern**: Exportiert einen Schlüssel in einer geschützten Form.  
-   **Wiederherstellen**: Importiert einen zuvor gesicherten Schlüssel.  

Weitere Informationen finden Sie unter [Schlüsselvorgänge in der Referenz zur REST-API für Azure Key Vault](/rest/api/keyvault).  

Nachdem ein Schlüssel in Azure Key Vault erstellt wurde, können die folgenden kryptografischen Vorgänge mit dem Schlüssel ausgeführt werden:  

-   **Signieren und Überprüfen**: Streng genommen handelt es sich um den Vorgang „Hash signieren“ oder „Hash überprüfen“, da Azure Key Vault das Hashing von Inhalten als Teil der Signaturerstellung nicht unterstützt. Anwendungen sollten ein Hashing der Daten durchführen, die lokal signiert werden sollen, und dann anfordern, dass Azure Key Vault den Hash signiert. Die Überprüfung von signierten Hashes wird zur Vereinfachung für Anwendungen unterstützt, die möglicherweise keinen Zugriff auf [öffentliches] Schlüsselmaterial haben. Um die Leistung der Anwendung zu optimieren, sollten Überprüfungen lokal ausgeführt werden.  
-   **Verschlüsselung mit Schlüssel / Umschließen**: Ein in Azure Key Vault gespeicherter Schlüssel kann zum Schutz eines anderen Schlüssels, in der Regel eines symmetrischen Inhaltsverschlüsselungsschlüssels (Content Encryption Key, CEK) verwendet werden. Wenn der Schlüssel in Azure Key Vault asymmetrisch ist, wird die Schlüsselverschlüsselung verwendet, z.B. sind RSA-OAEP und die WRAPKEY/UNWRAPKEY-Vorgänge äquivalent zu ENCRYPT/DECRYPT. Wenn der Schlüssel in Azure Key Vault symmetrisch ist, wird das Umschließen des Schlüssels verwendet, z.B. AES-KW. Der WRAPKEY-Vorgang wird zur Vereinfachung für Anwendungen unterstützt, die möglicherweise keinen Zugriff auf [öffentliches] Schlüsselmaterial haben. Um die Leistung der Anwendung zu optimieren, sollten WRAPKEY-Vorgänge lokal ausgeführt werden.  
-   **Verschlüsseln und Entschlüsseln**: Ein in Azure Key Vault gespeicherter Schlüssel kann zum Verschlüsseln oder Entschlüsseln eines einzelnen Datenblocks verwendet werden, dessen Größe sich nach dem Schlüsseltyp und dem ausgewählten Verschlüsselungsalgorithmus richtet. Der Verschlüsselungsvorgang dient zur Vereinfachung für Anwendungen, die möglicherweise keinen Zugriff auf [öffentliches] Schlüsselmaterial haben. Um die Leistung der Anwendung zu optimieren, sollten Verschlüsselungsvorgänge lokal ausgeführt werden.  

Während WRAPKEY/UNWRAPKEY mit asymmetrischen Schlüsseln überflüssig zu sein scheint, weil der Vorgang zu ENCRYPT/DECRYPT äquivalent ist, ist die Verwendung unterschiedlicher Vorgänge wichtig hinsichtlich einer semantischen und Autorisierungstrennung dieser Vorgänge und der Konsistenz, wenn andere Schlüsseltypen von dem Dienst unterstützt werden.  

Azure Key Vault unterstützt keine EXPORT-Vorgänge: Nachdem ein Schlüssel im System bereitgestellt ist, kann er weder extrahiert noch sein Schlüsselmaterial geändert werden.  Jedoch können Benutzer von Azure Key Vault, die ihren Schlüssel möglicherweise für andere Anwendungsfälle benötigen, oder nachdem er in Azure Key Vault gelöscht wurde, die Vorgänge BACKUP und RESTORE verwenden, um den Schlüssel in einer geschützten Form zu exportieren/importieren. Vom BACKUP-Vorgang erstellte Schlüssel können außerhalb von Azure Key Vault nicht verwendet werden. Alternativ kann der IMPORT-Vorgang für mehrere Azure Key Vault-Instanzen verwendet werden.  

Benutzer können die kryptografischen Vorgänge, die Azure Key Vault pro Schlüssel unterstützt, mithilfe der key_ops-Eigenschaft des JWK-Objekts einschränken.  

Weitere Informationen zu JWK-Objekten finden Sie unter [JSON Web Key (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="BKMK_KeyAttributes"></a> Schlüsselattribute

Zusätzlich zum Schlüsselmaterial können die folgenden Attribute angegeben werden. In einer JSON-Anforderung sind die Attribute Schlüsselwort und Klammern, „{“ „}“, auch dann erforderlich, wenn keine Attribute angegeben werden.  

- *enabled*: Boolesch, optional, Standardwert ist **true**. Gibt an, ob der Schlüssel aktiviert und für kryptografische Vorgänge verwendbar ist. Das Attribut *enabled* wird in Verbindung mit *nbf* und *exp* verwendet. Wenn ein Vorgang zwischen *nbf* und *exp* auftritt, wird er nur zugelassen, wenn *enabled* auf **true** gesetzt ist. Vorgänge außerhalb des *nbf* / *exp*-Fensters sind automatisch nicht zugelassen, mit Ausnahme bestimmter Vorgangstypen unter [besonderen Bedingungen](#BKMK_key-date-time-ctrld-ops).
- *nbf*: IntDate, optional, Standardwert ist „now“. Das Attribut *nbf* („not before“, nicht vor) gibt den Zeitpunkt an, vor dem der Schlüssel NICHT für kryptografische Vorgänge verwendet werden darf, mit Ausnahme bestimmter Vorgangstypen unter [besonderen Bedingungen](#BKMK_key-date-time-ctrld-ops). Die Verarbeitung des *nbf*-Attributs erfordert, dass das aktuelle Datum / die aktuelle Uhrzeit mindestens dem Wert des *nbf*-Attributs entsprechen MUSS. Azure Key Vault KANN einen kleinen Spielraum von in der Regel nicht mehr als ein paar Minuten einräumen, um Abweichungen der Uhr zu berücksichtigen. Der Wer MUSS eine Zahl sein, die einen IntDate-Wert enthält.  
- *exp*: IntDate, optional, Standardwert ist „forever“. Das Attribut *exp* („expiration time“, Ablaufzeit) gibt die Ablaufzeit an, nach deren Verstreichen der Schlüssel NICHT für kryptografische Vorgänge verwendet werden darf, mit Ausnahme bestimmter Vorgangstypen unter [besonderen Bedingungen](#BKMK_key-date-time-ctrld-ops). Die Verarbeitung des *exp*-Attributs erfordert, dass das aktuelle Datum / die aktuelle Uhrzeit vor dem Wert des *exp*-Attributs liegen MUSS. Azure Key Vault KANN einen kleinen Spielraum von in der Regel nicht mehr als ein paar Minuten einräumen, um Abweichungen der Uhr zu berücksichtigen. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.  

Es gibt zusätzliche schreibgeschützte Attribute, die in alle Antworten einbezogen werden, die Schlüsselattribute enthalten:  

- *created*: IntDate, optional. Das *created*-Attribut gibt an, wann diese Version des Schlüssels erstellt wurde. Dieser Wert ist NULL für Schlüssel, die vor dem Hinzufügen dieses Attributs erstellt wurden. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.  
- *updated*: IntDate, optional. Das *updated*-Attribut gibt an, wann diese Version des Schlüssels aktualisiert wurde. Dieser Wert ist NULL für Schlüssel, die vor dem Hinzufügen dieses Attributs zuletzt aktualisiert wurden. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.  

Weitere Informationen zu IntDate und anderen Datentypen finden Sie unter [Datentypen](#BKMK_DataTypes).  

#### <a name="BKMK_key-date-time-ctrld-ops"></a> Durch Datum und Uhrzeit gesteuerte Vorgänge

Noch nicht gültige und abgelaufene Schlüssel, die außerhalb des *nbf* / *exp*-Fensters liegen, sind in **decrypt**-, **unwrap**- und **verify**-Vorgängen verwendbar (ohne Rückgabe von „403 Verboten“). Grund für die Verwendung des „Noch-nicht-gültig“-Status ist, den Test eines Schlüssels vor der Verwendung in der Produktion zu ermöglichen. Grund für die Verwendung des abgelaufenen Zustands ist, Wiederherstellungsvorgänge bei Daten zu ermöglichen, die erstellt wurden, als der Schlüssel gültig war. Sie können den Zugriff auf einen Schlüssel auch mittels Key Vault-Richtlinien oder durch Aktualisieren des *enabled*-Schlüsselattributs mit **false** deaktivieren.

Weitere Informationen zu Datentypen finden Sie unter [Datentypen](#BKMK_DataTypes).

Weitere Informationen zu anderen möglichen Attributen finden Sie unter [JSON Web Key (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="BKMK_Keytags"></a> Schlüsseltags

Sie können zusätzliche anwendungsspezifische Metadaten in Form von Tags angeben. Azure Key Vault unterstützt bis zu 15 Tags, von denen jeder einen 256 Zeichen langen Namen und einen Wert von 256 Zeichen aufweisen kann.  

>[!Note]
>Tags sind für Aufrufer lesbar, die über die *list*- oder *get*-Berechtigung für diesen Objekttyp (Schlüssel, Geheimnis oder Zertifikat) verfügen.

###  <a name="BKMK_KeyAccessControl"></a> Schlüsselzugriffssteuerung

Die Zugriffssteuerung für Schlüssel, die von Key Vault verwaltet werden, ist auf der Ebene eines Schlüsseltresors möglich, der als Schlüsselcontainer fungiert. Es gibt eine Zugriffssteuerungsrichtlinie für Schlüssel, die sich von der Zugriffssteuerungsrichtlinie für Geheimnisse in derselben Key Vault-Instanz unterscheidet. Benutzer können einen oder mehrere Tresore für die Aufnahme von Schlüsseln erstellen und müssen eine dem Szenario entsprechende Segmentierung und Verwaltung von Schlüsseln aufrechterhalten. Die Zugriffssteuerung für Schlüssel ist unabhängig von der Zugriffssteuerung für Geheimnisse.  

Die folgenden Berechtigungen können auf Benutzer-/Dienstprinzipalbasis im Schlüsselzugriffssteuerungs-Eintrag für einen Tresor erteilt werden. Diese Berechtigungen spiegeln präzise die für ein Schlüsselobjekt zulässigen Vorgänge wider:  

-   *create*: Erstellen neuer Schlüssel
-   *get*: Lesen des öffentlichen Teils eines Schlüssels sowie seiner Attribute
-   *list*: Auflisten der Schlüssel oder Versionen eines in einem Schlüsseltresor gespeicherten Schlüssels
-   *update*: Aktualisieren der Attribute für einen Schlüssel
-   *delete*: Löschen des Schlüsselobjekts
-   *sign*: Verwenden des Schlüssels zum Signieren von Zusammenfassungen
-   *verify*: Verwenden des Schlüssels zum Überprüfen von Zusammenfassungen
-   *wrapKey*: Verwenden des Schlüssels zum Schützen eines symmetrischen Schlüssels
-   *unwrapKey*: Verwenden des Schlüssels zum Aufheben des Schutzes eines umschlossenen symmetrischen Schlüssels
-   *encrypt*: Verwenden des Schlüssels zum Schützen einer beliebigen Folge von Bytes
-   *decrypt*: Verwenden des Schlüssels zum Aufheben des Schutzes einer Folge von Bytes
-   *import*: Importieren eines Schlüssels in einen Schlüsseltresor
-   *backup*: Sichern eines Schlüssels in einem Schlüsseltresor
-   *restore*: Wiederherstellen eines gesicherten Schlüssels in einem Schlüsseltresor
-   *all*: Alle Berechtigungen

## <a name="key-vault-secrets"></a>Key Vault-Geheimnisse 

###  <a name="BKMK_WorkingWithSecrets"></a> Arbeiten mit Geheimnissen

Geheimnisse sind in Azure Key Vault Oktettsequenzen mit einer jeweiligen maximalen Größe von 25KB. Der Azure Key Vault-Dienst bietet keine Semantik für Geheimnisse; er akzeptiert, verschlüsselt und speichert die Daten lediglich, wobei er einen geheimen Bezeichner („id“) zurückgibt, der zu einem späteren Zeitpunkt zum Aufrufen des Geheimnisses verwendet werden kann.  

Bei hochsensiblen Daten, die in Azure Key Vault gespeichert sind, sollten Clients zusätzliche Schutzebenen in Betracht ziehen, beispielsweise indem sie die Daten mithilfe eines separaten Schutzschlüssels vorab verschlüsseln.  

Azure Key Vault unterstützt auch ein contentType-Feld für Geheimnisse. Clients können den Inhaltstyp, contentType, eines Geheimnisses zur Unterstützung des Interpretierens der geheimen Daten beim Abrufen angeben. Dieses Feld darf maximal 255 Zeichen lang sein. Es gibt keine vordefinierten Werte. Die Verwendungsempfehlung dient als Hinweis für das Interpretieren der geheimen Daten. Wenn eine Implementierung z.B. sowohl Kennwörter als auch Zertifikate als Geheimnisse speichert, verwenden Sie dieses Feld, um anzuzeigen, worum es sich handelt. Es gibt keine vordefinierten Werte.  

###  <a name="BKMK_SecretAttrs"></a> Geheimnisattribute

Zusätzlich zu den Geheimnisdaten können die folgenden Attribute angegeben werden:  

- *exp*: IntDate, optional, Standardwert ist **forever**. Das *exp*-Attribut (Ablaufzeit) gibt die Ablaufzeit an, nach deren Verstreichen die geheimen Daten – außer in [bestimmten Situationen](#BKMK_secret-date-time-ctrld-ops) – NICHT abgerufen werden sollen. Dieses Feld dient nur zu **Informationszwecken**, da es die Benutzer des Azure Key Vault-Diensts darüber informiert, dass ein bestimmter geheimer Schlüssel möglicherweise nicht verwendet werden kann. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält.   
- *nbf*: IntDate, optional, Standardwert ist **now**. Das *nbf*-Attribut („not before“, nicht vor) gibt den Zeitpunkt an, vor dem die geheimen Daten – außer in [bestimmten Situationen](#BKMK_secret-date-time-ctrld-ops) – NICHT abgerufen werden sollen. Dieses Feld dient nur zu **Informationszwecken**. Der Wert MUSS eine Zahl sein, die einen IntDate-Wert enthält. 
- *enabled*: Boolesch, optional, Standardwert ist **true**. Dieses Attribut gibt an, ob die geheimen Daten abgerufen werden können. Das enabled-Attribut wird in Verbindung mit „nbf“ und *exp* verwendet. Wenn ein Vorgang zwischen „nbf“ und „exp“ auftritt, wird er nur zugelassen, wenn „enabled“ auf **true** festgelegt ist. Vorgänge außerhalb des *nbf*- und *exp*-Fensters sind automatisch unzulässig, [bestimmte Situationen](#BKMK_secret-date-time-ctrld-ops) ausgenommen.  

Es gibt zusätzliche schreibgeschützte Attribute, die in alle Antworten einbezogen werden, die Geheimnisattribute enthalten:  

- *created*: IntDate, optional. Das „created“-Attribut gibt an, wann diese Version des Geheimnisses erstellt wurde. Dieser Wert ist NULL für Geheimnisse, die vor dem Hinzufügen dieses Attributs erstellt wurden. Der Wert muss eine Zahl sein, die einen IntDate-Wert enthält.  
- *updated*: IntDate, optional. Das updated-Attribut gibt an, wann diese Version des Geheimnisses aktualisiert wurde. Dieser Wert ist NULL für Geheimnisse, die vor dem Hinzufügen dieses Attributs zuletzt aktualisiert wurden. Der Wert muss eine Zahl sein, die einen IntDate-Wert enthält.

#### <a name="BKMK_secret-date-time-ctrld-ops"></a> Durch Datum und Uhrzeit gesteuerte Vorgänge

Der **get**-Geheimnisvorgang funktioniert für noch nicht gültige und abgelaufene Geheimnisse außerhalb des *nbf* / *exp*-Fensters. Das Aufrufen eines **get**-Geheimnisvorgangs für ein noch nicht gültiges Geheimnis ist zu Testzwecken möglich. Abrufen (**get**) eines abgelaufenen Geheimnisses, kann für Wiederherstellungsvorgänge verwendet werden.

Weitere Informationen zu Datentypen finden Sie unter [Datentypen](#BKMK_DataTypes).  

###  <a name="BKMK_SecretAccessControl"></a> Geheimniszugriffssteuerung

Die Zugriffssteuerung für Geheimnisse, die in Azure Key Vault verwaltet werden, ist auf der Ebene eines Schlüsseltresors möglich, der als Schlüsselcontainer für diese Geheimnisse fungiert. Es gibt eine Zugriffssteuerungsrichtlinie für Geheimnisse, die sich von der Zugriffssteuerungsrichtlinie für Schlüssel in derselben Key Vault-Instanz unterscheidet. Benutzer können einen oder mehrere Tresore für die Aufnahme von Geheimnissen erstellen und müssen eine dem Szenario entsprechende Segmentierung und Verwaltung von Geheimnissen aufrechterhalten. Die Zugriffssteuerung für Geheimnisse ist unabhängig von der Zugriffssteuerung für Schlüssel.  

Die folgenden Berechtigungen können pro Prinzipal im Zugriffssteuerungseintrag für Geheimnisse in einem Tresor verwendet werden und spiegeln die für ein Geheimnisobjekt zulässigen Vorgänge präzise wider:  

-   *set*: Erstellen neuer Geheimnisse  
-   *get*: Lesen eines Geheimnis  
-   *list*: Auflisten der Geheimnisse oder Versionen eines in einem Schlüsseltresor gespeicherten Geheimnisses  
-   *delete*: Löschen des Geheimnisses  
-   *all*: Alle Berechtigungen  

Weitere Informationen zum Arbeiten mit geheimen Schlüsseln finden Sie unter [Vorgänge mit geheimen Schlüsseln in der Referenz zur REST-API für Azure Key Vault](/rest/api/keyvault).  

###  <a name="BKMK_SecretTags"></a> Geheimnistags  
Sie können zusätzliche anwendungsspezifische Metadaten in Form von Tags angeben. Azure Key Vault unterstützt bis zu 15 Tags, von denen jeder einen 256 Zeichen langen Namen und einen Wert von 256 Zeichen aufweisen kann.  

>[!Note]
>Tags sind für Aufrufer lesbar, die über die *list*- oder *get*-Berechtigung für diesen Objekttyp (Schlüssel, Geheimnis oder Zertifikat) verfügen.

## <a name="key-vault-certificates"></a>Key Vault-Zertifikate

Die Unterstützung von Key Vault-Zertifikaten ermöglicht die Verwaltung Ihrer x509-Zertifikate sowie folgende Szenarien:  

-   Ermöglicht einem Zertifikatbesitzer die Erstellung eines Zertifikats über einen Key Vault-Erstellungsvorgang oder durch den Import eines vorhandenen Zertifikats. Dies schließt sowohl selbstsignierte als auch von einer Zertifizierungsstelle generierte Zertifikate ein.
-   Ermöglicht dem Besitzer eines Key Vault-Zertifikats die Implementierung von sicherem Speicher sowie die Verwaltung von X509-Zertifikaten ohne Interaktion mit privaten Schlüsseln.  
-   Ermöglicht einem Zertifikatbesitzer die Erstellung einer Richtlinie, die Key Vault anweist, den Lebenszyklus eines Zertifikats zu verwalten.  
-   Ermöglicht Zertifikatbesitzern die Angabe von Kontaktinformationen zur Benachrichtigung über Lebenszyklusereignisse wie Ablauf und Verlängerung eines Zertifikats.  
-   Unterstützt die automatische Verlängerung mit dem ausgewählten Aussteller – Key Vault-Partneranbieter oder -Partnerzertifizierungsstellen für X509-Zertifikate.

>[!Note]
>Anbieter/Zertifizierungsstellen, mit denen keine Partnerschaft besteht, sind ebenfalls zulässig. Das Feature für die automatische Verlängerung wird von ihnen jedoch nicht unterstützt.

###  <a name="BKMK_CompositionOfCertificate"></a> Zusammensetzung eines Zertifikats

Wenn ein Key Vault-Zertifikat erstellt wird, werden auch ein adressierbarer Schlüssel und ein Geheimnis gleichen Namens erstellt. Der Key Vault-Schlüssel ermöglicht Schlüsselvorgänge, und das Key Vault-Geheimnis ermöglicht den Abruf des Zertifikatwerts als Geheimnis. Ein Key Vault-Zertifikat enthält auch öffentliche X509-Zertifikatmetadaten.  

Bezeichner und Version der Zertifikate entsprechen denen von Schlüsseln und Geheimnissen. Eine bestimmte Version eines mit der Key Vault-Zertifikatversion erstellten adressierbaren Schlüssels und Geheimnisses ist in der Antwort des Key Vault-Zertifikats verfügbar.
 
![Zertifikate sind komplexe Objekte](media/azure-key-vault.png)

###  <a name="BKMK_CertificateExportableOrNonExportableKey"></a> Exportierbarer oder nicht exportierbarer Schlüssel

Wenn ein Key Vault-Zertifikat erstellt wird, kann es von dem adressierbaren Geheimnis mit dem privaten Schlüssel entweder im PFX- oder PEM-Format abgerufen werden, wenn die zum Erstellen des Zertifikats verwendete Richtlinie angibt, dass der Schlüssel exportierbar ist. Wenn die zum Erstellen des Key Vault-Zertifikats verwendete Richtlinie angibt, dass der Schlüssel nicht exportierbar ist, dann ist der private Schlüssel beim Abruf als Geheimnis nicht Teil des Werts.  

Der adressierbare Schlüssel erhält bei nicht exportierbaren Key Vault-Zertifikaten höhere Relevanz. Die Vorgänge des adressierbaren Key Vault-Schlüssels werden vom Feld *keyusage* der Key Vault-Zertifikatrichtlinie zugeordnet, mit der das Key Vault-Zertifikat erstellt wird.  

Zwei Typen des Schlüssels werden unterstützt – *RSA* oder *RSA-HSM* mit Zertifikaten. Die Exportierbarkeit ist nur mit RSA zulässig, sie wird nicht von RSA-HSM unterstützt.  

###  <a name="BKMK_CertificateAttributesAndTags"></a> Zertifikatattribute und Tags

Außer den Zertifikatmetadaten, einem adressierbaren Schlüssel und einem adressierbaren Geheimnis enthält ein Key Vault-Zertifikat auch Attribute und Tags.  

#### <a name="attributes"></a>Attribute

Die Zertifikatattribute werden in Attributen des adressierbaren Schlüssels und Geheimnisses gespiegelt, wenn ein Key Vault-Zertifikat erstellt wird.  

Ein Key Vault-Zertifikat weist folgende Attribute auf:  

-   *enabled*: Boolesch, optional, Standardwert ist **true**. Mithilfe dieses Attributs kann angegeben werden, ob die Zertifikatdaten als Geheimnis abgerufen werden können oder als Schlüssel funktionsfähig sind. Es wird in Verbindung mit *nbf* und *exp* verwendet. Wenn ein Vorgang zwischen *nbf* und *exp* stattfindet, wird er nur zugelassen, wenn „enabled“ auf „true“ festgelegt ist. Vorgänge außerhalb des Fensters zwischen *nbf* und *exp* werden automatisch nicht zugelassen.  

Es gibt zusätzliche schreibgeschützte Attribute, die in die Antwort einbezogen werden:

-   *created*: IntDate: Gibt an, wann diese Version des Zertifikats erstellt wurde.  
-   *updated*: IntDate: Gibt an, wann diese Version des Zertifikats aktualisiert wurde.  
-   *exp*: IntDate: Enthält den Wert des Ablaufdatums des X509-Zertifikats.  
-   *nbf*: IntDate: Enthält den Wert des Datums des X509-Zertifikats.  

> [!Note] 
> Wenn ein Key Vault-Zertifikat abläuft, sind sein adressierbarer Schlüssel und sein Geheimnis nicht mehr funktionsfähig.  

#### <a name="tags"></a>Tags

 Vom Client angegebenes Wörterbuch von Schlüssel-Wert-Paaren, die Tags in Schlüsseln und Geheimnissen ähneln.  

 > [!Note]
> Tags sind für Aufrufer lesbar, die über die *list*- oder *get*-Berechtigung für diesen Objekttyp (Schlüssel, Geheimnis oder Zertifikat) verfügen.

###  <a name="BKMK_CertificatePolicy"></a> Zertifikatrichtlinie

Eine Zertifikatrichtlinie enthält Informationen zum Erstellen und Verwalten des Lebenszyklus eines Key Vault-Zertifikats. Wenn ein Zertifikat mit privatem Schlüssel in den Schlüsseltresor importiert wird, wird durch Lesen des X509-Zertifikats eine Standardrichtlinie erstellt.  

Wenn ein Key Vault-Zertifikat von Grund auf neu erstellt wird, muss für Key Vault eine Richtlinie bereitgestellt werden, die darüber informiert, wie diese oder die nächste Key Vault-Zertifikatsversion erstellt werden soll. Nachdem eine Richtlinie eingerichtet wurde, ist es nicht erforderlich, mit aufeinander folgenden Erstellungsvorgängen die nächsten Key Vault-Zertifikatsversionen zu erstellen.  

Es gibt nur eine Instanz einer Richtlinie für alle Versionen eines Key Vault-Zertifikats.  

Auf hoher Ebene enthält eine Zertifikatsrichtlinie Folgendes:  

-   X509-Zertifikateigenschaften: enthält Antragstellernamen, alternative Antragstellernamen usw., die zum Erstellen einer X509-Zertifikatanforderung verwendet werden.  
-   Schlüsseleigenschaften: enthalten den Typ des Schlüssels, Schlüssellänge, Exportierbarkeits- und Wiederverwendungs-Schlüsselfelder. Diese Felder teilen Key Vault mit, wie ein Schlüssel generiert werden soll.  
-   Geheimniseigenschaften: enthält Geheimniseigenschaften wie den Inhaltstyp eines adressierbaren Geheimnisses zum Generieren des Geheimniswerts, um das Zertifikat als Geheimnis abzurufen.  
-   Lebensdaueraktionen: enthält Lebensdaueraktionen für das Key Vault-Zertifikat. Jede Lebensdaueraktion enthält:  

     - Trigger: wird über Tage vor Ablauf oder Lebensdauerprozentsatz angegeben  

     - Aktion: Angabe des Aktionstyps – *emailContacts* oder *autoRenew*  

-   Aussteller: Parameter über den zum Ausstellen von X509-Zertifikaten zu verwendenden Aussteller des Zertifikats.  
-   Richtlinienattribute: enthält Attribute, die der Richtlinie zugeordnet sind  

#### <a name="x509-to-key-vault-usage-mapping"></a>Zuordnung von X509 zur Key Vault-Verwendung

In der folgenden Tabelle wird eine X509-Schlüsselverwendungsrichtlinie effektiven Schlüsselvorgängen eines Schlüssels zugeordnet, der im Rahmen einer Key Vault-Zertifikatserstellung erstellt wurde.

|**X509-Flags für Schlüsselverwendung**|**Key Vault-Schlüsseloptionen**|**Standardverhalten**|
|----------|--------|--------|
|DataEncipherment|encrypt, decrypt| N/V |
|DecipherOnly|Entschlüsseln| N/V  |
|DigitalSignature|sign, verify| Key Vault-Standard ohne Verwendungsspezifikation zum Zeitpunkt der Zertifikatserstellung | 
|EncipherOnly|Verschlüsseln| N/V |
|KeyCertSign|sign, verify|N/V|
|KeyEncipherment|wrapKey, unwrapKey| Key Vault-Standard ohne Verwendungsspezifikation zum Zeitpunkt der Zertifikatserstellung | 
|NonRepudiation|sign, verify| N/V |
|crlsign|sign, verify| N/V |

###  <a name="BKMK_CertificateIssuer"></a> Zertifikataussteller

Ein Key Vault-Zertifikatsobjekt enthält eine Konfiguration zur Kommunikation mit einem ausgewählten Zertifikatsausstelleranbieter über das Bestellen von X509-Zertifikaten.  

-   Key Vault-Partner mit den folgenden Zertifikatsausstelleranbietern für SSL-Zertifikate

|**Anbietername**|**Standorte**|
|----------|--------|
|DigiCert|Wird an allen Key Vault-Dienststandorten in öffentlicher Cloud und Azure Government unterstützt|
|GlobalSign|Wird an allen Key Vault-Dienststandorten in öffentlicher Cloud und Azure Government unterstützt|

Bevor ein Zertifikatsaussteller in einer Key Vault-Instanz erstellt werden kann, müssen die folgenden erforderlichen Schritte 1 und 2 erfolgreich durchgeführt werden.  

1. Integrieren in Anbieter von Zertifizierungsstellen (CA)  

    -   Ein Organisationsadministrator muss sein Unternehmen (z.B. Contoso) in mindestens einen CA-Anbieter integrieren.  

2. Der Administrator erstellt die Anmeldeinformationen der anfordernden Person, damit Key Vault SSL-Zertifikate registrieren (und erneuern) kann.  

    -   Stellt die Konfiguration zum Erstellen eines Ausstellerobjekts des Anbieters im Schlüsseltresor bereit  

Weitere Informationen zum Erstellen von Ausstellerobjekten im Zertifikateportal finden Sie im [Blog zu Key Vault-Zertifikaten](http://aka.ms/kvcertsblog).  

Key Vault ermöglicht die Erstellung mehrerer Ausstellerobjekte mit unterschiedlicher Ausstelleranbieterkonfiguration. Sobald ein Ausstellerobjekt erstellt ist, kann in einer oder mehreren Zertifikatrichtlinien auf seinen Namen verwiesen werden. Beim Verweis auf das Ausstellerobjekt wird Key Vault angewiesen, die Konfiguration gemäß den Angaben im Ausstellerobjekt zu verwenden, wenn das X509-Zertifikat bei Zertifikaterstellung und -verlängerung beim CA-Anbieter angefordert wird.  

Ausstellerobjekte werden im Tresor erstellt und können nur in demselben Tresor mit Key Vault-Zertifikaten verwendet werden.  

###  <a name="BKMK_CertificateContacts"></a> Zertifikatkontakte

Zertifikatkontakte enthalten Kontaktinformationen zum Senden von Benachrichtigungen, die durch Zertifikatlebensdauer-Ereignisse ausgelöst werden. Die Kontaktinformationen werden von allen Zertifikaten im Schlüsseltresor gemeinsam genutzt. Eine Benachrichtigung zu einem Ereignis eines beliebigen Zertifikats im Schlüsseltresor wird an alle angegebenen Kontakte gesendet.  

Wenn für die Richtlinie eines Zertifikats eine automatische Verlängerung festgelegt ist, wird eine Benachrichtigung bei den folgenden Ereignissen gesendet.  

-   Vor der Zertifikatverlängerung
-   Nach der Zertifikatverlängerung, um mitzuteilen, ob das Zertifikat erfolgreich verlängert wurde, oder ob ein Fehler aufgetreten ist, sodass die manuelle Zertifikatverlängerung erforderlich ist.  

 Wenn in der Richtlinie für ein Zertifikat die manuelle Verlängerung festgelegt ist (nur E-Mail), wird eine Benachrichtigung gesendet, sobald das Zertifikat verlängert werden muss.  

###  <a name="BKMK_CertificateAccessControl"></a> Zertifikatzugriffssteuerung

 Die Zugriffssteuerung für Zertifikate, die von Key Vault verwaltet werden, ist auf der Ebene einer Key Vault-Instanz möglich, die als Container dieser Zertifikate fungiert. Es gibt eine Zugriffssteuerungsrichtlinie für Zertifikate, die sich von der Zugriffssteuerungsrichtlinie für Schlüssel und Geheimnisse in derselben Key Vault-Instanz unterscheidet. Benutzer können einen oder mehrere Tresore für die Aufnahme von Zertifikaten erstellen und müssen eine dem Szenario entsprechende Segmentierung und Verwaltung von Zertifikaten aufrechterhalten.  

 Die folgenden Berechtigungen können pro Prinzipal im Zugriffssteuerungseintrag für Geheimnisse in einem Schlüsseltresor verwendet werden und spiegeln die für ein Geheimnisobjekt zulässigen Vorgänge präzise wider:  

-   *get*: ermöglicht das Abrufen der aktuellen oder einer beliebigen Version eines Zertifikats. 
-   *list*: ermöglicht das Auflisten der aktuellen Zertifikate oder von Versionen eines Zertifikats.  
-   *delete*: ermöglicht das Löschen eines Zertifikats, seiner Richtlinie und aller seiner Versionen.  
-   *create*: ermöglicht das Erstellen eines Key Vault-Zertifikats.  
-   *import*: ermöglicht das Importieren von Zertifikatmaterial in ein Key Vault-Zertifikat.  
-   *update*: ermöglicht das Aktualisieren eines Zertifikats.  
-   *managecontacts*: ermöglicht die Verwaltung der Key Vault-Zertifikatkontakte.  
-   *getissuers*: ermöglicht das Abrufen der Aussteller eines Zertifikats.  
-   *listissuers*: ermöglicht das Auflisten der Aussteller eines Zertifikats.  
-   *setissuers*: ermöglicht das Erstellen oder Aktualisieren des Key Vault-Zertifikatausstellers.  
-   *deleteissuers*: ermöglicht das Löschen von Key Vault-Zertifikatausstellern.  
-   *all*: gewährt alle Berechtigungen.  

Weitere Informationen finden Sie unter [Zertifikatvorgänge in der Referenz zur REST-API für Azure Key Vault](/rest/api/keyvault). 

## <a name="see-also"></a>Siehe auch

- [Authentifizierung, Anforderungen und Antworten](authentication-requests-and-responses.md)
- [Key Vault-Versionen](key-vault-versions.md)
- [Entwicklerhandbuch für Key Vault](/azure/key-vault/key-vault-developers-guide)
