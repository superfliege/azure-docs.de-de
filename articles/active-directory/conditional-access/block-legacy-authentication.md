---
title: Blockieren der Legacyauthentifizierung bei Azure Active Directory (Azure AD) mit bedingtem Zugriff | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Richtlinie für bedingten Zugriff in Azure Active Directory (Azure AD) für Zugriffsversuche von nicht vertrauenswürdigen Netzwerken konfigurieren.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.subservice: conditional-access
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2019
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 164ff12eeec057855476d3808d3d1d44ef3179fe
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076842"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Gewusst wie: Blockieren der Legacyauthentifizierung bei Azure AD mit bedingtem Zugriff   

Um Ihren Benutzern den einfachen Zugriff auf Ihre Cloud-Apps zu ermöglichen, unterstützt Azure Active Directory (Azure AD) eine Vielzahl von Authentifizierungsprotokollen einschließlich der Legacyauthentifizierung. Ältere Protokolle unterstützen jedoch nicht die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA). MFA ist in vielen Umgebungen eine allgemeine Anforderung zum Schutz vor Identitätsdiebstahl. 

Wenn Ihre Umgebung für das Blockieren der Legacyauthentifizierung bereit ist, um den Schutz Ihres Mandanten zu verbessern, können Sie dieses Ziel mit bedingtem Zugriff erreichen. In diesem Artikel wird erläutert, wie Sie die Richtlinien für bedingten Zugriff konfigurieren können, mit denen die Legacyauthentifizierung für Ihren Mandanten blockiert wird.



## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind: 

- Mit den [grundlegenden Konzepten](overview.md) des bedingten Azure AD-Zugriffs 
- Mit den [Best Practices](best-practices.md) für das Konfigurieren von Richtlinien für bedingten Zugriff im Azure-Portal



## <a name="scenario-description"></a>Beschreibung des Szenarios

Azure AD unterstützt mehrere der am häufigsten verwendeten Protokolle zur Authentifizierung und Autorisierung, einschließlich der Legacyauthentifizierung. Die Legacyauthentifizierung bezieht sich auf Protokolle, die die Standardauthentifizierung verwenden. In der Regel können diese Protokolle keinen Typ der zweistufigen Authentifizierung erzwingen. Beispiele für Apps, die auf der Legacyauthentifizierung basieren, sind:

- Ältere Microsoft Office-Apps

- Apps, die E-Mail-Protokolle wie POP, IMAP und SMTP verwenden

Eine einstufige Authentifizierung (z. B. mit Benutzername und Kennwort) reicht heutzutage nicht mehr aus. Kennwörter sind unzulänglich, weil sie leicht zu erraten sind, und wir (Menschen) sind schlecht darin, gute Kennwörter auszuwählen. Kennwörter sind auch für eine Vielzahl von Angriffen wie Phishing und Kennwort-Spray anfällig. Eine der einfachsten Maßnahmen, die Sie ergreifen können, um sich vor Kennwortsicherheitsverletzungen zu schützen, ist das Implementieren von MFA. Denn selbst wenn ein Angreifer in den Besitz des Kennworts eines Benutzers gelangt, reicht bei Verwendung von MFA das Kennwort allein nicht aus, um sich erfolgreich authentifizieren und auf die Daten zugreifen zu können.

Wie können Sie verhindern, dass Apps mit Legacyauthentifizierung auf Ressourcen Ihres Mandanten zugreifen? Es wird empfohlen, diese Apps einfach mit einer Richtlinie für bedingten Zugriff zu blockieren. Gegebenenfalls können Sie nur bestimmten Benutzern und bestimmten Netzwerkadressen die Verwendung von Apps erlauben, die auf der Legacyauthentifizierung basieren.




## <a name="implementation"></a>Implementierung

In diesem Abschnitt wird erläutert, wie eine Richtlinie für bedingten Zugriff zum Blockieren der Legacyauthentifizierung konfiguriert wird. 

### <a name="block-legacy-authentication"></a>Blockieren älterer Authentifizierungsmethoden 

In einer Richtlinie für bedingten Zugriff können Sie eine Bedingung festlegen, die an die Client-Apps gebunden ist, die für den Zugriff auf Ihre Ressourcen verwendet werden. Mit der Client-App-Bedingung können Sie den Bereich für Apps mit Legacyauthentifizierung eingrenzen, indem Sie für **Mobile Apps und Desktopclients** die Option **Andere Clients** auswählen.

