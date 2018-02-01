---
title: 'Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS) | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Amazon Web Services (AWS) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/16/2017
ms.author: jeedes
ms.openlocfilehash: 8d77215fd2923e22a9cc87e469cb135d035d22d9
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS)

In diesem Tutorial erfahren Sie, wie Sie Amazon Web Services (AWS) in Azure Active Directory (Azure AD) integrieren.

Die Integration von Amazon Web Services (AWS) in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Amazon Web Services (AWS) Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für Amazon Web Services (AWS) anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Amazon Web Services (AWS) konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein AWS-Abonnement (Amazon Web Services), das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
Zum Konfigurieren der Integration von Amazon Web Services (AWS) in Azure AD müssen Sie Amazon Web Services (AWS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Amazon Web Services (AWS) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Amazon Web Services (AWS)** ein, wählen Sie im Ergebnisbereich **Amazon Web Services (AWS)** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Amazon Web Services (AWS) in der Ergebnisliste](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Amazon Web Services (AWS) basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Amazon Web Services (AWS) als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Amazon Web Services (AWS) muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Amazon Web Services (AWS) den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Amazon Web Services (AWS) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers für Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user)**, um eine Entsprechung von Britta Simon in Amazon Web Services (AWS) zu erhalten, die mit der Benutzerdarstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Amazon Web Services-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Amazon Web Services (AWS) die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Amazon Web Services (AWS)** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. Im Abschnitt **Domäne und URLs für Amazon Web Services (AWS)** muss der Benutzer keine Schritte ausführen, da die App bereits in Azure integriert ist.

    ![SSO-Informationen zur Domäne und zu den URLs für Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Ihre Amazon Web Services-Softwareanwendung erwartet die SAML-Assertions in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

    ![Einmaliges Anmelden konfigurieren, Attribut](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)   

5. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname  | Attributwert | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rolle            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Sie müssen die Benutzerbereitstellung in Azure AD so konfigurieren, dass alle Rollen aus der AWS-Konsole abgerufen werden. Im Folgenden finden Sie die erforderlichen Schritte.

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren, Hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Einmaliges Anmelden konfigurieren, Attribut hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Geben Sie im Textfeld **Namespace** den für die betreffende Zeile angezeigten Namespacewert ein.
    
    d. Klicken Sie auf **OK**.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Melden Sie sich in einem anderen Browserfenster bei der Unternehmenswebsite von Amazon Web Services (AWS) als Administrator an.

9. Klicken Sie auf die **Startseite der Konsole**.
   
    ![Einmaliges Anmelden konfigurieren – Startseite][11]

10. Klicken Sie auf **Identitäts- und Zugriffsverwaltung**. 
   
    ![Einmaliges Anmelden konfigurieren – Identität][12]

11. Klicken Sie auf **Identitätsanbieter**, und klicken Sie dann auf **Anbieter erstellen**. 
   
    ![Einmaliges Anmelden konfigurieren – Anbieter][13]

12. Führen Sie auf der Dialogfeldseite **Anbieter konfigurieren** die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren – Dialogfeld][14]
 
    a. Wählen Sie für **Anbietertyp** die Option **SAML** aus.

    b. Geben Sie im Textfeld **Anbietername** einen Anbieternamen ein (zum Beispiel: *WAAD*).

    c. Klicken Sie auf **Datei auswählen**, um die aus dem Azure-Portal heruntergeladene **Metadatendatei** hochzuladen.

    d. Klicken Sie auf **Nächster Schritt**.

13. Klicken Sie auf der Dialogfeldseite **Anbieterinformationen überprüfen** auf **Erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren – Überprüfen][15]

14. Klicken Sie auf **Rollen**, und klicken Sie dann auf **Neue Rolle erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren – Rollen][16]

15. Führen Sie im Dialogfeld **Rollennamen festlegen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren – Name][17] 

    a. Geben Sie im Textfeld **Rollenname** einen Rollennamen ein (zum Beispiel: *TestUser*). 

    b. Klicken Sie auf **Nächster Schritt**.

