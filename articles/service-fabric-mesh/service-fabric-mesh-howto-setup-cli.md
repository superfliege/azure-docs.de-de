---
title: Einrichten der Azure Service Fabric Mesh CLI | Microsoft Docs
description: Erfahren Sie, wie die Azure Service Fabric CLI eingerichtet wird.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/26/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 567f2afdea44f439779212c61fb3a129f4f979be
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281571"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Einrichten der Service Fabric Mesh CLI
Die Service Fabric Mesh CLI ist zum Bereitstellen und Verwalten von Ressourcen in Service Fabric Mesh erforderlich. 

Für die Vorschauversion wird die Azure Service Fabric Mesh CLI als eine Erweiterung der Azure CLI bereitgestellt. Sie können sie in der Azure Cloud Shell oder in einer lokalen Installation der Azure CLI installieren. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie die Version 2.0.35 oder höher der Azure CLI installieren. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und zu einem Upgrade auf die neueste Version der CLI finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install].

Entfernen Sie alle vorherigen Installationen des Azure Service Fabric Mesh CLI-Moduls.

```azurecli-interactive
az extension remove --name mesh
```

Installieren Sie das Azure Service Fabric Mesh CLI-Erweiterungsmodul mit dem folgenden Befehl. 

```azurecli-interactive
az extension add --source https://meshcli.blob.core.windows.net/cli/mesh-0.9.1-py2.py3-none-any.whl
```

Sie können auch Ihre [Windows-Entwicklungsumgebung](service-fabric-mesh-howto-setup-developer-environment-sdk.md) einrichten.

[azure-cli-install]: /cli/azure/install-azure-cli