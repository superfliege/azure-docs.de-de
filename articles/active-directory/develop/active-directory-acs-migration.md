---
title: Migrieren von Azure Access Control Service (ACS)
description: "Optionen für das Verschieben von Apps und Diensten aus Azure Access Control Service | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
ms.openlocfilehash: e32cac7feda929a63c4a80fc0078b221117eb2b5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2017
---
# <a name="migrating-from-azure-access-control-service-acs"></a>Migrieren von Azure Access Control Service (ACS)

Microsoft Azure Active Directory Access Control (auch als Access Control Service oder ACS bezeichnet) wird im November 2018 eingestellt.  Anwendungen und Dienste, die derzeit ACS nutzen, müssen vor diesem Datum vollständig zu einem anderen Authentifizierungsmechanismus migriert werden. In diesem Dokument werden Empfehlungen für aktuelle Kunden beschrieben, die ACS nicht mehr nutzen möchten. Wenn Sie ACS derzeit nicht verwenden, müssen Sie keine Maßnahmen ergreifen.


## <a name="brief-acs-overview"></a>ACS-Kurzübersicht

ACS ist ein Cloudauthentifizierungsdienst, der eine einfache Methode zur Authentifizierung und Autorisierung von Benutzern für den Zugang zu Ihren Webanwendungen und -diensten bietet. Gleichzeitig werden zahlreiche Funktionen zur Authentifizierung und Autorisierung aus Ihrem Code ausgeklammert. ACS wird hauptsächlich von Entwicklern und Architekten von .NET-Clients, ASP.NET-Webanwendungen und WCF-Webdiensten eingesetzt.

Die Anwendungsfälle für ACS können in die drei folgenden Hauptkategorien unterteilt werden:

- Authentifizierung bei bestimmten Microsoft-Clouddiensten, einschließlich Azure Service Bus, Dynamics CRM usw. Clientanwendungen können Token von ACS abrufen, die dann zum Authentifizieren bei diesen Diensten zum Ausführen verschiedener Aktionen verwendet werden können.
- Hinzufügen von Authentifizierung zu Webanwendungen (eigene und vorkonfigurierte Authentifizierung, etwa Sharepoint). Mithilfe der passiven Authentifizierung von ACS konnten Webanwendungen die Anmeldung bei Google-, Facebook-, Yahoo-, Azure Active Directory- und ADFS-Konten sowie beim Microsoft-Konto (Live ID) unterstützen.
- Sichern benutzerdefinierter Webdienste mit von ACS ausgestellten Token. Mithilfe der aktiven Authentifizierung konnten Webdienste sicherstellen, dass nur der Zugriff von bekannten Clients zugelassen wurde, die sich über ACS authentifiziert haben.

Die einzelnen Anwendungsfälle und ihre empfohlenen Migrationsstrategien werden in den folgenden Abschnitten erläutert. In den meisten Fällen sind erhebliche Codeänderungen erforderlich, um vorhandene Apps und Dienste zu neueren Technologien zu migrieren. Es wird empfohlen, umgehend mit der Planung und Ausführung der Migration zu beginnen, um mögliche Ausfälle und Ausfallzeiten zu vermeiden.

> [!WARNING]
> In den meisten Fällen sind erhebliche Codeänderungen erforderlich, um vorhandene Apps und Dienste zu neueren Technologien zu migrieren. Es wird empfohlen, umgehend mit der Planung und Ausführung der Migration zu beginnen, um mögliche Ausfälle und Ausfallzeiten zu vermeiden.

Aus architektonischer Sicht setzt sich ACS aus den folgenden Komponenten zusammen:

- Sicherheitstokendienst (Secure Token Service, STS), der Authentifizierungsanforderungen empfängt und daraufhin Sicherheitstoken ausstellt
- Klassisches Azure-Portal, über das ACS-Namespacese erstellt, gelöscht und aktiviert/deaktiviert werden
- Separates ACS-Verwaltungsportal, das zum Anpassen und Konfigurieren des Verhaltens eines ACS-Namespace verwendet wird
- Verwaltungsdienst, der zum Automatisieren der Funktionen des Portals verwendet werden kann
- Tokentransformationsregel-Modul, mit dem komplexe Logik zum Verarbeiten des Ausgabeformats der von ACS ausgestellten Token festgelegt werden kann

