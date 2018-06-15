---
title: 'Tutorial: Konfigurieren von xMatters On-Demand für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in xMatters On-Demand konfigurieren.
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
ms.date: 04/19/2018
ms.author: v-ant-msft
ms.openlocfilehash: 52fe23f5353a68e3afbac6a91955a61c7b8fb44c
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35292669"
---
# <a name="tutorial-configure-xmatters-ondemand-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von xMatters On-Demand für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in xMatters On-Demand und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in xMatters On-Demand zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial beschriebene Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure AD-Mandanten
*   Einen xMatters On-Demand-Mandanten mit aktiviertem Tarif des Typs [Starter](https://www.xmatters.com/pricing) oder einem höheren Tarif 
*   Ein Benutzerkonto in xMatters On-Demand mit Administratorrechten

> [!NOTE]
> Die Integration der Azure AD-Bereitstellung basiert auf der [xMatters On-Demand-API](https://help.xmatters.com/xmAPI), die für xMatters On-Demand-Teams mit dem Starter-Tarif oder einem höheren Tarif zur Verfügung steht.

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Hinzufügen von xMatters OnDemand aus dem Katalog

Bevor Sie xMatters On-Demand für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie xMatters On-Demand aus dem Azure AD-Anwendungskatalog der Liste der verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von xMatters On-Demand aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Abschnitt „Unternehmensanwendungen“][2]
    
3. Klicken Sie im oberen Bereich des Dialogfelds auf die Schaltfläche **Neue Anwendung**, um xMatters On-Demand hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **xMatters OnDemand** ein.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearch.png)

5. Wählen Sie im Ergebnisbereich **xMatters On-Demand** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um xMatters OnDemand der Liste der SaaS-Anwendungen hinzuzufügen.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-xmatters-ondemand"></a>Zuweisen von Benutzern zu xMatters On-Demand

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf xMatters On-Demand benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen xMatters On-Demand anhand der folgenden Anweisungen zuweisen:

*   [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-xmatters-ondemand"></a>Wichtige Tipps zum Zuweisen von Benutzern zu xMatters On-Demand

*   Es wird empfohlen, xMatters On-Demand einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu xMatters On-Demand müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-xmatters-ondemand"></a>Konfigurieren der automatischen Benutzerbereitstellung für xMatters On-Demand 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in xMatters On-Demand auf der Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für xMatters On-Demand aktivieren. Befolgen Sie dazu die Anweisungen im [SSO-Tutorial zu xMatters On-Demand](active-directory-saas-xmatters-ondemand-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-xmatters-ondemand-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für xMatters On-Demand in Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.

2. Wählen Sie xMatters On-Demand in der Liste der SaaS-Anwendungen aus.
 
    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ApplicationInstanceSearch.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.
    
    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** die Werte für **Administratorbenutzername**, **Administratorkennwort** und **Domäne** Ihres xMatters On-Demand-Kontos ein.

    *   Geben Sie im Feld **Administratorbenutzername** den Benutzernamen des Administratorkontos im xMatters On-Demand-Mandanten ein. Beispiel: admin@contoso.com.

    *   Geben Sie im Feld **Administratorkennwort** das Kennwort für den Administratorbenutzernamen ein.

    *   Geben Sie im Feld **Domäne** die Unterdomäne Ihres xMatters On-Demand-Mandanten ein.
    Beispiel: Für ein Konto mit der Mandanten-URL https://my-tenant.xmatters.com würde Ihre Unterdomäne **my-tenant** lauten.

6. Klicken Sie nach dem Ausfüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit xMatters On-Demand herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr xMatters On-Demand-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/TestConnection.png)
    
7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/EmailNotification.png)

8. Klicken Sie auf **Speichern**.

9. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit xMatters On-Demand synchronisieren** aus.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncUsers.png)

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit xMatters On-Demand synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in xMatters On-Demand für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/UserAttributes.png)

11. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit xMatters On-Demand synchronisieren** aus.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncGroups.png)

12. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit xMatters On-Demand synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in xMatters On-Demand für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/GroupAttributes.png)

13. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](./active-directory-saas-scoping-filters.md).

14. Um den Azure AD-Bereitstellungsdienst für xMatters On-Demand zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningStatus.png)

15. Legen Sie die Benutzer und/oder Gruppen fest, die in xMatters On-Demand bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ScopeSelection.png)

16. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![xMatters On-Demand-Bereitstellung](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SaveProvisioning.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in xMatters On-Demand ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_03.png
