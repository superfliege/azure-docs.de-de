---
title: Konfigurieren des gerätebasierten bedingten Zugriffs für Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory konfigurieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1c21c915bc0a83cdafb221a2cd592890577437ee
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849524"
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Konfigurieren von Richtlinien für den gerätebasierten bedingten Zugriff für Azure Active Directory

Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie den Zugriff von autorisierten Benutzern auf Ihre Ressourcen steuern. Sie können z. B. den Zugriff auf bestimmte Ressourcen auf verwaltete Geräte beschränken. Eine Richtlinie für den bedingten Zugriff, die ein verwaltetes Gerät erfordert, wird auch als Richtlinie für den gerätebasierten bedingten Zugriff bezeichnet.

In diesem Thema wird beschrieben, wie Sie gerätebasierte Richtlinien für bedingten Zugriff für über Azure AD verbundene Anwendungen konfigurieren können. 


## <a name="before-you-begin"></a>Voraussetzungen

Beim gerätebasierten bedingten Zugriff werden der **bedingte Zugriff von Azure AD** und die **Geräteverwaltung von Azure AD** miteinander kombiniert. Wenn Sie mit einem dieser Bereiche noch nicht vertraut sind, sollten Sie zunächst die folgenden Themen lesen:

- **[Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md):** Dieses Thema bietet eine konzeptionelle Übersicht über den bedingten Zugriff und die zugehörige Terminologie.

- **[Einführung in die Geräteverwaltung in Azure Active Directory](device-management-introduction.md):** Dieses Thema bietet eine Übersicht über die verschiedenen Optionen zum Verbinden von Geräten mit Azure AD. 



## <a name="managed-devices"></a>Verwaltete Geräte  

In einer Welt, in der Mobilität und die Cloud an erster Stelle stehen, ermöglicht Azure Active Directory das einmalige Anmelden an Geräten, Apps und Diensten an jedem Ort. Bei bestimmten Ressourcen in Ihrer Umgebung ist das Beschränken des Zugriffs auf die richtigen Benutzer möglicherweise nicht ausreichend. Zusätzlich zu den richtigen Benutzern möchten Sie möglicherweise auch vorschreiben, dass der Zugriff nur über ein verwaltetes Gerät ausgeführt werden kann.

Ein verwaltetes Gerät ist ein Gerät, das Ihre Standards bezüglich Sicherheit und Konformität erfüllt. Einfach ausgedrückt: Verwaltete Geräte sind Geräte, die *in irgendeiner Weise* von der Organisation kontrolliert werden. In Azure AD ist die Registrierung bei Azure AD die Voraussetzung für ein verwaltetes Gerät. Bei der Registrierung eines Geräts wird eine Identität für das Gerät in Form eines Geräteobjekts erstellt. Dieses Objekt wird von Azure verwendet, um Statusinformationen zu einem Gerät zu verfolgen. Als Azure AD-Administrator können Sie dieses Objekt bereits zum Ein-/Ausschalten (Aktivieren/Deaktivieren) des Status eines Geräts verwenden.
  
![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/32.png)

Zum Registrieren eines Geräts bei Azure AD gibt drei Optionen:

- **[Bei Azure AD registrierte Geräte](device-management-introduction.md#azure-ad-registered-devices)**: zum Registrieren eines persönlichen Geräts bei Azure AD.

- **[In Azure AD eingebundene Geräte](device-management-introduction.md#azure-ad-joined-devices)**: Diese Option ermöglicht es, ein Windows 10-Organisationsgerät bei Azure AD zu registrieren, das nicht in ein lokales Active Directory eingebunden ist. 

- **[In Azure AD eingebundene Hybridgeräte](device-management-introduction.md#hybrid-azure-ad-joined-devices)**: zum Registrieren eines Organisationsgeräts mit Windows 10 bei Azure AD, das in ein lokales Active Directory eingebunden ist.

Damit aus einem registrierten Gerät ein verwaltetes Gerät wird, muss das Gerät ein in Azure AD eingebundenes Hybridgerät oder ein als kompatibel markiertes Gerät sein.  

![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/47.png)


 
## <a name="require-hybrid-azure-ad-joined-devices"></a>In Azure AD eingebundene Hybridgeräte erforderlich

In der Richtlinie für bedingten Zugriff können Sie mit der Option **In Azure AD eingebundene Hybridgeräte erforderlich** angeben, dass auf die ausgewählten Cloud-Apps nur über ein verwaltetes Gerät zugegriffen werden kann. 

![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/10.png)

Diese Einstellung ist nur für Windows 10-Geräte relevant, die in ein lokales AD eingebunden sind. Sie können diese Geräte nur mit der Azure AD-Einbindung für Hybridgeräte bei Azure AD registrieren – ein [automatischer Vorgang](device-management-hybrid-azuread-joined-devices-setup.md) zum Abrufen einer Windows 10-Geräteregistrierung. 

![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/45.png)

Wodurch wird ein in Azure AD eingebundenes Hybridgerät zum verwalteten Gerät?  Bei Geräten, die in ein lokales AD eingebunden sind, wird angenommen, dass die Kontrolle über diese Geräte durch Verwaltungslösungen wie **System Center Configuration Manager (SCCM)** oder **Gruppenrichtlinie (GP)** erzwungen wird und die Geräte auf diese Weise verwaltet werden. Da Azure AD über keine Methode verfügt, anhand der festgestellt werden kann, ob eine dieser Methoden auf ein Gerät angewendet wurde, ist das Erforderlichmachen eines in Azure AD eingebundenen Hybridgeräts ein relativ schwacher Mechanismus zum Anfordern eines verwalteten Geräts. Es ist Ihre Aufgabe als Administrator zu ermitteln, ob die für Ihre lokalen domäneneingebundenen Geräte angewandten Methoden stark genug sind, um ein verwaltetes Gerät zu bilden, wenn ein solches Gerät auch ein in Azure AD eingebundenes Hybridgerät ist.


## <a name="require-device-to-be-marked-as-compliant"></a>Markieren des Geräts als kompatibel erforderlich

Die Option *Markieren des Geräts als kompatibel erforderlich* ist die sicherste Form, ein verwaltetes Gerät anzufordern.

![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/11.png)

Diese Option erfordert, dass ein Gerät bei Azure AD registriert und zusätzlich auch als kompatibel markiert wurde von:
         
- Intune 
- Ein durch ein Mobilgerät eines Drittanbieters verwaltetes System, das Windows 10-Geräte über Azure AD-Integration verwaltet. 
 
![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/46.png)



Bei einem als kompatibel markierten Gerät können Sie Folgendes voraussetzen: 

- Die mobilen Geräte, mit denen Ihre Mitarbeiter auf Unternehmensdaten zugreifen, sind verwaltet
- Mobile Apps, die Ihre Mitarbeiter verwenden, sind verwaltet
- Ihre Unternehmensdaten sind durch eine bessere Kontrolle der Zugriffs- und Freigabemöglichkeiten Ihrer Mitarbeiter geschützt
- Das Gerät und seine Apps sind kompatibel mit den Sicherheitsanforderungen des Unternehmens




## <a name="next-steps"></a>Nächste Schritte

Bevor Sie eine Richtlinie für den gerätebasierten bedingten Zugriff in Ihrer Umgebung konfigurieren, sollten Sie die [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) kennen.

