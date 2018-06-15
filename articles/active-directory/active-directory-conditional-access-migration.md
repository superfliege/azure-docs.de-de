---
title: Migrieren klassischer Richtlinien in das Azure-Portal | Microsoft-Dokumentation
description: Wichtige Informationen zum Migrieren klassischer Richtlinien in das Azure-Portal.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: femila
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: b6285381833526cdbdd7c1392894458a47c1cf34
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34723727"
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Migrieren klassischer Richtlinien in das Azure-Portal 


Der [bedingte Zugriff](active-directory-conditional-access-azure-portal.md) ist eine Funktion von Azure Active Directory (Azure AD), mit der Sie den Zugriff autorisierter Benutzer auf Ihre Cloud-Apps steuern können. Während der Zweck immer noch derselbe ist, hat die Veröffentlichung des neuen Azure-Portals bedeutende Verbesserungen bei der Funktionsweise des bedingten Zugriffs mit sich gebracht.

Aus den folgenden Gründen sollten Sie das Migrieren der Richtlinien, die Sie nicht im Azure-Portal erstellt haben, in Erwägung ziehen:

- Sie können jetzt Szenarien berücksichtigen, die bisher nicht behandelt werden konnten.

- Sie können die Anzahl der zu verwaltenden Richtlinien reduzieren, indem Sie diese zusammenführen.   

- Sie können alle bedingten Zugriffsrichtlinien an einem zentralen Ort verwalten.

- Das klassische Azure-Portal wird eingestellt.   

In diesem Artikel erhalten Sie wichtige Informationen zum Migrieren Ihrer vorhandenen Richtlinien für bedingten Zugriff zum neuen Framework.
 
## <a name="classic-policies"></a>Klassische Richtlinien

Im [Azure-Portal](https://portal.azure.com) stellt die Seite [Bedingter Zugriff – Richtlinien](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) Ihren Einstiegspunkt in die Richtlinien für bedingten Zugriff dar. In Ihrer Umgebung sind jedoch möglicherweise auch Richtlinien für den bedingten Zugriff in Kraft, die Sie nicht mithilfe dieser Seite erstellt haben. Diese Richtlinien werden als *klassische Richtlinien* bezeichnet. Klassische Richtlinien sind Richtlinien für bedingten Zugriff, die Sie an diesen Orten erstellt haben:

- Klassisches Azure-Portal
- Klassisches Intune-Portal
- Intune-App-Schutz-Portal


Auf der Seite **Bedingter Zugriff** können Sie auf Ihre klassischen Richtlinien zugreifen, indem Sie im Bereich **Verwalten** auf [**Klassische Richtlinien (Vorschau)**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) klicken. 


![Azure Active Directory](./media/active-directory-conditional-access-migration/71.png)


Die Ansicht **Klassische Richtlinien** bietet Optionen für diese Aktionen:

- Filtern Ihrer klassischen Richtlinien.
 
    ![Azure Active Directory](./media/active-directory-conditional-access-migration/72.png)

- Deaktivieren von klassischen Richtlinien.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/73.png)
   
- Überprüfen der Einstellungen einer klassischen Richtlinie (mit der Möglichkeit zur Deaktivierung).

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/74.png)


Wenn Sie eine klassische Richtlinie deaktiviert haben, lässt sich dieser Schritt nicht mehr rückgängig machen. Das ist der Grund, warum sich die Gruppenmitgliedschaft bei einer klassischen Richtlinie mithilfe der Ansicht **Details** ändern lässt. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/75.png)

Indem Sie entweder die ausgewählten Gruppen ändern oder bestimmte Gruppen ausschließen, können Sie die Wirkung einer deaktivierten klassischen Richtlinie an einigen Testbenutzern ausprobieren, bevor Sie die Richtlinie für alle enthaltenen Benutzer und Gruppen deaktivieren. 



## <a name="azure-ad-conditional-access-policies"></a>Azure AD-Richtlinien für bedingten Zugriff

Mit den Optionen für bedingten Zugriff im Azure-Portal können Sie alle Ihre Richtlinien an einem zentralen Ort verwalten. Da sich die Art der Implementierung von bedingtem Zugriff erheblich geändert hat, sollten Sie sich vor dem Migrieren Ihrer klassischen Richtlinien mit den zugrundeliegenden Konzepten vertraut machen.

Siehe:

- [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md), um mehr über die grundlegenden Konzepte und die Terminologie zu erfahren.

- [Bewährte Methoden für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md), um hilfreiche Informationen zum Bereitstellen von bedingtem Zugriff in Ihrer Organisation zu erhalten.

- [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md), um sich mit der Benutzeroberfläche im Azure-Portal vertraut zu machen.


 
## <a name="migration-considerations"></a>Überlegungen zur Migration

