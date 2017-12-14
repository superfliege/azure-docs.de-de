---
title: "Konfigurieren von Netzwerkmodi für Azure Service Fabric-Containerdienste | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie die verschiedenen Netzwerkmodi einrichten, die von Azure Service Fabric unterstützt werden."
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
ms.openlocfilehash: 1dacbbef915580b0095ef588f3dafad35daf1bde
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2017
---
# <a name="service-fabric-container-networking-modes"></a>Netzwerkmodi für Service Fabric-Container

Für einen Azure Service Fabric-Cluster für Containerdienste wird standardmäßig der Netzwerkmodus **nat** verwendet. Wenn mehr als ein Containerdienst über denselben Port lauscht und der Modus „nat“ verwendet wird, können Bereitstellungsfehler auftreten. Für die Unterstützung des Lauschens mehrerer Containerdienste über denselben Port verfügt Service Fabric über den Netzwerkmodus **Open** (Versionen 5.7 und höher). Im Modus „Open“ weist jeder Container eine interne, dynamisch zugewiesene IP-Adresse auf, mit der unterstützt wird, dass mehrere Dienste über denselben Port lauschen.  

Wenn in Ihrem Dienstmanifest ein Containerdienst mit einem statischen Endpunkt enthalten ist, können Sie neue Dienste erstellen und löschen, indem Sie den Modus „Open“ ohne Bereitstellungsfehler verwenden. Dieselbe Datei „docker-compose.yml“ kann auch mit statischen Portzuordnungen verwendet werden, um mehrere Dienste zu erstellen.

Wenn ein Containerdienst neu gestartet oder im Cluster auf einen anderen Knoten verschoben wird, ändert sich die IP-Adresse. Aus diesem Grund wird nicht empfohlen, die dynamisch zugewiesene IP-Adresse zum Ermitteln von Containerdiensten zu verwenden. Für die Dienstermittlung sollte nur der „Service Fabric Naming Service“ oder der „DNS-Dienst“ genutzt werden. 

>[!WARNING]
>In Azure sind maximal 4.096 IPs pro virtuellem Netzwerk zulässig. Die Summe aus Knotenanzahl und Anzahl von Containerdienstinstanzen (mit Verwendung des Modus „Open“) darf in einem virtuellen Netzwerk 4.096 IPs nicht übersteigen. Für Szenarien mit hoher Dichte ist es ratsam, den Netzwerkmodus „nat“ zu verwenden.
>

## <a name="set-up-open-networking-mode"></a>Einrichten des Netzwerkmodus „Open“

1. Richten Sie die Azure Resource Manager-Vorlage ein. Aktivieren Sie im Abschnitt **fabricSettings** den DNS-Dienst und den IP-Anbieter: 

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

2. Richten Sie den Netzwerkprofilabschnitt so ein, dass für jeden Knoten des Clusters mehrere IP-Adressen konfiguriert werden können. Im folgenden Beispiel werden für einen Windows/Linux Service Fabric-Cluster fünf IP-Adressen pro Knoten eingerichtet. Über den Port auf jedem Knoten können fünf Dienstinstanzen lauschen.

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
 
3. Richten Sie nur für Windows-Cluster eine Azure-Netzwerksicherheitsgruppen-Regel, mit der Port UDP/53 für das virtuelle Netzwerk geöffnet wird, mit den folgenden Werten ein:

   |Einstellung |Wert | |
   | --- | --- | --- |
   |Priority |2000 | |
   |Name |Custom_Dns  | |
   |Quelle |VirtualNetwork | |
   |Ziel | VirtualNetwork | |
   |Dienst | DNS (UDP/53) | |
   |Aktion | ZULASSEN  | |
   | | |

4. Geben Sie im Anwendungsmanifest für jeden Dienst den Netzwerkmodus an: `<NetworkConfig NetworkType="Open">`. Im Netzwerkmodus **Open** erhält der Dienst eine dedizierte IP-Adresse. Wenn kein Modus angegeben ist, wird für den Dienst standardmäßig der Modus **nat** verwendet. Im folgenden Beispielmanifest können die Dienste `NodeContainerServicePackage1` und `NodeContainerServicePackage2` über denselben Port lauschen. (Beide Dienste lauschen über `Endpoint1`.) Wenn der Netzwerkmodus „Open“ angegeben ist, können keine `PortBinding`-Konfigurationen angegeben werden.

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

    Sie können verschiedene Netzwerkmodi dienstübergreifend in einer Anwendung für einen Windows-Cluster miteinander kombinieren. Es ist möglich, dass einige Dienste den Modus „Open“ nutzen, während andere den Modus „nat“ verwenden. Wenn ein Dienst für die Verwendung des Modus „nat“ konfiguriert ist, muss der Port, über den der Dienst lauscht, eindeutig sein.

    >[!NOTE]
    >Bei Linux-Clustern wird die Verwendung unterschiedlicher Netzwerkmodi für verschiedene Dienste nicht unterstützt. 
    >

## <a name="next-steps"></a>Nächste Schritte
* [Modellieren von Anwendungen in Service Fabric](service-fabric-application-model.md)
* [Angeben von Ressourcen in einem Dienstmanifest](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources)
* [Bereitstellen eines Windows-Containers in Service Fabric unter Windows Server 2016](service-fabric-get-started-containers.md)
* [Bereitstellen eines Docker-Containers in Service Fabric unter Linux](service-fabric-get-started-containers-linux.md)
