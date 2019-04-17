---
title: Erzwingen der App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs in Azure Active Directory erzwingen.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 4/4/2019
ms.author: joflore
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: f695d50e251d0104cf9f0d38fe4489a0e66dfe15
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59287561"
---
# <a name="how-to-require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Anleitung: Erzwingen der App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs (Vorschauversion)

Ihre Mitarbeiter verwenden mobile Geräte sowohl für private als auch für berufliche Zwecke. Während Sie einerseits die Produktivität der Mitarbeiter sicherstellen möchten, sollten Sie andererseits Datenverluste vermeiden. Mit dem bedingten Zugriff von Azure Active Directory (Azure AD) können Sie Ihre Unternehmensdaten schützen, indem Sie den Zugriff auf Ihre Cloud-Apps auf Client-Apps beschränken, für die zuerst eine App-Schutzrichtlinie erforderlich ist.

In diesem Thema wird das Konfigurieren von Richtlinien für bedingten Zugriff erläutert, bei denen vor dem Zugriff auf Daten die App-Schutzrichtlinie erforderlich ist.

## <a name="overview"></a>Übersicht

Mit dem [bedingten Zugriff von Azure AD](overview.md) können Sie präzise steuern, wie autorisierte Benutzer auf Ihre Ressourcen zugreifen können. Sie können z.B. den Zugriff auf Ihre Cloud-Apps auf vertrauenswürdige Geräte beschränken.

