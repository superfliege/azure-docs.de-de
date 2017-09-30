---
title: Konfigurieren der Dienstendpunkte von virtuellen Azure-Netzwerken | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie Dienstendpunkte über Ihr virtuelles Netzwerk aktivieren und deaktivieren."
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: anithaa
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 9c953e697d5d1c6e53df8eaf72c4e4fbd83ca286
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="configure-virtual-network-service-endpoints"></a>Konfigurieren der Dienstendpunkte von virtuellen Netzwerken

Mit Dienstendpunkten von virtuellen Netzwerken können Sie Azure-Dienstressourcen in Ihrem virtuellen Azure-Netzwerk schützen, indem Sie den Zugriff auf diese Ressourcen aus dem Internet vollständig verhindern. Dienstendpunkte ermöglichen eine direkte Verbindung aus Ihrem virtuellen Netzwerk mit einem Azure-Dienst, sodass Sie den privaten Adressraum Ihres VNET nutzen können, um auf die Azure-Dienste zuzugreifen. Datenverkehr, der über Dienstendpunkte an Azure-Dienste geleitet werden soll, fließt nur im Microsoft Azure-Backbone-Netzwerk. Informieren Sie sich über [Dienstendpunkte von virtuellen Netzwerken](virtual-network-service-endpoints-overview.md).

Dieser Artikel enthält die Schritte zum Aktivieren und Deaktivieren von Dienstendpunkten. Nachdem die Endpunkte in einem Subnetz für einen Azure-Dienst aktiviert wurden, können Sie bestimmte Dienstressourcen in einem virtuellen Netzwerk schützen.

