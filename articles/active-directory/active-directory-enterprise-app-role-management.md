---
title: Konfigurieren von im SAML-Token ausgestellten Rollenansprüchen für Unternehmensanwendungen in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie im SAML-Token ausgestellte Rollenansprüche für Unternehmensanwendungen in Azure Active Directory konfigurieren.
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.assetid: eb2b3741-3cde-45c8-b639-a636f3df3b74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: d869a21230661e473ffff6bbdb1ea29b1ea336d5
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="configuring-role-claim-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Konfigurieren von im SAML-Token ausgestellten Rollenansprüchen für Unternehmensanwendungen in Azure Active Directory

Dieses Feature ermöglicht Benutzern, den Anspruchstyp für Rollenansprüche im Antworttoken, das für das Autorisieren einer App mithilfe von Azure AD empfangen wurde, anzupassen.

## <a name="prerequisites"></a>Voraussetzungen
- Ein Azure AD-Abonnement mit eingerichtetem Verzeichnis
- Ein Abonnement, das für das einmalige Anmelden aktiviert ist
- Sie müssen SSO mit Ihrer Anwendung konfigurieren

## <a name="when-to-use-this-feature"></a>Verwendung dieses Features

Wenn die Anwendung erwartet, dass benutzerdefinierte Rollen in SAML-Antworten übergeben werden, müssen Sie dieses Feature verwenden. Mit diesem Feature können Sie so viele Rollen erstellen, wie von Azure AD an Ihre Anwendung zurückgegeben werden müssen.

