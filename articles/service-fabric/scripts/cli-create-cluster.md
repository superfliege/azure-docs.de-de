---
title: "Azure Service Fabric-CLI-Skript – Bereitstellungsbeispiel"
description: Erstellen Sie einen sicheren Service Fabric-Linux-Cluster in Azure mithilfe von Azure Service Fabric CLI.
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 09/18/2017
ms.author: ryanwi
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: 7f4abeb6947b7df7e7960ae854c8f208e6c36c53
ms.contentlocale: de-de
ms.lasthandoff: 09/21/2017

---

# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Erstellen eines sicheren Service Fabric-Linux-Clusters in Azure

Dieser Befehl erstellt ein selbst signiertes Zertifikat, fügt es zum Schlüsseltresor hinzu und lädt das Zertifikat lokal herunter.  Mit dem neuen Zertifikat wird der Cluster gesichert, wenn er bereitgestellt wird.  Anstelle der Erstellung eines neuen Zertifikats können Sie auch ein vorhandenes Zertifikat verwenden.  Der Name des Antragstellers für das Zertifikat muss in jedem Fall der Domäne entsprechen, über die Sie auf den Service Fabric-Cluster zugreifen. Dies ist erforderlich, damit SSL für die HTTPS-Verwaltungsendpunkte des Clusters und für Service Fabric Explorer bereitgestellt werden kann. Für die Domäne `.cloudapp.azure.com` können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Installieren Sie ggf. [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Beispielskript

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, der Cluster und alle zugehörigen Ressourcen entfernt werden.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/en-us/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Erstellt einen neuen Service Fabric-Cluster.  |

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Service Fabric-CLI-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../samples-cli.md).

