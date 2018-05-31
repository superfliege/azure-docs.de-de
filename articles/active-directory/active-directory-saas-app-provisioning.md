---
title: Automatisierte SaaS-App-Benutzerbereitstellung in Azure AD | Microsoft-Dokumentation
description: Einführung in die Verwendung von Azure AD für die automatisierte Bereitstellung, Bereitstellungsaufhebung und fortlaufende Aktualisierung von Benutzerkonten für verschiedene SaaS-Drittanbieteranwendungen.
services: active-directory
documentationcenter: ''
author: asmalser-msft
manager: mtillman
editor: ''
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2017
ms.author: asmalser
ms.openlocfilehash: 72f796f0a4522b66feb55b827b02a83dcfdd3a01
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069892"
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
* Anpassbare Attributzuordnungen, die definieren, welche Benutzerdaten vom Quellsystem an das Zielsystem fließen sollen.
* Optionale E-Mail-Warnungen zu Bereitstellungsfehlern
* Berichterstellung und Aktivitätsprotokolle unterstützen Sie bei der Überwachung und Problembehandlung.

## <a name="why-use-automated-provisioning"></a>Argumente für die automatisierte Bereitstellung
Nachfolgend werden einige Gründe aufgeführt, die für die Verwendung dieser Funktion sprechen:

* Vermeiden von Kosten, Ineffizienz und Fehlern, die in Zusammenhang mit manuellen Bereitstellungsvorgängen auftreten.
* Vermeiden der Kosten im Zusammenhang mit dem Hosten und Verwalten benutzerdefiniert entwickelter Bereitstellungslösungen und Skripts.
* Sicherstellen, dass Ihre Organisation Benutzeridentitäten sofort aus wichtigen SaaS-Apps entfernen kann, wenn Benutzer die Organisation verlassen.
* Einfaches Importieren einer großen Zahl von Benutzern in eine bestimmte SaaS-Anwendung oder ein bestimmtes System.
* Vorteil eines einzelnen Richtliniensatzes für die Ermittlung, wer bereitgestellt wird und wer sich an einer App anmelden kann.


## <a name="how-does-automatic-provisioning-work"></a>Funktionsweise der automatischen Bereitstellung
    
Der **Azure AD-Bereitstellungsdienst** stellt Benutzer für SaaS-Apps und andere Systeme bereit, indem er eine Verbindung mit Endpunkten der Benutzerverwaltungs-API herstellt, die von den jeweiligen Anwendungsanbietern zur Verfügung gestellt werden. Diese Endpunkte der Benutzerverwaltungs-API ermöglichen Azure AD das programmgesteuerte Erstellen, Aktualisieren und Entfernen von Benutzern. Bei bestimmten Anwendungen kann der Bereitstellungsdienst auch zusätzliche identitätsbezogene Objekte wie Gruppen und Rollen erstellen, aktualisieren und entfernen. 

