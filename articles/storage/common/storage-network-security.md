---
title: Konfigurieren von Firewalls und virtuellen Netzwerken in Azure Storage (Vorschau) | Microsoft-Dokumentation
description: "Konfigurieren Sie mehrstufige Netzwerksicherheit für Ihr Speicherkonto."
services: storage
documentationcenter: 
author: cbrooksmsft
manager: cbrooks
editor: cbrooks
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 09/25/2017
ms.author: cbrooks
ms.openlocfilehash: 8117a5ef9bc4f785256a7a7d70f459529c771a56
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2017
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks-preview"></a>Konfigurieren von Firewalls und virtuellen Netzwerken in Azure Storage (Vorschau)
Azure Storage bietet ein mehrstufiges Sicherheitsmodell, mit dem Sie Ihre Speicherkonten für eine bestimmte Gruppe zulässiger Netzwerke sichern können.  Wenn Netzwerkregeln konfiguriert sind, können nur Anwendungen aus zulässigen Netzwerken auf ein Speicherkonto zugreifen.  Anwendungen, die aus einem zulässigen Netzwerk aufgerufen werden, erfordern für den Zugriff auf das Speicherkonto weiterhin eine ordnungsgemäße Autorisierung (einen gültigen Zugriffsschlüssel oder ein gültiges SAS-Token).

## <a name="preview-availability-and-support"></a>Verfügbarkeit und Unterstützung der Vorschauversion
Firewalls und virtuelle Netzwerke in Storage sind als Vorschauversion verfügbar.  Diese Funktion ist derzeit für neue oder vorhandene Speicherkonten in den folgenden Regionen verfügbar:
- USA (Ost)
- USA (Westen)
- USA, Westen 2
- USA, Westen-Mitte
- Australien (Osten)
- Australien, Südosten

> [!NOTE]
> Produktionsworkloads werden in der Vorschauversion nicht unterstützt.
>

## <a name="scenarios"></a>Szenarien
Speicherkonten können konfiguriert werden, um standardmäßig den Zugriff auf Datenverkehr aus allen Netzwerken (einschließlich Internetdatenverkehr) zu verweigern.  Es kann Zugriff auf Datenverkehr aus bestimmten Azure Virtual Networks gewährt werden, sodass Sie eine sichere Netzwerkgrenze für Ihre Anwendungen erstellen können.  Es kann auch Zugriff auf öffentliche Internet-IP-Adressbereiche gewährt werden, um Verbindungen von bestimmten Internetclients oder lokalen Clients zu ermöglichen.

Netzwerkregeln werden für alle Netzwerkprotokolle in Azure Storage, einschließlich REST und SMB, erzwungen.  Für den Zugriff auf Ihre Daten über Tools, z.B. Azure-Portal, Storage-Explorer und AZCopy, müssen explizite Netzwerkregeln Zugriff gewähren, wenn Netzwerkregeln in Kraft sind.

Netzwerkregeln können auf vorhandene Speicherkonten oder während der Erstellung neuer Speicherkonten angewendet werden.

Sobald Netzwerkregeln angewendet werden, werden sie für alle Anforderungen erzwungen.  SAS-Token, die Zugriff auf einen Dienst unter einer bestimmten IP-Adresse gewähren, **beschränken** den Zugriff des Tokeninhabers, gewähren jedoch keinen neuen Zugriff außerhalb der konfigurierten Netzwerkregeln. 

Datenverkehr virtueller Computer (einschließlich Bereitstellungsvorgängen, des Aufhebens von Bereitstellungen und Datenträger-E/A) ist von Netzwerkregeln **nicht** betroffen.  Der REST-Zugriff auf Seitenblobs wird durch Netzwerkregeln geschützt.

Die Funktion „Firewalls und virtuelle Netzwerke“ wird von klassischen Speicherkonten **nicht** unterstützt.

## <a name="change-the-default-network-access-rule"></a>Ändern der Standard-Netzwerkzugriffsregel
Standardmäßig akzeptieren Speicherkonten Verbindungen von Clients in jedem Netzwerk.  Um den Zugriff auf ausgewählte Netzwerke zu beschränken, müssen Sie zunächst die Standardaktion ändern.