Um diese Komponenten verwenden zu können, müssen Sie mindestens einen ACS-**Namespace** erstellen. Ein Namespace ist eine dedizierte Instanz von ACS, mit der Ihre Anwendungen und Dienste kommunizieren. Er ist von allen anderen ACS-Kunden isoliert. Diese besitzen eigene Namespaces. Ein Namespace in ACS verfügt über eine dedizierte URL, etwa:

```HTTP
https://mynamespace.accesscontrol.windows.net
```

Die gesamte Kommunikation mit dem STS und die Verwaltungsvorgänge erfolgen unter dieser URL. Dabei werden verschiedene Pfade für unterschiedliche Zwecke verwendet. Um zu ermitteln, ob Ihre Anwendungen oder Dienste ACS verwenden, überprüfen Sie, ob Datenverkehr an `https://{namespace}.accesscontrol.windows.net` gesendet wird.  Der gesamte Datenverkehr an diese URL wird von ACS verarbeitet und muss eingestellt werden.  Die einzige Ausnahme ist Datenverkehr an `https://accounts.accesscontrol.windows.net`: An diese URL gesendeter Datenverkehr wird bereits von einem anderen Dienst verarbeitet und ist von der Deaktivierung von ACS nicht betroffen.  Sie sollten sich außerdem beim klassischen Azure-Portal anmelden und nach ACS-Namespaces in Ihren Abonnements suchen.  ACS-Namespaces werden im **Active Directory**-Dienst auf der Registerkarte **Access Control-Namespaces** aufgeführt.

