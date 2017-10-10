---
title: Bereitstellen von Apps mit Bereichsfiltern | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe von Bereichsfiltern verhindern, dass Objekte in Apps mit automatisierter Benutzerbereitstellung auch dann bereitgestellt werden, wenn ein Objekt Ihre Geschäftsanforderungen nicht erfüllt."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 469246d6cb64d6aaf995ef3b7c4070f8d24372b1
ms.openlocfilehash: e40ba3d3035d5c04017a8ed558981b01fae40a13
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern
In diesem Abschnitt wird die Verwendung von Bereichsfiltern zum Definieren attributbasierter Regeln beschrieben, die festlegen, welche Benutzer der Anwendung bereitgestellt werden.

## <a name="scoping-filter-use-cases"></a>Anwendungsfälle für Bereichsfilter

Mit einem Bereichsfilter kann der Azure AD-Bereitstellungsdienst Benutzer anhand eines Attributs, das mit einem bestimmten Wert übereinstimmt, ein- oder ausschließen. Wenn Sie Benutzer beispielsweise über Azure AD für eine SaaS-Anwendung bereitstellen, die von einem Vertriebsteam genutzt wird, können Sie angeben, dass die Bereitstellung nur für Benutzer mit der Einstellung „Sales“ für das Attribut „Department“ durchgeführt werden soll.

Bereichsfilter werden je nach Typ des Bereitstellungsconnectors unter Umständen unterschiedlich verwendet:

* **Ausgehende Bereitstellung aus Azure AD für SaaS-Anwendungen**: Wenn Azure AD das Quellsystem ist, sind [Benutzer- und Gruppenzuweisungen](active-directory-coreapps-assign-user-azure-portal.md) die häufigste Methode, um zu ermitteln, welche Benutzer im Umfang der Bereitstellung enthalten sind. Diese Zuweisungen werden auch verwendet, um das einmalige Anmelden zu aktivieren und eine zentrale Methode zum Verwalten des Zugriffs und der Bereitstellung zu ermöglichen. Bereichsfilter können optional verwendet werden – zusätzlich zu oder anstelle von Zuweisungen –, um Benutzer basierend auf Attributwerten zu filtern.

>[!TIP]
> Sie können die Bereitstellung basierend auf Zuweisungen für eine Unternehmensanwendung deaktivieren, indem Sie das Menü **[Bereich](active-directory-saas-app-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application)** in den Einstellungen für die Bereitstellung auf **Alle Benutzer und Gruppen synchronisieren** festlegen. Diese Option zusammen mit attributbasierten Bereichsfiltern ermöglicht eine schnellere Leistung als bei Verwendung von gruppenbasierten Zuweisungen.  

* **Eingehende Bereitstellung aus HCM-Anwendungen nach Azure AD und Active Directory**: Wenn eine [HCM-Anwendung, z.B. Workday](active-directory-saas-workday-tutorial.md), das Quellsystem ist, gilt Folgendes: Bereichsfilter sind die Hauptmethode für die Ermittlung, welche Benutzer aus der HCM-Anwendung für Active Directory oder Azure Active Directory bereitgestellt werden sollen.

Standardmäßig sind für Azure AD-Bereitstellungsconnectors keine attributbasierten Bereichsfilter konfiguriert. 

## <a name="scoping-filter-construction"></a>Erstellung von Bereichsfiltern

Ein Bereichsfilter besteht aus einer oder mehreren **Klauseln**. Mit Klauseln wird bestimmt, welche Benutzer die Bereichsfilter passieren dürfen, indem die Attribute der einzelnen Benutzer überprüft werden. Wenn es für eine Klausel beispielsweise erforderlich ist, dass das state-Attribut eines Benutzers „New York“ lautet, werden nur in New York lebende Benutzer in der Anwendung bereitgestellt. 

Mit einer einzelnen Klausel wird eine einzelne Bedingung für einen einzelnen Attributwert definiert. Wenn in einem einzelnen Bereichsfilter mehrere Klauseln erstellt werden, werden sie per „AND“-Logik zusammen ausgewertet. Dies bedeutet, dass die Auswertung für alle Klauseln „true“ ergeben muss, damit ein Benutzer bereitgestellt werden kann.

Für eine einzelne Anwendung können außerdem mehrere Bereichsfilter erstellt werden. Wenn mehrere Bereichsfilter vorhanden sind, werden sie per „OR“-Logik zusammen ausgewertet. Wenn die Auswertung für alle Klauseln der konfigurierten Bereichsfilter „true“ ergibt, wird der Benutzer bereitgestellt.

