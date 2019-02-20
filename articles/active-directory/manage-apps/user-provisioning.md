---
title: Automatisierte SaaS-App-Benutzerbereitstellung in Azure AD | Microsoft-Dokumentation
description: Einführung in die Verwendung von Azure AD für die automatisierte Bereitstellung, Bereitstellungsaufhebung und fortlaufende Aktualisierung von Benutzerkonten für verschiedene SaaS-Drittanbieteranwendungen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: celested
ms.reviewer: asmalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e8b099f845df66dfe8c43bc6f968fd63b30d09d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56186351"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory

## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Was ist eine automatisierte Benutzerbereitstellung für SaaS-Apps?
Azure Active Directory (Azure AD) ermöglicht Ihnen das automatisierte Erstellen, Warten und Entfernen von Benutzeridentitäten in Cloudanwendungen ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)), wie z.B. Dropbox, Salesforce und ServiceNow.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**Nachstehend finden Sie einige Beispiele für Aufgaben, die Sie mit diesem Feature ausführen können:**

* Automatisches Erstellen neuer Konten in den richtigen Systemen für neue Benutzer, wenn diese Ihr Team oder Ihre Organisation verstärken
* Automatisches Deaktivieren von Konten in den richtigen Systemen, wenn Benutzer das Team oder die Organisation verlassen
* Sicherstellen, dass die Identitäten in Ihren Apps und Systemen bei Änderungen im Verzeichnis oder in Ihrem Personalsystem auf den neuesten Stand gebracht werden
* Bereitstellen benutzerfremder Objekte (beispielsweise Gruppen) für Anwendungen mit entsprechender Unterstützung

**Die automatisierte Benutzerbereitstellung bietet außerdem folgende Funktionalität:**

* Abgleich vorhandener Identitäten zwischen Quell- und Zielsystemen.
* Anpassbare Attributzuordnungen, die definieren, welche Benutzerdaten vom Quellsystem an das Zielsystem fließen sollen.
* Optionale E-Mail-Warnungen zu Bereitstellungsfehlern.
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

![Bereitstellung](./media/user-provisioning/provisioning0.PNG)
*Abbildung 1: Der Azure AD-Bereitstellungsdienst*

![Ausgehende Bereitstellung](./media/user-provisioning/provisioning1.PNG)
*Abbildung 2: Workflow für die ausgehende Benutzerbereitstellung von Azure AD in beliebten SaaS-Anwendungen*

![Eingehende Bereitstellung](./media/user-provisioning/provisioning2.PNG)
*Abbildung 3: Workflow für die eingehende Benutzerbereitstellung aus beliebten Personalverwaltungsanwendungen (Human Capital Management, HCM) in Azure Active Directory und Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Welche Anwendungen und Systeme kann ich mit der automatischen Benutzerbereitstellung von Azure AD verwenden?

Azure AD unterstützt standardmäßig verschiedene beliebte SaaS-Apps und Personalsysteme und bietet allgemeine Unterstützung für Apps, die bestimmte Teile des SCIM 2.0-Standards implementieren.

### <a name="pre-integrated-applications"></a>Vorab integrierte Anwendungen

Eine Aufstellung aller Anwendungen, für die Azure AD einen vorab integrierten Bereitstellungsconnector unterstützt, finden Sie in der [Liste mit den Anwendungstutorials zur Benutzerbereitstellung](../saas-apps/tutorial-list.md).