Weitere Informationen zu ACS finden Sie in [dieser archivierten Dokumentation auf MSDN](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>Deaktivierungszeitplan

Im November 2017 werden alle ACS-Komponenten vollständig unterstützt und sind voll betriebsfähig. Die einzige Einschränkung besteht darin, dass [keine neuen ACS-Namespaces über das klassische Azure-Portal erstellt werden können](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Hier sehen Sie den Zeitplan für die Deaktivierung dieser Komponenten:

- **November 2017**: Die Azure AD-Administratoroberfläche im klassischen Azure-Portal wird [deaktiviert](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). Zu diesem Zeitpunkt können Namespaces für ACS unter der folgenden neuen dedizierten URL verwaltet werden: `http://manage.windowsazure.com?restoreClassic=true`. Dadurch soll das Anzeigen, Aktivieren/Deaktivieren und das vollständige Löschen vorhandener Namespaces ermöglicht werden.
- **April 2018**: ACS-Namespaces können nicht mehr unter dieser dedizierten URL verwaltet werden. Sie können Ihre ACS-Namespaces dann nicht mehr deaktivieren/aktivieren, löschen oder auflisten. Das ACS-Verwaltungsportal ist jedoch voll funktionsfähig und unter `https://{namespace}.accesscontrol.windows.net` verfügbar. Alle anderen Komponenten von ACS werden ebenfalls weiterhin normal ausgeführt.
- **November 2018**: Alle ACS-Komponenten werden dauerhaft heruntergefahren. Dazu zählen das ACS-Verwaltungsportal, der Verwaltungsdienst, STS und das Tokentransformationsregel-Modul. Bei an ACS (unter `{namespace}.accesscontrol.windows.net`) gesendete Anforderungen tritt ein Fehler auf. Alle vorhandenen Apps und Dienste müssen vor diesem Zeitpunkt zu anderen Technologien migriert worden sein.


## <a name="migration-strategies"></a>Migrationsstrategien

Die folgenden Abschnitte beschreiben allgemeine Empfehlungen für die Migration von ACS zu anderen Microsoft-Technologien.

### <a name="clients-of-microsoft-cloud-services"></a>Clients von Microsoft-Clouddiensten

Alle Microsoft-Clouddienste, die von ACS ausgestellte Token akzeptieren, unterstützen jetzt mindestens eine alternative Authentifizierungsmethode. Der richtige Authentifizierungsmechanismus variiert je nach Dienst. Daher wird empfohlen, die offiziellen Anleitungen in der speziellen Dokumentation der jeweiligen Dienste zu lesen. Der Einfachheit halber sind die Dokumentationen hier aufgeführt:

| Dienst | Anleitungen |
| ------- | -------- |
| Azure-Servicebus | [Migrieren vom Azure Active Directory-Access Control Service zur SAS-Autorisierung](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Relay | [Migrieren vom Azure Active Directory-Access Control Service zur SAS-Autorisierung](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Cache | [Migration zu Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure Data Market | [DataMarket ist jetzt veraltet](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [DataMarket ist jetzt veraltet](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Azure Media Services announces support for AAD and deprecation of ACS authentication](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) (Azure Media Services verkündet Unterstützung für AAD sowie Einstellung der ACS-Authentifizierung) |
| Azure Backup | [Fragen zum Azure Backup-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-using-passive-authentication"></a>Anwendung mit passiver Authentifizierung

Für Webanwendungen, die ACS für die Benutzerauthentifizierung nutzen, hat ACS die folgenden Features und Funktionen für Webanwendungsentwickler und -architekten bereitgestellt:

- Umfassende Integration mit Windows Identity Foundation (WIF)
- Verbund mit Google, Facebook, Yahoo, Microsoft-Konto (Live ID), Azure Active Directory und ADFS
- Unterstützung für die folgenden Authentifizierungsprotokolle: OAuth 2.0 (Entwurf 13), WS-Trust und WS-Verbund
- Unterstützung für die folgenden Tokenformate: JSON-Webtoken (JWT), SAML 1.1, SAML 2.0 und Simple Web Token (SWT)
- In WIF integrierte Startbereichsermittlung, die Benutzern das Auswählen des Kontotyps für die Anmeldung ermöglichte. Diese Funktion wurde von der Webanwendung gehostet und war vollständig anpassbar.
- Tokentransformation, die die umfassende Anpassung der Ansprüche ermöglichte, die von der Webanwendung von ACS empfangen wurden, einschließlich:
    - Übergeben von Ansprüchen von Identitätsanbietern
    - Hinzufügen zusätzlicher benutzerdefinierter Ansprüche
    - Einfache If-Then-Logik zum Ausstellen von Ansprüchen unter bestimmten Bedingungen

Leider gibt es keinen Dienst, der alle diese Funktionen bereitstellt.  Sie sollten entscheiden, welche Funktionen von ACS Sie benötigen, und dann zwischen [**Azure Active Directory**](https://azure.microsoft.com/develop/identity/signin/) (Azure AD), [**Azure AD B2C**](https://azure.microsoft.com/services/active-directory-b2c/) und anderen Cloudauthentifizierungsdiensten wählen.

#### <a name="migrating-to-azure-active-directory"></a>Migrieren zu Azure Active Directory

Sie können Ihre Apps und Dienste direkt in Azure Active Directory integrieren. Azure AD ist der cloudbasierte Identitätsanbieter für Geschäfts-, Schul- oder Unikonten von Microsoft – der Identitätsanbieter für Office 365, Azure und vieles mehr. Es bietet ähnliche Verbundauthentifizierungsfunktionen wie ACS, unterstützt jedoch nicht alle ACS-Features. Ein gutes Beispiel ist der Verbund mit Identitätsanbietern aus sozialen Netzwerken wie Facebook, Google und Yahoo. Wenn Ihre Benutzer sich mit dieser Art von Anmeldeinformationen anmelden, ist Azure AD nicht für Sie geeignet. Darüber hinaus werden nicht unbedingt die gleichen Authentifizierungsprotokolle wie von ACS unterstützt: Beispielsweise unterstützen ACS und Azure AD zwar beide OAuth, es gibt jedoch feine Unterschiede zwischen den einzelnen Implementierungen, die im Rahmen der Migration eine Anpassung des Codes erfordern.

Azure AD bietet jedoch mehrere potenzielle Vorteile für Kunden von ACS. Der Dienst unterstützt nativ in der Cloud gehostete Geschäfts-, Schul- oder Unikonten von Microsoft, die häufig von ACS-Kunden verwendet werden.  Ein Azure AD-Mandant kann über ADFS ebenfalls einen Verbund mit mehreren Instanzen der lokalen Active Directory-Instanz bilden und Ihrer App damit die Authentifizierung von cloudbasierten und lokal gehosteten Benutzern ermöglichen.  Darüber hinaus unterstützt Azure AD das WS-Verbundprotokoll, wodurch sich die Integration in eine Webanwendung mit Windows Identity Foundation (WIF) relativ unkompliziert gestaltet.

Die folgende Tabelle vergleicht die für Webanwendungen relevanten Funktionen von ACS mit den in Azure AD verfügbaren Features. Im Allgemeinen ist **Azure Active Directory wahrscheinlich die richtige Wahl für Ihre Migration, wenn Sie für Ihre Benutzer nur die Anmeldung mit ihren Geschäfts-, Schul- oder Unikonten von Microsoft zulassen**.

| Funktion | ACS-Unterstützung | Azure AD-Unterstützung |
| ---------- | ----------- | ---------------- |
| **Kontotyp** | | |
| Geschäfts-, Schul- oder Unikonto von Microsoft | Unterstützt | Unterstützt |
| Konten von Windows Server Active Directory und ADFS | Unterstützt über den Verbund mit einem Azure AD-Mandanten <br /> Unterstützt über den direkten Verbund mit ADFS | Nur unterstützt über den Verbund mit einem Azure AD-Mandanten | 
| Konten aus anderen Identitätsverwaltungssystemen für Unternehmen | Möglich über den Verbund mit einem Azure AD-Mandanten <br />Unterstützt über den direkten Verbund | Möglich über den Verbund mit einem Azure AD-Mandanten |
| Microsoft-Konten für den persönlichen Gebrauch (Windows Live ID) | Unterstützt | Unterstützt über das v2.0 OAuth-Protokoll von Azure AD, aber nicht über andere Protokolle | 
| Facebook-, Google-, Yahoo-Konten | Unterstützt | Überhaupt nicht unterstützt |
| **Protokolle und SDK-Kompatibilität** | | |
| Windows Identity Foundation (WIF) | Unterstützt | Unterstützt, jedoch nur begrenzte Anweisungen verfügbar |
| WS-Verbund | Unterstützt | Unterstützt |
| OAuth 2.0 | Unterstützung für Entwurf 13 | Unterstützung für RFC 6749, die modernste Spezifikation |
| WS-Trust | Unterstützt | Nicht unterstützt |
| **Tokenformate** | | |
| JSON-Webtoken (JWTs) | Unterstützt in der Betaversion | Unterstützt |
| SAML 1.1-Token | Unterstützt | Unterstützt |
| SAML 2.0-Token | Unterstützt | Unterstützt |
| Einfache Webtoken (Simple Web Tokens, SWTs) | Unterstützt | Nicht unterstützt |
| **Anpassungen** | | |
| Anpassbare Benutzeroberfläche für Startbereichsermittlung/Kontoauswahl | Code zum Herunterladen, der in Apps integriert werden kann | Nicht unterstützt |
| Hochladen von benutzerdefinierten Tokensignaturzertifikaten | Unterstützt | Unterstützt |
| Anpassen von Ansprüchen in Token | Übergeben von Eingabeansprüchen von Identitätsanbietern<br />Abrufen des Zugriffstokens von Identitätsanbietern als Anspruch<br />Ausstellen von Ausgabeansprüchen basierend auf den Werten der Eingabeansprüche<br />Ausstellen von Ausgabeansprüchen mit konstanten Werten | Übergeben von Ansprüchen von Verbundidentitätsanbietern nicht möglich<br />Abrufen des Zugriffstokens von Identitätsanbietern als Anspruch nicht möglich<br />Ausstellen von Ausgabeansprüchen basierend auf den Werten der Eingabeansprüche nicht möglich<br />Ausstellen von Ausgabeansprüchen mit konstanten Werten<br />Ausstellen von Ausgabeansprüchen basierend auf den Eigenschaften von mit Azure AD synchronisierten Benutzern |
| **Automation** | | |
| Automatisieren von Konfigurations-/Verwaltungsaufgaben | Unterstützt über den ACS-Verwaltungsdienst | Unterstützt über Microsoft Graph- und Azure AD Graph-API |

Wenn Sie der Meinung sind, dass Azure AD die richtige Lösung für Ihre Anwendungen und Dienste ist, beachten Sie die zwei Möglichkeiten für die Integration Ihrer App in Azure AD.

Wenn Sie WS-Verbund/WIF für die Integration in Azure AD verwenden möchten, wird der [in diesem Artikel beschriebene Ansatz](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery) empfohlen. Der Artikel bezieht sich auf die Konfiguration von Azure AD für SAML-basiertes einmaliges Anmelden, gilt aber auch für das Konfigurieren des WS-Verbunds. Für diesen Ansatz ist zwar eine Azure AD Premium-Lizenz erforderlich, er hat aber zwei Vorteile:

- Sie profitieren von der vollständigen Flexibilität der Azure AD-Tokenanpassung. Dadurch können Sie die von Azure AD ausgestellten Ansprüche an die von ACS ausgestellten Ansprüche anpassen, insbesondere die Ansprüche für Benutzer-ID oder Namensbezeichner. Sie müssen sicherstellen, dass die von Azure AD ausgestellten Benutzer-IDs mit den von ACS ausgestellten Benutzer-IDs übereinstimmen, sodass Sie auch nach der Umstellung der Technologie weiterhin konsistente Benutzer-IDs für Ihre Benutzer erhalten.
- Sie können ein für Ihre Anwendung spezifisches Tokensignaturzertifikat konfigurieren und seine Gültigkeitsdauer steuern.

<!--

Possible nameIdentifiers from ACS (via AAD or ADFS):
- ADFS - Whatever ADFS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Für diesen Ansatz ist eine Azure AD Premium-Lizenz erforderlich. Wenn Sie ACS-Kunde sind und eine Premium-Lizenz zum Einrichten des einmaligen Anmeldens für eine Anwendung benötigen, wenden Sie sich an uns, und wir stellen Ihnen gerne Entwicklerlizenzen zur Verfügung.

Eine andere Möglichkeit ist die Verwendung [dieses Codebeispiels](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), das etwas andere Anweisungen zum Einrichten des WS-Verbunds enthält. Dieses Codebeispiel verwendet nicht WIF, sondern stattdessen ASP.NET 4.5-OWIN-Middleware. Die Anweisungen für die App-Registrierung gelten jedoch auch für Apps, die WIF nutzen, und es ist keine Azure AD Premium-Lizenz erforderlich.  Wenn Sie sich für diesen Ansatz entscheiden, müssen Sie mit dem [Rollover von Signaturschlüsseln in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover) vertraut sein. Dieser Ansatz verwendet den globalen Azure AD-Signaturschlüssel zum Ausstellen von Token. Standardmäßig aktualisiert WIF Signaturschlüssel nicht automatisch. Wenn Azure AD durch die globalen Signaturschlüssel wechselt, muss Ihre WIF-Implementierung entsprechend vorbereitet sein und die Änderungen akzeptieren.

Wenn die Integration in Azure AD über das OpenID Connect- oder das OAuth-Protokoll möglich ist, wird diese Vorgehensweise empfohlen.  Unter [Azure Active Directory für Entwickler](http://aka.ms/aaddev) stehen ausführliche Dokumentationen und Anweisungen für die Integration von Azure AD in Ihre Webanwendung zur Verfügung.

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrating-to-azure-ad-b2c"></a>Migrieren zu Azure AD B2C

Sie können als Migrationspfad auch Azure AD B2C in Erwägung ziehen.  B2C ist ein Cloudauthentifizierungsdienst, der – ähnlich wie ACS – Benutzern die Auslagerung der Authentifizierungs- und Identitätsverwaltungslogik in einen Clouddienst ermöglicht.  Es handelt sich dabei um einen kostenpflichtigen Dienst (mit Free- und Premium-Tarif) für auf Endbenutzer ausgerichtete Anwendungen, die Millionen aktiver Benutzer haben können.

Wie bei ACS besteht eines der attraktivsten Features von B2C darin, dass viele verschiedene Kontotypen unterstützt werden. Mit B2C können sich Benutzer u.a. mit ihrem Facebook-, Google-, Microsoft-, LinkedIn-, GitHub- oder Yahoo-Konto anmelden. B2C unterstützt darüber hinaus „lokale Konten“ bzw. Benutzername und Kennwort, die Benutzer speziell für Ihre Anwendung erstellen. Außerdem bietet Azure AD B2C umfassende Erweiterbarkeit, die das Anpassen von Anmeldeabläufen ermöglicht. Es werden jedoch nicht so viele Authentifizierungsprotokolle und Tokenformate unterstützt, wie sie ACS-Kunden unter Umständen benötigen. Der Dienst kann zudem nicht zum Abrufen von Token oder Abfragen zusätzlicher Informationen zum Benutzer vom Identitätsanbieter, von Microsoft oder anderen Anbietern verwendet werden.

Die folgende Tabelle vergleicht die für Webanwendungen relevanten Funktionen von ACS mit den in Azure AD B2C verfügbaren Features. Im Allgemeinen ist **Azure AD B2C wahrscheinlich die richtige Wahl für Ihre Migration, wenn Ihre Anwendung auf Endbenutzer ausgerichtet ist oder viele verschiedene Kontotypen unterstützt**.

| Funktion | ACS-Unterstützung | Azure AD B2C-Unterstützung |
| ---------- | ----------- | ---------------- |
| **Kontotyp** | | |
| Geschäfts-, Schul- oder Unikonto von Microsoft | Unterstützt | Unterstützt über benutzerdefinierte Richtlinien  |
| Konten von Windows Server Active Directory und ADFS | Unterstützt über den direkten Verbund mit ADFS | Unterstützt über SAML-Verbund mit benutzerdefinierten Richtlinien |
| Konten aus anderen Identitätsverwaltungssystemen für Unternehmen | Unterstützt über den direkten Verbund über den WS-Verbund | Unterstützt über SAML-Verbund mit benutzerdefinierten Richtlinien |
| Microsoft-Konten für den persönlichen Gebrauch (Windows Live ID) | Unterstützt | Unterstützt | 
| Facebook-, Google-, Yahoo-Konten | Unterstützt | Facebook und Google nativ unterstützt, Yahoo über OpenID Connect-Verbund mit benutzerdefinierten Richtlinien unterstützt |
| **Protokolle und SDK-Kompatibilität** | | |
| Windows Identity Foundation (WIF) | Unterstützt | Nicht unterstützt. |
| WS-Verbund | Unterstützt | Nicht unterstützt. |
| OAuth 2.0 | Unterstützung für Entwurf 13 | Unterstützung für RFC 6749, die modernste Spezifikation |
| WS-Trust | Unterstützt | Nicht unterstützt. |
| **Tokenformate** | | |
| JSON-Webtoken (JWTs) | Unterstützt in der Betaversion | Unterstützt |
| SAML 1.1-Token | Unterstützt | Nicht unterstützt |
| SAML 2.0-Token | Unterstützt | Nicht unterstützt |
| Einfache Webtoken (Simple Web Tokens, SWTs) | Unterstützt | Nicht unterstützt |
| **Anpassungen** | | |
| Anpassbare Benutzeroberfläche für Startbereichsermittlung/Kontoauswahl | Code zum Herunterladen, der in Apps integriert werden kann | Vollständig anpassbare Benutzeroberfläche über benutzerdefiniertes CSS |
| Hochladen von benutzerdefinierten Tokensignaturzertifikaten | Unterstützt | Benutzerdefinierte Signaturschlüssel, keine Zertifikate, unterstützt über benutzerdefinierte Richtlinien |
| Anpassen von Ansprüchen in Token | Übergeben von Eingabeansprüchen von Identitätsanbietern<br />Abrufen des Zugriffstokens von Identitätsanbietern als Anspruch<br />Ausstellen von Ausgabeansprüchen basierend auf den Werten der Eingabeansprüche<br />Ausstellen von Ausgabeansprüchen mit konstanten Werten | Übergeben von Ansprüchen von Identitätsanbietern. Für einige Ansprüche sind benutzerdefinierte Richtlinien erforderlich.<br />Abrufen des Zugriffstokens von Identitätsanbietern als Anspruch nicht möglich<br />Ausstellen von Ausgabeansprüchen basierend auf den Werten der Eingabeansprüche über benutzerdefinierte Richtlinien<br />Ausstellen von Ausgabeansprüchen mit konstanten Werten über benutzerdefinierte Richtlinien |
| **Automation** | | |
| Automatisieren von Konfigurations-/Verwaltungsaufgaben | Unterstützt über den ACS-Verwaltungsdienst | Erstellung von Benutzern zulässig über Azure AD Graph-API. B2C-Mandanten, -Anwendungen oder -Richtlinien können nicht programmgesteuert erstellt werden. |

Wenn Sie der Meinung sind, dass Azure AD B2C die richtige Lösung für Ihre Anwendungen und Dienste ist, lesen Sie zunächst die Informationen in den folgenden Ressourcen:

- [Azure AD B2C: Konzentrieren Sie sich auf Ihre App, wir kümmern uns um Registrierung und Anmeldung](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure Active Directory B2C: Benutzerdefinierte Richtlinien](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure Active Directory B2C – Preise ](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="other-migration-options"></a>Andere Optionen für die Migration

Wenn weder Azure AD noch Azure AD B2C die Anforderungen Ihrer Webanwendung erfüllen, wenden Sie sich an uns. Wir können Sie dabei unterstützen, die für Sie am besten geeignete Lösung zu ermitteln.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD nor Azure AD B2C is sufficient to replace ACS in your web applications without making major code changes.  Some common examples might include:

- web applications using WIF and/or WS-Federation for sign-in with social identity providers such as Google or Facebook.
- web applications performing direct federation to an enterprise IDP over the Ws-Federation protocol.
- web applications that require the access token issued by a social identity provider (such as Google or Facebook) as a claim in the tokens issued by ACS.
- web applications with complex token transformation rules that Azure AD or Azure AD B2C cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

<!--

## Sharepoint 2010, 2013, 2016

TODO: AAD only, use AAD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-using-active-authentication"></a>Webdienste mit aktiver Authentifizierung

Für Webdienste, die mit von ACS ausgestellten Token gesichert wurden, wurden von ACS die folgenden Features und Funktionen bereitgestellt:

- Möglichkeit, **Dienstidentitäten** in Ihrem ACS-Namespace zum Anfordern von Token zu registrieren
- Unterstützung für die Protokolle OAuth WRAP und OAuth 2.0 (Entwurf 13) zum Anfordern von Token mithilfe der folgenden Arten von Anmeldeinformationen:
    - Für die Dienstidentität erstelltes einfaches Kennwort
    - Signiertes SWT, das einen symmetrischen Schlüssel oder ein X509-Zertifikat nutzt
    - Von einem vertrauenswürdigen Anbieter (in der Regel einer ADFS-Instanz) ausgestelltes SAML-Token
- Unterstützung für die folgenden Tokenformate: JSON-Webtoken (JWT), SAML 1.1, SAML 2.0 und Simple Web Token (SWT)
- Einfache Tokentransformationsregeln

Dienstidentitäten in ACS werden in der Regel zum Implementieren von Server-zu-Server-ähnlicher (S2S) Authentifizierung verwendet.  

#### <a name="migrating-to-azure-active-directory"></a>Migrieren zu Azure Active Directory

Für diese Art the Authentifizierung wird die Migration zu [**Azure Active Directory**](https://azure.microsoft.com/develop/identity/signin/) (Azure AD) empfohlen. Azure AD ist der cloudbasierte Identitätsanbieter für Geschäfts-, Schul- oder Unikonten von Microsoft – der Identitätsanbieter für Office 365, Azure und vieles mehr. Der Dienst kann jedoch auch für die S2S-Authentifizierung verwendet werden, die die Azure AD-Implementierung zum Gewähren von OAuth 2.0-Clientanmeldeinformationen nutzt.  Die folgende Tabelle vergleicht die Funktionen von ACS bei der S2S-Authentifizierung mit den in Azure AD verfügbaren Features:

| Funktion | ACS-Unterstützung | Azure AD-Unterstützung |
| ---------- | ----------- | ---------------- |
| Registrieren eines Webdiensts | Erstellen einer vertrauenden Seite im ACS-Verwaltungsportal | Erstellen einer Azure AD-Webanwendung im Azure-Portal |
| Registrieren eines Clients | Erstellen einer Dienstidentität im ACS-Verwaltungsportal | Erstellen einer weiteren Azure AD-Webanwendung im Azure-Portal |
| Verwendetes Protokoll | OAuth WRAP-Protokoll<br />Gewähren von OAuth 2.0 (Entwurf 13)-Clientanmeldeinformationen | Gewähren von OAuth 2.0-Clientanmeldeinformationen |
| Clientauthentifizierungsmethoden | Einfaches Kennwort<br />Signiertes SWT<br />SAML-Token von Verbund-IDP | Einfaches Kennwort<br />Signiertes JWT |
| Tokenformate | JWT<br />SAML 1.1<br />SAML 2.0<br />SWT<br /> | Nur JWT |
| Tokentransformation | Hinzufügen von benutzerdefinierten Ansprüchen<br />Einfache If-Then-Logik für die Anspruchsausstellung | Hinzufügen von benutzerdefinierten Ansprüchen | 
| Automatisieren von Konfigurations-/Verwaltungsaufgaben | Unterstützt über den ACS-Verwaltungsdienst | Unterstützt über Microsoft Graph- und Azure AD Graph-API |

Anleitungen zum Implementieren von Server-zu-Server-Szenarien finden Sie in den folgenden Ressourcen:

- [Azure Active Directory für Entwickler](https://aka.ms/aaddev)
- [Daemon code sample using simple password client credentials](https://github.com/Azure-Samples/active-directory-dotnet-daemon) (Daemon-Codebeispiel mit Clientanmeldeinformationen mit einfachem Kennwort)
- [Daemon code sample using certificate client credentials](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) (Daemon-Codebeispiel mit Clientanmeldeinformationen mit Zertifikat)

#### <a name="other-migration-options"></a>Andere Optionen für die Migration

Wenn Azure AD die Anforderungen für Ihren Webdienst nicht erfüllt, hinterlassen Sie einen Kommentar. Wir unterstützen Sie dabei, die beste Methode für Ihren speziellen Fall zu ermitteln.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD's client credentials OAuth grant implementation is not sufficient to replace ACS in your architecture without major code changes.  Some common examples might include:

- server-to-server authentication using token formats other than JWTs.
- server-to-server authentication using an input token provided by an external identity provider.
- server-to-server authentication with token transformation rules that Azure AD cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

## <a name="questions-concerns--feedback"></a>Fragen, Bedenken und Feedback

Uns ist bewusst, dass viele ACS-Kunden nach dem Lesen dieses Artikels keinen eindeutigen Migrationspfad gefunden haben und unter Umständen Unterstützung oder Beratung beim Ermitteln der richtigen Methode benötigen. Wenn Sie Ihre Migrationsszenarien und Fragen besprechen möchten, hinterlassen Sie einen Kommentar auf dieser Seite.