Sie können Dienstendpunkte mit dem [Azure-Portal](#azure-portal), [Azure PowerShell](#azure-powershell), der [Azure-Befehlszeilenschnittstelle](#azure-cli) oder einer Azure Resource Manager-[Vorlage](#resource-manager-template) konfigurieren.

>[!NOTE]
Während der Vorschauphase wird das Feature für VNET-Dienstendpunkte für bestimmte Regionen unterstützt. Eine Liste mit den unterstützten Regionen finden Sie auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="service-endpoint-configuration-overview"></a>Übersicht über die Konfiguration von Dienstendpunkten

- Dienstendpunkte können nur in VNETs konfiguriert werden, die über das Azure Resource Manager-Bereitstellungsmodell bereitgestellt wurden.

- Dienstendpunkte werden jeweils in allen Subnetzen eines VNET festgelegt.

- Für ein Subnetz können Sie nur einen Dienstendpunkt für einen Dienst konfigurieren. Sie können mehrere Dienstendpunkte für verschiedene Dienste (z.B. Azure Storage, Azure SQL) konfigurieren.

- Sie können die Endpunkte in einem neuen oder vorhandenen Subnetz aktivieren.

- Der Standort wird für einen Endpunkt automatisch konfiguriert. Standardmäßig werden Dienstendpunkte für die VNET-Region konfiguriert. Damit Azure Storage Szenarien für regionale Failover unterstützt, werden Endpunkte automatisch für [Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions) konfiguriert.

  >[!NOTE]
  Je nach Größe des VNET bzw. Subnetzes kann das Aktivieren des Dienstendpunkts einige Zeit dauern. Achten Sie darauf, dass keine kritischen Aufgaben durchgeführt werden, während die Dienstendpunkte aktiviert werden. Dienstendpunkte wechseln die Routen an jeder NIC Ihres Subnetzes und beenden ggf. alle geöffneten TCP-Verbindungen. 

- Für den Aufruf des Dienstendpunkts wird „Succeeded“ (Erfolgreich) zurückgegeben, nachdem die Datenverkehrsflüsse an den Dienst auf allen NICs im Subnetz auf private IP-Adressen des VNET umgestellt wurden.

- Effektive Routen zum Überprüfen der Endpunktkonfiguration:

   Um zu überprüfen, ob der Dienstendpunkt richtig konfiguriert ist, wird für die „effektiven Routen“ einer NIC im Subnetz jeweils eine neue Standardroute mit „nextHopType: VirtualNetworkServiceEndpoint“ angezeigt (pro Dienst und pro Region). Informieren Sie sich über das [Verwenden von effektiven Routen zur Problembehandlung des Datenverkehrsflusses auf virtuellen Computern](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#using-effective-routes-to-troubleshoot-vm-traffic-flow).

   >[!NOTE]
   Effektive Routen können nur angezeigt werden, wenn Sie mindestens eine Netzwerkschnittstelle (NIC) konfiguriert und einem aktiven virtuellen Computer im Subnetz zugeordnet haben.

## <a name="azure-portal"></a>Azure-Portal

### <a name="setting-up-service-endpoint-on-a-subnet-during-vnet-create"></a>Einrichten des Dienstendpunkts in einem Subnetz während der VNET-Erstellung

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).
Melden Sie sich mit Ihrem Azure-Konto an Azure an. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie sich für eine kostenlose Testversion registrieren. Das Konto muss über die [Berechtigungen](#provisioning) zum Erstellen eines virtuellen Netzwerks und Dienstendpunkts verfügen.
2. Klicken Sie auf „+Neu“ > „Netzwerk“ > „Virtuelles Netzwerk“ > „+Hinzufügen“.
3. Geben Sie unter „Virtuelles Netzwerk erstellen“ die folgenden Werte ein, und klicken Sie anschließend auf „Erstellen“:

Einstellung | Wert
------- | -----
Name    | myVnet
Adressraum | 10.0.0.0/16
Subnetzname|mySubnet
Subnetzadressbereich|10.0.0.0/24
Ressourcengruppe|Lassen Sie „Neu erstellen“ ausgewählt, und geben Sie einen Namen ein.
Standort|Alle unterstützten Regionen, z.B. Australien, Osten
Abonnement|Wählen Sie Ihr Abonnement aus.
__ServiceEndpoints__|Aktiviert
__Dienste__ | Wählen Sie einen oder alle verfügbaren Dienste aus. Unterstützte Dienste in der Vorschauversion: __„Microsoft.Storage“, „Microsoft.Sql“__.

Wählen Sie die Dienste für Endpunkte aus: ![Auswählen von Diensten für Dienstendpunkte](media/virtual-network-service-endpoints-portal/vnet-create-flow-services.png)

4. Überprüfen Sie, ob alle Einstellungen richtig sind, und klicken Sie auf „Erstellen“.

![Festlegen des Dienstendpunkts](media/virtual-network-service-endpoints-portal/create-vnet-flow.png)

5. Klicken Sie unter [Nächste Schritte](#Next%20Steps) auf die Dienstdokumentation, um den Vorgang zum Schützen der Ressourcen von Azure-Diensten in Ihrem VNET abzuschließen.


### <a name="validating-service-endpoint-configuration"></a>Überprüfen der Konfiguration von Dienstendpunkten

Führen Sie die folgenden Schritte aus, um zu bestätigen, dass die Dienstendpunkte richtig konfiguriert sind:

- Klicken Sie für die Ressourcen auf „Virtuelle Netzwerke“. Suchen Sie nach dem VNET.
- Klicken Sie auf den Namen des VNET, und navigieren Sie zu „Dienstendpunkte“.
- Für konfigurierte Endpunkte wird „Succeeded“ (Erfolgreich) angezeigt. Es werden auch die automatisch konfigurierten Standorte angezeigt.

![Bestätigen der Konfiguration von Dienstendpunkten](media/virtual-network-service-endpoints-portal/vnet-validate-settings.png
)

### <a name="effective-routes-to-validate-endpoint-configuration"></a>Effektive Routen zum Überprüfen der Endpunktkonfiguration

Klicken Sie im Subnetz auf eine beliebige NIC, um die effektive Route für eine Netzwerkschnittstelle (NIC) des Subnetzes anzuzeigen. Klicken Sie unter „Support und Problembehandlung“ auf „Effektive Routen“. Nachdem der Endpunkt konfiguriert wurde, wird eine neue Standardroute mit Adresspräfixen des Diensts als Ziel angezeigt, und für „nextHopType“ ist „VirtualNetworkServiceEndpoint“ angegeben.

![Effektive Routen für Dienstendpunkte](media/virtual-network-service-endpoints-portal/effective-routes.png)

### <a name="setting-up-service-endpoints-for-existing-subnets-in-a-vnet"></a>Einrichten von Dienstendpunkten für vorhandene Subnetze in einem VNET

1. Klicken Sie für Ressourcen auf „Virtuelle Netzwerke“, und suchen Sie nach vorhandenen VNETs.
2. Klicken Sie auf den Namen des VNET, und navigieren Sie zu „Dienstendpunkte“.
3. Klicken Sie auf „Hinzufügen“. Wählen Sie die Option „Dienst“. Sie können einen Endpunkt nur für jeweils einen Dienst erstellen. 
4. Wählen Sie alle Subnetze aus, in denen Sie den Endpunkt anwenden möchten. Klicken Sie auf „Hinzufügen“.

![Konfiguration von Dienstendpunkten im Subnetz](media/virtual-network-service-endpoints-portal/existing-subnet.png)

### <a name="deleting-service-endpoints"></a>Löschen von Dienstendpunkten
1. Klicken Sie für die Ressourcen auf „Virtuelle Netzwerke“. Suchen Sie nach einem vorhandenen VNET, indem Sie nach dem VNET-Namen filtern.
2. Klicken Sie auf den Namen des VNET, und navigieren Sie zu „Dienstendpunkte“.
3. Klicken Sie auf den Dienstnamen und dann mit der rechten Maustaste auf den Eintrag des Dienstendpunkts.
4. Wählen Sie „Löschen“.

![Löschen des Dienstendpunkts](media/virtual-network-service-endpoints-portal/delete-endpoint.png)

## <a name="azure-powershell"></a>Azure Powershell

Voraussetzungen für die Einrichtung:

- Installieren Sie die neueste Version des PowerShell-Moduls [AzureRm](https://www.powershellgallery.com/packages/AzureRM/). Wenn Sie noch nicht mit Azure PowerShell vertraut sind, lesen Sie die [Übersicht über Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Um eine PowerShell-Sitzung zu starten, wechseln Sie zu **Start**, geben Sie **PowerShell** ein, und klicken Sie dann auf **PowerShell**.
- Melden Sie sich in PowerShell bei Azure an, indem Sie den Befehl `login-azurermaccount` eingeben. Das Konto muss über die [Berechtigungen](#provisioning) zum Erstellen eines virtuellen Netzwerks und Dienstendpunkts verfügen.

### <a name="get-available-service-endpoints-for-azure-region"></a>Abrufen der verfügbaren Dienstendpunkte für die Azure-Region

Verwenden Sie den unten angegebenen Befehl, um die Liste mit den Diensten abzurufen, die für Endpunkte für eine Azure-Region unterstützt werden.
 ```powershell
Get-AzureRmVirtualNetworkAvailableEndpointService -location eastus
```

Ausgabe: 
Name | ID | Typ
-----|----|-------
Microsoft.Storage|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage|Microsoft.Network/virtualNetworkEndpointServices
Microsoft.Sql|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql|Microsoft.Network/virtualNetworkEndpointServices

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Hinzufügen eines Azure Storage-Dienstendpunkts zum Subnetz *mySubnet* beim Erstellen des virtuellen Netzwerks *myVNet*

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix "10.0.1.0/24" -ServiceEndpoint “Microsoft.Storage”

New-AzureRmVirtualNetwork -Name "myVNet" -AddressPrefix "10.0.0.0/16" -Subnet $subnet -ResourceGroupName "myRG" -Location "WestUS"
```
Sie können mehrere Dienste aktivieren, indem Sie eine kommagetrennte Liste mit Dienstnamen verwenden.
Beispiel: „Microsoft.Storage“, „Microsoft.Sql“

Erwartete Ausgabe:
```
Subnets : [
            {
            "Name": "mySubnet",
             ...
            "ServiceEndpoints": [
              {
                   "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "westus",
                        "eastus"
                                 ]
               }
                                ],
            "ProvisioningState": "Succeeded"
            }
          ]
```

Klicken Sie unter [Nächste Schritte](#Next%20Steps) auf die Dienstdokumentation, um den Vorgang zum Schützen der Ressourcen von Azure-Diensten in Ihrem VNET abzuschließen.

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Hinzufügen von mehreren Dienstendpunkten zu einem vorhandenen Subnetz

```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint "Microsoft.Storage", "Microsoft.Sql" | Set-AzureRmVirtualNetwork
```

Erwartete Ausgabe: 
```
Subnets : [
            {
                "Name": "mySubnet",
                 ...
                "ServiceEndpoints": [
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "eastus",
                        "westus"
                                 ]
                },
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Sql",
                    "Locations": [
                        "eastus"
                                 ]
                }
                ],
               "ProvisioningState": "Succeeded"
            }
         ]
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Anzeigen von konfigurierten Dienstendpunkten für ein Subnetz

```powershell
$subnet=Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"
$subnet.ServiceEndpoints
```
Ausgabe:
```
ProvisioningState Service           Locations
----------------- -------           ---------
Succeeded         Microsoft.Storage {eastus, westus}
Succeeded         Microsoft.Sql     {eastus}
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Löschen von Dienstendpunkten in einem Subnetz
```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint $null | Set-AzureRmVirtualNetwork
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Voraussetzungen für die Einrichtung:
- Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. Weitere Informationen zur Protokollierung finden Sie unter [Get Started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) (Erste Schritte mit Azure CLI 2.0).
 - Das Konto muss über die [Berechtigungen](#provisioning) zum Erstellen eines virtuellen Netzwerks und Dienstendpunkts verfügen.

 Eine vollständige Liste mit Befehlen für virtuelle Netzwerke finden Sie auf der Seite mit den [Azure CLI-Befehlen für virtuelle Netzwerke](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest).

### <a name="get-available-service-endpoints-for-azure-region"></a>Abrufen der verfügbaren Dienstendpunkte für die Azure-Region

Verwenden Sie den unten angegebenen Befehl, um die Liste mit den Diensten abzurufen, die für Endpunkte für eine Azure-Region unterstützt werden, z.B. „EastUS“.
```azure-cli
az network vnet list-endpoint-services -l eastus
```
Ausgabe:
```
    {
    "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage",
    "name": "Microsoft.Storage",
    "type": "Microsoft.Network/virtualNetworkEndpointServices"
     },
     {
     "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql",
     "name": "Microsoft.Sql",
     "type":   "Microsoft.Network/virtualNetworkEndpointServices"
     }
```

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Hinzufügen eines Azure Storage-Dienstendpunkts zum Subnetz *mySubnet* beim Erstellen des virtuellen Netzwerks *myVNet*

```azure-cli
az network vnet create -g myRG -n myVNet --address-prefixes 10.0.0.0/16 -l eastUS

az network vnet subnet create -g myRG -n mySubnet --vnet-name myVNet --address-prefix 10.0.1.0/24 --service-endpoints Microsoft.Storage
```

Fügen Sie mehrere Endpunkte wie folgt hinzu: --service-endpoints Microsoft.Storage Microsoft.Sql

Ausgabe:
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    }
  ]
}
```

Klicken Sie unter [Nächste Schritte](#Next%20Steps) auf die Dienstdokumentation, um den Vorgang zum Schützen der Ressourcen von Azure-Diensten in Ihrem VNET abzuschließen.

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Hinzufügen von mehreren Dienstendpunkten zu einem vorhandenen Subnetz

```azure-cli
az network vnet subnet update -g myRG -n mySubnet2 --vnet-name myVNet --service-endpoints Microsoft.Storage Microsoft.Sql
```

Erwartete Ausgabe:
```
{
  "addressPrefix": "10.0.2.0/24",
  ...
  "name": "mySubnet2",
  ...
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    },
    {
      "locations": [
        "eastus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Sql"
    }
  ]
}
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Anzeigen von konfigurierten Dienstendpunkten für ein Subnetz

