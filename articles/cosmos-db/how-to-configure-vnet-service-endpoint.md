---
title: Konfigurieren des VNET- und subnetzbasierten Zugriffs für Ihr Azure Cosmos-Konto
description: Dieses Dokument beschreibt die Schritte, die zur Einrichtung eines Virtual Network-Dienstendpunkts für Azure Cosmos DB erforderlich sind.
author: kanshiG
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: a7c2d1e41fa4ac26854e2e6ab57184cd6ed0bd0c
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633681"
---
# <a name="how-to-access-azure-cosmos-db-resources-from-virtual-networks"></a>Zugreifen auf Azure Cosmos DB-Ressourcen über virtuelle Netzwerke

Azure Cosmos DB-Konten können so konfiguriert werden, dass nur der Zugriff aus bestimmten Azure Virtual Network-Subnetzen zugelassen wird. Um den Zugriff auf das Azure Cosmos-Konto mit Verbindungen aus einem Subnetz in einem virtuellen Netzwerk (VNET) einzuschränken, sind zwei Schritte erforderlich.
 
1. Aktivieren Sie das Subnetz, um die Subnetz- und VNET-Identität an Azure Cosmos DB zu senden. Aktivieren Sie dazu den Dienstendpunkt für Azure Cosmos DB im entsprechenden Subnetz.

1. Fügen Sie im Azure Cosmos DB-Konto eine Regel hinzu, die dieses Subnetz als eine Quelle für den Zugriff auf das Konto angibt.

