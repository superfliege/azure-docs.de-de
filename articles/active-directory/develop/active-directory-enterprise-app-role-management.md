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
ms.openlocfilehash: 88a9f5988d1fe3f4de4fe10da23a5f713e3f3370
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
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

    ![Eigenschaftenseite](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Öffnen Sie den [Azure AD Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster.

    a. Melden Sie sich bei der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators/Co-Admins für Ihren Mandanten an.

    b. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste der Dienstprinzipale von Ihrem Mandanten ab:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Wenn Sie mehrere Verzeichnisse verwenden, sollten Sie diesem Muster folgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    c. Entnehmen Sie der Liste der abgerufenen Dienstprinzipale den Namen, den Sie ändern möchten. Sie können auch mit STRG+F die Anwendung in den aufgeführten Dienstprinzipalen suchen. Suchen Sie nach der **Objekt-ID**, die Sie auf der Seite „Eigenschaften“ kopiert haben, und verwenden Sie folgende Abfrage, um zum jeweiligen Dienstprinzipal zu gelangen.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.

    d. Extrahieren Sie die appRoles-Eigenschaft aus dem Dienstprinzipalobjekt.

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    e. Sie müssen nun neue Rollen für Ihre Anwendung generieren. Sie können den Azure AD Role Generator [hier](https://app.box.com/s/jw6m9p9ehmf4ut5jx8xhcw87cu09ml3y) herunterladen.

    f. Öffnen Sie den Azure AD Role Generator, und führen Sie folgende Schritte aus:

    ![Azure AD Role Generator](./media/active-directory-enterprise-app-role-management/azure_ad_role_generator.png)
    
    Geben Sie **Role Name** (Rollenname), **Role Description** (Rollenbeschreibung) und **Role Value** (Rollenwert) ein. Klicken Sie auf **Add** (Hinzufügen), um die Rolle hinzuzufügen.
    
    Nachdem Sie alle erforderlichen Rollen hinzugefügt haben, klicken Sie auf **Generate** (Generieren).
    
    Kopieren Sie den Inhalt, indem Sie auf **Copy Content** (Inhalt kopieren) klicken.

    > [!NOTE] 
    > Stellen Sie sicher, dass Sie die Benutzerrolle **msiam_access** besitzen und die ID in der generierten Rolle übereinstimmt.

    g. Wechseln Sie zurück zum Graph-Tester. Ändern Sie die Methode von **GET** in **PATCH**. Ändern Sie das Dienstprinzipalobjekt so, dass es über die gewünschten Anwendungsrollen verfügt, indem Sie die appRoles-Eigenschaft mit den kopierten Werten aktualisieren. Klicken Sie auf **Run Query** (Abfrage ausführen).

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-patch.png)

    > [!NOTE]
    > Im Folgenden finden Sie ein Beispiel für ein appRoles-Objekt. 
    > 
    >   ```
    > {
    > "appRoles": [
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "msiam_access",
    >   "displayName": "msiam_access",
    >   "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
    >   "isEnabled": true,
    >   "origin": "Application",
    >   "value": null
    > },
    > {
    >   "allowedMemberTypes": [
    >   "User"
    >   ],
    >   "description": "teacher",
    >   "displayName": "teacher",
    >   "id": "6478ffd2-5dbd-4584-b2ce-137390b09b60",
    >   "isEnabled": ,
    >   "origin": "ServicePrincipal",
    >   "value": "teacher"
    > }
    > ] 
    > }
    >
    >   ```

7. Wenn dem Dienstprinzipal weitere Rollen hinzugefügt wurden, können Sie den jeweiligen Rollen Benutzer zuweisen. Wechseln Sie zu diesem Zweck zum Portal, und navigieren Sie zu der entsprechenden App. Anschließend klicken Sie oben auf die Registerkarte **Benutzer und Gruppen**. Durch diesen Vorgang werden alle Benutzer und Gruppen aufgeführt.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-enterprise-app-role-management/userrole.png)

    a. Um einem Benutzer eine Rolle zuzuweisen, wählen Sie einfach den jeweiligen Benutzer bzw. die Gruppe aus, und klicken Sie im unteren Bereich der Seite auf die Schaltfläche **Zuweisen**.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-enterprise-app-role-management/userandgroups.png)

    b. Durch Klicken auf diese Schaltfläche wird ein Popupfenster geöffnet, in dem Sie eine Rolle aus unterschiedlichen Rollen auswählen können, die für den jeweiligen Dienstprinzipal definiert sind.

    c. Wählen Sie die erforderliche Rolle aus, und klicken Sie dann auf „Senden“.

8. Nachdem den Benutzern Rollen zugewiesen wurden, müssen Sie die Tabelle **Attribute** aktualisieren, um eine angepasste Zuordnung des Anspruchs **Rolle** zu definieren.

9. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der Abbildung gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | -------------- | ----------------|    
    | Rollenname      | user.assignedrole |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

    ![Konfigurieren des SSO-Attributs](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie den **Namespace** leer.
    
    e. Klicken Sie auf **OK**.

10. Um Ihre Anwendung mit einem durch den Identitätsanbieter initiierten Prozess für das einmalige Anmelden zu testen, melden Sie sich beim Zugriffspanel (https://myapps.microsoft.com) an, und klicken Sie auf die Kachel Ihrer Anwendung. Im SAML-Token sollten alle zugewiesenen Rollen für den Benutzer mit dem von Ihnen festgelegten Anspruchsnamen angezeigt werden.

## <a name="update-existing-role"></a>Aktualisieren vorhandener Rollen

1. Führen Sie folgende Schritte aus, um eine vorhandene Rolle zu aktualisieren:

    a. Öffnen Sie den [Azure AD Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster.

    b. Melden Sie sich bei der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators/Co-Admins für Ihren Mandanten an.
    
    c. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste der Dienstprinzipale von Ihrem Mandanten ab:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
        
    Wenn Sie mehrere Verzeichnisse verwenden, sollten Sie diesem Muster folgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Entnehmen Sie der Liste der abgerufenen Dienstprinzipale den Namen, den Sie ändern möchten. Sie können auch mit STRG+F die Anwendung in den aufgeführten Dienstprinzipalen suchen. Suchen Sie nach der **Objekt-ID**, die Sie auf der Seite „Eigenschaften“ kopiert haben, und verwenden Sie folgende Abfrage, um zum jeweiligen Dienstprinzipal zu gelangen.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.
    
    e. Extrahieren Sie die appRoles-Eigenschaft aus dem Dienstprinzipalobjekt.
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)
    
    f. Führen Sie folgende Schritte aus, um die vorhandene Rolle zu aktualisieren:

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)
    
    * Ändern Sie die Methode von **GET** in **PATCH**.

    * Kopieren Sie die vorhandenen Rollen aus der Anwendung, und fügen Sie sie in **Request Body** (Anforderungstext) ein.
    
    * Aktualisieren Sie den Wert der Rolle, indem Sie **Role Description** (Rollenbeschreibung), **Role Value** (Rollenwert) und **Role Display Name** (Anzeigename der Rolle) entsprechend den Anforderungen Ihrer Organisation ersetzen.
    
    * Nachdem Sie alle erforderlichen Rollen aktualisiert haben, klicken Sie auf **Run Query** (Abfrage ausführen).
        
## <a name="delete-existing-role"></a>Löschen einer vorhandener Rolle

1. Führen Sie folgende Schritte aus, um eine vorhandene Rolle zu löschen:

    a. Öffnen Sie den [Azure AD Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster.

    b. Melden Sie sich bei der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators/Co-Admins für Ihren Mandanten an.

    c. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste der Dienstprinzipale von Ihrem Mandanten ab:
    
    `https://graph.microsoft.com/beta/servicePrincipals`
    
    Wenn Sie mehrere Verzeichnisse verwenden, sollten Sie diesem Muster folgen: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer1-updated.png)
    
    d. Entnehmen Sie der Liste der abgerufenen Dienstprinzipale den Namen, den Sie ändern möchten. Sie können auch mit STRG+F die Anwendung in den aufgeführten Dienstprinzipalen suchen. Suchen Sie nach der **Objekt-ID**, die Sie auf der Seite „Eigenschaften“ kopiert haben, und verwenden Sie folgende Abfrage, um zum jeweiligen Dienstprinzipal zu gelangen.
     
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.
    
    e. Extrahieren Sie die appRoles-Eigenschaft aus dem Dienstprinzipalobjekt.
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-approles.png)

    f. Führen Sie folgende Schritte aus, um die vorhandene Rolle zu löschen:

    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-patchdelete.png)

    Ändern Sie die Methode von **GET** in **PATCH**.

    Kopieren Sie die vorhandenen Rollen aus der Anwendung, und fügen Sie sie in **Request Body** (Anforderungstext) ein.
    
    Legen Sie den Wert **IsEnabled** für die Rolle, die Sie löschen möchten, auf **false** fest.

    Klicken Sie auf **Run Query** (Abfrage ausführen).
    
    > [!NOTE] 
    > Stellen Sie sicher, dass Sie die Benutzerrolle **msiam_access** besitzen und die ID in der generierten Rolle übereinstimmt.
    
    g. Nachdem Sie den oben angegebenen Vorgang ausgeführt haben, behalten Sie die Methode als **PATCH** bei, fügen Sie den verbleibenden Rolleninhalt in **Request Body** (Anforderungstext) ein, und klicken Sie auf **Run Query** (Abfrage ausführen).
    
    ![Dialogfeld „Graph-Tester“](./media/active-directory-enterprise-app-role-management/graph-explorer-patchfinal.png)

    h. Nach dem Ausführen der Abfrage wird die Rolle gelöscht.
    
    > [!NOTE]
    > Die Rolle muss zuerst deaktiviert werden, bevor sie entfernt werden kann. 

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Schritte finden Sie in der [App-Dokumentation](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-tutorial-list).

<!--Image references-->
<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png
