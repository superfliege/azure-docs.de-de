---
title: Untersuchen von riskanten Benutzern und Anmeldungen in Azure Active Directory Identity Protection (aktualisiert) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie riskante Benutzer und Anmeldungen in Azure Active Directory Identity Protection untersuchen (aktualisiert).
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.author: markvi
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.date: 01/25/2019
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.reviewer: raluthra
ms.openlocfilehash: f72819bacd7c65ced7ccff3665789d3b96c5e30f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478364"
---
# <a name="how-to-investigate-risky-users-and-sign-ins"></a>Anleitung: Überprüfen riskanter Benutzer und Anmeldungen 


Mit den Berichten „Riskante Anmeldungen“ und „Riskante Benutzer“ können Sie einen Einblick in die Risiken in Ihrer Umgebung gewinnen und genauere Untersuchungen anstellen. Durch die Möglichkeit, die riskanten Anmeldungen und Benutzer zu filtern und zu sortieren, können Sie potenzielle Angriffe auf Ihre Organisation besser verstehen. 


## <a name="risky-users-report"></a>Bericht „Riskante Benutzer“

Mit den Informationen im Bericht „Riskante Benutzer“ können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Welche Benutzer haben ein hohes Risiko?
- Bei welchen Benutzern wurde der Risikozustand bereinigt?



Der erste Einstiegspunkt für diesen Bericht ist der Abschnitt **Untersuchen** auf der Seite „Sicherheit“.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/01.png)


In der Standardansicht für den Bericht „Riskante Benutzer“ wird Folgendes anzeigt:

- Name

- Risikozustand

- Risikostufe

- Risikodetail

- Letzte Aktualisierung des Risikos

- Typ

- Status
 

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/03.png)


Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/04.png)

In den Spalten des Dialogfelds können Sie weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden alle dazu verfügbaren Details in einer horizontalen Ansicht angezeigt.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/05.png)


In der Detailansicht wird Folgendes angezeigt:

- Grundlegende Informationen

- Letzte riskante Anmeldungen

- Risikoereignisse ohne Bezug zu einer Anmeldung

- Risikoverlauf



Außerdem können Sie Folgendes tun:

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/08.png)

- Zeigen Sie alle Anmeldungsverknüpfungen an, um den Anmeldungsbericht für den jeweiligen Benutzer anzuzeigen.

- Zeigen Sie alle riskanten Anmeldungen an, um alle Anmeldungen für den Benutzer anzuzeigen, die als riskant gekennzeichnet wurden.

- Setzen Sie das Kennwort eines Benutzers zurück, wenn Sie der Meinung sind, dass die Identität des Benutzers gefährdet ist.

- Verwerfen Sie ein Benutzerrisiko, wenn Sie glauben, dass die aktiven Risikoereignisse eines Benutzers falsch-positive Meldungen sind. Weitere Informationen finden Sie unter [Verbessern der Erkennungsgenauigkeit](howto-improve-detection-accuracy.md).



### <a name="filter-risky-users"></a>Filtern von riskanten Benutzern

Um die gemeldeten Daten auf die von Ihnen gewünschte Stufe einzugrenzen, können Sie die Daten zu den riskanten Benutzern anhand der folgenden Standardfelder filtern:

- Name

- Benutzername

- Risikozustand

- Risikostufe

- Typ

- Status

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/06.png)



Für den Filter **Name** können Sie den Namen oder den Dienstprinzipalnamen (UPN) des gewünschten Benutzers angeben.


Für den Filter **Risikozustand** können Sie eine der folgenden Optionen auswählen:

- Gefährdet
- Bereinigt
- Verworfen


Für den Filter **Risikostufe** können Sie eine der folgenden Optionen auswählen:

- Hoch
- Mittel
- Niedrig


Für den Filter **Typ** können Sie eine der folgenden Optionen auswählen:

- Gast
- Mitglied

Für den Filter **Zustand** können Sie eine der folgenden Optionen auswählen:

- Gelöscht
- Aktiv


### <a name="download-risky-users-data"></a>Herunterladen von Daten zu riskanten Benutzern

Sie können die Daten zu riskanten Benutzern herunterladen, wenn Sie sie außerhalb des Azure-Portals verwenden möchten. Wenn Sie auf „Herunterladen“ klicken, wird eine CSV-Datei mit den letzten 5.000 Datensätzen erstellt. 

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/07.png)


Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf „Spalten“ klicken.
 
Sie können dann weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.
 
