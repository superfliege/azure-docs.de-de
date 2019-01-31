---
title: Was ist Azure Active Directory B2C? | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Anwendungsanmeldung mithilfe von Azure Active Directory B2C erstellen und verwalten.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: a7ae48d65a5cceb11339c4f304a3149741c2a7e8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55172764"
---
# <a name="what-is-azure-active-directory-b2c"></a>Was ist Azure Active Directory B2C?

Azure Active Directory (Azure AD) B2C ist ein Identitätsverwaltungsdienst, mit dem Sie die Interaktion von Kunden mit Ihrer Anwendung anpassen und steuern können. Diese Interaktion umfasst unter anderem das An- und Abmelden sowie das Verwalten des eigenen Profils, wenn Kunden Ihre Anwendungen verwenden. Zu Auswahl stehen unter anderem Anwendungen für iOS, Android und .NET. Azure AD B2C ermöglicht diese Aktionen und schützt gleichzeitig die Identitäten Ihrer Kunden.

Sie können eine in Azure AD B2C registrierte Anwendung so konfigurieren, dass sie verschiedene Identitätsverwaltungsaufgaben übernimmt. Hier einige Beispiele:

- Einem Kunden ermöglichen, sich zur Verwendung Ihrer registrierten Anwendung zu registrieren
- Einem registrierten Kunden ermöglichen, sich anzumelden und die Anwendung zu verwenden
- Einem registrierten Kunden ermöglichen, sein Profil zu bearbeiten
- Aktivieren der mehrstufigen Authentifizierung in der Anwendung
- Dem Kunden ermöglichen, sich bei bestimmten Identitätsanbietern zu registrieren und anzumelden
- Gewähren von Zugriff in Ihrer Anwendung auf von Ihnen erstellte APIs
- Anpassen des Erscheinungsbilds für die Registrierung und Anmeldung
- Verwalten von SSO-Sitzungen für Ihre Anwendung

## <a name="what-do-i-need-to-think-about-before-using-azure-ad-b2c"></a>Was muss ich vor der Verwendung von Azure AD B2C berücksichtigen?

- Wie soll der Kunde mit der Anwendung interagieren können?
- Welche Benutzeroberfläche möchte ich Kunden bereitstellen?
- Welche Identitätsanbieter sollen Kunden in der Anwendung auswählen können?
- Müssen beim Anmeldevorgang zusätzliche APIs ausgeführt werden?

### <a name="customer-interaction"></a>Kundeninteraktion