> [!WARNING]
> Änderungen an Netzwerkregeln können die Fähigkeit von Anwendungen, eine Verbindung mit Azure Storage herzustellen, beeinträchtigen.  Durch das Festlegen der Standardnetzwerkregel auf das **Verweigern** des Zugriffs wird sämtlicher Zugriff auf die Daten blockiert, es sei denn, es werden auch bestimmte Netzwerkregeln angewendet, die Zugriff *gewähren*.  Gewähren Sie mithilfe von Netzwerkregeln Zugriff auf alle zulässigen Netzwerke, bevor Sie die Standardregel ändern, um Zugriff zu verweigern.
>

#### <a name="azure-portal"></a>Azure-Portal
1. Navigieren Sie zu dem Speicherkonto, das Sie sichern möchten.  
> [!NOTE]
> Stellen Sie sicher, dass sich Ihr Speicherkonto in einer der für die öffentliche Vorschauversion unterstützten Regionen befindet.
>

2. Klicken Sie auf das Einstellungsmenü mit dem Namen **Firewalls und virtuelle Netzwerke**.
3. Um den Zugriff standardmäßig zu verweigern, wählen Sie das Zulassen des Zugriffs aus „Ausgewählte Netzwerke“ aus.  Um Datenverkehr aus allen Netzwerken zuzulassen, lassen Sie den Zugriff aus „Alle Netzwerke“ zu.
4. Klicken Sie zum Übernehmen der Änderungen auf *Speichern*.

#### <a name="powershell"></a>PowerShell
1. Installieren Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-azurerm-ps), und [melden Sie sich an](/powershell/azure/authenticate-azureps).

2. Zeigen Sie den Status der Standardregel für das Speicherkonto an.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet  -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
``` 

3. Legen Sie die Standardregel auf das standardmäßige Verweigern jeglichen Netzwerkzugriffs fest.  
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
```    

4. Legen Sie die Standardregel auf das standardmäßige Zulassen von Netzwerkzugriff fest.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
```    

#### <a name="cliv2"></a>CLI v2
1. [Installieren Sie Azure CLI 2.0](/cli/azure/install-azure-cli), und [melden Sie sich an](/cli/azure/authenticate-azure-cli).
2. Zeigen Sie den Status der Standardregel für das Speicherkonto an.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkAcls.defaultAction
```

3. Legen Sie die Standardregel auf das standardmäßige Verweigern jeglichen Netzwerkzugriffs fest.  
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Deny
```

4. Legen Sie die Standardregel auf das standardmäßige Zulassen von Netzwerkzugriff fest.
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Allow
```

## <a name="grant-access-from-a-virtual-network"></a>Gewähren des Zugriffs aus einem virtuellen Netzwerk
Speicherkonten können so konfiguriert werden, dass nur der Zugriff aus bestimmten Azure Virtual Networks zugelassen wird. 

Durch Aktivieren eines [Dienstendpunkts](/azure/virtual-network/virtual-network-service-endpoints-overview) für Azure Storage im virtuellen Netzwerk wird eine optimale Route für den Datenverkehr zum Azure Storage-Dienst sichergestellt. Mit jeder Anforderung wird zudem die Identität des virtuellen Netzwerks und des Subnetzes übertragen.  Administratoren können anschließend Netzwerkregeln für das Speicherkonto konfigurieren, die den Empfang von Anforderungen aus bestimmten Subnetzen im virtuellen Netzwerk zulassen.  Clients, denen über diese Netzwerkregeln Zugriff gewährt wird, müssen weiterhin die Autorisierungsanforderungen des Speicherkontos erfüllen, um auf die Daten zugreifen zu können.

