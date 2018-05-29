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
ms.openlocfilehash: 2563f7c36283521541562bcd88f973d86a6f672a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198985"
---
## <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Konfigurieren des AKS-Clusters zur Verwendung von Azure Dev Spaces

Öffnen Sie ein Befehlsfenster, und geben Sie die folgenden Azure CLI-Befehle ein. Verwenden Sie dabei die Ressourcengruppe, die Ihr AKS-Cluster enthält, und den Namen Ihres AKS-Clusters:

   ```cmd
   az extension add --name dev-spaces-preview 
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```
Mit dem ersten Befehl wird eine Erweiterung für die Azure CLI installiert, um Unterstützung für Azure Dev Spaces Hinzuzufügen, und mit dem zweiten Befehl wird Ihr Cluster mit Unterstützung für Azure Dev Spaces konfiguriert.