Azure AD B2C unterstützt [OpenID Connect](https://openid.net/connect/) für alle Kunden. In der Azure AD B2C-Implementierung von OpenID Connect startet Ihre Anwendung die User Journey, indem sie Authentifizierungsanforderungen an Azure AD B2C ausgibt. Das Ergebnis der Anforderung ist ein `id_token`. Dieses Sicherheitstoken definiert die Identität des Kunden.

Jede Anwendung, die Azure AD B2C verwendet, muss über das Azure-Portal in einem Azure AD B2C-Mandanten registriert werden. Beim Registrierungsprozess werden Werte erfasst und der Anwendung zugewiesen. Zu diesen Werten gehört auch eine Anwendungs-ID, die die Anwendung eindeutig identifiziert. Es wird ein Umleitungs-URI definiert, der dazu dient, Antworten wieder an die Anwendung zurückzuleiten.

Die Interaktion der einzelnen Anwendungen folgt einem ähnlichen allgemeinen Muster:

1. Der Kunde wird in der Anweisung angewiesen, eine Richtlinie auszuführen.
2. Der Kunde schließt die Richtlinie gemäß Richtliniendefinition ab.
3. Die Anwendung empfängt ein Sicherheitstoken.
4. Die Anwendung verwendet das Sicherheitstoken für den Zugriff auf eine geschützte Ressource.
5. Der Ressourcenserver überprüft das Sicherheitstoken, um sicherzustellen, dass der Zugriff gewährt werden kann.
6. Die Anwendung aktualisiert das Sicherheitstoken in regelmäßigen Abständen.

Diese Schritte unterscheiden sich je nach erstelltem Anwendungstyp geringfügig.

Azure AD B2C interagiert der Reihe nach mit Identitätsanbietern, Kunden, anderen Systemen und dem lokalen Verzeichnis, um eine Identitätsaufgabe abzuschließen. Beispiel: Anmelden eines Kunden, Registrieren eines neuen Kunden oder Zurücksetzen eines Kennworts. Die zugrunde liegende Plattform, die die Vertrauensstellung zwischen mehreren Seiten herstellt und diese Schritte ausführt, wird als Identity Experience Framework bezeichnet. Mit diesem Framework und einer Richtlinie (auch als „User Journey“ oder „Vertrauensframework-Richtlinie“ bezeichnet) werden explizit die Akteure, Aktionen, Protokolle und die Abfolge der durchzuführenden Schritte definiert.

Azure AD B2C schützt Ihre Anwendungen vor Denial-of-Service- und Kennwortangriffen. Azure AD B2C verwendet Erkennungs- und Abwehrmaßnahmen wie SYN-Cookies und Raten- und Verbindungsbeschränkungen, um Ressourcen vor Denial-of-Service-Angriffen zu schützen. Diese decken zudem u.a. Brute-Force-Kennwortangriffe und Wörterbuchangriffe ab.

#### <a name="user-flows"></a>Benutzerabläufe

Jede an Azure AD B2C gesendete Anforderung gibt einen Benutzerflow an. Dabei handelt es sich um eine Richtlinie, die das Verhalten Ihrer Anwendung bei der Interaktion mit Azure AD B2C steuert. Für die am häufigsten verwendeten Identitätsaufgaben (z.B. Registrierung, Anmeldung und Profilbearbeitung) sind vordefinierte Benutzerflows im Azure AD B2C-Portal verfügbar.  Ein Registrierungsbenutzerflow ermöglicht beispielsweise das Steuern des Verhaltens durch Konfigurieren der folgenden Einstellungen:

- Konten für soziale Netzwerke, mit denen sich der Kunde bei der Anwendung registrieren kann
- Vom Kunden erfasste Daten, z.B. Vorname oder Postleitzahl
- Multi-Factor Authentication
- Erscheinungsbild aller Registrierungsseiten
- Informationen, die an die Anwendung zurückgegeben werden

#### <a name="custom-policies"></a>Benutzerdefinierte Richtlinien 

[Benutzerdefinierte Richtlinien](active-directory-b2c-overview-custom.md) sind Konfigurationsdateien, die das Verhalten des [Identity Experience Framework](trustframeworkpolicy.md) in Ihrem Azure AD B2C-Mandanten definieren. Benutzerdefinierte Richtlinien können zur Durchführung zahlreicher Aufgaben geändert werden. Bei benutzerdefinierten Richtlinien handelt es sich um einzelne oder mehrere Dateien im XML-Format, die in einer hierarchischen Kette aufeinander verweisen. 

Benutzerdefinierte Richtlinien verschiedener Typen werden in Ihrem Azure AD B2C-Mandanten nach Bedarf verwendet und können anwendungsübergreifend wiederverwendet werden. Durch diese Flexibilität können Sie Benutzeroberflächen im Zusammenhang mit der Kundenidentität mit minimalen oder ganz ohne Änderungen am Code definieren und ändern. Richtlinien werden durch Hinzufügen eines speziellen Abfrageparameters in HTTP-Authentifizierungsanforderungen verwendet.

Benutzerdefinierte Richtlinien werden wie folgt zur Steuerung von User Journeys verwendet:

- Definieren der Interaktion mit APIs zum Erfassen zusätzlicher Informationen, Überprüfen vom Kunden bereitgestellter Ansprüche oder Auslösen externer Prozesse.
- Ändern des Verhaltens basierend auf von APIs stammenden Ansprüchen oder von Ansprüchen im Verzeichnis wie z.B. *migrationStatus*.
- Alle Workflows, die nicht durch integrierte Richtlinien abgedeckt sind, beispielsweise Erfassen zusätzlicher Informationen von einem Kunden bei der Anmeldung und Durchführen einer Autorisierungsprüfung für den Zugriff auf eine Ressource.

### <a name="identity-providers"></a>Identitätsanbieter

Ein Identitätsanbieter ist ein Dienst, der Kundenidentitäten authentifiziert und Sicherheitstokens ausgibt. In Azure AD B2C konfigurieren Sie verschiedene Identitätsanbieter in Ihrem Mandanten – etwa ein [Microsoft-Konto](active-directory-b2c-setup-msa-app.md), [Facebook](active-directory-b2c-setup-fb-app.md) oder [Amazon](active-directory-b2c-setup-amzn-app.md). 

Zum Konfigurieren eines Identitätsanbieters in Ihrem Azure AD B2C-Mandanten müssen Sie den Anwendungsbezeichner oder die Client-ID und das Kennwort oder Clientgeheimnis der erstellten Identitätsanbieteranwendung aufzeichnen. Mit diesem Bezeichner und Kennwort wird die Anwendung dann konfiguriert.

### <a name="user-interface-experience"></a>Benutzeroberfläche

Die meisten der HTML- und CSS-Inhalte, die für Kunden angezeigt werden, sind steuerbar. Mit dem Feature zum Anpassen der Benutzeroberfläche können Sie das Erscheinungsbild von Richtlinien anpassen. Darüber hinaus dient dieses Anpassungsfeature zur Steuerung der Konsistenz von Marken und visuellen Elementen zwischen Ihrer Anwendung und Azure AD B2C.

Azure AD B2C führt den Code im Browser des Kunden aus und verwendet einen modernen Ansatz namens Cross-Origin Resource Sharing (CORS, Ressourcenfreigabe zwischen verschiedenen Ursprüngen). Zuerst legen Sie eine URL in einer Richtlinie mit benutzerdefiniertem HTML-Inhalt fest. Azure AD B2C führt die UI-Elemente mit dem HTML-Inhalt, der über Ihre URL geladen wird, zusammen und zeigt anschließend die Seite für den Kunden an.

Sie senden Parameter in einer Abfragezeichenfolge an Azure AD B2C. Durch Übergeben des Parameters an Ihren HTML-Endpunkt wird der Seiteninhalt dynamisch geändert. So ändern Sie beispielsweise das Hintergrundbild auf der Azure AD B2C-Registrierungs- oder Anmeldeseite auf der Grundlage eines Parameters, den Sie aus Ihrer Web- oder Mobilanwendung übergeben.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Wie beginne ich mit der Nutzung von Azure AD B2C?

In Azure AD B2C stellt ein Mandant Ihre Organisation dar und ist ein Verzeichnis von Benutzern. Jeder Azure AD B2C-Mandant unterscheidet sich von anderen Azure AD B2C-Mandanten und ist von diesen getrennt. Ein Mandant enthält Informationen über die Kunden, die sich zur Verwendung Ihrer Anwendung registriert haben. Dazu gehören beispielsweise Kennwörter, Profildaten und Berechtigungen.

Verknüpfen Sie Ihren Azure AD B2C-Mandanten mit Ihrem Azure-Abonnement, um sämtliche Funktionen zu aktivieren, und bezahlen Sie die Nutzungsgebühren. Führen Sie eine Registrierung bei einem Azure AD B2C-Mandanten durch, um Ihren Kunden die Anmeldung bei Ihrer Anwendung zu ermöglichen.

Damit Sie Ihre Anwendung zur Verwendung von Azure AD B2C konfigurieren können, müssen Sie zunächst einen Azure AD B2C-Mandanten erstellen und die Anwendung registrieren. Führen Sie zum Registrieren der Anwendung die Schritte unter [Tutorial: Registrieren einer Anwendung zum Aktivieren der Registrierung und Anmeldung mit Azure AD B2C](tutorial-register-applications.md) aus.
  
Wenn Sie ASP.NET-Webanwendungen entwickeln, richten Sie Ihre Anwendung zum Authentifizieren von Konten anhand der Schritte unter [Tutorial: Aktivieren der Authentifizierung von Web-Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-web-app.md) ein.

Wenn Sie Desktopanwendungen entwickeln, richten Sie Ihre Anwendung zum Authentifizieren von Konten anhand der Schritte unter [Tutorial: Aktivieren der Authentifizierung von Desktop-Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md) ein.

Wenn Sie Single-Page-Webanwendung mit Node.js entwickeln, richten Sie Ihre Anwendung zum Authentifizieren von Konten anhand der Schritte unter [Tutorial: Aktivieren der Authentifizierung einseitiger Apps mit Konten unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md) ein.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem Tutorial fort, und beginnen Sie mit der Konfiguration Ihrer Anwendung für die Registrierung und Anmeldung.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen eines Azure Active Directory B2C-Mandanten](tutorial-create-tenant.md)
