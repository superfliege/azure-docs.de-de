- Das virtuelle Netzwerk (VNet) muss sich in der gleichen **Azure-Region** und im gleichen **Azure-Abonnement** befinden wie das Batch-Konto.

- Für Pools, die mit einer VM-Konfiguration erstellt wurden, werden nur virtuelle Netzwerke unterstützt, die auf Azure Resource Manager basieren. Für Pools, die mit einer Clouddienstkonfiguration erstellt wurden, werden nur klassische virtuelle Netzwerke unterstützt. 
  
- Um ein klassisches VNet verwenden zu können, muss das Dienstprinzipal `MicrosoftAzureBatch` über die rollenbasierte Zugriffssteuerungsrolle `Classic Virtual Machine Contributor` für das angegebene VNet verfügen. Es ist jedoch keine zusätzliche Einrichtung von Berechtigungen erforderlich, um ein Azure Resource Manager-basiertes VNet zu verwenden.

- Das für den Pool angegebene Subnetz muss über ausreichend nicht zugewiesene IP-Adressen verfügen, um die Anzahl virtueller Computer aufnehmen zu können, die für den Pool geplant sind, d. h. die Summe der `targetDedicatedNodes`- und `targetLowPriorityNodes`-Eigenschaften des Pools. Wenn das Subnetz nicht über ausreichend nicht zugewiesene IP-Adressen verfügt, belegt der Pool teilweise die Computeknoten und es tritt ein Anpassungsfehler auf. 

- Das VNet muss eine Kommunikation vom Batch-Dienst aus zulassen, um Tasks auf den Computeknoten planen zu können. Dies kann durch die Überprüfung bestätigt werden, ob dem VNet Netzwerksicherheitsgruppen (NSGs) zugewiesen sind. Falls die Kommunikation mit den Computeknoten im angegebenen Subnetz durch eine Netzwerksicherheitsgruppe (NSG) verhindert wird, legt der Batch-Dienst den Zustand der Computeknoten auf **Nicht verwendbar** fest. 

- Wenn das angegebene VNet über zugewiesene Netzwerksicherheitsgruppen (NSGs) und/oder eine Firewall verfügt, konfigurieren Sie die eingehenden und ausgehenden Ports, wie in den folgenden Tabellen gezeigt:


  |    Zielport(s)    |    Quell-IP-Adresse      |   Quellport    |    Fügt Batch NSGs hinzu?    |    Erforderlich für die Verwendung des virtuellen Computers?    |    Aktion von Benutzer   |
  |---------------------------|---------------------------|----------------------------|----------------------------|-------------------------------------|-----------------------|
  |   <ul><li>Für Pools, die mit der Konfiguration des virtuellen Computers erstellt wurden: 29876, 29877</li><li>Für Pools, die mit der Clouddienstkonfiguration erstellt wurden: 29876, 29877: 10100, 20100, 30100</li></ul>        |    Nur IP-Adressen der Batch-Dienstrolle | * oder 443 |    Ja. Batch fügt NSGs auf der Ebene der Netzwerkschnittstellen (NIC) hinzu, die virtuellen Computern angefügt sind. Diese NSGs lassen nur Datenverkehr von IP-Adressen der Batch-Dienstrolle zu. Auch wenn Sie diese Ports für das gesamte Web öffnen, wird der Datenverkehr an der Netzwerkschnittstellen blockiert. |    Ja  |  Sie müssen keine NSG angeben, da der Batch nur Batch-IP-Adressen zulässt. <br /><br /> Wenn Sie jedoch eine NSG angeben, stellen Sie sicher, dass diese Ports für eingehenden Datenverkehr geöffnet sind. <br /><br /> Bei Angabe von „*“ als Quell-IP in Ihrer NSG fügt Batch NSGs weiterhin auf der Ebene der Netzwerkschnittstellen (NIC) hinzu, die virtuellen Computern angefügt sind. |
  |    3389 (Windows), 22 (Linux)               |    Benutzercomputer, die für das Debuggen verwendet werden, sodass Sie remote auf den virtuellen Computer zugreifen können.    |   *  | Nein                                    |    Nein                    |    Fügen Sie Netzwerksicherheitsgruppen hinzu, wenn Sie den Remotezugriff (RDP oder SSH) auf den virtuellen Computer zulassen möchten.   |                                


  |    Ausgehende Ports    |    Ziel    |    Fügt Batch NSGs hinzu?    |    Erforderlich für die Verwendung des virtuellen Computers?    |    Aktion von Benutzer    |
  |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
  |    443    |    Azure Storage    |    Nein    |    Ja    |    Wenn Sie NSGs hinzufügen, stellen Sie sicher, dass dieser Port für den ausgehenden Datenverkehr geöffnet ist.    |

   Stellen Sie außerdem sicher, dass Ihr Azure Storage-Endpunkt von benutzerdefinierten DNS-Servern aufgelöst werden kann, die Ihr VNET bedienen. Insbesondere URLs im Format `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` und `<account>.blob.core.windows.net` müssen auflösbar sein. 

   Wenn Sie eine auf Resource Manager basierende NSG hinzufügen, können Sie [Diensttags](../articles/virtual-network/security-overview.md#service-tags) verwenden, um die Storage-IP-Adressen für die jeweilige Region für ausgehende Verbindungen auszuwählen. Beachten Sie, dass die Storage-IP-Adressen derselben Region wie Ihr Batch-Konto und das VNet zugeordnet sein müssen. Diensttags befinden sich in ausgewählten Azure-Regionen derzeit in der Vorschauphase.