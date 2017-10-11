---
title: 'Lernprogramm: Azure Active Directory-Integration mit Cisco Spark | Microsoft Docs'
description: Informationen Sie zum Konfigurieren von einmaliges Anmelden zwischen Azure Active Directory und Cisco Spark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: a0a221622afe1c801a331e2319f3a7ace3111dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Lernprogramm: Azure Active Directory-Integration mit Cisco Spark

In diesem Lernprogramm erfahren Sie, wie Cisco Spark mit Azure Active Directory (Azure AD) integrieren.

Integrieren von Cisco Spark in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Cisco Spark hat
- Sie können Ihre Benutzer für die automatisch angemeldet, Cisco Spark (Single Sign-On) bei Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort - Azure-Portal verwalten.

Wenn Sie weitere Informationen zum Integrieren von SaaS-Apps in Azure AD wissen möchten, finden Sie unter [was Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory ist](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren von Azure AD-Integration mit Cisco Spark benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Ein Cisco Spark einmaliges Anmelden für Abonnement mit aktivierter

> [!NOTE]
> Um die Schritte in diesem Lernprogramm zu testen, führen Sie wird nicht empfohlen, mithilfe einer produktiven Umgebung.

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlungen beachten:

- Verwenden Sie nicht Ihre produktionsumgebung, wenn es erforderlich ist.
- Wenn Sie nicht über eine Testversion Azure Active Directory-Umgebung verfügen, erhalten Sie eine einmonatige Testversion [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm testen Sie Azure AD einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebene Szenario besteht aus zwei Hauptbausteinen:

1. Cisco Spark aus dem Katalog hinzufügen
2. Konfigurieren und Testen von Azure AD-einmaliges Anmelden

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark aus dem Katalog hinzufügen
Um die Integration von Cisco Spark in Azure AD zu konfigurieren, müssen Sie Cisco Spark der Liste der verwalteten SaaS-apps aus dem Katalog hinzufügen.

**Um Cisco Spark aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. In der  **[Azure-Portal](https://portal.azure.com)**, klicken Sie auf den linken Navigationsbereich auf **Azure Active Directory** Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **unternehmensanwendungen**. Klicken Sie dann auf **alle Anwendungen**.

    ![Applications][2]
    
3. Neue Anwendung hinzufügen möchten, klicken Sie auf **neue Anwendung** Schaltfläche oben Dialogfeld.

    ![Applications][3]

4. Geben Sie in das Suchfeld **Cisco Spark**.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. Wählen Sie im Ergebnisbereich, **Cisco Spark**, und klicken Sie dann auf **hinzufügen** Schaltfläche, um die Anwendung hinzuzufügen.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD-einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Sie Azure AD einmaliges Anmelden für mit Cisco Spark basierend auf einen Testbenutzer namens "Britta Simon."

Für einmaliges Anmelden funktioniert muss Azure AD zu wissen, was der Benutzer Gegenstück in Cisco Spark für einen Benutzer in Azure AD ist. Das heißt, muss eine linkbeziehung zwischen Azure AD-Benutzers und die entsprechenden Benutzer in Cisco Spark hergestellt werden.

In Cisco Spark, weisen Sie den Wert der die **Benutzername** in Azure AD als Wert des der **Benutzername** auf die linkbeziehung zu erstellen.

Zum Konfigurieren und Testen Sie Azure AD einmaliges Anmelden mit Cisco Spark, müssen Sie den folgenden Bausteinen ausführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Informationen zum Aktivieren Ihrer Benutzer dieses Feature verwenden.
2. **[Erstellen ein Azure AD-Testbenutzer](#creating-an-azure-ad-test-user)**  – Informationen zum Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eines Testbenutzers Cisco Spark](#creating-a-cisco-spark-test-user)**  – um eine Entsprechung Britta Simon in Cisco Spark verfügen, die mit der Azure AD-Darstellung eines Benutzers verknüpft ist.
4. **[Zuweisen von Azure AD-Testbenutzer](#assigning-the-azure-ad-test-user)**  – zum Aktivieren der Britta Simon einmaliges Anmelden für Azure AD verwenden.
5. **[Testen Sie einmaliges Anmelden für](#testing-single-sign-on)**  – zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt werden Azure AD einmaliges Anmelden im Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Ihrer Cisco Spark-Anwendung.

**Führen Sie zum Konfigurieren von Azure AD einmaliges Anmelden mit Cisco Spark die folgenden Schritte aus:**

1. Im Azure-Portal auf der **Cisco Spark** anwendungsintegrationsseite, klicken Sie auf **des einmaligen Anmeldens**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Auf der **des einmaligen Anmeldens** wählen Sie im Dialogfeld **Modus** als **SAML-basierten anmelden** einmaliges Anmelden aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. Auf der **Cisco Spark-Domänen und URLs** führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    a. In der **Anmelde-URL** Textfeld eine URL als:`https://web.ciscospark.com/#/signin`

    b. In der **Bezeichner** Textfeld eine URL mithilfe des folgenden Musters:`https://idbroker.webex.com/<companyname>`

    > [!NOTE] 
    > Dieser Wert ist keine echte. Aktualisieren Sie diesen Wert mit dem eigentlichen Bezeichner. Wenden Sie sich an [Cisco Spark Client Support-Team](https://support.ciscospark.com/) zum Abrufen dieses Werts. 
 
4. Auf der **SAML-Signaturzertifikat** auf **Metadata XML** und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. Cisco Spark-Anwendung erwartet die SAML-Assertionen, bestimmte Attribute enthalten. Konfigurieren Sie die folgenden Attribute für diese Anwendung. Sie können die Werte dieser Attribute aus verwalten die **Benutzerattribute** Abschnitt auf der Seite "Verzeichnisintegration" Anwendung. Der folgende Screenshot zeigt ein Beispiel dafür an.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. In der **Benutzerattribute** Abschnitt der **des einmaligen Anmeldens** Dialogfeld SAML-token-Attribut konfigurieren, wie in der Abbildung oben dargestellt, und führen die folgenden Schritte aus:
    
    | Attributname  | Attributwert |
    | --------------- | -------------------- |    
    |   UID    | User.userPrincipalName |   

    a. Klicken Sie auf **Add Attribut** So öffnen die **Attribut hinzufügen** Dialogfeld.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    b. In der **Namen** Textbox, geben Sie den für diese Zeile angezeigten Attributnamen.
    
    c. Aus der **Wert** aufzulisten, geben Sie für diese Zeile angezeigten Wert des Attributs.
    
    d. Klicken Sie auf **OK**.

7. Klicken Sie auf **speichern** Schaltfläche.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. Melden Sie sich beim [Cisco Zusammenarbeit Cloudverwaltung](https://admin.ciscospark.com/) mit Ihren Anmeldeinformationen Hauptadministrator.

9. Wählen Sie **Einstellungen** und wählen Sie unter der **Authentifizierung** auf **ändern**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. Wählen Sie **3rd-Identitätsanbieter zu integrieren. (Erweitert)**  und fahren Sie mit dem nächsten Bildschirm.

11. Auf der **Idp-Metadaten importieren** Seite entweder durch Ziehen und löschen Sie die Azure AD-Metadatendatei auf der Seite aus, oder verwenden Sie die Browser-Dateioption suchen und die Azure AD-Metadatendatei hochzuladen. Aktivieren Sie das Kontrollkästchen **ist ein Zertifikat von einer Zertifizierungsstelle in den Metadaten (sicherer) signiert erforderlich** , und klicken Sie auf **Weiter**. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. Wählen Sie **SSO Testverbindung**, und wenn eine neue Browserregisterkarte geöffnet wird, bei Azure AD authentifiziert, durch die Anmeldung bei.

13. Zurück zu den **Cisco Zusammenarbeit Cloudverwaltung** Browserregisterkarte. Wenn der Test erfolgreich war, wählen Sie **diesen Test erfolgreich war. Aktivieren Sie einmaliges Anmelden für Option** , und klicken Sie auf **Weiter**.

> [!TIP]
> Erfahren Sie jetzt eine kürzere Version dieser Anweisungen innerhalb der [Azure-Portal](https://portal.azure.com), während Sie die app einrichten.  Nach dem Hinzufügen der diese app aus der **Active Directory > Unternehmensanwendungen** einfach auf die **einmaliges Anmelden für** Registerkarte und Zugriff auf die embedded-Dokumentation über die **Konfiguration** Abschnitt unten. Weitere Informationen zu dieser Funktion embedded-Dokumentation: [Azure AD eingebettet Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen ein Azure AD-Testbenutzer
Das Ziel dieses Abschnitts ist zum Erstellen eines Testbenutzers im Azure-Portal Britta Simon aufgerufen.

![Erstellen von Azure AD-Benutzer][100]

**Führen Sie zum Erstellen eines Testbenutzers in Azure AD die folgenden Schritte aus:**

1. In der **Azure-Portal**, klicken Sie im linken Navigationsbereich auf **Azure Active Directory** Symbol.

    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wechseln Sie zu **Benutzer und Gruppen** , und klicken Sie auf **alle Benutzer**.
    
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. So öffnen die **Benutzer** Dialogfeld klicken Sie auf **hinzufügen** oben im Dialogfeld auf.
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Auf der **Benutzer** Dialogfeld Seite, die folgenden Schritte aus:
 
    ![Erstellen ein Azure AD-Testbenutzer](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    a. In der **Namen** Textfeld **BrittaSimon**.

    b. In der **Benutzername** Textfeld die **e-Mail-Adresse** des BrittaSimon.

    c. Wählen Sie **Kennwort anzeigen** und notieren Sie sich den Wert, der die **Kennwort**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-cisco-spark-test-user"></a>Erstellen eines Testbenutzers Cisco Spark

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Cisco Spark. In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Cisco Spark.

1. Wechseln Sie zu der [Cisco Zusammenarbeit Cloudverwaltung](https://admin.ciscospark.com/) mit Ihren Anmeldeinformationen Hauptadministrator.

2. Klicken Sie auf **Benutzer** und dann **Verwalten von Benutzern**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. In der **Benutzer verwalten** wählen **manuell hinzufügen oder Ändern von Benutzern** , und klicken Sie auf **Weiter**.

4. Wählen Sie **Namen und e-Mail-Adresse**. Füllen Sie dann das Textfeld wie folgt aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    a. In der **Vorname** Textfeld **Britta**. 
    
    b. In der **Nachname** Textfeld **Simon**.
    
    c. In der **e-Mail-Adresse** Textfeld  **britta.simon@contoso.com** .

5. Klicken Sie auf das Pluszeichen, um Britta Simon hinzufügen. Klicken Sie anschließend auf **Weiter**.

6. In der **Hinzufügen von Diensten für Benutzer** Fenster, klicken Sie auf **speichern** und dann **Fertig stellen**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon durch Gewähren des Zugriffs auf Cisco Spark Azure einmaliges Anmelden zu verwenden.

![Weisen Sie Benutzer][200] 

**Cisco Spark Britta Simon zuweisen möchten, führen Sie die folgenden Schritte aus:**

1. Im Azure-Portal, öffnen Sie die Anwendungen anzuzeigen, und navigieren Sie zu dem Verzeichnisansicht, und wechseln Sie zu **unternehmensanwendungen** klicken Sie dann auf **alle Anwendungen**.

    ![Weisen Sie Benutzer][201] 

2. Wählen Sie in der Liste der Anwendungen **Cisco Spark**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Weisen Sie Benutzer][202] 

4. Klicken Sie auf **hinzufügen** Schaltfläche. Wählen Sie dann **Benutzer und Gruppen** auf **Zuweisung hinzufügen** Dialogfeld.

    ![Weisen Sie Benutzer][203]

5. Auf **Benutzer und Gruppen** wählen Sie im Dialogfeld **Britta Simon** in der Benutzerliste.

6. Klicken Sie auf **wählen** Schaltfläche **Benutzer und Gruppen** Dialogfeld.

7. Klicken Sie auf **zuweisen** Schaltfläche **Zuweisung hinzufügen** Dialogfeld.
    
### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Dieser Abschnitt soll gezeigt werden Ihre Azure AD-SSO-Konfiguration über den Zugriffsbereich testen.

Wenn Sie auf den Zugriffsbereich die Cisco Spark-Kachel klicken, Sie sollten automatisch für Ihre Anwendung Cisco Spark angemeldete abrufen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zur Integration von SaaS-Apps mit Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was ist Anwendungszugriff und einmaliges Anmelden bei Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