Zum Schutz Ihrer Unternehmensdaten können Sie [Intune-Richtlinien für den App-Schutz](https://docs.microsoft.com/intune/app-protection-policy) verwenden. Da die Intune-Richtlinien für den App-Schutz keine MDM-Lösung (Mobile-Device Management, Verwaltung mobiler Geräte) erfordern, sind Ihre Unternehmensdaten unabhängig davon geschützt, ob Sie die Geräte in einer Lösung für die Geräteverwaltung registrieren oder nicht.

Der bedingte Zugriff von Azure Active Directory beschränkt den Zugriff auf Ihre Cloud-Apps auf Clientanwendungen, die Intune an Azure AD als Empfänger einer App-Schutzrichtlinie gemeldet hat. Sie können zum Beispiel den Zugriff auf Exchange Online auf die Outlook-App beschränken, die eine Intune-App-Schutzrichtlinie aufweist.

In der Terminologie des bedingten Zugriffs sind diese Client-Apps bekanntermaßen durch eine **App-Schutzrichtlinie** geschützt.  

![Bedingter Zugriff](./media/app-protection-based-conditional-access/05.png)

Eine Liste der durch Richtlinien geschützten Client-Apps finden Sie unter [App-Schutzrichtlinie als Voraussetzung](technical-reference.md#approved-client-app-requirement).

Sie können Richtlinien für auf App-Schutz basierenden bedingten Zugriff mit anderen Richtlinien wie etwa [gerätebasierten Richtlinien für bedingten Zugriff](require-managed-devices.md) kombinieren, um für Flexibilität beim Schutz von Daten für Privat- und Unternehmensgeräte zu sorgen.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Vorteile des auf App-Schutz basierenden bedingten Zugriffs als Voraussetzung

Ähnlich wie bei Compliance, die von Intune für iOS und Android für ein verwaltetes Gerät gemeldet wird, teilt Intune Azure AD jetzt mit, wenn eine App-Schutzrichtlinie angewendet wird, sodass der bedingte Zugriff diese Richtlinie als Zugriffsprüfung verwenden kann. Diese neue Richtlinie für bedingten Zugriff, die **App-Schutzrichtlinie**, erhöht die Sicherheit durch den Schutz vor administrativen Fehlern. Beispiele dafür sind:

- Benutzer, die nicht über eine Intune-Lizenz verfügen
- Benutzer, die keine Intune-App-Schutzrichtlinie erhalten können
- Apps mit Intune-App-Schutzrichtlinie, die nicht für den Erhalt einer Richtlinie konfiguriert wurden


## <a name="before-you-begin"></a>Voraussetzungen

In diesem Thema wird vorausgesetzt, dass Sie mit Folgendem vertraut sind:

- Der technischen Referenz [App-Schutzrichtlinie als Voraussetzung](technical-reference.md#app-protection-policy-requirement)

- Der technischen Referenz [Genehmigte Client-App als Voraussetzung](technical-reference.md#approved-client-app-requirement)

- Den grundlegenden Konzepten des [bedingten Zugriffs in Azure Active Directory](overview.md)

- Der Vorgehensweise zum [Konfigurieren einer Richtlinie zum bedingten Zugriff](app-based-mfa.md)


## <a name="prerequisites"></a>Voraussetzungen

Um eine Richtlinie für auf App-Schutz basierenden bedingten Zugriff erstellen zu können, müssen Sie:
- Über ein Enterprise Mobility + Security- oder ein Azure Active Directory Premium-Abonnement und Intune verfügen
- Sicherstellen, dass die Benutzer für EMS oder Azure AD und Intune lizenziert sind
- Sicherstellen, dass die Client-App in Intune für den Erhalt einer App-Schutzrichtlinie konfiguriert wurde
- Sicherstellen, dass die Benutzer in Intune für den Erhalt einer Intune-App-Schutzrichtlinie konfiguriert wurden

## <a name="app-protection-based-policy-for-exchange-online"></a>Auf App-Schutz basierende Richtlinie für Exchange Online

Dieses Szenario besteht aus einer Richtlinie für auf App-Schutz basierenden bedingten Zugriff für den Zugriff auf Exchange Online.

### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.

- Er erhält eine E-Mail mit dem Hinweis, dass der Zugriff nur über die Outlook-App möglich ist.

- Er lädt die Anwendung über den Link herunter.

- Er öffnet die Outlook-Anwendung und meldet sich mit den Azure AD-Anmeldeinformationen an.

- Er wird aufgefordert, entweder Authenticator (iOS) oder das Unternehmensportal (Android) zu installieren, um den Vorgang fortzusetzen.

- Er installiert die Anwendung und kann zur Outlook-App zurückkehren, um den Vorgang fortzusetzen.

- Er wird aufgefordert, ein Gerät zu registrieren.

- Er kann eine Intune-App-Schutzrichtlinie erhalten.

- Er kann auf die E-Mails zugreifen.

Alle Intune-App-Schutzrichtlinien müssen für die Anwendung gelten, um auf Unternehmensdaten zugreifen zu können, und der Benutzer wird ggf. aufgefordert, die Anwendung neu zu starten, eine zusätzliche PIN zu verwenden oder eine andere Aktion auszuführen (sofern dies für die Anwendung und die Plattform konfiguriert wurde).

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/01.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren:

    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

    b. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Clients mit moderner Authentifizierung**.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

5. Für **Zugriffssteuerungen** müssen Sie **App-Schutzrichtlinie erforderlich (Vorschau)** aktivieren.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/05.png)
 

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/06.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.


3. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen Sie **Client-Apps (Vorschauversion)** konfigurieren. 

    a. Wählen Sie als **Client-Apps (Vorschauversion)** die Option **Mobile Apps und Desktop-Apps** und **ActiveSync-Clients austauschen**.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/92.png)

    b. Für **Zugriffssteuerungen** müssen Sie **App-Schutzrichtlinie erforderlich (Vorschau)** aktivieren.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/05.png)


**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).



## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>Auf App-Schutz basierende Richtlinie oder Richtlinie für konforme Geräte für Exchange Online

Dieses Szenario besteht aus einer Richtlinie für auf App-Schutz basierenden bedingten Zugriff oder einer Richtlinie für bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.


### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für dieses Szenario:
 
- Einige Benutzer sind bereits registriert (mit Unternehmensgeräten oder ohne).

- Benutzer, die nicht bei Azure AD registriert sind und eine durch eine App geschützte Anwendung verwenden, müssen ein Gerät registrieren, um auf Ressourcen zugreifen zu können.

- Registrierte Benutzer, die die durch eine App geschützte Anwendung verwenden, müssen das Gerät nicht erneut registrieren.

- Benutzer können eine Intune-App-Schutzrichtlinie erhalten, wenn sie nicht registriert sind.

- Der Benutzer kann mit Outlook und einer Intune-App-Schutzrichtlinie auf E-Mails zugreifen, wenn er nicht registriert ist.

