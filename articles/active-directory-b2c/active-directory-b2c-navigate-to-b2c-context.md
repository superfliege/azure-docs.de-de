---
title: 'Azure Active Directory B2C: Wechseln zu einem B2C-Mandanten | Microsoft-Dokumentation'
description: Wechseln zum Kontext des Active Directory B2C-Mandanten
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 6c1fd08c52f33a062d06e0593cbbe00346bb44f1
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Wechseln zum Azure AD B2C-Mandanten

Damit Sie Azure AD B2C konfigurieren können, müssen Sie sich im Kontext des Azure AD B2C-Mandanten befinden.

## <a name="log-into-azure-ad-b2c-tenant"></a>Anmelden bei einem Azure AD-B2C-Mandanten

Damit Sie zu Ihrem Azure AD B2C-Mandanten navigieren können, müssen Sie beim Azure-Portal als globaler Administrator des Azure AD B2C-Mandanten angemeldet sein.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
1. Klicken Sie auf Ihre E-Mail-Adresse oder das Bild in der oberen rechten Ecke, um Mandanten zu wechseln.
1. Wählen Sie in der daraufhin angezeigten `Directory`-Liste den Azure AD B2C-Mandanten aus, den Sie verwalten möchten.

Das Azure-Portal wird aktualisiert.  Nun sind Sie beim Azure-Portal im Kontext Ihres Azure AD B2C-Mandanten angemeldet.

## <a name="navigate-to-the-b2c-features-blade"></a>Navigieren zum Blatt „B2C-Funktionen“

1. Klicken Sie im linken Navigationsbereich auf **Durchsuchen** .
1. Klicken Sie im linken Navigationsbereich auf **> Weitere Dienste**, und suchen Sie nach `Azure AD B2C`.  (Klicken Sie zum Anheften an das linke Startmenü auf das Sternchen links neben Azure AD B2C).
1. Klicken Sie auf **Azure AD B2C** , um auf das Blatt „B2C-Funktionen“ zuzugreifen.
   
    ![Screenshot des Blatts „Navigieren zu B2C-Funktionen“](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Sie müssen als globaler Administrator des B2C-Mandanten festgelegt sein, um auf das Blade mit den B2C-Features zugreifen zu können. Globale Administratoren anderer Mandanten oder Benutzer von Mandanten haben keinen Zugriff.  Mit dem Mandantenumschalter oben rechts im Azure-Portals können Sie zu Ihrem B2C-Mandanten wechseln.
