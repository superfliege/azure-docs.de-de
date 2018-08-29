---
title: 'Tutorial: Konfigurieren von Workday für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Workday konfigurieren.
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: asmalser
ms.openlocfilehash: 0df23d50fa208482e45d2d35555ec79c587cc80a
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42445659"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-preview"></a>Tutorial: Konfigurieren von Workday für die automatische Benutzerbereitstellung (Vorschau)

In diesem Tutorial werden die Schritte vorgestellt, die Sie zum Importieren von Personen aus Workday in sowohl Active Directory als auch Azure Active Directory ausführen müssen, wobei einige Attribute optional in Workday zurückgeschrieben werden.

## <a name="overview"></a>Übersicht

Der [Azure Active Directory-Benutzerbereitstellungsdienst](../active-directory-saas-app-provisioning.md) ist zum Bereitstellen von Benutzerkonten mit der [Workday Human Resources-API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) integriert. Azure AD nutzt diese Verbindung zum Ermöglichen der folgenden Workflows für die Benutzerbereitstellung:

* **Bereitstellung von Benutzern in Active Directory**: Sie können ausgewählte Gruppen von Benutzern aus Workday in eine oder mehrere Active Directory-Gesamtstrukturen synchronisieren.

* **Bereitstellung reiner Cloudbenutzer in Azure Active Directory**: In Szenarien, in denen kein lokales Active Directory verwendet wird, können Benutzer mithilfe des Azure AD-Benutzerbereitstellungsdiensts direkt aus Workday in Azure Active Directory bereitgestellt werden. 

* **Zurückschreiben von E-Mail-Adressen in Workday**: Der Azure AD-Benutzerbereitstellungsdienst kann die E-Mail-Adressen von Azure AD-Benutzern in Workday zurückschreiben. 

### <a name="what-human-resources-scenarios-does-it-cover"></a>Welche Szenarien im Personalwesen werden unterstützt?

Die vom Azure AD-Benutzerbereitstellungsdienst unterstützten Workday-Workflows zur Benutzerbereitstellung ermöglichen die Automatisierung der folgenden Szenarien im Personalwesen und bei der Verwaltung des Lebenszyklus von Identitäten:

* **Einstellung neuer Mitarbeiter**: Wenn Workday ein neuer Mitarbeiter hinzugefügt wird, wird in Active Directory, Azure Active Directory und optional Office 365 sowie [anderen von Azure AD unterstützten SaaS-Anwendungen](../active-directory-saas-app-provisioning.md) automatisch ein Benutzerkonto erstellt. Die E-Mail-Adresse wird dabei in Workday zurückgeschrieben.

* **Aktualisierungen von Mitarbeiterattributen und -profil**: Wenn ein Mitarbeiterdatensatz in Workday aktualisiert wird (wie z.B. der Name, Titel oder Vorgesetzte), wird das entsprechende Benutzerkonto in Active Directory, Azure Active Directory und optional Office 365 und [anderen von Azure AD unterstützten SaaS-Anwendungen](../active-directory-saas-app-provisioning.md) automatisch aktualisiert.

* **Kündigungen von Mitarbeitern** : Wenn einem Mitarbeiter in Workday gekündigt wird, wird das entsprechende Benutzerkonto in Active Directory, Azure Active Directory und optional Office 365 und [anderen von Azure AD unterstützten SaaS-Anwendungen](../active-directory-saas-app-provisioning.md) automatisch deaktiviert.

* **Erneute Einstellung von Mitarbeitern**: Wenn ein Mitarbeiter in Workday erneut eingestellt wird, kann sein altes Konto in Active Directory, Azure Active Directory und optional Office 365 und [anderen von Azure AD unterstützten SaaS-Anwendungen](../active-directory-saas-app-provisioning.md) automatisch reaktiviert oder erneut bereitgestellt werden (je nachdem, was Sie bevorzugen).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Für wen ist diese Benutzerbereitstellungslösung am besten geeignet?

Diese Workday-Benutzerbereitstellungslösung ist derzeit als öffentliche Vorschauversion verfügbar und eignet sich ideal für:

* Organisationen, die eine vorgefertigte, cloudbasierte Lösung für die Workday-Benutzerbereitstellung verwenden möchten

* Organisationen, bei denen Benutzer direkt aus Workday in Active Directory oder Azure Active Directory bereitgestellt werden müssen

