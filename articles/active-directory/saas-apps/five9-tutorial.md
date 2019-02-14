---
title: 'Tutorial: Azure Active Directory-Integration mit Five9 Plus Adapter (CTI, Contact Center-Agents) | Microsoft-Dokumentation'
description: Informationen zum Konfigurieren des einmaligen Anmeldens zwischen Azure Active Directory und Five9 Plus Adapter (CTI, Contact Center-Agents)
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 124bc858e9950962d3ae0f69db8fb962a3725f87
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56206513"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Tutorial: Azure Active Directory-Integration mit Five9 Plus Adapter (CTI, Contact Center-Agents)

In diesem Tutorial erfahren Sie, wie Sie Five9 Plus Adapter (CTI, Contact Center-Agents) in Azure Active Directory (Azure AD) integrieren.

Durch das Integrieren von Five9 Plus Adapter (CTI, Contact Center-Agents) in Azure Active Directory erhalten Sie die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Five9 Plus Adapter (CTI, Contact Center-Agents) hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Five9 Plus Adapter (CTI, Contact Center-Agents) anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Konfiguration der Azure AD-Konfiguration mit Five9 Plus Adapter (CTI, Contact Center-Agents) benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Abonnement von Five9 Plus-Adapter (CTI, Contact Center-Agents)

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie über keine Azure AD-Testumgebung verfügen, können Sie hier eine einmonatige Testversion anfordern: [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Five9 Plus Adapter (CTI, Contact Center-Agents) aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Hinzufügen von Five9 Plus Adapter (CTI, Contact Center-Agents) aus dem Katalog
Um die Integration von Five9 Plus Adapter (CTI, Contact Center-Agents) in Azure Active Directory zu konfigurieren, müssen Sie Five9 Plus Adapter (CTI, Contact Center-Agents) aus dem Katalog Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Um Five9 Plus Adapter (CTI, Contact Center-Agents) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![ANWENDUNGEN][3]

1. Geben Sie in das Suchfeld **Five9 Plus Adapter (CTI, Contact Center-Agents)** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/five9-tutorial/tutorial_five9_search.png)

1. Wählen Sie im Ergebnisbereich **Five9 Plus Adapter (CTI, Contact Center-Agents)** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Five9 Plus Adapter (CTI, Contact Center-Agents) mithilfe einer Testbenutzerin namens „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Five9 Plus Adapter (CTI, Contact Center-Agents) als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Five9 Plus Adapter (CTI, Contact Center-Agents) muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Five9 Plus Adapter (CTI, Contact Center-Agents) den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Five9 Plus Adapter (CTI, Contact Center-Agents) müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines Five9 Plus Adapter-Testbenutzers (CTI, Contact Center-Agents)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** – um eine Entsprechung von Britta Simon in Five9 Plus Adapter (CTI, Contact Center-Agents) zu erhalten, die mit der Benutzerdarstellung von Azure AD übereinstimmt.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Five9 Plus Adapter-Anwendung (CTI, Contact Center-Agents).

**Führen Sie für die Konfiguration des einmaligen Anmeldens von Azure AD mit Five9 Plus Adapter (CTI, Contact Center-Agents) die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Seite für die Anwendungsintegration, **Five9 Plus Adapter (CTI, Contact Center-Agents)**, auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. Führen Sie im Abschnitt **Five9 Plus Adapter (CTI, Contact Center Agents) Domain and URLs** (Domäne und URLs für Five9 Plus Adapter (CTI, Contact Center-Agents)) die folgenden Schritte durch:

    ![Configure single sign-on](./media/five9-tutorial/tutorial_five9_url.png)
    
    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein:

    |    Environment      |       URL      |
    | :-- | :-- |
    | Für „Five9 Plus Adapter für Microsoft Dynamics CRM“ | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Für „Five9 Plus Adapter für Zendesk“ | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Für „Five9 Plus Adapter für Agent Desktop Toolkit“ | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:

    |      Environment     |      URL      |
    | :--                  | :--           |
    | Für „Five9 Plus Adapter für Microsoft Dynamics CRM“ | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Für „Five9 Plus Adapter für Zendesk“ | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Für „Five9 Plus Adapter für Agent Desktop Toolkit“ | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Configure single sign-on](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/five9-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **Five9 Plus Adapter (CTI, Contact Center Agents) Configuration** (Konfiguration von Five9 Plus Adapter (CTI, Contact Center-Agents)) auf **Configure Five9 Plus Adapter (CTI, Contact Center Agents)** (Five9 Plus Adapter (CTI, Contact Center-Agents) konfigurieren), um das Fenster **Einmaliges Anmelden** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Configure single sign-on](./media/five9-tutorial/tutorial_five9_configure.png) 

1. Zum Konfigurieren des einmaligen Anmeldens bei **Five9 Plus Adapter (CTI, Contact Center-Agents)** müssen Sie das heruntergeladene **Zertifikat (Base64)** sowie Abmelde-URL, SAML-Entitäts-ID und SAML-Dienst-URL für das einmalige Anmelden an das [Supportteam von Five9 Plus Adapter (CTI, Contact Center-Agents)](https://www.five9.com/about/contact) senden. Um zudem SSO weiter zu konfigurieren, befolgen Sie die nachstehenden Schritte gemäß des Adapters:

    a. Administratorhandbuch „Five9 Plus Adapter für Agent Desktop Toolkit“: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. Administratorhandbuch „Five9 Plus Adapter für Microsoft Dynamics CRM“: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. Administratorhandbuch „Five9 Plus Adapter für Zendesk“: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/five9-tutorial/create_aaduser_01.png) 

1. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/five9-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/five9-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/five9-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Erstellen eines Five9 Plus Adapter-Testbenutzers (CTI, Contact Center-Agents)

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Five9 Plus Adapter (CTI, Contact Center-Agents). Arbeiten Sie mit dem  [Supportteam von Five9 Plus Adapter (CTI, Contact Center-Agents)](https://www.five9.com/about/contact)  zusammen, um die Benutzer zur Five9 Plus Adapter-Plattform (CTI, Contact Center-Agents) hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Five9 Plus Adapter (CTI, Contact Center-Agents) gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Five9 Plus Adapter (CTI, Contact Center-Agents) zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Liste der Anwendungen **Five9 Plus Adapter (CTI, Contact Center-Agents)** aus.

    ![Configure single sign-on](./media/five9-tutorial/tutorial_five9_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie auf die Kachel „Five9 Plus Adapter (CTI, Contact Center-Agents)“ im Zugriffsbereich klicken, sollten Sie automatisch bei Ihrer Anwendung Five9 Plus Adapter (CTI, Contact Center-Agents) angemeldet werden.
Weitere Informationen zum Zugriffspanel finden Sie unter [Einführung in das Zugriffspanel](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

