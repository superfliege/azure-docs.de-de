---
title: Unterstützte Plattformen in Azure Security Center | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Liste mit Windows- und Linux-Betriebssystemen, die in Azure Security Center unterstützt werden.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: terrylan
ms.openlocfilehash: 54d173caa0e3eb4bbd8bda7c924e56d546a99662
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297501"
---
# <a name="supported-platforms-in-azure-security-center"></a>Unterstützte Plattformen in Azure Security Center
Die Überwachung des Sicherheitsstatus und entsprechende Empfehlungen sind für virtuelle Computer (VMs), die mit dem klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, und für Computer verfügbar.

> [!NOTE]
> Erfahren Sie mehr über das [klassische und das Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) für Azure-Ressourcen.
>
>

## <a name="supported-platforms-for-windows-computers-and-vms"></a>Unterstützte Plattformen für Windows-Computer und -VMs
Unterstützte Windows-Betriebssysteme:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


## <a name="supported-platforms-for-linux-computers-and-vms"></a>Unterstützte Plattformen für Linux-Computer und -VMs
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

## <a name="next-steps"></a>Nächste Schritte

- [Planungs- und Betriebshandbuch für Azure Security Center:](security-center-planning-and-operations-guide.md) Hier erfahren Sie, wie Sie die Einführung von Azure Security Center planen, und Sie erhalten grundlegende Informationen zu Entwurfsaspekten.
- [Security alerts by type in Azure Security Center (Sicherheitswarnungen nach Typ in Azure Security Center):](security-center-alerts-type.md#virtual-machine-behavioral-analysis) Hier finden Sie Informationen zur Verhaltensanalyse bei virtuellen Computern und zur Absturzabbild-Speicheranalyse in Security Center.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog (Blog zur Azure-Sicherheit):](http://blogs.msdn.com/b/azuresecurity/) Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.
