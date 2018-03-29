---
title: 'Azure Active Directory B2C: Wechseln zu einem B2C-Mandanten | Microsoft-Dokumentation'
description: Wechseln zum Kontext des Active Directory B2C-Mandanten
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 4/13/2017
ms.author: davidmu
ms.openlocfilehash: 6469636ee05c2b28a24b1d559f0ee7569e784393
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Wechseln zum Azure AD B2C-Mandanten

Damit Sie Azure AD B2C konfigurieren können, müssen Sie sich im Kontext des Azure AD B2C-Mandanten befinden.

## <a name="log-into-azure-ad-b2c-tenant"></a>Anmelden bei einem Azure AD-B2C-Mandanten

Damit Sie zu Ihrem Azure AD B2C-Mandanten navigieren können, müssen Sie beim Azure-Portal als globaler Administrator des Azure AD B2C-Mandanten angemeldet sein.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an.
1. Klicken Sie auf Ihre E-Mail-Adresse oder das Bild in der oberen rechten Ecke, um Mandanten zu wechseln.
1. Wählen Sie in der daraufhin angezeigten `Directory`-Liste den Azure AD B2C-Mandanten aus, den Sie verwalten möchten.

Das Azure-Portal wird aktualisiert.  Nun sind Sie beim Azure-Portal im Kontext Ihres Azure AD B2C-Mandanten angemeldet.

## <a name="navigate-to-the-b2c-features-pane"></a>Navigieren zum Bereich „B2C-Funktionen“

1. Klicken Sie im linken Navigationsbereich auf **Durchsuchen**.
1. Klicken Sie im linken Navigationsbereich auf **Alle Dienste**, und suchen Sie nach `Azure AD B2C`.  (Klicken Sie zum Anheften an das linke Startmenü auf das Sternchen links neben Azure AD B2C).
1. Klicken Sie auf **Azure AD B2C**, um auf den Bereich „B2C-Funktionen“ zuzugreifen.
   
    ![Screenshot der Navigation zum Bereich „B2C-Funktionen“](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Sie müssen ein globaler Administrator des B2C-Mandanten sein, um auf den Bereich mit den B2C-Features zugreifen zu können. Globale Administratoren anderer Mandanten oder Benutzer von Mandanten haben keinen Zugriff.  Mit dem Mandantenumschalter oben rechts im Azure-Portals können Sie zu Ihrem B2C-Mandanten wechseln.
