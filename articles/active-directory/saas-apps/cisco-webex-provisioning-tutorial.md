---
title: 'Tutorial: Konfigurieren von Cisco für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Cisco Webex konfigurieren.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: v-wingf
ms.openlocfilehash: 76a83ef4f647dcf7d79218cb281f1f976b292870
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "42141960"
---
# <a name="tutorial-configure-cisco-webex-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Cisco Webex für die automatische Benutzerbereitstellung


In diesem Tutorial werden die Schritte erläutert, die in Cisco Webex und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern in Cisco Webex zu konfigurieren.


> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

*   Einen Azure AD-Mandanten
*   Einen Cisco Webex-Mandanten
*   Ein Benutzerkonto in Cisco Webex mit Administratorrechten


> [!NOTE]
> Die Azure AD-Bereitstellungsintegration basiert auf dem [Cisco Webex-Webdienst](https://developer.webex.com/getting-started.html), der für Cisco Webex-Teams verfügbar ist.

## <a name="adding-cisco-webex-from-the-gallery"></a>Hinzufügen von Cisco Webex aus dem Katalog
Bevor Sie Cisco Webex für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Cisco Webex aus dem Azure AD-Anwendungskatalog der Liste der verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Cisco Webex aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Abschnitt „Unternehmensanwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um Cisco Webex hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Begriff **Cisco Webex** ein.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/AppSearch.png)

5. Wählen Sie im Ergebnisbereich **Cisco Webex** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um Cisco Webex der Liste der SaaS-Anwendungen hinzuzufügen.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/AppSearchResults.png)

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-webex"></a>Zuweisen von Benutzern zu Cisco Webex

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer in Azure AD Zugriff auf Cisco Webex benötigen. Anschließend können Sie diese Benutzer Cisco Webex zuweisen, indem Sie diese Anweisungen befolgen:

*   [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-webex"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Cisco Webex

*   Es wird empfohlen, Cisco Webex einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu Cisco Webex müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-cisco-webex"></a>Konfigurieren der automatischen Benutzerbereitstellung für Cisco Webex

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern in Cisco Webex auf der Grundlage von Benutzerzuweisungen in Azure AD erläutert.


### <a name="to-configure-automatic-user-provisioning-for-cisco-webex-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Cisco Webex in Azure AD:


1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory > Unternehmensanwendungen > Alle Anwendungen**.

2. Wählen Sie Cisco Webex in der Liste der SaaS-Anwendungen aus.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/Successcenter2.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/ProvisioningTab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/ProvisioningCredentials.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** **-Mandanten-URL** und **Geheimes Token** Ihres Cisco Webex-Kontos ein.

    *   Tragen Sie in das Feld **Mandanten-URL** die Cisco Webex-SCIM-API-URL für Ihren Mandanten ein, die die Form `https://api.webex.com/v1/scim/[Tenant ID]/` hat, wobei `[Tenant ID]` wie in Schritt 6 beschrieben eine alphanumerische Zeichenfolge ist.

    *   Geben Sie im Feld **Geheimes Token** das geheime Token wie in Schritt 6 beschrieben ein.

1. Um **Mandanten-ID** und **Geheimes Token** für Ihr Cisco Webex-Konto zu ermitteln, melden Sie sich mit Ihrem Administratorkonto bei der [Cisco Webex-Entwicklerwebsite](https://developer.webex.com/) an. Sobald Sie angemeldet sind:
    * Gehen Sie zur Seite [Getting Started (Erste Schritte)](https://developer.webex.com/getting-started.html).
    * Scrollen Sie nach unten zum Abschnitt [Authentication (Authentifizierung)](https://developer.webex.com/getting-started.html#authentication)
    ![Cisco Webex Authentication Token (Cisco Webex-Authentifizierungstoken)](./media/cisco-webex-provisioning-tutorial/SecretToken.png).
    * Die alphanumerische Zeichenfolge im Feld ist Ihr **Geheimes Token**. Kopieren Sie dieses Token in die Zwischenablage.
    * Wechseln Sie zu der Seite [Get My Own Details (Meine eigenen Details abrufen)](https://developer.webex.com/endpoint-people-me-get.html).
        * Stellen Sie sicher, dass der Testmodus auf „ON“ (EIN) festgelegt ist.
        * Geben Sie das Wort „Bearer“ gefolgt von einem Leerzeichen ein, und fügen Sie das geheime Token in das Feld „Authorization“ (Autorisierung) ein. ![Cisco Webex-Authentifizierungstoken](./media/cisco-webex-provisioning-tutorial/GetMyDetails.png)
        * Klicken Sie auf „Run“ (Ausführen).
    * Im Antworttext auf der rechten Seite wird die **Mandanten-ID** als „orgId“ angezeigt:

    ```json
    {
        "id": "(...)",
        "emails": [
            "admin.user@contoso.com"
        ],
        "displayName": "John Smith",
        "nickName": "John",
        "orgId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        (...)
    }
    ```

1. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Cisco Webex herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr Cisco Webex-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/TestConnection.png)

8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/EmailNotification.png)

9. Klicken Sie auf **Speichern**.

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Cisco Webex synchronisieren**.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/UserMapping.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Cisco Webex synchronisiert werden. Die als **Übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Cisco Webex für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/UserMappingAttributes.png)

12. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../active-directory-saas-scoping-filters.md).

13. Um den Azure AD-Bereitstellungsdienst für Cisco Webex zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/ProvisioningStatus.png)

14. Legen Sie die Benutzer und/oder Gruppen fest, die in Cisco Webex bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/SyncScope.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Cisco Webex-Bereitstellung](./media/cisco-webex-provisioning-tutorial/Save.png)


Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Cisco Webex ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../active-directory-saas-provisioning-reporting.md).

## <a name="connector-limitations"></a>Connector-Einschränkungen

* Cisco Webex befindet sich derzeit in der EFT-Phase (Early Field Testing) von Cisco. Weitere Informationen erhalten Sie vom [Cisco-Supportteam](https://www.webex.co.in/support/support-overview.html). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_03.png