In diesen Artikel werden Azure AD-Richtlinien für bedingten Zugriff auch als *neue Richtlinien* bezeichnet.
Ihre klassischen Richtlinien funktionieren weiterhin parallel zu Ihren neuen Richtlinien, bis Sie sie deaktivieren oder löschen. 

Die folgenden Aspekte sind im Zusammenhang mit der Konsolidierung von Richtlinien wichtig:

- Während klassische Richtlinien an eine bestimmte Cloud-App gebunden sind, können Sie in einer neuen Richtlinie beliebig viele Cloud-Apps auswählen.

- In den Steuerelementen einer klassischen Richtlinie müssen in Verbindung mit einer neuen Richtlinie für eine Cloud-Apps alle Steuerelemente (*UND*) erfüllt sein. 


- Eine neue Richtlinie eröffnet Ihnen folgende Möglichkeiten:
 
    - Kombinieren mehrerer Bedingungen, wenn das für Ihr Szenario erforderlich ist. 

    - Auswählen mehrerer Erteilungsanforderungen als Zugriffssteuerung, die mit logischem *ODER* (eins der ausgewählten Steuerelemente muss erfüllt sein) oder mit logischen *UND* (alle aktivierten Steuerelemente müssen erfüllt sein) kombiniert werden können.

        ![Azure Active Directory](./media/active-directory-conditional-access-migration/25.png)




### <a name="office-365-exchange-online"></a>Office 365 Exchange Online

Wenn Sie klassische Richtlinien für **Office 365 Exchange Online** migrieren möchten, die **Exchange Active Sync** als Bedingung für Client-Apps beinhalten, ist eine Konsolidierung in einer neuen Richtlinie möglicherweise nicht möglich. 

Dies ist beispielsweise dann der Fall, wenn Sie alle Typen von Client-Apps unterstützen möchten. In einer neuen Richtlinie, die **Exchange Active Sync** als Bedingung für Client-Apps beinhaltet, können keine weiteren Client-Apps ausgewählt werden.

![Azure Active Directory](./media/active-directory-conditional-access-migration/64.png)

Eine Konsolidierung in einer neuen Richtlinie ist auch dann nicht möglich, wenn Ihre klassischen Richtlinien mehrere Bedingungen enthalten. Eine neue Richtlinie, in der **Exchange Active Sync** als Bedingung für Client-Apps konfiguriert ist, unterstützt keine weiteren Bedingungen:   

![Azure Active Directory](./media/active-directory-conditional-access-migration/08.png)

Wenn Sie über eine neue Richtlinie verfügen, in der **Exchange Active Sync** als Client-App-Bedingung konfiguriert ist, müssen Sie sich vergewissern, dass keine weiteren Bedingungen konfiguriert sind. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/16.png)
 

[App-basierte](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) klassische Richtlinien für Office 365 Exchange Online, die **Exchange Active Sync** als Bedingung für Client-Apps enthalten, lassen **unterstützte** und **nicht unterstützte** [Geräteplattformen](active-directory-conditional-access-technical-reference.md#device-platform-condition) zu. Zwar können in einer zugeordneten neuen Richtlinie keine einzelnen Geräteplattformen konfiguriert werden, jedoch können Sie die Unterstützung auf [unterstützte Geräteplattformen](active-directory-conditional-access-technical-reference.md#device-platform-condition) einschränken. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/65.png)

Sie können mehrere klassische Richtlinien konsolidieren, die **Exchange Active Sync** als Bedingung für Client-Apps enthalten, wenn diese Punkte zutreffen:

- Nur **Exchange Active Sync** ist als Bedingung festgelegt 

- Es sind mehrere Anforderungen für das Erteilen des Zugriffs konfiguriert

Ein häufiges Szenario ist die Konsolidierung von:

- einer gerätebasierten klassischen Richtlinie aus dem klassischen Azure-Portal 
- einer App-basierten klassischen Richtlinie in Intune-App-Schutz-Portal 
 
In diesem Fall können Sie Ihre klassischen Richtlinien in einer neuen Richtlinie konsolidieren, für die beide Anforderungen aktiviert sind.

![Azure Active Directory](./media/active-directory-conditional-access-migration/62.png)



### <a name="device-platforms"></a>Geräteplattformen

Klassische Richtlinien mit [App-basierten Steuerelementen](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) sind bereits mit iOS und Android als [Bedingung für die Geräteplattform](active-directory-conditional-access-technical-reference.md#device-platform-condition) konfiguriert. 

In einer neuen Richtlinie müssen Sie die zu unterstützenden [Geräteplattformen](active-directory-conditional-access-technical-reference.md#device-platform-condition) einzeln auswählen.

![Azure Active Directory](./media/active-directory-conditional-access-migration/41.png)



 
 


## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, helfen Ihnen die Informationen unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) weiter.

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 