Jeder Benutzer oder jede Gruppe, der bzw. die vom Azure AD-Bereitstellungsdienst verarbeitet wird, wird für jeden Bereichsfilter immer einzeln ausgewertet.

Betrachten Sie beispielsweise den folgenden Bereichsfilter:

![Bereichsfilter](./media/active-directory-saas-scoping-filters/scoping-filter.PNG) 

Gemäß diesem Bereichsfilter müssen Benutzer die folgenden Kriterien erfüllen, damit sie bereitgestellt werden können:

1. Sie müssen in New York leben.
2. Sie müssen in der Engineeringabteilung arbeiten.
3. Ihre Unternehmensmitarbeiter-ID muss zwischen 1.000.000 und 2.000.000 liegen.
4. Die Position darf nicht null oder leer sein.

## <a name="creating-scoping-filters"></a>Erstellen von Bereichsfiltern
Bereichsfilter werden im Rahmen der Attributzuordnungen für jeden Bereitstellungsconnector für Azure AD-Benutzer konfiguriert. Im Verfahren unten wird angenommen, dass Sie die automatische Bereitstellung für [eine der unterstützten Anwendungen](active-directory-saas-tutorial-list.md) bereits eingerichtet haben und dafür einen Bereichsfilter hinzufügen.

### <a name="to-create-a-scoping-filter"></a>Gehen Sie wie folgt vor, um einen Bereichsfilter zu erstellen:
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.
2. Wählen Sie die Anwendung aus, für die Sie die automatische Bereitstellung konfiguriert haben. Beispiel: „ServiceNow“
3. Wählen Sie die Registerkarte **Bereitstellung**.
4. Wählen Sie im Abschnitt **Zuordnungen** die Zuordnung aus, für die Sie einen Bereichsfilter konfigurieren möchten. Beispiel: „Azure Active Directory-Benutzer für ServiceNow synchronisieren“
5. Wählen Sie das Menü **Quellobjektbereich**.
6. Wählen Sie **Bereichsfilter hinzufügen**.
7. Definieren Sie eine Klausel, indem Sie als Quelle einen **Attributnamen**, einen **Operator** und einen **Attributwert** für den Abgleich auswählen. Unten sind die unterstützten Operatoren aufgeführt:
   * **EQUALS**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut exakt mit dem Wert der Eingabezeichenfolge übereinstimmt (Groß-/Kleinschreibung wird berücksichtigt).
   * **NOT EQUALS**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut nicht mit dem Wert der Eingabezeichenfolge übereinstimmt (Groß-/Kleinschreibung wird berücksichtigt).
   * **IS TRUE**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut den booleschen Wert „true“ enthält.
   * **IS FALSE**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut den booleschen Wert „false“ enthält.
   * **IS NULL**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut leer ist.
   * **IS NOT NULL**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut nicht leer ist.
   * **REGEX MATCH**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut mit einem Muster für reguläre Ausdrücke übereinstimmt. Beispiel: ([1-9][0-9]) ergibt eine Übereinstimmung mit einer beliebigen Zahl zwischen 10 und 99.
   * **NOT REGEX MATCH**: Die Klausel gibt „true“ zurück, wenn das ausgewertete Attribut nicht mit einem Muster für reguläre Ausdrücke übereinstimmt.
8. Wählen Sie die Option **Neue Bereichsklausel hinzufügen**.
9. Optional: Wiederholen Sie die Schritte 7 und 8, um weitere Bereichsklauseln hinzuzufügen.
10. Fügen Sie unter **Bereichsfiltertitel** einen Namen für Ihren Bereichsfilter hinzu.
11. Klicken Sie auf **OK**.
12. Wählen Sie auf dem Bildschirm „Bereichsfilter“ erneut **OK** (oder wiederholen Sie die Schritte 6 bis 11, um einen weiteren Bereichsfilter hinzuzufügen).
13. Wählen Sie auf dem Bildschirm „Attributzuordnung“ die Option **Speichern**. 

>[!IMPORTANT] 
> Beim Speichern eines neuen Bereichsfilters wird eine neue vollständige Synchronisierung für die Anwendung ausgelöst, bei der alle Benutzer im Quellsystem für den neuen Bereichsfilter neu ausgewertet werden. Wenn ein Benutzer der Anwendung zuvor Bereitstellungsempfänger war und sich dies ändert, wird sein Konto in der Anwendung deaktiviert (bzw. die Bereitstellung aufgehoben).


## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen](active-directory-saas-app-provisioning.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](active-directory-saas-customizing-attribute-mappings.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kontobereitstellungsbenachrichtigungen](active-directory-saas-account-provisioning-notifications.md)
* [Verwenden von SCIM für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen](active-directory-scim-provisioning.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)


