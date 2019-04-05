---
ms.openlocfilehash: 307e4d4e21eca3a568a7e4761058b67aa7d5236f
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/24/2019
ms.locfileid: "56752099"
---
Wenn ein AAD-Token nicht festgelegt ist, müssen Sie wie bei Zugriffstoken das TokenRequired-Ereignis behandeln oder die tokenRequired-Methode auf dem Delegatenprotokoll implementieren.

Sie können das Ereignis synchron behandeln, indem Sie die Eigenschaft für die Ereignisargumente festlegen.
