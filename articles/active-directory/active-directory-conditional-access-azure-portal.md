---
title: Was ist der bedingte Zugriff in Azure Active Directory? | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe des bedingten Zugriffs in Azure Active Directory die automatisierte Zugriffswahl implementieren, die nicht nur darauf basiert, wer auf eine Ressource zugreifen möchte, sondern auch darauf, wie auf die Ressource zugegriffen wird.
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
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 7483cda4b2170e18549ebfbe0c8d30f8d3016013
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39531976"
---
# <a name="what-is-conditional-access-in-azure-active-directory"></a>Was ist der bedingte Zugriff in Azure Active Directory?

Sicherheit ist ein wichtiges Anliegen von Organisationen, die die Cloud nutzen. Wichtige Aspekte der Cloudsicherheit, wenn es um die Verwaltung von Cloudressourcen geht, sind Identität und Zugriff. In einer von Mobilgeräten und der Cloud geprägten Welt können Benutzer mithilfe einer Vielzahl von Geräten und Apps von überall aus auf Ressourcen in Ihrer Organisation zugreifen. Daher reicht eine Konzentration darauf, wer auf Ihre Ressourcen Zugriff hat, nicht mehr aus. Um das Gleichgewicht zwischen Sicherheit und Produktivität zu bewahren, müssen Sie bei der Wahl der Zugriffssteuerung auch berücksichtigen, auf welche Weise auf eine Ressource zugegriffen wird. Mit dem bedingten Zugriff von Azure AD können Sie diese Anforderung erfüllen. Der bedingte Zugriff ist eine Funktion von Azure Active Directory. Mit dem bedingten Zugriff können Sie basierend auf bestimmten Bedingungen automatisierte Entscheidungen hinsichtlich der Zugriffssteuerung für den Zugriff auf Ihre Cloud-Apps implementieren. 

![Kontrolle](./media/active-directory-conditional-access-azure-portal/81.png)

Dieser Artikel bietet eine konzeptionelle Übersicht über den bedingten Zugriff in Azure AD.



## <a name="common-scenarios"></a>Häufige Szenarios

In einer Welt, in der Mobilität und die Cloud an erster Stelle stehen, ermöglicht Azure Active Directory das einmalige Anmelden an Geräten, Apps und Diensten an jedem Ort. Aufgrund der steigenden Zahl von Geräten (z.B. durch das BYOD-Konzept [Bring Your Own Device]), der zunehmenden Arbeit außerhalb von Unternehmensnetzwerken und der vermehrten Nutzung von SaaS-Apps von Drittanbietern stehen Sie vor zwei gegensätzlichen Zielsetzungen:

- Schaffen von Bedingungen für Benutzer, unter denen sie an jedem Ort und zu jeder Zeit produktiv sein können
- Lückenloses Schützen der Assets eines Unternehmens

Mithilfe von Richtlinien für bedingten Zugriff können Sie die passende Zugriffssteuerung unter den erforderlichen Bedingungen anwenden. Der bedingte Zugriff von Azure AD bietet zusätzliche Sicherheit, wenn er benötigt wird, ohne die Benutzer andernfalls zu behindern. 

Im Folgenden finden Sie einige allgemeine Überlegungen zum Zugriff, bei denen der bedingte Zugriff Ihnen helfen kann:



