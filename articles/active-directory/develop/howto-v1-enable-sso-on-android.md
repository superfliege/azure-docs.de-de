---
title: Aktivieren von App-übergreifendem SSO auf Android mit ADAL | Microsoft Docs
description: 'Verwenden der ADAL SDK-Features zum Aktivieren von SSO über Ihre Anwendungen hinweg. '
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/13/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 4abf6bd2d82753e22d4fde92e219109274ce36be
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39580578"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Aktivieren von App-übergreifendem SSO auf Android mit ADAL
Die Möglichkeit des einmaligen Anmeldens (Single Sign-On, SSO), bei dem Benutzer einmal eingegebene Anmeldeinformationen automatisch auch für andere Anwendungen verwenden können, ist inzwischen Branchenstandard. Die Schwierigkeit, den Benutzernamen und das Kennwort auf einem kleinen Bildschirm einzugeben – häufig in Kombination mit einer weiteren Datenabfrage wie etwa einem Telefonanruf oder einem per SMS gesendeten Code (zweistufige Authentifizierung) – führt beim Benutzer zu Unmut, wenn er sich mehrfach anmelden muss.

Wenn Sie außerdem eine Identitätsplattform einsetzen, die andere Anwendungen verwenden können (beispielsweise Microsoft-Konten oder ein Microsoft365-Geschäftskonto), erwarten Kunden, dass diese Anmeldeinformationen unabhängig vom Herausgeber in all ihren Anwendungen verfügbar sind.

Die Microsoft Identity Platform gibt Ihnen zusammen mit den Microsoft Identity-SDKs die Möglichkeit, Ihren Kunden SSO entweder innerhalb Ihrer eigenen Anwendungssuite oder – mithilfe der Brokerfeatures und Authentifizierungsanwendungen – für das gesamte Gerät bereitzustellen.

In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie das SDK innerhalb Ihrer Anwendung konfigurieren, damit Ihre Kunden von SSO profitieren.

In diesem Dokument wird davon ausgegangen, dass Sie wissen, wie Sie Ihre Anwendung in das [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android) integrieren.

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO-Konzepte in der Microsoft Identity-Plattform
### <a name="microsoft-identity-brokers"></a>Microsoft-Identitätsbroker
Microsoft stellt Anwendungen für jede mobile Plattform bereit, die als Brücke für Anmeldeinformationen von Anwendungen verschiedener Anbieter fungieren sowie spezielle erweiterte Features unterstützen, bei denen die Überprüfung der Anmeldeinformationen über eine einzige sichere Stelle erfolgt. Diese **Broker** werden durch das SDK aufgerufen. Unter iOS und Android werden die Broker über herunterladbare Anwendungen bereitgestellt. Diese Anwendungen können Kunden entweder unabhängig installieren, oder sie werden von Unternehmen, die einige oder alle Geräte für ihre Mitarbeiter verwalten, per Push auf das Gerät übertragen. Diese Broker unterstützen die Sicherheitsverwaltung basierend auf der Konfiguration des IT-Administrators entweder nur für einige Anwendungen oder für das gesamte Gerät. Unter Windows wird diese Funktionalität durch eine Kontoauswahl bereitgestellt, die in das Betriebssystem integriert ist. Die technische Bezeichnung hierfür lautet Webauthentifizierungsbroker.