16. Führen Sie im Dialogfeld **Rollentyp auswählen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren – Rollentyp][18] 

    a. Wählen Sie **Rolle für Identitätsanbieterzugriff**aus. 

    b. Klicken Sie im Abschnitt **Zugriff mit einmaliger Webanmeldung für SAML-Anbieter gewähren** auf **Auswählen**.

17. Führen Sie im Dialogfeld **Vertrauensstellung herstellen** die folgenden Schritte aus:  
    
    ![Einmaliges Anmelden konfigurieren – Vertrauensstellung][19] 

    a. Wählen Sie als SAML-Anbieter den zuvor erstellten SAML-Anbieter aus (zum Beispiel: *WAAD*). 
  
    b. Klicken Sie auf **Nächster Schritt**.

18. Klicken Sie im Dialogfeld **Vertrauenswürdigkeit der Rolle überprüfen** auf **Nächster Schritt**. 
    
    ![Einmaliges Anmelden konfigurieren – Vertrauenswürdigkeit der Rolle][32]

19. Klicken Sie im Dialogfeld **Richtlinie anhängen** auf **Nächster Schritt**.  
    
    ![Einmaliges Anmelden konfigurieren – Richtlinie][33]

20. Führen Sie im Dialogfeld **Überprüfung** die folgenden Schritte aus:   
    
    ![Einmaliges Anmelden konfigurieren – Überprüfung][34] 

    a. Klicken Sie auf **Rolle erstellen**.

    b. Erstellen Sie so viele Rollen, wie Sie benötigen, und ordnen Sie diese dem Identitätsanbieter zu.

21. Verwenden Sie die Anmeldeinformationen des AWS-Dienstkontos, um die Rollen aus dem AWS-Konto in der Azure AD-Benutzerbereitstellung abzurufen. Öffnen Sie hierzu die Startseite der AWS-Konsole.

