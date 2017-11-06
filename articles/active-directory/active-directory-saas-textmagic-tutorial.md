---
title: 'Tutorial: Azure Active Directory-Integration mit TextMagic | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und TextMagic konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3e5b49d2-7096-46bc-a9ce-90e09177ba28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: jeedes
ms.openlocfilehash: 701a97aa2001256354fe487c9499ff3cbb8b4540
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-textmagic"></a>Tutorial: Azure Active Directory-Integration mit TextMagic

In diesem Tutorial erfahren Sie, wie Sie TextMagic in Azure Active Directory (Azure AD) integrieren.

Die Integration von TextMagic in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf TextMagic hat.
- Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei TextMagic anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit TextMagic konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein TextMagic-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von TextMagic aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-textmagic-from-the-gallery"></a>Hinzufügen von TextMagic aus dem Katalog
Zum Konfigurieren der Integration von TextMagic in Azure AD müssen Sie TextMagic aus dem Katalog zu der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um TextMagic aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **TextMagic** ein, wählen Sie im Ergebnisbereich **TextMagic** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![TextMagic in der Ergebnisliste](./media/active-directory-saas-textmagic-tutorial/tutorial_textmagic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei TextMagic mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in TextMagic als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in TextMagic muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in TextMagic den Wert des **Benutzernamens** in Azure AD als Wert für den **Benutzernamen** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei TextMagic müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines TextMagic-Testbenutzers](#create-a-textmagic-test-user)**, um eine Entsprechung von Britta Simon in TextMagic zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden in Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer TextMagic-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens in Azure AD mit TextMagic die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **TextMagic** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-textmagic-tutorial/tutorial_textmagic_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für TextMagic** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für TextMagic](./media/active-directory-saas-textmagic-tutorial/tutorial_textmagic_url.png)

    Geben Sie im Textfeld **Bezeichner** eine URL ein: `https://my.textmagic.com/saml/metadata`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für TextMagic](./media/active-directory-saas-textmagic-tutorial/url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL wie die Folgende ein: `https://my.textmagic.com/login/sso`.


5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-textmagic-tutorial/tutorial_textmagic_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-textmagic-tutorial/tutorial_general_400.png)
    
7. Klicken Sie im Abschnitt **TextMagic-Konfiguration** auf **TextMagic konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![TextMagic-Konfiguration](./media/active-directory-saas-textmagic-tutorial/tutorial_textmagic_configure.png) 

8. Melden Sie sich in einem anderen Webbrowserfenster auf der TextMagic-Unternehmenswebsite als Administrator an.

9. Wählen Sie unter dem Benutzernamen **Kontoeinstellungen** aus.

    ![TextMagic-Konfiguration](./media/active-directory-saas-textmagic-tutorial/config1.png) 
10. Klicken Sie auf die Registerkarte **Einmaliges Anmelden (SSO)**, und füllen Sie folgende Felder aus:  
    
    ![TextMagic-Konfiguration](./media/active-directory-saas-textmagic-tutorial/config2.png)

    a. Fügen Sie in das Textfeld **dentity provider Entity ID:** (Entitäts-ID des Identitätsanbieters) den Wert der **SAML Entity ID** (SAML-Entitäts-ID) ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Textfeld **Identity provider SSO URL:** (SSO-URL des Identitätsanbieters) den Wert der **Single Sign-On Service URL** (Dienst-URL für SSO) ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld für die **Identity provider SLO URL:** (SSO-URL des Identitätsanbieters) den Wert der **Sign-Out URL** (Abmelde-URL) ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Öffnen Sie das **Base64-codierte Zertifikat** im Editor, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Public x509 certificate:** (Öffentliches x509-Zertifikat) ein.

    e. Klicken Sie auf **Speichern**.


> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-textmagic-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-textmagic-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-textmagic-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-textmagic-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-textmagic-test-user"></a>Erstellen eines TextMagic-Testbenutzers

Die Anwendung unterstützt die Just-in-Time-Benutzerbereitstellung. Nach der Authentifizierung werden in der Anwendung automatisch Benutzer erstellt. Sie müssen die Informationen nur einmal beim ersten Anmelden eingeben, um ein Unterkonto in Ihrem System zu aktivieren.
Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf TextMagic gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon TextMagic zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **TextMagic** aus.

    ![TextMagic-Link in der Anwendungsliste](./media/active-directory-saas-textmagic-tutorial/tutorial_textmagic_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „TextMagic“ klicken, sollten Sie automatisch bei Ihrer TextMagic-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-textmagic-tutorial/tutorial_general_203.png

