---
title: 'Tutorial: Konfigurieren von Tableau Online für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Tableau Online konfigurieren.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: f732eebd410a6b52a21a46925a29bf4676f7c8cb
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270785"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Tableau Online für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Tableau Online und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern bzw. Gruppen in Tableau Online zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial beschriebene Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure AD-Mandanten
*   Einen [Tableau Online-Mandanten](https://www.tableau.com/)
*   Ein Benutzerkonto in Tableau Online mit Administratorberechtigungen

> [!NOTE]
> Die Azure AD-Bereitstellungsintegration basiert auf der [Tableau Online-REST-API](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm), die für Tableau Online-Entwickler verfügbar ist.

## <a name="adding-tableau-online-from-the-gallery"></a>Hinzufügen von Tableau Online aus dem Katalog
Bevor Sie Tableau Online für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Tableau Online aus dem Azure AD-Anwendungskatalog der Liste mit den verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Tableau Online aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Tableau Online** ein, wählen Sie im Ergebnisfenster **Tableau Online** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Tableau Online in der Ergebnisliste](common/search-new-app.png)

## <a name="assigning-users-to-tableau-online"></a>Zuweisen von Benutzern zu Tableau Online

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Tableau Online benötigen. Anschließend können Sie diese Benutzer bzw. Gruppen Tableau Online anhand der folgenden Anweisungen zuweisen:

*   [Assign a user or group to an enterprise app (Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App)](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Tableau Online

*   Es wird empfohlen, Tableau Online einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu Tableau Online müssen Sie im Dialogfeld für die Zuweisung eine gültige anwendungsspezifische Rolle auswählen (sofern verfügbar). Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-tableau-online"></a>Konfigurieren der automatischen Benutzerbereitstellung in Tableau Online

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern bzw. Gruppen in Tableau Online auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Tableau Online aktivieren. Befolgen Sie hierzu die Anweisungen im [SSO-Tutorial zu Tableau Online](tableauonline-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für Tableau Online in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie **Unternehmensanwendungen**, **Alle Anwendungen** und dann **Tableau Online** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Tableau Online**aus.

    ![Tableau Online-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. Geben Sie unter dem Abschnitt **Administratoranmeldeinformationen** die **Domäne**, den **Administratorbenutzernamen**, das **Administratorkennwort** und die **Inhalts-URL** Ihres Tableau Online-Kontos ein:

   * Tragen Sie im Feld **Domäne** die Unterdomäne basierend auf Schritt 6 ein.

   * Geben Sie im Feld **Administratorbenutzername** den Benutzernamen des Administratorkontos im Clarizen-Mandanten ein. Beispiel: admin@contoso.com.

   * Geben Sie im Feld **Administratorkennwort** das Kennwort des Administratorkontos für den Administratorbenutzernamen ein.

   * Tragen Sie im Feld **Inhalts-URL** die Unterdomäne basierend auf Schritt 6 ein.

6. Nach der Anmeldung Ihres Administratorkontos für Tableau Online können die Werte für **Domäne** und **Inhalts-URL** aus der URL der Administratorseite extrahiert werden.

    * Die **Domäne** für Ihr Tableau Online-Konto kann aus diesem Teil der URL kopiert werden:

        ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * Die **Inhalts-URL** für Ihr Tableau Online-Konto kann aus diesem Abschnitt kopiert werden. Dieser Wert wird beim Einrichten des Kontos definiert. In diesem Beispiel ist der Wert „contoso“:

        ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > Ihre **Domäne** kann sich von der hier dargestellten Domäne unterscheiden.

7. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Tableau Online herstellen kann. Wenn die Verbindung nicht möglich ist, sollten Sie sicherstellen, dass Ihr Tableau Online-Konto über Administratorberechtigungen verfügt, und den Vorgang wiederholen.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. Klicken Sie auf **Speichern**.

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Tableau Online synchronisieren** aus.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Tableau Online synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Tableau Online für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. Wählen Sie im Abschnitt **Zuordnungen** die Option **Synchronize Azure Active Directory Groups to Tableau** (Azure Active Directory-Gruppen mit Tableau synchronisieren) aus.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Tableau Online synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Tableau Online für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Um den Azure AD-Bereitstellungsdienst für Tableau Online zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. Legen Sie die Benutzer bzw. Gruppen fest, die in Tableau Online bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Bereitstellung von Tableau Online](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Tableau Online ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Managing user account provisioning for Enterprise Apps (Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps)](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Berichterstellung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
