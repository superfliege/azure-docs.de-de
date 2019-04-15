---
ms.openlocfilehash: 7219a457a2631f9ff6beee06eff34bce0ff5a23f
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631663"
---
Wenn ein Azure AD-Token nicht festgelegt ist, müssen Sie wie bei Zugriffstoken das TokenRequired-Ereignis behandeln oder die tokenRequired-Methode auf dem Delegatenprotokoll implementieren.

Sie können das Ereignis synchron behandeln, indem Sie die Eigenschaft für die Ereignisargumente festlegen.