Jedes Speicherkonto unterstützt bis zu 100 Regeln für virtuelle Netzwerke, die mit [IP-Netzwerkregeln](#grant-access-from-an-internet-ip-range) kombiniert werden können.

### <a name="available-virtual-network-regions"></a>Verfügbare Regionen virtueller Netzwerke
Dienstendpunkte können in der Regel zwischen virtuellen Netzwerken und Dienstinstanzen in derselben Azure-Region verwendet werden.  Wenn Dienstendpunkte mit Azure Storage verwendet werden, umfasst dieser Bereich auch [Regionspaare](/azure/best-practices-availability-paired-regions).  Dies ermöglicht Kontinuität während eines regionalen Failovers sowie den unterbrechungsfreien Zugriff auf Instanzen von georedundantem Speicher mit Lesezugriff (Read-Access Geo-Redundant Storage, RA-GRS).  Netzwerkregeln, die Zugriff aus einem virtuellen Netzwerk auf ein Speicherkonto gewähren, gewähren auch Zugriff auf jede RA-GRS-Instanz.

Wenn Sie die Notfallwiederherstellung für einen regionalen Ausfall planen, sollten Sie die virtuellen Netzwerke im Voraus im Regionspaar bereitstellen. Es sollten Dienstendpunkte für Azure Storage aktiviert werden, und auf Ihre GRS-Konten (georedundanter Speicher) sollten Netzwerkregeln angewendet werden, die Zugriff aus den alternativen virtuellen Netzwerken gewähren.

> [!NOTE]
> Dienstendpunkte gelten nicht für Datenverkehr außerhalb der Region des virtuellen Netzwerks und des angegebenen Regionspaars.  Netzwerkregeln, die Zugriff aus virtuellen Netzwerken auf Speicherkonten gewähren, können nur auf virtuelle Netzwerke in der primären Region eines Speicherkontos oder im angegebenen Regionspaar angewendet werden.
>

### <a name="required-permissions"></a>Erforderliche Berechtigungen
Um eine Regel für virtuelle Netzwerke auf ein Speicherkonto anzuwenden, muss der Benutzer über die Berechtigung zum *Hinzufügen eines Diensts zu einem Subnetz* für die hinzuzufügenden Subnetze verfügen.  Diese Berechtigung ist in der integrierten Rolle *Speicherkontomitwirkender* enthalten und kann benutzerdefinierten Rollendefinitionen hinzugefügt werden.

Der für das Speicherkonto und die virtuellen Netzwerke gewährte Zugriff ist **möglicherweise** in unterschiedlichen Abonnements enthalten, diese müssen jedoch zu demselben Azure Active Directory-Mandanten gehören.

### <a name="managing-virtual-network-rules"></a>Verwalten von Regeln für virtuelle Netzwerke
Regeln für virtuelle Netzwerke für Speicherkonten können über das Azure-Portal, über PowerShell oder CLI v2 verwaltet werden.

#### <a name="azure-portal"></a>Azure-Portal
1. Navigieren Sie zu dem Speicherkonto, das Sie sichern möchten.  
2. Klicken Sie auf das Einstellungsmenü mit dem Namen **Firewalls und virtuelle Netzwerke**.
3. Stellen Sie sicher, dass Sie das Zulassen des Zugriffs aus „Ausgewählte Netzwerke“ ausgewählt haben.
4. Wenn Sie mit einer neuen Netzwerkregel den Zugriff auf ein virtuelles Netzwerk zulassen möchten, klicken Sie unter „Virtuelle Netzwerke“ auf „Vorhandene hinzufügen“ um ein vorhandenes virtuelles Netzwerk und vorhandene Subnetze auszuwählen, und klicken Sie auf *Hinzufügen*.  Um ein neues virtuelles Netzwerk zu erstellen und ihm Zugriff zu gewähren, klicken Sie auf *Neues hinzufügen*, geben Sie die erforderlichen Informationen zum Erstellen des neuen virtuellen Netzwerks an, und klicken Sie dann auf *Erstellen*.

> [!NOTE]
> Wenn für das ausgewählte virtuelle Netzwerk und die Subnetze bisher noch kein Dienstendpunkt für Azure Storage konfiguriert wurde, kann dies im Rahmen dieses Vorgangs erfolgen.
>

5. Klicken Sie zum Entfernen einer Regel für virtuelle Netzwerke oder Subnetze auf „...“, um das Kontextmenü für das virtuelle Netzwerk oder Subnetz zu öffnen, und klicken Sie auf „Entfernen“.
6. Klicken Sie zum Übernehmen der Änderungen auf *Speichern*.

#### <a name="powershell"></a>PowerShell
1. Installieren Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-azurerm-ps), und [melden Sie sich an](/powershell/azure/authenticate-azureps).
2. Listen Sie Regeln für virtuelle Netzwerke auf.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
```

3. Aktivieren Sie den Dienstendpunkt für Azure Storage in einem vorhandenen virtuellen Netzwerk und Subnetz.
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"  -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzureRmVirtualNetwork
```

4. Fügen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz hinzu.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

5. Entfernen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Remove-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