#### <a name="broker-assisted-logins"></a>Brokergestützte Anmeldungen
Brokergestützte Anmeldungen sind Anmeldeabläufe, die innerhalb der Brokeranwendung stattfinden und auf den Speicher und die Sicherheit des Brokers zurückgreifen, um Anmeldeinformationen für alle Anwendungen auf dem Gerät einzusetzen, die die Microsoft Identity Platform anwenden. Das bedeutet, dass Ihre Anwendungen bei der Benutzeranmeldung auf den Broker zurückgreifen. Unter iOS und Android werden diese Broker über herunterladbare Anwendungen bereitgestellt. Diese Anwendungen können Kunden entweder unabhängig installieren oder sie werden von Unternehmen, die das Gerät für ihre Benutzer verwalten, per Push auf das Gerät übertragen. Ein Beispiel für diesen Anwendungstyp ist die Microsoft Authenticator-App unter iOS. Unter Windows wird diese Funktionalität durch eine Kontoauswahl bereitgestellt, die in das Betriebssystem integriert ist. Die technische Bezeichnung hierfür lautet Webauthentifizierungsbroker.
Der Ablauf variiert je nach Plattform, und eine nicht ordnungsgemäße Verwaltung kann die Benutzererfahrung beeinträchtigen. Sie kennen dieses Muster sicherlich, wenn Sie Facebook installiert haben und den Facebook Connect-Dienst in einer anderen Anwendung verwenden. Die Microsoft Identity Platform bedient sich des gleichen Musters.

Unter Android wird die Kontoauswahl im Vordergrund Ihrer Anwendung angezeigt, was für den Benutzer weniger störend ist.

#### <a name="how-the-broker-gets-invoked"></a>Aufrufen des Brokers
Wenn ein kompatibler Broker auf dem Gerät installiert ist, beispielsweise die Microsoft Authenticator-Anwendung, übernehmen die Microsoft Identity SDKs automatisch den Aufruf des Brokers für Sie, wenn ein Benutzer angibt, dass er sich mit einem beliebigen Konto von der Microsoft Identity-Plattform aus anmelden möchte. 
 
 #### <a name="how-microsoft-ensures-the-application-is-valid"></a>So stellt Microsoft sicher, dass Ihre Anwendung gültig ist
 
 Die Identität einer Anwendung muss durch Aufrufen des Brokers sichergestellt werden. Denn dies ist ausschlaggebend, um Sicherheit bei brokergestützten Anmeldungen zu gewährleisten. Weder iOS noch Android erfordern eindeutige IDs, die nur für eine bestimmte Anwendung gültig sind. Somit können bösartige Anwendungen möglicherweise die ID einer legitimen Anwendung spoofen und die Token empfangen, die für die legitime Anwendung bestimmt sind. Um sicherzustellen, dass Microsoft zur Laufzeit stets mit der richtigen Anwendung kommunizieren, wird der Entwickler aufgefordert, bei der Registrierung seiner Anwendung bei Microsoft einen benutzerdefinierten redirectURI anzugeben. **Im Folgenden wird ausführlich beschrieben, wie Entwickler diesen Umleitung-URI erstellen sollten.** Dieser benutzerdefinierte Umleitungs-URI beinhaltet den Zertifikatfingerabdruck der Anwendung und wird vom Google Play Store als eindeutig für die Anwendung angesehen. Wenn eine Anwendung den Broker aufruft, fordert der Broker das Android-Betriebssystem auf, den Zertifikatfingerabdruck bereitzustellen, mit dem der Broker aufgerufen wurde. Der Broker stellt diesen Zertifikatfingerabdruck für Microsoft beim Aufrufen des Identitätssystems bereit. Wenn der Zertifikatfingerabdruck der Anwendung nicht mit dem Zertifikatfingerabdruck übereinstimmt, der uns vom Entwickler bei der Registrierung bereitgestellt wurde, wird der Zugriff auf die Token für die Ressource verweigert, die von der Anwendung angefordert wird. Durch diese Prüfung wird sichergestellt, dass nur die vom Entwickler registrierte Anwendung Tokens empfängt.

Durch SSO vermittelte Anmeldungen bieten folgende Vorteile:

