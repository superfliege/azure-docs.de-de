---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 23b92f7d1b1c3ef488e182a6443f275365a03d9a
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572132"
---
## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach der Ausführung des Skriptbeispiels können Sie den folgenden Befehl verwenden, um die Ressourcengruppe, die Azure Redis Cache-Instanz und alle dazugehörigen Ressourcen in der Ressourcengruppe zu entfernen.

```azurecli
az group delete --name contosoGroup
```