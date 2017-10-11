---
title: Verwalten des Zugriffs auf apps mithilfe von Azure AD | Microsoft Docs
description: Beschreibt, wie Azure Active Directory sind Organisationen in die apps anzugeben, auf die einzelnen Benutzer Zugriff hat.
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: b0829f18-9e57-4107-925d-5f0457d81671
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 46e001b440802e0d5d16b7cf75344c7b9ce6fad3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-access-to-apps"></a>Verwalten des Zugriffs auf apps
Ständigen Zugriff auf Verwaltung, Verwendung Auswertung und reporting weiterhin eine Herausforderung darstellen, nachdem eine Anwendung in Ihrer Organisation Identitätssystem integriert wird. In vielen Fällen müssen IT-Administratoren oder dem Helpdesk eine laufende aktive Rolle Verwalten des Zugriffs auf Ihre apps übernehmen. In manchen Fällen erfolgt die Zuweisung von einem Allgemein "oder" bereichsadministratoren IT-Team. Häufig dient die Entscheidung für die Zuweisung an Entscheidungsträger, deren Genehmigung vor der IT-macht delegiert werden die Zuweisung.  Andere Organisationen investieren Sie in der Integration mit einem vorhandenen automatisierten Identitäts- und Zugriffs-System, wie mit der rollenbasierten Zugriffssteuerung (RBAC) oder Attribut-Based Access Control (ABAC). Die Integration und die Regel Entwicklung tendenziell spezialisierte und teuer. Überwachung oder Berichterstattung entweder Managementansatz ist einen eigenen separaten kostspielige und komplexe Investitionen.

## <a name="how-does-azure-active-directory-help"></a>So hilft Azure Active Directory?
 Azure AD unterstützt eine umfangreiche zugriffsverwaltung für konfigurierten Anwendungen ermöglicht Organisationen einfach die richtigen Zugriffsrichtlinien angefangen bei automatischen, attributbasierte Zuordnung (Szenarien ABAC oder RBAC) durch die Delegierung bis einschließlich administratorverwaltung zu erreichen. Mit Azure AD können einfach erreichen komplexe Richtlinien Kombinieren mehrerer Management Modelle für eine einzelne Anwendung und können erneut verwenden, Management-Regeln für Anwendungen mit der gleichen Zielgruppen.

* [Hinzufügen von neuen oder vorhandenen Anwendungen](active-directory-sso-integrate-saas-apps.md)

 Azure AD-Anwendung Zuweisung konzentriert sich auf zwei primären Zuweisung Modi:

* **Einzelne Zuweisung** ein IT-Administrator mit Globaladministrator von Verzeichnisberechtigungen einzelne Benutzerkonten auswählen und ihnen Zugriff auf die Anwendung zu gewähren.
* **Gruppenbasierte Zuweisung (nur Azure AD bezahlt)** ein IT-Administrator mit Globaladministrator von Verzeichnisberechtigungen kann eine Gruppe zuweisen, an die Anwendung. Zugriff für bestimmte Benutzer richtet sich nach, ob Mitglieder der Gruppe zu dem Zeitpunkt, die Sie versuchen werden, auf die Anwendung zugreifen. Das heißt, kann ein Administrator effektiv eine agentzuweisungsregel, die besagt "alle aktuellen Mitglieder der zugewiesenen Gruppe Zugriff auf die Anwendung ist" erstellen. Diese Zuweisung-Option verwenden, können Administratoren mithilfe eines Azure AD-Gruppenverwaltungsoptionen, einschließlich profitieren [dynamische Gruppen attributbasierte](active-directory-accessmanagement-manage-groups.md), externe Systemgruppen (z. B. lokale Active Directory oder Workday) oder vom Administrator verwaltete oder self-service verwalteten Gruppen. Eine einzelne Gruppe kann problemlos zugewiesen werden mehrere apps sicherstellen, dass Anwendungen mit der Zuweisung Affinität Zuweisungsregeln, freigeben können verringert die allgemeine Verwaltung. Bitte beachten Sie, dass der geschachtelte Gruppenname Mitgliedschaften für gruppenbasierte Zuweisung zu Anwendungen zu diesem Zeitpunkt nicht unterstützt werden.

Verwenden diese Zuordnung zwei Modi, können Administratoren alle wünschenswert Zuweisung Ansatz erreichen.

In Azure AD ist Verwendung und Zuweisung reporting vollständig integriert, Administratoren einfach Zuweisungsstatus Zuweisungsfehler und sogar Nutzung Berichte zu aktivieren.