![Andere Clients](./media/block-legacy-authentication/01.png)

Um den Zugriff für diese Apps zu blockieren, müssen Sie **Zugriff blockieren** auswählen.

![Zugriff blockieren](./media/block-legacy-authentication/02.png)


### <a name="select-users-and-cloud-apps"></a>Auswählen von Benutzern und Cloud-Apps

Wenn Sie die Legacyauthentifizierung für Ihre Organisation blockieren möchten, denken Sie wahrscheinlich, dass Sie dies durch Auswählen der folgenden Optionen erreichen können:

- Alle Benutzer

- Alle Cloud-Apps

- Zugriff blockieren
 

![Arbeitsaufträge](./media/block-legacy-authentication/03.png)



Azure verfügt über eine Sicherheitsfunktion, die Sie am Erstellen einer solchen Richtlinie hindert, weil diese Konfiguration gegen die [Best Practices](best-practices.md) für Richtlinien für bedingten Zugriff verstößt.
 
![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/04.png)


Diese Sicherheitsfunktion ist erforderlich, weil das *Blockieren aller Benutzer und aller Cloud-Apps* potenziell dazu führen kann, für Ihre gesamte Organisation die Anmeldung bei Ihrem Mandanten zu blockieren. Sie müssen mindestens einen Benutzer davon ausschließen, um die Mindestanforderung der Best Practices zu erfüllen. Sie können auch eine Verzeichnisrolle ausschließen.

![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/05.png)


Sie können diese Sicherheitsfunktion erfüllen, indem Sie einen Benutzer aus der Richtlinie ausschließen. Im Idealfall sollten Sie einige [Administratorkonten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md) definieren und diese aus der Richtlinie ausschließen.
 

## <a name="policy-deployment"></a>Richtlinienbereitstellung

Kümmern Sie sich vor dem Einsatz der Richtlinie in der Produktionsumgebung um Folgendes:
 
- **Dienstkonten**: Identifizieren Sie Benutzerkonten, die als Dienstkonten verwendet werden oder nach Geräten, wie Telefone im Konferenzraum. Stellen Sie sicher, dass diese Konten über sichere Kennwörter verfügen, und fügen Sie sie einer ausgeschlossenen Gruppe hinzu.
 
- **Anmeldeberichte**: Überprüfen Sie den Anmeldebericht, und suchen Sie nach dem Datenverkehr **anderer Clients**. Identifizieren Sie die höchste Nutzung, und untersuchen Sie den Grund für die Verwendung. In der Regel wird der Datenverkehr von älteren Office-Clients generiert, die keine moderne Authentifizierung oder einige E-Mail-Apps von Drittanbietern verwenden. Erarbeiten Sie einen Plan, um die Nutzung dieser Apps zu reduzieren, oder wenn die Auswirkungen gering sind, teilen Sie Ihren Benutzern mit, dass sie diese Apps nicht mehr verwenden können.
 
Weitere Informationen finden Sie unter [Wie stellen Sie eine neue Richtlinie bereit?](best-practices.md#how-should-you-deploy-a-new-policy)



## <a name="what-you-should-know"></a>Wichtige Informationen

Durch das Konfigurieren einer Richtlinie für **Andere Clients** wird die gesamte Organisation für bestimmte Clients blockiert, z.B. SPConnect. Dies tritt ein, weil sich ältere Clients auf unerwartete Weise authentifizieren. Dieses Problem gilt nicht für Office-Hauptanwendungen wie ältere Office-Clients.

Es kann bis zu 24 Stunden dauern, bis die Richtlinie wirksam wird.

Sie können alle verfügbaren Gewährungssteuerelemente für die Bedingung „Andere Clients“ auswählen. Die Endbenutzererfahrung ist jedoch immer die gleiche: Der Zugriff ist blockiert.

Sie können alle anderen Bedingungen neben der Bedingung „Andere Clients“ konfigurieren.
Wenn Sie nur ältere Authentifizierungen für mobile Geräte blockieren möchten, legen Sie z.B. die Bedingung **Geräteplattformen** fest, indem Sie Folgendes auswählen:

- Android

- iOS

- Windows Phone

![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/06.png)




## <a name="next-steps"></a>Nächste Schritte

Wenn Sie noch nicht mit dem Konfigurieren von Richtlinien für bedingten Zugriff vertraut sind, sehen Sie sich das Beispiel unter [Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md) an.
