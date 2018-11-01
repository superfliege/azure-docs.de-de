---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226467"
---
## <a name="issue-custom-image-provisioning-errors"></a>Problem: benutzerdefiniertes Image; Bereitstellungsfehler
Bereitstellungsfehler treten beim Hochladen oder Erfassen eines generalisierten VM-Images als spezialisiertes VM-Image (oder umgekehrt) auf. Im ersten Fall tritt bei der Bereitstellung ein Timeoutfehler auf und im zweiten Fall ein Bereitstellungsfehler. Um das benutzerdefinierte Image ohne Fehler bereitzustellen, müssen Sie sicherstellen, dass sich der Typ des Images während des Erfassungsvorgangs nicht ändert.

In der folgenden Tabelle sind die möglichen Kombinationen von generalisierten und spezialisierten Images aufgeführt. Zudem werden die verschiedenen Fehlertypen und die Lösungen zum Beheben der Fehler beschrieben.

