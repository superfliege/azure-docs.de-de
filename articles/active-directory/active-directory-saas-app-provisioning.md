---
title: Automatisierte SaaS-App-Benutzerbereitstellung in Azure AD | Microsoft-Dokumentation
description: "Einführung in die Verwendung von Azure AD für die automatisierte Bereitstellung, Bereitstellungsaufhebung und fortlaufende Aktualisierung von Benutzerkonten für verschiedene SaaS-Drittanbieteranwendungen."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2017
ms.author: asmalser
ms.openlocfilehash: 3fe57e9c22d04a3557978093ce3fe86613c5c1d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Was ist eine automatisierte Benutzerbereitstellung für SaaS-Apps?
Azure Active Directory (Azure AD) ermöglicht Ihnen das automatisierte Erstellen, Warten und Entfernen von Benutzeridentitäten in Cloudanwendungen ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)), wie z.B. Dropbox, Salesforce und ServiceNow.

**Nachstehend finden Sie einige Beispiele für Aufgaben, die Sie mit diesem Feature ausführen können:**

* Automatisches Erstellen neuer Konten in den richtigen Systemen für neue Benutzer, wenn diese Ihr Team oder Ihre Organisation verstärken
* Automatisches Deaktivieren von Konten in den richtigen Systemen, wenn Benutzer das Team oder die Organisation verlassen
* Sicherstellen, dass die Identitäten in Ihren Apps und Systemen bei Änderungen im Verzeichnis oder in Ihrem Personalsystem auf den neuesten Stand gebracht werden
* Bereitstellen benutzerfremder Objekte (beispielsweise Gruppen) für Anwendungen mit entsprechender Unterstützung

**Die automatisierte Benutzerbereitstellung bietet außerdem folgende Funktionalität:**

* Abgleich vorhandener Identitäten zwischen Quell- und Zielsystemen.
* Optionen zur Anpassung von Azure AD an die aktuellen Konfigurationen der Apps und Systeme, die Ihre Organisation zurzeit verwendet.
* Optionale E-Mail-Warnungen zu Bereitstellungsfehlern.
* Berichterstellung und Aktivitätsprotokolle unterstützen Sie bei der Überwachung und Problembehandlung.

## <a name="why-use-automated-provisioning"></a>Argumente für die automatisierte Bereitstellung
Nachfolgend werden einige Gründe aufgeführt, die für die Verwendung dieser Funktion sprechen:

* Vermeiden von Kosten, Ineffizienz und Fehlern, die in Zusammenhang mit manuellen Bereitstellungsvorgängen auftreten.
* Vermeiden der Kosten im Zusammenhang mit dem Hosten und Verwalten benutzerdefiniert entwickelter Bereitstellungslösungen und Skripts.
* Sicherstellen, dass Ihre Organisation Benutzeridentitäten sofort aus wichtigen SaaS-Apps entfernen kann, wenn Benutzer die Organisation verlassen.
* Einfaches gleichzeitiges Importieren zahlreicher Benutzer in eine bestimmte SaaS-Anwendung oder in ein bestimmtes System.
* Mit dieser Bereitstellungslösung werden dieselben App-Zugriffsrichtlinien angewendet wie diejenigen, die Sie für die einmalige Azure AD-Anmeldung (SSO) definiert haben.


## <a name="how-does-automatic-provisioning-work"></a>Funktionsweise der automatischen Bereitstellung
    
Der **Azure AD-Bereitstellungsdienst** stellt Benutzer für SaaS-Apps und andere Systeme bereit, indem er eine Verbindung mit Endpunkten der Benutzerverwaltungs-API herstellt, die von den jeweiligen Anwendungsanbietern zur Verfügung gestellt werden. Diese Endpunkte der Benutzerverwaltungs-API ermöglichen Azure AD das programmgesteuerte Erstellen, Aktualisieren und Entfernen von Benutzern. Bei bestimmten Anwendungen kann der Bereitstellungsdienst auch zusätzliche identitätsbezogene Objekte wie Gruppen und Rollen erstellen, aktualisieren und entfernen. 

