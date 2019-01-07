---
title: 'Tutorial: Konfigurieren von Samanage für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Samanage konfigurieren.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2018
ms.author: v-wingf-msft
ms.openlocfilehash: d3442710e1e1327dcafc1b4ed6617aeb7ff1bf0f
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322429"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Samanage für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Samanage und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern bzw. Gruppen in Samanage zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial beschriebene Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure AD-Mandanten
*   Einen [Samanage-Mandanten](https://www.samanage.com/pricing/) mit dem Professional-Tarif
*   Ein Benutzerkonto in Samanage mit Teamadministratorberechtigungen

> [!NOTE]
> Die Integration der Azure AD-Bereitstellung basiert auf der [Samanage-REST-API](https://www.samanage.com/api/), die für Samanage-Entwickler für Konten mit dem Professional-Tarif zur Verfügung steht.

## <a name="adding-samanage-from-the-gallery"></a>Hinzufügen von Samanage über den Katalog
Bevor Sie Samanage für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Samanage aus dem Azure AD-Anwendungskatalog der Liste mit den verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Samanage aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Abschnitt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um Samanage hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld als Suchbegriff **Samanage** ein.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/AppSearch.png)

5. Wählen Sie im Ergebnisbereich **Samanage** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um Samanage der Liste mit den SaaS-Anwendungen hinzuzufügen.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/AppSearchResults.png)

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-samanage"></a>Zuweisen von Benutzern zu Samanage

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer bzw. Gruppen in Azure AD Zugriff auf Samanage benötigen. Anschließend können Sie diese Benutzer bzw. Gruppen Samanage anhand der folgenden Anweisungen zuweisen:

*   [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Samanage

*    Samanage-Rollen werden derzeit automatisch und dynamisch auf der Benutzeroberfläche des Azure-Portals aufgefüllt. Bevor Sie Benutzern Samanage-Rollen zuweisen, muss eine Synchronisierung mit Samanage ausgeführt werden, um die aktuellen Rollen Ihres Samanage-Mandanten abzurufen.

*    Es empfiehlt sich, Samanage einen einzelnen Azure AD-Benutzer zuzuweisen, um die anfängliche Konfiguration der automatischen Benutzerbereitstellung zu testen. Nach erfolgreichem Abschluss der Tests können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu Samanage müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Konfigurieren der automatischen Benutzerbereitstellung für Samanage

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern bzw. Gruppen in Samanage auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Samanage aktivieren. Befolgen Sie hierzu die Anweisungen im [SSO-Tutorial zu Samanage](samanage-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für Samanage in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.

2. Wählen Sie Samanage in der Liste mit den SaaS-Anwendungen aus.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/AppInstanceSearch.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** den **Administratorbenutzernamen** und das **Administratorkennwort** Ihres Samanage-Kontos ein. Beispiele für diese Werte:

    *   Geben Sie im Feld **Administratorbenutzername** den Benutzernamen des Administratorkontos im Samanage-Mandanten ein. Beispiel: admin@contoso.com.

    *   Geben Sie im Feld **Administratorkennwort** das Kennwort des Administratorkontos für den Administratorbenutzernamen ein.

6. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Samanage herstellen kann. Wenn die Verbindung nicht möglich ist, sollten Sie sicherstellen, dass Ihr Samanage-Konto über Administratorberechtigungen verfügt, und den Vorgang wiederholen.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/TestConnection.png)

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. Klicken Sie auf **Speichern**.

9. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Samanage synchronisieren**.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/UserMappings.png)

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Samanage synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Samanage für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. Wählen Sie zum Aktivieren von Gruppenzuordnungen im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Samanage synchronisieren**.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. Legen Sie **Aktiviert** auf **Ja** fest, um Gruppen zu synchronisieren. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Samanage synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Samanage für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Um den Azure AD-Bereitstellungsdienst für Samanage zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. Legen Sie die Benutzer bzw. Gruppen fest, die in Samanage bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus. Berücksichtigen Sie bei Auswahl der Option **Alle Benutzer und Gruppen synchronisieren** die im nachstehenden Abschnitt **Connectoreinschränkungen** beschriebenen Beschränkungen.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Samanage ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-Einschränkungen

* Wenn die Option **Alle Benutzer und Gruppen synchronisieren** ausgewählt und ein Standardwert für das Attribut Samanage-Attribut **roles** konfiguriert wird, stellen Sie sicher, dass der gewünschte Wert unter dem Feld **Standardwert bei NULL (optional)** im folgenden Format angegeben wird: **{"displayName": "role"}**. Hierbei steht „role“ für den gewünschten Standardwert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
