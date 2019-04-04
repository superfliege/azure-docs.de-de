---
title: 'Tutorial: Konfigurieren von Zscaler ZSCloud für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Zscaler ZSCloud konfigurieren.
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
ms.date: 03/03/2019
ms.author: v-ant-msft
ms.openlocfilehash: 3f7fcd59bafe5619a1ef411bf81a6b8c3431f22c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087413"
---
# <a name="tutorial-configure-zscaler-zscloud-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Zscaler ZSCloud für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Zscaler ZSCloud und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Zscaler ZSCloud zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../active-directory-saas-app-provisioning.md).
> 
> Dieser Connector befindet sich derzeit in der Public Preview. Weitere Informationen zu den allgemeinen Microsoft Azure-Nutzungsbedingungen für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial beschriebene Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure AD-Mandanten
*   Einen Zscaler ZSCloud-Mandanten
*   Ein Benutzerkonto in Zscaler ZSCloud mit Administratorberechtigungen

> [!NOTE]
> Die Integration der Azure AD-Bereitstellung basiert auf der Zscaler ZSCloud-SCIM-API, die für Zscaler ZSCloud-Entwickler für Konten mit dem Enterprise-Paket zur Verfügung steht.

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Hinzufügen von Zscaler ZSCloud über den Katalog
Bevor Sie Zscaler ZSCloud für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Zscaler ZSCloud aus dem Azure AD-Anwendungskatalog der Liste mit den verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Zscaler ZSCloud aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Abschnitt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um Zscaler ZSCloud hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Zscaler ZSCloud** ein.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/appsearch.png)

5. Wählen Sie im Ergebnisbereich **Zscaler ZSCloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um Zscaler ZSCloud der Liste mit den SaaS-Anwendungen hinzuzufügen.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/appsearchresults.png)

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/appcreation.png)

## <a name="assigning-users-to-zscaler-zscloud"></a>Zuweisen von Benutzern zu Zscaler ZSCloud

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Zscaler ZSCloud benötigen. Anschließend können Sie diese Benutzer bzw. Gruppen Zscaler ZSCloud anhand der folgenden Anweisungen zuweisen:

*   [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-zscloud"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Zscaler ZSCloud

*   Es wird empfohlen, Zscaler ZSCloud einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu Zscaler ZSCloud müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-zscaler-zscloud"></a>Konfigurieren der automatischen Benutzerbereitstellung in Zscaler ZSCloud

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern bzw. Gruppen in Zscaler ZSCloud auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Zscaler ZSCloud aktivieren. Befolgen Sie dazu die Anweisungen im [SSO-Tutorial zu Zscaler ZSCloud](zscaler-zsCloud-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-zscloud-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für Zscaler ZSCloud in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.

2. Wählen Sie Zscaler ZSCloud in der Liste mit den SaaS-Anwendungen aus.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/appinstancesearch.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/provisioningtab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/provisioningcredentials.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** wie in Schritt 6 beschrieben die **Mandanten-URL** und das **geheime Token** Ihres Zscaler ZSCloud-Kontos ein.

6. Navigieren Sie in der Benutzeroberfläche des Zscaler ZSCloud-Portals zu **Administration (Verwaltung) > Authentication Settings (Authentifizierungseinstellungen)**, und klicken Sie unter **Authentifizierungstyp** auf **SAML**, um die **Mandanten-URL** und das **geheime Token** abzurufen. 

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/secrettoken1.png)

    Klicken Sie auf **Configure SAML** (SAML konfigurieren), um die Optionen zur **SAML-Konfiguration** zu öffnen. 

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/secrettoken2.png)
    
    Wählen Sie **Enable SCIM-Based Provisioning** (SCIM-basierte Bereitstellung aktivieren) aus, um die **Basis-URL** und das **Bearertoken** abzurufen, und speichern Sie anschließend die Einstellungen. Kopieren Sie im Azure-Portal die **Basis-URL** in das Feld **Mandanten-URL** und das **Bearertoken** in das Feld **Geheimes Token**.

7. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Zscaler ZSCloud herstellen kann. Wenn die Verbindung nicht möglich ist, sollten Sie sicherstellen, dass Ihr Zscaler ZSCloud-Konto über Administratorberechtigungen verfügt, und den Vorgang wiederholen.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/testconnection.png)
    
8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/Notification.png)

9. Klicken Sie auf **Speichern**.

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Synchronize Azure Active Directory Users to Zscaler ZSCloud** (Azure Active Directory-Benutzer mit Zscaler ZSCloud synchronisieren) aus.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/usermappings.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Zscaler ZSCloud synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Zscaler ZSCloud für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/userattributemappings.png)

12. Wählen Sie im Abschnitt **Zuordnungen** die Option **Synchronize Azure Active Directory Groups to Zscaler ZSCloud** (Azure Active Directory-Gruppen mit Zscaler ZSCloud synchronisieren) aus.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/groupmappings.png)

13. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Zscaler ZSCloud synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in Zscaler ZSCloud für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/groupattributemappings.png)

14. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](./../active-directory-saas-scoping-filters.md).

15. Ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**, um den Azure AD-Bereitstellungsdienst für Zscaler ZSCloud zu aktivieren.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/provisioningstatus.png)

16. Legen Sie die Benutzer bzw. Gruppen fest, die in Zscaler ZSCloud bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/scoping.png)

17. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Zscaler ZSCloud-Bereitstellung](./media/zscaler-zscloud-provisioning-tutorial/saveprovisioning.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Zscaler ZSCloud ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-zscloud-provisioning-tutorial/tutorial-general-03.png
