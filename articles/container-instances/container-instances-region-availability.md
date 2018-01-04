---
title: "Regions- und Ressourcenverfügbarkeit für Azure Container Instances"
description: "Erfahren Sie, in welchen Azure-Regionen die Bereitstellung von Containerinstanzen unterstützt wird und welche CPU- und Arbeitsspeicherlimits für diese Instanzen gelten."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 12/15/2017
ms.author: marsma
ms.openlocfilehash: ec7f469c47924f4ae22d6509996ca9cf498fc9ad
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Regionale Verfügbarkeit für Azure Container Instances

Während der Vorschauphase ist Azure Container Instances mit den angegebenen CPU- und Arbeitsspeicherlimits in den folgenden Regionen verfügbar.

| Speicherort | Betriebssystem | CPU | Arbeitsspeicher (GB) |
| -------- | -- | :---: | :-----------: |
| Europa, Westen; USA, Westen; USA, Osten | Linux | 4 | 14 |
| Europa, Westen; USA, Westen; USA, Osten | Windows | 4 | 14 |

## <a name="resource-availability"></a>Ressourcenverfügbarkeit

Containerinstanzen, die innerhalb dieser Ressourcenlimits erstellt werden, unterliegen der Verfügbarkeit in der Bereitstellungsregion. Wenn eine Region stark ausgelastet ist, kann bei der Bereitstellung von Instanzen ein Fehler auftreten.

Um einen solchen Fehler bei der Bereitstellung zu beheben, versuchen Sie, Instanzen mit niedrigeren CPU- und Arbeitsspeichereinstellungen bereitzustellen, oder führen Sie die Bereitstellung zu einem späteren Zeitpunkt durch.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung bei der Bereitstellung von Containerinstanzen finden Sie unter [Beheben von Bereitstellungsproblemen für Azure Container Instances](container-instances-troubleshooting.md).