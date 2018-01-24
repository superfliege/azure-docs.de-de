---
title: "Entscheidungen für die Bereitstellung von in Azure Stack integrierten Systemen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Entscheidungen bei der Bereitstellungsplanung für Azure Stack-Bereitstellungen mit mehreren Knoten treffen."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: d4aaf5af4fdb9ff5d185ef14333db4e204b9a955
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Planungsentscheidungen für in Azure Stack integrierte Systeme
Wenn Sie an einem in Azure Stack integrierten System interessiert sind, sollten Sie einige der [wichtigsten Aspekte bei Planung](azure-stack-deployment-planning.md) hinsichtlich der Azure Stack-Bereitstellung verstehen und dann bestimmen, wie das System in Ihr Rechenzentrum passt. Darüber hinaus müssen Sie sich genau überlegen, wie Sie Azure Stack in Ihre Hybrid Cloud-Umgebung integrieren möchten. Dieser Artikel bietet einen Überblick über diese wichtigen Entscheidungen, so z.B. zur Azure-Verbindung, zum Identitätsspeicher und Abrechnungsmodell.

Wenn Sie ein integriertes System erwerben möchten, führt Sie Ihr OEM-Hardwareanbieter (Originalgerätehersteller) dabei ausführlich durch einen Großteil des Planungsprozesses. Er führt auch die eigentliche Bereitstellung durch.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Wählen eines Verbindungsmodells für die Azure Stack-Bereitstellung
Sie können Azure Stack entweder bei vorhandener Verbindung mit dem Internet (und mit Azure) oder offline bereitstellen. Wenn Sie Azure Stack optimal nutzen möchten, einschließlich der Hybridszenarien zwischen Azure Stack und Azure, sollten Sie die Bereitstellung mit Azure-Verbindung wählen. Diese Auswahl legt fest, welche Optionen für Ihren Identitätsspeicher (Azure Active Directory oder Active Directory-Verbunddienste) und Ihr Abrechnungsmodell (nutzungs- oder kapazitätsbasiert) verfügbar sind, wie in der folgenden Abbildung und Tabelle zusammengefasst: 

![Azure Stack-Bereitstellungs- und Abrechnungsszenarien](media/azure-stack-deployment-decisions/azure-stack-scenarios.png)   
  
> [!IMPORTANT]
> Dies ist eine überaus wichtige Entscheidung! Die Wahl von Active Directory-Verbunddienste (AD FS) oder Azure Active Directory (Azure AD) ist eine Entscheidung, die Sie bei der Bereitstellung einmalig treffen müssen. Sie können diese Einstellung später nicht mehr ändern, ohne das gesamte System neu zu installieren.  


|Optionen|Mit Azure verbunden|Nicht mit Azure verbunden|
|-----|-----|-----|
|Azure AD|![Unterstützt](media/azure-stack-deployment-decisions/check.png)| |
|AD FS|![Unterstützt](media/azure-stack-deployment-decisions/check.png)|![Unterstützt](media/azure-stack-deployment-decisions/check.png)|
|Nutzungsbasierte Abrechnung|![Unterstützt](media/azure-stack-deployment-decisions/check.png)| |
|Kapazitätsbasierte Abrechnung|![Unterstützt](media/azure-stack-deployment-decisions/check.png)|![Unterstützt](media/azure-stack-deployment-decisions/check.png)|
|Updatepakete direkt in Azure Stack herunterladen|![Unterstützt](media/azure-stack-deployment-decisions/check.png)|  |

Nachdem Sie sich für das Azure-Verbindungsmodell entschieden haben, das für die Bereitstellung von Azure Stack verwendet werden soll, müssen zusätzliche, verbindungsabhängige Entscheidungen für den Identitätsspeicher und die Abrechnungsmethode getroffen werden. 

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Entscheidungen zur Azure Stack-Bereitstellung bei Verbindung mit Azure](azure-stack-connected-deployment.md).
- Erfahren Sie mehr über [Entscheidungen zur Azure Stack-Bereitstellung ohne Verbindung mit Azure](azure-stack-disconnected-deployment.md).
