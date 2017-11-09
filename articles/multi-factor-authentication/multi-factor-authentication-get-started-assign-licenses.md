---
title: "Zuweisen von Lizenzen für Azure MFA | Microsoft-Dokumentation"
description: "Erfahren Sie wie Sie Benutzerlizenzen für Microsoft Azure Multi-Factor Authentication zuweisen."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 514ef423-8ee6-4987-8a4e-80d5dc394cf9
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/13/2017
ms.author: joflore
ms.reviewer: yossib
ms.custom: it-pro
ROBOTS: NOINDEX
ms.openlocfilehash: 86e8e5ce183d11a76fb679da0e6c707383ce86d0
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2017
---
# <a name="assigning-an-azure-mfa-azure-ad-premium-or-enterprise-mobility-license-to-users"></a>Zuweisen einer Azure MFA-, Azure AD Premium- oder Enterprise Mobility-Lizenz zu Benutzern
Wenn Sie Azure MFA-, Azure AD Premium- oder Enterprise Mobility Suite-Lizenzen erworben haben, müssen Sie keinen Multi-Factor Auth-Anbieter erstellen. Nachdem Sie Ihren Benutzern die Lizenzen zugewiesen haben, können Sie sie für MFA aktivieren.

## <a name="to-assign-a-license"></a>So weisen Sie eine Lizenz zu
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator an.
2. Wählen Sie im linken Bereich **Active Directory**aus.
3. Doppelklicken Sie auf der Seite „Active Directory“ auf das Verzeichnis mit den Benutzern, die Sie aktivieren möchten.
4. Wählen Sie oben auf der Seite des Verzeichnisses die Option **Lizenzen**aus.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign1.png)
5. Wählen Sie auf der Seite mit den Lizenzen die Option **Azure Multi-Factor Authentication**, **Active Directory Premium** oder **Enterprise Mobility Suite** aus.  Bei nur einer Lizenz ist diese automatisch ausgewählt.
6. Klicken Sie unten auf der Seite auf **Zuweisen**.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign3.png)
7. Klicken Sie im angezeigten Feld neben die Benutzer oder Gruppen, denen Sie Lizenzen zuweisen möchten.  Daraufhin wird ein grünes Häkchen angezeigt.
8. Klicken Sie auf das Häkchen, um die Änderungen zu speichern.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign4.png)
9. Anschließend erscheint eine Nachricht, die angibt, wie viele Lizenzen zugewiesen wurden und wie viele fehlgeschlagen sind.  Klicken Sie auf **OK**.
   ![Lizenzen zuweisen](./media/multi-factor-authentication-get-started-assign-licenses/assign5.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Was ist Microsoft Azure Active Directory-Lizenzierung?](../active-directory/active-directory-licensing-what-is.md)