> [!IMPORTANT]
> Sie müssen [die Standardregel auf „Verweigern“](#change-the-default-network-access-rule) festlegen. Andernfalls haben die Netzwerkregeln keine Auswirkung.
>

#### <a name="cliv2"></a>CLI v2
1. [Installieren Sie Azure CLI 2.0](/cli/azure/install-azure-cli), und [melden Sie sich an](/cli/azure/authenticate-azure-cli).
2. Listen Sie Regeln für virtuelle Netzwerke auf.
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
```

2. Aktivieren Sie den Dienstendpunkt für Azure Storage in einem vorhandenen virtuellen Netzwerk und Subnetz.
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
```

3. Fügen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz hinzu.  
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "TestVNET" --name "default" --query id --output tsv)
az storage account network-rule add --resource-group myresourcegroup --account-name mystorageaccount --subnet $subnetid
```

4. Entfernen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz. 
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "TestVNET" --name "default" --query id --output tsv)
az storage account network-rule remove --resource-group myresourcegroup --account-name mystorageaccount --subnet $subnetid
```

> [!IMPORTANT]
> Sie müssen [die Standardregel auf „Verweigern“](#change-the-default-network-access-rule) festlegen. Andernfalls haben die Netzwerkregeln keine Auswirkung.
>

## <a name="grant-access-from-an-internet-ip-range"></a>Gewähren von Zugriff aus einem Internet-IP-Adressbereich
Speicherkonten können konfiguriert werden, um den Zugriff von bestimmten öffentlichen Internet-IP-Adressbereichen zuzulassen.  Diese Konfiguration ermöglicht es, bestimmten internetbasierten Diensten und lokalen Netzwerken Zugriff zu gewähren, während der allgemeine Internetdatenverkehr blockiert wird.

Zulässige Internetadressbereiche können in [CIDR-Notation](https://tools.ietf.org/html/rfc4632) im Format *16.17.18.0/24* oder als einzelne IP-Adressen, z.B. *16.17.18.19*, angegeben werden.

> [!NOTE]
> Kleine Adressbereiche mit der Präfixgröße „/ 31“ oder „/ 32“ werden nicht unterstützt.  Diese Bereiche müssen mit einzelnen IP-Adressregeln konfiguriert werden.
>

IP-Netzwerkregeln sind nur für **öffentliche Internet**-IP-Adressen zulässig.  Für private Netzwerke reservierte IP-Adressbereiche (gemäß RFC 1918) sind in IP-Adressregeln nicht zulässig.  Private Netzwerke enthalten Adressen, die mit *10.\**, *172.16.\** und *192.168.\** beginnen.

Derzeit werden nur IPv4-Adressen unterstützt.

Jedes Speicherkonto unterstützt bis zu 100 IP-Netzwerkregeln, die mit [Regeln für virtuelle Netzwerke](#grant-access-from-a-virtual-network) kombiniert werden können.

### <a name="configuring-access-from-on-premises-networks"></a>Konfigurieren des Zugriffs aus lokalen Netzwerken
Um mit einer IP-Netzwerkregel den Zugriff aus den lokalen Netzwerken auf das Speicherkonto zu gewähren, müssen Sie die von Ihrem Netzwerk verwendeten Internet-IP-Adressen ermitteln.  Hilfe erhalten Sie von Ihrem Netzwerkadministrator.

Wenn Ihr Netzwerk mithilfe von [ExpressRoute](/azure/expressroute/expressroute-introduction) mit dem Azure-Netzwerk verbunden ist, ist jede Verbindung mit zwei öffentlichen IP-Adressen bei Microsoft konfiguriert. Diese werden zum Herstellen der Verbindung mit Microsoft-Diensten, z.B. Azure Storage, mithilfe von [öffentlichem Azure-Peering](/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains) verwendet.  Um die Kommunikation von Ihrer Verbindung mit Azure Storage zu ermöglichen, müssen Sie IP-Adressregeln für die öffentlichen IP-Adressen Ihrer Verbindungen erstellen.  [Öffnen Sie über das Azure-Portal ein Supportticket für ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview), um die öffentlichen IP-Adressen Ihrer ExpressRoute-Verbindung zu ermitteln.


### <a name="managing-ip-network-rules"></a>Verwalten von IP-Netzwerkregeln
IP-Netzwerkregeln für Speicherkonten können über das Azure-Portal, über PowerShell oder CLI v2 verwaltet werden.

#### <a name="azure-portal"></a>Azure-Portal
1. Navigieren Sie zu dem Speicherkonto, das Sie sichern möchten.  
2. Klicken Sie auf das Einstellungsmenü mit dem Namen **Firewalls und virtuelle Netzwerke**.
3. Stellen Sie sicher, dass Sie das Zulassen des Zugriffs aus „Ausgewählte Netzwerke“ ausgewählt haben.
4. Geben Sie unter „Firewall“, „Adressbereiche“ die IP-Adresse oder den IP-Adressbereich (im CIDR-Format) ein, um Zugriff auf einen Internet-IP-Adressbereich zu gewähren.
5. Klicken Sie zum Entfernen einer IP-Netzwerkregel auf „...“, um das Kontextmenü für die Regel zu öffnen, und klicken Sie auf „Entfernen“.
6. Klicken Sie zum Übernehmen der Änderungen auf *Speichern*.

#### <a name="powershell"></a>PowerShell
1. Installieren Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-azurerm-ps), und [melden Sie sich an](/powershell/azure/authenticate-azureps).
2. Listen Sie IP-Netzwerkregeln auf.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
```

3. Fügen Sie eine Netzwerkregel für eine einzelne IP-Adresse hinzu.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19" 
``` 

4. Fügen Sie eine Netzwerkregel für einen IP-Adressbereich hinzu.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24" 
```    

5. Entfernen Sie eine Netzwerkregel für eine einzelne IP-Adresse. 
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"  
```

6. Entfernen Sie eine Netzwerkregel für einen IP-Adressbereich.  
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"  
```    

> [!IMPORTANT]
> Sie müssen [die Standardregel auf „Verweigern“](#change-the-default-network-access-rule) festlegen. Andernfalls haben die Netzwerkregeln keine Auswirkung.
>

#### <a name="cliv2"></a>CLI v2
1. [Installieren Sie Azure CLI 2.0](/cli/azure/install-azure-cli), und [melden Sie sich an](/cli/azure/authenticate-azure-cli).
2. Listen Sie IP-Netzwerkregeln auf.
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
```

3. Fügen Sie eine Netzwerkregel für eine einzelne IP-Adresse hinzu.
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

4. Fügen Sie eine Netzwerkregel für einen IP-Adressbereich hinzu.  
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

5. Entfernen Sie eine Netzwerkregel für eine einzelne IP-Adresse.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

6. Entfernen Sie eine Netzwerkregel für einen IP-Adressbereich.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

> [!IMPORTANT]
> Sie müssen [die Standardregel auf „Verweigern“](#change-the-default-network-access-rule) festlegen. Andernfalls haben die Netzwerkregeln keine Auswirkung.
>

## <a name="exceptions"></a>Ausnahmen
In den meisten Szenarien ermöglichen Netzwerkregeln eine sichere Netzwerkkonfiguration. In einigen Fällen müssen jedoch Ausnahmen gewährt werden, um vollständige Funktionalität zu erzielen.  Speicherkonten können mit Ausnahmen für vertrauenswürdige Microsoft-Dienste und für den Zugriff auf Storage Analytics-Daten konfiguriert werden.

### <a name="trusted-microsoft-services"></a>Vertrauenswürdige Microsoft-Dienste
Einige Microsoft-Dienste, die mit Speicherkonten interagieren, werden über Netzwerke ausgeführt, denen nicht mit Netzwerkregeln Zugriff gewährt werden kann. 

Damit Dienste dieses Typs wie vorgesehen ausgeführt werden, können Sie den vertrauenswürdigen Microsoft-Diensten das Umgehen der Netzwerkregeln gestatten. Diese Dienste verwenden dann für den Zugriff auf das Speicherkonto strenge Authentifizierung.

Wenn die Ausnahme „Vertrauenswürdige Microsoft-Dienste“ aktiviert ist, wird den folgenden Diensten (sofern sie in Ihrem Abonnement registriert sind) Zugriff auf das Speicherkonto gewährt:

|Dienst|Name des Ressourcenanbieters|Zweck|
|:------|:---------------------|:------|
|Azure DevTest Labs|Microsoft.DevTestLab|Erstellung benutzerdefinierter Images und Installation von Artefakten.  [Weitere Informationen](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-overview).|
|Azure Event Grid|Microsoft.EventGrid|Aktivieren der Veröffentlichung von Blob Storage-Ereignissen.  [Weitere Informationen](https://docs.microsoft.com/en-us/azure/event-grid/overview).|
|Azure Event Hubs|Microsoft.EventHub|Archivieren von Daten mit Event Hubs Capture.  [Weitere Informationen](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-capture-overview).|
|Azure HDInsight|Microsoft.HDInsight|Clusterbereitstellung und -installation.  [Weitere Informationen](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-use-blob-storage).|
|Azure-Netzwerke|Microsoft.Networking|Speichern und Analysieren von Protokollen des Netzwerkdatenverkehrs.  [Weitere Informationen](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-packet-capture-overview).|
|Azure SQL Data Warehouse|Microsoft.Sql|Import und Export von Daten.  [Weitere Informationen](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-overview-load#load-from-azure-blob-storage).|
|Azure Backup|Microsoft.RecoveryServices|Sicherung und Wiederherstellung von nicht verwalteten Datenträgern.  [Weitere Informationen](https://docs.microsoft.com/en-us/azure/backup/backup-introduction-to-azure-backup).|
||||

### <a name="storage-analytics-data-access"></a>Zugriff auf Storage Analytics-Daten
In manchen Fällen ist der Lesezugriff auf Diagnoseprotokolle und -metriken von außerhalb des Netzwerks erforderlich.  Es können Ausnahmen von den Netzwerkregeln gewährt werden, um den Lesezugriff auf Protokolldateien und/oder Metriktabellen des Speicherkontos zuzulassen. [Weitere Informationen zum Arbeiten mit Storage Analytics](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Verwalten von Ausnahmen
Ausnahmen von Netzwerkregeln können über das Azure-Portal, über PowerShell oder Azure CLI v2 verwaltet werden.

#### <a name="azure-portal"></a>Azure-Portal
1. Navigieren Sie zu dem Speicherkonto, das Sie sichern möchten.  
2. Klicken Sie auf das Einstellungsmenü mit dem Namen **Firewalls und virtuelle Netzwerke**.
3. Stellen Sie sicher, dass Sie das Zulassen des Zugriffs aus „Ausgewählte Netzwerke“ ausgewählt haben.
4. Wählen Sie unter „Ausnahmen“ die Ausnahmen aus, die Sie gewähren möchten.
5. Klicken Sie zum Übernehmen der Änderungen auf *Speichern*.

#### <a name="powershell"></a>PowerShell
1. Installieren Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-azurerm-ps), und [melden Sie sich an](/powershell/azure/authenticate-azureps).
2. Zeigen Sie die Ausnahmen für die Speicherkonto-Netzwerkregeln an.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
```

3. Konfigurieren Sie die Ausnahmen von den Speicherkonto-Netzwerkregeln.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass AzureServices,Metrics,Logging
```

4. Entfernen Sie die Ausnahmen von den Speicherkonto-Netzwerkregeln.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass None
```

> [!IMPORTANT]
> Sie müssen [die Standardregel auf „Verweigern“ festlegen](#change-the-default-network-access-rule). Andernfalls bleibt das Entfernen von Ausnahmen ohne Auswirkung.
>

#### <a name="cliv2"></a>CLI v2
1. [Installieren Sie Azure CLI 2.0](/cli/azure/install-azure-cli), und [melden Sie sich an](/cli/azure/authenticate-azure-cli).
2. Zeigen Sie die Ausnahmen für die Speicherkonto-Netzwerkregeln an.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkAcls.bypass
```

3. Konfigurieren Sie die Ausnahmen von den Speicherkonto-Netzwerkregeln.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
```

4. Entfernen Sie die Ausnahmen von den Speicherkonto-Netzwerkregeln.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
```

> [!IMPORTANT]
> Sie müssen [die Standardregel auf „Verweigern“ festlegen](#change-the-default-network-access-rule). Andernfalls bleibt das Entfernen von Ausnahmen ohne Auswirkung.
>

## <a name="next-steps"></a>Nächste Schritte
Unter [Dienstendpunkte](/azure/virtual-network/virtual-network-service-endpoints-overview) erhalten Sie weitere Informationen über Dienstendpunkte in Azure-Netzwerken.

Im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md) erhalten Sie weitere Informationen zur Sicherheit von Azure Storage.
