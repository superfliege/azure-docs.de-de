---
title: Azure PowerShell-Skriptbeispiel – Erstellen eines Service Fabric-Clusters | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen eines Service Fabric-Testclusters mit drei Knoten.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 169f68d179c7f895078fe649d0e2a69e58d148cb
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-three-node-test-service-fabric-cluster"></a>Erstellen eines Service Fabric-Testclusters mit drei Knoten

Dieses Beispielskript erstellt einen Service Fabric-Testcluster mit drei Knoten, der mit einem X.509-Zertifikat geschützt wird. Die Clusterkonfiguration mit drei Knoten wird für Dev/Test unterstützt, da Sie sicher Upgrades ausführen und den Ausfall einzelner Knoten überstehen können (solange sie nicht gleichzeitig ausfallen). Produktionscluster benötigen fünf oder mehr Knoten, um Resilienz für gleichzeitige Ausfälle zu bieten.  

Der Befehl erstellt ein selbstsigniertes Zertifikat und lädt es in einen neuen Schlüsseltresor hoch, der in derselben Ressourcengruppe wie der Cluster erstellt wird. Das Zertifikat wird außerdem in ein lokales Verzeichnis kopiert.  Legen Sie den *-OS*-Parameter fest, um die Version von Windows oder Linux auszuwählen, in der die Clusterknoten ausgeführt werden.  Passen Sie die Parameter nach Bedarf an.

Installieren Sie bei Bedarf Azure PowerShell mithilfe der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Connect-AzureRmAccount` aus, um eine Verbindung mit Azure herzustellen. 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, der Cluster und alle zugehörigen Ressourcen entfernt werden.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Erstellt einen neuen Service Fabric-Cluster. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).