Wenn Sie das Azure AD-Entwicklungsteam kontaktieren möchten, um Bereitstellungsunterstützung für zusätzliche Anwendungen anzufordern, senden Sie uns eine Nachricht über das [Azure Active Directory-Feedbackforum](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> Damit eine Anwendung die automatisierte Benutzerbereitstellung unterstützt, müssen zunächst die erforderlichen APIs für die Benutzerverwaltung bereitgestellt werden, die externen Programmen die Automatisierung der Erstellung, Wartung und Entfernung von Benutzern ermöglichen. Daher sind nicht alle SaaS-Apps mit diesem Feature kompatibel. Für Apps, die Benutzerverwaltungs-APIs unterstützen, kann das Azure AD-Entwicklerteam anschließend einen Bereitstellungsconnector erstellen. Diese Tätigkeit wird in Abhängigkeit von den Anforderungen bestehender und potenzieller Kunden priorisiert. 

### <a name="connecting-applications-that-support-scim-20"></a>Verbinden von Anwendungen mit SCIM 2.0-Unterstützung

Informationen zum generischen Verbinden von Anwendungen, die SCIM 2.0-basierte APIs für die Benutzerverwaltung implementieren, finden Sie unter [Verwenden von SCIM (System for Cross-domain Identity Management) für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](use-scim-to-provision-users-and-groups.md).

    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Wie richte ich die automatische Bereitstellung für eine Anwendung ein?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Die Konfiguration des Azure AD-Bereitstellungsdiensts für eine bestimmte Anwendung beginnt im **[Azure-Portal](https://portal.azure.com)**. Klicken Sie im Abschnitt **Azure Active Directory > Unternehmensanwendungen** auf **Hinzufügen**. Klicken Sie anschließend auf **Alle**, und fügen Sie dann abhängig von Ihrem Szenario eine der folgenden Optionen hinzu:

* Alle Anwendungen im Abschnitt **Ausgewählte Anwendungen** unterstützen die automatische Bereitstellung. Weitere Anwendungen finden Sie in der [Liste mit den Anwendungstutorials zur Benutzerbereitstellung](../saas-apps/tutorial-list.md).

* Verwenden Sie die Option „Nicht-Kataloganwendung“ für benutzerdefiniert entwickelte SCIM-Integrationen.

![Gallery](./media/user-provisioning/gallery.png)

Die Bereitstellung wird auf der Registerkarte **Bereitstellung** des Anwendungsverwaltungsbildschirms konfiguriert.

![Einstellungen](./media/user-provisioning/provisioning_settings0.PNG)


* Für den Azure AD-Bereitstellungsdienst müssen **Administratoranmeldeinformationen** angegeben werden, die die Verbindungsherstellung mit der von der Anwendung bereitgestellten Benutzerverwaltungs-API ermöglichen. In diesem Abschnitt können Sie auch E-Mail-Benachrichtigungen aktivieren, wenn für die Anmeldeinformationen ein Fehler auftritt oder der Bereitstellungsauftrag in [Quarantäne](#quarantine) versetzt wird.

* Es können **Attributzuordnungen** konfiguriert werden, die angeben, die Inhalte welche Felder im Quellsystem (Beispiel: Azure AD) mit den Inhalten welcher Felder im Zielsystem (Beispiel: ServiceNow) synchronisiert werden. Sofern dies vom Zielsystem unterstützt wird, können Sie in diesem Abschnitt neben Benutzerkonten auch die Bereitstellung von Gruppen konfigurieren. Dies ist jedoch optional. Mithilfe des Eigenschaftenabgleichs können Sie auswählen, welche Felder für den Kontenabgleich zwischen den Systemen herangezogen werden sollen. Mithilfe von [Ausdrücken](functions-for-customizing-application-data.md) können Sie die aus dem Quellsystem abgerufenen Werte vor dem Schreiben in das Zielsystem ändern und transformieren. Weitere Informationen finden Sie unter [Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory](customize-application-attributes.md).

![Einstellungen](./media/user-provisioning/provisioning_settings1.PNG)

* **Bereichsdefinitionsfilter** teilen dem Bereitstellungsdienst mit, welche Benutzer und Gruppen aus dem Quellsystem im Zielsystem bereitgestellt werden sollen bzw. für welche Benutzer und Gruppen diese Bereitstellung aufgehoben werden soll. Zwei Aspekte der Bereichsdefinitionsfilter werden zusammen ausgewertet und bestimmen, wer zum Geltungsbereich für die Bereitstellung gehört:

    * **Filter für Attributwerte:** Über das Menü „Quellobjektbereich“ in den Attributzuordnungen kann nach bestimmten Attributwerten gefiltert werden. So können Sie beispielsweise angeben, dass nur Benutzer mit dem Abteilungsattribut „Vertrieb“ zum Geltungsbereich für die Bereitstellung gehören sollen. Weitere Informationen finden Sie unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](define-conditional-rules-for-provisioning-user-accounts.md).

    * **Filter für Zuweisungen:** Über das Bereichsmenü im Abschnitt „Bereitstellung“ &gt; „Einstellungen“ des Portals können Sie angeben, ob nur zugewiesene Benutzer und Gruppen zum Geltungsbereich für die Bereitstellung gehören oder ob alle Benutzer im Azure AD-Verzeichnis bereitgestellt werden sollen. Informationen zum Zuweisen von Benutzern und Gruppen finden Sie unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory](assign-user-or-group-access-portal.md).
    
* **Einstellungen** steuern den Betrieb des Bereitstellungsdiensts für eine Anwendung (unter anderem, ob er derzeit ausgeführt wird).

* **Überwachungsprotokolle** stellen Datensätze für jeden Vorgang bereit, der vom Azure AD-Bereitstellungsdienst ausgeführt wurde. Ausführlichere Informationen finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](check-status-user-account-provisioning.md).

