---
title: Einführung in die auf virtuellen Data Science-Computern basierenden Teamumgebungen – Azure | Microsoft-Dokumentation
description: Muster für die Bereitstellung eines virtuellen Data Science-Computers als Umgebung für Unternehmensteams.
keywords: Deep Learning, KI, Data Science-Tools, virtueller Data Science-Computer, räumliche Analysen, Data Science-Teamprozess
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: d2aa3c8582227363e9365f213cdf351b9f4a81af
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830536"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Auf virtuellen Data Science-Computern basierende Teamanalyse und KI-Umgebung 
Der [virtuelle Data Science-Computer](overview.md) (Data Science Virtual Machine, DSVM) bietet eine umfangreiche Umgebung in der Azure-Cloud mit vorgefertigter Software für KI und Datenanalyse. Bisher wurde der DSVM als einzelner Analyse-Desktop verwendet, und einzelne Datenanalysten erzielen durch diese gemeinsame Nutzung ihrer vorgefertigten Analyseumgebung einen Produktivitätsgewinn. Bei der Planung von Analyseumgebungen für Data Scientists und KI-Entwickler durch große Analyseteams ist eines des Leitmotive eine gemeinsam genutzte Analysenentwicklungs- und Experimentinfrastruktur, die entsprechend den IT-Richtlinien des Unternehmens verwaltet wird und außerdem die Zusammenarbeit und Konsistenz über alle Data Science-/Analyseteams hinweg vereinfacht. Eine gemeinsam genutzte Infrastruktur ermöglicht der IT-Abteilung außerdem eine bessere Nutzung der Analyseumgebung. Die teambasierte Data Science-/Analyseinfrastruktur wird von einigen Organisationen auch als „Analyse-Sandbox“ bezeichnet, mit dessen Hilfe die Data Scientists schnell Daten verstehen, Experimente ausführen, Hypothesen überprüfen und Vorhersagemodelle auf sichere Weise und ohne Beeinträchtigung der Produktionsumgebung erstellen können, während sie über Zugriff auf verschiedene Datenressourcen verfügen. 

Da der DSVM auf der Azure-Infrastrukturebene ausgeführt wird, kann er von den IT-Administratoren problemlos so konfiguriert werden, dass er den IT-Richtlinien des Unternehmens entspricht und umfassende Flexibilität bei der Implementierung verschiedener Freigabearchitekturen mit kontrolliertem Zugriff auf Unternehmensdatenressourcen bietet. 

In diesem Abschnitt werden einige Muster und Richtlinien behandelt, die zum Bereitstellen des DSVM als teambasierte Data Science-Infrastruktur verwendet werden können.  Die Bausteine für diese Muster werden direkt in Azure IaaS (Infrastructure-as-a-Service) genutzt und lassen sich daher auf alle virtuellen Azure-Computer anwenden. Der spezifische Schwerpunkt dieser Artikelreihe liegt auf der Anwendung dieser standardmäßigen Azure-Infrastrukturfunktionen auf den virtuellen Data Science-Computer. 

Zu den wichtigsten Bausteinen einer Teamanalyseumgebung für Unternehmen gehören:

* [Automatisch skalierter Pool von virtuellen Data Science-Computern](dsvm-pools.md)
* [Gemeinsame Identität und Zugriff auf den Arbeitsbereich über jeden DSVM im Pool](dsvm-common-identity.md)
* [Sicherer Zugriff auf Datenquellen](dsvm-secure-access-keys.md)


In dieser Artikelreihe werden Anleitungen und Hinweise zu jedem dieser oben genannten Aspekte bereitgestellt. Natürlich gibt es einige zusätzliche Überlegungen und Anforderungen bei der Bereitstellung von DSVM in großen Unternehmenskonfigurationen, die in dieser Artikelserie noch nicht direkt behandelt werden. Hier sind einige der anderen Überlegungen und Hinweise auf die allgemeine Azure-Dokumentation, die bei der Implementierung auf den DSVM-Instanzen in Ihrem Unternehmen verwendet werden kann. 

* [Netzwerksicherheit](https://docs.microsoft.com/azure/security/azure-network-security)
* [Überwachung](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) und [Verwaltung](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Protokollierung und Überwachung](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Rollenbasierte Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [Richtlinieneinstellung und -erzwingung](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)
* [Antischadsoftware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Verschlüsselung](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Datenermittlung und Governance](https://docs.microsoft.com/azure/data-catalog/)

Das [Azure-Architekturzentrum](https://docs.microsoft.com/en-us/azure/architecture/) ist auch eine großartige Ressource, die detaillierte End-to-End-Architekturen und Muster für den Aufbau und die Verwaltung Ihrer cloudbasierten Analyseinfrastruktur bietet. 
