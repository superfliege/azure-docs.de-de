## <a name="load-balancer"></a>Lastenausgleich
Ein Load Balancer wird verwendet, wenn Sie Ihre Anwendungen zu skalieren möchten. Typische Szenarien umfassen Anwendungen, die auf mehrere VM-Instanzen ausgeführt. Die VM-Instanzen werden von einem Lastenausgleich Produktkatalogsystem, die hilft, um den Netzwerkdatenverkehr zu den verschiedenen Anwendungsinstanzen zu verteilen. 

![NIC auf einem einzelnen virtuellen Computer](./media/resource-groups-networking/figure8.png)

| Eigenschaft | Beschreibung |
| --- | --- |
| *frontendIPConfigurations* |ein Load Balancer kann eine oder mehrere front-End-IP-Adressen, auch bezeichnet als eine virtuelle IP-Adressen (VIPs) enthalten. Diese IP-Adressen können dienen als für den Datenverkehr eingehend und öffentliche IP-Adresse oder privaten IP- |
| *backendAddressPools* |Hierbei handelt es sich um virtuelle Computer-NICs, die an die Last verteilt wird zugeordneten IP-Adressen |
| *loadBalancingRules* |eine Regeleigenschaft ordnet eine angegebene Front-End-IP- und die Kombination aus Port auf einen Satz von Back-End-IP-Adressen und die Kombination aus Port. Mit einer einzelnen Definition einer Load Balancer-Ressource Sie können mehrere Load balancer-Regeln definieren, jede Regel eine Kombination von der Vorderseite reflektieren Ende IP-Adresse und Port und Sichern von End-IP- und Port virtuellen Computern zugeordnet werden sollen. Die Regel ist ein Port im front-End-Pool zu viele virtuelle Computer im Back-End-pool |
| *Prüfpunkte* |Prüfpunkte ermöglichen Ihnen die Integrität des VM-Instanzen des. Wenn eine zustandsüberprüfung fehlschlägt, wird die virtuelle Computerinstanz aus der Rotation automatisch ausgeführt werden |
| *inboundNatRules* |NAT-Regeln, die den eingehenden Datenverkehr durch den Vordergrund fließen definieren IP zu beenden und an den Back-End-IP mit einer bestimmten virtuellen Maschine Instanz verteilt. NAT-Regel ist ein Port im front-End-Pool an einen virtuellen Computer im Back-End-pool |

Beispiel der Lastenausgleichsmodul-Vorlage im Json-Format:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "dnsNameforLBIP": {
          "type": "string",
          "metadata": {
            "description": "Unique DNS name"
          }
        },
        "location": {
          "type": "string",
          "allowedValues": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ],
          "metadata": {
            "description": "Location to deploy"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address Prefix"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/24",
          "metadata": {
            "description": "Subnet Prefix"
          }
        },
        "publicIPAddressType": {
          "type": "string",
          "defaultValue": "Dynamic",
          "allowedValues": [
            "Dynamic",
            "Static"
          ],
          "metadata": {
            "description": "Public IP type"
          }
        }
      },
      "variables": {
        "virtualNetworkName": "virtualNetwork1",
        "publicIPAddressName": "publicIp1",
        "subnetName": "subnet1",
        "loadBalancerName": "loadBalancer1",
        "nicName": "networkInterface1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "backEndIPConfigID": "[concat(variables('nicId'),'/ipConfigurations/ipconfig1')]"
      },
      "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsNameforLBIP')]"
        }
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[parameters('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(variables('lbID'), '/backendAddressPools/LoadBalancerBackend')]"
                }
              ],
              "loadBalancerInboundNatRules": [
                {
                  "id": "[concat(variables('lbID'),'/inboundNatRules/RDP')]"
                }
              ]
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[variables('publicIPAddressID')]"
              }
            }
          }
        ],
        "backendAddressPools": [
          {
            "name": "loadBalancerBackEnd"
          }
        ],
        "inboundNatRules": [
          {
            "name": "RDP",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPort": 3389,
              "backendPort": 3389,
              "enableFloatingIP": false
            }
          }
        ]
      }
    }
      ]
    }

### <a name="additional-resources"></a>Zusätzliche Ressourcen
Lesen [den load Balancer-REST-API](https://msdn.microsoft.com/library/azure/mt163651.aspx) für Weitere Informationen.

