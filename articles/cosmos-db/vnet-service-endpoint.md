---
title: Sicherer Zugriff auf ein Azure Cosmos DB-Konto durch Verwenden eines Azure Virtual Network-Dienstendpunkts | Microsoft-Dokumentation
description: Dieses Dokument beschreibt die Schritte, die zur Einrichtung eines Virtual Network-Dienstendpunkts für Azure Cosmos DB erforderlich sind.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: govindk
ms.openlocfilehash: de52521824c146f63fb16e2690e2a24167ae2efe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333911"
---
# <a name="secure-access-to-an-azure-cosmos-db-account-by-using-azure-virtual-network-service-endpoint"></a>Sicherer Zugriff auf ein Azure Cosmos DB-Konto durch Verwenden eines Azure Virtual Network-Dienstendpunkts

Azure Cosmos DB-Konten können so konfiguriert werden, dass nur der Zugriff aus bestimmten Azure Virtual Network-Subnetzen zugelassen wird. Durch Aktivieren eines [Dienstendpunkts](../virtual-network/virtual-network-service-endpoints-overview.md) für Azure Cosmos DB aus einem Virtual Netzwerk und den zugehörigen Subnetzen wird für den Datenverkehr eine optimale und sichere Route zur Azure Cosmos DB-Instanz sichergestellt.  

Azure Cosmos DB ist ein global verteilter Datenbankdienst mit Unterstützung mehrerer Modelle. Sie können die Daten, die im Azure Cosmos DB-Konto vorhanden sind, in mehrere Regionen replizieren. Wenn Azure Cosmos DB mit einem Virtual Network-Dienstendpunkt konfiguriert ist, erlaubt jedes Virtual Network Zugriff von IPs, die zu einem bestimmten Subnetz gehören. Die folgende Abbildung zeigt eine Azure Cosmos DB-Instanz, für die ein Virtual Network-Dienstendpunkt aktiviert wurde:

![Architektur eines Virtual Network-Dienstendpunkts](./media/vnet-service-endpoint/vnet-service-endpoint-architecture.png)

Nachdem ein Azure Cosmos DB-Konto mit einem Virtual Network-Dienstendpunkt konfiguriert wurde, kann nur vom angegebenen Subnetz aus darauf zugegriffen werden, jeglicher öffentliche Zugriff oder Zugriff aus dem Internet wird entfernt. Detaillierte Informationen zu Dienstendpunkten finden Sie im Artikel [Dienstendpunkte im virtuellen Netzwerk](../virtual-network/virtual-network-service-endpoints-overview.md).

> [!NOTE]
> Derzeit können Virtual Network-Dienstendpunkte für Azure Cosmos DB SQL-API- oder Mongo-API-Konten konfiguriert werden. Die Möglichkeit zur Konfiguration von Dienstendpunkten für andere APIs und unabhängige Clouds wie Azure Deutschland oder Azure Government wird bald verfügbar sein. Wenn für Ihr Azure Cosmos DB-Konto eine IP-Firewall konfiguriert ist, notieren Sie sich die Firewallkonfiguration, entfernen die IP-Firewall und konfigurieren die Zugriffssteuerungsliste (Access Control List, ACL) für den Dienstendpunkt. Nachdem Sie den Dienstendpunkt konfiguriert haben, können Sie die IP-Firewall bei Bedarf wieder aktivieren.

## <a name="configure-service-endpoint-by-using-azure-portal"></a>Konfigurieren des Dienstendpunkts über das Azure-Portal
### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Konfigurieren des Dienstendpunkts für ein vorhandenes Azure Virtual Network und Subnetz

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Virtual Network, für das Sie einen Dienstendpunkt für Azure Cosmos DB konfigurieren möchten. Navigieren Sie zum Blatt **Dienstendpunkte**, und stellen Sie sicher, dass das Subnetz des Virtual Network für den Dienstendpunkt „Azure.CosmosDB“ aktiviert wurde.  

   ![Bestätigen der Aktivierung des Dienstendpunkts](./media/vnet-service-endpoint/confirm-service-endpoint-enabled.png)

2. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, das Sie sichern möchten.  

