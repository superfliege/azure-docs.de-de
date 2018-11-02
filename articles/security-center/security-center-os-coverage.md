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
ms.date: 10/10/2018
ms.author: rkarlin
ms.openlocfilehash: 279818e6b43e53206deb9e33591f75ef381a8962
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319981"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Von Azure Security Center unterstützte Features und Plattformen

Die Überwachung des Sicherheitsstatus und entsprechende Empfehlungen sind für virtuelle Computer (VMs), die mit dem klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, und für Computer verfügbar.

> [!NOTE]
> Erfahren Sie mehr über das [klassische und das Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) für Azure-Ressourcen.
>
>

## <a name="supported-platforms"></a>Unterstützte Plattformen 

In diesem Abschnitt werden die Plattformen aufgeführt, auf denen der Azure Security Center-Agent ausgeführt werden kann und von denen er Daten sammeln kann.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Unterstützte Plattformen für Windows-Computer und -VMs
Unterstützte Windows-Betriebssysteme:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


### <a name="supported-platforms-for-linux-computers-and-vms"></a>Unterstützte Plattformen für Linux-Computer und -VMs
Unterstützte Linux-Betriebssysteme:

* Ubuntu-Versionen 12.04 LTS, 14.04 LTS, 16.04 LTS
* Debian-Versionen 6, 7, 8, 9
* CentOS-Versionen 5, 6, 7
* Red Hat Enterprise Linux-Versionen (RHEL) 5, 6, 7
* SUSE Linux Enterprise Server-Versionen (SLES) 11, 12
* Oracle Linux-Versionen 5, 6, 7
* Amazon Linux 2012.09 bis 2017
* OpenSSL 1.1.0 wird nur auf x86_64-Plattformen (64-Bit) unterstützt.

> [!NOTE]
> Verhaltensanalysen für virtuelle Computer sind für Linux-Betriebssysteme noch nicht verfügbar.
>
>

## <a name="vms-and-cloud-services"></a>VMs und Clouddienste
VMs, die in einem Clouddienst ausgeführt werden, werden ebenfalls unterstützt. Es werden nur Clouddienst-Webrollen und -Workerrollen überwacht, die in Produktionsslots ausgeführt werden. Weitere Informationen zum Clouddienst finden Sie unter [Sollte ich mich für Clouddienste oder für eine andere Lösung entscheiden?](../cloud-services/cloud-services-choose-me.md).


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
|Adaptive Anwendungssteuerungen|✔ (nur Azure)|X|X|X|
|FIM|✔|✔|✔|✔|
|Datenträgerverschlüsselung|✔|X|✔|X|
|Bereitstellung durch Drittanbieter|✔|X|✔|X|
|NSGs|✔|X|✔|X|
|Filess V1|✔|✔|X|X|
|Netzwerkübersicht|✔|X|✔|X|
|Adaptives Erhöhen des Netzwerkschutzes|✔|X|✔|X|

* Diese Features werden derzeit in der öffentlichen Vorschau unterstützt.


## <a name="supported-paas-features"></a>Unterstützte PaaS-Features


|Dienst|Empfehlungen|Bedrohungserkennung|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Blob-Speicherkonten*|✔| ✔|
|App Services|✔| ✔|
|Clouddienste|✔| X|
|Redis-Cache|✔| X|
|Service Fabric|✔| X|
|Azure Automation|✔| X|
|Data Lake |✔| X|
|Schlüsseltresor|✔| X|
|Service Bus|✔| X|
|Stream Analytics|✔| X|
|Batch|✔| X|
|Logik-Apps|✔| X|
|VNETs|✔| Nicht verfügbar|
|Subnetze|✔| Nicht verfügbar|
|NICs|✔| ✔|
|NSGs|✔| Nicht verfügbar|
|Abonnement|✔| ✔|

* Diese Features werden derzeit in der öffentlichen Vorschau unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- [Planungs- und Betriebshandbuch für Azure Security Center:](security-center-planning-and-operations-guide.md) Hier erfahren Sie, wie Sie die Einführung von Azure Security Center planen, und Sie erhalten grundlegende Informationen zu Entwurfsaspekten.
- [Security alerts by type in Azure Security Center (Sicherheitswarnungen nach Typ in Azure Security Center):](security-center-alerts-type.md#virtual-machine-behavioral-analysis) Hier finden Sie Informationen zur Verhaltensanalyse bei virtuellen Computern und zur Absturzabbild-Speicheranalyse in Security Center.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog (Blog zur Azure-Sicherheit):](http://blogs.msdn.com/b/azuresecurity/) Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.