## <a name="complex-application-assignment-with-azure-ad"></a>Zuweisung von komplexen Anwendung in Azure AD
Betrachten Sie eine Anwendung wie Salesforce. In vielen Organisationen wird Salesforce in erster Linie von der Marketing- und Organisationen verwendet. Häufig haben Mitglieder des Marketingteams hoch privilegierter Zugriff an Salesforce, während die Mitglieder der Vertriebsgruppe beschränkten Zugriff haben. In vielen Fällen wird eine umfassende Auffüllung der Information Worker haben nur eingeschränkten Zugriff auf die Anwendung. Ausnahmen von diesen Regeln erschweren spielt eine Rolle. Es ist häufig die Befugnis der Marketing- oder wieder zu entziehen Teams zum Erteilen des Benutzerzugriffs, oder ändern ihre Rollen unabhängig von dieser allgemeinen Regeln.

Mit Azure AD können Anwendungen wie Salesforce vorkonfiguriert, dass für einmaliges Anmelden für (Unternehmen SSO) und die automatisierte Bereitstellung sein. Sobald die Anwendung konfiguriert ist, kann ein Administrator führen Sie die einmalige Aktion zum Erstellen und Zuweisen der entsprechenden Gruppen. In diesem Beispiel kann ein Administrator die folgenden Aufgaben ausführen:

* [Dynamische Gruppen](active-directory-accessmanagement-manage-groups.md) kann definiert werden, automatisch alle Mitglieder der Marketing- und Teams, die mithilfe von Attributen wie Abteilung oder Rolle dar:
  
  * Alle Mitglieder der Gruppen marketing würde die "marketing" "in Salesforce zugewiesen werden
  * Alle Mitglieder der Vertriebsteam, der die Rolle "sales" in Salesforce Gruppen zugewiesen werden. Eine weitere Verfeinerung kann mehreren Gruppen verwenden, die Darstellung von regionalen Verkaufsteams für verschiedene Salesforce-Rollen zugewiesen.
* Zum Aktivieren der Ausnahmemechanismus konnte eine Self-service-gruppenverwaltung für jede Rolle erstellt werden. Beispielsweise kann die Gruppe "Salesforce marketing Ausnahme" als Self-service-Gruppe erstellt werden. Die Gruppe kann die Rolle "marketing" Salesforce zugewiesen werden und des Marketingteams wieder zu entziehen Besitzer vorgenommen werden kann. Mitglieder des Teams marketing wieder zu entziehen konnte hinzufügen oder Entfernen Benutzer, eine Join-Richtlinie festgelegt oder sogar genehmigen bzw. verweigern einzelne Benutzer Anforderungen verknüpfen. Dies wird über eine entsprechende Informationen Worker-Plattform unterstützt, die keine spezielle Training für Besitzer oder Member benötigt.

In diesem Fall würden alle zugewiesenen Benutzer automatisch in Salesforce, bereitgestellt werden, sobald sie für verschiedene Gruppen hinzugefügt werden, die ihre rollenzuweisung in Salesforce aktualisiert wurde. Benutzer wäre ermitteln und Salesforce zuzugreifen, über die Anwendung den Zugriffsbereich Office Webclients, oder auch nur, um ihre Organisation Salesforce-Anmeldeseite navigieren können. Administratoren wäre, Verwendung und Zuweisung Status mithilfe von Azure AD-Berichten leicht anzeigen können.

Administratoren können beschäftigen [Zugangsberechtigung für Azure AD](active-directory-conditional-access.md) Zugriffsrichtlinien für bestimmte Rollen festgelegt. Diese Richtlinien können enthalten, ob ein Zugriff außerhalb der Umgebung des Unternehmens und sogar Multi-Factor Authentication oder ein Gerät Anforderungen Zugriff in verschiedenen Fällen erzielen zulässig ist.

## <a name="how-can-i-get-started"></a>Wie kann ich beginnen?
Erste, wenn Sie nicht sind bereits von Azure AD mithilfe und IT-Administrator sind:

* [Probieren Sie es aus!](https://azure.microsoft.com/trial/get-started-active-directory/) -Sie können registrieren Sie sich für eine kostenlose 30-Tage-Testversion heute und weniger als 5 Minuten mit dem folgenden Link Ihre erste Cloudlösung bereitstellen

Azure AD-Funktionen, die gemeinsame Nutzung von Konten zu ermöglichen:

* [Gruppenzuordnung](active-directory-accessmanagement-self-service-group-management.md)
* Hinzufügen von Anwendungen in Azure AD
* Erste Schritte bei der Zuweisung
* Zuweisung der Anwendung häufig gestellte Fragen
* [App-Nutzung/Dashboardberichte](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Wo kann ich mehr erfahren?
* [Artikel-Index für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Schützen von apps mit bedingtem Zugriff](active-directory-conditional-access.md)
* [Self-service-gruppenverwaltung Management/SSAA](active-directory-accessmanagement-self-service-group-management.md)

