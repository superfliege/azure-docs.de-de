---
title: "Zuweisen von Icenses für Azure MFA | Microsoft Docs"
description: "Informationen Sie zum Zuweisen von Lizenzen für Microsoft Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 514ef423-8ee6-4987-8a4e-80d5dc394cf9
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/13/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ROBOTS: NOINDEX
ms.openlocfilehash: 45522bf526c4aeab1d6ccc8891a55a0436ff9320
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="assigning-an-azure-mfa-azure-ad-premium-or-enterprise-mobility-license-to-users"></a>Benutzern eine Azure MFA, Azure AD Premium oder Enterprise Mobility-Lizenz zugewiesen
Wenn Sie Azure MFA, Azure AD Premium oder Enterprise Mobility Suite-Lizenzen erworben haben, müssen Sie nicht zum Erstellen eines Multi-Factor Authentication-Anbieters. Nachdem Sie Ihren Benutzern Lizenzen zuweisen, können Sie beginnen, aktivieren sie für MFA.

## <a name="to-assign-a-license"></a>Zuweisen eine Lizenz
1. Melden Sie sich bei der [klassischen Azure-Portal](https://manage.windowsazure.com) als Administrator.
2. Wählen Sie auf der linken Seite **Active Directory**.
3. Doppelklicken Sie auf der Seite "Active Directory" auf das Verzeichnis, das der Benutzer gibt, die Sie aktivieren möchten.
4. Wählen Sie am oberen Rand der Seite "Quellverzeichnis", **Lizenzen**.
   ![Zuweisen von Lizenzen](./media/multi-factor-authentication-get-started-assign-licenses/assign1.png)
5. Wählen Sie auf der Seite "Lizenzen" **Azure Multi-Factor Authentication**, **Active Directory Premium**, oder **Enterprise Mobility Suite**.  Wenn Sie nur eine haben, sollte dieser automatisch ausgewählt.
6. Klicken Sie am unteren Rand der Seite, auf **zuweisen**.
   ![Zuweisen von Lizenzen](./media/multi-factor-authentication-get-started-assign-licenses/assign3.png)
7. Klicken Sie in das Feld, das auftritt neben dem Benutzer oder Gruppen, denen Sie Lizenzen zuweisen möchten.  Daraufhin sollte ein grünes Häkchen angezeigt.
8. Klicken Sie auf das Häkchensymbol, um die Änderungen zu speichern.
   ![Zuweisen von Lizenzen](./media/multi-factor-authentication-get-started-assign-licenses/assign4.png)
9. Daraufhin sollte eine Meldung angezeigt, wie viele Lizenzen zugewiesen wurden und wie viele fehlgeschlagen ist.  Klicken Sie auf **OK**.
   ![Zuweisen von Lizenzen](./media/multi-factor-authentication-get-started-assign-licenses/assign5.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Was ist Microsoft Azure Active Directory-Lizenzierung?](../active-directory/active-directory-licensing-what-is.md)