![Einstellungen](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> Der Benutzerbereitstellungsdienst von Azure AD kann auch über die [Microsoft Graph-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) konfiguriert und verwaltet werden.


## <a name="what-happens-during-provisioning"></a>Vorgänge während der Bereitstellung

Wenn Azure AD das Quellsystem ist, verwendet der Bereitstellungsdienst das [Feature „Differenzielle Abfragen“ der Azure AD Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query), um Benutzer und Gruppen zu überwachen. Der Bereitstellungsdienst führt eine erste Synchronisierung für das Quellsystem und das Zielsystem aus, gefolgt von regelmäßigen inkrementellen Synchronisierungen. 

### <a name="initial-sync"></a>Erste Synchronisierung

Nachdem der Bereitstellungsdienst gestartet wurde, umfasst die erste jemals durchgeführte Synchronisierung Folgendes:

1. Abfragen aller Benutzer und Gruppen aus dem Quellsystem und Abrufen aller Attribute, die in den [Attributzuordnungen](customize-application-attributes.md) definiert sind.
2. Filtern der zurückgegebenen Benutzer und Gruppen, indem alle konfigurierten [Zuweisungen](assign-user-or-group-access-portal.md) oder [attributbasierten Bereichsfilter](define-conditional-rules-for-provisioning-user-accounts.md) verwendet werden.
3. Wenn für einen Benutzer ermittelt wird, dass er zugewiesen ist oder in den Bereich für die Bereitstellung fällt, sendet der Dienst eine Anfrage zu einem übereinstimmenden Benutzer an das Zielsystem, indem die angegebenen [Abgleichattribute](customize-application-attributes.md#understanding-attribute-mapping-properties) verwendet werden. Beispiel: Wenn der userPrincipal-Name auf dem Quellsystem das übereinstimmende Attribut ist und dem userName auf dem Zielsystem zugeordnet ist, fragt der Bereitstellungsdienst beim Zielsystem userNames ab, die mit den Werten von userPrincipal-Namen auf dem Quellsystem übereinstimmen.
4. Wenn auf dem Zielsystem kein übereinstimmender Benutzer gefunden wird, wird er erstellt, indem die vom Quellsystem zurückgegebenen Attribute verwendet werden. Nachdem das Benutzerkonto erstellt wurde, erkennt der Bereitstellungsdienst die ID des Zielsystems für den neuen Benutzer, die verwendet wird, um alle zukünftigen Vorgänge für diesen Benutzer auszuführen, und speichert diese zwischen.
5. Falls ein übereinstimmender Benutzer gefunden wird, wird er aktualisiert, indem die vom Quellsystem bereitgestellten Attribute verwendet werden. Nachdem das Benutzerkonto zugeordnet wurde, erkennt der Bereitstellungsdienst die ID des Zielsystems für den neuen Benutzer und speichert diese zwischen, die verwendet wird, um alle zukünftigen Vorgänge für diesen Benutzer auszuführen.
6. Wenn die Attributzuordnungen „Referenzattribute“ enthalten, führt der Dienst auf dem Zielsystem zusätzliche Updates durch, um die referenzierten Objekte zu erstellen und zu verknüpfen. Ein Benutzer kann auf dem Zielsystem beispielsweise über das Attribut „Manager“ verfügen, das mit einem anderen Benutzer verknüpft ist, der im Zielsystem erstellt wurde.
7. Legen Sie am Ende der ersten Synchronisierung einen Grenzwert fest, der als Startpunkt für die nachfolgenden inkrementellen Synchronisierungen dient.

Einige Anwendungen, z.B. ServiceNow, Google Apps und Box, unterstützen nicht nur die Bereitstellung von Benutzern, sondern auch die Bereitstellung von Gruppen und ihrer Mitglieder. Wenn die Gruppenbereitstellung in den [Zuordnungen](customize-application-attributes.md) aktiviert ist, synchronisiert der Bereitstellungsdienst die Benutzer und Gruppen und anschließend die Gruppenmitgliedschaften. 

### <a name="incremental-syncs"></a>Inkrementelle Synchronisierungen

Nach der ersten Synchronisierung gilt für alle nachfolgenden Synchronisierungen Folgendes:

1. Fragen Sie vom Quellsystem alle Benutzer und Gruppen ab, die aktualisiert wurden, seitdem der letzte Grenzwert gespeichert wurde.
2. Filtern der zurückgegebenen Benutzer und Gruppen, indem alle konfigurierten [Zuweisungen](assign-user-or-group-access-portal.md) oder [attributbasierten Bereichsfilter](define-conditional-rules-for-provisioning-user-accounts.md) verwendet werden.
3. Wenn für einen Benutzer ermittelt wird, dass er zugewiesen ist oder in den Bereich für die Bereitstellung fällt, sendet der Dienst eine Anfrage zu einem übereinstimmenden Benutzer an das Zielsystem, indem die angegebenen [Abgleichattribute](customize-application-attributes.md#understanding-attribute-mapping-properties) verwendet werden.
4. Wenn auf dem Zielsystem kein übereinstimmender Benutzer gefunden wird, wird er erstellt, indem die vom Quellsystem zurückgegebenen Attribute verwendet werden. Nachdem das Benutzerkonto erstellt wurde, erkennt der Bereitstellungsdienst die ID des Zielsystems für den neuen Benutzer, die verwendet wird, um alle zukünftigen Vorgänge für diesen Benutzer auszuführen, und speichert diese zwischen.
5. Falls ein übereinstimmender Benutzer gefunden wird, wird er aktualisiert, indem die vom Quellsystem bereitgestellten Attribute verwendet werden. Nachdem das Benutzerkonto zugeordnet wurde, erkennt der Bereitstellungsdienst die ID des Zielsystems für den neuen Benutzer, die verwendet wird, um alle zukünftigen Vorgänge für diesen Benutzer auszuführen, und speichert diese zwischen.
6. Wenn die Attributzuordnungen „Referenzattribute“ enthalten, führt der Dienst auf dem Zielsystem zusätzliche Updates durch, um die referenzierten Objekte zu erstellen und zu verknüpfen. Ein Benutzer kann auf dem Zielsystem beispielsweise über das Attribut „Manager“ verfügen, das mit einem anderen Benutzer verknüpft ist, der im Zielsystem erstellt wurde.
7. Wenn ein Benutzer, der sich zuvor im Bereich für die Bereitstellung befunden hat, aus dem Bereich entfernt (und die Zuweisung aufgehoben) wird, deaktiviert der Dienst den Benutzer im Zielsystem per Update.
8. Wenn ein Benutzer, der sich zuvor im Bereich für die Bereitstellung befunden hat, im Quellsystem deaktiviert oder vorläufig gelöscht wird, deaktiviert der Dienst den Benutzer im Zielsystem per Update.
9. Wenn ein Benutzer, der sich zuvor im Bereich für die Bereitstellung befunden hat, im Quellsystem endgültig gelöscht wird, löscht der Dienst den Benutzer im Zielsystem. In Azure AD werden Benutzer 30 Tage nach dem vorläufigen Löschen endgültig gelöscht.
10. Legen Sie am Ende der inkrementellen Synchronisierung einen neuen Grenzwert fest, der als Startpunkt für die nachfolgenden inkrementellen Synchronisierungen dient.

>[!NOTE]
> Sie können Erstellungs-, Aktualisierungs- und Löschvorgänge optional deaktivieren, indem Sie die Kontrollkästchen für **Zielobjektaktionen** im Abschnitt [Attributzuordnungen](customize-application-attributes.md) verwenden. Die Logik zum Deaktivieren eines Benutzers während eines Updates wird ebenfalls per Attributzuordnung über ein Feld wie „accountEnabled“ gesteuert.

Der Bereitstellungsdienst fährt damit fort, so lange nacheinander inkrementelle Synchronisierungen durchzuführen (die Intervalle sind in den [einzelnen Tutorials der Anwendungen](../saas-apps/tutorial-list.md) angegeben), bis eines der folgenden Ereignisse eintritt:

* Der Dienst wird mit dem Azure-Portal oder mit dem entsprechenden Graph-API-Befehl manuell beendet. 
* Eine neue erste Synchronisierung wird ausgelöst, indem im Azure-Portal die Option **Clear state and restart** (Status löschen und neu starten) oder der entsprechende Graph-API-Befehl verwendet wird. Hierdurch werden alle gespeicherten Grenzwerte gelöscht, und alle Quellobjekte erneut ausgewertet.
* Eine neue erste Synchronisierung wird aufgrund einer Änderung in den Attributzuordnungen oder Bereichsfiltern ausgelöst. Hierdurch werden ebenfalls alle gespeicherten Grenzwerte gelöscht und alle Quellobjekte erneut ausgewertet.
* Der Bereitstellungsprozess wird aufgrund einer hohen Fehlerrate in Quarantäne versetzt (siehe unten) und verbleibt mehr als vier Wochen lang in diesem Zustand. In diesem Fall wird der Dienst automatisch deaktiviert.

### <a name="errors-and-retries"></a>Fehler und Wiederholungen

Wenn ein einzelner Benutzer im Zielsystem aufgrund eines Fehlers nicht hinzugefügt, aktualisiert oder gelöscht werden kann, wird während des nächsten Synchronisierungszyklus versucht, den Vorgang zu wiederholen. Falls für den Benutzer weiterhin ein Fehler auftritt, wird die Häufigkeit der Wiederholungen allmählich verringert, bis schließlich nur noch ein Versuch pro Tag durchgeführt wird. Zum Beheben des Fehlers müssen Administratoren die [Überwachungspfade](check-status-user-account-provisioning.md) auf Ereignisse vom Typ „Process Escrow“ (Prozesshinterlegung) prüfen, um die Grundursache zu ermitteln, und dann entsprechende Maßnahmen ergreifen. Beispiele für häufig auftretende Fehler sind:

* Für Benutzer ist auf dem Quellsystem ein bestimmtes Attribut nicht angegeben, das auf dem Zielsystem erforderlich ist.
* Benutzer verfügen auf dem Quellsystem über einen Attributwert, für den auf dem Zielsystem eine eindeutige Einschränkung besteht, und der gleiche Wert ist auch in einem anderen Benutzerdatensatz vorhanden.

Diese Fehler können behoben werden, indem die Attributwerte für den betroffenen Benutzer im Quellsystem oder die Attributzuordnungen angepasst werden, um Konflikte zu vermeiden.   

### <a name="quarantine"></a>Quarantäne

Falls die meisten oder alle Aufrufe an das Zielsystem dauerhaft aufgrund eines Fehlers nicht erfolgreich sind (z.B. bei ungültigen Administratoranmeldeinformationen), wird der Bereitstellungsauftrag in den Zustand „Quarantäne“ versetzt. Dies ist im [Zusammenfassungsbericht für die Bereitstellung](check-status-user-account-provisioning.md) angegeben. Wenn im Azure-Portal E-Mail-Benachrichtigungen konfiguriert wurden, wird auch eine E-Mail gesendet. 

In der Quarantäne wird die Häufigkeit der inkrementellen Synchronisierungen allmählich auf einmal pro Tag verringert. 

Die Quarantäne für den Bereitstellungsauftrag wird aufgehoben, nachdem alle relevanten Fehler behoben wurden, und der nächste Synchronisierungszyklus beginnt. Falls der Bereitstellungsauftrag länger als vier Wochen in Quarantäne verbleibt, wird er deaktiviert.


## <a name="how-long-will-it-take-to-provision-users"></a>Wie lange dauert die Bereitstellung von Benutzern?

Die Leistung unterscheidet sich jeweils in Abhängigkeit davon, ob Ihr Bereitstellungsauftrag eine erste oder eine inkrementelle Synchronisierung durchführt, wie im vorherigen Abschnitt beschrieben.

Bei **ersten Synchronisierungen** hängt die Auftragsdauer von einer Vielzahl von Faktoren ab, einschließlich der Anzahl der Benutzer und Gruppen im Umfang der Bereitstellung und der Gesamtzahl der Benutzer und Gruppen im Quellsystem. Eine umfassende Liste der Faktoren, die die Leistung erster Synchronisierungen beeinflussen, finden Sie später in diesem Abschnitt.

Bei **inkrementellen Synchronisierungen** hängt die Auftragsdauer davon ab, wie viele Änderungen im jeweiligen Synchronisierungszyklus erkannt werden. Wenn weniger als 5.000 Änderungen an Benutzern oder Gruppenmitgliedschaften vorhanden sind, kann der Auftrag innerhalb eines einzelnen inkrementellen Synchronisierungszyklus ausgeführt werden. 

Die folgende Tabelle fasst die Synchronisierungsdauer für gängige Bereitstellungsszenarien zusammen. In diesen Szenarien ist das Quellsystem Azure AD und das Zielsystem eine SaaS-Anwendung. Die jeweilige Synchronisierungsdauer ergibt sich aus einer statistischen Analyse der Synchronisierungsaufträge für die SaaS-Anwendungen ServiceNow, Workplace, Salesforce und Google Apps.


| Bereichskonfiguration | Benutzer, Gruppen und Mitglieder im Bereich | Dauer der ersten Synchronisierung | Dauer der inkrementellen Synchronisierung |
| -------- | -------- | -------- | -------- |
| Nur zugewiesene Benutzer und Gruppen synchronisieren |  < 1.000 |  < 30 Minuten | < 30 Minuten |
| Nur zugewiesene Benutzer und Gruppen synchronisieren |  1.000 - 10.000 | 142 - 708 Minuten | < 30 Minuten |
| Nur zugewiesene Benutzer und Gruppen synchronisieren |   10.000 - 100.000 | 1.170 - 2.340 Minuten | < 30 Minuten |
| Alle Benutzer und Gruppen in Azure AD synchronisieren |  < 1.000 | < 30 Minuten  | < 30 Minuten |
| Alle Benutzer und Gruppen in Azure AD synchronisieren |  1.000 - 10.000 | < 30 - 120 Minuten | < 30 Minuten |
| Alle Benutzer und Gruppen in Azure AD synchronisieren |  10.000 - 100.000  | 713 - 1.425 Minuten | < 30 Minuten |
| Alle Benutzer in Azure AD synchronisieren|  < 1.000  | < 30 Minuten | < 30 Minuten |
| Alle Benutzer in Azure AD synchronisieren | 1.000 - 10.000  | 43 - 86 Minuten | < 30 Minuten |


Nur für die Konfiguration **Nur zugewiesene Benutzer und Gruppen synchronisieren** können Sie die folgenden Formeln verwenden, um den ungefähren Mindest- und Höchstwert für die erwartete **Dauer der ersten Synchronisierung** zu bestimmen:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Zusammenfassung der Faktoren, die sich auf die Dauer bis zum Abschluss einer **ersten Synchronisierung** auswirken:

* Die Gesamtanzahl von Benutzern und Gruppen im Bereitstellungsumfang.

* Die Gesamtanzahl von Benutzern, Gruppen und Gruppenmitgliedern im Quellsystem (Azure AD).

* Ob Benutzer im Bereitstellungsumfang mit vorhandenen Benutzern in der Zielanwendung übereinstimmen, oder ob sie zum ersten Mal erstellt werden müssen. Synchronisierungsaufträge, für die alle Benutzer zum ersten Mal angelegt werden, dauern etwa *doppelt so lange* wie Synchronisierungsaufträge, für die alle Benutzer mit vorhandenen Benutzern übereinstimmen.

* Anzahl von Fehlern in den [Überwachungsprotokollen](check-status-user-account-provisioning.md). Die Leistung wird zudem beeinträchtigt, wenn viele Fehler auftreten und der Bereitstellungsdienst in den Quarantänezustand versetzt wurde.    

* Fordern Sie die Einschränkung der Datenübertragungsrate an, die vom Zielsystem implementiert wird. Einige Zielsysteme implementieren Anforderungen von Grenzwerten und Einschränkungen der Datenübertragungsrate, die die Leistung bei umfangreichen Synchronisierungsvorgängen beeinträchtigen können. Unter diesen Bedingungen kann eine App, die zu viele Anforderungen zu schnell empfängt, ihre Antwortrate verlangsamen oder die Verbindung trennen. Zur Leistungssteigerung muss der Connector angepasst werden, sodass er die App-Anforderungen nicht schneller sendet, als die App sie verarbeiten kann. Diese Anpassung ist durch die Bereitstellung von Connectors möglich, die von Microsoft erstellt wurden. 

* Die Anzahl und Größe der zugewiesenen Gruppen. Das Synchronisieren zugewiesener Gruppen dauert länger als das Synchronisieren von Benutzern. Sowohl die Anzahl als auch die Größe der zugewiesenen Gruppen beeinflussen die Leistung. Wenn für eine Anwendung [Zuordnungen für die Synchronisierung von Gruppenobjekten aktiviert sind](customize-application-attributes.md#editing-group-attribute-mappings), werden zusätzlich zu den Benutzern auch Gruppeneigenschaften wie Gruppennamen und Mitgliedschaften synchronisiert. Diese zusätzlichen Synchronisierungen dauern länger als die ausschließliche Synchronisierung von Benutzerobjekten.


## <a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>Wie kann ich feststellen, ob Benutzer ordnungsgemäß bereitgestellt werden?

Alle vom Benutzerbereitstellungsdienst ausgeführten Vorgänge werden in den Azure AD-Überwachungsprotokollen erfasst. Dazu zählen alle Lese- und Schreibvorgänge in den Quell- und Zielsystemen sowie die Angabe, welche Benutzerdaten im Rahmen des jeweiligen Vorgangs gelesen oder geschrieben wurden.

Informationen zum Lesen der Überwachungsprotokolle im Azure-Portal finden Sie in der [Anleitung zur Erstellung von Bereitstellungsberichten](check-status-user-account-provisioning.md).


## <a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>Wie kann ich Probleme bei der Benutzerbereitstellung behandeln?

Einen szenariobasierten Leitfaden zur Problembehandlung bei der automatischen Benutzerbereitstellung finden Sie unter [Probleme bei der Konfiguration und Bereitstellung von Benutzern für eine Anwendung](application-provisioning-config-problem.md).


## <a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>Welche Best Practices gibt es für das Rollout der automatischen Benutzerbereitstellung?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Einen ausführlichen exemplarischen Bereitstellungsplan für die ausgehende Benutzerbereitstellung einer Anwendung finden Sie im [Identitätsbereitstellungsleitfaden für die Benutzerbereitstellung](https://aka.ms/userprovisioningdeploymentplan).

## <a name="more-frequently-asked-questions"></a>Weitere häufig gestellte Fragen

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-b2b-users-in-azure-ad"></a>Funktioniert automatische Benutzerbereitstellung für SaaS-Apps mit B2B-Benutzern in Azure AD?

Ja, der Azure AD-Benutzerbereitstellungsdienst kann zum Bereitstellen von B2B (oder Gast)-Benutzern in Azure AD für SaaS-Anwendungen verwendet werden.

Damit B2B-Benutzer sich bei der SaaS-Anwendung mithilfe von Azure AD anmelden können, muss jedoch die SAML-basierte Funktion der SaaS-Anwendung für einmaliges Anmelden auf eine bestimmte Weise konfiguriert sein. Weitere Informationen zum Konfigurieren von SaaS-Anwendungen, sodass sie Anmeldungen von B2B-Benutzern unterstützen, finden Sie unter [Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit]( https://docs.microsoft.com/azure/active-directory/b2b/configure-saas-apps).

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-dynamic-groups-in-azure-ad"></a>Funktioniert automatische Benutzerbereitstellung für SaaS-Apps mit dynamischen Gruppen in Azure AD?

Ja. Wenn der Azure AD-Benutzerbereitstellungsdienst so konfiguriert ist, dass er „nur zugewiesene Benutzer und Gruppen synchronisiert“, kann er Benutzer in einer SaaS-Anwendung bereitstellen oder ihre Bereitstellung aufheben, basierend darauf, ob sie Mitglieder einer [dynamischen Gruppe](../users-groups-roles/groups-create-rule.md) sind oder nicht. Dynamische Gruppen funktionieren auch mit der Option „Alle Benutzer und Gruppen synchronisieren“.

Die Verwendung dynamischer Gruppen kann jedoch die Gesamtleistung der End-to-End-Benutzerbereitstellung aus dem Azure AD in SaaS-Anwendungen beeinträchtigen. Wenn Sie dynamische Gruppen verwenden, berücksichtigen Sie diese Einschränkungen und Empfehlungen:

* Wie schnell ein Benutzer in einer dynamischen Gruppe bereitgestellt oder seine Bereitstellung in einer SaaS-Anwendung aufgehoben wird, hängt davon ab, wie schnell die dynamische Gruppe Änderungen an der Mitgliedschaft auswerten kann. Informationen, wie Sie den Verarbeitungsstatus einer dynamischen Gruppe überprüfen, finden Sie unter [Überprüfen des Verarbeitungsstatus für eine Mitgliedschaftsregel](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule#check-processing-status-for-a-membership-rule).

* Bei Verwendung dynamischer Gruppen muss bei den Regeln sorgfältig die Benutzerbereitstellung und Aufhebung von Benutzerbereitstellungen berücksichtigt werden, weil ein Verlust der Mitgliedschaft zu einem Aufhebungsereignis der Bereitstellung führt.

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-nested-groups-in-azure-ad"></a>Funktioniert automatische Benutzerbereitstellung für SaaS-Apps mit geschachtelten Gruppen in Azure AD?

 Nein. Wenn der Azure AD-Benutzerbereitstellungsdienst auf „Nur zugewiesene Benutzer und Gruppen synchronisieren“ konfiguriert ist, kann er Benutzer, die sich in geschachtelten Gruppen befinden, weder lesen noch bereitstellen. Er kann nur Benutzer lesen und bereitstellen, die direkte Mitglieder der explizit zugewiesenen Gruppe sind.

Dies ist eine Einschränkung „gruppenbasierter Zuweisungen zu Anwendungen“, die sich auch auf das einmalige Anmelden auswirkt und unter [Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-saasapps ) beschrieben ist.

Als Problemumgehung sollten Sie die Gruppen, die die Benutzer enthalten, die bereitgestellt werden sollen, explizit zuweisen (oder auf andere Weise [den Bereich festlegen](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)).

## <a name="related-articles"></a>Verwandte Artikel

* [Liste der Tutorials zur Integration von SaaS-Apps](../saas-apps/tutorial-list.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](customize-application-attributes.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](functions-for-customizing-application-data.md)
* [Bereichsfilter für die Benutzerbereitstellung](define-conditional-rules-for-provisioning-user-accounts.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](use-scim-to-provision-users-and-groups.md)
* [Azure AD synchronization API overview](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) (Azure AD-Synchronisierung – API-Übersicht)
