---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: afa3fea0c172134afb06abc63c90805a32c89639
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57907694"
---
## <a name="set-up-authentication"></a>Einrichten der Authentifizierung

Um auf den Dienst zugreifen zu können, müssen Sie einen Kontoschlüssel, ein Zugriffstoken oder ein AAD-Authentifizierungstoken angeben.

### <a name="account-keys"></a>Kontoschlüssel

Kontoschlüssel sind eine Anmeldeinformationen, die es Ihrer Anwendung ermöglichen, sich beim Azure Spatial Anchors-Dienst zu authentifizieren. Der Zweck von Kontoschlüsseln besteht darin, Ihnen einen schnellen Einstieg zu ermöglichen. Vor allem während der Entwicklungsphase der Integration Ihrer Anwendung mit Azure Spatial Anchors. Sie können Kontoschlüssel verwenden, indem Sie sie während der Entwicklung in Ihre Clientanwendungen einbetten. Nach Abschluss der Entwicklung wird dringend empfohlen, zu einem Authentifizierungsmechanismus für die Produktionsebene zu wechseln, der von Zugriffstoken oder AAD-basierter Benutzerauthentifizierung unterstützt wird. Um einen Kontoschlüssel für die Entwicklung zu erhalten, besuchen Sie Ihr Azure Spatial Anchors-Konto, und navigieren Sie zur Registerkarte „Schlüssel“.
