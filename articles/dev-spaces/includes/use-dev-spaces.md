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
ms.openlocfilehash: 44ce986fcfdf079d3077c007a378f3467073d00d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991013"
---
### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Konfigurieren des AKS-Clusters zur Verwendung von Azure Dev Spaces

Öffnen Sie ein Befehlsfenster, und geben Sie den folgenden Azure CLI-Befehl ein. Verwenden Sie dabei die Ressourcengruppe, die Ihren AKS-Cluster enthält, und den Namen Ihres AKS-Clusters. Der Befehl konfiguriert Ihren Cluster mit Unterstützung für Azure Dev Spaces.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

