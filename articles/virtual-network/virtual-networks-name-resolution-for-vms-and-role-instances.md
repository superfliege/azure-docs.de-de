---
title: "Namensauflösung für virtuelle Computer und Rolleninstanzen"
description: "Szenarien für die Namensauflösung für Azure IaaS, Hybridlösungen, zwischen verschiedenen Clouddiensten, Active Directory und Verwenden eines eigenen DNS-Servers"
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: jdial
ms.openlocfilehash: 5ea3e4ad68fd37ccaa6e081febe4827bdb2e196d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Namensauflösung für virtuelle Computer und Rolleninstanzen

Abhängig davon, wie Sie Azure zum Hosten von IaaS, PaaS und Hybridlösungen verwenden, müssen Sie für die virtuellen Computer und Rolleninstanzen, die Sie erstellen, die Kommunikation untereinander zulassen. Obwohl diese Kommunikation über IP-Adressen erfolgen kann, ist es deutlich einfacher, Namen zu verwenden, die leicht zu merken sind und sich nicht verändern. 

Wenn in Azure gehostete Rolleninstanzen und virtuelle Computer Domänennamen in interne IP-Adressen auflösen müssen, können sie eine dieser beiden Methoden verwenden:

* [Von Azure bereitgestellte Namensauflösung](#azure-provided-name-resolution)
* [Namensauflösung mit einem eigenen DNS-Server](#name-resolution-using-your-own-dns-server) (dieser kann Abfragen an die von Azure bereitgestellten DNS-Server weiterleiten) 

Welche Art der Namensauflösung Sie verwenden, hängt davon ab, wie die virtuellen Computer und Rolleninstanzen miteinander kommunizieren müssen. In der folgenden Tabelle sind die Szenarien und entsprechenden Lösungen für die Namensauflösung aufgeführt:

| **Szenario** | **Lösung** | **Suffix** |
| --- | --- | --- |
| Namensauflösung zwischen Rolleninstanzen oder virtuellen Computern im gleichen Clouddienst oder virtuellen Netzwerk |[Von Azure bereitgestellte Namensauflösung](#azure-provided-name-resolution) |Hostname oder FQDN |
| Namensauflösung aus einem Azure App Service (Web-App, Funktion oder Bot) mithilfe von Integration virtueller Netzwerke in Rolleninstanzen oder virtuelle Computer im gleichen virtuellen Netzwerk |Vom Kunden verwaltete DNS-Server, die Abfragen zwischen virtuellen Netzwerken zur Auflösung durch Azure weiterleiten (DNS-Proxy). Siehe [Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server). |Nur FQDN |
| Namensauflösung zwischen Rolleninstanzen oder virtuellen Computern in unterschiedlichen virtuellen Netzwerken |Vom Kunden verwaltete DNS-Server, die Abfragen zwischen virtuellen Netzwerken zur Auflösung durch Azure weiterleiten (DNS-Proxy). Siehe [Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server). |Nur FQDN |
| Namensauflösung aus App Service-Web-Apps in virtuelle Computer im gleichen virtuellen Netzwerk |Vom Kunden verwaltete DNS-Server, die Abfragen zwischen virtuellen Netzwerken zur Auflösung durch Azure weiterleiten (DNS-Proxy). Siehe [Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server). |Nur FQDN |
| Namensauflösung aus App Service-Web-Apps in virtuelle Computer in unterschiedlichen virtuellen Netzwerken |Vom Kunden verwaltete DNS-Server, die Abfragen zwischen virtuellen Netzwerken zur Auflösung durch Azure weiterleiten (DNS-Proxy). Siehe [Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server-for-web-apps). |Nur FQDN |
| Auflösung lokaler Computer- und Dienstnamen von Rolleninstanzen oder virtuellen Computern in Azure |Vom Kunden verwaltete DNS-Server (z. B. lokale Domänencontroller, lokale schreibgeschützte Domänencontroller oder ein sekundärer DNS-Server, der mithilfe von Zonenübertragungen synchronisiert wird). Siehe [Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server). |Nur FQDN |
| Auflösung von Azure-Hostnamen von lokalen Computern |Weiterleiten von Abfragen an einen vom Kunden verwalteten DNS-Proxyserver im zugehörigen virtuellen Netzwerk. Der Proxyserver leitet Abfragen zur Auflösung an Azure weiter. Siehe [Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server). |Nur FQDN |
| Reverse-DNS für interne IPs |[Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server). |Nicht zutreffend |
| Namensauflösung zwischen virtuellen Computern oder Rolleninstanzen in unterschiedlichen Clouddiensten, nicht in einem virtuellen Netzwerk |Nicht zutreffend. Die Konnektivität zwischen virtuellen Computern und Rolleninstanzen in verschiedenen Clouddiensten wird außerhalb eines virtuellen Netzwerks nicht unterstützt. |Nicht zutreffend|

## <a name="azure-provided-name-resolution"></a>Von Azure bereitgestellte Namensauflösung

Zusammen mit der Auflösung des öffentlichen DNS-Namens bietet Azure die Auflösung interner Namen für virtuelle Computer und Rolleninstanzen, die sich innerhalb des gleichen virtuellen Netzwerks oder Clouddiensts befinden. Virtuelle Computer und Instanzen in einem Clouddienst verwenden dasselbe DNS-Suffix (deshalb ist der Hostname allein ausreichend). In klassischen virtuellen Netzwerken hingegen haben verschiedene Clouddienste unterschiedliche DNS-Suffixe. Deshalb wird hier der FQDN (Fully Qualified Domain Name, vollqualifizierter Domänenname) benötigt, um Namen zwischen verschiedenen Clouddiensten aufzulösen. In virtuellen Netzwerken im Resource Manager-Bereitstellungsmodell ist das DNS-Suffix im ganzen virtuellen Netzwerk gleich (FQDN nicht erforderlich), und DNS-Namen können entweder Netzwerkkarten oder virtuellen Computern zugewiesen werden. Obwohl für die von Azure durchgeführte Namensauflösung keine Konfiguration erforderlich ist, ist sie nicht für alle Bereitstellungsszenarien die beste Lösung (siehe die vorherige Tabelle).

> [!NOTE]
> Bei Verwendung von Web- und Workerrollen können Sie auf die internen IP-Adressen von Rolleninstanzen auch auf Grundlage des Rollennamens und der Instanznummer über die REST-API für die Azure-Dienstverwaltung zugreifen. Weitere Informationen finden Sie unter [Referenz zur REST-API der Dienstverwaltung](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features"></a>Features

* Benutzerfreundlichkeit: Es ist keine Konfiguration erforderlich, um die von Azure bereitgestellte Namensauflösung zu verwenden.
* Der von Azure bereitgestellte Dienst zur Namensauflösung ist hoch verfügbar und erspart Ihnen das Erstellen und Verwalten von Clustern Ihrer eigenen DNS-Server.
* Kann in Verbindung mit Ihren eigenen DNS-Servern zum Auflösen von lokalen und Azure-Hostnamen verwendet werden.
* Die Namensauflösung wird zwischen Rolleninstanzen/virtuellen Computern im gleichen Clouddienst bereitgestellt, ohne dass ein FQDN erforderlich ist.
* Die Namensauflösung wird zwischen virtuellen Computern in virtuellen Netzwerken bereitgestellt, die das Resource Manager-Bereitstellungsmodell verwenden, ohne dass ein FQDN erforderlich ist. Virtuelle Netzwerke im klassischen Bereitstellungsmodell erfordern einen FQDN beim Auflösen von Namen in verschiedenen Clouddiensten. 
* Sie können Hostnamen verwenden, die Ihre Bereitstellungen am besten beschreiben, und müssen nicht mit automatisch generierten Namen arbeiten.

### <a name="considerations"></a>Überlegungen

* Das von Azure erstellte DNS-Suffix kann nicht geändert werden.
* Sie können keine eigenen Einträge manuell registrieren.
* WINS und NetBIOS werden nicht unterstützt (Ihre virtuellen Computer werden im Windows-Explorer nicht angezeigt).
* Hostnamen müssen DNS-kompatibel sein. Für die Namen dürfen nur die Zeichen 0–9, a–z und „-“ verwendet werden, zudem dürfen sie nicht mit „-“ beginnen oder enden. Siehe RFC 3696 Abschnitt 2.
* Der DNS-Abfragedatenverkehr wird für den jeweiligen virtuellen Computer gedrosselt. Die Drosselung sollte auf die meisten Anwendungen keine Auswirkungen haben. Wenn eine Drosselung der Anforderungen festgestellt wird, stellen Sie sicher, dass clientseitiges Zwischenspeichern aktiviert ist. Weitere Informationen finden Sie unter [Die optimale Nutzung der von Azure bereitgestellten Namensauflösung](#Getting-the-most-from-Azure-provided-name-resolution).
* Nur virtuelle Computer in den ersten 180 Clouddiensten werden für jedes virtuelle Netzwerk in einem klassischen Bereitstellungsmodell registriert. Diese Begrenzung gilt nicht für virtuelle Netzwerke in Resource Manager-Bereitstellungsmodellen.

## <a name="dns-client-configuration"></a>DNS-Clientkonfiguration

### <a name="client-side-caching"></a>Clientseitiges Caching

Nicht alle DNS-Abfragen müssen über das Netzwerk gesendet werden. Clientseitiges Zwischenspeichern kann die Latenz verringern und die Flexibilität bei Netzwerkproblemen verbessern, indem sich wiederholende DNS-Abfragen aus einem lokalen Cache aufgelöst werden. DNS-Einträge enthalten eine Gültigkeitsdauer (Time-To-Live, TTL), damit der Eintrag so lange wie möglich im Cache gespeichert werden kann, ohne die Aktualität des Eintrags zu beeinträchtigen, sodass das clientseitige Zwischenspeichern für die meisten Situationen geeignet ist.

Der standardmäßige DNS-Client von Windows verfügt über einen integrierten DNS-Cache. Einige Linux-Distributionen bieten standardmäßig kein Zwischenspeichern. Es wird empfohlen, jedem virtuellen Linux-Computer einen DNS-Cache hinzuzufügen (nachdem Sie sich vergewissert haben, dass noch kein lokaler Cache vorhanden ist).

Es sind mehrere unterschiedliche DNS-Cachingpakete verfügbar. Zum Beispiel dnsmasq. In den folgenden Schritten wird gezeigt, wie dnsmasq in den gängigsten Distributionen installiert wird:

* **Ubuntu (verwendet resolvconf)**:
  * Installieren Sie das dnsmasq-Paket mit `sudo apt-get install dnsmasq`.
* **SUSE (verwendet netconf)**:
  * Installieren Sie das dnsmasq-Paket mit `sudo zypper install dnsmasq`.
  * Aktivieren Sie den dnsmasq-Dienst mit `systemctl enable dnsmasq.service`. 
  * Starten Sie den dnsmasq-Dienst mit `systemctl start dnsmasq.service`. 
  * Bearbeiten Sie **/etc/sysconfig/network/config**, und ändern Sie *NETCONFIG_DNS_FORWARDER=""* in *dnsmasq*.
  * Aktualisieren Sie „resolv.conf“ mit `netconfig update`, um den Cache als lokale DNS-Auflösung festzulegen.
* **OpenLogic (verwendet NetworkManager)**:
  * Installieren Sie das dnsmasq-Paket mit `sudo yum install dnsmasq`.
  * Aktivieren Sie den dnsmasq-Dienst mit `systemctl enable dnsmasq.service`.
  * Starten Sie den dnsmasq-Dienst mit `systemctl start dnsmasq.service`.
  * Fügen Sie *prepend domain-name-servers 127.0.0.1;* zu **/etc/dhclient-eth0.conf** hinzu.
  * Starten Sie den Netzwerkdienst mit `service network restart` neu, um den Cache als lokale DNS-Auflösung festzulegen.

> [!NOTE]
> Das dnsmasq-Paket ist nur einer der vielen DNS-Caches, die für Linux verfügbar sind. Bevor Sie es nutzen, überprüfen Sie dessen Eignung für Ihre besonderen Bedürfnisse und außerdem, ob kein anderer Cache installiert ist.
> 
> 
    
### <a name="client-side-retries"></a>Clientseitige Wiederholungsversuche

DNS ist in erster Linie ein UDP-Protokoll. Da das UDP-Protokoll keine Nachrichtenübermittlung garantiert, wird die Wiederholungslogik im DNS-Protokoll selbst behandelt. Jeder DNS-Client (Betriebssystem) kann je nach Bevorzugung des Erstellers eine unterschiedliche Wiederholungslogik aufweisen:

* Windows-Betriebssysteme starten nach einer Sekunde einen Wiederholungsversuch und dann erneut nach weiteren zwei, vier und weiteren vier Sekunden. 
* Das standardmäßige Linux-Setup führt nach fünf Sekunden einen Wiederholungsversuch aus. Es wird empfohlen, die Wiederholungsversuche in fünf Versuche in Intervallen von einer Sekunde zu ändern.

Überprüfen Sie die aktuellen Einstellungen auf einem virtuellen Linux-Computer mit `cat /etc/resolv.conf`. Sehen Sie sich beispielsweise die Zeile *options* an:

```bash
options timeout:1 attempts:5
```

Die Datei "resolv.conf" wird normalerweise automatisch generiert und sollte nicht bearbeitet werden. Die entsprechenden Schritte zum Hinzufügen der Zeile *options* variieren je nach Distribution:

* **Ubuntu** (verwendet resolvconf):
  * Fügen Sie in **/etc/resolveconf/resolv.conf.d/head** die Zeile „options“ hinzu.
  * Führen Sie `resolvconf -u` zum Aktualisieren aus.
* **SUSE** (verwendet netconf):
  * Fügen Sie *timeout:1 attempts:5* dem Parameter **NETCONFIG_DNS_RESOLVER_OPTIONS=""** in **/etc/sysconfig/network/config** hinzu. 
  * Führen Sie `netconfig update` zum Aktualisieren aus.
* **OpenLogic** (verwendet NetworkManager):
  * Fügen Sie *echo "options timeout:1 attempts:5"* zu **/etc/NetworkManager/dispatcher.d/11-dhclient** hinzu. 
  * Aktualisieren Sie mit `service network restart`.

## <a name="name-resolution-using-your-own-dns-server"></a>Namensauflösung mithilfe eines eigenen DNS-Servers

### <a name="vms-and-role-instances"></a>Virtuelle Computer und Rolleninstanzen

Es gibt verschiedene Situationen, in denen die Namensauflösung über die von Azure bereitgestellten Features hinausgeht. Dies kann beispielsweise bei der Verwendung von Active Directory-Domänen oder dann der Fall sein, wenn eine DNS-Auflösung zwischen virtuellen Netzwerken erforderlich ist. Um diese Szenarien abzudecken, bietet Azure Ihnen die Möglichkeit, eigene DNS-Server zu verwenden.

DNS-Server innerhalb eines virtuellen Netzwerks können DNS-Abfragen an die rekursiven Resolver von Azure weiterleiten, um Hostnamen innerhalb dieses virtuellen Netzwerks aufzulösen. Beispielsweise kann ein in Azure ausgeführter Domänencontroller (DC) auf DNS-Abfragen für die eigenen Domänen antworten und alle anderen Abfragen an Azure weiterleiten. Durch das Weiterleiten von Abfragen sind sowohl Ihre lokalen Ressourcen (über den DC) als auch die von Azure bereitgestellten Hostnamen (über die Weiterleitung) für die virtuellen Computer sichtbar. Der Zugriff auf die rekursiven Resolver von Azure wird über die virtuelle IP 168.63.129.16 bereitgestellt.

Durch die DNS-Weiterleitung wird außerdem eine DNS-Auflösung zwischen virtuellen Netzwerken ermöglicht, sodass die lokalen Computer von Azure bereitgestellte Hostnamen auflösen können. Um einen Hostnamen eines virtuellen Computers aufzulösen, muss sich die DNS-Server-VM im selben virtuellen Netzwerk befinden und zur Weiterleitung von Abfragen für Hostnamen an Azure konfiguriert sein. Da jedes virtuelle Netzwerk ein eigenes DNS-Suffix verwendet, können Sie mithilfe von Regeln für die bedingte Weiterleitung DNS-Abfragen zur Auflösung an das richtige virtuelle Netzwerk senden. In der folgenden Abbildung sind zwei virtuelle Netzwerke und ein lokales Netzwerk dargestellt, für die eine DNS-Auflösung zwischen virtuellen Netzwerken mit dieser Methode vorgenommen wird. Ein Beispiel für eine DNS-Weiterleitung steht im [Azure-Katalog mit Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) und auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder) zur Verfügung.

> [!NOTE]
> Rolleninstanzen können die Namensauflösung virtueller Computer innerhalb des gleichen virtuellen Netzwerks mit dem FQDN ausführen, der den Namen des virtuellen Computers zusammen mit dem DNS-Suffix „internal.cloudapp.net“ verwendet. In diesem Fall ist die Namensauflösung jedoch nur erfolgreich, wenn für die Rolleninstanz der Namen des virtuellen Computers im [Rollenschema (CSCFG-Datei)](https://msdn.microsoft.com/library/azure/jj156212.aspx) definiert ist. 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Rolleninstanzen, die eine Namensauflösung virtueller Computer in einem anderen virtuellen Netzwerk durchführen müssen (FQDN mit dem Suffix **internal.cloudapp.net**), müssen dies über benutzerdefinierte DNS-Server vornehmen, die für eine Weiterleitung zwischen den beiden virtuellen Netzwerken sorgen, wie es in diesem Abschnitt beschrieben ist.
>

![DNS zwischen virtuellen Netzwerken](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Bei Verwendung der von Azure bereitgestellten Namensauflösung wird jedem virtuellen Computer durch DHCP von Azure ein internes DNS-Suffix (`*.internal.cloudapp.net`) bereitgestellt. Dieses Suffix ermöglicht die Auflösung von Hostnamen, da sich die Einträge für die Hostnamen in der Zone *internal.cloudapp.net* befinden. Wenn Sie eine eigene Lösung für die Namensauflösung verwenden, wird dieses Suffix nicht für die virtuellen Computer bereitgestellt, weil es Konflikte mit anderen DNS-Architekturen verursacht (z. B. in Szenarien mit Domäneneinbindung). Stattdessen stellt Azure einen nicht funktionsfähigen Platzhalter bereit (*reddog.microsoft.com*).

Bei Bedarf kann das interne DNS-Suffix mithilfe von PowerShell oder mit der API ermittelt werden:

* In virtuellen Netzwerken in Resource Manager-Bereitstellungsmodellen steht das Suffix über die [Netzwerkschnittstellenkarte](virtual-network-network-interface.md) oder das Cmdlet [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) zur Verfügung.
* In klassischen Bereitstellungsmodellen steht das Suffix über einen Aufruf von [Get Deployment-API](https://msdn.microsoft.com/library/azure/ee460804.aspx) oder das Cmdlet [Get-AzureVM -Debug](/powershell/module/azure/get-azurevm) zur Verfügung.

Wenn eine Abfrageweiterleitung an Azure nicht Ihren Anforderungen entspricht, müssen Sie eine eigene DNS-Lösung bereitstellen. Die DNS-Lösung muss Folgendes leisten:

* Bereitstellung einer geeigneten Hostnamensauflösung, z. B. über [DDNS](virtual-networks-name-resolution-ddns.md). Beachten Sie, dass Sie bei der Verwendung von DDNS möglicherweise die DNS-Eintragsbereinigung deaktivieren müssen, da die DHCP-Leases von Azure lange gültig sind und die DNS-Einträge durch eine Bereinigung möglicherweise zu früh entfernt werden. 
* Bereitstellung einer geeigneten rekursiven Lösung, um eine Auflösung externer Domänennamen zu ermöglichen.
* Möglichkeit zum Zugriff (TCP und UDP auf Port 53) von den Clients, die Dienste beziehen, und Internetzugriff.
* Schutz vor einem Zugriff aus dem Internet, um mögliche Bedrohungen durch externe Agents zu minimieren.

> [!NOTE]
> Für eine optimale Leistung bei Verwendung von virtuellen Azure-Computern als DNS-Server sollte IPv6 deaktiviert und eine [öffentliche IP-Adresse auf Instanzebene](virtual-networks-instance-level-public-ip.md) jedem virtuellen DNS-Servercomputer zugewiesen werden. Zusätzliche Informationen zur Leistungsanalyse und zu Optimierungsmöglichkeiten bei Verwendung von Windows Server als DNS-Server finden Sie im Artikel zur [Leistung der Namensauflösung bei einem rekursiven Windows DNS-Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web-Apps
Wenn Sie eine Namensauflösung von Ihrer App Service-Web-App, die mit einem virtuellen Netzwerk verknüpft ist, in virtuelle Computer im gleichen virtuellen Netzwerk ausführen müssen, führen Sie zusätzlich zur Einrichtung eines benutzerdefinierten DNS-Servers, der über eine DNS-Weiterleitung für Abfragen an Azure verfügt (virtuelle IP 168.63.129.16), auch die folgenden Schritte aus:
* Aktivieren Sie die Integration virtueller Netzwerke für Ihre App Service-Web-App (falls noch nicht geschehen), wie es unter [Integrieren Ihrer App in ein virtuelles Netzwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) beschrieben ist.
* Wählen Sie im Azure-Portal für den AppService-Plan, in dem die Web-App gehostet wird, unter **Netzwerk** > **Integration des virtuellen Netzwerks** die Option **Netzwerk synchronisieren** aus, wie es in der folgenden Abbildung gezeigt ist:

    ![Namensauflösung für virtuelles Netzwerk für Web-Apps](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Für die Namensauflösung von Ihrer App Service-Web-App, die mit einem virtuellen Netzwerk verknüpft ist, in virtuelle Computer in einem anderen virtuellen Netzwerk müssen benutzerdefinierte DNS-Server in beiden virtuellen Netzwerken auf folgende Weise verwendet werden:
* Richten Sie einen DNS-Server im virtuellen Zielnetzwerk auf einem virtuellen Computer ein, der auch Abfragen an den rekursiven Resolver von Azure (virtuelle IP 168.63.129.16) weiterleiten kann. Ein Beispiel für eine DNS-Weiterleitung steht im [Azure-Katalog mit Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) und auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder) zur Verfügung. 
* Richten Sie eine DNS-Weiterleitung im virtuellen Quellnetzwerk auf einem virtuellen Computer ein. Konfigurieren Sie diese DNS-Weiterleitung für das Weiterleiten von Abfragen an den DNS-Server in Ihrem virtuellen Zielnetzwerk.
* Konfigurieren Sie den DNS-Quellserver unter den Einstellungen für Ihr virtuelles Quellnetzwerk.
* Aktivieren Sie die Integration virtueller Netzwerke für Ihre App Service-Web-App, um eine Verknüpfung mit dem virtuellen Quellnetzwerk herzustellen. Befolgen Sie dazu die Anweisungen unter [Integrieren Ihrer App in ein virtuelles Netzwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Wählen Sie im Azure-Portal für den AppService-Plan, in dem die Web-App gehostet wird, unter **Netzwerk** > **Integration des virtuellen Netzwerks** die Option **Netzwerk synchronisieren** aus. 

## <a name="specifying-dns-servers"></a>Angeben von DNS-Servern
Wenn Sie eigene DNS-Server verwenden, bietet Azure die Möglichkeit, für jedes virtuelle Netzwerk oder jede Netzwerkschnittstelle (Resource Manager)/jeden Clouddienst (klassisch) mehrere DNS-Server anzugeben. Die für einen Clouddienst/eine Netzwerkschnittstelle angegebenen DNS-Server haben Vorrang gegenüber den für das virtuelle Netzwerk angegebenen DNS-Servern.

> [!NOTE]
> Netzwerkverbindungseigenschaften wie DNS-Server-IP-Adressen sollten nicht direkt in Windows-VMs bearbeitet werden, da sie bei Dienstwartungen gelöscht werden können, wenn der virtuelle Netzwerkadapter ersetzt wird. 
> 
> 

Wenn Sie das Resource Manager-Bereitstellungsmodell verwenden, können DNS-Server im Portal, mit der API/mit Vorlagen ([Virtuelles Netzwerk](https://msdn.microsoft.com/library/azure/mt163661.aspx) und [Netzwerkschnittstelle](https://msdn.microsoft.com/library/azure/mt163668.aspx)) oder mit PowerShell ([Virtuelles Netzwerk](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) und [Netzwerkschnittstelle](/powershell/module/azurerm.network/new-azurermnetworkinterface)) angegeben werden.

Wenn Sie das klassische Bereitstellungsmodell verwenden, können DNS-Server für das virtuelle Netzwerk im Portal oder [in der *Netzwerkkonfigurationsdatei*](https://msdn.microsoft.com/library/azure/jj157100) angegeben werden. Für Clouddienste werden DNS-Server über die[ *Dienstkonfigurationsdatei*](https://msdn.microsoft.com/library/azure/ee758710) oder mithilfe von PowerShell mit [New-AzureVM](/powershell/module/azure/new-azurevm) angegeben.

> [!NOTE]
> Wenn Sie die DNS-Einstellungen für virtuelle Netzwerke oder virtuelle Computer ändern, die bereits bereitgestellt wurden, müssen Sie alle betroffenen virtuellen Computer neu starten, damit die Änderungen wirksam werden.
> 
> 

## <a name="next-steps"></a>Nächste Schritte

Resource Manager-Bereitstellungsmodell

* [Erstellen oder Aktualisieren eines virtuellen Netzwerks](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Erstellen oder Aktualisieren einer Netzwerkschnittstellenkarte](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)
* [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)

Klassisches Bereitstellungsmodell:

* [Azure-Dienstkonfigurationsschema (.cscfg-Datei)](https://msdn.microsoft.com/library/azure/ee758710)
* [Konfigurationsschema für virtuelle Netzwerke](https://msdn.microsoft.com/library/azure/jj157100)
* [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md)
