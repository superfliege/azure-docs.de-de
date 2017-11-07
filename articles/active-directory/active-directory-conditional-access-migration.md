---
title: Migrieren klassischer Richtlinien in das Azure-Portal | Microsoft-Dokumentation
description: Informationen zum Migrieren klassischer Richtlinien in das Azure-Portal.
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: c584eddb5542c2c49d08d35bcaf8e7acb5c5b83a
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Migrieren klassischer Richtlinien in das Azure-Portal 


Der [bedingte Zugriff](active-directory-conditional-access-azure-portal.md) ist eine Funktion von Azure Active Directory (Azure AD), mit der Sie den Zugriff autorisierter Benutzer auf Ihre Cloud-Apps steuern können. Während der Zweck immer noch derselbe ist, hat die Veröffentlichung des neuen Azure-Portals auch bedeutende Verbesserungen bei der Funktionsweise des bedingten Zugriffs mit sich gebracht. Die Richtlinien für den bedingten Zugriff, die Sie außerhalb des Azure-Portals konfiguriert haben, können mit den neuen Richtlinien, die Sie im Azure-Portal erstellen, gemeinsam vorhanden sein. Solange Sie sie nicht deaktivieren oder entfernen, werden sie weiterhin in Ihrer Umgebung angewendet. Wir empfehlen Ihnen jedoch, Ihre klassischen Richtlinien in die neuen Azure AD-Richtlinien für bedingten Zugriff zu migrieren, und zwar aus folgenden Gründen:

- Die neuen Richtlinien ermöglichen Ihnen, Szenarien anzupacken, die Sie mit klassischen Richtlinien nicht bewältigen konnten.

- Sie können die Anzahl der zu verwaltenden Richtlinien reduzieren, indem Sie diese zusammenführen.   

Dieses Thema unterstützt Sie bei der Migration Ihrer bestehenden klassischen Richtlinien in die neuen Azure AD-Richtlinien für bedingten Zugriff.


## <a name="classic-policies"></a>Klassische Richtlinien

Die Richtlinien für den bedingten Zugriff auf Azure AD und Intune, die Sie nicht im Azure-Portal erstellt haben, werden auch als **klassische Richtlinien** bezeichnet. Um Ihre klassischen Richtlinien zu migrieren, benötigen Sie keinen Zugriff auf Ihr klassisches Azure-Portal. Das Azure-Portal bietet Ihnen die [Ansicht **Klassische Richtlinien (Vorschau)**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies), in der Sie die klassischen Richtlinien überprüfen können.

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


### <a name="open-a-classic-policy"></a>Öffnen einer klassischen Richtlinie

**So öffnen Sie eine klassische Richtlinie**

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Navigationsleiste auf **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/02.png)
 
2. Klicken Sie auf der Seite **Bedingter Zugriff – Richtlinien** im Abschnitt **Verwalten** auf **Klassische Richtlinien (Vorschau)**.

3. Wählen Sie in der Liste der klassischen Richtlinien die gewünschte Richtlinie aus.   

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/34.png)



## <a name="azure-ad-conditional-access-policies"></a>Azure AD-Richtlinien für bedingten Zugriff

In diesem Thema finden Sie detaillierte Anweisungen zum Migrieren Ihrer klassischen Richtlinien, ohne mit Azure AD-Richtlinien für bedingten Zugriff vertraut zu sein. Doch wenn Sie mit den grundlegenden Konzepte und der Terminologie des bedingten Zugriffs in Azure AD vertraut sind, können Sie die Migration verbessern.

Siehe:

- [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md), um mehr über die grundlegenden Konzepte und die Terminologie zu erfahren

- [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md), um sich mit der Benutzeroberfläche im Azure-Portal vertraut zu machen


 





## <a name="multi-factor-authentication-policy"></a>Richtlinie für die mehrstufige Authentifizierung 

Dieses Beispiel zeigt, wie Sie eine klassische Richtlinie migrieren, die eine mehrstufige Authentifizierung** für eine Cloud-App erfordert. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


**So migrieren Sie eine klassische Richtlinie**

1. [Öffnen Sie die klassische Richtlinie](#open-a-classic-policy) zum Abrufen der Konfigurationseinstellungen.
2. Erstellen Sie eine neue Azure AD-Richtlinie für bedingten Zugriff, um Ihre klassische Richtlinie zu ersetzen. 


### <a name="create-a-new-conditional-access-policy"></a>Erstellen einer neuen Richtlinie für bedingten Zugriff


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

    ![Bedingter Zugriff](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

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

11. Deaktivieren Sie die klassische Richtlinie. 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-migration/38.png)



 


## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 
