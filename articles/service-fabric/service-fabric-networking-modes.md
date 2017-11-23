---
title: "Konfigurieren von Netzwerkmodi für Azure Service Fabric-Containerdienste | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie die verschiedenen Netzwerkmodi einrichten, die Azure Service Fabric unterstützt."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 855e315f66858210875039f91f7f05055ff7d9b9
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="service-fabric-container-networking-modes"></a>Netzwerkmodi für Service Fabric-Container

Im Service Fabric-Cluster für Containerdienste wird standardmäßig der Netzwerkmodus `nat` verwendet. Wenn im Netzwerkmodus `nat` mehrere Containerdienste am gleichen Port lauschen, kommt es zu Bereitstellungsfehlern. Für die Ausführung mehrerer Dienste, die am gleichen Port lauschen, unterstützt Service Fabric den Netzwerkmodus `Open` (ab Version 5.7). Bei Verwendung des Netzwerkmodus `Open` wird jeder Containerdienst intern mit einer dynamisch zugewiesenen IP-Adresse versehen, sodass mehrere Dienste am gleichen Port lauschen können.   

Bei Verwendung eines einzelnen Diensttyps mit einem statischen, im Dienstmanifest definierten Endpunkt können daher im Netzwerkmodus `Open` neue Dienste ohne Bereitstellungsfehler erstellt und gelöscht werden. Analog dazu kann die gleiche Datei vom Typ `docker-compose.yml` mit statischen Portzuordnungen für die Erstellung mehrerer Dienste verwendet werden.

Die Ermittlung von Diensten anhand der dynamisch zugewiesenen IP-Adresse ist nicht ratsam, da sich die IP-Adresse ändert, wenn der Dienst neu gestartet oder an einen anderen Knoten verschoben wird. Verwenden Sie zur Dienstermittlung nur den **Service Fabric Naming Service** oder den **DNS-Dienst**. 


> [!WARNING]
> Pro VNET sind in Azure maximal 4.096 IP-Adressen zulässig. Innerhalb eines VNETs darf daher die Summe der Knoten und Containerdienstinstanzen (mit dem Netzwerkmodus `Open`) 4.096 nicht überschreiten. In solchen Szenarien mit hoher Dichte empfiehlt sich die Verwendung des Netzwerkmodus `nat`.
>

## <a name="setting-up-open-networking-mode"></a>Einrichten des Netzwerkmodus „Open“

1. Richten Sie die Azure Resource Manager-Vorlage ein, indem Sie den DNS-Dienst und den IP-Anbieter unter `fabricSettings` aktivieren. 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. Richten Sie den Netzwerkprofilabschnitt so ein, dass für jeden Knoten des Clusters mehrere IP-Adressen konfiguriert werden können. Im folgenden Beispiel werden für einen Windows/Linux-basierten Service Fabric-Cluster fünf IP-Adressen pro Knoten eingerichtet. (Pro Knoten können also fünf Dienstinstanzen am Port lauschen.)

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
   ```
 

3. Richten Sie für Windows-Cluster eine NSG-Regel ein, um den Port „UDP/53“ für das VNET zu öffnen. Verwenden Sie dazu folgende Werte:

   | Priority |    Name    |    Quelle      |  Ziel   |   Dienst    | Aktion |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     2000 | Custom_Dns | VirtualNetwork | VirtualNetwork | DNS (UDP/53) | ZULASSEN  |


4. Geben Sie im App-Manifest für jeden Dienst den Netzwerkmodus an: `<NetworkConfig NetworkType="Open">`.  Im Modus `Open` erhält der Dienst eine dedizierte IP-Adresse. Ohne Modusangabe wird standardmäßig der einfache Modus `nat` verwendet. Im folgenden Beispielmanifest können `NodeContainerServicePackage1` und `NodeContainerServicePackage2` am gleichen Port lauschen. (Beide Dienste lauschen an `Endpoint1`.) Bei Angabe des Netzwerkmodus `Open` können keine `PortBinding`-Konfigurationen angegeben werden.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```
Sie können verschiedene Netzwerkmodi dienstübergreifend in einer Anwendung für einen Windows-Cluster miteinander kombinieren. So können einige Dienste im Netzwerkmodus `Open` und andere im Modus `nat` betrieben werden. Wenn ein Dienst mit `nat` konfiguriert ist, muss der Port, an dem gelauscht wird, eindeutig sein. Bei Linux-Clustern wird die Verwendung unterschiedlicher Netzwerkmodi für verschiedene Dienste nicht unterstützt. 


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurden Netzwerkmodi von Service Fabric behandelt.  

* [Service Fabric-Anwendungsmodell](service-fabric-application-model.md)
* [Service Fabric service manifest resources](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-manifest-resources) (Service Fabric-Dienstmanifestressourcen)
* [Bereitstellen eines Windows-Containers in Service Fabric unter Windows Server 2016](service-fabric-get-started-containers.md)
* [Bereitstellen eines Docker-Containers in Service Fabric unter Linux](service-fabric-get-started-containers-linux.md)