* Benutzer profitieren bei allen ihren Anwendungen von SSO – unabhängig vom Anbieter.
* Ihre Anwendung kann mit innovativeren Geschäftsfunktionen wie etwa dem bedingten Zugriff verwendet werden oder Intune-Szenarios unterstützen.
* Ihre Anwendung kann eine zertifikatbasierte Authentifizierung für Geschäftsbenutzer unterstützen.
* Die Anmeldung ist sicherer, da die Identität der Anwendung und des Benutzers von der Brokeranwendung mit zusätzlichen Sicherheitsalgorithmen und Verschlüsselung verifiziert wird.

Die folgende Abbildung zeigt, wie die Microsoft Identity SDKs zusammen mit den Brokeranwendungen für die Bereitstellung von SSO sorgen:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Mit diesen Hintergrundinformationen verfügen Sie nun über ein besseres Verständnis der Funktionsweise und Implementierung von SSO innerhalb Ihrer Anwendung mithilfe der Microsoft Identity-Plattform und den zugehörigen SDKs verfügen.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Aktivieren von SSO für brokergestütztes SSO
Die Fähigkeit einer Anwendung, jeden der auf dem Gerät installierten Broker zu verwenden, ist **standardmäßig deaktiviert**. Damit Ihre Anwendung mit dem Broker verwendet werden kann, müssen Sie einige zusätzliche Konfigurationsschritte ausführen und Ihrer Anwendung Code hinzufügen.

Erforderliche Schritte:

1. Aktivieren des Brokermodus im Anwendungscode für den MS SDK-Aufruf
2. Festlegen eines neuen Umleitungs-URI und Bereitstellen dieses URI in Ihrer App und App-Registrierung
3. Einrichten der korrekten Berechtigungen im Android-Manifest

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Schritt 1: Aktivieren des Brokermodus in Ihrer Anwendung
Die Fähigkeit Ihrer Anwendung zur Verwendung des Brokers wird aktiviert, wenn Sie die Einstellungen oder die anfängliche Einrichtung Ihrer Authentifizierungsinstanz erstellen. Legen Sie hierzu in Ihrer App Folgendes fest:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Schritt 2: Festlegen eines neuen Umleitungs-URI mit Ihrem URL-Schema
Um sicherzustellen, dass die Anmeldeinformationstoken an die richtige Anwendung zurückgegeben werden, muss sichergestellt werden, dass der Rückruf an Ihre Anwendung in einer Weise erfolgt, die das Android-Betriebssystem überprüfen kann. Das Android-Betriebssystem verwendet den Hash des Zertifikats im Google Play Store. Dieser Hash des Zertifikats kann nicht durch eine nicht autorisierte Anwendung ausgespäht werden. Microsoft stellt mit dem URI der Brokeranwendung sicher, dass die Token an die richtige Anwendung zurückgegeben werden. Für die Anwendung muss ein eindeutiger Umleitungs-URI registriert werden.

Der Umleitungs-URI muss das folgende Format besitzen:

`msauth://packagename/Base64UrlencodedSignature`

Beispiel: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Sie können diesen Umleitungs-URI in Ihrer App-Registrierung über das [Azure-Portal](https://portal.azure.com/) registrieren. Weitere Informationen zur Azure AD-App-Registrierung finden Sie unter [Integration in Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Schritt 3: Einrichten der korrekten Berechtigungen in Ihrer Anwendung
Die Brokeranwendung in Android verwendet das Feature zur Kontoverwaltung des Android-Betriebssystems, um Anmeldeinformationen anwendungsübergreifend zu verwalten. Damit der Broker in Android verwendet werden kann, muss Ihr App-Manifest über Berechtigungen zum Verwenden von AccountManager-Konten verfügen. Diese Berechtigungen werden in der [Google-Dokumentation zu Account Manager](http://developer.android.com/reference/android/accounts/AccountManager.html) ausführlich beschrieben.

Insbesondere sind dies folgende Berechtigungen:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Sie haben SSO konfiguriert!
Das Microsoft Identity SDK verwendet Anmeldeinformationen nun automatisch anwendungsübergreifend und ruft den Broker auf, wenn dieser auf dem Gerät vorhanden ist.