Wenn Sie weitere Informationen zu einem riskanten Benutzer haben möchten, klicken Sie zum Erweitern auf den Drawer „Details“.

 



## <a name="risky-sign-ins-report"></a>Bericht „Riskante Anmeldungen“

Mit den Informationen im Bericht „Riskante Anmeldungen“ können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Wie viele erfolgreiche Anmeldungen gab es in der letzten Woche, für die aufgrund einer anonymen IP-Adresse ein Risikoereignis gemeldet wurde?

- Bei welchen Benutzern wurde im letzten Monat eine Gefährdung bestätigt?

- Bei welchen Benutzern gab es riskante Anmeldungen im Office 365-Portal?




Der erste Einstiegspunkt für diesen Bericht ist der Abschnitt **Untersuchen** auf der Seite „Sicherheit“.

![Bericht „Riskante Anmeldungen“](./media/howto-investigate-risky-users-signins/02.png)

In der Standardansicht für den Bericht „Riskante Anmeldungen“ wird Folgendes anzeigt:

- Datum

- Benutzer

- Anwendung

- Anmeldestatus

- Risikozustand

- Risikostufe (aggregiert)

- Risikostufe (Echtzeit)

- Bedingter Zugriff

- MFA erforderlich  
 

![Bericht „Riskante Anmeldungen“](./media/howto-investigate-risky-users-signins/09.png)


Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/11.png)

In den Spalten des Dialogfelds können Sie weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden alle dazu verfügbaren Details in einer horizontalen Ansicht angezeigt.

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/12.png)


In der Detailansicht wird Folgendes angezeigt:

- Grundlegende Informationen

- Geräteinformationen

- Informationen zum Risiko

- MFA-Informationen

- Bedingter Zugriff





Außerdem können Sie Folgendes tun:

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/13.png)

- Gefährdung bestätigen 

- Sicherheit bestätigen

Weitere Informationen finden Sie unter [Verbessern der Erkennungsgenauigkeit](howto-improve-detection-accuracy.md).




### <a name="filter-risky-sign-ins"></a>Filtern von riskanten Anmeldungen

Um die gemeldeten Daten auf die von Ihnen gewünschte Stufe einzugrenzen, können Sie die Daten zu den riskanten Anmeldungen anhand der folgenden Standardfelder filtern:

- Benutzer
- Anwendung
- Anmeldestatus
- Risikozustand
- Risikostufe (aggregiert)
- Risikostufe (Echtzeit)
- Bedingter Zugriff
- Datum
- Art der Risikostufe

![Bericht „Riskante Anmeldungen“](./media/howto-investigate-risky-users-signins/14.png)



Für den Filter **Name** können Sie den Namen oder den Dienstprinzipalnamen (UPN) des gewünschten Benutzers angeben.

Für den Filter **Anwendung** können Sie die Cloud-App angeben, auf die der Benutzer zugreifen wollte.

Für den Filter **Anmeldestatus** können Sie eine der folgenden Optionen auswählen:

- Alle
- Erfolgreich
- Fehler


Für den Filter **Risikozustand** können Sie eine der folgenden Optionen auswählen:

- Gefährdet
- Als gefährdet bestätigt
- Als sicher bestätigt
- Verworfen
- Bereinigt


Für den Filter **Risikostufe (aggregiert)** können Sie eine der folgenden Optionen auswählen:

- Hoch
- Mittel
- Niedrig

Für den Filter **Risikostufe (Echtzeit)** können Sie eine der folgenden Optionen auswählen:

- Hoch
- Mittel
- Niedrig


Für den Filter **Bedingter Zugriff** können Sie eine der folgenden Optionen auswählen:

- Alle
- Nicht angewendet
- Erfolgreich
- Fehler


Mit dem Filter **Datum** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.
Mögliche Werte:

- Letzter Monat
- Letzte 7 Tage
- Letzte 24 Stunden
- Benutzerdefiniertes Zeitintervall





### <a name="download-risky-sign-ins-data"></a>Herunterladen von Daten zu riskanten Anmeldungen

Sie können die Daten zu riskanten Anmeldungen herunterladen, wenn Sie sie außerhalb des Azure-Portals verwenden möchten. Wenn Sie auf „Herunterladen“ klicken, wird eine CSV-Datei mit den letzten 5.000 Datensätzen erstellt. 

![Bericht „Riskante Benutzer“](./media/howto-investigate-risky-users-signins/15.png)



## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure AD Identity Protection finden Sie [in diesem Artikel](overview-v2.md).
