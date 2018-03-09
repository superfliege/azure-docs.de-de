---
title: "Sicherheitsbericht „Benutzer mit Risikomarkierung“ im Azure Active Directory-Portal | Microsoft-Dokumentation"
description: "Hier erfahren Sie mehr über den Sicherheitsbericht „Benutzer mit Risikomarkierung“ im Azure Active Directory-Portal."
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ed6201e9edcef39b14b948b6b2f6e0b5da01ec60
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Behandeln von „Benutzer mit Risikomarkierung“ im Azure Active Directory-Portal

Mit den Sicherheitsberichten in Azure Active Directory (Azure AD) erhalten Sie Einblicke in die Wahrscheinlichkeit für kompromittierte Benutzerkonten in Ihrer Umgebung. Ein [Benutzer mit Risikomarkierung](active-directory-identityprotection.md#users-flagged-for-risk) ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto.

Microsoft ist bestrebt, Sie beim Schutz Ihrer Umgebungen zu unterstützen. Im Rahmen dieses Engagements sucht Microsoft kontinuierlich nach ungewöhnlichen Aktivitäten sowie nach Aktivitäten, die bekannten Angriffsmustern entsprechen. 


Werden ungewöhnliche Aktivitäten erkannt, die auf nicht autorisierte Zugriffe auf einige Konten Ihrer Benutzer hindeuten, erhalten Sie Benachrichtigungen, um darauf reagieren zu können. Die Bereitstellung von Benachrichtigungen bedeutet nicht, dass Microsoft-eigene Systeme in irgendeiner Weise kompromittiert wurden.
 

## <a name="azure-active-directory-report-access"></a>Zugriff auf den Azure Active Directory-Bericht

Benutzer mit Risikomarkierung können über einen Azure Active Directory-Onlinebericht überprüft werden. Sollten Sie über kein Azure-Abonnement verfügen, können Sie unter [http://aka.ms/AccessAAD](http://aka.ms/AccessAAD) ein kostenloses Abonnement erstellen.  
Anschließend können Sie unter Verwendung Ihrer Office 365-Anmeldeinformationen auf das Azure Admin Center zugreifen. Hinweis: Auf der Abonnementebene „Basic“ stehen nur eingeschränkte Details zur Verfügung. Azure Premium-Abonnementen haben dagegen Zugang zu weiteren Daten und Analysen. Weitere Informationen finden Sie unter [Sicherheitsbericht „Benutzer mit Risikomarkierung“ im Azure Active Directory-Portal](active-directory-reporting-security-user-at-risk.md).

Nachdem Sie Ihren Zugriff auf Azure AD aktiviert haben, werden Sie zum [Azure AD-Portal](https://portal.azure.com) umgeleitet. Unter der folgenden URL gelangen Sie direkt zum Bericht: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk).

**So greifen Sie im Office 365 Admin Center auf den Bericht „Benutzer mit Risikomarkierung“ zu:**

1.  Klicken Sie im Navigationsbereich auf der linken Seite auf **Admin Center**. 
2.  Klicken Sie auf **Azure AD**.
3.  Melden Sie sich beim **Azure Active Directory Admin Center** an.
4.  Falls am oberen Seitenrand ein Banner mit dem Text **Check out the new portal** (Neues Portal ausprobieren) erscheint, klicken Sie auf den Link.
4.  Klicken Sie im Navigationsmenü auf der linken Seite auf **Azure Active Directory**. 
5.  Klicken Sie im Navigationsbereich unter **Sicherheit** auf **Benutzer mit Risikomarkierung**.

Sehen Sie sich die angezeigten Informationen an. Es empfiehlt sich, die Kennwörter aller hier aufgeführten Konten zurückzusetzen. 

## <a name="remediation-actions"></a>Aktionen zur Problembehebung

Führen Sie die folgenden Schritte aus, um die betroffenen Konten zu korrigieren und Ihre Umgebung zu schützen:

1.  [Überprüfen](http://aka.ms/MFAValid) Sie die Informationen für die mehrstufige Authentifizierung und die Self-Service-Kennwortzurücksetzung. 
2.  [Aktivieren](http://aka.ms/MFAuth) Sie die mehrstufige Authentifizierung für alle Benutzer. 
3.  Mit [diesem Bereinigungsskript](http://aka.ms/remediate) können Sie für jedes betroffene Konto automatisch folgende Schritte ausführen: 

    a. Zurücksetzen des Kennworts, um das Konto zu schützen und die Beendigung aktiver Sitzungen zu erzwingen

    b. Entfernen von Postfachstellvertretungen

    c. Deaktivieren von Regeln zur Weiterleitung von E-Mails an externe Domänen

    d. Entfernen der globalen E-Mail-Weiterleitungseigenschaft für das Postfach

    e. Aktivieren der mehrstufigen Authentifizierung für das Konto des Benutzers

    f. Festlegen einer hohen Kennwortkomplexität für das Konto

    g. Aktivieren der Postfachüberwachung

    h. Erzeugen eines Überwachungsprotokolls zur Überprüfung durch den Administrator

4. Untersuchen Sie Ihren Office 365-Mandanten sowie die restliche IT-Infrastruktur – einschließlich aller Mandanteneinstellungen, Benutzerkonten und der benutzerspezifischen Konfigurationseinstellungen – auf mögliche Änderungen. Suchen Sie nach Anzeichen für Persistenzmethoden sowie nach Anzeichen dafür, dass ein Eindringling an VPN-Anmeldeinformationen gelangt ist oder sich Zugriff auf andere Ressourcen der Organisation verschafft hat. 

5.  Überlegen Sie sich im Rahmen Ihrer Untersuchung, ob Sie ggf. die Behörden einschalten sollten oder müssen.

Darüber hinaus empfiehlt sich Folgendes:

- Lesen und implementieren Sie diesen [Leitfaden](http://aka.ms/fixaccount) für den Umgang mit ungewöhnlichen Aktivitäten. 
- [Aktivieren Sie die Überwachungspipeline](http://aka.ms/improvesecurity), um die Analyse der Aktivitäten Ihres Mandanten zu vereinfachen. Nach Abschluss des Vorgangs wird Ihr Überwachungsspeicher mit sämtlichen Aktivitätsprotokollen gefüllt. Nun können Sie auch die [Such- und Untersuchungsfunktionen des Security & Compliance Centers](http://aka.ms/sccsearch) nutzen. 
- Verwenden Sie [dieses Skript](http://aka.ms/mailboxaudit1), um die Postfachüberwachung für alle Ihre Konten zu aktivieren. 
- Überprüfen Sie die Berechtigungen der Stellvertretung und die E-Mail-Weiterleitungsregeln für alle Ihre Postfächer. Hierzu können Sie [dieses PowerShell-Skript](http://aka.ms/delegateforwardrules) verwenden. 



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Active Directory Identity Protection finden Sie unter [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

