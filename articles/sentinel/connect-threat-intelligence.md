---
title: Sammeln von Threat Intelligence-Daten in der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Threat Intelligence-Daten mit Azure Sentinel verknüpfen.
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 56412543-5664-44c1-b026-2dbaf78a9a50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: bc8a644f99d9a84e1f2c177a87e2668ae9a57868
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400626"
---
# <a name="collect-data-from-threat-intelligence-providers"></a>Erfassen von Daten von Threat Intelligence-Anbietern 

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Nachdem Sie Ihre Daten in Azure Sentinel gestreamt haben, können Sie sie mit dem Threat Intelligence-Feed anreichern, den Sie in Ihrem gesamten Unternehmen verwenden. 

Azure Sentinel ermöglicht die Integration mit [Threat Intelligence-Anbietern](https://aka.ms/graphsecuritytips), um Ihre Warnungen und Regeln mit echter Threat Intelligence zu vergleichen, wenn Sie z.B. eine Warnung von einer bestimmten IP-Adresse erhalten, kann Ihre Threat Intelligence Provider-Integration Sie darüber informieren, ob diese IP-Adresse kürzlich als bösartig befunden wurde. 

Sie können Protokolle von Threat Intelligence-Anbietern mit einem einzigen Klick in Azure Sentinel streamen. Diese Verbindung ermöglicht es Ihnen, Indikatoren mit verschiedenen Arten von beobachtbare Elementen zu integrieren, z.B. IP-Adresse, Domäne, URL und Dateihash, um benutzerdefinierte Warnungsregeln in Azure Sentinel zu durchsuchen und zu erstellen.  
> [!NOTE]
> Sie können benutzerdefinierte Bedrohungsindikatoren in Azure Sentinel eingeben, um sie in Warnungsregeln, Dashboards und Bedrohungssuchszenarien (Hunting-Szenarien) zu verwenden. Dies geschieht, indem Sie sie mit der [Microsoft Graph-Sicherheitsentität tiIndicator](https://aka.ms/graphsecuritytiindicators) einbinden oder eine [in die Microsoft Graph-Sicherheit integrierte Bedrohungsanalyseplattform](https://aka.ms/graphsecuritytips) verwenden.

## <a name="prerequisites"></a>Voraussetzungen  

- Ein Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen 

- In Microsoft Intelligent Security Graph integrierte Threat Intelligence-Anwendung 

## <a name="connect-to-threat-intelligence"></a>Herstellen einer Verbindung mit Threat Intelligence 

1. Wenn Sie bereits einen Threat Intelligence-Anbieter (TIP) verwenden, sollten Sie zu Ihrer TIP-Anwendung navigieren und dieser die Berechtigung erteilen, Indikatoren an Microsoft zu senden, und den Dienst als Azure Sentinel spezifizieren.  

2. Wählen Sie in Azure Sentinel **Datenerfassung** aus, und klicken Sie dann auf die Kachel **Threat Intelligence**.

3. Klicken Sie auf **Verbinden**. 

4. Um in Log Analytics das relevante Schema für Threat Intelligence-Feeds zu verwenden, suchen Sie nach **ThreatIntelligenceIndicator**. 

 
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Ihren Anbieter für Informationen zu Bedrohungen mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln.

- Für den Einstieg in Azure Sentinel benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testabonnement](https://azure.microsoft.com/free/)registrieren.
- Hier erfahren Sie, wie Sie [Ihre Daten in Azure Sentinel integrieren](quickstart-onboard.md) und [Einblicke in Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
