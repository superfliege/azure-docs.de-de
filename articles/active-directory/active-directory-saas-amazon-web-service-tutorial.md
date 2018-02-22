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
ms.openlocfilehash: 0ff14365323d66a101e5847d7959045c3f20dea2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS)

In diesem Tutorial erfahren Sie, wie Sie Amazon Web Services (AWS) in Azure Active Directory (Azure AD) integrieren.

Die Integration von Amazon Web Services (AWS) in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Amazon Web Services (AWS) Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für Amazon Web Services (AWS) anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Amazon Web Services (AWS) konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein AWS-Abonnement (Amazon Web Services), das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [kostenlose einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-amazon-web-services-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
Zum Konfigurieren der Integration von Amazon Web Services (AWS) in Azure AD müssen Sie Amazon Web Services (AWS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Amazon Web Services (AWS) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** das Symbol für **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Wählen Sie oben im Dialogfeld die Schaltfläche **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld als Suchbegriff **Amazon Web Services (AWS)** ein. Wählen Sie im Ergebnisbereich die Option **Amazon Web Services (AWS)** aus, und wählen Sie dann die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Amazon Web Services (AWS) in der Ergebnisliste](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Amazon Web Services (AWS) basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD Kenntnis darüber haben, welcher Benutzer in Amazon Web Services (AWS) als Pendant für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und einem entsprechenden Benutzer in Amazon Web Services (AWS) muss ein Link eingerichtet werden.

Um den Link herzustellen, geben Sie in Amazon Web Services (AWS) in **Benutzername** denselben Wert wie unter **Benutzername** in Azure AD ein. 

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Amazon Web Services (AWS) führen Sie die folgenden Bausteine aus:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen eines Testbenutzers für Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user), um ein Pendant von Britta Simon in Amazon Web Services (AWS) zu erhalten, das mit der Benutzerdarstellung in Azure AD verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen des einmaligen Anmeldens](#test-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Amazon Web Services-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Amazon Web Services (AWS) die folgenden Schritte aus:**

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Amazon Web Services (AWS)** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie zum Aktivieren des einmaligen Anmeldens im Dialogfeld **Einmaliges Anmelden** in der Dropdownliste **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. Im Abschnitt **Domäne und URLs für Amazon Web Services (AWS)** muss der Benutzer keine Schritte ausführen, da die App bereits in Azure integriert ist.

    ![SSO-Informationen zur Domäne und zu den URLs für Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Ihre Amazon Web Services-Softwareanwendung erwartet die SAML-Assertions in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Im folgenden Screenshot wird ein Beispiel gezeigt:

    ![Konfigurieren des SSO-Attributs](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. Konfigurieren Sie das SAML-Tokenattribut im Bereich **Benutzerattribute** im Feld **Einmaliges Anmelden**, wie in der obigen Abbildung dargestellt. Führen Sie anschließend die folgenden Schritte aus:
    
    | Attributname  | Attributwert | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rolle            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Konfigurieren Sie die Benutzerbereitstellung in Azure AD so, dass alle Rollen aus der Konsole von Amazon Web Services (AWS) abgerufen werden. Beachten Sie die folgenden Bereitstellungsschritte.

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Attribut hinzufügen** zu öffnen.

    ![Konfigurieren des SSO-Attributs](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Konfigurieren des SSO-Attributs](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Feld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Attributwert ein.

    d. Geben Sie im Feld **Namespace** den für die betreffende Zeile angezeigten Namespacewert ein.
    
    d. Klicken Sie auf **OK**.

6. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Metadaten-XML** aus. Speichern Sie dann die Metadatendatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Wählen Sie **Speichern**aus.

    ![Einmaliges Anmelden konfigurieren: Schaltfläche „Speichern“](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Melden Sie sich in einem anderen Browserfenster bei der Unternehmenswebsite von Amazon Web Services (AWS) als Administrator an.

9. Wählen Sie **Startseite der Konsole**.
   
    ![Einmaliges Anmelden konfigurieren – Startseite][11]

10. Wählen Sie **Identitäts- und Zugriffsverwaltung**. 
   
    ![Einmaliges Anmelden konfigurieren – Identität][12]

11. Wählen Sie **Identitätsanbieter**. Wählen Sie dann **Anbieter erstellen**. 
   
    ![Einmaliges Anmelden konfigurieren – Anbieter][13]

12. Führen Sie im Dialogfeld **Anbieter konfigurieren** die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren – Dialogfeld][14]
 
    a. Wählen Sie für **Anbietertyp** die Option **SAML**.

    b. Geben Sie im Feld **Anbietername** einen Anbieternamen ein (zum Beispiel: *WAAD*).

    c. Wählen Sie **Datei auswählen**, um die aus dem Azure-Portal heruntergeladene **Metadatendatei** hochzuladen.

    d. Wählen Sie **Nächster Schritt**.

13. Wählen Sie im Dialogfeld **Anbieterinformationen überprüfen** die Option **Erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren – Überprüfung][15]

14. Wählen Sie **Rollen** aus. Wählen Sie anschließend **Neue Rolle erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren – Rollen][16]

15. Führen Sie im Dialogfeld **Rollennamen festlegen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren – Name][17] 

    a. Geben Sie im Feld **Rollenname** einen Rollennamen ein (zum Beispiel: *TestUser*). 

    b. Wählen Sie **Nächster Schritt**.

16. Führen Sie im Dialogfeld **Rollentyp auswählen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren – Rollentyp][18] 

    a. Wählen Sie **Rolle für Identitätsanbieterzugriff**aus. 

    b. Klicken Sie im Abschnitt **Zugriff mit einmaliger Webanmeldung für SAML-Anbieter gewähren** auf **Auswählen**.