```azure-cli
az network vnet subnet show -g myRG -n mySubnet --vnet-name myVNet
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Löschen von Dienstendpunkten in einem Subnetz
```azure-cli
az network vnet subnet update -g myRG -n mySubnet --vnet-name myVNet --service-endpoints ""
```

Ausgabe: 
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": null
}
```

## <a name="resource-manager-template"></a>Resource Manager-Vorlage

### <a name="securing-azure-service-resources-to-vnets"></a>Schützen der Ressourcen von Azure-Diensten in VNETs

Sie können bestimmte Azure-Ressourcen über Dienstendpunkte in Ihrem virtuellen Netzwerk schützen.

Laden Sie die [Resource Manager-Beispielvorlage](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) herunter, um ein Speicherkonto in einem Subnetz eines VNET zu schützen.

Mit der Vorlage wird ein VNET mit zwei Subnetzen erstellt, wobei in jedem Subnetz jeweils eine VM mit einer NIC enthalten ist. Der Endpunkt für ein Subnetz wird aktiviert, und ein Speicherkonto wird in diesem Subnetz geschützt.

Sie können die Vorlage herunterladen und Teile davon an Ihr Szenario anpassen.

Mit der Vorlage wird eine Anleitung für Ihre Bereitstellung über das Azure-Portal, PowerShell oder die Azure-CLI geliefert. Stellen Sie sicher, dass Sie über die erforderlichen [Berechtigungen](#provisioning) zum Einrichten des Endpunkts und Schützen des Kontos verfügen.

Gehen Sie wie folgt vor, um Azure-Ressourcen in einem Subnetz zu schützen:

- In diesem Subnetz sollte ein Dienstendpunkt konfiguriert werden.
- Die Ressource sollte im VNET geschützt werden, indem für die Ressource ein virtuelles Netzwerk hinzugefügt wird.

### <a name="deleting-service-endpoints-with-resources-secured-to-the-subnet"></a>Löschen von Dienstendpunkten mit im Subnetz geschützten Ressourcen
Wenn Ressourcen von Azure-Diensten im Subnetz geschützt sind und der Dienstendpunkt gelöscht wird, können Sie über das Subnetz nicht auf die Ressource zugreifen.
Allein durch das erneute Aktivieren des Endpunkts kann der Zugriff auf die Ressourcen, die bisher im Subnetz geschützt waren, nicht wiederhergestellt werden.

Folgende Schritte sind erforderlich, um die Dienstressource wieder für das Subnetz zu schützen:

- Erneutes Aktivieren des Endpunkts
- Entfernen der alten VNET-Regel für die Ressource
- Hinzufügen einer neuen Regel zum Schützen der Ressource im Subnetz

## <a name="provisioning"></a>Bereitstellung

Dienstendpunkte können in virtuellen Netzwerken einzeln von einem Benutzer konfiguriert werden, der über Schreibzugriff auf das virtuelle Netzwerk verfügt.

Zum Schützen der Ressourcen von Azure-Diensten in einem VNET muss der Benutzer die Berechtigung „Microsoft.Network/JoinServicetoaSubnet“ für die hinzuzufügenden Subnetze haben. Diese Berechtigung ist standardmäßig in die integrierten Dienstadministratorrollen integriert und kann durch die Erstellung von benutzerdefinierten Rollen geändert werden.

Erfahren Sie mehr über [integrierte Rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles).

VNETs und Ressourcen von Azure-Diensten können sich in demselben oder in unterschiedlichen Abonnements befinden. Falls sie sich in unterschiedlichen Abonnements befinden, sollten die Ressourcen im Rahmen dieser Vorschauversion unter demselben Active Directory-Mandanten vorhanden sein.

## <a name="next-steps"></a>Nächste Schritte

Weitere Anleitungen zum Schützen der Dienstressource in VNETs finden Sie unter den folgenden Links:

[Securing Azure Storage accounts to Virtual Networks](https://docs.microsoft.com/azure/storage/common/storage-network-security) (Schützen von Azure Storage-Konten in virtuellen Netzwerken)

[Securing Azure SQL to Virtual networks](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview) (Schützen von Azure SQL in virtuellen Netzwerken)

