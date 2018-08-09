---
title: Erstellen einer Zugriffsüberprüfung für Mitglieder einer Gruppe oder Benutzer mit Zugriff auf eine Anwendung mit Azure AD | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Zugriffsüberprüfung für Mitglieder einer Gruppe oder Benutzer mit Zugriff auf eine Anwendung erstellen.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 24df484a571e838eeaa9bd86ef69a3dd10843843
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620959"
---
# <a name="create-an-access-review-of-group-members-or-application-access-with-azure-ad"></a>Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD

Zugriffszuweisungen sind „veraltet“, wenn Benutzer über Zugriff verfügen, den sie nicht mehr benötigen. Um das mit veralteten Zugriffszuweisungen verbundene Risiko zu reduzieren, können Administratoren mit Azure Active Directory (Azure AD) eine Zugriffsüberprüfung erstellen. Diese kann für Mitglieder einer Gruppe oder Benutzer, die einer Anwendung zugewiesen sind, durchgeführt werden. Das Erstellen periodischer Zugriffsüberprüfungen kann Zeit sparen. Wenn Sie Benutzer, die Zugriff auf eine Anwendung haben oder Mitglied einer Gruppe sind, routinemäßig überprüfen, können Sie die Häufigkeit dieser Überprüfungen definieren. Weitere Informationen zu diesen Szenarien finden Sie unter [Verwalten des Benutzerzugriffs mit Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md) und [Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). 

## <a name="create-an-access-review"></a>Erstellen einer Zugriffsüberprüfung

1. Navigieren Sie als globaler Administrator oder Benutzerkontoadministrator zur Seite [Zugriffsüberprüfungen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), und klicken Sie auf **Programme**.

2. Wählen Sie das Programm, das das Zugriffsüberprüfungs-Steuerelement enthält, das Sie erstellen möchten. Das **Standardprogramm** ist immer vorhanden. Sie können aber auch ein anderes Programm erstellen. Sie können z.B. ein Programm für jede Konformitätsinitiative oder für jedes Geschäftsziel verwenden.

3. Wählen Sie innerhalb des Programms **Steuerelemente**, und wählen Sie dann **Hinzufügen**, um ein Steuerelement hinzuzufügen.

4. Benennen Sie die Zugriffsüberprüfung. Wahlweise können Sie jeder Überprüfung eine Beschreibung hinzufügen. Den Prüfern werden Name und Beschreibung angezeigt.

5. Legen Sie das Startdatum fest. Standardmäßig findet eine Zugriffsüberprüfung einmalig statt. Sie beginnt an dem Tag, an dem sie erstellt wird, und endet nach einem Monat. Sie können Start- und Enddatum so ändern, dass der Start der Zugriffsüberprüfung in der Zukunft liegt und sie so lange dauert, wie Sie wünschen.

6. Um die Zugriffsüberprüfung wiederholt auszuführen, ändern Sie die Häufigkeit von „Einmal“ in „Wöchentlich“, „Monatlich“, „Vierteljährlich“ oder „Jährlich“. Definieren Sie mithilfe des Schiebereglers oder im Textfeld die Anzahl der Tage, an denen jeweils Eingaben der Prüfer in den einzelnen periodischen Überprüfungen in der Serie möglich sind. Die maximale Dauer, die Sie beispielsweise für eine monatliche Überprüfung festlegen können, beträgt 27 Tage, um überlappende Überprüfungen zu vermeiden. 

7.  Die Serienzugriffsüberprüfungen können auf dreierlei Weise enden: Die Serie wird unendlich ausgeführt, um Überprüfungen auf unbestimmte Zeit zu starten, die Serie wird bis zu einem bestimmten Datum ausgeführt, oder sie wird nach einer bestimmten Anzahl von Vorkommen beendet. Sie (oder ein anderer Benutzerkontoadministrator oder globaler Administrator) können die Serie nach der Erstellung beenden, indem Sie in den Einstellungen das Datum ändern, sodass die Serie an diesem Datum beendet wird.

8. Zugriffsüberprüfungen können für die Mitglieder einer Gruppe oder für Benutzer erfolgen, die einer Anwendung zugewiesen wurden. Sie können die Zugriffsüberprüfung weiter anpassen, um nur die Gastbenutzer zu überprüfen, die Mitglieder (oder der App zugewiesen) sind, anstatt alle Benutzer zu überprüfen, die Mitglieder sind oder Zugriff auf die Anwendung haben.

9. Wählen Sie entweder eine oder mehrere Personen aus, um alle Benutzer im Bereich zu überprüfen. Alternativ können Sie auswählen, dass die Mitglieder ihren eigenen Zugriff überprüfen. Wenn es sich bei der Ressource um eine Gruppe handelt, können Sie die Gruppenbesitzer um die Überprüfung bitten. Sie können auch festlegen, dass die Prüfer einen Grund angeben müssen, wenn sie den Zugriff genehmigen.

