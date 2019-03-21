---
title: Renderinganwendungen – Azure Batch
description: Vorinstallierte Batch Rendering-Anwendungen
services: batch
ms.service: batch
author: laurenhughes
ms.author: lahugh
ms.date: 12/11/2018
ms.topic: conceptual
ms.openlocfilehash: 8ce430d83c52014b3d1d3d2a985f74aeb488caea
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791884"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Vorinstallierte Anwendungen in Rendering-VM-Images

Mit Azure Batch können beliebige Renderinganwendungen verwendet werden. In Azure Marketplace-VM-Images sind gängige Anwendungen jedoch bereits vorinstalliert.

Für vorinstallierte Renderinganwendungen ist ggf. Lizenzierung mit nutzungsbasierter Zahlung verfügbar. Bei der Erstellung eines Batch-Pools können die erforderlichen Anwendungen angegeben werden, und die Kosten des virtuellen Computers sowie der Anwendungen werden pro Minute abgerechnet. Anwendungspreise sind auf der [Seite mit den Preisen für Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering) aufgeführt.

Einige Anwendungen unterstützen nur Windows, die meisten werden jedoch unter Windows und Linux unterstützt.

## <a name="applications-on-centos-7-rendering-nodes"></a>Anwendungen auf CentOS 7-Renderingknoten:

* Autodesk Maya I/O 2017 Update 5 (Cut 201708032230)
* Autodesk Maya I/O 2018 Update 2 (Cut 201711281015)
* Autodesk Arnold for Maya 2017 (Arnold-Version 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold for Maya 2018 (Arnold-Version 5.0.1.4) MtoA-2.1.0.3-2018
* Chaos Group V-Ray for Maya 2017 (Version 3.60.04)
* Chaos Group V-Ray for Maya 2018 (Version 3.60.04)
* Blender (2.68)

## <a name="applications-on-windows-server-2016-rendering-nodes"></a>Anwendungen auf Windows Server 2016-Renderingknoten:

* Autodesk Maya I/O 2017 Update 5 (Version 17.4.5459)
* Autodesk Maya I/O 2018 Update 4 (Version 18.4.0.7622)  
* Autodesk 3ds Max I/O 2019 Update 1 (Version 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Update 4 (Version 20.4.0.4254)
* Autodesk Arnold for Maya 2017 (Arnold-Version 5.2.0.1) MtoA-3.1.0.1-2017
* Autodesk Arnold for Maya 2018 (Arnold-Version 5.2.0.1) MtoA-3.1.0.1-2018
* Autodesk Arnold for 3ds Max (Arnold-Version 5.0.2.4) (Version 1.2.926)
* Chaos Group V-Ray for Maya (Version 3.52.03)
* Chaos Group V-Ray for 3ds Max (Version 3.60.02)
* Blender (2.79)

## <a name="next-steps"></a>Nächste Schritte

Damit Sie die Rendering-VM-Images verwenden können, müssen Sie sie beim Erstellen eines Pools in der Poolkonfiguration angeben. Informationen finden Sie unter [Azure Batch rendering capabilities](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools) (Renderingfunktionen von Azure Batch).