17. Führen Sie im Dialogfeld **Vertrauensstellung herstellen** die folgenden Schritte aus:  
    
    ![Einmaliges Anmelden konfigurieren – Vertrauensstellung][19] 

    a. Wählen Sie den zuvor erstellten SAML-Anbieter aus (zum Beispiel: *WAAD*). 
  
    b. Wählen Sie **Nächster Schritt**.

18. Wählen Sie im Dialogfeld **Vertrauenswürdigkeit der Rolle überprüfen** die Option **Nächster Schritt**. 
    
    ![Einmaliges Anmelden konfigurieren – Vertrauenswürdigkeit der Rolle][32]

19. Wählen Sie im Dialogfeld **Richtlinie anhängen** die Option **Nächster Schritt**.  
    
    ![Einmaliges Anmelden konfigurieren – Richtlinie][33]

20. Führen Sie im Dialogfeld **Überprüfung** die folgenden Schritte aus:   
    
    ![Einmaliges Anmelden konfigurieren – Überprüfung][34] 

    a. Wählen Sie **Rolle erstellen**.

    b. Erstellen Sie so viele Rollen, wie erforderlich, und ordnen Sie diese dann dem Identitätsanbieter zu.

21. Verwenden Sie die Anmeldeinformationen des Dienstkontos von Amazon Web Services (AWS), um die Rollen aus diesem Konto in der Azure AD-Benutzerbereitstellung abzurufen. Öffnen Sie für diese Aufgabe die Startseite der Konsole von Amazon Web Services (AWS).

