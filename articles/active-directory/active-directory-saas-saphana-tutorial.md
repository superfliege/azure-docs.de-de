---
title: 'Tutorial: Azure Active Directory-Integration mit SAP HANA | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP HANA konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 02c751b5d9e6f13c81b9c957cd6689c1fc744a07
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Tutorial: Azure Active Directory-Integration mit SAP HANA

In diesem Tutorial erfahren Sie, wie Sie SAP HANA in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP HANA in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SAP HANA hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP HANA anzumelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in SAP HANA konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SAP HANA-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist
- Eine in einer öffentlichen IaaS-Umgebung, lokal, auf virtuellen Azure-Computern oder in großen SAP-Instanzen in Azure ausgeführte HANA-Instanz
- Die XSA-Webschnittstelle für die Verwaltung sowie HANA Studio, installiert in der HANA-Instanz

> [!NOTE]
> Es empfiehlt sich nicht, die Schritte in diesem Tutorial in einer SAP HANA-Produktionsumgebung zu testen. Testen Sie die Integration zuerst in der Entwicklungs- oder Stagingumgebung der Anwendung, und verwenden Sie erst danach die Produktionsumgebung.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Registrieren Sie sich für eine [einmonatige kostenlose Testversion von Azure AD](https://azure.microsoft.com/pricing/free-trial/), sofern Sie noch keine Azure AD-Testumgebung besitzen.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP HANA aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-sap-hana-from-the-gallery"></a>Hinzufügen von SAP HANA aus dem Katalog
Zum Konfigurieren der Integration von SAP HANA in Azure AD müssen Sie SAP HANA aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um SAP HANA aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen ausführen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um die neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Suchbegriff **SAP HANA** ein. Wählen Sie im Ergebnisbereich **SAP HANA** aus. Klicken Sie abschließend auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen. 

    ![Die neue Anwendung](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP HANA mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP HANA als Pendant eines Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP HANA muss eine Verknüpfung eingerichtet werden.

Verwenden Sie in SAP HANA für **Benutzername** den gleichen Wert wie für **Benutzername** in Azure AD. Durch diesen Schritt wird die Verknüpfung zwischen den zwei Benutzern hergestellt.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei SAP HANA müssen Sie die folgenden Aufgaben ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on), um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. [Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen eines SAP HANA-Testbenutzers](#creating-a-sap-hana-test-user), um ein Pendant von Britta Simon in SAP HANA zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
4. [Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen der einmaligen Anmeldung](#testing-single-sign-on), um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SAP HANA-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei SAP HANA die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP HANA** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Klicken Sie im Dialogfeld **Einmaliges Anmelden** unter **SAML-basierte Anmeldung** auf **Modus**.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für SAP HANA** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. Geben Sie im Feld **Bezeichner** Folgendes ein: `HA100` 

    b. Geben Sie im Feld **Antwort-URL** eine URL im folgenden Format ein: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam für den SAP HANA-Client](https://cloudplatform.sap.com/contact.html), um diese Werte zu erhalten. 

4. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Metadaten-XML**. Speichern Sie dann die Metadatendatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Wenn das Zertifikat nicht aktiv ist, aktivieren Sie es, indem Sie in Azure AD das Kontrollkästchen **Neues Zertifikat aktivieren** auswählen. 

5. Die SAP HANA-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Der folgende Screenshot zeigt ein Beispiel für dieses Format. 

    Hier wurde die **Benutzer-ID** mit der **ExtractMailPrefix()**-Funktion von **user.mail** zugeordnet. Das Ergebnis ist der Präfixwert der E-Mail des Benutzers und damit die eindeutige Benutzer-ID. Die Benutzer-ID wird in jeder erfolgreichen Antwort an die SAP HANA-Anwendung gesendet.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. Führen Sie im Abschnitt **Benutzerattribute** des Dialogfelds **Einmaliges Anmelden** die folgenden Schritte aus:

    a. Wählen Sie in der Dropdownliste **Benutzer-ID** den Eintrag **ExtractMailPrefix** aus.
    
    b. Wählen Sie in der Dropdownliste **E-Mail** den Eintrag **user.mail** aus.

7. Wählen Sie die Schaltfläche **Speichern** aus.

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Zum Konfigurieren des einmaligen Anmeldens auf der Seite von SAP HANA melden Sie sich bei Ihrer **HANA XSA-Webkonsole** an, indem Sie zum entsprechenden HTTPS-Endpunkt navigieren.

    > [!NOTE]
    > In der Standardkonfiguration leitet die URL die Anforderung an einen Anmeldebildschirm weiter. Dafür sind die Anmeldeinformationen eines authentifizierten SAP HANA-Datenbankbenutzers erforderlich. Der Benutzer, der sich anmeldet, benötigt Berechtigungen zum Ausführen von SAML-Verwaltungsaufgaben.

9. Navigieren Sie in der XSA-Webschnittstelle zu **SAML-Identitätsanbieter**. Klicken Sie dort am unteren Rand des Bildschirms auf die Schaltfläche **+**, um den Bereich **Add Identity Provider Info** (Identitätsanbieterinformationen hinzufügen) anzuzeigen. Führen Sie dann die folgenden Schritte aus:

    ![Identitätsanbieter hinzufügen](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. Fügen Sie im Fenster **Add Identity Provider Info** (Identitätsanbieterinformationen hinzufügen) den Inhalt der aus dem Azure-Portal heruntergeladenen XML-Metadatendatei in das Feld **Metadata** (Metadaten) ein.

    ![Einstellungen für Identitätsanbieter hinzufügen](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Wenn der Inhalt des XML-Dokuments gültig ist, extrahiert der Analyseprozess die Informationen, die für die Felder **Subject, Entity ID, and Issuer** (Antragsteller, Entitäts-ID, Zertifikataussteller) im Bildschirmbereich **General data** (Allgemeine Daten) erforderlich sind. Darüber hinaus werden die Informationen für die URL-Felder im Bildschirmbereich **Ziel** extrahiert, etwa **Basis-URL und SingleSignOn URL (*)** (SingleSignOn-URL).

    ![Einstellungen für Identitätsanbieter hinzufügen](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. Geben Sie im Bildschirmbereich **General Data** (Allgemeine Daten) im Feld **Name** einen Namen für den neuen SAML-SSO-Identitätsanbieter ein.

    > [!NOTE]
    > Der Name des SAML-IDP ist zwingend erforderlich und muss eindeutig sein. Er wird in der Liste der verfügbaren SAML-IDPs aufgeführt, die angezeigt wird, wenn Sie SAML als Authentifizierungsmethode für SAP HANA-XS-Anwendungen auswählen. Dieser Schritt kann beispielsweise im Bildschirmbereich **Authentication** (Authentifizierung) des XS-Tools für die Artefaktverwaltung ausgeführt werden.

10. Wählen Sie **Save** (Speichern), um die Details des SAML-Identitätsanbieters zu speichern und den neuen SAML-IDP zur Liste der bekannten SAML-Identitätsanbieter hinzuzufügen.

    ![Schaltfläche „Speichern“](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. Filtern Sie in HANA Studio in den Systemeigenschaften der Registerkarte **Konfiguration** die Einstellungen nach **saml**. Ändern Sie dann den Wert für **assertion_timeout** von **10** Sekunden in **120** Sekunden.

    ![assertion_timeout-Einstellung](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Erstellen eines Azure AD-Benutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des **Azure-Portals** auf das Symbol **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen**. Wählen Sie dann **Alle Benutzer** aus.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich das Kennwort.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-sap-hana-test-user"></a>Erstellen eines SAP HANA-Testbenutzers

Um für Azure AD-Benutzer das Anmelden bei SAP HANA zu aktivieren, müssen Sie sie in SAP HANA bereitstellen.
SAP HANA unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Wenn Sie manuell einen Benutzer erstellen müssen, führen Sie die folgenden Schritte aus:

>[!NOTE]
>Sie können die vom Benutzer verwendete externe Authentifizierung ändern. Die Benutzer können sich mit einem externen System wie Kerberos authentifizieren. Um detaillierte Informationen zu externen Identitäten zu erhalten, wenden Sie sich an Ihren [Domänenadministrator](https://cloudplatform.sap.com/contact.html).

1. Öffnen Sie [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) als Administrator, und aktivieren Sie den Datenbankbenutzer für SAML-SSO.

    ![Benutzer erstellen](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Aktivieren Sie das unsichtbare Kontrollkästchen links neben **SAML**, und klicken Sie dann auf den Link **Configure** (Konfigurieren).

3. Klicken Sie auf **Add** (Hinzufügen), um den SAML-IDP hinzuzufügen.  Wählen Sie den entsprechenden SAML-IDP aus, und klicken Sie dann auf **OK**.

4. Fügen Sie den Wert für **External Identity** (Externe ID) (in diesem Fall BrittaSimon) hinzu, oder wählen Sie **Any** (Beliebig). Wählen Sie dann **OK**aus.

    >[!Note]
    >Wenn das Kontrollkästchen **Any** (Beliebig) nicht aktiviert ist, muss der Benutzername in HANA exakt dem Namen des Benutzers im Benutzerprinzipalnamen vor dem Domänensuffix entsprechen. (Beispiel: BrittaSimon@contoso.com wird in HANA zu „BrittaSimon“.)

5. Weisen Sie dem Benutzer zu Testzwecken alle **XS**-Rollen zu.

    ![Zuweisen von Rollen](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Sie sollten nur die Berechtigungen zuweisen, die für Ihre Anwendungsfälle erforderlich sind.

6. Speichern Sie den Benutzer.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf SAP HANA gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu SAP HANA zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Wechseln Sie dann zur Verzeichnisansicht und zu **Unternehmensanwendungen**. Wählen Sie **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **SAP HANA** aus.

    ![Benutzer zuweisen](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202] 

4. Wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** unter **Benutzer** die Option **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SAP HANA“ klicken, sollten Sie automatisch bei Ihrer SAP HANA-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