22. Klicken Sie auf **Services** (Dienste) -> **Security, Identity & Compliance** (Sicherheit, Identität und Konformität) -> **IAM**.

    ![Rollen aus AWS-Konto abrufen](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. Wählen Sie im IAM-Abschnitt die Registerkarte **Policies** (Richtlinien) aus.

    ![Rollen aus AWS-Konto abrufen](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Erstellen Sie eine neue Richtlinie, indem Sie auf **Create policy** (Richtlinie erstellen) klicken.

    ![Neue Richtlinie erstellen](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Erstellen Sie anhand der folgenden Schritte eine eigene Richtlinie, um alle Rollen aus AWS-Konten abzurufen:

    ![Neue Richtlinie erstellen](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. Klicken Sie im Abschnitt **Richtlinie erstellen** auf die Registerkarte **JSON**.

    b. Fügen Sie in diesem Richtliniendokument den folgenden JSON-Code hinzu.
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam: ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. Klicken Sie auf die Schaltfläche **Richtlinie überprüfen**, um die Richtlinie zu überprüfen.

    ![Neue Richtlinie definieren](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Führen Sie zum Festlegen der **neuen Richtlinie** die folgenden Schritte aus:

    ![Neue Richtlinie definieren](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Als **Policy Name** (Richtlinienname) geben Sie **AzureAD_SSOUserRole_Policy** an.

    b. Sie können für die Richtlinie Text in **Description** (Beschreibung) angeben, da hierfür **This policy will allow to fetch the roles from AWS accounts** (Diese Richtlinie erlaubt das Abrufen der Rollen aus AWS) gilt.
    
    c. Klicken Sie auf die Schaltfläche **Richtlinie erstellen**.
        
27. Erstellen Sie ein neues Benutzerkonto im AWS-IAM-Dienst, indem Sie die folgenden Schritte ausführen:

    a. Klicken Sie im Navigationsmenü der AWS-IAM-Konsole auf **Users** (Benutzer).

    ![Neue Richtlinie definieren](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b. Klicken Sie auf die Schaltfläche **Add user** (Benutzer hinzufügen), um einen neuen Benutzer zu erstellen.

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. Führen Sie im Abschnitt **Add user** (Benutzer hinzufügen) die folgenden Schritte aus:
    
    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Geben Sie **AzureADRoleManager** als Benutzernamen ein.
    
    * Wählen Sie für den Zugriffstyp die Option **Programmatic access** (Programmgesteuerter Zugriff). Auf diese Weise kann der Benutzer die APIs aufrufen und die Rollen aus dem AWS-Konto abrufen.
    
    * Klicken Sie in der unteren rechten Ecke auf die Schaltfläche **Next Permissions** (Nächste Berechtigungen).

28. Nun erstellen Sie eine neue Richtlinie für diesen Benutzer, indem Sie die folgenden Schritte ausführen:

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Klicken Sie auf die Schaltfläche **Attach existing policies directly** (Vorhandene Richtlinien direkt anfügen).

    b. Suchen Sie im Filterabschnitt **AzureAD_SSOUserRole_Policy** nach der neu erstellte Richtlinie.
    
    c. Wählen Sie die **Richtlinie** aus, und klicken Sie dann auf die Schaltfläche **Next: Review** (Weiter: Überprüfung).

29. Überprüfen Sie die Richtlinie für den angefügten Benutzer, indem Sie folgende Schritte ausführen:

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Überprüfen Sie den Benutzernamen, den Zugriffstyp und die Richtlinie, die dem Benutzer zugeordnet wurde.
    
    b. Klicken Sie in der unteren rechten Ecke auf die Schaltfläche **Create user** (Benutzer erstellen), um den Benutzer zu erstellen.

30. Laden Sie die Anmeldeinformationen eines Benutzers herunter, indem Sie die folgenden Schritte ausführen:

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Kopieren Sie **Access key ID** (Zugriffsschlüssel-ID) und **Secret access key** (Geheimer Zugriffsschlüssel) des Benutzers.
    
    b. Geben Sie diese Anmeldeinformationen im Azure AD-Abschnitt für die Benutzerbereitstellung ein, um die Rollen aus der AWS-Konsole abzurufen.
    
    c. Klicken Sie unten auf die Schaltfläche **Close** (Schließen).

31. Navigieren Sie im Azure AD-Verwaltungsportal in der Amazon Web Services-App zum Abschnitt **User Provisioning** (Benutzerbereitstellung).

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Geben Sie den **Zugriffsschlüssel** und den **geheimen Zugriffsschlüssel** in das Feld **Client Secret** (Geheimer Clientschlüssel) bzw. **Secret Token** (Geheimes Token) ein.

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Geben Sie in das Feld **clientsecret** (Geheimer Clientschlüssel) den AWS-Benutzerzugriffsschlüssel ein.
    
    b. Geben Sie in das Feld **Secret Token** (Geheimes Token) die geheimen AWS-Benutzerdaten ein.
    
    c. Klicken Sie auf die Schaltfläche **Test Connection** (Verbindung testen), um sicherzustellen, dass die Verbindung ordnungsgemäß funktioniert.

    d. Speichern Sie die Einstellung, indem Sie oben auf die Schaltfläche **Save** (Speichern) klicken.
 
33. Stellen Sie nun sicher, dass Sie im Abschnitt „Settings“ (Einstellungen) die Option „Provisioning Status“ (Bereitstellungsstatus) über den Schalter **On** (Ein) aktivieren, und klicken Sie dann oben auf die Schaltfläche **Save** (Speichern).

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Erstellen eines Testbenutzers für Amazon Web Services (AWS)

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Amazon Web Services (AWS). Bei Amazon Web Services (AWS) muss für das einmalige Anmelden nicht eigens ein Benutzer im AWS-System erstellt werden, daher ist hier keine Aktion erforderlich.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Amazon Web Services (AWS) gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Amazon Web Services (AWS) zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Amazon Web Services (AWS)** aus.

    ![Der Link „Amazon Web Services (AWS)“ in der Anwendungsliste](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Amazon Web Services (AWS)“ klicken, sollten Sie automatisch in Ihrer Anwendung Amazon Web Services (AWS) angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png

