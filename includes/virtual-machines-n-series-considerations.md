---
title: Includedatei
description: Includedatei
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3267f649e360c512a5523ce1d5948719a1969934
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32297585"
---
## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

* Informationen zur Verfügbarkeit von virtuellen Computern der N-Serie finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

* Virtuelle Computer der N-Serie können nur im Resourcen Manager-Bereitstellungsmodell bereitgestellt werden.

* Virtuelle Computer der Serie N unterscheiden hinsichtlich des Typs von Azure Storage, den sie für ihre Datenträger unterstützen. Virtuelle NC- oder NV-Computer unterstützen nur VM-Datenträger, die durch Standard Disk Storage (HDD) gesichert sind. Virtuelle NCv2-, ND- und NCv3-Computer unterstützen nur VM-Datenträger, die durch Premium Disk Storage (SSD) gesichert sind.

* Wenn Sie eine größere Anzahl von virtuellen Computern der N-Serie bereitstellen möchten, sollten Sie ein nutzungsbasiertes Abonnement oder andere Kaufoptionen in Erwägung ziehen. Bei Verwendung eines [kostenlosen Azure-Kontos](https://azure.microsoft.com/free/)können Sie nur eine begrenzte Anzahl von Azure-Compute-Kernen nutzen.

* Möglicherweise müssen Sie das Kernnutzungskontingent (pro Region) in Ihrem Azure-Abonnement sowie das separate Kontingent für NC-, NCv2-, NCv3-, ND- oder NV-Kerne erhöhen. In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../articles/azure-supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern. Standardgrenzwerte variieren unter Umständen je nach Abonnementkategorie.




