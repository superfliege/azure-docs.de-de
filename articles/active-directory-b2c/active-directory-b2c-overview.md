---
title: Was ist Azure Active Directory B2C? | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Anwendungsanmeldung mithilfe von Azure Active Directory B2C erstellen und verwalten können.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 04/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6949ab89cf806818783c86199e6df334e263b046
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440880"
---
# <a name="what-is-azure-active-directory-b2c"></a>Was ist Azure Active Directory B2C?

Azure Active Directory (Azure AD) B2C ist ein Identitätsverwaltungsdienst, mit dem Sie die Kundenregistrierung und -anmeldung anpassen und steuern sowie Profile bei der Verwendung Ihrer Anwendungen verwalten können. Dazu gehören u.a. für iOS, Android und .NET entwickelte Anwendungen. Azure AD B2C ermöglicht diese Aktionen und schützt gleichzeitig die Identitäten Ihrer Kunden.

Sie können eine in Azure AD B2C registrierte Anwendung konfigurieren, um verschiedene Aktionen zur Identitätsverwaltung auszuführen. Hier einige Beispiele:

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

Azure AD B2C unterstützt [OpenID Connect](https://openid.net/connect/) für alle Kunden. In der Azure AD B2C-Implementierung von OpenID Connect initiiert Ihre Anwendung diese Benutzeroberfläche, indem Authentifizierungsanforderungen für Azure AD B2C ausgegeben werden. Das Ergebnis der Anforderung ist ein `id_token`. Dieses Sicherheitstoken stellt die Identität des Kunden dar.

Jede Anwendung, die Azure AD B2C verwendet, muss über das Azure-Portal in einem Azure AD B2C-Mandanten registriert werden. Beim Registrierungsprozess werden Werte erfasst und der Anwendung zugewiesen. Dazu gehören eine Anwendungs-ID, die die Anwendung eindeutig identifiziert, und ein Umleitungs-URI, der verwendet werden kann, um Antworten an die Anwendung zurückzuleiten.

Die Interaktion der einzelnen Anwendungen folgt einem ähnlichen allgemeinen Muster:

1. Der Kunde wird in der Anweisung angewiesen, eine Richtlinie auszuführen.
2. Der Kunde schließt die Richtlinie gemäß Richtliniendefinition ab.
3. Die Anwendung empfängt ein Sicherheitstoken.
4. Die Anwendung verwendet das Sicherheitstoken für den Zugriff auf eine geschützte Ressource.
5. Der Ressourcenserver überprüft das Sicherheitstoken, um sicherzustellen, dass der Zugriff gewährt werden kann.
6. Die Anwendung aktualisiert das Sicherheitstoken in regelmäßigen Abständen.

Diese Schritte können sich je nach erstelltem Anwendungstyp geringfügig unterscheiden.

Azure AD B2C interagiert der Reihe nach mit Identitätsanbietern, Kunden, anderen Systemen und dem lokalen Verzeichnis, um eine Identitätsaufgabe abzuschließen. Beispiel: Anmelden eines Kunden, Registrieren eines neuen Kunden oder Zurücksetzen eines Kennworts. Die zugrunde liegende Plattform, die die Vertrauensstellung zwischen mehreren Seiten herstellt und diese Schritte ausführt, wird als Identity Experience Framework bezeichnet. Mit diesem Framework und einer Richtlinie (auch als „User Journey“ oder „Vertrauensframework-Richtlinie“ bezeichnet) werden explizit die Akteure, Aktionen, Protokolle und die Abfolge der durchzuführenden Schritte definiert.

Azure AD B2C schützt Ihre Anwendungen auf verschiedene Weise vor Denial-of-Service- und Kennwortangriffen. Azure AD B2C verwendet Erkennungs- und Abwehrmaßnahmen wie SYN-Cookies und Raten- und Verbindungsbeschränkungen, um Ressourcen vor Denial-of-Service-Angriffen zu schützen. Diese decken zudem u.a. Brute-Force-Kennwortangriffe und Wörterbuchangriffe ab.

#### <a name="built-in-policies"></a>Integrierte Richtlinien

Für jede Anforderung, die an Azure AD B2C gesendet wird, wird eine Richtlinie angegeben. Eine Richtlinie steuert das Verhalten der Interaktion zwischen Ihrer Anwendung und Azure AD B2C. Für die am häufigsten verwendeten Identitätsaufgaben sind integrierte Richtlinien vordefiniert, z.B. Registrierung, Anmeldung und Profilbearbeitung.  Eine Registrierungsrichtlinie ermöglicht es z. B., das Verhalten zu steuern, indem die folgenden Einstellungen konfiguriert werden:

- Konten für soziale Netzwerke, die der Kunde zur Registrierung für die Anwendung verwenden kann
- Vom Kunden erfasste Daten, z.B. Vorname oder Postleitzahl
- Multi-Factor Authentication
- Erscheinungsbild aller Registrierungsseiten
- Informationen, die an die Anwendung zurückgegeben werden

#### <a name="custom-policies"></a>Benutzerdefinierte Richtlinien 

[Benutzerdefinierte Richtlinien](active-directory-b2c-overview-custom.md) sind Konfigurationsdateien, die das Verhalten des Identity Experience Framework in Ihrem Azure AD B2C-Mandanten definieren. Benutzerdefinierte Richtlinien können zur Durchführung vieler Aufgaben vollumfänglich bearbeitet werden. Eine benutzerdefinierte Richtlinie wird als eine oder mehrere XML-formatierte Dateien dargestellt, die aufeinander in einer hierarchischen Kette verweisen. 

Mehrere benutzerdefinierte Richtlinien verschiedener Typen können in Ihrem Azure AD B2C-Mandanten bei Bedarf verwendet und anwendungsübergreifend wiederverwendet werden. Durch diese Flexibilität können Sie Benutzeroberflächen im Zusammenhang mit der Kundenidentität mit minimalen oder ganz ohne Änderungen am Code definieren und ändern. Richtlinien können durch Hinzufügen eines speziellen Abfrageparameters in HTTP-Authentifizierungsanforderungen verwendet werden.

Benutzerdefinierte Richtlinien können auf folgende Weise zur Steuerung von User Journeys verwendet werden:

- Definieren der Interaktion mit APIs zum Erfassen zusätzlicher Informationen, Überprüfen vom Kunden bereitgestellter Ansprüche oder Auslösen externer Prozesse.
- Ändern des Verhaltens basierend auf von APIs stammenden Ansprüchen oder von Ansprüchen im Verzeichnis wie z.B. *migrationStatus*.
- Alle Workflows, die nicht durch integrierte Richtlinien abgedeckt sind, beispielsweise Erfassen zusätzlicher Informationen von einem Kunden bei der Anmeldung und Durchführen einer Autorisierungsprüfung für den Zugriff auf eine Ressource.

### <a name="identity-providers"></a>Identitätsanbieter

Ein Identitätsanbieter ist ein Dienst, der Kundenidentitäten authentifiziert und Sicherheitstokens ausgibt. In Azure AD B2C können Sie verschiedene Identitätsanbieter in Ihrem Mandanten konfigurieren, z.B. ein Microsoft-Konto, Facebook oder Amazon. 

Zum Konfigurieren eines Identitätsanbieters in Ihrem Azure AD B2C-Mandanten müssen Sie den Anwendungsbezeichner oder die Client-ID und das Kennwort oder Clientgeheimnis der erstellten Identitätsanbieteranwendung aufzeichnen. Mit diesem Bezeichner und Kennwort wird die Anwendung dann konfiguriert.

### <a name="user-interface-experience"></a>Benutzeroberfläche

Die meisten der HTML- und CSS-Inhalte, die für Kunden angezeigt werden, können gesteuert werden. Mit dem Feature zum Anpassen der Benutzeroberfläche können Sie das Erscheinungsbild von Richtlinien anpassen. Außerdem können Sie die Konsistenz von Marken und visuellen Elementen zwischen Ihrer Anwendung und Azure AD B2C verwalten.

Azure AD B2C führt den Code im Browser des Kunden aus und verwendet einen modernen Ansatz namens Cross-Origin Resource Sharing (CORS, Ressourcenfreigabe zwischen verschiedenen Ursprüngen). Zuerst legen Sie eine URL in einer Richtlinie mit benutzerdefiniertem HTML-Inhalt fest. Azure AD B2C führt die UI-Elemente mit dem HTML-Inhalt, der über Ihre URL geladen wird, zusammen und zeigt anschließend die Seite für den Kunden an.

Sie können Parameter in einer Abfragezeichenfolge an Azure AD B2C senden. Durch Übergeben des Parameters an Ihren HTML-Endpunkt können Sie den Seiteninhalt dynamisch ändern. Sie können z.B. das Hintergrundbild auf der Azure AD B2C-Registrierungs- oder Anmeldeseite auf der Basis eines Parameters ändern, den Sie von der Web- oder Mobilanwendung übergeben.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Wie beginne ich mit der Nutzung von Azure AD B2C?

In Azure AD B2C stellt ein Mandant Ihre Organisation dar und ist ein Verzeichnis von Benutzern. Jeder Azure AD B2C-Mandant unterscheidet sich von anderen Azure AD B2C-Mandanten und ist von diesen getrennt. Ein Mandant enthält Informationen über die Kunden, die sich zur Verwendung Ihrer Anwendung registriert haben. Dazu gehören beispielsweise Kennwörter, Profildaten und Berechtigungen.

Sie müssen Ihren Azure AD B2C-Mandanten mit Ihrem Azure-Abonnement verknüpfen, um die gesamte Funktionalität zu aktivieren und die Nutzungsgebühren zahlen zu können. Damit sich Azure AD B2C-Kunden bei Ihrer Anwendung anmelden können, müssen Sie die Anwendung in einem Azure AD B2C-Mandanten registrieren.

Damit Sie Ihre Anwendung zur Verwendung von Azure AD B2C konfigurieren können, müssen Sie zunächst einen Azure AD B2C-Mandanten erstellen und die Anwendung registrieren. Führen Sie zum Registrieren der Anwendung die Schritte unter [Tutorial: Registrieren einer Anwendung zum Aktivieren der Registrierung und Anmeldung mit Azure AD B2C](tutorial-register-applications.md) aus.
  
Als Entwickler von ASP.NET-Webanwendungen richten Sie Ihre Anwendung zum Authentifizieren von Konten entsprechend den Schritten unter [Tutorial: Authentifizieren von Benutzern mit Azure Active Directory B2C in einer ASP.NET-Web-App](active-directory-b2c-tutorials-web-app.md) ein.

Als Entwickler von Desktopanwendungen richten Sie Ihre Anwendung zum Authentifizieren von Konten entsprechend den Schritten unter [Tutorial: Authentifizieren von Benutzern mit Azure Active Directory B2C in einer Desktopanwendung](active-directory-b2c-tutorials-desktop-app.md) ein.

Als Entwickler von Single-Page-Webanwendungen richten Sie Ihre Anwendung zum Authentifizieren von Konten entsprechend den Schritten unter [Tutorial: Authentifizieren von Benutzern mit Azure Active Directory B2C in einer Single-Page-Webanwendung](active-directory-b2c-tutorials-spa.md) ein.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem Tutorial fort, und beginnen Sie mit der Konfiguration Ihrer Anwendung für die Registrierung und Anmeldung.

> [!div class="nextstepaction"]
> [Tutorial: Registrieren einer Anwendung zum Aktivieren der Registrierung und Anmeldung mit Azure Active Directory B2C](tutorial-register-applications.md)
