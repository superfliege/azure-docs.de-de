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
ms.openlocfilehash: 4c4a5b66fe35da01a3661715e17a9fda20bc2411
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43184721"
---
## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Erstellen eines Kubernetes-Clusters mit Aktivierung für Azure Dev Spaces

1. Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.
1. Wählen Sie **Ressource erstellen**, suchen Sie nach **Kubernetes**, und wählen Sie dann **Kubernetes Service** > **Erstellen**.

   Führen Sie unter jeder Überschrift des Formulars zur Erstellung des AKS-Clusters die folgenden Schritte aus.

    - **PROJEKTDETAILS**: Wählen Sie ein Azure-Abonnement und eine neue oder vorhandene Azure-Ressourcengruppe aus.
    - **CLUSTERDETAILS**: Geben Sie Name, Region (derzeit müssen Sie zwischen „EastUS“, „USA, Mitte“, „WestEurope“, „WestUS2“, „CanadaCentral“ und „CanadaEast“ wählen), Version und DNS-Namenspräfix für den AKS-Cluster ein.
    - **SKALIERUNG**: Wählen Sie eine VM-Größe für die AKS-Agent-Knoten und die Anzahl von Knoten aus. Beim Einstieg in Azure Dev Spaces reicht ein Knoten aus, um alle Features zu erkunden. Sie können die Knotenanzahl jeweils auf einfache Weise anpassen, nachdem der Cluster bereitgestellt wurde. Beachten Sie, dass die VM-Größe nicht mehr geändert werden kann, nachdem ein AKS-Cluster erstellt wurde. Nach der Bereitstellung eines AKS-Clusters können Sie aber leicht einen neuen AKS-Cluster mit größeren virtuellen Computern erstellen und Dev Spaces für die erneute Bereitstellung in diesem größeren Cluster nutzen, wenn Sie zentral hochskalieren müssen.

   Achten Sie darauf, dass Sie die Kubernetes-Version 1.9.6 oder höher wählen.

   ![Einstellungen für Kubernetes-Konfiguration](../media/common/Kubernetes-Create-Cluster-2.PNG)

   Wählen Sie nach Abschluss des Vorgangs **Next: Authentication** (Nächster Schritt: Authentifizierung).

1. Wählen Sie die gewünschte Einstellung für die rollenbasierte Zugriffssteuerung (RBAC). Azure Dev Spaces unterstützt Cluster mit aktivierter und deaktivierter RBAC.

    ![RBAC-Einstellung](../media/common/k8s-RBAC.PNG)

1. Stellen Sie sicher, dass das HTTP-Anwendungsrouting aktiviert ist.

   ![Aktivieren von HTTP-Anwendungsrouting](../media/common/Kubernetes-Create-Cluster-3.PNG)

    > [!Note]
    > Führen Sie zum Aktivieren von [HTTP-Anwendungsrouting](/azure/aks/http-application-routing) auf einem vorhandenen Cluster den folgenden Befehl aus: `az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing`

1. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen**.
