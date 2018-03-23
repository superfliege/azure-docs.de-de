---
title: 'Tutorial: Konfigurieren von Clarizen für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Clarizen konfigurieren.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant-msft
ms.openlocfilehash: 3e75ce17f2806e4cb66f90f233b265398cdd0878
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-configure-clarizen-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Clarizen für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Clarizen und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Clarizen zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial beschriebene Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure AD-Mandanten
*   Einen Clarizen-Mandanten, für den mindestens der [Enterprise Edition-Tarif](https://www.clarizen.com/product/pricing/) aktiviert ist 
*   Ein Benutzerkonto in Clarizen mit Administratorrechten

> [!NOTE]
> Die Integration der Azure AD-Bereitstellung basiert auf der [Clarizen-API](https://api.clarizen.com/v2.0/services/), die für Clarizen-Teams mit dem Enterprise Edition-Tarif oder einem höheren Tarif zur Verfügung steht.

## <a name="adding-clarizen-from-the-gallery"></a>Hinzufügen von Clarizen aus dem Katalog
Bevor Sie Clarizen für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Clarizen aus dem Azure AD-Anwendungskatalog der Liste der verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Clarizen aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Abschnitt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um Clarizen hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld als Suchbegriff **Clarizen** ein.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearch.png)

5. Wählen Sie im Ergebnisbereich **Clarizen** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um Clarizen der Liste der SaaS-Anwendungen hinzuzufügen.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearchResults.png)

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-clarizen"></a>Zuweisen von Benutzern zu Clarizen

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Clarizen benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Clarizen anhand der folgenden Anweisungen zuweisen:

*   [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-clarizen"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Clarizen

*   Es wird empfohlen, Clarizen einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu Clarizen müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-clarizen"></a>Konfigurieren der automatischen Benutzerbereitstellung für Clarizen 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Clarizen auf der Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Clarizen aktivieren. Befolgen Sie hierzu die Anweisungen im [SSO-Tutorial zu Clarizen](active-directory-saas-clarizen-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-clarizen-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Clarizen in Azure AD:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.

2. Wählen Sie Clarizen in der Liste der SaaS-Anwendungen aus.
 
    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/AppInstanceSearch.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.
    
    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningTab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningCredentials.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** **Domäne**, **Administratorbenutzername** und **Administratorkennwort** Ihres Clarizen-Kontos ein. Beispiele für diese Werte:

    *   Geben Sie im Feld **Administratorbenutzername** den Benutzernamen des Administratorkontos im Clarizen-Mandanten ein. Beispiel: admin@contoso.com.

    *   Geben Sie im Feld **Administratorkennwort** das Kennwort des Administratorkontos für den Administratorbenutzernamen ein.

    *   Tragen Sie im Feld **Domäne** die Unterdomäne basierend auf Schritt 6 ein.
    
6. Rufen Sie die **serverLocation** für Ihr Clarizen-Konto auf Basis der unter **Authentication (Authentifizierung)** im [Rest API Guide (Rest-API-Handbuch) von Clarizen](https://success.clarizen.com/hc/en-us/articles/205711828-REST-API-Guide-Version-2) beschriebenen Schritte ab. Verwenden Sie beim Abrufen der serverLocation die Unterdomäne der URL wie unten hervorgehoben, um das Feld **Domäne** zu füllen.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/ClarizenDomain.png)

7. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Clarizen herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Clarizen-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/TestConnection.png)
    
8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/NotificationEmail.png)

9. Klicken Sie auf **Speichern**.

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Clarizen** synchronisieren.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMapping.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Clarizen synchronisiert werden. Die als **Übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Clarizen für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMappingAttributes.png)

12. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Clarizen synchronisieren**.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMapping.png)

13. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Clarizen synchronisiert werden. Die als **Übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in Clarizen für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMappingAttributes.png)

14. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](./active-directory-saas-scoping-filters.md).

15. Um den Azure AD-Bereitstellungsdienst für Clarizen zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningStatus.png)

16. Legen Sie die Benutzer und/oder Gruppen fest, die in Clarizen bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/ScopeSelection.png)

17. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Clarizen-Bereitstellung](./media/active-directory-saas-clarizen-provisioning-tutorial/SaveProvisioning.png)


Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Clarizen ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-clarizen-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
