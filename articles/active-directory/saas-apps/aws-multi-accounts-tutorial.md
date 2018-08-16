---
title: 'Tutorial: Azure Active Directory-Integration von Amazon Web Services (AWS) zum Verbinden mehrerer Konten | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure AD und mehreren Konten von Amazon Web Services (AWS) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: jeedes
ms.openlocfilehash: 9634d8ede40500bf0a92ae07a1a514895d355a31
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437932"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Tutorial: Azure Active Directory-Integration von mehreren Amazon Web Services-Konten (AWS-Konten)

In diesem Tutorial erfahren Sie, wie Sie mehrere Konten von Amazon Web Services (AWS) in Azure Active Directory (Azure AD) integrieren.

Die Integration von Amazon Web Services (AWS) in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Amazon Web Services (AWS) Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für Amazon Web Services (AWS) anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Amazon Web Services (AWS) konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD Basic- oder Premium-Abonnement
- Mehrere Amazon Web Services-Konten (AWS-Konten), die für das einmalige Anmelden aktiviert sind

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
Zum Konfigurieren der Integration von Amazon Web Services (AWS) in Azure AD müssen Sie Amazon Web Services (AWS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Amazon Web Services (AWS) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **Amazon Web Services (AWS)** ein, wählen Sie im Ergebnisbereich **Amazon Web Services (AWS)** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Amazon Web Services (AWS) in der Ergebnisliste](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

1. Nachdem die Anwendung hinzugefügt wurde, wechseln Sie zur Seite **Eigenschaften**, und kopieren Sie die **Objekt-ID**.

    ![Amazon Web Services (AWS) in der Ergebnisliste](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Amazon Web Services (AWS) basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Amazon Web Services (AWS) als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Amazon Web Services (AWS) muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Amazon Web Services (AWS) den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Amazon Web Services (AWS) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Amazon Web Services-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Amazon Web Services (AWS) die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Amazon Web Services (AWS)** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

1. Im Abschnitt **Domäne und URLs für Amazon Web Services (AWS)** muss der Benutzer keine Schritte ausführen, da die App bereits in Azure integriert ist.

    ![SSO-Informationen zur Domäne und zu den URLs für Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_url.png)

1. Ihre Amazon Web Services-Softwareanwendung erwartet die SAML-Assertions in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

    ![Konfigurieren des SSO-Attributs](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_attribute.png)    

1. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname  | Attributwert | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rolle            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Sie müssen die Benutzerbereitstellung in Azure AD so konfigurieren, dass alle Rollen aus der AWS-Konsole abgerufen werden. Im Folgenden finden Sie die erforderlichen Schritte.

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/aws-multi-accounts-tutorial/tutorial_attribute_04.png)

    ![Konfigurieren des SSO-Attributs](./media/aws-multi-accounts-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Geben Sie im Textfeld **Namespace** den für die betreffende Zeile angezeigten Namespacewert ein.
    
    d. Klicken Sie auf **OK**.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/aws-multi-accounts-tutorial/tutorial_general_400.png)

1. Melden Sie sich in einem anderen Browserfenster bei der Unternehmenswebsite von Amazon Web Services (AWS) als Administrator an.

1. Klicken Sie auf **AWS Home**.
   
    ![Einmaliges Anmelden konfigurieren – Startseite][11]

1. Klicken Sie auf **IAM** (Identity & Access Management). 
   
    ![Einmaliges Anmelden konfigurieren – Identität][12]

1. Klicken Sie auf **Identitätsanbieter**, und klicken Sie dann auf **Anbieter erstellen**. 
   
    ![Einmaliges Anmelden konfigurieren – Anbieter][13]

1. Führen Sie auf der Dialogfeldseite **Anbieter konfigurieren** die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren – Dialogfeld][14]
 
    a. Wählen Sie für **Anbietertyp** die Option **SAML** aus.

    b. Geben Sie im Textfeld **Anbietername** einen Anbieternamen ein (zum Beispiel: *WAAD*).

    c. Klicken Sie auf **Datei auswählen**, um die aus dem Azure-Portal heruntergeladene **Metadatendatei** hochzuladen.

    d. Klicken Sie auf **Nächster Schritt**.

