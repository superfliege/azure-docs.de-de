---
title: Migrieren einer klassischen Richtlinie, die mehrstufige Authentifizierung erfordert, in das Azure-Portal | Microsoft-Dokumentation
description: Dieser Artikel zeigt, wie Sie eine klassische Richtlinie, die eine mehrstufige Authentifizierung erfordert, in das Azure-Portal migrieren.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6190a8ee90855223779751373bf16ca3db0fe761
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37870835"
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Migrieren einer klassischen Richtlinie, die mehrstufige Authentifizierung erfordert, in das Azure-Portal 

Dieser Artikel zeigt, wie Sie eine klassische Richtlinie migrieren, die eine **mehrstufige Authentifizierung** für eine Cloud-App erfordert. Es stellt zwar keine Voraussetzung dar, wir empfehlen Ihnen aber, [Migrieren klassischer Richtlinien in das Azure-Portal](active-directory-conditional-access-migration.md) zu lesen, bevor Sie mit der Migration Ihrer klassischen Richtlinien beginnen.


 
## <a name="overview"></a>Übersicht 

Das Szenario in diesem Artikel zeigt, wie Sie eine klassische Richtlinie migrieren, die eine **mehrstufige Authentifizierung** für eine Cloud-App erfordert. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


Der Migrationsvorgang besteht aus folgenden Schritten:

1. [Öffnen Sie die klassische Richtlinie](#open-a-classic-policy) zum Abrufen der Konfigurationseinstellungen.
2. Erstellen Sie eine neue Azure AD-Richtlinie für bedingten Zugriff, um Ihre klassische Richtlinie zu ersetzen. 
3. Deaktivieren Sie die klassische Richtlinie.



## <a name="open-a-classic-policy"></a>Öffnen einer klassischen Richtlinie

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Navigationsleiste auf **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration-mfa/01.png)

2. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration-mfa/02.png)

3. Klicken Sie im Abschnitt **Verwalten** auf **Klassische Richtlinien (Vorschau)**.

    ![Klassische Richtlinien](./media/active-directory-conditional-access-migration-mfa/12.png)

4. Klicken Sie in der Liste der klassischen Richtlinien auf die Richtlinie, die eine **mehrstufige Authentifizierung** für eine Cloud-App erfordert.

    ![Klassische Richtlinien](./media/active-directory-conditional-access-migration-mfa/13.png)


## <a name="create-a-new-conditional-access-policy"></a>Erstellen einer neuen Richtlinie für bedingten Zugriff


1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Navigationsleiste auf **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/02.png)



3. Klicken Sie zum Öffnen der Seite **Neu** auf der Seite **Bedingter Zugriff** oben auf der Symbolleiste auf **Hinzufügen**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/03.png)

4. Geben Sie auf der Seite **Neu** im Textfeld **Name** einen Namen für Ihre Richtlinie ein.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/29.png)

5. Klicken Sie im Abschnitt **Zuweisungen** auf **Benutzer und Gruppen**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/05.png)

    a. Wenn Sie alle Benutzer in der klassischen Richtlinie ausgewählt haben, klicken Sie auf **Alle Benutzer**. 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/35.png)

    b. Wenn Sie Gruppen in Ihrer klassischen Richtlinie ausgewählt haben, klicken Sie auf **Benutzer und Gruppen auswählen**, und wählen Sie dann die gewünschten Benutzer und Gruppen aus.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/36.png)

    c. Wenn Sie ausgeschlossene Gruppen haben, klicken Sie auf die Registerkarte **Ausschließen**, und wählen Sie dann die gewünschten Benutzer und Gruppen aus. 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/37.png)

6. Klicken Sie zum Öffnen der Seite **Cloud-Apps** auf der Seite **Neu** im Abschnitt **Zuweisung** auf **Cloud-Apps**.

8. Führen Sie auf der Seite **Cloud-Apps** die folgenden Schritte aus:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/08.png)

    a. Klicken Sie auf **Apps auswählen**.

    b. Klicken Sie auf **Auswählen**.

    c. Wählen Sie auf der Seite **Auswählen** Ihre Cloud-App aus, und klicken Sie dann auf **Auswählen**.

    d. Klicken Sie auf der Seite **Cloud-Apps** auf **Fertig**.



9. Falls **Mehrstufige Authentifizierung anfordern** ausgewählt ist:

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/26.png)

    a. Klicken Sie im Abschnitt **Zugriffssteuerungen** auf **Gewähren**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/27.png)

    b. Klicken Sie auf der Seite **Gewähren** auf **Zugriff gewähren**, und klicken Sie dann auf **Mehrstufige Authentifizierung anfordern**.

    c. Klicken Sie auf **Auswählen**.


10. Klicken Sie auf **Ein**, um Ihre Richtlinie zu aktivieren.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/30.png)



## <a name="disable-the-classic-policy"></a>Deaktivieren der klassischen Richtlinie

Klicken Sie zum Deaktivieren Ihrer klassischen Richtlinie in der Ansicht **Details** auf **Deaktivieren**.

![Klassische Richtlinien](./media/active-directory-conditional-access-migration-mfa/14.png)



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über die Migration von klassischen Richtlinien finden Sie unter [Migrieren klassischer Richtlinien in das Azure-Portal](active-directory-conditional-access-migration.md).


- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, finden Sie Informationen unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](active-directory-conditional-access-app-based-mfa.md).

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 
