---
title: Erstellen eines Kubernetes-Clusters mit Aktivierung für Azure Dev Spaces mithilfe von Azure Cloud Shell | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: article
description: Hier erfahren Sie, wie Sie schnell und ohne Installation zusätzlicher Komponenten direkt über Ihren Browser einen Kubernetes-Cluster mit Aktivierung für Azure Dev Spaces erstellen.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 61a466efdacb56889a7e437d84d3c9de43927e65
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705751"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Erstellen eines Kubernetes-Clusters mit Azure Cloud Shell

Sie können mit [Azure Cloud Shell](/azure/cloud-shell) einen Cluster für Azure Dev Spaces erstellen. Verwenden Sie dazu die Schaltfläche **Ausprobieren** auf dieser Seite. Wenn Sie nicht angemeldet sind, folgen Sie den Anweisungen zum Anmelden mit einem Azure-Konto, und geben Sie dann die Befehle an der angezeigten Azure Cloud Shell-Eingabeaufforderung ein.

## <a name="create-the-cluster"></a>Erstellen des Clusters

Erstellen Sie zunächst die Ressourcengruppe. Verwenden Sie eine der derzeit unterstützten Regionen („USA, Osten“, „USA, Osten 2“, „USA, Mitte“, „USA, Westen 2“, „Europa, Westen“, „Asien, Südosten“, „Kanada, Mitte“ oder „Kanada, Osten“).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Erstellen Sie mit dem folgenden Befehl einen Kubernetes-Cluster:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.3 --enable-addons http_application_routing
```

Die Erstellung des Clusters dauert einige Minuten.  Nach Abschluss des Vorgangs wird die Ausgabe im JSON-Format angezeigt. Suchen Sie nach `provisioningState`, und vergewissern Sie sich, dass `Succeeded` dafür angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Links zu vollständigen Tutorials finden Sie unter [Azure Dev Spaces](/azure/dev-spaces/).
