---
title: ASC für IoT-Voraussetzungen (Vorschauversion) | Microsoft-Dokumentation
description: Hier finden Sie Details zu allen Voraussetzungen für den Einstieg in den ASC für IoT-Dienst.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 790cbcb7-1340-4cc1-9509-7b262e7c3181
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 213e6a95484b5f6953f8423474953125f8739015
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541495"
---
# <a name="asc-for-iot-prerequisites"></a>ASC für IoT-Voraussetzungen

> [!IMPORTANT]
> ASC für IoT befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dieser Artikel erläutert die verschiedenen Bausteine des ASC für IoT-Diensts, die Voraussetzungen sowie grundlegende Konzepte, um den Dienst zu verstehen. 

## <a name="minimum-requirements"></a>Mindestanforderungen

- IoT Hub-Standard-Tarif
    - Berechtigungen der RBAC-Rolle auf **Besitzer**-Ebene 
- [Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) 
- Azure Security Center (empfohlen)
    - Die Verwendung von Azure Security Center ist zwar nur eine Empfehlung und keine Anforderung, ohne Azure Security Center können Sie aber Ihre anderen Azure-Ressourcen in Azure IoT Hub nicht anzeigen. 
 
## <a name="working-with-asc-for-iot-service"></a>Arbeiten mit dem ASC für IoT-Dienst

ASC für IoT-Einblicke und -Berichte sind über Azure IoT Hub und Azure Security Center verfügbar. Um ASC für IoT auf Ihrer Instanz von Azure IoT Hub zu aktivieren, ist ein Konto mit Berechtigungen der Ebene **Besitzer** erforderlich. Nach der Aktivierung von ASC für IoT in Ihrer IoT Hub-Instanz werden ASC für IoT-Einblicke in Azure IoT Hub als das Feature **Sicherheit** und in Azure Security Center als **IoT** angezeigt. 

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche 

ASC für IoT wird derzeit für IoT Hub-Instanzen in den folgenden Azure-Regionen unterstützt:
  - USA (Mitte)
  - Nordeuropa
  - Asien, Südosten

## <a name="wheres-my-iot-hub"></a>Wo ist meine IoT Hub-Instanz?

Überprüfen Sie Ihren IoT Hub-Standort, um die Verfügbarkeit des Diensts zu sicherzustellen, bevor Sie beginnen. 

1. Öffnen Sie Ihre IoT Hub-Instanz. 
2. Klicken Sie auf **Overview**. 
3. Überprüfen Sie, ob der aufgeführte Standort mit einer der [unterstützten Dienstregionen](#supported-service-regions) übereinstimmt. 


## <a name="supported-platforms-for-agents"></a>Unterstützte Plattformen für Agents 

ASC für IoT-Agents unterstützt eine wachsende Anzahl von Geräten und Plattformen. In der [Liste der unterstützten Plattformen](select-deploy-agent.md) können Sie Ihre vorhandene oder geplante Gerätebibliothek überprüfen.  

## <a name="next-steps"></a>Nächste Schritte
- [Übersicht](overview.md)
- [Aktivieren des Diensts](quickstart-onboard-iot-hub.md)
- [ASC für IoT – Häufig gestellte Fragen](resources-frequently-asked-questions.md)
- [Grundlegendes zu ASC für IoT-Warnungen](concept-security-alerts.md)
