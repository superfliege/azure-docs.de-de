---
title: Unterstützte Ressourcen für Azure Red Hat OpenShift | Microsoft-Dokumentation
description: Sie erfahren, welche Azure-Regionen und VM-Größen von Microsoft Azure Red Hat OpenShift unterstützt werden.
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 22cc195b7808ad4a9111aafcf883a68c51f3709f
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080264"
---
# <a name="azure-red-hat-openshift-resources"></a>Azure Red Hat OpenShift-Ressourcen

In diesem Thema werden die Azure-Regionen und VM-Größen aufgeführt, die vom Microsoft Azure Red Hat OpenShift-Dienst unterstützt werden.

## <a name="azure-regions"></a>Azure-Regionen

Sie können Azure Red Hat OpenShift-Cluster in folgenden Azure-Regionen weltweit bereitstellen:

|Region|CLI-Code|
|-|-|
|🇦🇺 Australien, Osten|`australiaeast`|
|🇨🇦 Kanada, Mitte|`canadacentral`|
|🇨🇦 Kanada, Osten|`canadaeast`|
|🇺🇸 USA, Osten|`eastus`|
|🇺🇸 USA, Westen|`westus`|
|🇪🇺 Europa, Westen|`westeurope`|
|🇪🇺 Europa, Norden|`northeurope`|

## <a name="virtual-machine-sizes"></a>Größen virtueller Computer

Hier finden Sie die unterstützten Größen virtueller Computer, die Sie für die Computeknoten in Ihrem Azure Red Hat OpenShift-Cluster angeben können.

> [!Important]
> Jede VM verfügt über eine unterschiedliche Anzahl von Laufwerken, die angefügt werden können. Dies ist möglicherweise nicht direkt so klar wie bei Arbeitsspeicher oder CPU-Größe.
> Es sind nicht alle VM-Größen in allen Regionen verfügbar. Auch wenn die API die von Ihnen angegebene Größe unterstützt, erhalten Sie möglicherweise eine Fehlermeldung, wenn die Größe in der von Ihnen angegebenen Region nicht verfügbar ist.
> Weitere Informationen finden Sie unter [Aktuelle Liste unterstützter VM-Größen nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

## <a name="compute-node-sizes"></a>Computeknotengrößen

Die folgenden Computeknotengrößen werden von der Azure Red Hat OpenShift-REST-API unterstützt:

|Größe|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|
|-|-|-|
|Standard E4s v3|4|32 GB|
|Standard E8s v3|8|64 GB|
|Standard E16s v3|16|128 GB|
|Standard E32s v3|32|256 GB|
|-|-|-|
|Standard F8s v2|8|16 GB|
|Standard F16s v2|16|32 GB|
|Standard F32s v2|32|64 GB|

## <a name="master-node-sizes"></a>Master Node Sizes (Masterknotengrößen)

Die folgenden Master-/Infrastrukturknotengrößen werden von der Azure Red Hat OpenShift-REST-API unterstützt:

|Größe|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie das Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md).