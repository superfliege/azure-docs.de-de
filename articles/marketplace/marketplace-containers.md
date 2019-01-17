---
title: Leitfaden für die Veröffentlichung von Containerangeboten im Azure Marketplace
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Containern im Marketplace beschrieben.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: 6b02714c3a62e8d11512c1cc2dfc7a75a422441d
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076381"
---
# <a name="containers-offer-publishing-guide"></a>Leitfaden für die Veröffentlichung von Containerangeboten

Containerangebote erleichtern Ihnen die Veröffentlichung Ihres Containerimages im Azure Marketplace. Informieren Sie sich in diesem Handbuch über die Anforderungen für dieses Angebot. 

Hierbei handelt es sich um Transaktionsangebote, die über den Marketplace bereitgestellt und abgerechnet werden. Die Benutzer sehen hier den Aktionsaufruf „Jetzt kaufen“.

Verwenden Sie den Angebotstyp „Container“, wenn Ihre Lösung ein Docker-Container-Image ist, das als Kubernetes-basierter Azure Container Service bereitgestellt wird.

>[!NOTE]
>Z.B. ein Kubernetes-basierter Azure Container Cervice wie Azure Kubernetes Service oder Azure Container Instances, die Wahl der Azure-Kunden für eine Kubernetes-basierte Containerlaufzeit.  

Microsoft unterstützt derzeit kostenlose und Bring-Your-Own-License-Lizenzierungsmodelle (BYOL).

## <a name="containers-offer"></a>Containerangebot

| Anforderung | Details |  
|:--- |:--- |  
| Abrechnung und Messung | Es wird entweder das kostenlose oder BYOL-Abrechnungsmodell unterstützt. |  
| Auf Grundlage von Dockerfile erstelltes Image | Containerimages müssen auf der Docker-Imagespezifikation basieren und auf der Grundlage einer Dockerfile-Datei erstellt werden.<ul> <li>Weitere Informationen zum Erstellen von Docker-Images finden Sie im Abschnitt zur Verwendung auf [docs.docker.com/engine/reference/builder/#usage](https://docs.docker.com/engine/reference/builder/#usage).</li> </ul> |  
| Hosten in ACR | Containerimages müssen in einem ACR-Repository (Azure Container Registry) gehostet werden.<ul> <li>Weitere Informationen zum Arbeiten mit ACR finden Sie im „Schnellstart: Erstellen einer Containerregistrierung mit dem Azure-Portal“ auf [docs.microsoft.com/de-de/azure/container-registry/container-registry-get-started-portal](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal).</li> </ul> |  
| Imagemarkierung | Containerimages müssen mindestens 1 Tag enthalten (maximale Tags: 16).<ul> <li>Weitere Informationen zum Markieren eines Images finden Sie auf der Docker-Tag-Seite auf [docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag).</li> </ul> |  


## <a name="next-steps"></a>Nächste Schritte

Falls Sie dies noch nicht getan haben, 

- [Registrieren Sie](https://azuremarketplace.microsoft.com/sell) beim Marketplace.

Wenn Sie registriert sind und ein neues Angebot erstellen oder an einem vorhandenen arbeiten,

- [melden Sie sich beim Cloud-Partnerportal an](https://cloudpartner.azure.com), um Ihr Angebot zu erstellen oder zu vervollständigen.
- Weitere Informationen finden Sie unter [Container](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-containers-offer).
