---
title: Behebung unerwarteter Neustarts von VMs mit angefügten VHDs auf virtuellen Windows-Computern in Azure | Microsoft-Dokumentation
description: So beheben Sie unerwartete Neustarts virtueller Windows-Computer.
keywords: SSH-Verbindung abgelehnt, SSH-Fehler, Azure SSH, SSH-Verbindungsfehler
services: virtual-machines-windows
author: tamram
manager: jeconnoc
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/01/2018
ms.author: tamram
ms.openlocfilehash: 2057ffe2d5ec61e1192ff12291114c43dc2cc049
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32779693"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Problembehandlung bei unerwarteten Neustarts von virtuellen Computern mit angefügten VHDs

Wenn ein virtueller Azure-Computer über eine große Anzahl von angefügten VHDs verfügt, die sich im gleichen Speicherkonto befinden, können die Skalierbarkeitsziele für ein einzelnes Speicherkonto überschritten werden. Dies führt zu einem unerwarteten Neustart der VM (Virtual Machine, virtueller Computer). Überprüfen Sie die Minutenmetriken für das Speicherkonto (**TotalRequests**/**TotalIngress**/**TotalEgress**) auf Spitzen, die die Skalierbarkeitsziele für ein Speicherkonto überschreiten. Unter [Metriken zeigen einen Anstieg bei PercentThrottlingError an](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) finden Sie Unterstützung bei der Ermittlung, ob in Ihrem Speicherkonto eine Drosselung stattgefunden hat.

In der Regel wird jeder einzelne Eingabe- oder - oder Ausgabevorgang auf einer VHD von einem virtuellen Computer in **Get Page**- oder **Put Page**-Vorgänge des zugrunde liegenden Seitenblobs übersetzt. Daher können Sie die geschätzten IOPS für Ihre Umgebung verwenden, um zu bestimmen, über wie viele virtuelle Festplatten Sie in einem einzigen Speicherkonto auf Grundlage des spezifischen Anwendungsverhaltens verfügen können. Microsoft empfiehlt höchstens 40 Datenträger in einem Speicherkonto. Unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../../storage/common/storage-scalability-targets.md) finden Sie nähere Informationen über Skalierbarkeitsziele für Speicherkonten, insbesondere die Gesamtanforderungsrate und die Gesamtbandbreite für den Speicherkontotyp, den Sie verwenden.

Wenn Sie die Skalierbarkeitsziele für Ihr Speicherkonto überschreiten, platzieren Sie die virtuellen Festplatten in mehreren Speicherkonten, um die Aktivität in den einzelnen Konten zu reduzieren.
