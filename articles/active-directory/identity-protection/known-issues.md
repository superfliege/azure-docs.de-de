---
title: FAQs und bekannte Probleme mit Identity Protection (aktualisiert) in Azure Active Directory | Microsoft-Dokumentation
description: FAQs und bekannte Probleme mit Identity Protection (aktualisiert) in Azure Active Directory
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2019
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 912326bb1e5420fd4c9051f5a71aa98e6cff80e5
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55470381"
---
# <a name="faqs-and-known-issues-with-identity-protection-refreshed-in-azure-active-directory"></a>FAQs und bekannte Probleme mit Identity Protection (aktualisiert) in Azure Active Directory


## <a name="dismiss-user-risk"></a>Ignorieren eines Benutzerrisikos

Über **Benutzerrisiko ignorieren** in Identity Protection (klassisch) wird der Akteur im Risikoverlauf des Benutzers in Identity Protection (aktualisiert) auf **Azure AD** festgelegt.


Über **Benutzerrisiko ignorieren** in Identity Protection (aktualisiert) wird der Akteur im Risikoverlauf des Benutzers in Identity Protection (aktualisiert) auf **\<Administratorname mit Link zum Blatt des Benutzers\>** festgelegt.


## <a name="risky-users-report"></a>Bericht zu riskanten Benutzern

Bei Abfragen für das Feld **Benutzername** wird die Groß-/Kleinschreibung beachtet, bei Abfragen für das Feld **Name** hingegen nicht.

Durch Umschalten von **Datum anzeigen als** wird die Spalte **LETZTE AKTUALISIERUNG DES RISIKOS** ausgeblendet. Um die Spalte zu lesen, klicken Sie oben auf dem Blatt „Riskante Benutzer“ auf **Spalten**.

Über **Alle Ereignisse schließen** im klassischen Identity Protection wird der Status der Risikoereignisse auf **Geschlossen (gelöst)** festgelegt.

Wenn Sie versuchen, auf den Bericht für riskante Benutzer zuzugreifen, indem Sie innerhalb eines Anmeldedatensatzes im Bericht zu riskanten Anmeldungen auf **Bericht zu riskanten Benutzern** klicken, kann es gelegentlich vorkommen, dass folgender Fehler angezeigt wird: **Da hat etwas nicht geklappt. Versuchen Sie es noch mal.** Um das Problem zu beheben, klicken Sie oben auf dem Bildschirm auf **Anwenden** oder **Zurücksetzen**, um die Daten zu riskanten Benutzern aufzufüllen.


## <a name="risky-sign-ins-report"></a>Bericht über riskante Anmeldungen

Bei einem Risikoereignis führt **Auflösen** dazu, dass der Status auf **Der Benutzer hat die durch eine risikobasierte Richtlinie ausgelöste MFA erfolgreich durchgeführt** festgelegt wird.

Über **Zurücksetze** im Bericht **Riskante Anmeldungen** wird der Wert für **Risikoereignistyp** nicht gelöscht.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-event"></a>Warum kann ich meine eigenen Risikostufen für die einzelnen Risikoereignisse nicht anzeigen?

Die Risikostufen in Identity Protection basieren auf der Genauigkeit der Erkennung und werden durch unser überwachtes Machine Learning-System unterstützt. Um festzulegen, welche Werte den Benutzern angezeigt werden, kann der Administrator bestimmte Benutzer/Gruppen in die Richtlinien für Benutzer- und Anmelderisiken einschließen oder sie davon ausschließen.


### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>Warum stimmt der Standort für eine Anmeldung nicht mit dem tatsächlichen Standort überein, von dem aus sich der Benutzer angemeldet hat?

Die IP-Zuordnung bei der Geolocation stellt branchenweit eine Herausforderung dar. Wenn der im Anmeldebericht aufgeführte Standort nicht mit dem tatsächlichen Standort übereinstimmt, wenden Sie sich an den Support. 


### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>Wie funktionieren die Feedbackmechanismen in Identity Protection?

**Gefährdung bestätigen** (bei Anmeldung): Informiert Azure AD Identity Protection darüber, dass die Anmeldung nicht vom Identitätsinhaber durchgeführt wurde und weist auf eine Gefährdung hin.

- Nach Erhalt dieses Feedbacks wird der Anmelde- und Benutzerrisikostatus auf **Als gefährdet bestätigt** und die Risikostufe auf **Hoch** festgelegt.

- Darüber hinaus geben wir die Informationen an unsere Machine Learning-Systeme weiter, um die künftige Risikobewertung zu verbessern.

    > [!NOTE]
    > Wenn für den Benutzer bereits eine Bereinigung durchgeführt wurde, klicken Sie nicht auf **Gefährdung bestätigen**, weil dadurch der Anmelde- und Benutzerrisikostatus auf **Als gefährdet bestätigt** und die Risikostufe auf **Hoch** festgelegt wird.

**Sicherheit bestätigen** (bei Anmeldung): Informiert Azure AD Identity Protection darüber, dass die Anmeldung vom Identitätsinhaber durchgeführt wurde und keine Gefährdung vorliegt.

- Nach Erhalt dieses Feedbacks wird der Anmelde- und Benutzerrisikostatus auf **Als sicher bestätigt** und die Risikostufe auf **-** festgelegt.

- Darüber hinaus geben wir die Informationen an unsere Machine Learning-Systeme weiter, um die künftige Risikobewertung zu verbessern.

    > [!NOTE]
    > Wenn Sie davon ausgehen, dass der Benutzer nicht gefährdet ist, verwenden Sie anstelle von **Als sicher bestätigt** auf Anmeldeebene die Option **Benutzerrisiko ignorieren** auf Benutzerebene. Durch **Benutzerrisiko ignorieren** auf Benutzerebene werden das Benutzerrisiko und alle vergangenen riskanten Anmeldungen und Risikoereignisse geschlossen.



### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-events-are-shown-in-identity-protection"></a>Warum sehe ich einen Benutzer mit einer niedrigen (oder höheren) Risikobewertung, auch wenn in Identity Protection keine riskanten Anmeldungen oder Risikoereignisse angezeigt werden?

Da das Benutzerrisiko naturgemäß kumulativ zunimmt und nicht abläuft, kann für einen Benutzer selbst dann ein niedriges oder höheres Benutzerrisiko vorliegen, wenn keine aktuellen riskanten Anmeldungen oder Risikoereignisse in Identity Protection angezeigt werden. Dies könnte der Fall sein, wenn die einzige schädliche Aktivität für einen Benutzer außerhalb des Zeitraums stattgefunden hat, für den Details zu riskanten Anmeldungen und Risikoereignissen gespeichert werden. Es gibt keine Ablaufzeiträume für das Benutzerrisiko, weil böswillige Akteure nach der Kompromittierung einer Identität mitunter mehr als 140 Tage in der Kundenumgebung verbleiben, bevor sie ihren Angriff starten. Hier können Kunden die Risikozeitachse für einen Benutzer überprüfen, um die Gründe für ein Benutzerrisiko zu verstehen: `Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>Warum liegt die Bewertung „Hoch“ für „Anmelderisiko (aggregiert)“ vor, wenn die zugehörigen Erkennungen ein niedriges oder mittleres Risiko aufweisen?

Die hohe aggregierte Risikobewertung könnte auf anderen Faktoren der Anmeldung oder darauf basieren, dass für diese Anmeldung mehrere Erkennungen ausgelöst wurden. Umgekehrt kann die aggregierte Risikobewertung „Mittel“ vorliegen, obwohl die mit der Anmeldung verknüpften Erkennungen ein hohes Risiko aufweisen. 
