---
title: Upgrade von Azure Dev Spaces-Tools | Microsoft-Dokumentation
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/03/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
manager: douge
ms.openlocfilehash: bcf36e14a12119dd13b8e566bc0a5b0ac96925c5
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37934122"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Upgrade von Azure Dev Spaces-Tools

Wenn eine neue Release vorliegt und Sie Azure Dev Spaces bereits verwenden, müssen Sie die Azure Dev Spaces-Clienttools eventuell aktualisieren.

## <a name="update-the-azure-cli"></a>Aktualisieren der Azure-Befehlszeilenschnittstelle

Wenn Sie die neueste Azure-Befehlszeilenschnittstelle aktualisieren, erhalten Sie auch die neueste Version der Dev Spaces-CLI-Erweiterung.

Sie müssen die frühere Version nicht deinstallieren, sondern nur den geeigneten Download unter [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?view=azure-cli-latest) suchen.


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Upgrade der Dev Spaces-CLI-Erweiterung und -Befehlszeilentools

Führen Sie den folgenden Befehl aus:

```cmd
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>Aktualisieren der VS Code-Erweiterung

Nach der Installation wird die Erweiterung automatisch aktualisiert. Sie müssen die Erweiterung möglicherweise erneut laden, um die neuen Features nutzen zu können. Öffnen Sie in VS Code den Bereich **Erweiterungen**, wählen Sie die **Azure Dev Spaces-Erweiterungen** aus, und wählen Sie **Erneut laden** aus.

## <a name="update-the-visual-studio-extension"></a>Aktualisieren der Visual Studio-Erweiterung

Wie bei anderen Erweiterungen und Updates erhalten Sie von Visual Studio eine Benachrichtigung, wenn ein Update für die Visual Studio-Tools für Kubernetes, die Azure Dev Spaces enthalten, verfügbar ist. Suchen Sie nach einem Flaggensymbol rechts oben auf dem Bildschirm.

Um die Tools in Visual Studio zu aktualisieren, wählen Sie die Menüoption **Tools > Erweiterungen und Updates** aus, und wählen Sie auf der linken Seite **Updates** aus. Suchen Sie **Visual Studio-Tools für Kubernetes**, und wählen Sie die Schaltfläche **Aktualisieren** aus.

## <a name="next-steps"></a>Nächste Schritte

Testen Sie die neuen Tools durch Erstellen eines neuen Clusters. Probieren Sie die Schnellstarts und Tutorials unter [Azure Dev Spaces](/azure/dev-spaces) aus.

> [!WARNING]
> Azure Dev Spaces auf vorhandenen Clustern wird nicht sofort gepatcht. Stellen Sie daher sicher, dass Sie die neueste Version in allen Ihren Azure-Bereitstellungen verwenden, indem Sie nach dem Upgrade der Tools einen neuen Cluster erstellen.