- **[Anmelderisiko:](conditional-access/conditions.md#sign-in-risk)** Azure AD Identity Protection erkennt Anmelderisiken. Wie schränken Sie den Zugriff ein, wenn ein erkanntes Anmelderisiko auf einen böswilligen Benutzer hindeutet? Was geschieht, wenn Sie einen fundierteren Beweis dafür benötigen, dass eine Anmeldung durch einen berechtigten Benutzer durchgeführt wurde? Was geschieht, wenn Ihre Zweifel so stark sind, dass Sie für bestimmte Benutzer den Zugriff auf eine App blockieren möchten? Durch Konfiguration 

- **[Netzwerkadresse:](conditional-access/location-condition.md)** Der Zugriff auf Azure AD ist von überall aus möglich. Was geschieht, wenn ein Zugriffsversuch von einer Netzwerkadresse ausgeführt wird, die nicht durch Ihre IT-Abteilung kontrolliert wird? Die Kombination aus Benutzername und Kennwort ist vermutlich ausreichend als Nachweis der Identität für den Zugriff auf Ihre Ressourcen aus Ihrem Unternehmensnetzwerk. Was geschieht aber, wenn Sie einen stärkeren Nachweis der Identität für den Zugriff aus anderen, unerwarteten Ländern oder Regionen der Welt benötigen? Was geschieht, wenn Sie sogar alle Zugriffsversuche von bestimmten Standorten blockieren möchten?  

- **[Geräteverwaltung:](conditional-access/conditions.md#device-platforms)** In Azure AD können Benutzer über eine breite Palette von Geräten, einschließlich mobiler und persönlicher Geräte, auf Cloud-Apps zugreifen. Was geschieht, wenn Sie festlegen möchten, dass ein Zugriff nur noch über Geräte möglich sein soll, die von Ihrer IT-Abteilung verwaltet werden? Was geschieht, wenn Sie darüber hinaus den Zugriff auf Cloud-Apps in Ihrer Umgebung für bestimmte Gerätetypen blockieren möchten? 

- **[Clientanwendung](conditional-access/conditions.md#client-apps)**: Heute können Sie mit verschiedenen Arten von Apps auf viele Cloud-Apps zugreifen, z.B. mit webbasierten Apps, mobilen Apps oder Desktop-Apps. Was geschieht, wenn ein Zugriffsversuch mithilfe eines Client-App-Typs durchgeführt wird, der bekannte Probleme verursacht? Was geschieht, wenn Sie für bestimmte App-Typen ein Gerät vorschreiben möchten, das von Ihrer IT-Abteilung verwaltet wird? 

Diese Fragen und die zugehörigen Antworten stellen allgemeinen Szenarien für den bedingten Zugriff von Azure AD dar. Der bedingte Zugriff ist eine Funktion von Azure Active Directory, die es Ihnen ermöglicht, Zugriffsszenarien mithilfe eines richtlinienbasierten Ansatzes zu behandeln.


## <a name="conditional-access-policies"></a>Richtlinien für bedingten Zugriff

Eine Richtlinie für bedingten Zugriff ist die Definition eines Zugriffsszenarios mithilfe des folgenden Musters:

![Kontrolle](./media/active-directory-conditional-access-azure-portal/10.png)

**Then do this** (Dann diese Aktion ausführen) definiert die Antwort Ihrer Richtlinie. Es ist wichtig zu beachten, dass das Ziel einer Richtlinie für bedingten Zugriff nicht das Gewähren des Zugriffs auf eine Cloud-App ist. In Azure AD erfolgt das Gewähren des Zugriffs auf Cloud-Apps über Benutzerzuweisungen. Mit einer Richtlinie für bedingten Zugriff steuern Sie, wie autorisierte Benutzer (Benutzer, denen der Zugriff auf einen Cloud-App gewährt wurde) unter bestimmten Bedingungen auf Cloud-Apps zugreifen können. In Ihrer Antwort erzwingen Sie zusätzliche Anforderungen wie z.B. eine mehrstufige Authentifizierung, ein verwaltetes Gerät usw. Im Kontext des bedingten Zugriffs von Azure AD heißen diese Anforderungen, die Ihre Richtlinie erzwingt, „Zugriffssteuerungen“. In der am stärksten einschränkenden Form kann Ihre Richtlinie den Zugriff blockieren. Weitere Informationen finden Sie unter [Zugriffssteuerungen beim bedingten Zugriff von Azure Active Directory](conditional-access/controls.md).
     

**When this happens** (Wenn dies geschieht) definiert den Grund für das Auslösen der Richtlinie. Dieser Grund wird durch eine Gruppe von Bedingungen gekennzeichnet, die erfüllt wurden. Beim bedingten Zugriff von Azure AD spielen zwei Zuordnungsbedingungen eine besondere Rolle:

- **[Benutzer:](conditional-access/conditions.md#users-and-groups)** Dies ist der Benutzer, der einen Zugriffsversuch durchführt (**Wer**). 

- **[Cloud-Apps:](conditional-access/conditions.md#cloud-apps)** Dies sind die Ziele des Zugriffsversuchs (**Was**).    

Beide Bedingungen müssen in einer Richtlinie für bedingten Zugriff angegeben werden. Zusätzlich zu den beiden obligatorischen Bedingungen können Sie auch zusätzliche Bedingungen einfügen, die beschreiben, wie der Zugriffsversuch erfolgt. Gängige Beispiele sind mobile Geräte oder Standorte außerhalb des Unternehmensnetzwerks. Weitere Informationen finden Sie unter [Bedingungen beim bedingten Zugriff von Azure Active Directory](conditional-access/conditions.md).   

Die Kombination aus Bedingungen und Ihren Zugriffssteuerungen ergibt eine Richtlinie für bedingten Zugriff. 

![Kontrolle](./media/active-directory-conditional-access-azure-portal/51.png)

Mit dem bedingten Zugriff von Azure AD können Sie den Zugriff von autorisierten Benutzern auf Ihre Cloud-Apps steuern. Ziel einer Richtlinie für bedingten Zugriff ist es, bei einem Zugriffsversuch auf eine Cloud-App zusätzliche Zugriffssteuerungen zu erzwingen, die durch die Methodik eines Zugriffsversuchs ausgelöst werden.

Ein Vorteil der Verwendung eines richtlinienbasierten Ansatzes zum Schutz des Zugriffs auf Ihre Cloud-Apps ist, dass Sie die Richtlinienanforderungen für Ihre Umgebung mit der in diesem Artikel beschriebenen Struktur beginnen können, ohne sich über die technische Implementierung Gedanken machen zu müssen. 


## <a name="license-requirements-for-using-conditional-access"></a>Lizenzanforderungen für die Verwendung des bedingten Zugriffs

Für die Verwendung des bedingten Zugriffs ist eine Azure AD Premium-Lizenz erforderlich. Um die richtige Lizenz für Ihre Anforderungen zu ermitteln, lesen Sie [Vergleich: Allgemein verfügbare Features der Editionen Free, Basic und Premium](https://azure.microsoft.com/pricing/details/active-directory/).


## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie mehr erfahren möchten:
    - Bedingungen: [Bedingungen beim bedingten Zugriff in Azure Active Directory](conditional-access/conditions.md)

    - Zugriffssteuerungen: [Zugriffssteuerungen beim bedingten Zugriff von Azure Active Directory](conditional-access/controls.md)

- Wenn Sie Erfahrungen beim Konfigurieren von Richtlinien für bedingten Zugriff sammeln möchten, lesen Sie die Informationen unter [Quickstart: Require MFA for specific apps with Azure Active Directory conditional access](conditional-access/app-based-mfa.md) (Schnellstart: Vorschreiben der Verwendung der MFA für bestimmte Apps über den bedingten Zugriff von Azure Active Directory).

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](conditional-access/best-practices.md) nach. 

- Einen schrittweisen Bereitstellungsplan mit empfohlenen Richtlinien finden Sie bei Bedarf unter [Bereitstellungsplan für bedingten Zugriff](http://aka.ms/conditionalaccessdeploymentplan).
