---
title: Anpassen von Azure AD-Attributzuordnungen | Microsoft-Dokumentation
description: Erfahren Sie, was Attributzuordnungen für SaaS-Apps in Azure Active Directory sind und wie Sie sie an Ihre geschäftlichen Anforderungen anpassen können.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: barbkess
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: accc2e994e7ea361315d5dfb33b257e58410490c
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2018
ms.locfileid: "42140170"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Anpassen von Attributzuordnungen für die Benutzerbereitstellung für SaaS-Anwendungen in Azure Active Directory
Microsoft Azure AD bietet Unterstützung für die Benutzerbereitstellung für SaaS-Anwendungen von Drittanbietern, z. B. Salesforce, Google Apps usw. Wenn Sie die Benutzerbereitstellung für eine SaaS-Anwendung eines Drittanbieters aktiviert haben, steuert das Azure-Portal deren Attributwerte mithilfe von Attributzuordnungen.

Es ist eine vorkonfigurierte Sammlung von Attributen und Attributzuordnungen zwischen Azure AD-Benutzerobjekten und den Benutzerobjekten der einzelnen SaaS-Apps verfügbar. Einige Apps verwalten neben Benutzern andere Objekttypen wie beispielsweise Gruppen. <br> 
 Sie können die Standardattributzuordnungen den Anforderungen Ihres Unternehmens entsprechend anpassen. Dies bedeutet, dass Sie vorhandene Attributzuordnungen ändern oder löschen und neue Attributzuordnungen erstellen können.
 
## <a name="editing-user-attribute-mappings"></a>Bearbeiten von Benutzerattributzuordnungen

Im Azure AD-Portal können Sie auf dieses Feature zugreifen, indem Sie auf eine **Zuordnungen**-Konfiguration unter **Bereitstellung** im Abschnitt **Verwalten** einer **Unternehmensanwendung** klicken.


![Salesforce][5] 

Wenn Sie auf eine **Zuordnungskonfiguration** klicken, wird der zugehörige Bildschirm **Attributzuordnung** geöffnet. Es gibt Attributzuordnungen, die erforderlich sind, damit eine SaaS-Anwendung ordnungsgemäß funktioniert. Für die erforderlichen Attribute ist das Feature **Löschen** nicht verfügbar.


![Salesforce][6]  

Im Beispiel oben können Sie sehen, dass das Attribut **Benutzername** eines verwalteten Objekts in Salesforce mit dem Wert **userPrincipalName** des verknüpften Azure AD-Objekts aufgefüllt wird.

Sie können vorhandene **Attributzuordnungen** durch Klicken auf eine Zuordnung anpassen. Daraufhin wird der Bildschirm **Attribut bearbeiten** geöffnet.

![Salesforce][7]  


### <a name="understanding-attribute-mapping-types"></a>Grundlegendes zu Attributzuordnungstypen
Mit Attributzuordnungen steuern Sie, wie die Attribute in einer SaaS-Anwendung eines Drittanbieters mit Daten aufgefüllt werden. Vier verschiedene Zuordnungstypen werden unterstützt:

