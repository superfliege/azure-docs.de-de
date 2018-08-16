---
title: Includedatei
description: Includedatei
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: b8b8b1a593328197c61f8edba9f8f849518dd2e5
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129091"
---
## <a name="clean-up"></a>Bereinigen
Um eine Azure Dev Spaces-Instanz in einem Cluster vollständig zu löschen, einschließlich aller Entwicklungsbereiche und der darin ausgeführten Dienste, können Sie den Befehl `az aks remove-dev-spaces` verwenden. Beachten Sie, dass diese Aktion nicht rückgängig gemacht werden kann. Sie können dem Cluster die Unterstützung für Azure Dev Spaces wieder hinzufügen, aber dies entspricht einem kompletten Neubeginn. Ihre alten Dienste und Bereiche werden nicht wiederhergestellt.

Im folgenden Beispiel werden die Azure Dev Spaces-Controller Ihres aktiven Abonnements aufgelistet, und anschließend wird der Azure Dev Spaces-Controller gelöscht, der dem AKS-Cluster „myaks“ in der Ressourcengruppe „myaks-rg“ zugeordnet ist.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```

