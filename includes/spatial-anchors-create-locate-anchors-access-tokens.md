---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 0e4c0b6d622fe0d8d20692ec6c0c889779e4601a
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56751744"
---
### <a name="access-tokens"></a>Zugriffstoken

Zugriffstoken sind eine robustere Methode zur Authentifizierung bei Azure Spatial Anchors. Vor allem, wenn Sie Ihre Anwendung für eine Produktionsbereitstellung vorbereiten. Die Zusammenfassung dieses Ansatzes besteht darin, einen Back-End-Dienst einzurichten, bei dem sich Ihre Clientanwendung sicher authentifizieren kann. Ihr Back-End-Dienst kommuniziert zur Runtime mit AAD und mit dem STS-Dienst von Azure Spatial Anchors, um ein Zugriffstoken anzufordern. Dieses Token wird dann der Clientanwendung bereitgestellt und im SDK zur Authentifizierung bei Azure Spatial Anchors verwendet.