> [!NOTE]
> Wenn der Dienstendpunkt für Azure Cosmos-Konto in einem Subnetz aktiviert ist, wechselt die Quelle des Datenverkehrs, der Azure Cosmos DB erreicht, von der öffentlichen IP-Adresse zu einem VNET und Subnetz. Der Wechsel des Datenverkehrs gilt für jedes Azure Cosmos-Konto, auf das über dieses Subnetz zugegriffen wird. Wenn Ihre Azure Cosmos-Konten über eine IP-basierte Firewall verfügen, um dieses Subnetz zuzulassen, würden Anfragen von vom Dienst aktivierten Subnetzen nicht mehr den Regeln der IP-Firewall entsprechen und sie werden abgelehnt. Weitere Informationen finden Sie in den Schritten unter [Migrieren der IP-Firewallregel in die VNET-Zugriffssteuerungsliste](#migrate-from-firewall-to-vnet) in diesem Artikel. 

Die folgenden Abschnitte beschreiben, wie Sie den VNET-Dienstendpunkt für ein Azure Cosmos DB-Konto konfigurieren.

## <a id="configure-using-portal"></a>Konfigurieren des Dienstendpunkts über das Azure-Portal

### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Konfigurieren des Dienstendpunkts für ein vorhandenes Azure Virtual Network und Subnetz

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos-Konto, das Sie sichern möchten.

1. Wählen Sie aus dem Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke** und lassen Sie den Zugriff von **ausgewählten Netzwerken** zu.

1. Um Zugriff auf das Subnetz eines vorhandenen virtuellen Netzwerks zu gewähren, wählen Sie unter **Virtuelle Netzwerke** die Option **Vorhandenes Azure Virtual Network hinzufügen** aus.

1. Wählen Sie das **Abonnement** aus, aus dem das Azure Virtual Network hinzugefügt werden soll. Wählen Sie die Azure **Virtual Networks** und **Subnetze** aus, denen Sie Zugriff auf Ihr Azure Cosmos-Konto gewähren möchten. Klicken Sie als Nächstes auf **Aktivieren**, um die ausgewählten Netzwerke mit Dienstendpunkten für „Microsoft.AzureCosmosDB“ zu aktivieren. Wenn der Vorgang abgeschlossen ist, klicken Sie auf **Hinzufügen**. 

   ![Auswählen des Virtual Network und des Subnetzes](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)


1. Nachdem das Azure Cosmos-Konto für den Zugriff aus einem virtuellen Netzwerk aktiviert wurde, wird es nur den Datenverkehr aus diesem ausgewählten Subnetz zulassen. Das von Ihnen hinzugefügte virtuelle Netzwerk und das Subnetz sollten wie im folgenden Screenshot angezeigt werden:

   ![Virtual Network und Subnetz erfolgreich konfiguriert](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Um Endpunkte für virtuelle Netzwerkdienste zu aktivieren, benötigen Sie die folgenden Abonnementberechtigungen:
  * Abonnement mit VNET: Mitwirkender von virtuellem Netzwerk
  * Abonnement mit Azure Cosmos-Konto: DocumentDB-Kontomitwirkender

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Konfigurieren des Dienstendpunkts für ein neues Azure Virtual Network und Subnetz

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, das Sie sichern möchten.  

2. Bevor Sie den Virtual Network-Dienstendpunkt aktivieren, kopieren Sie die IP-Firewallinformationen für Ihr Azure Cosmos DB-Konto für die spätere Verwendung. Sie können die IP-Firewall nach der Konfiguration des Dienstendpunkts erneut aktivieren.  

3. Wählen Sie aus dem Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke** und lassen Sie den Zugriff von **ausgewählten Netzwerken** zu.  

4. Um Zugriff auf ein neues Azure Virtual Network zu gewähren, wählen Sie unter „Virtuelle Netzwerke“ die Option **Neues virtuelles Netzwerk hinzufügen** aus.  

5. Geben Sie die Informationen ein, die zum Erstellen des neuen Virtual Network erforderlich sind, und klicken Sie dann auf „Erstellen“. Das Subnetz wird mit einem für „Microsoft.AzureCosmosDB“ aktivierten Dienstendpunkt erstellt.

   ![Auswählen des Virtual Network und des Subnetzes für das neue Virtual Network](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Wenn Ihr Azure Cosmos-Konto von anderen Azure-Diensten wie z.B. Azure Search verwendet wird oder Stream Analytics oder Power BI darauf zugreifen, gewähren Sie den Zugriff, indem Sie das Kontrollkästchen „Zugriff auf Azure-Dienste zulassen“ aktivieren.

Um sicherzustellen, dass Sie vom Portal aus auf Azure Cosmos DB-Metriken zugreifen können, müssen Sie Optionen zum **Zugriff auf das Azure-Portal zulassen** aktivieren. Weitere Informationen zu diesen Optionen finden Sie in den Abschnitten „Anfragen aus dem Azure-Portal„ und „Anfragen aus den Azure-PaaS-Diensten“ im Artikel zum Konfigurieren der [IP-Firewall](how-to-configure-firewall.md). Nachdem Sie den Zugriff ausgewählt haben, klicken Sie auf **Speichern**, um die Einstellungen zu speichern.

## <a id="remove-vnet-or-subnet"></a>Entfernen eines virtuellen Netzwerks oder eines Subnetzes 

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, dem Sie Dienstendpunkte zugewiesen haben.  

2. Wählen Sie aus Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke** aus.  

3. Um eine Regel für ein Virtual Network oder ein Subnetz zu entfernen, klicken Sie neben dem Virtual Network oder dem Subnetz auf „...“, und wählen Sie **Entfernen** aus.

   ![Entfernen eines Virtual Network](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

4.  Klicken Sie zum Übernehmen der Änderungen auf **Speichern**.

## <a id="configure-using-powershell"></a>Konfigurieren des Dienstendpunkts über Azure PowerShell 

Führen Sie die folgenden Schritte aus, um über Azure PowerShell einen Dienstendpunkt für ein Azure Cosmos DB-Konto zu konfigurieren:  

1. Installieren Sie die neueste Version von [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps), und [melden Sie sich an](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Aktivieren Sie den Dienstendpunkt für ein vorhandenes Subnetz eines Virtual Network.  

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. Bereiten Sie die Aktivierung einer Zugriffssteuerungsliste (Access Control List, ACL) im Azure Cosmos-Konto vor, indem Sie sicherstellen, dass im virtuellen Netzwerk und im Subnetz ein Dienstendpunkt für Ihr Konto aktiviert ist.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

1. Rufen Sie mit dem folgenden Cmdlet die Eigenschaften des Azure Cosmos DB-Kontos ab:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Initialisieren Sie die Variablen für die spätere Verwendung. Richten Sie alle Variablen anhand der vorhandenen Kontodefinition ein. Wenn Sie über mehrere Standorte verfügen, müssen Sie diese im Rahme des Arrays hinzuzufügen. In diesem Schritt konfigurieren Sie auch den Virtual Network-Dienstendpunkt, indem Sie die Variable „accountVNETFilterEnabled“ auf „True“ festlegen. Dieser Wert wird später dem Parameter „isVirtualNetworkFilterEnabled“ zugewiesen. 

   ```powershell
   $locations = @(@{})

   <# If you have read regions in addition to a write region, use the following code to set the $locations variable instead.

   $locations = @(@{"locationName"="<Write location>"; 
                 "failoverPriority"=0}, 
               @{"locationName"="<Read location>"; 
                  "failoverPriority"=1}) #>

   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName

   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy

   $accountVNETFilterEnabled = $True
   $subnetID = $vnProp.Id+"/subnets/" + $sname  
   $virtualNetworkRules = @(@{"id"=$subnetID})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

1. Aktualisieren Sie die Azure Cosmos DB-Kontoeigenschaften mit der neuen Konfiguration, indem Sie die folgenden Cmdlets ausführen: 

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName -Properties $CosmosDBProperties
   ```

1. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Ihr Azure Cosmos DB-Konto mit dem Virtual Network-Dienstendpunkt aktualisiert wurde, das Sie im vorherigen Schritt konfiguriert haben:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a id="configure-using-cli"></a>Konfigurieren des Dienstendpunkts über die Azure CLI 

1. Aktivieren Sie den Dienstendpunkt für ein Subnetz in einem virtuellen Netzwerk.

1. Aktualisieren Sie ein vorhandenes Azure Cosmos-Konto mit den Subnetz-ACLs.

   ```azurecli-interactive

   name="<Azure Cosmos account name>"
   resourceGroupName="<Resource group name>"

   az cosmosdb update \
      --name $name \
    --resource-group $resourceGroupName \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/frontend"
   ```

1. Erstellen Sie ein neues Azure Cosmos-Konto mit Subnetz-ACLs

   ```azurecli-interactive
   az cosmosdb create \
    --name $name \
    --kind GlobalDocumentDB \
    --resource-group $resourceGroupName \
    --max-interval 10 \
    --max-staleness-prefix 200 \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/default"
   ```

## <a id="migrate-from-firewall-to-vnet"></a>Migrieren der IP-Firewallregel in die VNET-ACL 

Die folgenden Schritte sind nur für Azure Cosmos-Konten mit bestehenden IP-Firewallregeln erforderlich, die ein Subnetz zulassen, wenn Sie VNET- und subnetzbasierte ACLs anstelle der IP-Firewallregel verwenden möchten.

Sobald der Dienstendpunkt für das Azure Cosmos-Konto für ein Subnetz aktiviert ist, werden die Anforderungen mit einer Quelle gesendet, die VNET- und Subnetzinformationen anstelle von öffentlicher IP enthält. Daher entsprechen diese Anforderungen keinem IP-Filter. Dieser Quellenwechsel erfolgt für alle Azure Cosmos-Konten, auf die über das Subnetz mit aktiviertem Dienstendpunkt zugegriffen wird. Um Ausfallzeiten zu vermeiden, verwenden Sie die folgenden Schritte:

1. Rufen Sie mit dem folgenden Cmdlet die Eigenschaften des Azure Cosmos-Kontos ab:

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Initialisieren Sie die Variablen für die spätere Verwendung. Richten Sie alle Variablen aus der vorhandenen Kontodefinition ein. Fügen Sie die VNET-ACL zu allen Azure Cosmos-Konten hinzu, auf die über das Subnetz mit dem Flag `ignoreMissingVNetServiceEndpoint` zugegriffen wird.  

   Wenn Sie mehrere Standorte haben, müssen Sie diese als Teil des Arrays hinzufügen. In diesem Schritt konfigurieren Sie auch den Virtual Network-Dienstendpunkt, indem Sie die Variable „accountVNETFilterEnabled“ auf „True“ festlegen. Dieser Wert wird später dem Parameter „isVirtualNetworkFilterEnabled“ zugewiesen.

   ```powershell
   $locations = @(@{})

   <# If you have read regions in addition to a write region, use the following code to set the $locations variable instead.

   $locations = @(@{"locationName"="<Write location>"; 
              "failoverPriority"=0}, 
            @{"locationName"="<Read location>"; 
               "failoverPriority"=1}) #>

   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName
   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy
   $accountVNETFilterEnabled = $True
   $subnetID = “Subnet ARM URL” e.g "/subscriptions/f7ddba26-ab7b-4a36-a2fa-7d01778da30b/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/subnet1"

   $virtualNetworkRules = @(@{"id"=$subnetID, "ignoreMissingVNetServiceEndpoint"="True"})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

1. Aktualisieren Sie die Azure Cosmos-Kontoeigenschaften mit der neuen Konfiguration, indem Sie die folgenden Cmdlets ausführen:

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
    -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
    -ApiVersion $apiVersion `
    -ResourceGroupName $rgName `
    -Name $acctName -Properties $CosmosDBProperties
   ```

1. Wiederholen Sie die Schritte 1 bis 3 für alle Azure Cosmos-Konten, auf die Sie aus dem Subnetz zugreifen.

1.  Warten Sie 15 Minuten und aktualisieren Sie dann das Subnetz, um den Dienstendpunkt zu aktivieren.

1.  Aktivieren Sie den Dienstendpunkt für ein vorhandenes Subnetz eines Virtual Network.

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. Entfernen Sie die IP-Firewallregel für das Subnetz.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren einer Firewall für Azure Cosmos DB finden Sie im Artikel [Firewallunterstützung](firewall-support.md).

