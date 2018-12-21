---
title: Was ist ein Identity Secure Score? – Azure Active Directory (öffentliche Vorschauversion) | Microsoft-Dokumentation
description: Enthält eine Anleitung dazu, wie Sie den Identity Secure Score verwenden können, um den Sicherheitsstatus Ihres Azure AD-Mandanten zu verbessern.
services: active-directory
keywords: Identity Secure Score, Azure AD, sicherer Zugriff auf Unternehmensressourcen
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: fundamentals
ms.topic: overview
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2018
ms.author: markvi
ms.reviewer: nigu
ms.custom: it-pro
ms.openlocfilehash: 5a133d0a38852ca3e95e3ae07716c4fbd3c87f5d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53092067"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory-public-preview"></a>Was ist der Identity Secure Score in Azure Active Directory? (Öffentliche Vorschauversion)

Wie sicher ist Ihr Azure AD-Mandant? Wenn Sie diese Frage nicht beantworten können, lesen Sie diesen Artikel, um zu erfahren, wie Sie den Status Ihrer Identitätssicherheit mit dem Identity Secure Score überwachen und verbessern können. 

## <a name="what-is-an-identity-secure-score"></a>Was ist ein Identity Secure Score?

Der Identity Secure Score ist eine Zahl zwischen 1 und 248, die Aufschluss darüber gibt, in welchem Maße Sie die von Microsoft empfohlenen bewährten Sicherheitsmethoden befolgen.


![Secure Score](./media/identity-secure-score/01.png)



Anhand der Bewertung können Sie:

- Den Status Ihrer Identitätssicherheit objektiv messen

- Verbesserungen der Identitätssicherheit planen

- Den Erfolg Ihrer Verbesserungen überprüfen 


Sie können im Identity Secure Score-Dashboard auf die Bewertung und zugehörige Informationen zugreifen. Im Dashboard finden Sie folgende Informationen:

- Ihre Bewertung

    ![Secure Score](./media/identity-secure-score/02.png)

- Ein Vergleichsdiagramm

    ![Secure Score](./media/identity-secure-score/03.png)

- Ein Trenddiagramm

    ![Secure Score](./media/identity-secure-score/04.png)

- Eine Liste der bewährten Methoden für die Identitätssicherheit 

    ![Secure Score](./media/identity-secure-score/05.png)


Mithilfe der vorgeschlagenen Verbesserungsaktionen können Sie:

- Ihren Sicherheitsstatus und Ihre Bewertung verbessern
 
- Die Identitätsfunktionen von Microsoft nutzen 



## <a name="how-do-i-get-my-secure-score"></a>Wie erhalte ich meinen Secure Score?

Der Identity Secure Score ist in allen Editionen von Azure AD verfügbar.

Besuchen Sie das [Azure AD-Übersichtsdashboard](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore), um auf Ihre Bewertung zuzugreifen.



## <a name="how-does-it-work"></a>Wie funktioniert dies?

Azure führt alle 48 Stunden eine Untersuchung Ihrer Sicherheitskonfiguration durch, bei der Ihre Einstellungen mit den empfohlenen bewährten Methoden verglichen werden. Basierend auf dem Ergebnis dieser Untersuchung wird eine neue Bewertung für Ihren Mandanten berechnet. Dies bedeutet, dass es bis zu 48 Stunden dauern kann, bis eine von Ihnen vorgenommene Konfigurationsänderung in die Bewertung einfließt. 

Jede Empfehlung wird basierend auf Ihrer Azure AD-Konfiguration bewertet. Wenn Sie Drittanbieterprodukte verwenden, um eine Empfehlung gemäß den bewährten Methoden umzusetzen, können Sie dies in den Einstellungen einer Verbesserungsaktion angeben.

![Secure Score](./media/identity-secure-score/07.png)


Zudem können Sie festlegen, dass Empfehlungen ignoriert werden sollen, wenn sie nicht auf Ihre Umgebung zutreffen. Eine ignorierte Empfehlung wird bei der Berechnung Ihrer Bewertung nicht berücksichtigt. 
 
![Secure Score](./media/identity-secure-score/06.png)



## <a name="how-does-it-help-me"></a>Wie kann mir der Secure Score helfen?

Anhand des Secure Score können Sie:

- Den Status Ihrer Identitätssicherheit objektiv messen

- Verbesserungen der Identitätssicherheit planen

- Den Erfolg Ihrer Verbesserungen überprüfen



## <a name="what-you-should-know"></a>Wichtige Informationen

### <a name="who-can-use-the-identity-secure-score"></a>Wer kann den Identity Secure Score nutzen?