3. Bevor Sie den Virtual Network-Dienstendpunkt aktivieren, kopieren Sie die IP-Firewallinformationen für Ihr Azure Cosmos DB-Konto für die spätere Verwendung. Sie können die IP-Firewall nach der Konfiguration des Dienstendpunkts erneut aktivieren.  

4. Wählen Sie aus dem Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke** und lassen Sie den Zugriff von **ausgewählten Netzwerken** zu.  

3. Um Zugriff auf das Subnetz eines vorhandenen Virtual Network zu gewähren, wählen Sie unter „Virtuelle Netzwerke“ die Option **Vorhandenes Azure Virtual Network hinzufügen** aus.  

4. Wählen Sie das **Abonnement** aus, aus dem das Azure Virtual Network hinzugefügt werden soll. Wählen Sie die Azure **Virtual Networks** und **Subnetze** aus, denen Sie Zugriff auf Ihr Azure Cosmos DB-Konto gewähren möchten. Klicken Sie als Nächstes auf **Aktivieren**, um die ausgewählten Netzwerke mit Dienstendpunkten für „Microsoft.AzureCosmosDB“ zu aktivieren. Wenn der Vorgang abgeschlossen ist, klicken Sie auf **Hinzufügen**.  

   ![Auswählen des Virtual Network und des Subnetzes](./media/vnet-service-endpoint/choose-subnet-and-vnet.png)

   > [!NOTE]
   > Wenn für die ausgewählten Azure Virtual Networks und Subnetze bisher noch kein Dienstendpunkt für Azure Cosmos DB konfiguriert wurde, kann dies im Rahmen dieses Vorgangs erfolgen. Die Aktivierung des Zugriffs kann bis zu 15 Minuten dauern. Es ist sehr wichtig, die IP-Firewall zu deaktivieren, nachdem Sie den Inhalt der Firewall-ACL für die erneute Aktivierung notiert haben. 

   ![Virtual Network und Subnetz erfolgreich konfiguriert](./media/vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

Nun lässt Ihr Azure Cosmos DB-Konto nur Zugriff aus diesem ausgewählten Subnetz zu. Wenn Sie zuvor eine IP-Firewall aktiviert hatten, aktivieren Sie diese erneut anhand der früheren Informationen.

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Konfigurieren des Dienstendpunkts für ein neues Azure Virtual Network und Subnetz

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, das Sie sichern möchten.  

> [!NOTE]
> Wenn für Ihr Azure Cosmos DB-Konto eine IP-Firewall konfiguriert ist, notieren Sie sich die Firewallkonfiguration, entfernen die IP-Firewall und aktivieren Sie dann den Dienstendpunkt. Wenn Sie den Dienstendpunkt aktivieren, ohne die Firewall zu deaktivieren, verliert der Datenverkehr aus diesem IP-Bereich die virtuelle IP-Identität und wird mit einer Fehlermeldung des IP-Filters gelöscht. Um diesen Fehler zu vermeiden, sollten Sie immer die Firewallregeln deaktivieren, sie kopieren, den Dienstendpunkt aus dem Subnetz aktivieren und danach die ACL des Subnetzes aus Cosmos DB. Nachdem Sie den Dienstendpunkt konfiguriert und die ACL hinzugefügt haben, können Sie die IP-Firewall bei Bedarf wieder aktivieren.

2. Bevor Sie den Virtual Network-Dienstendpunkt aktivieren, kopieren Sie die IP-Firewallinformationen für Ihr Azure Cosmos DB-Konto für die spätere Verwendung. Sie können die IP-Firewall nach der Konfiguration des Dienstendpunkts erneut aktivieren.  

3. Wählen Sie aus dem Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke** und lassen Sie den Zugriff von **ausgewählten Netzwerken** zu.  

4. Um Zugriff auf ein neues Azure Virtual Network zu gewähren, wählen Sie unter „Virtuelle Netzwerke“ die Option **Neues virtuelles Netzwerk hinzufügen** aus.  

5. Geben Sie die Informationen ein, die zum Erstellen des neuen Virtual Network erforderlich sind, und klicken Sie dann auf „Erstellen“. Das Subnetz wird mit einem für „Microsoft.AzureCosmosDB“ aktivierten Dienstendpunkt erstellt.

   ![Auswählen des Virtual Network und des Subnetzes für das neue Virtual Network](./media/vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

## <a name="allow-access-from-azure-portal"></a>Zulassen des Zugriffs vom Azure-Portal

Nachdem die Azure Virtual Network-Dienstendpunkte für Ihr Azure Cosmos DB-Datenbankkonto aktiviert wurden, ist der Zugriff vom Portal oder anderen Azure-Diensten standardmäßig deaktiviert. Der Zugriff auf Ihr Azure Cosmos DB-Datenbankkonto von Computern außerhalb des konfigurierten Subnetzes, einschließlich des Zugriffs vom Portal, ist blockiert.

![Zulassen des Zugriffs vom Portal](./media/vnet-service-endpoint/allow-access-from-portal.png)

Wenn Ihr Azure Cosmos DB-Konto von anderen Azure-Diensten wie z.B. Azure Search verwendet wird oder Stream Analytics oder Power BI darauf zugreifen, gewähren Sie den Zugriff, indem Sie das Kontrollkästchen **Zugriff auf Azure-Dienste zulassen** aktivieren.

Um sicherzustellen, dass Sie vom Portal aus auf Azure Cosmos DB-Metriken zugreifen können, müssen Sie Optionen zum **Zugriff auf das Azure-Portal zulassen** aktivieren. Weitere Informationen zu diesen Optionen finden Sie in den Abschnitten [Verbindungen über das Azure-Portal](firewall-support.md#connections-from-the-azure-portal) und [Verbindungen über andere Azure-PaaS-Dienste](firewall-support.md#connections-from-global-azure-datacenters-or-azure-paas-services). Nachdem Sie den Zugriff ausgewählt haben, klicken Sie auf **Speichern**, um die Einstellungen zu speichern.

## <a name="remove-a-virtual-network-or-subnet"></a>Entfernen eines Virtual Network oder eines Subnetzes 

1. Suchen Sie auf dem Blatt **Alle Ressourcen** das Azure Cosmos DB-Konto, dem Sie Dienstendpunkte zugewiesen haben.  

2. Wählen Sie aus Einstellungsmenü die Option **Firewalls und virtuelle Netzwerke** aus.  

3. Um eine Regel für ein Virtual Network oder ein Subnetz zu entfernen, klicken Sie neben dem Virtual Network oder dem Subnetz auf „...“, und wählen Sie **Entfernen** aus.

   ![Entfernen eines Virtual Network](./media/vnet-service-endpoint/remove-a-vnet.png)

4.  Klicken Sie zum Übernehmen der Änderungen auf **Speichern**.

## <a name="configure-service-endpoint-by-using-azure-powershell"></a>Konfigurieren des Dienstendpunkts über Azure PowerShell 

Führen Sie die folgenden Schritte aus, um über Azure PowerShell einen Dienstendpunkt für ein Azure Cosmos DB-Konto zu konfigurieren:  

1. Installieren Sie die neueste Version von [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps), und [melden Sie sich an](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  Stellen Sie sicher, dass Sie sich die IP-Firewalleinstellungen notiert und die IP-Firewall vollständig gelöscht haben, bevor Sie einen Dienstendpunkt für das Konto aktivieren.


> [!NOTE]
> Wenn für Ihr Azure Cosmos DB-Konto eine IP-Firewall konfiguriert ist, notieren Sie sich die Firewallkonfiguration, entfernen die IP-Firewall und aktivieren Sie dann den Dienstendpunkt. Wenn Sie den Dienstendpunkt aktivieren, ohne die Firewall zu deaktivieren, verliert der Datenverkehr aus diesem IP-Bereich die virtuelle IP-Identität und wird mit einer Fehlermeldung des IP-Filters gelöscht. Um diesen Fehler zu vermeiden, sollten Sie immer die Firewallregeln deaktivieren, sie kopieren, den Dienstendpunkt aus dem Subnetz aktivieren und danach die ACL des Subnetzes aus Cosmos DB. Nachdem Sie den Dienstendpunkt konfiguriert und die ACL hinzugefügt haben, können Sie die IP-Firewall bei Bedarf wieder aktivieren.

2. Bevor Sie den Virtual Network-Dienstendpunkt aktivieren, kopieren Sie die IP-Firewallinformationen für Ihr Azure Cosmos DB-Konto für die spätere Verwendung. Sie werden die IP-Firewall nach der Konfiguration des Dienstendpunkts erneut aktivieren.  

3. Aktivieren Sie den Dienstendpunkt für ein vorhandenes Subnetz eines Virtual Network.  

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

4. Bereiten Sie die Aktivierung einer Zugriffssteuerungsliste im Cosmos DB-Konto vor, indem Sie sicherstellen, dass im Virtual Network und im Subnetz ein Dienstendpunkt für Azure Cosmos DB aktiviert ist.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

5. Rufen Sie mit dem folgenden Cmdlet die Eigenschaften des Azure Cosmos DB-Kontos ab:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

6. Initialisieren Sie die Variablen für die spätere Verwendung. Richten Sie alle Variablen anhand der vorhandenen Kontodefinition ein. Wenn Sie über mehrere Standorte verfügen, müssen Sie diese im Rahme des Arrays hinzuzufügen. In diesem Schritt konfigurieren Sie auch den Virtual Network-Dienstendpunkt, indem Sie die Variable „accountVNETFilterEnabled“ auf „True“ festlegen. Dieser Wert wird später dem Parameter „isVirtualNetworkFilterEnabled“ zugewiesen.  

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

7. Aktualisieren Sie die Azure Cosmos DB-Kontoeigenschaften mit der neuen Konfiguration, indem Sie die folgenden Cmdlets ausführen: 

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName -Properties $CosmosDBProperties
   ```

8. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob Ihr Azure Cosmos DB-Konto mit dem Virtual Network-Dienstendpunkt aktualisiert wurde, das Sie im vorherigen Schritt konfiguriert haben:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a name="add-virtual-network-service-endpoint-for-an-azure-cosmos-db-account-that-has-ip-firewall-enabled"></a>Hinzufügen eines Virtual Network-Dienstendpunkts für ein Azure Cosmos DB-Konto mit aktivierter IP-Firewall

1. Deaktivieren Sie zuerst den IP-Firewallzugriff auf das Azure Cosmos DB-Konto.  

2. Aktivieren Sie den Virtual Network-Endpunkt für das Azure Cosmos DB-Konto mithilfe einer der in den vorherigen Abschnitten beschriebenen Methoden.  

3. Aktivieren Sie den IP-Firewallzugriff erneut. 

## <a name="test-the-access"></a>Testen des Zugriffs

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob die Dienstendpunkte für Azure Cosmos DB erwartungsgemäß konfiguriert wurden:

* Richten Sie zwei Subnetze in einem Virtual Network als Front-End und Back-End ein, und aktivieren Sie den Cosmos DB-Dienstendpunkt für das Back-End-Subnetz.  

* Aktivieren Sie den Zugriff im Cosmos DB-Konto für das Back-End-Subnetz.  

* Erstellen Sie zwei virtuelle Computer: einen im Back-End-Subnetz und einen anderen im Front-End-Subnetz.  

* Versuchen Sie von beiden virtuellen Computern aus, auf das Azure Cosmos DB-Konto zuzugreifen. Sie sollten von dem virtuellen Computer, der im Back-End-Subnetz erstellt wurde, eine Verbindung herstellen können, nicht aber von dem Computer, der im Front-End-Subnetz erstellt wurde. Die Anforderung gibt den Fehler 404 aus, wenn Sie versuchen, von dem Front-End-Subnetz aus eine Verbindung herzustellen. Damit ist bestätigt, dass der Zugriff auf Azure Cosmos DB über den Virtual Network-Dienstendpunkt gesichert ist. Der Computer im Back-End-Subnetz funktioniert ordnungsgemäß.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="what-happens-when-you-access-an-azure-cosmos-db-account-that-has-virtual-network-access-control-list-acl-enabled"></a>Was passiert, wenn Sie auf ein Azure Cosmos DB-Konto zugreifen, für das eine Virtual Network-Zugriffssteuerungsliste aktiviert ist?  

Es wird der HTTP-Fehler 404 zurückgegeben.  

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-cosmos-db-account-in-another-region-for-example-if-azure-cosmos-db-account-is-in-west-us-or-east-us-and-virtual-networks-are-in-multiple-regions-can-the-virtual-network-access-azure-cosmos-db"></a>Können Subnetze eines Virtual Network, die in verschiedenen Regionen erstellt wurden, auf ein Azure Cosmos DB-Konto in einer anderen Region zugreifen? Wenn das Azure Cosmos DB-Konto z.B. in den Regionen „USA, Westen“ oder „USA, Osten“ vorhanden ist und sich die virtuellen Subnetze in verschiedenen Regionen befinden, kann das Virtual Network auf Azure Cosmos DB zugreifen?  

Ja, in verschiedenen Regionen erstellte Virtual Networks können durch die neue Funktion zugreifen. 

### <a name="can-an-azure-cosmos-db-account-have-both-virtual-network-service-endpoint-and-a-firewall"></a>Kann ein Azure Cosmos DB-Konto sowohl über einen Virtual Network-Dienstendpunkt als auch eine Firewall verfügen?  

Ja, es kann sowohl ein Virtual Network-Dienstendpunkt als auch eine Firewall vorhanden sein. Im Allgemeinen sollten Sie sicherstellen, dass der Zugriff auf das Portal immer aktiviert ist, bevor Sie einen Virtual Network-Dienstendpunkt konfigurieren, über den Sie die dem Container zugeordneten Metriken anzeigen können.

### <a name="can-i-allow-access-to-other-azure-services-from-a-given-azure-region-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Kann ich den Zugriff auf andere Azure-Dienste von einer bestimmten Azure-Region zulassen, wenn der Dienstendpunktzugriff für Azure Cosmos DB aktiviert ist?  

Dies ist nur erforderlich, wenn Sie den Zugriff auf Ihr Azure Cosmos DB-Konto über andere Azure-Erstanbieterdienste wie Azure Data Factory, Azure Search oder einen beliebigen anderen Dienst ermöglichen möchten, der in einer bestimmten Azure-Region bereitgestellt wurde.

### <a name="how-many-virtual-network-service-endpoints-are-allowed-for-azure-cosmos-db"></a>Wie viele Virtual Network-Dienstendpunkte sind für Azure Cosmos DB zulässig?  

Für ein Azure Cosmos DB-Konto sind 64 Virtual Network-Dienstendpunkte zulässig.

### <a name="what-is-the-relationship-between-service-endpoint-and-network-security-group-nsg-rules"></a>Welche Beziehung besteht zwischen Dienstendpunkten und Netzwerksicherheitsgruppen-Regeln?  

NSG-Regeln in Azure Cosmos DB erlauben es Ihnen, den Zugriff auf bestimmte Azure Cosmos DB-IP-Adressbereiche zu beschränken. Falls Sie den Zugriff auf eine Azure Cosmos DB-Instanz in einer bestimmten [Region](https://azure.microsoft.com/global-infrastructure/regions/) zulassen möchten, können Sie die Region im folgenden Format angeben: 

    AzureCosmosDB.<region name>

Um mehr über NSG-Tags zu erfahren, lesen Sie den Artikel über [virtuelle Netzwerkdienst-Tags](../virtual-network/security-overview.md#service-tags). 
  
### <a name="what-is-relationship-between-an-ip-firewall-and-virtual-network-service-endpoint-capability"></a>Welche Beziehung besteht zwischen einer IP-Firewall und den Funktionen eines Virtual Network-Dienstendpunkts?  

Diese beiden Features ergänzen einander, um die Isolierung und Sicherheit von Azure Cosmos DB-Ressourcen sicherzustellen. Die Verwendung einer IP-Firewall stellt sicher, dass statische IP-Adressen auf das Azure Cosmos DB-Konto zugreifen können.  

### <a name="can-an-on-premise-devices-ip-address-that-is-connected-through-azure-virtual-network-gatewayvpn-or-express-route-gateway-access-azure-cosmos-db-account"></a>Kann über die IP-Adresse eines lokalen Geräts, das über ein Azure Virtual Network-Gateway (VPN) oder ein ExpressRoute-Gateway verbunden ist, auf ein Azure Cosmos DB-Konto zugegriffen werden?  

Die IP-Adresse oder der IP-Adressbereich des lokalen Geräts muss der Liste der statischen IPs hinzugefügt werden, um den Zugriff auf das Azure Cosmos DB-Konto zu ermöglichen.  

### <a name="what-happens-if-you-delete-a-virtual-network-that-has-service-endpoint-setup-for-azure-cosmos-db"></a>Was passiert, wenn Sie ein Virtual Network löschen, das als Dienstendpunkt für Azure Cosmos DB eingerichtet ist?  

Wenn ein Virtual Network gelöscht wird, werden die Zugriffssteuerungslisten-Informationen, die dieser Azure Cosmos DB-Instanz zugeordnet sind, gelöscht. Die Interaktion zwischen Virtual Network und dem Azure Cosmos DB-Konto wird entfernt. 

### <a name="what-happens-if-an-azure-cosmos-db-account-that-has-virtual-network-service-endpoint-enabled-is-deleted"></a>Was passiert, wenn ein Azure Cosmos DB-Konto, für das ein Virtual Network-Dienstendpunkt aktiviert ist, gelöscht wird?

Die mit diesem bestimmten Azure Cosmos DB-Konto verknüpften Metadaten werden aus dem Subnetz gelöscht. Der Endbenutzer ist dafür zuständig, das verwendete Subnetz und Virtual Network zu löschen.

### <a name="can-i-use-a-peered-virtual-network-to-create-service-endpoint-for-azure-cosmos-db"></a>Kann ich ein virtuelles Peernetzwerk verwenden, um einen Dienstendpunkt für Azure Cosmos DB zu erstellen?  

Nein. Nur direkte Virtual Networks und zugehörige Subnetze können zum Erstellen von Azure Cosmos DB-Dienstendpunkten verwendet werden.

### <a name="what-happens-to-the-source-ip-address-of-resource-like-virtual-machine-in-the-subnet-that-has-azure-cosmos-db-service-endpoint-enabled"></a>Was passiert mit der IP-Quelladresse von Ressourcen wie z.B. virtuellen Computern in dem Subnetz, in dem ein Azure Cosmos DB-Dienstendpunkt aktiviert ist?

Wenn Virtual Network-Dienstendpunkte aktiviert sind, verwenden die IP-Quelladressen von Ressourcen im Subnetz Ihres Virtual Network keine öffentlichen IPv4-Adressen mehr, sondern private Azure Virtual Network-Adressen, um Datenverkehr an Azure Cosmos DB zu leiten.

### <a name="does-azure-cosmos-db-reside-in-the-azure-virtual-network--provided-by-the-customer"></a>Ist Azure Cosmos DB in dem Azure Virtual Network vorhanden, das vom Kunden bereitgestellt wird?  

Azure Cosmos DB ist ein mehrinstanzfähiger Dienst mit einer öffentlichen IP-Adresse. Wenn Sie den Zugriff auf ein Subnetz eines Azure Virtual Network mithilfe eines Dienstendpunkts einschränken, wird der Zugriff für Ihr Azure Cosmos DB-Konto über das angegebene Azure Virtual Network und das zugehörige Subnetz eingeschränkt.  Das Azure Cosmos DB-Konto befindet sich nicht in diesem Azure Virtual Network. 

### <a name="what-if-anything-will-be-logged-in-log-analyticsoms-if-it-is-enabled"></a>Was passiert, wenn Daten in Log Analytics oder OMS protokolliert werden, sofern diese Dienste aktiviert sind?  

Azure Cosmos DB sendet Protokolle mit der IP-Adresse (ohne das letzte Oktett) per Push mit dem Status 403 für Anforderungen, die von der Zugriffssteuerungsliste blockiert werden.  

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Konfigurieren einer Firewall für Azure Cosmos DB finden Sie im Artikel [Firewallunterstützung](firewall-support.md).

