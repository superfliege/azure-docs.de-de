---
title: Unterstützung von Azure Batch-Render-Managern
description: Verwenden von Azure für das Rendering mit Integration eines Azure Batch-Render-Managers
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 798b2b457016856662f392af25d987788f73c242
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40036721"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Verwenden von Azure Batch mit Renderfarm-Managern

Wenn Sie eine vorhandene lokale Renderfarm nutzen, ist die Wahrscheinlichkeit hoch, dass die Kapazität und die Renderaufträge der Renderfarm mit einem Render-Manager gesteuert werden.

In Azure werden entweder die integrierte Unterstützung oder Add-Ons für beliebte Render-Manager bereitgestellt. Sie können dann Azure-VMs hinzufügen und entfernen, z.B. auch VMs mit nutzungsbasierter Anwendungslizenzierung und VMs mit niedriger Priorität.

Die folgenden Render-Manager werden unterstützt:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Royal Render](http://www.royalrender.de/)
* [Thinkbox Deadline](https://deadline.thinkboxsoftware.com/)

## <a name="using-azure-with-pipelinefx-qube"></a>Verwenden von Azure mit PipelineFX Qube

Skripts und eine Anleitung zum Aktivieren von Azure Batch-Pool-VMs als Qube-Worker finden Sie im entsprechenden [GitHub-Repository](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Verwenden von Azure mit Royal Render

Royal Render verfügt standardmäßig über eine Azure- und Azure Batch-Integration, sodass Sie eine Renderfarm mit Azure-basierten VMs erweitern können. Eine Zusammenfassung hierzu finden Sie in den [Hilfedateien](http://www.royalrender.de/help8/index.html?Cloudrendering.html).

Ein Beispiel für einen Royal Render-Kunden, der die Azure-Integration nutzt, finden Sie unter dem [Jellyfish Pictures-Kundenbericht](https://customers.microsoft.com/en-gb/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Verwenden von Azure mit Thinkbox Deadline

Skripts und eine Anleitung zum Aktivieren von Azure Batch-Pool-VMs als Deadline-Slaves finden Sie im entsprechenden [GitHub-Repository](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Nächste Schritte

Testen Sie die Azure Batch-Integration für Ihren Render-Manager, indem Sie das entsprechende Plug-In und die Anleitung auf GitHub verwenden, falls zutreffend.