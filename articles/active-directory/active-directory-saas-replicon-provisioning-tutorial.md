---
title: 'Tutorial: Konfigurieren von Replicon für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Replicon konfigurieren.
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
ms.date: 02/20/2018
ms.author: v-ant
ms.openlocfilehash: c7ec58b0da3a918729733809d326e6f2e5916c9d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32139973"
---
# <a name="tutorial-configure-replicon-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Replicon für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Replicon und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Replicon zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure AD-Mandanten
*   Einen aktivierten Replicon-Mandanten mit dem [Plus-Tarif](https://www.replicon.com/time-bill-pricing/) oder höher
*   Ein Benutzerkonto in Replicon mit Administratorrechten

> [!NOTE]
> Die Integration der Azure AD-Bereitstellung basiert auf der [Replicon-API](https://www.replicon.com/help/developers), die für Replicon-Teams mit dem Plus-Tarif oder einem höheren Tarif zur Verfügung steht.

## <a name="adding-replicon-from-the-gallery"></a>Hinzufügen von Replicon aus dem Katalog
Bevor Sie Replicon für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Replicon aus dem Azure AD-Anwendungskatalog der Liste der verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Replicon aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Abschnitt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um Replicon hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Begriff **Replicon** ein.

    ![Suchen der Replicon-Anwendung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppSearch.png)

5. Wählen Sie im Ergebnisbereich **Replicon** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um Replicon der Liste der SaaS-Anwendungen hinzuzufügen.

    ![Suchergebnisse mit Replicon](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppSearchResults.png)

    ![Replicon – Erstellen der Anwendung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppCreate.png)


## <a name="assigning-users-to-replicon"></a>Zuweisen von Benutzern zu Replicon

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Replicon benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Replicon anhand der folgenden Anweisungen zuweisen:

*   [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-replicon"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Replicon

*   Es wird empfohlen, Replicon einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu Replicon müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-replicon"></a>Konfigurieren der automatischen Benutzerbereitstellung für Replicon 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Replicon auf der Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Replicon aktivieren. Befolgen Sie hierzu die Anweisungen im [SSO-Tutorial zu Replicon](active-directory-saas-replicon-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-replicon-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Replicon in Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.

2. Wählen Sie Replicon in der Liste der SaaS-Anwendungen aus.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/Replicon2.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconProvisioningTab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/Replicon1.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** die Werte für **Administratorbenutzername**, **Administratorkennwort**, **Unternehmens-ID** und **Domäne** Ihres Replicon-Kontos ein. Beispiele für diese Werte:

    *   Geben Sie im Feld **Administratorbenutzername** den Benutzernamen des Administratorkontos im Replicon-Mandanten ein. Beispiel: contosoadmin.

    *   Geben Sie im Feld **Administratorkennwort** das Kennwort für den Administratorbenutzernamen ein.

    *   Geben Sie im Feld **Unternehmens-ID** die Unternehmens-ID Ihres Replicon-Mandanten an. Beispiel: Die Unternehmens-ID für die Anmeldung unten ist „Contoso“.

    ![Replicon-Anmeldung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconLogin.png)

    *   Geben Sie im Feld **Domäne** die Domäne wie in Schritt 6 beschrieben an.
    
6. Rufen Sie die **serviceEndpointRootURL** für das Konto Ihres Replicon-Mandanten anhand der in der [Hilfe zum Replicon-Dienst](https://www.replicon.com/help/determining-the-url-for-your-service-calls) beschriebenen Schritte ab. Beim Abrufen der URL ist die **Domäne** die Unterdomäne der hervorgehobenen **serviceEndpointRootURL**. 

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconEndpoint.png)

7. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Replicon herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Replicon-Konto über Teamadministratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconTestConnection.png)

8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconNotificationEmail.png)

9. Klicken Sie auf **Speichern**.

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Replicon synchronisieren** aus.
    
    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconUserMapping.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Replicon synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Replicon für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconUserMappingAtrributes.png)

12. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](./active-directory-saas-scoping-filters.md).

13. Um den Azure AD-Bereitstellungsdienst für Replicon zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconProvisioningStatus.png)

14. Legen Sie die Benutzer und/oder Gruppen fest, die in Replicon bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/UserGroupSelection.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Replicon-Bereitstellung](./media/active-directory-saas-replicon-provisioning-tutorial/SaveProvisioning.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Replicon ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_03.png
