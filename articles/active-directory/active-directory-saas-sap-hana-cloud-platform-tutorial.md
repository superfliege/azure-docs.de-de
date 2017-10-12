---
title: 'Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP Cloud Platform konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 6b4b0e3ab126e70e4b27b971b15127aae0486bed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Tutorial: Azure Active Directory-Integration mit SAP Cloud Platform

In diesem Tutorial erfahren Sie, wie Sie SAP Cloud Platform in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP Cloud Platform in Azure AD hat folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SAP Cloud Platform haben soll.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei SAP Cloud Platform anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit SAP Cloud Platform benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges SAP Cloud Platform-Abonnement

Nach Abschluss dieses Tutorials können sich die SAP Cloud Platform zugewiesenen Azure AD-Benutzer wie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) beschrieben mittels einmaliger Anmeldung bei der Anwendung anmelden.

>[!IMPORTANT]
>Sie müssen eine eigene Anwendung bereitstellen oder eine Anwendung unter Ihrem SAP Cloud Platform-Konto abonnieren, um das einmalige Anmelden testen zu können. In diesem Tutorial wird eine Anwendung auf dem Konto bereitgestellt.
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP Cloud Platform über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>Hinzufügen von SAP Cloud Platform über den Katalog
Zum Konfigurieren der Integration von SAP Cloud Platform in Azure AD müssen Sie SAP Cloud Platform aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um SAP Cloud Platform über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **SAP Cloud Platform** ein, wählen Sie im Ergebnisbereich **SAP Cloud Platform** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![SAP Cloud Platform in der Ergebnisliste](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP Cloud Platform mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP Cloud Platform als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP Cloud Platform muss eine Linkbeziehung eingerichtet werden.

Weisen Sie den Wert für **Benutzername** aus Azure AD in SAP Cloud Platform als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei SAP Cloud Platform müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SAP Cloud Platform-Testbenutzers](#create-a-sap-cloud-platform-test-user)**, um ein Pendant von Britta Simon in SAP Cloud Platform zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer SAP Cloud Platform-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei SAP Cloud Platform die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Cloud Platform** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für SAP Cloud Platform** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für SAP Cloud Platform](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von den Benutzern zur Anmeldung bei der **SAP Cloud Platform**-Anwendung verwendet wird. Dies ist die kontospezifische URL einer geschützten Ressource in der SAP Cloud Platform-Anwendung. Die URL basiert auf dem folgenden Muster: `https://<applicationName><accountName>.<landscape host>.ondemand.com/<path_to_protected_resource>`
      
     >[!NOTE]
     >Dies ist die URL in Ihrer SAP Cloud Platform-Anwendung, die die Authentifizierung des Benutzers erfordert.
     > 

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |

    b. Geben Sie im Textfeld **Bezeichner** die URL von SAP Cloud Platform in einem der folgenden Formate an: 

    | |
    |--|
    | `https://hanatrial.ondemand.com/<instancename>` |
    | `https://hana.ondemand.com/<instancename>` |
    | `https://us1.hana.ondemand.com/<instancename>` |
    | `https://ap1.hana.ondemand.com/<instancename>` |

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.hana.ondemand.com/<instancename>` |

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL. Anmelde-URL und Bezeichner erhalten Sie vom [Supportteam für den SAP Cloud Plattform-Client](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/5dd739823b824b539eee47b7860a00be.html). Die Antwort-URL finden Sie im Abschnitt für die Verwaltung der Vertrauensstellung. Dies wird weiter unten im Tutorial erläutert.
    > 
     
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster unter `https://account.<landscape host>.ondemand.com/cockpit` (Beispiel: https://account.hanatrial.ondemand.com/cockpit) beim SAP Cloud Platform-Cockpit an.

7. Klicken Sie auf die Registerkarte **Vertrauen** .
   
    ![Vertrauen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790800.png "Vertrauen")

8. Führen Sie im Abschnitt für die Verwaltung der Vertrauensstellung unter **Local Service Provider** (Lokaler Dienstanbieter) die folgenden Schritte aus:

    ![Verwalten von Vertrauensstellungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793931.png "Verwalten von Vertrauensstellungen")
   
    a. Klicken Sie auf **Bearbeiten**.

    b. Wählen Sie als **Konfigurationstyp** die Option **Benutzerdefiniert**.

    c. Belassen Sie als **Name des lokalen Dienstanbieters**den Standardwert. Kopieren Sie diesen Wert, und fügen Sie ihn in der Azure AD-Konfiguration für SAP Cloud Plattform in das Feld **Bezeichner** ein.

    d. Klicken Sie zum Generieren eines **Signaturschlüssels** und eines **Signaturzertifikat**-Schlüsselpaars auf **Schlüsselpaar generieren**.

    e. Wählen Sie als **Prinzipalweitergabe** die Option **Deaktiviert**.

    f. Wählen Sie unter **Zwangsauthentifizierung** die Option **Deaktiviert**.

    g. Klicken Sie auf **Speichern**.

9. Führen Sie nach dem Speichern der Einstellungen für **Local Service Provider** (Lokaler Dienstanbieter) die folgenden Schritte aus, um die Antwort-URL zu erhalten:
   
    ![Abrufen von Metadaten](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793930.png "Abrufen von Metadaten")

    a. Klicken Sie zum Herunterladen der SAP Cloud Platform-Metadatendatei auf **Metadaten abrufen**.

    b. Öffnen Sie die heruntergeladene SAP Cloud Platform-Metadatendatei (XML-Datei), und navigieren Sie zum Tag **Ns3:AssertionConsumerService**.
 
    c. Kopieren Sie den Wert des Attributs **Speicherort**, und fügen Sie ihn in der Azure AD-Konfiguration für SAP Cloud Plattform in das Textfeld **Antwort-URL** ein.

10. Klicken Sie auf die Registerkarte **Vertrauenswürdiger Identitätsanbieter**, und klicken Sie dann auf **Vertrauenswürdigen Identitätsanbieter hinzufügen**.
   
    ![Verwalten von Vertrauensstellungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790802.png "Verwalten von Vertrauensstellungen")
   
    >[!NOTE]
    >Um die Liste der vertrauenswürdigen Identitätsanbieter verwalten zu können, müssen Sie den Konfigurationstyp „Benutzerdefinierte“ im Abschnitt „Lokale Dienstanbieter“ ausgewählt haben. Beim Standard-Konfigurationstyp haben Sie ein nicht-bearbeitbares und implizites Vertrauen gegenüber dem SAP ID-Dienst. Bei der Option „Keine“ haben Sie keine Vertrauenseinstellungen.
    > 
    > 

11. Klicken Sie auf die Registerkarte **Allgemein** und dann auf **Durchsuchen**, um die heruntergeladene Metadatendatei hochzuladen.
    
    ![Verwalten von Vertrauensstellungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic793932.png "Verwalten von Vertrauensstellungen")
    
    >[!NOTE]
    >Nach dem Hochladen der Metadatendatei werden die Werte für **URL für einmaliges Anmelden**, **URL für einmaliges Abmelden** und **Signaturzertifikat** automatisch ausgefüllt.
    > 
     
12. Klicken Sie auf die Registerkarte **Attribute** .

13. Führen Sie auf der Registerkarte **Attribute** die folgenden Schritte aus:
    
    ![Attribute](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790804.png "Attribute") 

    a. Klicken Sie auf **Assertion-Attribut hinzufügen**, und für Sie dann die folgenden Assertion-basierten Attribute hinzu:
       
    | Assertion-Attribut | Prinzipal-Attribut |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |firstname |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |lastname |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |E-Mail |
   
     >[!NOTE]
     >Die Konfiguration der Attribute hängt davon ab, wie die Anwendung(en) für SPC entwickelt wurden – also welche Attribute sie in der SAML-Antwort erwarten und unter welchem Namen (Prinzipal-Attribut) sie auf dieses Attribut im Code zugreifen.
     > 
    
    b. Das **Standard-Attribut** im Screenshot dient nur der Veranschaulichung. Es ist nicht erforderlich, damit das Szenario funktioniert.  
 
    c. Die Namen und Werte für das im Screenshot gezeigte **Prinzipal Attribut** ist abhängig davon, wie die Anwendung entwickelt wird. Es ist möglich, dass die Anwendung andere Zuordnungen erfordert.

###<a name="assertion-based-groups"></a>Assertion-Gruppen

Als optionalen Schritt können Sie Assertion-Gruppen für Ihren Azure Active Directory-Identitätsanbieter konfigurieren.

Mithilfe von Gruppen für SAP Cloud Platform können Sie einzelne oder mehrere Benutzer dynamisch einzelnen oder mehreren Rollen in Ihren SAP Cloud Platform-Anwendungen zuweisen – definiert durch Werte von Attributen in der SAML 2.0-Assertion. 

Beispiel: Wenn die Assertion das Attribut „*contract=temporary*“ enthält, kann es sein, dass Sie alle betroffenen Benutzer der Gruppe „*TEMPORÄR*“ hinzufügen möchten. Die Gruppe „*TEMPORÄR*“ enthält unter Umständen Rollen aus mindestens einer Anwendung, die unter Ihrem SAP Cloud Platform-Konto bereitgestellt wurde.
 
Verwenden Sie Assertion-basierte Gruppen, wenn Sie Anwendungsrollen in Ihrem SAP Cloud Platform-Konto gleichzeitig viele Benutzer zuweisen möchten. Wenn Sie nur einen einzelnen Benutzer oder eine geringe Anzahl von Benutzern bestimmten Rollen zuweisen möchten, empfiehlt es sich, diese direkt im SAP Cloud Platform-Cockpit auf der Registerkarte **Autorisierungen** zuzuweisen.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-sap-cloud-platform-test-user"></a>Erstellen eines SAP Cloud Plattform-Testbenutzers

Um Azure AD-Benutzern die Anmeldung bei SAP Cloud Platform zu ermöglichen, müssen Sie ihnen Rollen in SAP Cloud Platform zuweisen.

**Führen Sie die folgenden Schritte aus, um einem Benutzer eine Rolle zuzuweisen:**

1. Melden Sie sich bei Ihrem **SAP Cloud Platform**-Cockpit an.

2. Führen Sie Folgendes aus:
   
    ![Autorisierungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/ic790805.png "Autorisierungen")
   
    a. Klicken Sie auf **Autorisierung**.

    b. Klicken Sie auf die Registerkarte **Benutzer** .

    c. Geben Sie im Textfeld **Benutzer** die E-Mail-Adresse des Benutzers ein.

    d. Klicken Sie auf **Zuweisen** , um den Benutzer einer Rolle zuzuweisen.

    e. Klicken Sie auf **Speichern**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf SAP Cloud Platform gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu SAP Cloud Platform durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **SAP Cloud Platform** aus.

    ![Der Link „SAP Cloud Platform“ in der Anwendungsliste](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SAP Cloud Platform“ klicken, sollten Sie automatisch bei Ihrer SAP Cloud Platform-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-tutorial/tutorial_general_203.png

