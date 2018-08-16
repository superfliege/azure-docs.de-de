---
title: 'Gewusst wie: Konfigurieren von Richtlinien für bedingten Zugriff in Azure Active Directory für Zugriffsversuche von nicht vertrauenswürdigen Netzwerken | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie eine Richtlinie für bedingten Zugriff in Azure Active Directory (Azure AD) für Zugriffsversuche von nicht vertrauenswürdigen Netzwerken konfigurieren.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: protection
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 2dea5686add93d93f35e82445f411035a2451e33
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525976"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Gewusst wie: Konfigurieren von Richtlinien für bedingten Zugriff für Zugriffsversuche von nicht vertrauenswürdigen Netzwerken   

In einer Welt, in der Mobilität und die Cloud an erster Stelle stehen, ermöglicht Azure Active Directory (Azure AD) das einmalige Anmelden an Geräten, Apps und Diensten an jedem Ort. Dadurch können Benutzer nicht nur über das Netzwerk Ihres Unternehmens, sondern auch von jeder nicht vertrauenswürdigen Internetadresse aus auf Ihre Cloud-Apps zugreifen. Mit dem [bedingten Zugriff von Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md) können Sie den Zugriff von autorisierten Benutzern auf Ihre Cloud-Apps steuern. Eine häufige Anforderung in diesem Zusammenhang ist die Steuerung der Zugriffsversuche von nicht vertrauenswürdigen Netzwerken. Dieser Artikel liefert Ihnen die Informationen, die Sie benötigen, um eine Richtlinie für den bedingten Zugriff zu konfigurieren, die diese Anforderung erfüllt. 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind: 

- Die grundlegenden Konzepte des bedingten Zugriffs in Azure AD 
- Konfigurieren von Richtlinien für bedingten Zugriff im Azure-Portal

Siehe:

- Eine Übersicht über den bedingten Zugriff finden Sie unter [Was ist bedingter Zugriff in Azure Active Directory?](active-directory-conditional-access-azure-portal.md) 

- Wenn Sie Erfahrungen beim Konfigurieren von Richtlinien für bedingten Zugriff sammeln möchten, lesen Sie den [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](conditional-access/app-based-mfa.md). 


## <a name="scenario-description"></a>Beschreibung des Szenarios

Für ein besseres Gleichgewicht zwischen Sicherheit und Produktivität kann es ausreichen, wenn für die Authentifizierung Ihres Benutzers ein Kennwort erforderlich ist. Wird jedoch ein Zugriffsversuch von einer nicht vertrauenswürdigen Netzwerkadresse aus unternommen, besteht ein erhöhtes Risiko, dass die Anmeldung nicht von einem legitimen Benutzer durchgeführt wird. Um dieses Risiko zu vermeiden, können Sie Zugriffsversuche von nicht vertrauenswürdigen Netzwerken blockieren. Alternativ können Sie auch eine mehrstufige Authentifizierung (MFA) verlangen, um sicher zu sein, dass ein Versuch vom legitimen Besitzer des Kontos unternommen wurde. 

Mit dem bedingten Zugriff in Azure AD können Sie diese Anforderung mit einer einzigen Richtlinie erfüllen, die den Zugriff gewährt: 

- Für ausgewählte Cloud-Apps

- Für ausgewählte Benutzer und Gruppen  

- Mit erforderlicher mehrstufiger Authentifizierung 

- Wenn ein Zugriffsversuch unternommen wird von: 

    - Einer nicht als vertrauenswürdig eingestuften Adresse


## <a name="considerations"></a>Überlegungen

Die Herausforderung dieses Szenarios besteht darin, *einen Zugriffsversuch von einer nicht vertrauenswürdigen Adresse* in eine Bedingung für den bedingten Zugriff umzuwandeln. In einer Richtlinie für bedingten Zugriff können Sie die [Standortbedingung so konfigurieren](conditional-access/location-condition.md), dass sie Szenarien adressiert, die sich auf Netzwerkadressen beziehen. Die Standortbedingung ermöglicht es Ihnen, benannte Standorte auszuwählen, die logische Gruppierungen von IP-Adressbereichen, Ländern und Regionen darstellen.  

Typischerweise besitzt Ihre Organisation einen oder mehrere Adressbereiche, z.B. 199.30.16.0 bis 199.30.16.24.
Sie können einen benannten Speicherort wie folgt konfigurieren:

- Festlegen dieses Bereichs (199.30.16.0/24) 

- Zuweisen eines beschreibenden Namens wie **Unternehmensnetzwerk** 


Anstatt zu versuchen, alle Standorte zu definieren, die nicht vertrauenswürdig sind, können Sie:

- Include 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-untrusted-networks/02.png)

- Alle vertrauenswürdigen Standorte ausschließen 

    ![Bedingter Zugriff](./media/active-directory-conditional-access-untrusted-networks/01.png)



## <a name="implementation"></a>Implementierung

Mit dem in diesem Artikel beschriebenen Ansatz können Sie nun eine Richtlinie für den bedingten Zugriff für nicht vertrauenswürdige Standorte konfigurieren. Sie sollten Ihre Richtlinie immer testen, bevor Sie sie in der Produktionsumgebung einsetzen, um eine erwartungsgemäße Funktionsweise sicherzustellen. Idealerweise sollten Sie Ihre ersten Tests möglichst in einem Testmandanten durchführen. Weitere Informationen finden Sie unter [Wie stellen Sie eine neue Richtlinie bereit?](conditional-access/best-practices.md#how-should-you-deploy-a-new-policy) 



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum bedingten Zugriff finden Sie unter [Was ist bedingter Zugriff in Azure Active Directory?](active-directory-conditional-access-azure-portal.md)