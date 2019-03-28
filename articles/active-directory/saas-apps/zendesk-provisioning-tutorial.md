---
title: 'Tutorial: Konfigurieren von Zendesk für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Zendesk konfigurieren.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71e9a3f614048185d9444011da3c47b88931d0c5
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499946"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Zendesk für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Zendesk und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Zendesk zu konfigurieren. 

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

*   Einen Azure AD-Mandanten
*   Einen Zendesk-Mandanten, für den mindestens der [Enterprise](https://www.zendesk.com/product/pricing/)-Plan aktiviert ist 
*   Ein Benutzerkonto in Zendesk mit Administratorberechtigungen 

> [!NOTE]
> Die Integration der Azure AD-Bereitstellung beruht auf der [Zendesk-REST-API](https://developer.zendesk.com/rest_api/docs/zendesk-apis/resources), die Zendesk-Teams mit dem Enterprise-Plan oder einem höheren Plan zur Verfügung steht.

## <a name="adding-zendesk-from-the-gallery"></a>Hinzufügen von Zendesk aus dem Katalog
Bevor Sie Zendesk für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Zendesk aus dem Azure AD-Anwendungskatalog zu Ihrer Liste verwalteter SaaS-Anwendungen hinzufügen.

**Führen Sie die folgenden Schritte aus, um Zendesk aus dem Azure AD-Anwendungskatalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Abschnitt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um Zendesk hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld als Suchbegriff **Zendesk** ein.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk6.png)

5. Wählen Sie im Ergebnisbereich **Zendesk** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um Zendesk der Liste der SaaS-Anwendungen hinzuzufügen.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk7.png)

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk20.png)

## <a name="assigning-users-to-zendesk"></a>Zuweisen von Benutzern zu Zendesk

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Zendesk benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Zendesk wie folgt zuweisen:

*   [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Zendesk

*    Zendesk-Rollen werden aktuell automatisch und dynamisch auf der Benutzeroberfläche des Azure-Portals aufgefüllt. Vor dem Zuweisen von Zendesk-Rollen zu Benutzern muss eine Synchronisierung mit Zendesk ausgeführt worden sein, um die aktuellen Rollen Ihres Zendesk-Mandanten abzurufen.

*    Es empfiehlt sich, Zendesk einen einzelnen Azure AD-Benutzer zuzuweisen, um die anfängliche Konfiguration der automatischen Benutzerbereitstellung zu testen. Nach erfolgreichem Abschluss der Tests können weitere Benutzer und/oder Gruppen zugewiesen werden.
  
*   Es wird empfohlen, Zendesk einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu Zendesk müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-zendesk"></a>Konfigurieren der automatischen Benutzerbereitstellung in Zendesk 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Zendesk auf Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Zendesk aktivieren. Befolgen Sie dazu die Anweisungen im [SSO-Tutorial zu Zendesk](zendesk-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Zendesk in Azure AD:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.

2. Wählen Sie Zendesk in der Liste der SaaS-Anwendungen aus.
 
    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk3.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.
    
    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** die Werte für **Administratorbenutzername**, **Geheimes Token** und **Domäne** Ihres Zendesk-Kontos ein. Beispiele für diese Werte:

   * Geben Sie im Feld **Administratorbenutzername** den Benutzernamen des Administratorkontos in Ihrem Zendesk-Mandanten ein. Beispiel: admin@contoso.com.

   * Geben Sie im Feld **Geheimes Token** das geheime Token wie in Schritt 6 beschrieben ein.

   * Geben Sie im Feld **Domäne** die Unterdomäne Ihres Zendesk-Mandanten ein.
     Beispiel: Für ein Konto mit der Mandanten-URL https://my-tenant.zendesk.com lautet Ihre Unterdomäne **my-tenant**.

6. Das **Geheime Token** für Ihr Zendesk-Konto finden Sie unter **Administrator > API > Einstellungen**.
   Vergewissern Sie sich, dass **Tokenzugriff** auf **Aktiviert** festgelegt ist.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk4.png) ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Zendesk herstellen kann. Falls der Verbindungsaufbau fehlschlägt, stellen Sie sicher, dass Ihr Zendesk-Konto über Administratorberechtigungen verfügt, und versuchen Sie es noch einmal.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk19.png)
    
8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. Klicken Sie auf **Speichern**.

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Synchronize Azure Active Directory Users to Zendesk** (Azure Active Directory-Benutzer mit Zendesk synchronisieren) aus.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnung** die Benutzerattribute, die von Azure AD mit Zendesk synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Zendesk für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. Wählen Sie im Abschnitt **Zuordnungen** die Option **Synchronize Azure Active Directory Groups to ZenDesk** (Azure Active Directory-Gruppen mit Zendesk synchronisieren) aus.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Überprüfen Sie im Abschnitt **Attributzuordnung** die Gruppenattribute, die von Azure AD mit Zendesk synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in Zendesk für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Um den Azure AD-Bereitstellungsdienst für Zendesk zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. Legen Sie die Benutzer und/oder Gruppen fest, die in Zendesk bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Zendesk-Bereitstellung](./media/zendesk-provisioning-tutorial/ZenDesk18.png)


Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Zendesk ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-Einschränkungen
* Zendesk unterstützt die Verwendung von Gruppen für Benutzer mit ausschließlichen Agent-Rollen. Weitere Informationen finden Sie in der [Zendesk-Dokumentation](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).
* Wenn einem Benutzer und/oder einer Gruppe eine benutzerdefinierte Rolle zugewiesen wird, weist der Azure AD-Dienst für die automatische Benutzerbereitstellung auch die Standardrolle **Agent** zu. Nur **Agents** kann eine benutzerdefinierte Rolle zugewiesen werden. Weitere Informationen finden Sie in dieser [Zendesk-API-Dokumentation](https://developer.zendesk.com/rest_api/docs/support/users#json-format-for-agent-or-admin-requests).  

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