* Organisationen, bei denen Benutzer mithilfe von Daten bereitgestellt werden müssen, die aus dem HCM-Modul von Workday abgerufen werden (siehe [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organisationen, bei denen Benutzer beim Beitreten, Verschieben und Verlassen nur auf Grundlage von Änderungsinformationen, die im HCM-Modul von Workday erkannt werden, mit einer oder mehreren Active Directory-Gesamtstrukturen, -Domänen und -Organisationseinheiten synchronisiert werden müssen (siehe [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organisationen, die Office 365 für E-Mail verwenden

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="planning-your-solution"></a>Planen der Lösung

Überprüfen Sie vor Beginn der Workday-Integration die folgenden Voraussetzungen, und lesen Sie die folgende Anleitung zum Erfüllen Ihrer aktuellen Anforderungen an die Active Directory-Architektur und -Benutzerbereitstellung mithilfe der von Azure Active Directory gebotenen Lösungen.

### <a name="prerequisites"></a>Voraussetzungen

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Gültiges Azure AD Premium P1-Abonnement mit globalem Administratorzugriff
* Workday-Implementierungsmandant für Test- und Integrationszwecke
* Administratorberechtigungen in Workday zum Erstellen eines Systemintegrationsbenutzers für Testzwecke und Vornehmen von Änderungen zum Testen von Mitarbeiterdaten
* Für die Benutzerbereitstellung in Active Directory einen der Domäne beigetretenen Server mit mindestens Windows Server 2012 zum Hosten des [lokalen Synchronisierungs-Agents](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](../connect/active-directory-aadconnect.md) für die Synchronisierung zwischen Active Directory und Azure AD

### <a name="solution-architecture"></a>Lösungsarchitektur

Azure AD bietet einen umfangreichen Satz von Bereitstellungsconnectors, die Sie bei der Bereitstellung und der Verwaltung des Identitätslebenszyklus zwischen Workday und Active Directory, Azure AD, SaaS-Apps usw. unterstützen. Die zu verwendenden Features und die Einrichtung der Lösung variieren abhängig von der Umgebung und den Anforderungen Ihrer Organisation. Führen Sie als ersten Schritt eine Bestandsaufnahme durch, welche der folgenden Elemente in Ihrer Organisation vorhanden und bereitgestellt sind:

* Wie viele Active Directory-Gesamtstrukturen werden verwendet?
* Wie viele Active Directory-Domänen werden verwendet?
* Wie viele Active Directory-Organisationseinheiten (OUs) werden verwendet?
* Wie viele Azure Active Directory-Mandanten werden verwendet?
* Gibt es Benutzer, die sowohl in Active Directory als auch in Azure Active Directory bereitgestellt werden müssen (z.B. als „hybride“ Benutzer)?
* Gibt es Benutzer, die zwar in Azure Active Directory, aber nicht in Active Directory bereitgestellt werden müssen (z.B. „reine Cloudbenutzer“)?
* Müssen E-Mail-Adressen von Benutzern in Workday zurückgeschrieben werden?

Sobald Sie die Antworten auf diese Fragen kennen, können Sie Ihre Workday-Bereitstellung gemäß der folgenden Anleitung planen.

#### <a name="using-provisioning-connector-apps"></a>Verwenden von Bereitstellungsconnector-Apps

Azure Active Directory unterstützt vorintegrierte Bereitstellungsconnectors für Workday und eine große Anzahl von SaaS-Anwendungen.

Ein einzelner Bereitstellungsconnector kommuniziert mit der API eines einzelnen Quellsystems und hilft bei der Bereitstellung von Daten in einem einzelnen Zielsystem. Die meisten Bereitstellungsconnectors, die Azure AD unterstützt, sind für ein einzelnes Quell- und Zielsystem vorgesehen (z.B. Azure AD zu ServiceNow) und können eingerichtet werden, indem die betreffende App (etwa ServiceNow) aus dem Azure AD-App-Katalog hinzugefügt wird.

Es gibt eine direkte Beziehung zwischen Bereitstellungsconnectorinstanzen und App-Instanzen in Azure AD:

| Quellsystem | Zielsystem |
| ---------- | ---------- |
| Azure AD-Mandant | SaaS-Anwendung |

Beim Arbeiten mit Workday und Active Directory müssen jedoch mehrere Quell- und Zielsysteme berücksichtigt werden:

| Quellsystem | Zielsystem | Notizen |
| ---------- | ---------- | ---------- |
| Workday | Active Directory-Gesamtstruktur | Jede Gesamtstruktur wird als eigenes Zielsystem behandelt. |
| Workday | Azure AD-Mandant | Entsprechend dem Bedarf reiner Cloudbenutzer. |
| Active Directory-Gesamtstruktur | Azure AD-Mandant | Dieser Flow erfolgt derzeit über AAD Connect. |
| Azure AD-Mandant | Workday | Für das Zurückschreiben von E-Mail-Adressen. |

Um diese zahlreichen Workflows zu mehreren Quell- und Zielsystemen zu erleichtern, bietet Azure AD mehrere Bereitstellungsconnector-Apps, die über den Azure AD-App-Katalog hinzugefügt werden können:

![AAD-App-Katalog](./media/workday-inbound-tutorial/WD_Gallery.PNG)

* **Workday to Active Directory Provisioning**: Diese App vereinfacht die Bereitstellung von Benutzerkonten aus Workday in eine einzelne Active Directory-Gesamtstruktur. Wenn Sie über mehrere Gesamtstrukturen verfügen, können Sie eine Instanz dieser App aus dem Azure AD-App-Katalog für jede Active Directory-Gesamtstruktur hinzufügen, in der die Bereitstellung erfolgen soll.

* **Workday to Azure AD Provisioning**: Wenngleich AAD Connect das Tool ist, das Sie zum Synchronisieren von Active Directory-Benutzern mit Azure Active Directory verwenden sollten, können Sie auch diese App nutzen, um die Bereitstellung reiner Cloudbenutzer aus Workday in einem einzelnen Azure Active Directory-Mandanten zu erleichtern.

* **Workday Writeback**: Diese App vereinfacht das Zurückschreiben von E-Mail-Adressen von Benutzern aus Azure Active Directory in Workday.

> [!TIP]
> Die reguläre „Workday“-App dient zum Einrichten des einmaligen Anmeldens zwischen Workday und Azure Active Directory. 

Das Einrichten und Konfigurieren dieser spezielle Bereitstellungsconnector-Apps ist das Thema in den verbleibenden Abschnitten dieses Tutorials. Welche Apps Sie konfigurieren, hängt davon ab, in welchen Systemen die Bereitstellung erfolgen soll und wie viele Active Directory-Gesamtstrukturen und Azure AD-Mandanten in Ihrer Umgebung vorhanden sind.

![Übersicht](./media/workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Konfigurieren eines Systemintegrationsbenutzers in Workday
Eine gängige Anforderung an alle Workday-Bereitstellungsconnectors sind Anmeldeinformationen für ein Workday-Systemintegrationskonto, mit dem eine Verbindung mit der Workday Human Resources-API hergestellt wird. In diesem Abschnitt wird das Erstellen eines Systemintegrationskontos in Workday beschrieben.

> [!NOTE]
> Sie können diesen Schritt auslassen und stattdessen ein globales Workday-Administratorkonto als Systemintegrationskonto nutzen. Diese Vorgehensweise ist für Demos einwandfrei, wird jedoch für Produktionsbereitstellungen nicht empfohlen.

### <a name="create-an-integration-system-user"></a>Erstellen eines Integrationssystembenutzers

**So erstellen Sie einen Integrationssystembenutzer**

1. Melden Sie sich mithilfe eines Administratorkontos bei Ihrem Workday-Mandanten an. Geben Sie in der **Workday-Workbench** die Suchzeichenfolge „Benutzer erstellen“ in das Suchfeld ein, und klicken Sie dann auf den Link **Integrationssystembenutzer erstellen**.

    ![Benutzer erstellen](./media/workday-inbound-tutorial/IC750979.png "Benutzer erstellen")
2. Führen Sie die Aufgabe **Integrationssystembenutzer erstellen** aus, indem Sie einen Benutzernamen und ein Kennwort für einen neuen Integrationssystembenutzer angeben.  
 * Lassen Sie das Kontrollkästchen **Bei der nächsten Anmeldung neues Kennwort anfordern** deaktiviert. Dieser Benutzer meldet sich programmgesteuert an.
 * Übernehmen Sie für **Sitzungstimeout in Minuten** den Standardwert 0. Diese Einstellung verhindert, dass Sitzungen des Benutzers vorzeitig beendet werden.

    ![Integrationssystembenutzer erstellen](./media/workday-inbound-tutorial/IC750980.png "Integrationssystembenutzer erstellen")

### <a name="create-a-security-group"></a>Erstellen einer Sicherheitsgruppe
Sie müssen eine uneingeschränkte Integrationssystem-Sicherheitsgruppe erstellen und den Benutzer dieser Gruppe zuweisen.

**So erstellen Sie eine Sicherheitsgruppe**

1. Geben Sie „Sicherheitsgruppe erstellen“ in das Suchfeld ein, und klicken Sie dann auf **Sicherheitsgruppe erstellen**.

    ![Sicherheitsgruppe erstellen](./media/workday-inbound-tutorial/IC750981.png "Sicherheitsgruppe erstellen")
2. Führen Sie die Aufgabe **Sicherheitsgruppe erstellen** aus.  
3. Wählen Sie in der Dropdownliste **Type of Tenanted Security Group** (Typ der Mandantensicherheitsgruppe) die Option **Integration System Security Group (Unconstrained)** (Integrationssystem-Sicherheitsgruppe – uneingeschränkt) aus.
4. Erstellen Sie eine Sicherheitsgruppe, der die Mitglieder explizit hinzugefügt werden.

    ![Sicherheitsgruppe erstellen](./media/workday-inbound-tutorial/IC750982.png "Sicherheitsgruppe erstellen")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>Zuweisen des Integrationssystembenutzers zur Sicherheitsgruppe

**So weisen Sie den Integrationssystembenutzer zu**

1. Geben Sie „Sicherheitsgruppe bearbeiten“ in das Suchfeld ein, und klicken Sie dann auf **Sicherheitsgruppe bearbeiten**.

    ![Sicherheitsgruppe bearbeiten](./media/workday-inbound-tutorial/IC750983.png "Sicherheitsgruppe bearbeiten")
1. Suchen Sie anhand des Namens nach der neuen Integrationssicherheitsgruppe, und wählen Sie sie aus.

    ![Sicherheitsgruppe bearbeiten](./media/workday-inbound-tutorial/IC750984.png "Sicherheitsgruppe bearbeiten")
2. Fügen Sie den neuen Integrationssystembenutzer der neuen Sicherheitsgruppe hinzu. 

    ![Systemsicherheitsgruppe](./media/workday-inbound-tutorial/IC750985.png "Systemsicherheitsgruppe")  

### <a name="configure-security-group-options"></a>Konfigurieren von Sicherheitsgruppenoptionen
In diesem Schritt gewähren Sie der Sicherheitsgruppe Berechtigungen für Domänensicherheitsrichtlinien für die Mitarbeiterdaten.

**So konfigurieren Sie Sicherheitsgruppenoptionen**

1. Geben Sie **Domänensicherheitsrichtlinien** in das Suchfeld ein, und klicken Sie dann auf den Link **Domain Security Policies for Functional Area** (Domänensicherheitsrichtlinien für Funktionsbereich).  

    ![Domänensicherheitsrichtlinien](./media/workday-inbound-tutorial/IC750986.png "Domänensicherheitsrichtlinien")  
2. Suchen Sie nach "System", und wählen Sie den Funktionsbereich **System** aus.  Klicken Sie auf **OK**.  

    ![Domänensicherheitsrichtlinien](./media/workday-inbound-tutorial/IC750987.png "Domänensicherheitsrichtlinien")  
3. Erweitern Sie in der Liste der Sicherheitsrichtlinien für den Funktionsbereich „System“ den Eintrag **Sicherheitsverwaltung**, und wählen Sie die Domänensicherheitsrichtlinie **Externe Kontobereitstellung** aus.  

    ![Domänensicherheitsrichtlinien](./media/workday-inbound-tutorial/IC750988.png "Domänensicherheitsrichtlinien")  
1. Klicken Sie auf **Berechtigungen bearbeiten**, und fügen Sie die neue Sicherheitsgruppe auf der Dialogfeldseite **Berechtigungen bearbeiten** zur Liste der Sicherheitsgruppen mit Berechtigungen für die **Get**- und **Put**-Integration hinzu.

    ![Berechtigung bearbeiten](./media/workday-inbound-tutorial/IC750989.png "Berechtigung bearbeiten")  

1. Wiederholen Sie die obigen Schritte 1 bis 4 für jede der folgenden verbleibenden Sicherheitsrichtlinien:

| Vorgang | Domänensicherheitsrichtlinie |
| ---------- | ---------- | 
| Get und Put | Mitarbeiterdaten: öffentliche Mitarbeiterberichte |
| Get und Put | Mitarbeiterdaten: Kontaktinformationen von Mitarbeitern |
| Get | Mitarbeiterdaten: alle Positionen |
| Get | Mitarbeiterdaten: aktuelle Personalinformationen |
| Get | Mitarbeiterdaten: Berufsbezeichnung in Mitarbeiterprofil |


### <a name="activate-security-policy-changes"></a>Aktivieren von Sicherheitsrichtlinienänderungen

**So aktivieren Sie Sicherheitsrichtlinienänderungen**

1. Geben Sie „aktivieren“ in das Suchfeld ein, und klicken Sie dann auf den Link **Ausstehende Sicherheitsrichtlinienänderungen aktivieren**.

    ![Aktivieren](./media/workday-inbound-tutorial/IC750992.png "Aktivieren") 
2. Geben Sie zum Ausführen der Aufgabe „Ausstehende Sicherheitsrichtlinienänderungen aktivieren“ zunächst einen Kommentar für Überwachungszwecke ein, und klicken Sie dann auf die Schaltfläche **OK**. 

    ![Ausstehende Sicherheitsrichtlinienänderungen aktivieren](./media/workday-inbound-tutorial/IC750993.png "Ausstehende Sicherheitsrichtlinienänderungen aktivieren")  
1. Führen Sie die Aufgabe auf dem nächsten Bildschirm aus, indem Sie das Kontrollkästchen **Bestätigen** aktivieren und auf **OK** klicken.

    ![Ausstehende Sicherheitsrichtlinienänderungen aktivieren](./media/workday-inbound-tutorial/IC750994.png "Ausstehende Sicherheitsrichtlinienänderungen aktivieren")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Konfiguration der Benutzerbereitstellung aus Workday in Active Directory
Befolgen Sie diese Anweisungen zum Konfigurieren der Bereitstellung von Benutzerkonten aus Workday in den einzelnen Active Directory-Gesamtstrukturen, in denen die Bereitstellung erfolgen soll.

### <a name="planning"></a>Planung

Beantworten Sie die folgenden Fragen, bevor Sie die Benutzerbereitstellung in einer Active Directory-Gesamtstruktur konfigurieren. Die Antworten auf diese Fragen bestimmen, wie Ihre Bereichsfilter und Attributzuordnungen festgelegt werden müssen. 

* **Welche Benutzer in Workday müssen in dieser Active Directory-Gesamtstruktur bereitgestellt werden?**

   * *Beispiel: Benutzer, bei denen das Workday-Attribut „Company“ den Wert „Contoso“ und das Attribut „Worker_Type“ den Wert „Regular“ enthält*

* **Wie werden Benutzer in verschiedene Organisationseinheiten (OEs) weitergeleitet?**

   * *Beispiel: Benutzer werden wie in den Workday-Attributen „Municipality“ und „Country_Region_Reference“ definiert zu Organisationseinheiten weitergeleitet, die einem Bürostandort entsprechen*

* **Wie müssen die folgenden Attribute in Active Directory aufgefüllt werden?**

   * Allgemeiner Name (cn)
      * *Beispiel: Verwenden Sie den von der Personalabteilung festgelegten Workday-Wert „User_ID“.*
      
   * Mitarbeiter-ID (employeeId)
      * *Beispiel: Verwenden Sie den Workday-Wert „Worker_ID“.*
      
   * SAM-Kontoname (sAMAccountName)
      * *Beispiel: Verwenden Sie den Workday-Wert „User_ID“ gefiltert mit einem Azure AD-Bereitstellungsausdruck, um unzulässige Zeichen zu entfernen.*
      
   * Benutzerprinzipalname (userPrincipalName)
      * *Beispiel: Verwenden Sie den Workday-Wert „User_ID“ mit einem Azure AD-Bereitstellungsausdruck, um einen Domänennamen anzufügen.*

* **Wie müssen Benutzer zwischen Workday und Active Directory zugeordnet werden?**

  * *Beispiel: Benutzer mit einem bestimmten Workday-Wert „Worker_ID“ werden Active Directory-Benutzern zugeordnet, deren „EmployeeID“ den gleichen Wert enthält. Wenn der Wert von „Worker_ID“ nicht in Active Directory gefunden wird, erstellen Sie einen neuen Benutzer.*
  
* **Enthält die Active Directory-Gesamtstruktur bereits die Benutzer-IDs, die notwendig sind, damit die Zuordnungslogik funktioniert?**

  * *Beispiel: Im Fall einer neuen Workday-Bereitstellung wird dringend empfohlen, Active Directory vorab mit den korrekten „Worker_ID“-Werten aus Workday (oder einem eindeutigen ID-Wert Ihrer Wahl) aufzufüllen, um die Zuordnungslogik so einfach wie möglich zu halten.*
    
    
### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Teil 1: Hinzufügen der Bereitstellungsconnector-App und Herstellen der Verbindung mit Workday

**So konfigurieren Sie die Bereitstellung aus Workday in Active Directory**

1.  Besuchen Sie <https://portal.azure.com>.

2.  Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** aus.

3.  Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

4.  Klicken Sie auf **Anwendung hinzufügen**, und wählen Sie die Kategorie **Alle** aus.

5.  Suchen Sie nach **Workday Provisioning to Active Directory**, und fügen Sie die App aus dem Katalog hinzu.

6.  Sobald die App hinzugefügt wurde und der Bildschirm mit den App-Details angezeigt wird, wählen Sie **Bereitstellung** aus.

7.  Legen Sie **Bereitstellungsmodus** **auf** **Automatisch** fest.

8.  Vervollständigen Sie den Abschnitt **Administratoranmeldeinformationen** wie folgt:

   * **Administratorbenutzername**: Geben Sie den Benutzernamen des Workday-Systemintegrationskontos mit angefügtem Mandantendomänennamen ein. **Dies sollte ungefähr wie folgt aussehen: username@contoso4**

   * **Administratorkennwort**: Geben Sie das Kennwort des Workday-Systemintegrationskontos ein.

   * **Mandanten-URL**: Geben Sie die URL des Workday-Webdienstendpunkts für Ihren Mandanten ein. Diese sollte wie folgt lauten: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources. Dabei wird „contoso4“ durch den Namen Ihres Mandanten und „wd3-impl“ durch die ordnungsgemäße Umgebungszeichenfolge ersetzt.

   * **Active Directory-Gesamtstruktur**: Der Name der Active Directory-Gesamtstruktur entsprechend der Rückgabe des PowerShell-Cmdlets „Get-ADForest“. Dieser meist eine Zeichenfolge wie: *contoso.com*

   * **Active Directory-Container**: Geben Sie die Zeichenfolge des Containers ein, der alle Benutzer in Ihrer Active Directory-Gesamtstruktur enthält. Beispiel: *OU=Standard Users,OU=Users,DC=contoso,DC=test*

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**. Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, überprüfen Sie, ob die Workday-Anmeldeinformationen in Workday gültig sind. 

![Azure-Portal](./media/workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen 

In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus Workday in Active Directory.

1.  Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Workday-Mitarbeiter in lokale Umgebung synchronisieren**.

2.  Im Feld **Quellobjektbereich** können Sie die Benutzergruppen in Workday für die Bereitstellung in Active Directory auswählen, indem Sie verschiedene attributbasierte Filter definieren. Die Standardoption ist „Alle Benutzer in Workday“. Beispielfilter:

   * Beispiel: Auswählen der Benutzer mit Mitarbeiter-IDs von 1000000 bis 2000000

      * Attribut: WorkerID

      * Operator: REGEX.VERGLEICH

      * Wert: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Beispiel: Nur Mitarbeiter und keine vorübergehend Beschäftigten 

      * Attribut: EmployeeID

      * Operator: IST NICHT NULL

3.  Im Feld **Zielobjektaktionen** können Sie global filtern, welche Aktionen auf Active Directory angewendet werden können. **Erstellen** und **Aktualisieren** erfolgen am häufigsten.

4.  Im Abschnitt **Attributzuordnungen** können Sie definieren, wie einzelne Workday-Attribute Active Directory-Attributen zugeordnet werden.

5. Klicken Sie auf eine vorhandene Attributzuordnung, um sie zu aktualisieren. Oder klicken Sie am unteren Bildschirmrand auf **Neue Zuordnung hinzufügen**, um neue Zuordnungen hinzuzufügen. Eine einzelne Attributzuordnung unterstützt die folgenden Eigenschaften:

      * **Zuordnungstyp**

         * **Direkt**: Schreibt den Wert des Workday-Attributs unverändert in das AD-Attribut.

         * **Konstante**: Schreibt einen statischen, konstanten Zeichenfolgenwert in das AD-Attribut.

         * **Ausdruck**: Ermöglicht das Schreiben eines benutzerdefinierten Werts basierend auf einem oder mehreren Workday-Attributen in das AD-Attribut. [Weitere Informationen finden Sie im Artikel zu Ausdrücken](../active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Quellattribut**: Das Benutzerattribut aus Workday. Wenn das gesuchte Attribut nicht vorhanden ist, finden Sie weitere Informationen unter [Anpassen der Liste der Workday-Benutzerattribute](#customizing-the-list-of-workday-user-attributes).

      * **Standardwert**: Optional. Wenn das Quellattribut einen leeren Wert aufweist, wird von der Zuordnung stattdessen dieser Wert geschrieben.
            Die meisten Konfigurationen sehen vor, dieses Feld leer zu lassen.

      * **Zielattribut**: Das Benutzerattribut in Active Directory.

      * **Objekte mit diesem Attribut abgleichen**: Gibt an, ob diese Zuordnung zum eindeutigen Bestimmen von Benutzern zwischen Workday und Active Directory verwendet werden soll. Diese Einstellung wird meist auf das Feld „Mitarbeiter-ID“ für Workday festgelegt, das in der Regel den „Mitarbeiter-ID“-Attributen in Active Directory zugeordnet wird.

      * **Rangfolge für Abgleich**: Es können mehrere Attribute für den Abgleich festgelegt werden. Falls mehrere vorhanden sind, werden sie entsprechend der in diesem Feld festgelegten Reihenfolge ausgewertet. Sobald eine Übereinstimmung gefunden wird, werden keine weiteren Attribute für den Abgleich mehr ausgewertet.

      * **Diese Zuordnung anwenden**
       
         * **Immer**: Wenden Sie diese Zuordnung sowohl bei der Aktion zum Erstellen eines Benutzers als auch bei der zum Aktualisieren eines Benutzers an.

         * **Nur während der Erstellung**: Wenden Sie diese Zuordnung nur bei der Aktion zum Erstellen eines Benutzers an.

6. Klicken Sie oben im Abschnitt „Attributzuordnung“ auf **Speichern**, um Ihre Zuordnungen zu speichern.

![Azure-Portal](./media/workday-inbound-tutorial/WD_2.PNG)

**Nachstehend finden Sie einige Beispiele für Attributzuordnungen zwischen Workday und Active Directory sowie einige häufig verwendete Ausdrücke**

-   Der Ausdruck für die Zuordnung zum Attribut „parentDistinguishedName“ wird verwendet, um Benutzer basierend auf mindestens einem Workday-Quellattribut in anderen Organisationseinheiten bereitzustellen. In diesem Beispiel werden Benutzer basierend auf dem Ort in verschiedenen Organisationseinheiten platziert.

-   Das Attribut „userPrincipalName“ in Active Directory wird durch Verketten der Workday-Benutzer-ID mit einem Domänensuffix generiert.

-   [Hier finden Sie Dokumentation zum Schreiben von Ausdrücken](../active-directory-saas-writing-expressions-for-attribute-mappings.md). Die Dokumentation enthält auch Beispiele zum Entfernen von Sonderzeichen.

  
| WORKDAY-ATTRIBUT | ACTIVE DIRECTORY-ATTRIBUT |  ÜBEREINSTIMMENDE ID? | ERSTELLEN/AKTUALISIEREN |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  EmployeeID | **Ja** | Wird nur bei der Erstellung geschrieben |
| **UserID**    |  cn    |   |   Wird nur bei der Erstellung geschrieben |
| **Join („@“, [Benutzer-ID] „contoso.com“)**   | userPrincipalName     |     | Wird nur bei der Erstellung geschrieben 
| **Replace(Mid(Replace(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    sAMAccountName            |     |         Wird nur bei der Erstellung geschrieben |
| **Switch(\[Active\], , "0", "True", "1",)** |  accountDisabled      |     | Erstellen und aktualisieren |
| **Vorname**   | givenName       |     |    Erstellen und aktualisieren |
| **Nachname**   |   sn   |     |  Erstellen und aktualisieren |
| **PreferredNameData**  |  displayName |     |   Erstellen und aktualisieren |
| **Company**         | company   |     |  Erstellen und aktualisieren |
| **SupervisoryOrganization**  | department  |     |  Erstellen und aktualisieren |
| **ManagerReference**   | manager  |     |  Erstellen und aktualisieren |
| **BusinessTitle**   |  title     |     |  Erstellen und aktualisieren | 
| **AddressLineData**    |  streetAddress  |     |   Erstellen und aktualisieren |
| **Municipality**   |   l   |     | Erstellen und aktualisieren |
| **CountryReferenceTwoLetter**      |   co |     |   Erstellen und aktualisieren |
| **CountryReferenceTwoLetter**    |  c  |     |         Erstellen und aktualisieren |
| **CountryRegionReference** |  st     |     | Erstellen und aktualisieren |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Erstellen und aktualisieren |
| **PostalCode**  |   postalCode  |     | Erstellen und aktualisieren |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Erstellen und aktualisieren |
| **Fax**      | facsimileTelephoneNumber     |     |    Erstellen und aktualisieren |
| **Mobile**  |    mobile       |     |       Erstellen und aktualisieren |
| **LocalReference** |  preferredLanguage  |     |  Erstellen und aktualisieren |                                               
| **Switch(\[Municipality\], "OU=Standard Users,OU=Users,OU=Default,OU=Locations,DC=contoso,DC=com", "Dallas", "OU=Standard Users,OU=Users,OU=Dallas,OU=Locations,DC=contoso,DC=com", "Austin", "OU=Standard Users,OU=Users,OU=Austin,OU=Locations,DC=contoso,DC=com", "Seattle", "OU=Standard Users,OU=Users,OU=Seattle,OU=Locations,DC=contoso,DC=com", “London", "OU=Standard Users,OU=Users,OU=London,OU=Locations,DC=contoso,DC=com")**  | parentDistinguishedName     |     |  Erstellen und aktualisieren |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>Teil 3: Konfigurieren des lokalen Synchronisierungs-Agents

Um Active Directory lokal bereitzustellen, muss in der gewünschten Active Directory-Gesamtstruktur ein Agent auf einem in die Domäne eingebunden Server installiert werden. Für diesen Vorgang sind die Anmeldeinformationen eines Domänenadministrators oder (Unternehmensadministrators) erforderlich.

**[Sie können den lokalen Synchronisierungs-Agent hier herunterladen.](https://go.microsoft.com/fwlink/?linkid=847801)**

Führen Sie nach der Installation des Agents die folgenden PowerShell-Befehle zum Konfigurieren des Agents für Ihre Umgebung aus.

**Befehl 1**

> cd "C:\Programme\Microsoft Azure AD Connect Provisioning Agent\Modules\AADSyncAgent" Agent\\Modules\\AADSyncAgent

> Import-Module "C:\Programme\Microsoft Azure AD Connect Provisioning Agent\Modules\AADSyncAgent\AADSyncAgent.psd1"

**Befehl 2**

> Add-ADSyncAgentActiveDirectoryConfiguration

* Eingabe: Geben Sie für „DirectoryName“ (Verzeichnisname) den Namen der Active Directory-Gesamtstruktur entsprechend der Eingabe in Teil \#2 ein.
* Eingabe: Administratorbenutzername und -kennwort für die Active Directory-Gesamtstruktur

>[!TIP]
> Wenn die Fehlermeldung „Vertrauensstellung zwischen der primären Domäne und der vertrauenswürdigen Domäne konnte nicht hergestellt werden“ angezeigt wird, liegt das daran, dass sich der lokale Computer in einer Umgebung befindet, in der mehrere Active Directory-Gesamtstrukturen oder -Domänen konfiguriert sind und mindestens eine konfigurierte Vertrauensstellung entweder fehlerhaft oder nicht betriebsbereit ist. Um das Problem zu beheben, korrigieren oder entfernen Sie die fehlerhafte Vertrauensstellung.

**Befehl 3**

> Add-ADSyncAgentAzureActiveDirectoryConfiguration

* Eingabe: Benutzername und Kennwort des globalen Administrators für Ihren Azure AD-Mandanten

>[!IMPORTANT]
>Es gibt derzeit ein bekanntes Problem, das dazu führt, dass globale Administratoranmeldeinformationen bei Verwendung einer benutzerdefinierten Domäne nicht funktionieren (Beispiel: admin@contoso.com). Erstellen und verwenden Sie zur Umgehung dieses Problems ein globales Administratorkonto mit einer onmicrosoft.com-Domäne (Beispiel: admin@contoso.onmicrosoft.com).

>[!IMPORTANT]
>Es gibt derzeit ein bekanntes Problem, das dazu führt, dass globale Administratoranmeldeinformationen bei aktivierter mehrstufiger Authentifizierung nicht funktionieren. Deaktivieren Sie zur Umgehung dieses Problems die mehrstufige Authentifizierung für den globalen Administrator.

**Befehl 4**

> Get-AdSyncAgentProvisioningTasks

* Aktion: Bestätigen Sie, dass Daten zurückgegeben werden. Dieser Befehl ermittelt automatisch Workday-Bereitstellungs-Apps in Ihrem Azure AD-Mandanten. Beispielausgabe:

> Name: My AD Forest
>
> Aktiviert: TRUE
>
> DirectoryName: mydomain.contoso.com
>
> Credentialed: False
>
> Bezeichner: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Befehl 5**

> Start-AdSyncAgentSynchronization -Automatic

**Befehl 6**

> net stop aadsyncagent

**Befehl 7**

> net start aadsyncagent

>[!TIP]
>Neben den net-Befehlen in PowerShell kann der Synchronisierungs-Agent-Dienst auch über **Services.msc** gestartet und beendet werden. Sollte beim Ausführen der PowerShell-Befehle ein Fehler auftreten, vergewissern Sie sich unter **Services.msc**, dass **Microsoft Azure AD Connect Provisioning Agent** ausgeführt wird.

![Dienste](./media/workday-inbound-tutorial/Services.png)  

**Zusätzliche Konfiguration für Kunden in der Europäischen Union**

Falls sich Ihr Azure Active Directory-Mandant in einem der europäischen Datencenter befindet, führen Sie die folgenden Zusatzschritte aus.

1. Öffnen Sie **Services.msc**, und beenden Sie den Dienst **Microsoft Azure AD Connect-Bereitstellungs-Agent**.
2. Wechseln Sie zum Agent-Installationsordner (Beispiel: C:\Programme\Microsoft Azure AD Connect Provisioning Agent).
3. Öffnen Sie **SyncAgnt.exe.config** in einem Text-Editor.
4. Ersetzen Sie https://manage.hub.syncfabric.windowsazure.com/Management durch **https://eu.manage.hub.syncfabric.windowsazure.com/Management**.
5. Ersetzen Sie https://provision.hub.syncfabric.windowsazure.com/Provisioning durch **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**.
6. Speichern Sie die Datei **SyncAgnt.exe.config**.
7. Öffnen Sie **Services.msc**, und starten Sie den Dienst **Microsoft Azure AD Connect Provisioning Agent**.

**Behandeln von Problemen mit dem Agent**

Das [Windows-Ereignisprotokoll](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) auf dem Windows Server-Computer, der als Host für den Agent fungiert, enthält Ereignisse für alle Vorgänge, die der Agent ausgeführt hat. So zeigen Sie diese Ereignisse an:
    
1. Öffnen Sie **Eventvwr.msc**.
2. Klicken Sie auf **Windows-Protokolle > Anwendung**.
3. Sehen Sie sich alle Ereignisse an, die unter der Quelle **AADSyncAgent** protokolliert wurden. 
4. Suchen Sie nach Fehlern und Warnungen.

Im Falle eines Berechtigungsproblems mit den Active Directory- oder Azure Active Directory-Anmeldeinformationen aus den PowerShell-Befehlen wird ein Fehler wie der folgende angezeigt: 
    
![Ereignisprotokolle](./media/workday-inbound-tutorial/Windows_Event_Logs.png) 


### <a name="part-4-start-the-service"></a>Teil 4: Starten des Diensts
Nach Abschluss der Teile 1 bis 3 können Sie im Azure-Portal den Bereitstellungsdienst starten.

1.  Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dadurch wird die erste Synchronisierung gestartet, die abhängig von der Anzahl der Benutzer in Workday eine variable Anzahl von Stunden dauern kann.

4. Im Azure-Portal können Sie sich auf der Registerkarte **Überwachungsprotokolle** jederzeit ansehen, welche Aktionen der Bereitstellungsdienst ausgeführt hat. Die Überwachungsprotokolle enthalten alle einzelnen Synchronisierungsereignisse des Bereitstellungsdiensts – beispielsweise, welche Benutzer in Workday gelesen und anschließend Active Directory hinzugefügt oder dort aktualisiert wurden. **[Ausführliche Anweisungen zum Lesen der Überwachungsprotokolle finden Sie in der Anleitung zur Erstellung von Bereitstellungsberichten.](../active-directory-saas-provisioning-reporting.md)**

1.  Prüfen Sie im [Windows-Ereignisprotokoll](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) auf dem Windows Server-Computer, der als Host für den Agent fungiert, ob neue Fehler oder Warnungen vorliegen. Starten Sie zum Anzeigen dieser Ereignisse **Eventvwr.msc** auf dem Server, und klicken Sie auf **Windows-Protokolle > Anwendung**. Alle bereitstellungsbezogenen Meldungen werden unter der Quelle **AADSyncAgent** protokolliert.

6. Anschließend wird auf der Registerkarte **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben:

![Azure-Portal](./media/workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Konfiguration der Benutzerbereitstellung in Azure Active Directory
Die Konfiguration der Bereitstellung in Azure Active Directory hängt von Ihren Bereitstellungsanforderungen ab (siehe die folgende Tabelle).

| Szenario | Lösung |
| -------- | -------- |
| **Benutzer müssen in Active Directory und Azure AD bereitgestellt werden** | **[AAD Connect](../connect/active-directory-aadconnect.md)** verwenden |
| **Benutzer müssen nur in Active Directory bereitgestellt werden** | **[AAD Connect](../connect/active-directory-aadconnect.md)** verwenden |
| **Benutzer müssen nur in Azure AD (als reine Cloudbenutzer) bereitgestellt werden** | Die App **Workday to Azure Active Directory provisioning** im App-Katalog verwenden |

Anleitungen zum Einrichten von Azure AD Connect finden Sie in der [Dokumentation zu Azure AD Connect](../connect/active-directory-aadconnect.md).

In den folgenden Abschnitten wird das Einrichten einer Verbindung zwischen Workday und Azure AD beschrieben, um reine Cloudbenutzer bereitzustellen.

> [!IMPORTANT]
> Führen Sie diese Schritte nur aus, wenn es reine Cloudbenutzer gibt, die in Azure AD und nicht in lokalem Active Directory bereitgestellt werden müssen.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Teil 1: Hinzufügen der Bereitstellungsconnector-App für Azure AD und Herstellen der Verbindung mit Workday

**So konfigurieren Sie die Workday-zu-Azure Active Directory-Bereitstellung für reine Cloudbenutzer**

1.  Wechseln Sie zur Adresse <https://portal.azure.com>.

2.  Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** aus.

3.  Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

4.  Klicken Sie auf **Anwendung hinzufügen**, und wählen Sie dann die Kategorie **Alle** aus.

5.  Suchen Sie nach **Workday to Azure AD provisioning**, und fügen Sie die App aus dem Katalog hinzu.

6.  Sobald die App hinzugefügt wurde und der Bildschirm mit den App-Details angezeigt wird, wählen Sie **Bereitstellung** aus.

7.  Legen Sie **Bereitstellungsmodus** **auf** **Automatisch** fest.

8.  Vervollständigen Sie den Abschnitt **Administratoranmeldeinformationen** wie folgt:

   * **Administratorbenutzername**: Geben Sie den Benutzernamen des Workday-Systemintegrationskontos mit angefügtem Mandantendomänennamen ein. Dies sollte ungefähr wie folgt aussehen: username@contoso4

   * **Administratorkennwort**: Geben Sie das Kennwort des Workday-Systemintegrationskontos ein.

   * **Mandanten-URL**: Geben Sie die URL des Workday-Webdienstendpunkts für Ihren Mandanten ein. Diese sollte wie folgt lauten: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources. Dabei wird „contoso4“ durch den Namen Ihres Mandanten und „wd3-impl“ durch die ordnungsgemäße Umgebungszeichenfolge ersetzt. Ist diese URL nicht bekannt, erkundigen Sie sich bei Ihrem Workday-Integrationspartner oder bei einem Supportmitarbeiter nach der korrekten URL.

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**.

   * Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, überprüfen Sie, ob die Workday-URL und -Anmeldeinformationen in Workday gültig sind.

### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen 

In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus Workday in Azure Active Directory für reine Cloudbenutzer.

1. Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Mitarbeiter in Azure AD synchronisieren**.

2. Im Feld **Quellobjektbereich** können Sie die Benutzergruppen in Workday für die Bereitstellung in Azure Active Directory auswählen, indem Sie verschiedene attributbasierte Filter definieren. Die Standardoption ist „Alle Benutzer in Workday“. Beispielfilter:

   * Beispiel: Auswählen der Benutzer mit Mitarbeiter-IDs von 1000000 bis 2000000

      * Attribut: WorkerID

      * Operator: REGEX.VERGLEICH

      * Wert: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Beispiel: Nur vorübergehend Beschäftigte und keine regulären Mitarbeiter

      * Attribut: ContingentID

      * Operator: IST NICHT NULL

3. Im Feld **Zielobjektaktionen** können Sie global filtern, welche Aktionen auf Azure AD angewendet werden können. **Erstellen** und **Aktualisieren** erfolgen am häufigsten.

4. Im Abschnitt **Attributzuordnungen** können Sie definieren, wie einzelne Workday-Attribute Active Directory-Attributen zugeordnet werden.

5. Klicken Sie auf eine vorhandene Attributzuordnung, um sie zu aktualisieren. Oder klicken Sie am unteren Bildschirmrand auf **Neue Zuordnung hinzufügen**, um neue Zuordnungen hinzuzufügen. Eine einzelne Attributzuordnung unterstützt die folgenden Eigenschaften:

   * **Zuordnungstyp**

      * **Direkt**: Schreibt den Wert des Workday-Attributs unverändert in das AD-Attribut.

      * **Konstante**: Schreibt einen statischen, konstanten Zeichenfolgenwert in das AD-Attribut.

      * **Ausdruck**: Ermöglicht das Schreiben eines benutzerdefinierten Werts basierend auf einem oder mehreren Workday-Attributen in das AD-Attribut. [Weitere Informationen finden Sie im Artikel zu Ausdrücken](../active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Quellattribut**: Das Benutzerattribut aus Workday. Wenn das gesuchte Attribut nicht vorhanden ist, finden Sie weitere Informationen unter [Anpassen der Liste der Workday-Benutzerattribute](#customizing-the-list-of-workday-user-attributes).

   * **Standardwert**: Optional. Wenn das Quellattribut einen leeren Wert aufweist, wird von der Zuordnung stattdessen dieser Wert geschrieben.
            Die meisten Konfigurationen sehen vor, dieses Feld leer zu lassen.

   * **Zielattribut**: Das Benutzerattribut in Azure AD.

   * **Objekte mit diesem Attribut abgleichen**: Gibt an, ob diese Zuordnung zum eindeutigen Bestimmen von Benutzern zwischen Workday und Azure AD verwendet werden soll. Diese Einstellung wird meist auf das Feld „Mitarbeiter-ID“ für Workday festgelegt, das in der Regel dem „Mitarbeiter-ID“-Attribut (neu) oder einem Erweiterungsattribut in Azure AD zugeordnet wird.

   * **Rangfolge für Abgleich**: Es können mehrere Attribute für den Abgleich festgelegt werden. Falls mehrere vorhanden sind, werden sie entsprechend der in diesem Feld festgelegten Reihenfolge ausgewertet. Sobald eine Übereinstimmung gefunden wird, werden keine weiteren Attribute für den Abgleich mehr ausgewertet.

   * **Diese Zuordnung anwenden**

     * **Immer**: Wenden Sie diese Zuordnung sowohl bei der Aktion zum Erstellen eines Benutzers als auch bei der zum Aktualisieren eines Benutzers an.

     * **Nur während der Erstellung**: Wenden Sie diese Zuordnung nur bei der Aktion zum Erstellen eines Benutzers an.

6. Klicken Sie oben im Abschnitt „Attributzuordnung“ auf **Speichern**, um Ihre Zuordnungen zu speichern.

### <a name="part-3-start-the-service"></a>Teil 3: Starten des Diensts
Sobald Sie die Teile 1 und 2 abgeschlossen haben, können Sie den Bereitstellungsdienst starten.

1. Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dadurch wird die erste Synchronisierung gestartet, die abhängig von der Anzahl der Benutzer in Workday eine variable Anzahl von Stunden dauern kann.

4. Einzelne Synchronisierungsereignisse können auf der Registerkarte **Überwachungsprotokolle** angezeigt werden. **[Ausführliche Anweisungen zum Lesen der Überwachungsprotokolle finden Sie in der Anleitung zur Erstellung von Bereitstellungsberichten.](../active-directory-saas-provisioning-reporting.md)**

5. Anschließend wird auf der Registerkarte **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben:

## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Konfigurieren des Zurückschreibens von E-Mail-Adressen in Workday
Befolgen Sie diese Anweisungen zum Konfigurieren des Zurückschreibens von E-Mail-Adressen der Benutzer aus Azure Active Directory in Workday.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Teil 1: Hinzufügen der Bereitstellungsconnector-App und Herstellen der Verbindung mit Workday

**So konfigurieren Sie die Bereitstellung aus Workday in Active Directory**

1. Besuchen Sie <https://portal.azure.com>.

2. Wählen Sie auf der linken Navigationsleiste **Azure Active Directory** aus.

3. Klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

4. Klicken Sie auf **Anwendung hinzufügen**, und wählen Sie dann die Kategorie **Alle** aus.

5. Suchen Sie nach **Workday Writeback**, und fügen Sie die App aus dem Katalog hinzu.

6. Sobald die App hinzugefügt wurde und der Bildschirm mit den App-Details angezeigt wird, wählen Sie **Bereitstellung** aus.

7. Legen Sie **Bereitstellungsmodus** **auf** **Automatisch** fest.

8. Vervollständigen Sie den Abschnitt **Administratoranmeldeinformationen** wie folgt:

   * **Administratorbenutzername**: Geben Sie den Benutzernamen des Workday-Systemintegrationskontos mit angefügtem Mandantendomänennamen ein. Dies sollte ungefähr wie folgt aussehen: username@contoso4

   * **Administratorkennwort**: Geben Sie das Kennwort des Workday-Systemintegrationskontos ein.

   * **Mandanten-URL**: Geben Sie die URL des Workday-Webdienstendpunkts für Ihren Mandanten ein. Diese sollte wie folgt lauten: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources. Dabei wird „contoso4“ durch den Namen Ihres Mandanten und „wd3-impl“ (bei Bedarf) durch die ordnungsgemäße Umgebungszeichenfolge ersetzt.

   * **Benachrichtigungs-E-Mail**: Geben Sie Ihre E-Mail-Adresse ein, und aktivieren Sie das Kontrollkästchen „E-Mail senden, wenn Fehler auftritt“.

   * Klicken Sie auf die Schaltfläche **Verbindung testen**. Wenn der Verbindungstest erfolgreich ist, klicken Sie oben auf die Schaltfläche **Speichern**. Falls nicht, überprüfen Sie, ob die Workday-URL und -Anmeldeinformationen in Workday gültig sind.

### <a name="part-2-configure-attribute-mappings"></a>Teil 2: Konfigurieren von Attributzuordnungen 

In diesem Abschnitt konfigurieren Sie den Fluss von Benutzerdaten aus Workday in Active Directory.

1. Klicken Sie auf der Registerkarte „Bereitstellung“ unter **Zuordnungen** auf **Azure AD-Benutzer in Workday synchronisieren**.

2. Im Feld **Quellobjektbereich** können Sie optional filtern, für welche Gruppen von Benutzern in Azure Active Directory E-Mail-Adressen in Workday zurückgeschrieben werden sollen. Die Standardoption ist „Alle Benutzer in Azure AD“. 

3. Aktualisieren Sie im Abschnitt **Attributzuordnungen** die entsprechende ID, um das Attribut in Azure Active Directory anzugeben, in dem die Mitarbeiter-ID von Workday gespeichert ist. Eine gängige Methode für den Abgleich ist das Synchronisieren der Mitarbeiter-ID von Workday mit „extensionAttribute“ 1-15 in Azure AD und das anschließende Verwenden dieses Attributs in Azure AD, um die Benutzer wieder mit Workday abzugleichen. 

4. Klicken Sie oben im Abschnitt „Attributzuordnung“ auf **Speichern**, um Ihre Zuordnungen zu speichern.

### <a name="part-3-start-the-service"></a>Teil 3: Starten des Diensts
Sobald Sie die Teile 1 und 2 abgeschlossen haben, können Sie den Bereitstellungsdienst starten.

1. Legen Sie auf der Registerkarte **Bereitstellung** die Einstellung **Bereitstellungsstatus** auf **Ein** fest.

2. Klicken Sie auf **Speichern**.

3. Dadurch wird die erste Synchronisierung gestartet, die abhängig von der Anzahl der Benutzer in Workday eine variable Anzahl von Stunden dauern kann.

4. Einzelne Synchronisierungsereignisse können auf der Registerkarte **Überwachungsprotokolle** angezeigt werden. **[Ausführliche Anweisungen zum Lesen der Überwachungsprotokolle finden Sie in der Anleitung zur Erstellung von Bereitstellungsberichten.](../active-directory-saas-provisioning-reporting.md)**

5. Anschließend wird auf der Registerkarte **Bereitstellung** ein Überwachungszusammenfassungsbericht ausgegeben:

## <a name="customizing-the-list-of-workday-user-attributes"></a>Anpassen der Liste der Workday-Benutzerattribute
Die Workday-Bereitstellungs-Apps für Active Directory und Azure AD umfassen jeweils eine Liste mit Workday-Benutzerattributen, aus denen Sie auswählen können. Diese Listen sind jedoch nicht sehr umfangreich. Workday unterstützt mehrere Hundert möglicher Benutzerattribute, die entweder als Standardattribute oder als eindeutige Attribute für Ihren Workday-Mandanten eingerichtet werden können. 

Der Azure AD-Bereitstellungsdienst unterstützt die Möglichkeit, Ihre Liste der Workday-Attribute so anzupassen, dass alle im [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html)-Vorgang der Human Resources-API verfügbar gemachten Attribute enthalten sind.

Zu diesem Zweck müssen Sie [Workday Studio](https://community.workday.com/studio-download) verwenden, um die XPath-Ausdrücke zu extrahieren, die die gewünschten Attribute repräsentieren. Anschließend verwenden Sie den erweiterten Attribut-Editor im Azure-Portal, um sie zu Ihrer Bereitstellungskonfiguration hinzuzufügen.

**So rufen Sie einen XPath-Ausdruck für ein Workday-Benutzerattribut ab:**

1. Laden Sie [Workday Studio](https://community.workday.com/studio-download), herunter, und installieren Sie es. Sie benötigen ein Workday-Communitykonto, um auf das Installationsprogramm zuzugreifen.

2. Laden Sie die WDSL-Datei „Workday Human_Resources“ über diese URL herunter: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Starten Sie Workday Studio.

4. Wählen Sie in der Befehlsleiste die Option **Workday > Test Web Service in Tester** (Workday > Webdienst im Tester testen) aus.

5. Wählen Sie **External** und dann die WSDL-Datei „Human_Resources“ aus, die Sie in Schritt 2 heruntergeladen haben.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio1.PNG)

6. Legen Sie das Feld **Location** auf `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources` fest, ersetzen Sie jedoch „IMPL-CC“ durch den tatsächlichen Typ Ihrer Instanz und „TENANT“ durch den echten Namen Ihres Mandanten.

7. Legen Sie **Operation** auf **Get_Workers** fest.

8.  Klicken Sie auf den **configure**-Link unterhalb der Bereiche „Request“ (Anforderung) und „Response“ (Antwort), um Ihre Workday-Anmeldeinformationen festzulegen. Aktivieren Sie das Kontrollkästchen **Authentifizierung**, und geben Sie den Benutzernamen und das Kennwort für Ihr Systemkonto für die Workday-Integration ein. Stellen Sie sicher, dass der Benutzername das Format name@tenant aufweist, und behalten Sie die Auswahl der Option **WS-Security UsernameToken** bei.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio2.PNG)

9. Klicken Sie auf **OK**.

10. Fügen Sie im Bereich **Request** den unten stehenden XML-Code ein, und legen Sie **Employee_ID** auf die Mitarbeiter-ID eines realen Benutzers in Ihrem Workday-Mandanten fest. Wählen Sie einen Benutzer aus, für den das Attribut aufgefüllt ist, das Sie extrahieren möchten.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
      <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v21.1">
          <wd:Request_References wd:Skip_Non_Existing_Instances="true">
            <wd:Worker_Reference>
              <wd:ID wd:type="Employee_ID">21008</wd:ID>
            </wd:Worker_Reference>
          </wd:Request_References>
          <wd:Response_Group>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Personal_Information>true</wd:Include_Personal_Information>
            <wd:Include_Employment_Information>true</wd:Include_Employment_Information>
            <wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>
            <wd:Include_Organizations>true</wd:Include_Organizations>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Transaction_Log_Data>true</wd:Include_Transaction_Log_Data>
            <wd:Include_Photo>true</wd:Include_Photo>
            <wd:Include_User_Account>true</wd:Include_User_Account>
            <wd:Include_Roles>true</wd:Include_Roles>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```
 
11. Klicken Sie auf **Send Request** (Anforderung senden, grüner Pfeil), um den Befehl auszuführen. Bei erfolgreicher Ausführung wird die Antwort im Bereich **Response** angezeigt. Überprüfen Sie die Antwort, um sicherzustellen, dass sie die Daten des eingegebenen Benutzers enthält und fehlerfrei ist.

12. Wenn alles richtig ist, kopieren Sie den XML-Code aus dem Bereich **Response**, und speichern Sie ihn als XML-Datei.

13. Wählen Sie in der Befehlsleiste von Workday Studio **File > Open File...** (Datei > Datei öffnen...) aus, und öffnen Sie die gespeicherte XML-Datei. Die Datei wird im XML-Editor von Workday Studio geöffnet.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio3.PNG)

14. Navigieren Sie in der Dateistruktur durch **/env: Envelope > env: Body > wd:Get_Workers_Response > wd:Response_Data > wd: Worker**, um die Daten des Benutzers zu suchen. 

15. Suchen Sie unter **wd: Worker** das Attribut, das Sie hinzufügen möchten, und wählen Sie es aus.

16. Kopieren Sie den XPath-Ausdruck für das ausgewählte Attribut aus dem Feld **Document Path** (Dokumentpfad).

1. Entfernen Sie das Präfix **/env:Envelope/env:Body/wd:Get_Workers_Response/wd:Response_Data/** aus dem kopierten Ausdruck.

18. Wenn das letzte Element im kopierten Ausdruck ein Knoten ist (Beispiel: „/wd: Birth_Date“), fügen Sie am Ende des Ausdrucks **/text()** hinzu. Dies ist nicht erforderlich, wenn das letzte Element ein Attribut ist (Beispiel: „/@wd: type“).

19. Das Ergebnis sollte in etwa wie folgt aussehen: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Diese Zeile kopieren Sie in das Azure-Portal.


**So fügen Sie Ihr benutzerdefiniertes Workday-Benutzerattribut zu Ihrer Bereitstellungskonfiguration hinzu:**

1. Starten Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zum Bereitstellungsbereich Ihrer Workday-Bereitstellungsanwendung, wie oben in diesem Tutorial beschrieben.

2. Legen Sie den **Bereitstellungsstatus** auf **Aus** fest, und klicken Sie dann auf **Speichern**. So können Sie sicherstellen, dass Ihre Änderungen erst dann wirksam werden, wenn Sie bereit sind.

3. Wählen Sie unter **Zuordnungen** die Option **Worker mit lokalem System synchronisieren** (oder **Worker mit Azure AD synchronisieren**).

4. Scrollen Sie im nächsten Bildschirm nach unten, und wählen Sie **Erweiterte Optionen anzeigen** aus.

5. Wählen Sie **Attributliste für Workday bearbeiten** aus.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio_AAD1.PNG)

6. Scrollen Sie zu den Eingabefeldern am Ende der Attributliste.

7. Geben Sie unter **Name** einen Anzeigenamen für Ihr Attribut ein.

8. Wählen Sie einen **Typ** aus, der zu Ihrem Attribut passt (**Zeichenfolge** ist der am häufigsten verwendete Typ).

9. Geben Sie unter **API-Ausdruck** den XPath-Ausdruck ein, den Sie aus Workday Studio kopiert haben. Beispiel: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Wählen Sie **Attribut hinzufügen** aus.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio_AAD2.PNG)

11. Wählen Sie oben **Speichern** aus, und klicken Sie im angezeigten Dialogfeld auf **Ja**. Schließen Sie den Bildschirm „Attributzuordnung“, falls dieser noch geöffnet ist.

12. Wählen Sie auf der Registerkarte **Bereitstellung** erneut die Option **Worker mit lokalem System synchronisieren** (oder **Worker mit Azure AD synchronisieren**).

13. Wählen Sie **Neue Zuordnung hinzufügen** aus.

14. Das neue Attribut sollte jetzt in der Liste **Quellattribut** angezeigt werden.

15. Fügen Sie wie gewünscht eine Zuordnung für Ihr neues Attribut hinzu.

16. Denken Sie nach Abschluss der Konfiguration daran, den **Bereitstellungsstatus** wieder auf **Ein** festzulegen und zu speichern.

## <a name="known-issues"></a>Bekannte Probleme

* Beim Ausführen des PowerShell-Befehls **Add-ADSyncAgentAzureActiveDirectoryConfiguration** gibt es derzeit ein bekanntes Problem, das dazu führt, dass globale Administratoranmeldeinformationen bei Verwendung einer benutzerdefinierten Domäne nicht funktionieren (Beispiel: admin@contoso.com). Erstellen und verwenden Sie zur Umgehung dieses Problems in Azure AD ein globales Administratorkonto mit einer onmicrosoft.com-Domäne (Beispiel: admin@contoso.onmicrosoft.com).

* Das Schreiben von Daten in das Benutzerattribut „thumbnailPhoto“ im lokalen Active Directory wird derzeit nicht unterstützt.

* Der Connector „Workday to Azure AD“ wird auf Azure AD-Mandanten, auf denen AAD Connect aktiviert ist, derzeit nicht unterstützt.  

* Ein anderes Problem, das dazu führte, dass Überwachungsprotokolle bei Azure AD-Mandanten in der Europäischen Union nicht angezeigt wurden, ist inzwischen behoben. Für Azure AD-Mandanten in der EU müssen jedoch zusätzliche Agent-Konfigurationsschritte ausgeführt werden. Ausführlichere Informationen finden Sie in [Teil 3: Konfigurieren des lokalen Synchronisierungs-Agents](#Part 3: Configure the on-premises synchronization agent).

## <a name="managing-personal-data"></a>Verwalten von personenbezogenen Daten

Die Workday-Bereitstellungslösung für Active Directory benötigt einen Synchronisierungs-Agent, der auf einem in die Domäne eingebundenen Server installiert ist. Dieser Agent erstellt Protokolle im Windows-Ereignisprotokoll, die personenbezogene Daten enthalten können.

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../active-directory-saas-provisioning-reporting.md)
* [Lesen Sie, wie Sie das einmalige Anmelden zwischen Workday und Azure Active Directory konfigurieren.](workday-tutorial.md)
* [Erfahren Sie, wie Sie andere SaaS-Anwendungen in Azure Active Directory integrieren.](tutorial-list.md)