1. Klicken Sie auf der Dialogfeldseite **Anbieterinformationen überprüfen** auf **Erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren – Überprüfen][15]

1. Klicken Sie auf **Rollen**, und klicken Sie dann auf **Rolle erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren – Rollen][16]

1. Führen Sie auf der Seite **Rolle erstellen** die folgenden Schritte aus:  
    
    ![Einmaliges Anmelden konfigurieren – Vertrauensstellung][19] 

    a. Wählen Sie **SAML 2.0-Verbund** unter **Typ der vertrauenswürdige Entität auswählen**.

    b. Wählen Sie unter **SAML 2.0-Anbieterabschnitt auswählen** den **SAML-Anbieter** aus, den Sie zuvor erstellt haben (Beispiel: *WAAD*).

    c. Wählen Sie **Programmgesteuerten und AWS Management Console-Zugriff zulassen** aus.
  
    d. Klicken Sie auf **Weiter: Berechtigungen**.

1. Klicken Sie im Dialogfeld **Berechtigungsrichtlinien anfügen** auf **Weiter: Überprüfen**.  
    
    ![Einmaliges Anmelden konfigurieren – Richtlinie][33]

1. Führen Sie im Dialogfeld **Überprüfung** die folgenden Schritte aus:   
    
    ![Einmaliges Anmelden konfigurieren – Überprüfung][34] 

    a. Geben Sie im Textfeld **Rollenname** Ihren Rollennamen ein.

    b. Geben Sie im Textfeld **Rollenbeschreibung** die Beschreibung ein.

    a. Klicken Sie auf **Rolle erstellen**.

    b. Erstellen Sie so viele Rollen, wie Sie benötigen, und ordnen Sie diese dem Identitätsanbieter zu.

1. Melden Sie sich vom aktuellen AWS-Konto ab, und melden Sie sich mit einem anderen Konto an, für das Sie das einmalige Anmelden mit Azure AD konfigurieren möchten.

1. Führen Sie die Schritte 9 bis 17 aus, um mehrere Rollen zu erstellen, die Sie für dieses Konto einrichten möchten. Wenn Sie über mehr als zwei Konten verfügen, führen Sie dieselben Schritte für alle Konten aus, um Rollen für sie zu erstellen.

