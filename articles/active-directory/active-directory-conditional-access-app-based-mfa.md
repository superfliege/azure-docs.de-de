---
title: 'Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory | Microsoft-Dokumentation'
description: In diesem Schnellstart erfahren Sie, wie Sie Ihre Authentifizierungsanforderungen über den bedingten Zugriff von Azure Active Directory (Azure AD) an den Typ der Cloud-App binden, auf die zugegriffen wird.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 1501ca1c036a8db1d53b9b27170d9ae05d41f797
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724114"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory 

Um den Anmeldevorgang für Ihre Benutzer einfacher zu gestalten, möchten Sie ihnen vielleicht die Anmeldung bei Cloud-Apps über Benutzername und Kennwort gestatten. Allerdings gibt es in vielen Umgebungen mindestens ein paar Apps, bei denen es ratsam ist, eine stärkere Form der Kontoüberprüfung (z. B. die mehrstufige Authentifizierung) vorzuschreiben. Dies kann zum Beispiel beim Zugriff auf das E-Mail-System Ihres Unternehmens oder die Apps der Personalabteilung der Fall sein. In Azure Active Directory (Azure AD) können Sie dieses Ziel mit einer Richtlinie für bedingten Zugriff erreichen.    

Dieser Schnellstart veranschaulicht das Konfigurieren einer [Azure AD-Richtlinie für bedingten Zugriff](active-directory-conditional-access-azure-portal.md), durch die für eine ausgewählte Cloud-App in Ihrer Umgebung die mehrstufige Authentifizierung erforderlich wird.

![Richtlinie erstellen](./media/active-directory-conditional-access-app-based-mfa/32.png)


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.



## <a name="prerequisites"></a>Voraussetzungen 

Für die Durchführung des Szenarios im Rahmen dieses Schnellstarts benötigen Sie Folgendes:

- **Zugriff auf eine Azure AD Premium-Edition**: Der bedingte Zugriff von Azure AD ist eine Funktion, die in Azure AD Premium enthalten ist. 

- **Ein Testkonto unter dem Namen Isabella Simonsen**: Wenn Sie nicht wissen, wie Sie ein Testkonto erstellen, lesen Sie [Hinzufügen von cloudbasierten Benutzern](add-users-azure-active-directory.md#add-cloud-based-users).



## <a name="create-your-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff 

In diesem Abschnitt wird gezeigt, wie Sie die erforderliche Richtlinie für bedingten Zugriff erstellen. Für das Szenario in diesem Schnellstart wird Folgendes verwendet:

- Azure-Portal als Platzhalter für eine Cloud-App, für welche die mehrstufige Authentifizierung erforderlich ist 
- Beispielbenutzer zum Testen der Richtlinie für bedingten Zugriff  

Legen Sie in Ihrer Richtlinie Folgendes fest:

|Einstellung |Wert|
|---     | --- |
|Benutzer und Gruppen | Isabella Simonsen |
|Cloud-Apps | Microsoft Azure Management |
|Gewähren von Zugriff | Mehrstufige Authentifizierung erforderlich |
 

![Richtlinie erstellen](./media/active-directory-conditional-access-app-based-mfa/31.png)

 


**So konfigurieren Sie die Richtlinie für bedingten Zugriff**

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) als globaler Administrator an.

2. Klicken Sie im Azure-Portal auf der linken Navigationsleiste auf **Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-app-based-mfa/02.png)

3. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-app-based-mfa/03.png)
 
4. Klicken Sie auf der Seite **Bedingter Zugriff** auf der Symbolleiste am oberen Rand auf **Hinzufügen**.

    ![Hinzufügen](./media/active-directory-conditional-access-app-based-mfa/04.png)

5. Geben Sie auf der Seite **Neu** im Textfeld **Name** die Textfolge **Mehrstufige Authentifizierung für Zugriff auf Azure-Portal erforderlich** ein.

    ![NAME](./media/active-directory-conditional-access-app-based-mfa/05.png)

6. Klicken Sie im Abschnitt **Zuweisung** auf **Benutzer und Gruppen**.

    ![Benutzer und Gruppen](./media/active-directory-conditional-access-app-based-mfa/06.png)

7. Führen Sie auf der Seite **Benutzer und Gruppen** die folgenden Schritte aus:

    ![Benutzer und Gruppen](./media/active-directory-conditional-access-app-based-mfa/24.png)

    a. Klicken Sie auf **Benutzer und Gruppen auswählen**, und wählen Sie dann **Benutzer und Gruppen** aus.

    b. Klicken Sie auf **Auswählen**.

    c. Wählen Sie auf der Seite **Auswählen** den Benutzer **Isabella Simonsen** aus, und klicken Sie dann auf **Auswählen**.

    d. Klicken Sie auf der Seite **Benutzer und Gruppen** auf **Fertig**.

8. Klicken Sie auf **Cloud-Apps**.

    ![Cloud-Apps](./media/active-directory-conditional-access-app-based-mfa/08.png)