Der Identity Secure Score kann von Benutzern mit den folgenden Rollen verwendet werden:

- Globaler Administrator
- Sicherheitsadministrator 
- Benutzer mit Leseberechtigung für Sicherheitsfunktionen 

### <a name="what-does-not-scored-mean"></a>Was bedeutet der Hinweis „[Not Scored]“ (Nicht bewertet)?

Aktionen mit der Bezeichnung „[Not Scored]“ (Nicht bewertet) sind Aktionen, die Sie in Ihrer Organisation durchführen können, die aber nicht bewertet werden, weil sie (noch) nicht in das Tool integriert sind. Sie können die Sicherheit mit diesen Aktionen verbessern, sie werden gegenwärtig jedoch nicht in der Bewertung berücksichtigt.

### <a name="how-often-is-my-score-updated"></a>Wie oft wird meine Bewertung aktualisiert?

Die Bewertung wird einmal am Tag (ungefähr um 1:00 Uhr Pacific Normalzeit) berechnet. Wenn Sie eine Änderung an einer bewerteten Aktion vornehmen, wird die Bewertung automatisch am nächsten Tag aktualisiert. Es dauert bis zu 48 Stunden, bis eine Änderung in Ihrer Bewertung berücksichtigt wird.


### <a name="my-score-changed-how-do-i-figure-out-why"></a>Meine Bewertung hat sich geändert. Wie ermittle ich den Grund für diese Änderung?

Klicken Sie im [Secure Score-Portal](https://securescore.microsoft.com/#!/score) auf der Analyseseite der Bewertung auf einen Datenpunkt für einen bestimmten Tag, und scrollen Sie dann nach unten, um die abgeschlossenen und nicht abgeschlossenen Aktionen für den Tag anzuzeigen. So können Sie herausfinden, was sich geändert hat.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>Misst der Secure Score mein Risiko einer Sicherheitsverletzung?

Kurz gesagt: Nein. Der Secure Score ist kein absolutes Maß dafür, wie hoch die Wahrscheinlichkeit einer Sicherheitsverletzung für Sie ist. Er gibt wieder, in welchem Umfang Sie die Funktionen nutzen, mit denen das Risiko einer Sicherheitsverletzung beeinflusst werden kann. Kein Dienst kann garantieren, dass Sie nicht das Opfer einer Sicherheitsverletzung werden, und der Secure Score sollte nicht als eine solche Garantie aufgefasst werden.

### <a name="how-should-i-interpret-my-score"></a>Wie interpretiere ich meine Bewertung?

Sie erhalten Hinweise zur Konfiguration empfohlener Sicherheitsfunktionen oder Durchführung sicherheitsrelevanter Aufgaben (z. B. das Lesen von Berichten). Einige Aktionen werden bewertet, wenn sie teilweise abgeschlossen wurden, beispielsweise das Aktivieren von Multi-Factor Authentication (MFA) für Ihre Benutzer. Ihr Secure Score spiegelt die von Ihnen verwendeten Microsoft-Sicherheitsdienste direkt wider. Denken Sie daran, dass immer ein Gleichgewicht zwischen der Sicherheit und Benutzerfreundlichkeit bestehen sollte. Alle Sicherheitsmaßnahmen wirken sich auf Benutzer aus. Kontrollen mit geringen Benutzerauswirkungen sollten wenig oder gar keinen Einfluss auf das Tagesgeschäft Ihrer Benutzer haben.

Ihren Bewertungsverlauf finden Sie auf der Analyseseite der Bewertung im [Secure Score-Portal](https://securescore.microsoft.com/#!/score). Wählen Sie ein Datum aus, um anzuzeigen, welche Kontrollen für diesen Tag aktiviert waren und welche Bewertung Sie für die einzelnen Kontrollen erhalten haben.


### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>In welcher Beziehung steht der Identity Secure Score zum Office 365 Secure Score? 

Der [Office 365 Secure Score](https://docs.microsoft.com/office365/securitycompliance/office-365-secure-score) wird demnächst auf ein Aggregat von fünf verschiedenen Bewertungen umgestellt:

- Identity

- Daten

- Geräte

- Infrastruktur

- Apps

Der Identity Secure Score stellt den Teil des Office 365 Secure Score dar, der die Sicherheit von Identitäten betrifft. Dies bedeutet, dass Ihre Empfehlungen für den Identity Secure Score und die Identitätsbewertung in Office 365 identisch sind. 


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie ein Video zum Office 365 Secure Score ansehen möchten, klicken Sie [hier](https://www.youtube.com/watch?v=jzfpDJ9Kg-A).
 