![Bereitstellung](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*Abbildung 1: Der Azure AD-Bereitstellungsdienst*

![Ausgehende Bereitstellung](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*Abbildung 2: Workflow für die ausgehende Benutzerbereitstellung von Azure AD in beliebten SaaS-Anwendungen*

![Eingehende Bereitstellung](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*Abbildung 3: Workflow für die eingehende Benutzerbereitstellung aus beliebten Personalverwaltungsanwendungen (Human Capital Management, HCM) in Azure Active Directory und Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Welche Anwendungen und Systeme kann ich mit der automatischen Benutzerbereitstellung von Azure AD verwenden?

Azure AD unterstützt standardmäßig verschiedene beliebte SaaS-Apps und Personalsysteme und bietet allgemeine Unterstützung für Apps, die bestimmte Teile des [SCIM 2.0-Standards](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-scim-provisioning) implementieren.

Alle ausgewählten Apps im Azure AD-Anwendungskatalog unterstützen die automatisierte Benutzerbereitstellung. [Die Liste der ausgewählten Apps kann hier eingesehen werden.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Damit eine Anwendung die automatisierte Benutzerbereitstellung unterstützt, müssen zunächst die erforderlichen Benutzerverwaltungsendpunkte bereitgestellt werden, die externen Programmen die Automatisierung der Erstellung, Wartung und Entfernung von Benutzern ermöglichen. Daher sind nicht alle SaaS-Apps mit diesem Feature kompatibel. Für Apps, die Benutzerverwaltungs-APIs unterstützen, kann das Azure AD-Entwicklerteam anschließend einen Bereitstellungsconnector erstellen. Diese Tätigkeit wird in Abhängigkeit von den Anforderungen bestehender und potenzieller Kunden priorisiert. 

Wenn Sie das Azure AD-Entwicklungsteam kontaktieren möchten, um Bereitstellungsunterstützung für zusätzliche Anwendungen anzufordern, senden Sie uns eine Nachricht über das [Azure Active Directory-Feedbackforum](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning). 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Wie richte ich die automatische Bereitstellung für eine Anwendung ein?

Die Konfiguration des Azure AD-Bereitstellungsdiensts für eine bestimmte Anwendung beginnt im **[Azure-Portal](https://potal.azure.com)**. Klicken Sie im Abschnitt **Azure Active Directory > Unternehmensanwendungen** auf **Hinzufügen**. Klicken Sie anschließend auf **Alle**, und fügen Sie dann abhängig von Ihrem Szenario eine der folgenden Optionen hinzu:

* Alle Anwendungen im Abschnitt **Ausgewählte Anwendungen** unterstützen die automatische Bereitstellung.

* Verwenden Sie die Option „Nicht-Kataloganwendung“ für benutzerdefiniert entwickelte SCIM-Integrationen.

![Gallery](./media/active-directory-saas-app-provisioning/gallery.png)

Die Bereitstellung wird auf der Registerkarte **Bereitstellung** des Anwendungsverwaltungsbildschirms konfiguriert.

![Einstellungen](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* Für den Azure AD-Bereitstellungsdienst müssen **Administratoranmeldeinformationen** angegeben werden, die die Verbindungsherstellung mit der von der Anwendung bereitgestellten Benutzerverwaltungs-API ermöglichen.

* Konfigurierbare **Attributzuordnungen** geben an, welche Felder im Quellsystem (beispielsweise Azure AD) mit welchen Feldern im Zielsystem (beispielsweise ServiceNow) synchronisiert werden sollen. Sofern dies vom Zielsystem unterstützt wird, können Sie in diesem Abschnitt neben Benutzerkonten auch die Bereitstellung von Gruppen konfigurieren. Dies ist jedoch optional. Mithilfe des Eigenschaftenabgleichs können Sie auswählen, welche Felder für den Kontenabgleich zwischen den Systemen herangezogen werden sollen. Mithilfe von [Ausdrücken](active-directory-saas-writing-expressions-for-attribute-mappings.md) können Sie die aus dem Quellsystem abgerufenen Werte vor dem Schreiben in das Zielsystem ändern und transformieren. Weitere Informationen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](active-directory-saas-customizing-attribute-mappings.md).

![Einstellungen](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Bereichsdefinitionsfilter** teilen dem Bereitstellungsdienst mit, welche Benutzer und welche Gruppe aus dem Quellsystem im Zielsystem bereitgestellt werden sollen bzw. für welche Benutzer und für welche Gruppe diese Bereitstellung aufgehoben werden soll. Zwei Aspekte der Bereichsdefinitionsfilter werden zusammen ausgewertet und bestimmen, wer zum Geltungsbereich für die Bereitstellung gehört:

* **Filter für Attributwerte:** Über das Menü „Quellobjektbereich“ in den Attributzuordnungen kann nach bestimmten Attributwerten gefiltert werden. So können Sie beispielsweise angeben, dass nur Benutzer mit dem Abteilungsattribut „Vertrieb“ zum Geltungsbereich für die Bereitstellung gehören sollen.

* **Filter für Zuweisungen:** Über das Bereichsmenü im Abschnitt „Bereitstellung“ &gt; „Einstellungen“ des Portals können Sie angeben, ob nur zugewiesene Benutzer und Gruppen zum Geltungsbereich für die Bereitstellung gehören oder ob alle Benutzer im Azure AD-Verzeichnis bereitgestellt werden sollen. Informationen zum Zuweisen von Benutzern und Gruppen finden Sie unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md).
    
* **Einstellungen** steuern den Betrieb des Bereitstellungsdiensts für eine Anwendung (unter anderem, ob er derzeit ausgeführt wird).

* **Überwachungsprotokolle** stellen Datensätze für jeden Vorgang bereit, der vom Azure AD-Bereitstellungsdienst ausgeführt wurde. Ausführlichere Informationen finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](active-directory-saas-provisioning-reporting.md).

![Einstellungen](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

## <a name="what-happens-during-provisioning"></a>Vorgänge während der Bereitstellung

1. Wenn Sie die Bereitstellung für eine Anwendung das erste Mal aktivieren, werden die folgenden Aktionen ausgeführt:
   * Azure AD versucht, vorhandene Benutzer in der SaaS-App mit den entsprechenden Identitäten im Verzeichnis abzugleichen. Wenn ein Benutzer dem Abgleich entspricht, wird ihm *nicht* automatisch das einmalige Anmelden ermöglicht. Damit ein Benutzer auf die Anwendung zugreifen kann, muss er explizit in Azure AD der App zugewiesen werden, entweder direkt oder über eine Gruppenmitgliedschaft.
   * Wenn Sie bereits angegeben haben, welche Benutzer der Anwendung zugewiesen werden sollen, aber Azure AD keine vorhandenen Konten für diese Benutzer findet, stellt Azure AD für die Benutzer neue Konten in der Anwendung bereit.
2. Nach Abschluss der oben beschriebenen Erstsynchronisierung prüft Azure AD alle 20 Minuten, ob folgende Änderungen vorliegen:
   * Wenn der Anwendung neue Benutzer zugewiesen wurden (entweder direkt oder über eine Gruppenmitgliedschaft), werden sie mit einem neuen Konto in der SaaS-App bereitgestellt.
   * Wenn der Zugriff eines Benutzers entfernt wurde, wird das dazugehörige Konto in der SaaS-App als deaktiviert gekennzeichnet. (Benutzer werden niemals vollständig gelöscht, um Sie im Falle einer Fehlkonfiguration vor Datenverlusten zu schützen.)
   * Wenn ein Benutzer vor Kurzem der Anwendung zugewiesen wurde und bereits über ein Konto in der SaaS-App verfügt, wird das Konto als aktiviert gekennzeichnet, und bestimmte Benutzereigenschaften werden ggf. aktualisiert, wenn sie im Vergleich zum Verzeichnis veraltet sind.
   * Im Verzeichnis geänderte Benutzerinformationen (beispielsweise Telefonnummer oder Bürostandort) werden in der SaaS-Anwendung entsprechend aktualisiert.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
**Wie häufig schreibt Azure AD Verzeichnisänderungen in die SaaS-Apps?**

Nach Abschluss der vollständigen Erstsynchronisierung prüft der Azure AD-Bereitstellungsdienst üblicherweise alle 20 Minuten, ob Änderungen vorliegen. 

Wenn die SaaS-App mehrere Fehler zurückgibt (beispielsweise bei ungültigen Administratoranmeldeinformationen), verringert Azure AD die Häufigkeit auf eine Abfrage pro Tag, bis die Fehler behoben wurden. In diesem Fall wird der Azure AD-Bereitstellungsdienst in einen Quarantänezustand versetzt und gibt dies im [Zusammenfassungsbericht für die Bereitstellung](active-directory-saas-provisioning-reporting.md) an.

**Wie lange dauert die Bereitstellung meiner Benutzer?**

Nach Abschluss der vollständigen Erstsynchronisierung werden in der Regel innerhalb von 20 bis 40 Minuten inkrementelle Änderungen vorgenommen. Wenn Sie den Großteil Ihres Verzeichnisses bereitstellen möchten, hängt die Dauer von Ihrer Benutzer- und Gruppenanzahl ab. Die Leistung ist abhängig von der Leistung der Benutzerverwaltungs-APIs, die die Bereitstellungsdienste verwenden, um Daten aus dem Quellsystem zu lesen und in das Zielsystem zu schreiben. 

Die Leistung wird zudem beeinträchtigt, wenn viele Fehler auftreten und der Bereitstellungsdienst in den Quarantänezustand versetzt wurde. (Die Fehler werden in den [Überwachungsprotokollen](active-directory-saas-provisioning-reporting.md) erfasst.)

**Was ist eine vollständige Erstsynchronisierung, und warum dauert sie länger als nachfolgende Synchronisierungen?**

Bei der erstmaligen Ausführung des Azure AD-Bereitstellungsdiensts für eine bestimmte App wird eine Momentaufnahme der Benutzer (und optional der Gruppen) des Quellverzeichnisses erstellt. Diese Momentaufnahme ermöglicht dem Bereitstellungsdienst die Verringerung der Roundtrips zu den Quell- und Ziel-APIs sowie eine höhere Effizienz bei nachfolgenden Deltasynchronisierungen. 

Bei sehr kleinen Verzeichnissen ist die vollständige Erstsynchronisierung von Benutzern häufig nach wenigen Minuten abgeschlossen. Bei umfangreicheren Verzeichnissen kann der Vorgang dagegen mehrere Stunden dauern. Die Erstsynchronisierung von Unternehmensverzeichnissen mit mehreren hunderttausend Benutzern kann viele Stunden in Anspruch nehmen. Die nach der Erstsynchronisierung stattfindenden Deltasynchronisierungen sind allerdings wesentlich schneller.

> [!NOTE]
> Bei Anwendungen, die die Bereitstellung von Gruppen und Gruppenmitgliedschaften unterstützen, dauert die vollständige Erstsynchronisierung erheblich länger, wenn diese Option aktiviert wird. Wenn Sie in Ihrer Anwendung keine Gruppennamen und Gruppenmitgliedschaften bereitstellen möchten, deaktivieren Sie diese Option in den [Attributzuordnungen](active-directory-saas-customizing-attribute-mappings.md) Ihrer Bereitstellungskonfiguration.

**Wie kann ich den Fortschritt des aktuellen Bereitstellungsauftrags verfolgen?**

Informationen finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](active-directory-saas-provisioning-reporting.md).

**Woher weiß ich, dass Benutzer nicht ordnungsgemäß bereitgestellt wurden?**

Alle Fehler werden in den Überwachungsprotokollen von Azure AD erfasst. Weitere Informationen finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](active-directory-saas-provisioning-reporting.md).

**Wie kann ich Feedback an das Entwicklerteam senden?**

Setzen Sie über das [Azure Active Directory-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/) mit uns in Verbindung.


## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](active-directory-saas-customizing-attribute-mappings.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereichsfilter für die Benutzerbereitstellung](active-directory-saas-scoping-filters.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)
* [Kontobereitstellungsbenachrichtigungen](active-directory-saas-account-provisioning-notifications.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)

