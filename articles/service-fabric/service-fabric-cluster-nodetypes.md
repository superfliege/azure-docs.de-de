---
title: Azure Service Fabric-Knotentypen und VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure Service Fabric-Knotentypen mit VM-Skalierungsgruppen in Zusammenhang stehen und wie eine Remoteverbindung mit einer Skalierungsgruppeninstanz oder einem Clusterknoten hergestellt wird.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: a5f8735df2b230de2b0ddcdcccff09430bada9e3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64684695"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric-Knotentypen und VM-Skalierungsgruppen
[VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets) sind eine Azure Computeressource. Sie können Skalierungsgruppen verwenden, um eine Sammlung virtueller Computer als Gruppe bereitzustellen und zu verwalten. Jeder Knotentyp, den Sie in einem Azure Service Fabric-Cluster definieren, richtet eine separate Skalierung ein.  Die auf jedem virtuellen Computer in der Skalierungsgruppe von der Virtual Machines-Erweiterung Microsoft.Azure.ServiceFabric installierte Service Fabric-Laufzeit. Sie können jeden Knotentyp einzeln zentral hoch- oder herunterskalieren, auf jedem Clusterknoten die ausgeführte Betriebssystem-SKU ändern, bei jedem Typ unterschiedliche Portgruppen öffnen und verschiedene Kapazitätsmetriken verwenden.

Die folgende Abbildung zeigt einen Cluster mit den beiden Knotentypen FrontEnd und BackEnd. Jeder Knotentyp verfügt über fünf Knoten.

![Ein Cluster mit zwei Knotentypen][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Zuordnen von VM-Skalierungsgruppeninstanzen zu Knoten
Wie in der Abbildung oben gezeigt, beginnen Skalierungsgruppeninstanzen mit der Instanz 0. Die Instanznummer wird dann jeweils um 1 erhöht. Die Knotennamen spiegeln die Nummerierung wider. Der Knoten „BackEnd_0“ ist z.B. die Instanz 0 der Skalierungsgruppe „BackEnd“. Diese bestimmte Skalierungsgruppe hat fünf Instanzen mit den Namen „BackEnd_0“, „BackEnd_1“, „BackEnd_2“, „BackEnd_3“ und „BackEnd_4“.

Wenn Sie eine Skalierungsgruppe zentral hochskalieren, wird eine neue Instanz erstellt. Der neue Skalierungsgruppen-Instanzname hat in der Regel folgendes Format: Skalierungsgruppenname + Nummer der nächsten Instanz. In diesem Beispiel lautet er „BackEnd_5“.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Zuordnen des Lastenausgleichs von Skalierungsgruppen zu Knotentypen und Skalierungsgruppen
Wenn Sie Ihren Cluster aus dem Azure-Portal bereitgestellt oder die Azure Resource Manager-Beispielvorlage verwendet haben, werden alle Ressourcen aufgelistet, die zu einer Ressourcengruppe gehören. Der Lastenausgleich jeder Skalierungsgruppe oder jedes Knotentyps wird angezeigt. Für den Namen des Lastenausgleichs wird folgendes Format verwendet: **LB-&lt;Knotentypname&gt;** . Ein Beispiel ist „LB-sfcluster4doc-0“, wie in der folgenden Abbildung gezeigt:

![Ressourcen][Resources]

## <a name="service-fabric-virtual-machine-extension"></a>Service Fabric Virtual Machine-Erweiterung
Die Service Fabric Virtual Machine-Erweiterung wird zum Starten von Service Fabric in Azure Virtual Machines und zum Konfigurieren der Knotensicherheit verwendet.

Folgendes ist ein Codeausschnitt aus der Service Fabric Virtual Machine-Erweiterung:

```json
"extensions": [
  {
    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
    "properties": {
      "type": "ServiceFabricLinuxNode",
      "autoUpgradeMinorVersion": true,
      "protectedSettings": {
        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
       },
       "publisher": "Microsoft.Azure.ServiceFabric",
       "settings": {
         "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
         "nodeTypeRef": "[variables('vmNodeType0Name')]",
         "durabilityLevel": "Silver",
         "enableParallelJobs": true,
         "nicPrefixOverride": "[variables('subnet0Prefix')]",
         "certificate": {
           "commonNames": [
             "[parameters('certificateCommonName')]"
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
         }
       },
       "typeHandlerVersion": "1.1"
     }
   },
```

Dies sind die Beschreibungen der Eigenschaften:

| **Name** | **Zulässige Werte** | ** --- ** | **Anleitung oder Kurzbeschreibung** |
| --- | --- | --- | --- |
| name | Zeichenfolge | --- | Eindeutiger Name für die Erweiterung |
| type | „ServiceFabricLinuxNode“ oder „ServiceFabricWindowsNode“ | --- | Identifiziert das Betriebssystem, in dem Service Fabric gestartet wird. |
| autoUpgradeMinorVersion | "true" oder "false" | --- | Aktivieren des automatischen Upgrades von Nebenversionen der Service Fabric-Laufzeit |
| Herausgeber | Microsoft.Azure.ServiceFabric | --- | Name des Herausgebers der Service Fabric-Erweiterung |
| clusterEndpoint | Zeichenfolge | --- | URI:PORT zum Verwaltungsendpunkt |
| nodeTypeRef | Zeichenfolge | --- | Name von nodeType |
| durabilityLevel | Bronze, Silber, Gold, Platin | --- | Zulässige Zeitspanne zum Anhalten der unveränderliche Azure-Infrastruktur |
| enableParallelJobs | "true" oder "false" | --- | Aktivieren von Compute-ParallelJobs wie paralleles Entfernen und Neustarten von virtuellen Computern in der gleichen Skalierungsgruppe |
| nicPrefixOverride | Zeichenfolge | --- | Subnetzpräfix, z.B. „10.0.0.0/24“ |
| commonNames | string[] | --- | Allgemeine Namen von installierten Clusterzertifikaten |
| x509StoreName | Zeichenfolge | --- | Name des Speichers, in dem sich das installierte Clusterzertifikat befindet |
| typeHandlerVersion | 1.1 | --- | Version der Erweiterung. Für klassische Version 1.0 der Erweiterung wird Upgrade auf Version 1.1 empfohlen |

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen finden Sie unter [Übersicht über das Feature „Deploy Anywhere“ (umgebungsunabhängige Bereitstellung) und ein Vergleich mit Clustern, die von Azure verwaltet werden](service-fabric-deploy-anywhere.md).
* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* Stellen Sie eine [Remoteverbindung](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) mit der spezifischen Skalierungsgruppeninstanz her.
* [Aktualisieren der RDP-Portbereichswerte](./scripts/service-fabric-powershell-change-rdp-port-range.md) für Cluster-VMs nach der Bereitstellung
* [Ändern von Administratorbenutzername und -kennwort](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) für Cluster-VMs

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
