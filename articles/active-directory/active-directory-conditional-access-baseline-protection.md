---
title: Baselineschutz beim bedingten Zugriff in Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie durch den Baselineschutz sicherstellen, dass in Ihrer Umgebung mindestens eine Basisabsicherung aktiviert ist.
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
ms.date: 06/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 25ae4db2cd4f2a2cea74c428a272c6868acaa5c5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248935"
---
# <a name="what-is-baseline-protection"></a>Was ist Baselineschutz?  

Im vergangenen Jahr sind Angriffe auf Identitäten um 300 % gestiegen. Zum Schutz Ihrer Umgebung vor den ständig zunehmenden Angriffen wird in Azure Active Directory (Azure AD) ein neues Feature mit dem Namen Baselineschutz eingeführt. Baselineschutz umfasst mehrere vordefinierte Richtlinien für den bedingten Zugriff. Ziel dieser Richtlinien ist, sicherzustellen, dass in Ihrer Umgebung mindestens der Baselineschutz aktiviert ist. 

Während der Vorschauphase müssen Sie die Richtlinien des Baselineschutzes aktivieren, damit sie aktiv sind. Nach der allgemeinen Verfügbarkeit sind diese Richtlinien dann standardmäßig aktiviert. 

Die erste Richtlinie des Baselineschutzes erfordert MFA für privilegierte Konten. Angreifer, die Kontrolle über privilegierte Konten erhalten, können enormen Schaden anrichten. Daher ist es wichtig, zunächst diese Konten zu schützen. Folgende privilegierte Rollen fallen in den Anwendungsbereich dieser Richtlinie: 

- Globaler Administrator  

- SharePoint-Administrator  

- Exchange-Administrator  

- Administrator für bedingten Zugriff  

- Sicherheitsadministrator  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

## <a name="how-to-get-started"></a>Erste Schritte 

So aktivieren Sie eine Basisrichtlinie  

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.

2. Klicken Sie im **Azure-Portal** auf der linken Navigationsleiste auf **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

    ![Bedingter Zugriff](./media/active-directory-conditional-access-baseline-protection/03.png)

4. Klicken Sie in der Liste der Richtlinien auf **Baseline policy: Require MFA for admins (Preview)** (Basisrichtlinie: MFA für Administratoren erforderlich (Vorschau)). 

5. Klicken Sie zum Aktivieren der Richtlinie auf **Richtlinie sofort verwenden**.

6. Klicken Sie auf **Speichern**. 
 

In der Basisrichtlinie haben Sie die Möglichkeit, Benutzer und Gruppen auszuschließen. Möglicherweise möchten Sie ein *[Administratorkonto für den Notfallzugriff](active-directory-admin-manage-emergency-access-accounts.md)* ausschließen, um sicherzustellen, dass Sie für den Mandanten nicht gesperrt werden.
  
 

## <a name="what-you-should-know"></a>Wichtige Informationen 

Bei den Verzeichnisrollen, die in der Basisrichtlinie enthalten sind, handelt es sich um Azure AD-Rollen mit den höchsten Berechtigungen. Basierend auf Feedback werden zu einem späteren Zeitpunkt möglicherweise weitere Rollen hinzugefügt. 

Wenn Sie über Konten mit Administratorrechten in Ihren Skripts verfügen, sollten Sie stattdessen [Verwaltete Dienstidentität (MSI)](managed-service-identity/overview.md) oder [Dienstprinzipale (mit Zertifikaten)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) verwenden. Als vorläufige Problemumgehung können Sie bestimmte Benutzerkonten von der Basisrichtlinie ausschließen. 

Die Richtlinie gilt für ältere Authentifizierungsabläufe, z.B. POP, IMAP oder ältere Office-Desktopclients. 

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 