![Bereitstellung](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*Abbildung 1: Der Azure AD-Bereitstellungsdienst*

![Ausgehende Bereitstellung](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*Abbildung 2: Workflow für die ausgehende Benutzerbereitstellung von Azure AD in beliebten SaaS-Anwendungen*

![Eingehende Bereitstellung](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*Abbildung 3: Workflow für die eingehende Benutzerbereitstellung aus beliebten Personalverwaltungsanwendungen (Human Capital Management, HCM) in Azure Active Directory und Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Welche Anwendungen und Systeme kann ich mit der automatischen Benutzerbereitstellung von Azure AD verwenden?

Azure AD unterstützt standardmäßig verschiedene beliebte SaaS-Apps und Personalsysteme und bietet allgemeine Unterstützung für Apps, die bestimmte Teile des SCIM 2.0-Standards implementieren.

Eine Aufstellung aller Anwendungen, für die Azure AD einen vorab integrierten Bereitstellungsconnector unterstützt, finden Sie in der [Liste mit den Anwendungstutorials zur Benutzerbereitstellung](active-directory-saas-tutorial-list.md).

Informationen dazu, wie Sie einer Anwendung die Unterstützung für die Azure AD-Benutzerbereitstellung hinzufügen, finden Sie unter [Verwenden von SCIM (System for Cross-domain Identity Management) für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md).

Wenn Sie das Azure AD-Entwicklungsteam kontaktieren möchten, um Bereitstellungsunterstützung für zusätzliche Anwendungen anzufordern, senden Sie uns eine Nachricht über das [Azure Active Directory-Feedbackforum](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).    

> [!NOTE]
> Damit eine Anwendung die automatisierte Benutzerbereitstellung unterstützt, müssen zunächst die erforderlichen APIs für die Benutzerverwaltung bereitgestellt werden, die externen Programmen die Automatisierung der Erstellung, Wartung und Entfernung von Benutzern ermöglichen. Daher sind nicht alle SaaS-Apps mit diesem Feature kompatibel. Für Apps, die Benutzerverwaltungs-APIs unterstützen, kann das Azure AD-Entwicklerteam anschließend einen Bereitstellungsconnector erstellen. Diese Tätigkeit wird in Abhängigkeit von den Anforderungen bestehender und potenzieller Kunden priorisiert. 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Wie richte ich die automatische Bereitstellung für eine Anwendung ein?

Die Konfiguration des Azure AD-Bereitstellungsdiensts für eine bestimmte Anwendung beginnt im **[Azure-Portal](https://portal.azure.com)**. Klicken Sie im Abschnitt **Azure Active Directory > Unternehmensanwendungen** auf **Hinzufügen**. Klicken Sie anschließend auf **Alle**, und fügen Sie dann abhängig von Ihrem Szenario eine der folgenden Optionen hinzu:

* Alle Anwendungen im Abschnitt **Ausgewählte Anwendungen** unterstützen die automatische Bereitstellung. Weitere Anwendungen finden Sie in der [Liste mit den Anwendungstutorials zur Benutzerbereitstellung](active-directory-saas-tutorial-list.md).

* Verwenden Sie die Option „Nicht-Kataloganwendung“ für benutzerdefiniert entwickelte SCIM-Integrationen.

![Gallery](./media/active-directory-saas-app-provisioning/gallery.png)

Die Bereitstellung wird auf der Registerkarte **Bereitstellung** des Anwendungsverwaltungsbildschirms konfiguriert.

![Einstellungen](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* Für den Azure AD-Bereitstellungsdienst müssen **Administratoranmeldeinformationen** angegeben werden, die die Verbindungsherstellung mit der von der Anwendung bereitgestellten Benutzerverwaltungs-API ermöglichen. In diesem Abschnitt können Sie auch E-Mail-Benachrichtigungen aktivieren, wenn für die Anmeldeinformationen ein Fehler auftritt oder der Bereitstellungsauftrag in [Quarantäne](#quarantine) versetzt wird.

* Konfigurierbare **Attributzuordnungen** geben an, welche Felder im Quellsystem (beispielsweise Azure AD) mit welchen Feldern im Zielsystem (beispielsweise ServiceNow) synchronisiert werden sollen. Sofern dies vom Zielsystem unterstützt wird, können Sie in diesem Abschnitt neben Benutzerkonten auch die Bereitstellung von Gruppen konfigurieren. Dies ist jedoch optional. Mithilfe des Eigenschaftenabgleichs können Sie auswählen, welche Felder für den Kontenabgleich zwischen den Systemen herangezogen werden sollen. Mithilfe von [Ausdrücken](active-directory-saas-writing-expressions-for-attribute-mappings.md) können Sie die aus dem Quellsystem abgerufenen Werte vor dem Schreiben in das Zielsystem ändern und transformieren. Weitere Informationen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](active-directory-saas-customizing-attribute-mappings.md).

![Einstellungen](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Bereichsdefinitionsfilter** teilen dem Bereitstellungsdienst mit, welche Benutzer und welche Gruppe aus dem Quellsystem im Zielsystem bereitgestellt werden sollen bzw. für welche Benutzer und für welche Gruppe diese Bereitstellung aufgehoben werden soll. Zwei Aspekte der Bereichsdefinitionsfilter werden zusammen ausgewertet und bestimmen, wer zum Geltungsbereich für die Bereitstellung gehört:

    * **Filter für Attributwerte:** Über das Menü „Quellobjektbereich“ in den Attributzuordnungen kann nach bestimmten Attributwerten gefiltert werden. So können Sie beispielsweise angeben, dass nur Benutzer mit dem Abteilungsattribut „Vertrieb“ zum Geltungsbereich für die Bereitstellung gehören sollen. Weitere Informationen finden Sie unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](active-directory-saas-scoping-filters.md).

    * **Filter für Zuweisungen:** Über das Bereichsmenü im Abschnitt „Bereitstellung“ &gt; „Einstellungen“ des Portals können Sie angeben, ob nur zugewiesene Benutzer und Gruppen zum Geltungsbereich für die Bereitstellung gehören oder ob alle Benutzer im Azure AD-Verzeichnis bereitgestellt werden sollen. Informationen zum Zuweisen von Benutzern und Gruppen finden Sie unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md).
    
* **Einstellungen** steuern den Betrieb des Bereitstellungsdiensts für eine Anwendung (unter anderem, ob er derzeit ausgeführt wird).

* **Überwachungsprotokolle** stellen Datensätze für jeden Vorgang bereit, der vom Azure AD-Bereitstellungsdienst ausgeführt wurde. Ausführlichere Informationen finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](active-directory-saas-provisioning-reporting.md).

![Einstellungen](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

> [!NOTE]
> Der Benutzerbereitstellungsdienst von Azure AD kann auch über die [Microsoft Graph-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) konfiguriert und verwaltet werden.


## <a name="what-happens-during-provisioning"></a>Vorgänge während der Bereitstellung

Wenn Azure AD das Quellsystem ist, verwendet der Bereitstellungsdienst das [Feature „Differenzielle Abfragen“ der Azure AD Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query), um Benutzer und Gruppen zu überwachen. Der Bereitstellungsdienst führt eine erste Synchronisierung für das Quellsystem und das Zielsystem aus, gefolgt von regelmäßigen inkrementellen Synchronisierungen. 

### <a name="initial-sync"></a>Erste Synchronisierung
Nachdem der Bereitstellungsdienst gestartet wurde, umfasst die erste jemals durchgeführte Synchronisierung Folgendes:

1. Abfragen aller Benutzer und Gruppen aus dem Quellsystem und Abrufen aller Attribute, die in den [Attributzuordnungen](active-directory-saas-customizing-attribute-mappings.md) definiert sind.
2. Filtern der zurückgegebenen Benutzer und Gruppen, indem alle konfigurierten [Zuweisungen](active-directory-coreapps-assign-user-azure-portal.md) oder [attributbasierten Bereichsfilter](active-directory-saas-scoping-filters.md) verwendet werden.
3. Wenn für einen Benutzer ermittelt wird, dass er zugewiesen ist oder in den Bereich für die Bereitstellung fällt, sendet der Dienst eine Anfrage zu einem übereinstimmenden Benutzer an das Zielsystem, indem die angegebenen [Abgleichattribute](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties) verwendet werden. Beispiel: Wenn der userPrincipal-Name auf dem Quellsystem das übereinstimmende Attribut ist und dem userName auf dem Zielsystem zugeordnet ist, fragt der Bereitstellungsdienst beim Zielsystem userNames ab, die mit den Werten von userPrincipal-Namen auf dem Quellsystem übereinstimmen.
4. Wenn auf dem Zielsystem kein übereinstimmender Benutzer gefunden wird, wird er erstellt, indem die vom Quellsystem zurückgegebenen Attribute verwendet werden.
5. Falls ein übereinstimmender Benutzer gefunden wird, wird er aktualisiert, indem die vom Quellsystem bereitgestellten Attribute verwendet werden.
6. Wenn die Attributzuordnungen „Referenzattribute“ enthalten, führt der Dienst auf dem Zielsystem zusätzliche Updates durch, um die referenzierten Objekte zu erstellen und zu verknüpfen. Ein Benutzer kann auf dem Zielsystem beispielsweise über das Attribut „Manager“ verfügen, das mit einem anderen Benutzer verknüpft ist, der im Zielsystem erstellt wurde.
7. Legen Sie am Ende der ersten Synchronisierung einen Grenzwert fest, der als Startpunkt für die nachfolgenden inkrementellen Synchronisierungen dient.

Einige Anwendungen, z.B. ServiceNow, Google Apps und Box, unterstützen nicht nur die Bereitstellung von Benutzern, sondern auch die Bereitstellung von Gruppen und ihrer Mitglieder. Wenn die Gruppenbereitstellung in den [Zuordnungen](active-directory-saas-customizing-attribute-mappings.md) aktiviert ist, synchronisiert der Bereitstellungsdienst die Benutzer und Gruppen und anschließend die Gruppenmitgliedschaften. 

### <a name="incremental-syncs"></a>Inkrementelle Synchronisierungen
Nach der ersten Synchronisierung gilt für alle nachfolgenden Synchronisierungen Folgendes:

1. Fragen Sie vom Quellsystem alle Benutzer und Gruppen ab, die aktualisiert wurden, seitdem der letzte Grenzwert gespeichert wurde.
2. Filtern der zurückgegebenen Benutzer und Gruppen, indem alle konfigurierten [Zuweisungen](active-directory-coreapps-assign-user-azure-portal.md) oder [attributbasierten Bereichsfilter](active-directory-saas-scoping-filters.md) verwendet werden.
3. Wenn für einen Benutzer ermittelt wird, dass er zugewiesen ist oder in den Bereich für die Bereitstellung fällt, sendet der Dienst eine Anfrage zu einem übereinstimmenden Benutzer an das Zielsystem, indem die angegebenen [Abgleichattribute](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties) verwendet werden.
4. Wenn auf dem Zielsystem kein übereinstimmender Benutzer gefunden wird, wird er erstellt, indem die vom Quellsystem zurückgegebenen Attribute verwendet werden.
5. Falls ein übereinstimmender Benutzer gefunden wird, wird er aktualisiert, indem die vom Quellsystem bereitgestellten Attribute verwendet werden.
6. Wenn die Attributzuordnungen „Referenzattribute“ enthalten, führt der Dienst auf dem Zielsystem zusätzliche Updates durch, um die referenzierten Objekte zu erstellen und zu verknüpfen. Ein Benutzer kann auf dem Zielsystem beispielsweise über das Attribut „Manager“ verfügen, das mit einem anderen Benutzer verknüpft ist, der im Zielsystem erstellt wurde.
7. Wenn ein Benutzer, der sich zuvor im Bereich für die Bereitstellung befunden hat, aus dem Bereich entfernt (und die Zuweisung aufgehoben) wird, deaktiviert der Dienst den Benutzer im Zielsystem per Update.
8. Wenn ein Benutzer, der sich zuvor im Bereich für die Bereitstellung befunden hat, im Quellsystem deaktiviert oder vorläufig gelöscht wird, deaktiviert der Dienst den Benutzer im Zielsystem per Update.
9. Wenn ein Benutzer, der sich zuvor im Bereich für die Bereitstellung befunden hat, im Quellsystem endgültig gelöscht wird, löscht der Dienst den Benutzer im Zielsystem. In Azure AD werden Benutzer 30 Tage nach dem vorläufigen Löschen endgültig gelöscht.
10. Legen Sie am Ende der inkrementellen Synchronisierung einen neuen Grenzwert fest, der als Startpunkt für die nachfolgenden inkrementellen Synchronisierungen dient.

>[!NOTE]
> Sie können Erstellungs-, Aktualisierungs- und Löschvorgänge optional deaktivieren, indem Sie die Kontrollkästchen für **Zielobjektaktionen** im Abschnitt [Attributzuordnungen](active-directory-saas-customizing-attribute-mappings.md) verwenden. Die Logik zum Deaktivieren eines Benutzers während eines Updates wird ebenfalls per Attributzuordnung über ein Feld wie „accountEnabled“ gesteuert.

Der Bereitstellungsdienst fährt damit fort, so lange nacheinander inkrementelle Synchronisierungen durchzuführen (die Intervalle sind in den [einzelnen Tutorials der Anwendungen](active-directory-saas-tutorial-list.md) angegeben), bis eines der folgenden Ereignisse eintritt:

* Der Dienst wird mit dem Azure-Portal oder mit dem entsprechenden Graph-API-Befehl manuell beendet. 
* Eine neue erste Synchronisierung wird ausgelöst, indem im Azure-Portal die Option **Clear state and restart** (Status löschen und neu starten) oder der entsprechende Graph-API-Befehl verwendet wird. Hierdurch werden alle gespeicherten Grenzwerte gelöscht, und alle Quellobjekte erneut ausgewertet.
* Eine neue erste Synchronisierung wird aufgrund einer Änderung in den Attributzuordnungen oder Bereichsfiltern ausgelöst. Hierdurch werden ebenfalls alle gespeicherten Grenzwerte gelöscht und alle Quellobjekte erneut ausgewertet.
* Der Bereitstellungsprozess wird aufgrund einer hohen Fehlerrate in Quarantäne versetzt (siehe unten) und verbleibt mehr als vier Wochen lang in diesem Zustand. In diesem Fall wird der Dienst automatisch deaktiviert.

### <a name="errors-and-retries"></a>Fehler und Wiederholungen 
Wenn ein einzelner Benutzer im Zielsystem aufgrund eines Fehlers nicht hinzugefügt, aktualisiert oder gelöscht werden kann, wird während des nächsten Synchronisierungszyklus versucht, den Vorgang zu wiederholen. Falls für den Benutzer weiterhin ein Fehler auftritt, wird die Häufigkeit der Wiederholungen allmählich verringert, bis schließlich nur noch ein Versuch pro Tag durchgeführt wird. Zum Beheben des Fehlers müssen Administratoren die [Überwachungspfade](active-directory-saas-provisioning-reporting.md) auf Ereignisse vom Typ „Process Escrow“ (Prozesshinterlegung) prüfen, um die Grundursache zu ermitteln, und dann entsprechende Maßnahmen ergreifen. Beispiele für häufig auftretende Fehler sind:

* Für Benutzer ist auf dem Quellsystem ein bestimmtes Attribut nicht angegeben, das auf dem Zielsystem erforderlich ist.
* Benutzer verfügen auf dem Quellsystem über einen Attributwert, für den auf dem Zielsystem eine eindeutige Einschränkung besteht, und der gleiche Wert ist auch in einem anderen Benutzerdatensatz vorhanden.

Diese Fehler können behoben werden, indem die Attributwerte für den betroffenen Benutzer im Quellsystem oder die Attributzuordnungen angepasst werden, um Konflikte zu vermeiden.   

### <a name="quarantine"></a>Quarantäne
Falls die meisten oder alle Aufrufe an das Zielsystem dauerhaft aufgrund eines Fehlers nicht erfolgreich sind (z.B. bei ungültigen Administratoranmeldeinformationen), wird der Bereitstellungsauftrag in den Zustand „Quarantäne“ versetzt. Dies ist im [Zusammenfassungsbericht für die Bereitstellung](active-directory-saas-provisioning-reporting.md) angegeben. Wenn im Azure-Portal E-Mail-Benachrichtigungen konfiguriert wurden, wird auch eine E-Mail gesendet. 

In der Quarantäne wird die Häufigkeit der inkrementellen Synchronisierungen allmählich auf einmal pro Tag verringert. 

Die Quarantäne für den Bereitstellungsauftrag wird aufgehoben, nachdem alle relevanten Fehler behoben wurden, und der nächste Synchronisierungszyklus beginnt. Falls der Bereitstellungsauftrag länger als vier Wochen in Quarantäne verbleibt, wird er deaktiviert.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie lange dauert die Bereitstellung meiner Benutzer?**

Die Leistung unterscheidet sich jeweils in Abhängigkeit davon, ob Ihr Bereitstellungsauftrag eine erste oder eine inkrementelle Synchronisierung durchführt.

Bei einer ersten Synchronisierung hängt die Dauer bis zum Abschluss direkt davon ab, wie viele Benutzer, Gruppen und Gruppenmitglieder im Quellsystem vorhanden sind. Für sehr kleine Quellsysteme mit nur Hunderten von Objekten können erste Synchronisierungen innerhalb von wenigen Minuten durchgeführt werden. Bei Quellsystemen mit Hunderttausenden oder Millionen von kombinierten Objekten dauert dieser Vorgang allerdings länger.

Bei inkrementellen Synchronisierungen hängt die Dauer davon ab, wie viele Änderungen im jeweiligen Synchronisierungszyklus erkannt werden. Wenn weniger als 5.000 Änderungen für Benutzer oder der Gruppenmitgliedschaft erkannt werden, können diese meist innerhalb eines 40-minütigen Zyklus synchronisiert werden. 

Beachten Sie hierbei, dass die Gesamtleistung sowohl vom Quell- als auch vom Zielsystem abhängig ist. Bei einigen Zielsystemen werden Limits für die Anforderungsrate und Drosselung implementiert, die sich bei umfangreichen Synchronisierungsvorgängen auf die Leistung auswirken können. Dies wird von den vordefinierten Azure AD-Bereitstellungsconnectors für diese Systeme berücksichtigt.

Die Leistung wird zudem beeinträchtigt, wenn viele Fehler auftreten und der Bereitstellungsdienst in den Quarantänezustand versetzt wurde. (Die Fehler werden in den [Überwachungsprotokollen](active-directory-saas-provisioning-reporting.md) erfasst.)

**Wie kann ich die Synchronisierungsleistung verbessern?**

Die meisten Leistungsprobleme treten bei den ersten Synchronisierungen von Systemen auf, die über eine große Zahl von Gruppen und Gruppenmitgliedern verfügen.

Wenn keine Synchronisierung von Gruppen oder Gruppenmitgliedschaften erforderlich ist, kann die Synchronisierungsleistung wie folgt deutlich verbessert werden:

1. Legen Sie im Menü **Bereitstellung > Einstellungen > Bereich** die Option **Alle synchronisieren** fest, anstatt zugewiesene Benutzer und Gruppen zu synchronisieren.
2. Verwenden Sie [Bereichsfilter](active-directory-saas-scoping-filters.md) anstelle von Zuweisungen, um die Liste mit den bereitgestellten Benutzern zu filtern.

> [!NOTE]
> Für Anwendungen, die die Bereitstellung von Gruppennamen und Gruppeneigenschaften (z.B. ServiceNow und Google Apps) unterstützen, bewirkt eine Deaktivierung dieser Option auch eine Verringerung der erforderlichen Dauer für die erste Synchronisierung. Wenn Sie in Ihrer Anwendung keine Gruppennamen und Gruppenmitgliedschaften bereitstellen möchten, deaktivieren Sie diese Option in den [Attributzuordnungen](active-directory-saas-customizing-attribute-mappings.md) Ihrer Bereitstellungskonfiguration.

**Wie kann ich den Fortschritt des aktuellen Bereitstellungsauftrags verfolgen?**

Informationen finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](active-directory-saas-provisioning-reporting.md).

**Woher weiß ich, dass Benutzer nicht ordnungsgemäß bereitgestellt wurden?**

Alle Fehler werden in den Überwachungsprotokollen von Azure AD erfasst. Weitere Informationen finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](active-directory-saas-provisioning-reporting.md).

**Wie kann ich eine Anwendung erstellen, die mit dem Bereitstellungsdienst funktioniert?**

Weitere Informationen finden Sie unter [Verwenden von SCIM (System for Cross-domain Identity Management) für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning).

**Wie kann ich Feedback an das Entwicklerteam senden?**

Setzen Sie über das [Azure Active Directory-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/) mit uns in Verbindung.


## <a name="related-articles"></a>Verwandte Artikel
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](active-directory-saas-customizing-attribute-mappings.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereichsfilter für die Benutzerbereitstellung](active-directory-saas-scoping-filters.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)
* [Azure AD synchronization API overview](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) (Azure AD-Synchronisierung – API-Übersicht)
* [Ausführlicher Bereitstellungsplan für die ausgehende Benutzerbereitstellung einer Anwendung](https://aka.ms/userprovisioningdeploymentplan)