1. Nach der Erstellung aller Rollen in den Konten werden sie in der Liste **Rollen** für diese Konten angezeigt.

    ![Rolleneinrichtung](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

1. Wir müssen alle Rollen-ARN und vertrauenswürdigen Entitäten für alle Rollen in allen Konten erfassen, die der Azure AD-Anwendung manuell zugeordnet werden müssen. 

1. Klicken Sie auf die Rollen, um die Werte **Rollen-ARN** und **Vertrauenswürdige Entitäten** zu kopieren. Sie benötigen diese Werte für alle Rollen, die Sie in Azure AD erstellen müssen.

    ![Rolleneinrichtung](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)
 
1. Führen Sie den obigen Schritt für alle Rollen in allen Konten aus, und speichern Sie alle im Format **Rollen-ARN, vertrauenswürdige Entitäten** in Editor. 

1. Öffnen Sie den [Azure AD Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster.

    a. Melden Sie sich bei der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators/Co-Admins für Ihren Mandanten an.

    b. Sie müssen über ausreichende Berechtigungen zum Erstellen der Rollen verfügen. Klicken Sie auf **Berechtigungen ändern**, um die erforderlichen Berechtigungen zu erhalten. 

    ![Dialogfeld „Graph-Tester“](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Wählen Sie in der Liste die folgenden Berechtigungen aus (sofern Sie noch nicht darüber verfügen), und klicken Sie auf „Berechtigungen ändern“. 

    ![Dialogfeld „Graph-Tester“](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Darin werden Sie aufgefordert, sich erneut anzumelden und die Zustimmungserklärung zu akzeptieren. Nachdem Sie die Zustimmungserklärung akzeptiert haben, werden Sie erneut beim Graph-Tester angemeldet.

    e. Ändern Sie die Dropdownliste für die Version in **Beta**. Verwenden Sie die folgende Abfrage, um alle Dienstprinzipale aus Ihrem Mandanten abzurufen:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Wenn Sie mehrere Verzeichnisse verwenden, können Sie das folgende Muster nutzen, das Ihre primäre Domäne enthält: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`.
    
    ![Dialogfeld „Graph-Tester“](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)
    
    f. Entnehmen Sie der Liste der abgerufenen Dienstprinzipale den Namen, den Sie ändern möchten. Sie können auch mit STRG+F die Anwendung in den aufgeführten Dienstprinzipalen suchen. Sie können die folgende Abfrage mit der **Objekt-ID** verwenden, die Sie auf der Seite der Azure AD-Eigenschaften kopiert haben, um zum jeweiligen Dienstprinzipal zu gelangen.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist.

    ![Dialogfeld „Graph-Tester“](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Extrahieren Sie die appRoles-Eigenschaft aus dem Dienstprinzipalobjekt. 

    ![Dialogfeld „Graph-Tester“](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

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
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > Neue Rollen können nur nach der Rolle **msiam_access** für den Patchvorgang hinzugefügt werden. Darüber hinaus können Sie je nach Bedarf Ihrer Organisation beliebig viele Rollen hinzufügen. Azure AD sendet den **Wert** dieser Rollen als Anspruchswert in der SAML-Antwort.
    
    j. Wechseln Sie wieder zum Graph-Tester, und ändern Sie die Methode von **GET** in **PATCH**. Patchen Sie das Dienstprinzipalobjekt so, dass es über die gewünschten Rollen verfügt, indem Sie die appRoles-Eigenschaft entsprechend der oben im Beispiel gezeigten Eigenschaft aktualisieren. Klicken Sie auf **Abfrage ausführen**, um den Patchvorgang auszuführen. In einer Erfolgsmeldung wird die Erstellung der Rolle für Ihre Amazon Web Services-Anwendung bestätigt.

    ![Dialogfeld „Graph-Tester“](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. Nachdem der Dienstprinzipal mit weiteren Rollen gepatcht wurde, können Sie den jeweiligen Rollen Benutzer/Gruppen zuweisen. Wechseln Sie zu diesem Zweck zum Portal, und navigieren Sie zur Amazon Web Services-Anwendung. Klicken Sie oben auf die Registerkarte **Benutzer und Gruppen**. 

1. Es empfiehlt sich, neue Gruppen für jede AWS-Rolle zu erstellen, damit Sie die jeweilige Rolle in dieser Gruppe zuweisen können. Beachten Sie, dass es sich hierbei um eine 1: 1-Zuordnung handelt, bei der einer Gruppe eine Rolle zugeordnet ist. Anschließend können Sie die Mitglieder hinzufügen, die dieser Gruppe angehören.

1. Nachdem die Gruppen erstellt wurden, können Sie die Gruppe auswählen und der Anwendung zuweisen. 

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

1. Um der Gruppe die Rolle zuzuweisen, wählen Sie die Rolle aus, und klicken Sie unten auf der Seite auf die Schaltfläche **Zuweisen**.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

> [!Note]
> Beachten Sie, dass Sie die Sitzung im Azure-Portal aktualisieren müssen, damit neue Rollen angezeigt werden.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Amazon Web Services (AWS)“ klicken, sollte die Amazon Web Services-Anwendungsseite mit der Option zum Auswählen der Rolle angezeigt werden.

![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Sie können auch die SAML-Antwort überprüfen, um die als Ansprüche übergebenen Rollen anzuzeigen.

![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/aws-multi-accounts-tutorial/tutorial_general_01.png
[2]: ./media/aws-multi-accounts-tutorial/tutorial_general_02.png
[3]: ./media/aws-multi-accounts-tutorial/tutorial_general_03.png
[4]: ./media/aws-multi-accounts-tutorial/tutorial_general_04.png

[100]: ./media/aws-multi-accounts-tutorial/tutorial_general_100.png

[200]: ./media/aws-multi-accounts-tutorial/tutorial_general_200.png
[201]: ./media/aws-multi-accounts-tutorial/tutorial_general_201.png
[202]: ./media/aws-multi-accounts-tutorial/tutorial_general_202.png
[203]: ./media/aws-multi-accounts-tutorial/tutorial_general_203.png
[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_on.png