* **Direkt** : Das Zielattribut wird mit dem Wert eines Attributs des verknüpften Objekts in Azure AD aufgefüllt.
* **Konstant** : Das Zielattribut wird mit einer bestimmten Zeichenfolge, die Sie angegeben haben, aufgefüllt.
* **Ausdruck** : Das Zielattribut wird abhängig vom Ergebnis eines skriptähnlichen Ausdrucks mit Daten aufgefüllt. 
  Weitere Informationen finden Sie unter [Schreiben von Ausdrücken für Attributzuordnungen in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Kein** : Das Zielattribut bleibt unverändert. Wenn das Zielattribut allerdings leer ist, wird es mit dem von Ihnen angegebenen Standardwert aufgefüllt.

Zusätzlich zu diesen vier Basistypen unterstützen benutzerdefinierte Attributzuordnungen das Konzept einer optionalen Zuordnung von **Standardwerten**. Die Standardwertzuordnung stellt sicher, dass ein Zielattribut mit einem Wert aufgefüllt wird, wenn weder in Azure AD noch für das Zielobjekt ein Wert vorhanden ist. Bei der üblichen Konfiguration bleibt dieses Feld leer.


### <a name="understanding-attribute-mapping-properties"></a>Grundlegendes zu Attributzuordnungseigenschaften

Im vorherigen Abschnitt wurde bereits die Attributzuordnungstyp-Eigenschaft eingeführt.
Zusätzlich zu dieser Eigenschaft unterstützen Attributzuordnungen auch die folgenden Attribute:

- **Quellattribut**: Das Benutzerattribut aus dem Quellsystem (beispielsweise Azure Active Directory).
- **Zielattribut**: Das Benutzerattribut im Zielsystem (beispielsweise ServiceNow).
- **Objekte mit diesem Attribut abgleichen**: Gibt an, ob diese Zuordnung zum eindeutigen Bestimmen von Benutzern zwischen Quell- und Zielsystem verwendet werden soll. Dies wird in der Regel mit dem userPrincipalName- oder mail-Attribut in Azure AD festgelegt, das in der Regel einem Benutzernamenfeld in einer Zielanwendung zugeordnet ist.
- **Rangfolge für Abgleich**: Es können mehrere Attribute für den Abgleich festgelegt werden. Falls mehrere vorhanden sind, werden sie entsprechend der in diesem Feld festgelegten Reihenfolge ausgewertet. Sobald eine Übereinstimmung gefunden wird, werden keine weiteren Attribute für den Abgleich mehr ausgewertet.
- **Diese Zuordnung anwenden**
    - **Immer**: Wenden Sie diese Zuordnung sowohl bei der Aktion zum Erstellen eines Benutzers als auch bei der zum Aktualisieren eines Benutzers an.
    - **Nur während der Erstellung**: Wenden Sie diese Zuordnung nur bei der Aktion zum Erstellen eines Benutzers an.


## <a name="editing-group-attribute-mappings"></a>Bearbeiten von Gruppenattributzuordnungen

Einige ausgewählte Anwendungen (z. B. ServiceNow, Box und Google Apps) bieten die Möglichkeit, neben Benutzerobjekten auch Gruppenobjekte bereitzustellen. Gruppenobjekte können zusätzlich zu den Gruppenmitgliedern Gruppeneigenschaften wie Anzeigenamen und E-Mail-Aliase enthalten.

![ServiceNow][8]  

Die Gruppenbereitstellung kann optional aktiviert oder deaktiviert werden, indem Sie die Gruppenzuordnung unter **Zuordnungen** auswählen und die Einstellung **Aktiviert** auf dem Bildschirm **Attributzuordnung** wie gewünscht festlegen.

Die Attribute, die zusammen mit den Gruppenobjekten bereitgestellt werden, können wie Benutzerobjekte angepasst werden (siehe obige Beschreibung). 

>[!TIP]
>Die Bereitstellung von Gruppenobjekten (Eigenschaften und Mitglieder) unterscheidet sich vom Konzept der [Zuweisung von Gruppen](manage-apps/assign-user-or-group-access-portal.md) zu einer Anwendung. Es ist möglich, eine Gruppe einer Anwendung zuzuweisen, aber nur die in der Gruppe enthaltenen Benutzerobjekte bereitzustellen. Die Bereitstellung vollständiger Gruppenobjekte ist nicht erforderlich, um Gruppen in Zuweisungen verwenden zu können.


## <a name="editing-the-list-of-supported-attributes"></a>Bearbeiten der Liste unterstützter Attribute

Die unterstützen Attribute für eine bestimmte Anwendung sind vorkonfiguriert. Die meisten Benutzerverwaltungs-APIs von Anwendungen unterstützen die Schemaerkennung nicht. Der Azure AD-Bereitstellungsdienst kann die Liste der unterstützten Attribute daher nicht dynamisch durch Aufrufe an die Anwendung generieren. 

Einige Anwendungen unterstützen jedoch benutzerdefinierte Attribute. Damit der Azure AD-Bereitstellungsdienst benutzerdefinierte Attribute lesen und schreiben kann, müssen ihre Definitionen im Azure-Portal eingegeben werden. Verwenden Sie dazu das Kontrollkästchen **Erweiterte Optionen anzeigen** unten auf dem Bildschirm **Attributzuordnung**.

Folgende Anwendungen und Systeme unterstützen die Anpassung der Attributliste:

* Salesforce
* ServiceNow
* Workday
* Azure Active Directory ([Azure AD Graph-API-Standardattribute](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) und benutzerdefinierte Verzeichniserweiterungen werden unterstützt)
* Apps, die [SCIM 2.0](https://tools.ietf.org/html/rfc7643) unterstützen und das Hinzufügen von Attributen erfordern, die im [Kernschema](https://tools.ietf.org/html/rfc7643) definiert sind

>[!NOTE]
>Das Bearbeiten der Liste unterstützter Attribute wird nur für Administratoren empfohlen, die das Schema ihrer Anwendungen und Systeme angepasst haben und aus erster Hand wissen, wie ihre benutzerdefinierten Attribute definiert wurden. Dies erfordert mitunter Kenntnisse der von einer Anwendung oder einem System bereitgestellten APIs und Entwicklertools. 

![Editor][9]  

Beim Bearbeiten der Liste unterstützter Attribute sind die folgenden Eigenschaften verfügbar:

* **Name**: Der im Schema des Zielobjekts definierte Systemname des Attributs. 
* **Typ**: Der im Schema des Zielobjekts definierte Datentyp, den das Attribut speichert. Mögliche Werte:
   * *Binary* – Das Attribut enthält Binärdaten.
   * *Boolean* – Das Attribut enthält einen Wert „True“ oder „False“.
   * *DateTime* – Das Attribut enthält eine Datumszeichenfolge.
   * *Integer* – Das Attribut enthält eine ganze Zahl.
   * *Reference* – Das Attribut enthält eine ID, die auf einen in einer anderen Tabelle in der Zielanwendung gespeicherten Wert verweist.
   * *String* – Das Attribut enthält eine Textzeichenfolge. 
* **Primärschlüssel?**: Gibt an, ob das Attribut als Primärschlüsselfeld im Schema des Zielobjekts definiert ist.
* **Erforderlich?** Gibt an, ob das Attribut in der Zielanwendung oder im Zielsystem aufgefüllt werden muss.
* **Mehrwertig?**: Gibt an, ob das Attribut mehrere Werte unterstützt.
* **Exact case?** (Groß-/Kleinschreibung beachten?): Gibt an, ob die Attributwerte unter Berücksichtigung der Groß-/Kleinschreibung ausgewertet werden.
* **API-Ausdruck**: Verwenden Sie diese Eigenschaft nur, wenn Sie in der Dokumentation für einen bestimmten Bereitstellungsconnector (z. B. Workday) dazu angewiesen werden.
* **Referenced Object Attribute** (Referenzierter Objekttyp): Im Fall eines Attributs vom Typ „Reference“ können Sie in diesem Menü die Tabelle und das Attribut in der Zielanwendung auswählen, die den zugehörigen Wert für das Attribut enthält. Bei einem Attribut mit dem Namen „Abteilung“, dessen gespeicherter Wert auf ein Objekt in einer separaten Tabelle „Abteilungen“ verweist, würden Sie beispielsweise „Abteilungen.Name“ auswählen. Beachten Sie, dass die unterstützten Verweistabellen und Felder für die primäre ID für eine bestimmte Anwendung vorkonfiguriert sind und derzeit nicht im Azure-Portal, aber mit der [Graph-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes) bearbeitet werden können.

Um ein neues Attribut hinzuzufügen, scrollen Sie zum Ende der Liste der unterstützten Attribute, füllen Sie die obigen Felder anhand der verfügbaren Eingaben auf, und klicken Sie auf **Attribut hinzufügen**. Klicken Sie nach dem Hinzufügen von Attributen auf **Speichern**. Sie müssen die Registerkarte **Bereitstellung** anschließend erneut laden, damit die neuen Attribute im Attributzuordnungs-Editor verfügbar werden.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Wiederherstellen der Standardattribute und Attributzuordnungen

Sollten Sie noch einmal von vorn anfangen und Ihre vorhandenen Zuordnungen auf den Standardzustand zurücksetzen müssen, können Sie das Kontrollkästchen **Standardzuordnungen wiederherstellen** aktivieren und die Konfiguration speichern. Dadurch werden alle Zuordnungen auf den Zustand zurückgesetzt, den sie aufweisen, wenn die Anwendung dem Azure AD-Mandanten gerade aus dem Anwendungskatalog hinzugefügt wurde. 

Durch die Auswahl dieser Option wird eine erneute Synchronisierung aller Benutzer erzwungen, während der Bereitstellungsdienst ausgeführt wird. 

>[!IMPORTANT]
>Es wird dringend empfohlen, vor dem Aufrufen dieser Option **Bereitstellungsstatus** auf **Aus** festzulegen.


## <a name="what-you-should-know"></a>Wichtige Informationen

* Microsoft Azure AD stellt eine effiziente Implementierung eines Synchronisierungsprozesses zur Verfügung. In einer initialisierten Umgebung werden nur Objekte, die eine Aktualisierung erfordern, während eines Synchronisierungszyklus verarbeitet. 

* Das Aktualisieren von Attributzuordnungen hat Auswirkungen auf die Leistung eines Synchronisierungszyklus. Nach einer Aktualisierung der Attributzuordnungskonfiguration müssen alle verwalteten Objekte erneut ausgewertet werden. 

* Es hat sich bewährt, die Anzahl der aufeinanderfolgenden Änderungen an Attributzuordnungen so gering wie möglich zu halten.


## <a name="next-steps"></a>Nächste Schritte

* [Automatisieren der Bereitstellung/Bereitstellungsaufhebung von Benutzern für SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereichsfilter für die Benutzerbereitstellung](active-directory-saas-scoping-filters.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](manage-apps/use-scim-to-provision-users-and-groups.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](saas-apps/tutorial-list.md)

<!--Image references-->
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png
[8]: ./media/active-directory-saas-customizing-attribute-mappings/24.png
[9]: ./media/active-directory-saas-customizing-attribute-mappings/25.PNG

