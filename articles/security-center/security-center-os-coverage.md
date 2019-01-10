---
title: Von Azure Security Center unterstützte Features und Plattformen | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Liste der vom Azure Security Center unterstützten Features und Plattformen.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: rkarlin
ms.openlocfilehash: 2dcc72e0e3b9caef9ab01d9f754671cb0365a358
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608833"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Von Azure Security Center unterstützte Features und Plattformen

Die Überwachung des Sicherheitsstatus und entsprechende Empfehlungen sind für virtuelle Computer (VMs), die mit dem klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, und für Computer verfügbar.

> [!NOTE]
> Erfahren Sie mehr über das [klassische und das Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) für Azure-Ressourcen.
>
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Plattformen mit Unterstützung für den Datensammlungs-Agent 

In diesem Abschnitt werden die Plattformen aufgeführt, auf denen der Azure Security Center-Agent ausgeführt werden kann und von denen er Daten sammeln kann.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Unterstützte Plattformen für Windows-Computer und -VMs
Die folgenden Windows-Betriebssysteme werden unterstützt:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

> [!NOTE]
> Die Integration in Windows Defender ATP unterstützt nur Windows Server 2012 R2 und Windows Server 2016.
>
>

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Unterstützte Plattformen für Linux-Computer und -VMs
Die folgenden Linux-Betriebssysteme werden unterstützt:

* Ubuntu-Versionen 12.04 LTS, 14.04 LTS und 16.04 LTS
* Debian-Versionen 6, 7, 8 und 9
* CentOS-Versionen 5, 6 und 7
* Red Hat Enterprise Linux-Versionen (RHEL) 5, 6 und 7
* SUSE Linux Enterprise Server-Versionen (SLES) 11 und 12
* Oracle Linux-Versionen 5, 6 und 7
* Amazon Linux 2012.09 bis 2017
* OpenSSL 1.1.0 wird nur auf x86_64-Plattformen (64 Bit) unterstützt.

## <a name="vms-and-cloud-services"></a>VMs und Clouddienste
VMs, die in einem Clouddienst ausgeführt werden, werden ebenfalls unterstützt. Es werden nur Clouddienst-Webrollen und -Workerrollen überwacht, die in Produktionsslots ausgeführt werden. Weitere Informationen zu Clouddiensten finden Sie unter [Übersicht zu Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Unterstützte IaaS-Features

> [!div class="mx-tableFixed"]
> 

|Server|Windows||Linux||
|----|----|----|----|----|
|Environment|Azure|Nicht-Azure|Azure|Nicht-Azure|
|Warnungen der VMBA-Bedrohungserkennung|✔|✔|✔ (für unterstützte Versionen)|✔|
|Warnungen der netzwerkbasierten Bedrohungserkennung|✔|X|✔|X|
|Windows Defender ATP-Integration|✔ (für unterstützte Versionen)|✔|X|X|
|Fehlende Patches|✔|✔|✔|✔|
|Sicherheitskonfigurationen|✔|✔|✔|✔|
|Antischadsoftware|✔|✔|X|X|
|JIT-VM-Zugriff|✔|X|✔|X|
|Adaptive Anwendungssteuerungen|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Datenträgerverschlüsselung|✔|X|✔|X|
|Drittanbieterbereitstellung|✔|X|✔|X|
|NSGs|✔|X|✔|X|
|Erkennung von Fileless-Bedrohungen|✔|✔|X|X|
|Netzwerkübersicht|✔|X|✔|X|
|Adaptive Netzwerksteuerungen|✔|X|✔|X|

\* Diese Features werden derzeit in der öffentlichen Vorschau unterstützt.


## <a name="supported-paas-features"></a>Unterstützte PaaS-Features 


|Dienst|Empfehlungen|Bedrohungserkennung|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Azure Blob-Speicherkonten*|✔| ✔|
|App Services|✔| ✔|
|Cloud Services|✔| X|
|VNETs|✔| Nicht verfügbar|
|Subnetze|✔| Nicht verfügbar|
|NICs|✔| ✔|
|NSGs|✔| Nicht verfügbar|
|Abonnement|✔| ✔|

\* Diese Features werden derzeit in der öffentlichen Vorschau unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

- Hier erfahren Sie, wie Sie [die Entwurfsaspekte in Bezug auf die Einführung von Azure Security Center planen und verstehen](security-center-planning-and-operations-guide.md).
- Hier finden Sie Informationen zur [Verhaltensanalyse bei virtuellen Computern und zur Absturzabbild-Speicheranalyse in Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Lesen Sie [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md).
- Lesen Sie [Blogbeiträge zur Sicherheit und Compliance von Azure](https://blogs.msdn.com/b/azuresecurity/).
