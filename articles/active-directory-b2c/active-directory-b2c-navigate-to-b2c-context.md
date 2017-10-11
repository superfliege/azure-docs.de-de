---
title: 'Azure Active Directory B2C: Wechsel zu einem B2C-Mandanten | Microsoft Docs'
description: 'Gewusst wie: Wechseln Sie in den Kontext der Active Directory B2C-Mandanten'
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 40d8d57d974a949fbdc0a06eeceb2d06bfbaa09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Wechseln zu Ihrem Azure AD B2C-Mandanten

Um Azure AD B2C konfigurieren zu können, müssen Sie im Rahmen Ihres Azure AD B2C-Mandanten sein.

## <a name="log-into-azure-ad-b2c-tenant"></a>Melden Sie sich bei Azure AD B2C-Mandanten

Um Ihrem Azure AD B2C-Mandanten zu navigieren, müssen Sie als globaler Administrator des Azure AD B2C-Mandanten in Azure-Portal angemeldet sein.

1. Melden Sie sich die [Azure-Portal](http://portal.azure.com).
1. Wechseln Sie Mandanten, indem Sie auf Ihre e-Mail-Adresse oder ein Bild in der oberen rechten Ecke.
1. In der `Directory` Liste, die angezeigt wird, wählen Sie den Azure AD B2C-Mandanten, die Sie verwalten möchten.

Der Azure-Verwaltungsportal wird aktualisiert.  Sie sind jetzt an das Azure-Portal im Rahmen Ihres Azure AD B2C-Mandanten angemeldet.

## <a name="navigate-to-the-b2c-features-blade"></a>Navigieren Sie zu dem Blatt "B2C-Funktionen"

1. Klicken Sie auf **Durchsuchen** auf der linken Navigationsleiste.
1. Klicken Sie auf **> Weitere Dienste** , suchen Sie nach `Azure AD B2C` im linken Navigationsbereich.  (Um an der linken Seite Ihr Startmenü anzuheften anzuheften, klicken Sie auf den Stern auf der linken Seite des Azure AD B2C)
1. Klicken Sie auf **Azure AD B2C** auf dem Blatt "B2C-Features" zugreifen.
   
    ![Screenshot der navigieren Sie zum Blatt "B2C-Funktionen"](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Sie müssen ein Globaladministrator des B2C-Mandanten, für die auf dem Blatt "B2C-Features" zugreifen können. Ein Globaladministrator von keinem anderen Mandanten oder einen Benutzer aus jeder Mandant kann nicht darauf zugreifen.  Sie können in Ihren B2C-Mandanten mithilfe der Switcher Mandanten in der oberen rechten Ecke des Azure-Portal wechseln.
