---
title: Vorschreiben der Verwendung verwalteter Geräte für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs von Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie gerätebasierte bedingte Zugriffsrichtlinien von Azure Active Directory (Azure AD) konfigurieren, die für den Zugriff auf Cloud-Apps die Verwendung verwalteter Geräte erforderlich machen.
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
ms.date: 06/14/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 066d25e8953a2be4bd64cdd1af79b7f2a25dd5f9
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37034789"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Vorschreiben der Verwendung verwalteter Geräte für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs

In einer Welt, in der Mobilität und die Cloud an erster Stelle stehen, ermöglicht Azure Active Directory (Azure AD) das einmalige Anmelden bei Apps und Diensten an jedem Ort. Autorisierte Benutzer können über ein breites Spektrum von Geräten (darunter mobile und persönliche Geräte) auf Ihre Cloud-Apps zugreifen. Allerdings gibt es in vielen Umgebungen mindestens einige wenige Apps, auf die der Zugriff nur über Geräte möglich sein sollte, die Ihre Sicherheits- und Compliancestandards erfüllen. Diese Geräte werden auch als verwaltete Geräte bezeichnet. 

In diesem Artikel erfahren Sie, wie Sie Richtlinien für den bedingten Zugriff konfigurieren, die die Verwendung verwalteter Geräte für den Zugriff auf bestimmte Cloud-Apps in Ihrer Umgebung erforderlich machen. 


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie für Cloud-Apps die Verwendung verwalteter Geräte vorschreiben, wird der **bedingte Zugriff von Azure AD** mit der **Geräteverwaltung von Azure AD** verknüpft. Wenn Sie mit einem dieser Bereiche noch nicht vertraut sind, sollten Sie zunächst die folgenden Themen lesen:

- **[Bedingter Zugriff in Azure Active Directory:](active-directory-conditional-access-azure-portal.md)** Dieser Artikel enthält eine konzeptionelle Übersicht über den bedingten Zugriff und die entsprechende Terminologie.

- **[Einführung in die Geräteverwaltung in Azure Active Directory:](device-management-introduction.md)** Dieser Artikel enthält eine Übersicht über die verschiedenen Optionen, mit denen Sie Geräte unter die Kontrolle der Organisation bringen können. 


## <a name="scenario-description"></a>Beschreibung des Szenarios

Es ist nicht einfach, ein ausgewogenes Gleichgewicht zwischen Sicherheit und Produktivität zu finden. Die zunehmende Verbreitung unterstützter Geräte für den Zugriff auf Ihre Cloudressourcen kommt der Produktivität Ihrer Benutzer zugute. Auf der anderen Seite gibt es wahrscheinlich Ressourcen in Ihrer Umgebung, die nicht für Geräte mit unbekannter Schutzebene zugänglich sein sollen. Für die betroffenen Ressourcen sollten Sie daher sicherstellen, dass Benutzer nur unter Verwendung eines verwalteten Geräts auf sie zugreifen können. 

Mit dem bedingten Zugriff in Azure AD können Sie diese Anforderung mit einer einzigen Richtlinie erfüllen, die den Zugriff gewährt:

- Für ausgewählte Cloud-Apps

- Für ausgewählte Benutzer und Gruppen

- Vorschreiben der Verwendung eines verwalteten Geräts


## <a name="managed-devices"></a>Verwaltete Geräte  

Einfach ausgedrückt: Verwaltete Geräte sind Geräte, die *in irgendeiner Weise* von der Organisation kontrolliert werden. In Azure AD ist die Registrierung bei Azure AD die Voraussetzung für ein verwaltetes Gerät. Bei der Registrierung eines Geräts wird eine Identität für das Gerät in Form eines Geräteobjekts erstellt. Dieses Objekt wird von Azure verwendet, um Statusinformationen zu einem Gerät zu verfolgen. Als Azure AD-Administrator können Sie dieses Objekt bereits zum Ein-/Ausschalten (Aktivieren/Deaktivieren) des Status eines Geräts verwenden.
  
![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/32.png)

Zum Registrieren eines Geräts bei Azure AD gibt drei Optionen:

- **[Bei Azure AD registrierte Geräte](device-management-introduction.md#azure-ad-registered-devices)**: zum Registrieren eines persönlichen Geräts bei Azure AD.

- **[In Azure AD eingebundene Geräte](device-management-introduction.md#azure-ad-joined-devices)**: Diese Option ermöglicht es, ein Windows 10-Organisationsgerät bei Azure AD zu registrieren, das nicht in ein lokales Active Directory eingebunden ist. 

- **[In Azure AD eingebundene Hybridgeräte](device-management-introduction.md#hybrid-azure-ad-joined-devices)**: zum Registrieren eines Organisationsgeräts mit Windows 10 bei Azure AD, das in ein lokales Active Directory eingebunden ist.

Damit aus einem registrierten Gerät ein verwaltetes Gerät wird, muss das Gerät entweder ein **in Azure AD eingebundenes Hybridgerät** oder ein **als kompatibel markiertes Gerät** sein.  

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
- Ein Drittanbietersystem für die mobile Geräteverwaltung (Mobile Device Management, MDM), das Windows 10-Geräte über Azure AD-Integration verwaltet. MDM-Systeme von Drittanbietern für andere Arten von Gerätebetriebssystemen als Windows 10 werden nicht unterstützt.
 
![Gerätebasierte Bedingungen](./media/active-directory-conditional-access-policy-connected-applications/46.png)



Bei einem als kompatibel markierten Gerät können Sie Folgendes voraussetzen: 

- Die mobilen Geräte, mit denen Ihre Mitarbeiter auf Unternehmensdaten zugreifen, sind verwaltet
- Mobile Apps, die Ihre Mitarbeiter verwenden, sind verwaltet
- Ihre Unternehmensdaten sind durch eine bessere Kontrolle der Zugriffs- und Freigabemöglichkeiten Ihrer Mitarbeiter geschützt
- Das Gerät und seine Apps sind kompatibel mit den Sicherheitsanforderungen des Unternehmens




## <a name="next-steps"></a>Nächste Schritte

Bevor Sie eine Richtlinie für den gerätebasierten bedingten Zugriff in Ihrer Umgebung konfigurieren, sollten Sie die [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) kennen.