9. Führen Sie auf der Seite **Cloud-Apps** die folgenden Schritte aus:

    ![Auswählen der Cloud-Apps](./media/active-directory-conditional-access-app-based-mfa/26.png)

    a. Klicken Sie auf **Apps auswählen**.

    b. Klicken Sie auf **Auswählen**.

    c. Wählen Sie auf der Seite **Auswählen** die Option **Microsoft Azure Management** aus, und klicken Sie dann auf **Auswählen**.

    d. Klicken Sie auf der Seite **Cloud-Apps** auf **Fertig**.


10. Klicken Sie im Abschnitt **Zugriffssteuerungen** auf **Gewähren**.

    ![Zugriffssteuerung](./media/active-directory-conditional-access-app-based-mfa/10.png)

11. Führen Sie auf der Seite **Gewährung** die folgenden Schritte aus:

    ![Gewährung](./media/active-directory-conditional-access-app-based-mfa/11.png)

    a. Wählen Sie **Zugriff gewähren** aus.

    a. Wählen Sie **Erfordern von Multi-Factor Authentication**.

    b. Klicken Sie auf **Auswählen**.

12. Klicken Sie im Abschnitt **Richtlinie aktivieren** auf **Ein**.

    ![Richtlinie aktivieren](./media/active-directory-conditional-access-app-based-mfa/18.png)

13. Klicken Sie auf **Create**.


## <a name="evaluate-a-simulated-sign-in"></a>Auswerten einer simulierten Anmeldung

Nachdem Sie nun Ihre Richtlinie für bedingten Zugriff konfiguriert haben, möchten Sie wahrscheinlich wissen, ob sie erwartungsgemäß funktioniert. Verwenden Sie im ersten Schritt das What If-Richtlinientool des bedingten Zugriffs, um eine Anmeldung für Ihren Testbenutzer zu simulieren. Die Simulation schätzt die Auswirkungen dieser Anmeldungen auf Ihre Richtlinien ab und generiert einen Simulationsbericht.  

Legen Sie zum Initialisieren des What If-Richtlinienauswertungstools Folgendes fest:

- Benutzer: **Isabella Simonsen** 
- Cloud-App: **Microsoft Azure Management**

 Durch Klicken auf **What If** wird ein Simulationsbericht mit folgenden Informationen erstellt:

- **Mehrstufige Authentifizierung für Zugriff auf Azure-Portal erforderlich** unter **Anzuwendende Richtlinien** 
- **Mehrstufige Authentifizierung erforderlich** unter **Gewährungssteuerelemente**

![What If-Richtlinientool](./media/active-directory-conditional-access-app-based-mfa/23.png)



**So werten Sie die Richtlinie für bedingten Zugriff aus**

1. Klicken Sie auf der Seite [Bedingter Zugriff – Richtlinien](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) im Menü am oberen Rand auf **What If**.  
 
    ![What If](./media/active-directory-conditional-access-app-based-mfa/14.png)

2. Klicken Sie auf **Benutzer**, wählen Sie **Isabella Simonsen** aus, und klicken Sie dann auf **Auswählen**.

    ![Benutzer](./media/active-directory-conditional-access-app-based-mfa/15.png)

2. Um eine Cloud-App auszuwählen, führen Sie die folgenden Schritte aus:

    ![Cloud-Apps](./media/active-directory-conditional-access-app-based-mfa/16.png)

    a. Klicken Sie auf **Cloud-Apps**.

    b. Klicken Sie auf der Seite **Cloud-Apps** auf **Apps auswählen**.

    c. Klicken Sie auf **Auswählen**.

    d. Wählen Sie auf der Seite **Auswählen** die Option „Microsoft Azure Management“** aus, und klicken Sie dann auf **Auswählen**.

    e. Klicken Sie auf der Seite „Cloud-Apps“ auf **Fertig**.

3. Klicken Sie auf **What If**.


## <a name="test-your-conditional-access-policy"></a>Testen der Richtlinie für bedingten Zugriff

Im vorherigen Abschnitt haben Sie gelernt, wie Sie eine simulierte Anmeldung auswerten. Zusätzlich zu einer Simulation sollten Sie auch Ihre Richtlinie für bedingten Zugriff testen, um sicherzustellen, dass sie wie erwartet funktioniert. 

Um Ihre Richtlinie zu testen, versuchen Sie, sich im [Azure-Portal](https://portal.azure.com) mit dem Testkonto **Isabella Simonsen** anzumelden. Es sollte ein Dialogfeld angezeigt werden, in dem Sie Ihr Konto für eine zusätzliche Sicherheitsüberprüfung einrichten müssen.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den Testbenutzer, wenn er nicht mehr benötigt wird, und löschen Sie auch die Richtlinie für bedingten Zugriff:

- Wenn Sie nicht wissen, wie ein Azure AD-Benutzer gelöscht wird, lesen Sie [Löschen von Benutzern aus Azure AD](add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Zum Löschen Ihrer Richtlinie wählen Sie die Richtlinie aus, und klicken Sie dann in der Symbolleiste für den Schnellzugriff auf **Löschen**.

    ![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/33.png)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum bedingten Zugriff finden Sie unter [Conditional access in Azure Active Directory - preview](active-directory-conditional-access-azure-portal.md) (Bedingter Zugriff in Azure Active Directory – Vorschau).

