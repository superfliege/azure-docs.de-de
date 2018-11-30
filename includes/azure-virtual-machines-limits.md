---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 7e390e2134df02b0ca9c0d1752c3207aff7b9314
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279640"
---
| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| [Virtuelle Computer](../articles/virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pro Clouddienst<sup>1</sup> |50 |50 |
| Eingabeendpunkte pro Clouddienst<sup>2</sup> |150 |150 |

<sup>1</sup>Die in der Dienstverwaltung (und nicht im Ressourcen-Manager) erstellten virtuellen Computer werden automatisch in einem Clouddienst gespeichert. Für den Lastenausgleich und die Verfügbarkeit können Sie diesem Clouddienst weitere virtuelle Computer hinzufügen. 

<sup>2</sup>Eingabeendpunkte ermöglichen die Kommunikation mit einem virtuellen Computer von außerhalb des Clouddiensts des virtuellen Computers. Virtuelle Computer im gleichen Clouddienst oder virtuellen Netzwerk können automatisch miteinander kommunizieren. Siehe: [Einrichten von Endpunkten für einen virtuellen Computer](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 