## <a name="steps-to-use-this-feature"></a>Schritte zur Verwendung dieses Features

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Namen Ihrer Anwendung ein, wählen Sie die Anwendung im Ergebnispanel aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um sie hinzuzufügen.

    ![Anwendung in der Ergebnisliste](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Nachdem die Anwendung hinzugefügt wurde, wechseln Sie zur Seite **Eigenschaften**, und kopieren Sie die **Objekt-ID**.

    ![Eigenschaftenseite](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.PNG)

6. Öffnen Sie den [Azure AD Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster.

    a. Melden Sie sich bei der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators/Co-Admins für Ihren Mandanten an.

    b. Sie müssen über ausreichende Berechtigungen zum Erstellen der Rollen verfügen. Klicken Sie auf **Berechtigungen ändern**, um die erforderlichen Berechtigungen zu erhalten. 

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

    c. Wählen Sie in der Liste die folgenden Berechtigungen aus (sofern Sie noch nicht darüber verfügen), und klicken Sie auf „Berechtigungen ändern“. 

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    d. Darin werden Sie aufgefordert, sich erneut anzumelden und die Zustimmungserklärung zu akzeptieren. Nachdem Sie die Zustimmungserklärung akzeptiert haben, werden Sie erneut beim System angemeldet.

    e. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste der Dienstprinzipale von Ihrem Mandanten ab:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Wenn Sie mehrere Verzeichnisse verwenden, sollten Sie diesem Muster folgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
    f. Entnehmen Sie der Liste der abgerufenen Dienstprinzipale den Namen, den Sie ändern möchten. Sie können auch mit STRG+F die Anwendung in den aufgeführten Dienstprinzipalen suchen. Suchen Sie nach der **Objekt-ID**, die Sie auf der Seite „Eigenschaften“ kopiert haben, und verwenden Sie folgende Abfrage, um zum jeweiligen Dienstprinzipal zu gelangen.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    g. Extrahieren Sie die appRoles-Eigenschaft aus dem Dienstprinzipalobjekt. 

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

    > [!Note]
    > Wenn Sie die benutzerdefinierte (nicht im Katalog enthaltene) App verwenden, sehen Sie die zwei Standardrollen: „User“ und „msiam_access“. Bei einer Katalog-App ist „msiam_access“ die einzige Standardrolle. Sie müssen keine Änderungen in den Standardrollen vornehmen.

    h. Jetzt müssen Sie neue Rollen für Ihre Anwendung generieren. 

    i. Der nachfolgende JSON-Code ist ein Beispiel für ein appRoles-Objekt. Erstellen Sie ein ähnliches Objekt, um die gewünschten Rollen für Ihre Anwendung hinzuzufügen. 

    ```
    {
       "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "b9632174-c057-4f7e-951b-be3adc52bfe6",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Administrators Only",
            "displayName": "Admin",
            "id": "4f8f8640-f081-492d-97a0-caf24e9bc134",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "Administrator"
        }
    ],
    }
    ```
    > [!Note]
    > Neue Rollen können nur nach der Rolle **msiam_access** für den Patchvorgang hinzugefügt werden. Darüber hinaus können Sie je nach Bedarf Ihrer Organisation beliebig viele Rollen hinzufügen. Azure AD sendet den **Wert** dieser Rollen als Anspruchswert in der SAML-Antwort.
    
    j. Wechseln Sie wieder zum Graph-Tester, und ändern Sie die Methode von **GET** in **PATCH**. Patchen Sie das Dienstprinzipalobjekt so, dass es über die gewünschten Rollen verfügt, indem Sie die appRoles-Eigenschaft entsprechend der oben im Beispiel gezeigten Eigenschaft aktualisieren. Klicken Sie auf **Abfrage ausführen**, um den Patchvorgang auszuführen. Die Erstellung der Rolle wird in einer Erfolgsmeldung bestätigt.

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

7. Nachdem der Dienstprinzipal mit weiteren Rollen gepatcht wurde, können Sie den jeweiligen Rollen Benutzer zuweisen. Wechseln Sie zu diesem Zweck zum Portal, und navigieren Sie zu der entsprechenden App. Klicken Sie oben auf die Registerkarte **Benutzer und Gruppen**. Dadurch werden alle Benutzer und Gruppen angezeigt, die der Anwendung bereits zugewiesen sind. Sie können neue Benutzer für die neue Rolle hinzufügen und zudem den vorhandenen Benutzer auswählen und auf **Bearbeiten** klicken, um die Rolle zu ändern.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

     Um die Rolle einem Benutzer zuzuweisen, wählen Sie die neue Rolle aus, und klicken Sie unten auf der Seite auf die Schaltfläche **Zuweisen**.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    > [!Note]
    > Beachten Sie, dass Sie die Sitzung im Azure-Portal aktualisieren müssen, damit neue Rollen angezeigt werden.

8. Nachdem den Benutzern Rollen zugewiesen wurden, müssen Sie die Tabelle **Attribute** aktualisieren, um eine angepasste Zuordnung des Anspruchs **Rolle** zu definieren.

9. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | -------------- | ----------------|    
    | Rollenname      | user.assignedrole |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Konfigurieren des SSO-Attributs](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den gewünschten Attributnamen ein. In diesem Beispiel wurde **Role Name** als Anspruchsname verwendet.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie den **Namespace** leer.
    
    e. Klicken Sie auf **OK**.

10. Um Ihre Anwendung mit einem durch den Identitätsanbieter initiierten Prozess für das einmalige Anmelden zu testen, melden Sie sich beim Zugriffspanel (https://myapps.microsoft.com) an, und klicken Sie auf die Kachel Ihrer Anwendung. Im SAML-Token sollten alle zugewiesenen Rollen für den Benutzer mit dem von Ihnen festgelegten Anspruchsnamen angezeigt werden.

## <a name="update-existing-role"></a>Aktualisieren vorhandener Rollen

Führen Sie folgende Schritte aus, um eine vorhandene Rolle zu aktualisieren:

1. Öffnen Sie den [Azure AD Graph-Tester](https://developer.microsoft.com/graph/graph-explorer).

2. Melden Sie sich bei der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators/Co-Admins für Ihren Mandanten an.
    
3. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste der Dienstprinzipale von Ihrem Mandanten ab:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Wenn Sie mehrere Verzeichnisse verwenden, sollten Sie diesem Muster folgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Entnehmen Sie der Liste der abgerufenen Dienstprinzipale den Namen, den Sie ändern möchten. Sie können auch mit STRG+F die Anwendung in den aufgeführten Dienstprinzipalen suchen. Suchen Sie nach der **Objekt-ID**, die Sie auf der Seite „Eigenschaften“ kopiert haben, und verwenden Sie folgende Abfrage, um zum jeweiligen Dienstprinzipal zu gelangen.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extrahieren Sie die appRoles-Eigenschaft aus dem Dienstprinzipalobjekt.
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)
    
6. Führen Sie folgende Schritte aus, um die vorhandene Rolle zu aktualisieren:

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * Ändern Sie die Methode von **GET** in **PATCH**.

    * Kopieren Sie die vorhandenen Rollen, und fügen Sie sie in **Request Body** (Anforderungstext) ein.

    * Aktualisieren Sie den Wert der Rolle, indem Sie **Role description** (Rollenbeschreibung), **Role value** (Rollenwert) oder **Role displayname** (Anzeigename der Rolle) bei Bedarf ändern.

    * Nachdem Sie alle erforderlichen Rollen aktualisiert haben, klicken Sie auf **Run Query** (Abfrage ausführen).
        
## <a name="delete-existing-role"></a>Löschen einer vorhandener Rolle

Führen Sie folgende Schritte aus, um eine vorhandene Rolle zu löschen:

1. Öffnen Sie den [Azure AD Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster.

2. Melden Sie sich bei der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators/Co-Admins für Ihren Mandanten an.

3. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste der Dienstprinzipale von Ihrem Mandanten ab:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Wenn Sie mehrere Verzeichnisse verwenden, sollten Sie diesem Muster folgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)
    
4. Entnehmen Sie der Liste der abgerufenen Dienstprinzipale den Namen, den Sie ändern möchten. Sie können auch mit STRG+F die Anwendung in den aufgeführten Dienstprinzipalen suchen. Suchen Sie nach der **Objekt-ID**, die Sie auf der Seite „Eigenschaften“ kopiert haben, und verwenden Sie folgende Abfrage, um zum jeweiligen Dienstprinzipal zu gelangen.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)
    
5. Extrahieren Sie die appRoles-Eigenschaft aus dem Dienstprinzipalobjekt.
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

6. Führen Sie folgende Schritte aus, um die vorhandene Rolle zu löschen:

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    * Ändern Sie die Methode von **GET** in **PATCH**.

    * Kopieren Sie die vorhandenen Rollen aus der Anwendung, und fügen Sie sie in **Request Body** (Anforderungstext) ein.
        
    * Legen Sie den Wert **IsEnabled** für die Rolle, die Sie löschen möchten, auf **false** fest.

    * Klicken Sie auf **Run Query** (Abfrage ausführen).
    
    > [!NOTE] 
    > Stellen Sie sicher, dass Sie die Benutzerrolle **msiam_access** besitzen und die ID in der generierten Rolle übereinstimmt.
    
7. Löschen Sie nach dem Deaktivieren der Rolle den entsprechenden Rollenblock im appRoles-Abschnitt, behalten Sie die Methode **PATCH** bei, und klicken Sie auf **Abfrage ausführen**.
    
8. Nach dem Ausführen der Abfrage wird die Rolle gelöscht.
    
    > [!NOTE]
    > Die Rolle muss zuerst deaktiviert werden, bevor sie entfernt werden kann. 

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Schritte finden Sie in der [App-Dokumentation](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