10. Wenn Sie nach Abschluss der Überprüfung die Ergebnisse manuell anwenden möchten, klicken Sie auf **Start**.  Andernfalls wird im nächsten Abschnitt erläutert, wie Sie die Überprüfung so konfigurieren, dass sie automatisch angewendet wird.

### <a name="configuring-an-access-review-with-auto-apply"></a>Konfigurieren einer Zugriffsüberprüfung mit automatischer Anwendung

1.  Erweitern Sie das Menü „Einstellungen nach Abschluss“, und aktivieren Sie „Ergebnisse automatisch auf Ressource anwenden“. 

2.  Wenn die Benutzer innerhalb des Überprüfungszeitraums nicht vom Prüfer überprüft wurden, können Sie auswählen, ob für die Zugriffsüberprüfung die Systemempfehlung übernommen wird (sofern aktiviert), der weitere Zugriff des Benutzers verweigert/genehmigt wird, der Zugriff unverändert erhalten bleibt oder der Zugriff entfernt wird. Dies wirkt sich nicht auf die Benutzer aus, die von den Prüfern manuell überprüft wurden: Wenn die Entscheidung des Prüfers letztlich „Verweigern“ lautet, wird der Zugriff des Benutzers entfernt.

3.  Um bei Nichtreagieren des Prüfers die Empfehlungen zu übernehmen, erweitern Sie „Erweiterte Einstellungen“, und aktivieren Sie die Option „Empfehlungen anzeigen“.
 
4.  Klicken Sie abschließend auf **Start** (Starten).

Anhand Ihrer Auswahl unter „Einstellungen nach Abschluss“ wird nach dem Enddatum der Überprüfung oder bei manueller Beendigung der Überprüfung die automatische Anwendung ausgeführt. Der Status der Überprüfung ändert sich von „Abgeschlossen“ über Zwischenzustände wie „Wird angewandt“ bis zum Status „Angewandt“. Erwartungsgemäß sollten abgelehnte Benutzer (sofern vorhanden) innerhalb weniger Minuten aus der Gruppenmitgliedschaft oder der App-Zuweisung entfernt werden.


## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung

Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer. Wenn Sie nicht möchten, dass Azure AD die E-Mail sendet, stellen Sie sicher, dass die Prüfer darüber in Kenntnis gesetzt werden, dass sie eine ausstehende Zugriffsüberprüfung abschließen müssen. Sie können ihnen die Anweisungen zum [Überprüfen des Zugriffs mit der Azure AD-Zugriffsüberprüfung](active-directory-azure-ad-controls-perform-access-review.md) anzeigen. Falls Ihre Überprüfung für Gäste gedacht ist, die ihren eigenen Zugriff überprüfen sollen, können Sie ihnen die Anweisungen zum [Überprüfen des eigenen Zugriffs mit der Azure AD-Zugriffsüberprüfung](active-directory-azure-ad-controls-perform-access-review.md) anzeigen.

Wenn es sich bei einigen Prüfern um Gäste handelt: Gäste werden nur dann per E-Mail benachrichtigt, wenn sie die Einladung bereits angenommen haben.

Um eine Serie von Zugriffsüberprüfungen zu verwalten, navigieren Sie unter **Steuerelemente** zu der Zugriffsüberprüfung. Unter „Geplante Überprüfung“ finden Sie anstehende Überprüfungen. Hier können Sie das Enddatum bearbeiten oder Prüfer entsprechend hinzufügen/entfernen. 

Sie können den Fortschritt der Überprüfungen durch die Prüfer im Azure AD-Dashboard im Abschnitt **Zugriffsüberprüfungen** nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](active-directory-azure-ad-controls-complete-access-review.md) ist.

## <a name="next-steps"></a>Nächste Schritte

Wenn eine Zugriffsüberprüfung gestartet wurde, sendet Azure AD automatisch eine E-Mail an die Prüfer, in der diese zur Überprüfung des Zugriffs aufgefordert werden. Benutzern, die diese E-Mail nicht erhalten haben, können Sie die Anweisungen unter [Überprüfen des Zugriffs mit der Azure AD-Zugriffsüberprüfung](active-directory-azure-ad-controls-perform-access-review.md) senden. 

Einmalige Überprüfung: Führen Sie nach Ablauf des Zugriffsüberprüfungszeitraums oder nach Anhalten der Zugriffsüberprüfung durch den Administrator die Schritte unter [Abschließen einer Zugriffsüberprüfung](active-directory-azure-ad-controls-complete-access-review.md) aus, um die Ergebnisse anzuzeigen und anzuwenden.  

Überprüfungsserie: Navigieren Sie auf der Seite mit der Zugriffsüberprüfungsserie zu **Verlauf überprüfen**, um eine abgeschlossene Zugriffsüberprüfung auszuwählen.  Anstehende Überprüfungen werden unter **Geplante Überprüfung** aufgelistet. Hier können Sie die Dauer bearbeiten sowie Prüfer für einzelne Überprüfungen hinzufügen oder entfernen.
