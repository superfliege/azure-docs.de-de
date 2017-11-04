---
title: "Verwalten eines Azure Kubernetes-Clusters mit der Webbenutzeroberfläche | Microsoft-Dokumentation"
description: Verwenden des Kubernetes-Dashboards in AKS
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 764bbb95b661bb750d7802ee5996d8a491be928d
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="kubernetes-dashboard-with-azure-container-service-aks"></a>Kubernetes-Dashboard mit Azure Container Service (AKS)

Das Kubernetes-Dashboard kann über die Azure-Befehlszeilenschnittstelle gestartet werden. In diesem Dokument erfahren Sie Schritt für Schritt, wie Sie das Kubernetes-Dashboard über die Azure-Befehlszeilenschnittstelle starten und einige grundlegende Dashboardvorgänge ausführen. Weitere Informationen zum Kubernetes-Dashboard finden Sie unter [Web UI (Dashboard)](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) (Webbenutzeroberfläche – Dashboard). 

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine kubectl-Verbindung mit dem Cluster hergestellt haben. Informationen hierzu finden Sie in der [AKS-Schnellstartanleitung](./kubernetes-walkthrough.md).

Außerdem muss mindestens Version 2.0.20 der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie „az --version“ aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="start-kubernetes-dashboard"></a>Starten des Kubernetes-Dashboards

Verwenden Sie den Befehl `az aks browse`, um das Kubernetes-Dashboard zu starten. Ersetzen Sie beim Ausführen dieses Befehls die Ressourcengruppe und den Clusternamen.

```azurecli
az aks browse --resource-group myResourceGroup --name myK8SCluster
```

Der Befehl erstellt einen Proxy zwischen Ihrem Entwicklungssystem und der Kubernetes-API und öffnet einen Webbrowser mit dem Kubernetes-Dashboard.

## <a name="run-an-application"></a>Ausführen einer Anwendung

Klicken Sie im Kubernetes-Dashboard im rechten oberen Fenster auf die Schaltfläche **Erstellen**. Nennen Sie die Bereitstellung `nginx`, und geben Sie `nginx:latest` als Imagenamen ein. Klicken Sie unter **Dienst** auf **Extern**, und geben Sie für Port und Zielport jeweils `80` ein.

Klicken Sie abschließend auf **Bereitstellen**, um die Bereitstellung zu erstellen.

![Kubernetes-Dialogfeld für die Diensterstellung](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Anzeigen der Anwendung

Der Anwendungsstatus kann im Kubernetes-Dashboard überprüft werden. Sobald die Anwendung ausgeführt wird, wird neben den einzelnen Komponenten jeweils ein grünes Kontrollkästchen angezeigt.

![Kubernetes-Pods](./media/container-service-kubernetes-ui/complete-deployment.png)

Weitere Informationen zu den Anwendungspods erhalten Sie, indem Sie im linken Menü auf **Pods** klicken und den Pod **NGINX** auswählen. Hier stehen podspezifische Informationen (etwa zum Ressourcenverbrauch) zur Verfügung.

![Kubernetes-Ressourcen](./media/container-service-kubernetes-ui/running-pods.png)

Klicken Sie zum Ermitteln der IP-Adresse der Anwendung im linken Menü auf **Dienste**, und wählen Sie den Dienst **NGINX** aus.

![nginx-Ansicht](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Bearbeiten der Anwendung

Über das Kubernetes-Dashboard können Sie nicht nur Anwendungen erstellen und anzeigen, sondern auch Anwendungsbereitstellungen bearbeiten und aktualisieren.

Wenn Sie eine Bereitstellung bearbeiten möchten, klicken Sie im linken Menü auf **Bereitstellungen**, und wählen Sie die Bereitstellung **NGINX** aus. Klicken Sie anschließend auf der Navigationsleiste rechts oben auf **Bearbeiten**.

![Kubernetes-Schaltfläche „Edit“](./media/container-service-kubernetes-ui/view-deployment.png)

Suchen Sie nach dem `spec.replica`-Wert (müsste auf „1“ festgelegt sein), und ändern Sie ihn in „3“. Dadurch erhöht sich die Replikatanzahl der NGINX-Bereitstellung von 1 auf 3.

Klicken Sie abschließend auf **Aktualisieren**.

![Kubernetes-Schaltfläche „Edit“](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Kubernetes-Dashboard finden Sie in der Kubernetes-Dokumentation.

> [!div class="nextstepaction"]
> [Web UI (Dashboard)](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) (Webbenutzeroberfläche – Dashboard)