- Der Benutzer kann mit Outlook auf E-Mails zugreifen, wenn das Gerät registriert ist.


### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/62.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. 

     ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren. 
 
    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

    b. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Clients** und **Clients mit moderner Authentifizierung**.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

   - **Markieren des Geräts als kompatibel erforderlich**

   - **App-Schutzrichtlinie erforderlich (Vorschau)**

   - **Eine der ausgewählten Steuerungen anfordern**   
 
     ![Bedingter Zugriff](./media/app-protection-based-conditional-access/11.png)



**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/06.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. 

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen Sie **Client-Apps** konfigurieren. 

    Wählen Sie als **Client-Apps (Vorschauversion)** die Option **Mobile Apps und Desktop-Apps** und **ActiveSync-Clients austauschen**.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/92.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

   - **Markieren des Geräts als kompatibel erforderlich**

   - **App-Schutzrichtlinie erforderlich (Vorschau)**

   - **Eine der ausgewählten Steuerungen anfordern**   
    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/11.png)



**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).





## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>Auf App-Schutz basierende Richtlinie und Richtlinie für konforme Geräte für Exchange Online

Dieses Szenario besteht aus einer Richtlinie für auf App-Schutz basierenden bedingten Zugriff und einer Richtlinie für bedingten Zugriff für konforme Geräte für den Zugriff auf Exchange Online.


### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:
 
-   Er konfiguriert E-Mails mithilfe einer nativen E-Mail-Anwendung unter iOS oder Android, um eine Verbindung mit Exchange herzustellen.
-   Er erhält eine E-Mail mit dem Hinweis, dass das Gerät für den Zugriff registriert werden muss.
-   Er lädt das Unternehmensportal herunter und meldet sich an.
-   Er ruft seine E-Mails ab und wird zur Verwendung der Outlook-App aufgefordert.
-   Er lädt die Outlook-App herunter.
-   Er öffnet die Outlook-App und gibt die bei der Registrierung verwendeten Anmeldeinformationen ein.
-   Er kann eine Intune-App-Schutzrichtlinie erhalten.
-   Er kann mit Outlook und einer Intune-App-Schutzrichtlinie auf E-Mails zugreifen.

Intune-App-Schutzrichtlinien werden vor dem Zugriff auf die Unternehmensdaten aktiviert, und der Benutzer wird ggf. aufgefordert, die Anwendung neu zu starten, eine zusätzliche PIN zu verwenden oder eine andere Aktion auszuführen (sofern dies für die Anwendung und die Plattform konfiguriert wurde).


### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/01.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. 

     ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren. 
 
    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

    b. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Clients mit moderner Authentifizierung**.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

   - **Markieren des Geräts als kompatibel erforderlich**

   - **App-Schutzrichtlinie erforderlich (Vorschau)**

   - **Anfordern aller ausgewählten Kontrollen**   
 
     ![Bedingter Zugriff](./media/app-protection-based-conditional-access/13.png)



**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit Active Sync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/06.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. 

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/07.png)

4. **Bedingungen:** Als **Bedingungen** müssen Sie **Client-Apps (Vorschauversion)** konfigurieren. 

    Wählen Sie als **Client-Apps (Vorschauversion)** die Option **Mobile Apps und Desktop-Apps** und **ActiveSync-Clients austauschen**.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/92.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

   - **Markieren des Geräts als kompatibel erforderlich**

   - **Genehmigte Client-App erforderlich (Vorschau)**

   - **Anfordern aller ausgewählten Kontrollen**   
 
     ![Bedingter Zugriff](./media/app-protection-based-conditional-access/13.png)




**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).


## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>Auf App-Schutz basierende Richtlinie oder App-basierte Richtlinie für Exchange Online und SharePoint Online

Dieses Szenario besteht aus einer auf App-Schutz basierenden Richtlinie oder einer Richtlinie für genehmigte Apps für den Zugriff auf Exchange Online und SharePoint Online.


### <a name="scenario-playbook"></a>Szenario-Playbook

Annahmen für den Benutzer in diesem Szenario:

