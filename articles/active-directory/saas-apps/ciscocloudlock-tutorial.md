---
title: 'Tutorial: Azure Active Directory-Integration in The Cloud Security Fabric | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und The Cloud Security Fabric konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 549e8810-1b3b-4351-bf4b-f07de98980d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 56f88af5d1ec8adb44d5f5c7f539925e26459e19
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047942"
---
# <a name="tutorial-azure-active-directory-integration-with-the-cloud-security-fabric"></a>Tutorial: Azure Active Directory-Integration in The Cloud Security Fabric

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD) in The Cloud Security Fabric integrieren.

Die Integration von The Cloud Security Fabric in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf The Cloud Security Fabric Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei The Cloud Security Fabric anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in The Cloud Security Fabric konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein The Cloud Security Fabric-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von The Cloud Security Fabric aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-the-cloud-security-fabric-from-the-gallery"></a>Hinzufügen von The Cloud Security Fabric aus dem Katalog
Zum Konfigurieren der Integration von The Cloud Security Fabric in Azure AD müssen Sie The Cloud Security Fabric aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um The Cloud Security Fabric aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **The Cloud Security Fabric**  ein, wählen Sie im Ergebnisbereich **The Cloud Security Fabric** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![The Cloud Security Fabric in der Ergebnisliste](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit The Cloud Security Fabric.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in The Cloud Security Fabric als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in The Cloud Security Fabric muss eine Verknüpfungsbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei The Cloud Security Fabric müssen die folgenden Bausteine ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines The Cloud Security Fabric-Testbenutzers](#create-a-the-cloud-security-fabric-test-user)**, um ein Pendant von Britta Simon in The Cloud Security Fabric zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer The Cloud Security Fabric-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in The Cloud Security Fabric die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **The Cloud Security Fabric** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_samlbase.png)

3. Führen Sie die folgenden Schritte im Abschnitt **Domäne und URLs für The Cloud Security Fabric** aus:

    ![Informationen zum einmaligen Anmelden für The Cloud Security Fabric-Domäne und -URLs](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL wie die Folgende ein:
    | |
    |--|
    | `https://platform.cloudlock.com` |
    | `https://app.cloudlock.com` |

    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein:
    | |
    |--|
    | `https://platform.cloudlock.com/gate/saml/sso/<subdomain>` |
    | `https://app.cloudlock.com/gate/saml/sso/<subdomain>` |

    > [!NOTE]
    > Der ID-Wert ist nicht der tatsächliche Wert. Aktualisieren Sie den Wert mit dem tatsächlichen Bezeichner. Wenden Sie sich an das [The Cloud Security Fabric-Kundensupportteam](mailto:support@cloudlock.com), um den Wert in Erfahrung zu bringen. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_certificate.png)

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/ciscocloudlock-tutorial/tutorial_general_400.png)

6. Zum Konfigurieren des einmaligen Anmeldens bei **The Cloud Security Fabric** müssen Sie die heruntergeladene **Metadaten-XML**-Datei an das [The Cloud Security Fabric-Supportteam](mailto:support@cloudlock.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/ciscocloudlock-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/ciscocloudlock-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/ciscocloudlock-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/ciscocloudlock-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="create-a-the-cloud-security-fabric-test-user"></a>Erstellen eines The Cloud Security Fabric-Testbenutzers

In diesem Abschnitt erstellen Sie in The Cloud Security Fabric einen Benutzer namens Britta Simon. Das [The Cloud Security Fabric-Supportteam](mailto:support@cloudlock.com) kann Sie beim Hinzufügen von Benutzern auf der The Cloud Security Fabric-Plattform unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf The Cloud Security Fabric gewähren.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon The Cloud Security Fabric zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **The Cloud Security Fabric** aus.

    ![Der Link „The Cloud Security Fabric“ in der Anwendungsliste](./media/ciscocloudlock-tutorial/tutorial_ciscocloudlock_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „The Cloud Security Fabric“ klicken, sollten Sie automatisch bei Ihrer The Cloud Security Fabric-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ciscocloudlock-tutorial/tutorial_general_01.png
[2]: ./media/ciscocloudlock-tutorial/tutorial_general_02.png
[3]: ./media/ciscocloudlock-tutorial/tutorial_general_03.png
[4]: ./media/ciscocloudlock-tutorial/tutorial_general_04.png

[100]: ./media/ciscocloudlock-tutorial/tutorial_general_100.png

[200]: ./media/ciscocloudlock-tutorial/tutorial_general_200.png
[201]: ./media/ciscocloudlock-tutorial/tutorial_general_201.png
[202]: ./media/ciscocloudlock-tutorial/tutorial_general_202.png
[203]: ./media/ciscocloudlock-tutorial/tutorial_general_203.png
