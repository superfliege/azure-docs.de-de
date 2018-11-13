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
ms.openlocfilehash: c30f4b9de279f8c02b7f6bc7fa7d9765972899b1
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2018
ms.locfileid: "50977430"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Einrichten der Service Fabric Mesh CLI
Die Service Fabric Mesh-Befehlszeilenschnittstelle (Command Line Interface, CLI) ist zum Bereitstellen und Verwalten von Ressourcen in Service Fabric Mesh erforderlich. 

Für die Vorschauversion wird die Azure Service Fabric Mesh CLI als eine Erweiterung der Azure CLI bereitgestellt. Sie können sie in der Azure Cloud Shell oder in einer lokalen Installation der Azure CLI installieren. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-service-fabric-mesh-cli-locally"></a>Lokales Installieren der Service Fabric Mesh-Befehlszeilenschnittstelle
Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie die Version 2.0.43 oder höher der Azure CLI installieren. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und zu einem Upgrade auf die neueste Version der CLI finden Sie unter [Installieren der Azure CLI][azure-cli-install].

Installieren Sie das Azure Service Fabric Mesh CLI-Erweiterungsmodul mit dem folgenden Befehl. 

```azurecli-interactive
az extension add --name mesh
```

Führen Sie zum Aktualisieren eines vorhandenen Azure Service Fabric Mesh-CLI-Moduls den folgenden Befehl aus.

```azurecli-interactive
az extension update --name mesh
```
## <a name="next-steps"></a>Nächste Schritte

Sie können auch Ihre [Windows-Entwicklungsumgebung](service-fabric-mesh-howto-setup-developer-environment-sdk.md) einrichten.

[Hier](service-fabric-mesh-faq.md) finden Sie Antworten auf häufig gestellte Fragen sowie Informationen zu Problemen.

[azure-cli-install]: /cli/azure/install-azure-cli