- Er konfiguriert die Clientanwendungen, die entweder in der Liste der Apps enthalten sind, die eine App-Schutzrichtlinie als Voraussetzung oder genehmigte Apps als Voraussetzung unterstützen.  
- Der Benutzer verwendet Clientanwendungen, die die Anforderung „App-Schutzrichtlinie erforderlich“ (App-Schutzrichtlinie als Voraussetzung) erfüllen und eine Intune App-Schutzrichtlinie erhalten können.
- Der Benutzer verwendet Clientanwendungen, die die Richtlinienanforderung für genehmigte Apps (genehmigte Client-App als Voraussetzung) erfüllen, die Intune App-Schutzrichtlinien unterstützt.
- Er öffnet die Anwendung zum Zugreifen auf E-Mails oder Dokumente.
- Er öffnet die Outlook-Anwendung und meldet sich mit den Azure AD-Anmeldeinformationen an.
- Er wird aufgefordert, entweder Authenticator (iOS) oder das Unternehmensportal (Android) zu installieren (sofern nicht installiert), um den Vorgang fortzusetzen.
- Er installiert die Anwendung und kann zur Outlook-App zurückkehren, um den Vorgang fortzusetzen.
- Er wird aufgefordert, ein Gerät zu registrieren.
- Er kann eine Intune-App-Schutzrichtlinie erhalten.
- Er kann mit Outlook und einer Intune-App-Schutzrichtlinie auf E-Mails zugreifen.
- Er kann mit einer App auf Websites/Dokumente zugreifen, die nicht in der Liste der Apps enthalten ist, für die eine App-Schutzrichtlinie als Voraussetzung gilt, sondern in der Liste der Apps enthalten ist, die genehmigte Apps als Voraussetzung unterstützen.

Intune-App-Schutzrichtlinien sind vor dem Zugriff auf die Unternehmensdaten erforderlich, und der Benutzer wird ggf. aufgefordert, die Anwendung neu zu starten, eine zusätzliche PIN zu verwenden oder eine andere Aktion auszuführen (sofern dies für die Anwendung und die Plattform konfiguriert wurde).

**Anmerkungen**

- Sie können dieses Szenario verwenden, wenn Sie Richtlinien für auf App-Schutz basierenden bedingten Zugriff und Richtlinien für App-basierten bedingten Zugriff unterstützen möchten.

- Bei dieser *ODER*-Richtlinie werden Apps, für die eine **App-Schutzrichtlinie als Voraussetzung** gilt, vor Apps für den Zugriff ausgewertet, für die eine **genehmigte Client-App als Voraussetzung** gilt.

### <a name="configuration"></a>Konfiguration

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt müssen folgende Komponenten konfiguriert werden:

![Bedingter Zugriff](./media/app-protection-based-conditional-access/62.png)

1. Der **Name** Ihrer Richtlinie zum bedingten Zugriff.

2. **Benutzer und Gruppen**: Für jede Richtlinie für bedingten Zugriff muss mindestens ein Benutzer oder eine Gruppe ausgewählt werden.

3. **Cloud-Apps:** Als Cloud-Apps müssen Sie **Office 365 Exchange Online** auswählen. 

     ![Bedingter Zugriff](./media/app-protection-based-conditional-access/02.png)

4. **Bedingungen:** Als **Bedingungen** müssen Sie **Geräteplattformen** und **Client-Apps** konfigurieren. 
 
    a. Wählen Sie als **Geräteplattformen** die Optionen **Android** und **iOS** aus.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/03.png)

    b. Wählen Sie als **Client-Apps (Vorschau)** die Option **Mobile Apps und Desktop-Apps** und **Clients mit moderner Authentifizierung**.

    ![Bedingter Zugriff](./media/app-protection-based-conditional-access/91.png)

5. Als **Zugriffskontrollen** muss Folgendes ausgewählt werden:

   - **Genehmigte Client-App erforderlich**

   - **App-Schutzrichtlinie erforderlich (Vorschau)**

   - **Eine der ausgewählten Steuerungen anfordern**   
 
     ![Bedingter Zugriff](./media/app-protection-based-conditional-access/12.png)


**Schritt 2: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**


![Bedingter Zugriff](./media/app-protection-based-conditional-access/09.png)

Weitere Informationen finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).




## <a name="next-steps"></a>Nächste Schritte

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, finden Sie Informationen unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md).

Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md) nach. 