22. Wählen Sie **Services** (Dienste) > **Security, Identity & Compliance** (Sicherheit, Identität und Konformität) > **IAM**.

    ![Abrufen der Rollen vom AWS-Konto](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. Wählen Sie im IAM-Abschnitt die Registerkarte **Policies** (Richtlinien).

    ![Abrufen der Rollen vom AWS-Konto](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Wählen Sie zum Erstellen einer neuen Richtlinie die Option **Create policy** (Richtlinie erstellen).

    ![Erstellen einer neuen Richtlinie](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Um eine eigene Richtlinie zum Abrufen aller Rollen aus den Konten von Amazon Web Services (AWS) zu erstellen, führen Sie die folgenden Schritte aus:

    ![Erstellen einer neuen Richtlinie](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. Wählen Sie im Abschnitt **Richtlinie erstellen** die Registerkarte **JSON**.

    b. Fügen Sie in diesem Richtliniendokument den folgenden JSON-Code hinzu:
    
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

    c. Zum Überprüfen der Richtlinie wählen Sie die Schaltfläche **Richtlinie überprüfen**.

    ![Neue Richtlinie definieren](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Führen Sie zum Festlegen der **neuen Richtlinie** die folgenden Schritte aus:

    ![Neue Richtlinie definieren](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Als **Policy Name** (Richtlinienname) geben Sie **AzureAD_SSOUserRole_Policy** an.

    b. Sie können für die folgende **Description** (Beschreibung) für die Richtlinie angeben: **This policy will allow to fetch the roles from AWS accounts** (Diese Richtlinie erlaubt das Abrufen der Rollen aus AWS).
    
    c. Wählen Sie die Schaltfläche **Richtlinie erstellen**.
        
27. Erstellen Sie ein neues Benutzerkonto im IAM-Dienst von Amazon Web Services (AWS), indem Sie die folgenden Schritte ausführen:

    a. Wählen Sie in der IAM-Konsole von Amazon Web Services (AWS) die Option **Benutzer**.

    ![Neue Richtlinie definieren](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b. Wählen Sie zum Erstellen eines neuen Benutzers die Schaltfläche **Add user** (Benutzer hinzufügen).

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. Führen Sie im Abschnitt **Add User** (Benutzer hinzufügen) die folgenden Schritte aus:
    
    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Geben Sie **AzureADRoleManager** im Feld für den Benutzernamen ein.
    
    * Wählen Sie für den Zugriffstyp die Option **Programmatic access** (Programmgesteuerter Zugriff). Auf diese Weise kann der Benutzer die APIs aufrufen und die Rollen aus dem Konto in Amazon Web Services (AWS) abrufen.
    
    * Wählen Sie in der unteren rechten Ecke die Schaltfläche **Next Permissions** (Nächste Berechtigungen).

28. Erstellen Sie eine neue Richtlinie für diesen Benutzer, indem Sie die folgenden Schritte ausführen:

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Wählen Sie die Schaltfläche **Attach existing policies directly** (Vorhandene Richtlinien direkt anfügen).

    b. Suchen Sie im Filterabschnitt **AzureAD_SSOUserRole_Policy** nach der neu erstellte Richtlinie.
    
    c. Wählen Sie die **Richtlinie**. Wählen Sie dann die Schaltfläche **Next: Review** (Weiter: Überprüfung).

29. Überprüfen Sie die Richtlinie für den angefügten Benutzer, indem Sie folgende Schritte ausführen:

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Überprüfen Sie den Benutzernamen, den Zugriffstyp und die Richtlinie, die dem Benutzer zugeordnet wurden.
    
    b. Wählen Sie zum Erstellen des Benutzers in der unteren rechten Ecke die Schaltfläche **Create user** (Benutzer erstellen).

30. Laden Sie die Anmeldeinformationen eines Benutzers herunter, indem Sie die folgenden Schritte ausführen:

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Kopieren Sie **Access key ID** (Zugriffsschlüssel-ID) und **Secret access key** (Geheimer Zugriffsschlüssel) des Benutzers.
    
    b. Geben Sie diese Anmeldeinformationen im Azure AD-Abschnitt für die Benutzerbereitstellung ein, um die Rollen aus der Konsole von Amazon Web Services (AWS) abzurufen.
    
    c. Wählen Sie in der unteren rechten Ecke die Schaltfläche **Schließen**.

31. Navigieren Sie im Azure AD-Verwaltungsportal in der Amazon Web Services (AWS)-App zum Abschnitt **User Provisioning** (Benutzerbereitstellung).

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Geben Sie den **Zugriffsschlüssel** und den **geheimen Zugriffsschlüssel** in das Feld **Client Secret** (Geheimer Clientschlüssel) bzw. **Secret Token** (Geheimes Token) ein.

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Geben Sie in das Feld **clientsecret** (Geheimer Clientschlüssel) den Benutzerzugriffsschlüssel für Amazon Web Services (AWS) ein.
    
    b. Geben Sie in das Feld **Secret Token** (Geheimes Token) die geheimen Benutzerdaten für Amazon Web Services (AWS) ein.
    
    c. Wählen Sie die Schaltfläche **Test connection** (Verbindung testen). Der Verbindungstest sollte erfolgreich sein.

    d. Speichern Sie die Einstellung, indem Sie oben die Schaltfläche **Save** (Speichern) wählen.
 
33. Stellen Sie sicher, dass Sie den Bereitstellungstatus in **Settings** (Einstellungen)**** aktivieren. Wählen Sie hierzu **On** (Ein), und wählen Sie dann oben die Schaltfläche **Save** (Speichern).

    ![Benutzer hinzufügen](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App aus dem Bereich **Active Directory > Enterprise Applications** hinzugefügt haben, wählen Sie die Registerkarte **Einmaliges Anmelden** aus. Öffnen Sie dann die eingebettete Dokumentation über den Bereich **Konfiguration** am unteren Rand. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Wählen Sie im linken Bereich des Azure-Portals die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen**. Wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Erstellen eines Testbenutzers für Amazon Web Services (AWS)

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Amazon Web Services (AWS). Bei Amazon Web Services (AWS) muss für das einmalige Anmelden nicht eigens ein Benutzer im AWS-System erstellt werden, daher ist hier keine Aktion erforderlich.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Amazon Web Services (AWS) gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Amazon Web Services (AWS) zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Wechseln Sie dann zur Verzeichnisansicht und wählen Sie **Unternehmensanwendungen**. Wählen Sie anschließend **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Amazon Web Services (AWS)** aus.

    ![Der Link „Amazon Web Services (AWS)“ in der Anwendungsliste](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**. 

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Amazon Web Services (AWS)“ wählen, sollten Sie automatisch in Ihrer Anwendung Amazon Web Services (AWS) angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
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

