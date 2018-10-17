---
title: Verwalten von Anwendungen mit Azure Active Directory | Microsoft Docs
description: Dieser Artikel erläutert die Vorteile der Integration von Azure Active Directory in lokale, Cloud- und SaaS-Anwendungen.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 09/11/2018
ms.author: barbkess
ms.reviewer: arvinh
ms.openlocfilehash: 820c6a5cb0bf5fb2d98d27c171c1276866082700
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716184"
---
# <a name="application-management-with-azure-active-directory"></a>Anwendungsverwaltung mit Azure Active Directory

Azure Active Directory (Azure AD) bietet einen sicheren und nahtlosen Zugriff auf die Cloud und lokale Anwendungen. Benutzer müssen sich nur einmal anmelden, um auf Office 365 und andere Geschäftsanwendungen von Microsoft, Tausende von Software-as-a-Service-Anwendungen (SaaS), lokale Anwendungen und branchenspezifische Apps zuzugreifen. Senken Sie Verwaltungskosten durch die Automatisierung der Benutzerbereitstellung. Verwenden Sie die mehrstufige Authentifizierung und Richtlinien für bedingten Zugriff, um sicheren Zugriff auf Anwendungen zu ermöglichen.

![Apps, die per Azure AD-Verbund verknüpft sind](media/migrate-adfs-apps-to-azure/migrate2.png)

## <a name="why-manage-applications-with-a-cloud-solution"></a>Gründe für die Verwaltung von Anwendungen mit einer Cloudlösung

Organisationen besitzen häufig Hunderte von Anwendungen, die für die Arbeit ihrer Benutzer unerlässlich sind. Der Benutzerzugriff auf diese Anwendungen erfolgt über eine Vielzahl von Geräten und Standorten. Tagtäglich werden neue Anwendungen hinzugefügt und entwickelt. Angesichts dieser Vielzahl an Anwendungen und Zugriffspunkte gewinnt die Verwaltung des Benutzerzugriffs auf alle Anwendungen über eine cloudbasierte Lösung zunehmend an Bedeutung.

## <a name="manage-risk-with-conditional-access-policies"></a>Risikomanagement durch Richtlinien für bedingten Zugriff
In Kombination mit der einmaligen Anmeldung (Single Sign-On, SSO) mit Azure AD bieten Richtlinien für bedingten Zugriff ein hohes Maß an Sicherheit für den Zugriff auf Anwendungen. Zu den Sicherheitsfunktionen zählen der cloudgestützte Identitätsschutz, die risikobasierte Zugriffssteuerung, die native mehrstufige Authentifizierung und Richtlinien für bedingten Zugriff. Durch diese Funktionen können präzise Steuerungsrichtlinien basierend auf Anwendungen oder Gruppen, die höheres Maß an Sicherheit zu benötigen, geschaffen werden.

## <a name="improve-productivity-with-single-sign-on"></a>Produktivitätssteigerung durch einmaliges Anmelden
Die Aktivierung der einmaligen Anmeldung (SSO) für verschiedenste Anwendungen und in Office 365 bietet eine optimierte Anmeldeoberfläche für bestehende Benutzer, da hierdurch die Anzahl der Anmeldeeingabeaufforderungen reduziert wird oder diese gänzlich beseitigt werden. Ohne die Vielzahl an Eingabeaufforderungen oder die Verwaltung mehrerer Kennwörter wirkt die Umgebung des Benutzers ganzheitlicher und weniger verwirrend. Die Unternehmensgruppe kann Zugriffe über Self-Service-Funktionen und dynamische Mitgliedschaften verwalten und genehmigen. Indem Sie den richtigen Personen im Unternehmen die Verwaltung des Zugriffs auf eine Anwendung ermöglichen, erhöhen Sie die Sicherheit des Identitätssystems.

SSO erhöht die Sicherheit. *Ohne einmaliges Anmelden* müssen Administratoren Benutzerkonten für jede einzelne Anwendung erstellen und aktualisieren, was viel Zeit in Anspruch nimmt. Darüber hinaus müssen Benutzer mehrere Sätze von Anmeldeinformationen für den Zugriff auf ihre Anwendungen nachverfolgen. Die Folge: Benutzer neigen dazu, ihre Kennwörter aufzuschreiben oder andere Lösungen zur Kennwortverwaltung zu verwenden, was zu Risiken für die Datensicherheit führt. 

## <a name="address-governance-and-compliance"></a>Beheben von Governance- und Complianceproblemen
Mit Azure AD können Sie Anmeldungen bei Anwendungen durch Berichte überwachen, die SIEM-Tools (Security Information & Event Monitoring) nutzen. Sie können über das Portal oder über APIs auf die Berichte zugreifen. Lassen Sie programmgesteuert überwachen, wer auf Ihre Anwendungen zugreifen darf, und entfernen Sie den Zugriff für inaktive Benutzer durch Zugriffsüberprüfungen.

## <a name="manage-costs"></a>Verwalten von Kosten
Durch eine Migration zu Azure AD können Sie Ihre Kosten senken und sich den Aufwand der Verwaltung Ihrer lokalen Infrastruktur ersparen. Azure AD bietet außerdem Self-Service-Zugriff auf Anwendungen, was sowohl Administratoren als auch Benutzern Zeit erspart. Durch das einmalige Anmelden entfallen anwendungsspezifische Kennwörter, was wiederum Kosten für das Zurücksetzen von Kennwörtern für Anwendungen spart, und Produktivitätsverluste durch das Abrufen von Kennwörtern werden vermieden.

