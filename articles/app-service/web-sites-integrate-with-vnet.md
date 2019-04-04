---
title: Integrieren einer App in ein Azure Virtual Network – Azure App Service
description: Sie erfahren, wie Sie eine Verbindung zwischen einer App in Azure App Service und einem neuen oder vorhandenen Azure Virtual Network herstellen.
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: ed99bd3626bb44bff68e4122d6b50523f19e1797
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58112618"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrieren Ihrer App in ein Azure Virtual Network
In diesem Dokument wird die Azure App Service-Funktion für die Integration in ein Virtual Network beschrieben und veranschaulicht, wie Sie diese mit Apps in [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714)einrichten. Mit [Azure Virtual Networks][VNETOverview] (VNETs) können Sie viele Ihrer Azure-Ressourcen in einem Netzwerk platzieren, das nicht über das Internet geroutet werden kann. Diese Netzwerke können dann durch VPN-Technologien mit Ihren lokalen Netzwerken verbunden werden. 

Es gibt zwei Methoden, um Azure App Service zu nutzen. 

1. Mehrinstanzenfähige Systeme, die den gesamten Bereich der Tarife unterstützen, mit Ausnahme von Isoliert
2. App Service-Umgebung (App Service Environment, ASE), die in Ihrem VNET bereitgestellt wird 

In diesem Dokument wird das VNET-Integrationsfeature verwendet, das für die Verwendung im mehrinstanzenfähigen App Service vorgesehen ist.  In der [App Service-Umgebung][ASEintro] befindet sich Ihre App bereits in einem VNET und benötigt das VNET-Integrationsfeature nicht, um Ressourcen im selben VNET zu erreichen.

Die VNET-Integration ermöglicht Ihrer Web-App den Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, gewährt aber keinen privaten Zugriff auf Ihre Web-App aus dem virtuellen Netzwerk. Privater Websitezugriff bezieht sich darauf, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z.B. aus einem virtuellen Azure-Netzwerk heraus. Der private Websitezugriff ist nur verfügbar, wenn eine ASE mit einem internen Lastenausgleichsmodul (Internal Load Balancer, ILB) konfiguriert ist. Ausführlichere Informationen zum Verwenden einer ILB-ASE finden Sie hier: [Erstellen und Verwenden einer ILB-ASE][ILBASE]. 

VNET-Integration wird häufig verwendet, um Zugriff von Apps auf Datenbanken und Webdienste in Ihrem VNET zu ermöglichen. Bei der VNET-Integration müssen Sie keinen öffentlichen Endpunkt für Anwendungen auf Ihrer VM verfügbar machen, sondern Sie können stattdessen die privaten nicht über das Internet routbaren Adressen verwenden. 

Für die Funktion für die VNET-Integration gilt Folgendes:

* erfordert einen der Tarife Tarif Standard, Premium oder PremiumV2 
* funktioniert mit klassischem Modus oder Resource Manager-VNET 
* TCP und UDP wird unterstützt
* funktioniert mit Web-, mobilen, API- und Funktions-Apps
* eine App kann nur jeweils mit einem VNET eine Verbindung herstellen
* bis zu fünf VNETs können in einen App Service-Plan integriert werden 
* ein VNET kann im Rahmen eines App Service-Plans von mehreren Apps verwendet werden
* erfordert ein virtuelles Netzwerkgateway, das mit Point-to-Site-VPN konfiguriert ist
* unterstützt dank der SLA auf dem Gateway eine SLA mit einer Verfügbarkeit von 99,9 %

Einige Dinge werden von der VNET-Integration nicht unterstützt, z.B.:

* Bereitstellung eines Laufwerks
* AD-Integration 
* NetBios
* Privater Websitezugriff
* Zugriff auf Ressourcen über ExpressRoute 
* Zugriff auf Ressourcen über Dienstendpunkte 

### <a name="getting-started"></a>Erste Schritte
Beachten Sie Folgendes, bevor Sie Ihre Web-App mit einem virtuellen Netzwerk verbinden:

* Ein virtuelles Zielnetzwerk muss über ein Point-to-Site-VPN mit einem dynamischen Routinggateway aktiviert werden, bevor es mit einer App verbunden werden kann. 
* Das VNET muss sich im gleichen Abonnement befinden wie Ihr App Service-Plan (ASP).
* Die Apps, die in ein VNET integriert sind, nutzen das DNS, das für das VNET angegeben ist.

## <a name="enabling-vnet-integration"></a>Aktivieren der VNET-Integration

Es gibt eine neue Version des VNET-Integrationsfeatures, das sich in der Vorschau befindet. Es ist nicht von Point-to-Site-VPN abhängig und unterstützt auch den Zugriff auf Ressourcen über ExpressRoute oder Dienstendpunkte. Weitere Informationen zu der neuen Vorschaufunktion finden Sie am Ende dieses Dokuments. 

### <a name="set-up-a-gateway-in-your-vnet"></a>Einrichten eines Gateways in Ihrem VNET ###

Wenn Sie bereits über ein Gateway verfügen, das mit Point-to-Site-Adressen konfiguriert ist, können Sie mit dem Konfigurieren der VNET-Integration mit Ihrer App fortfahren.  
So erstellen Sie ein Gateway

1. [Erstellen Sie ein Gatewaysubnetz][creategatewaysubnet] in Ihrem VNET.  

1. [Erstellen Sie das VPN-Gateway][creategateway]. Wählen Sie einen routenbasierten VPN-Typ aus.

1. [Legen Sie die Point-to-Site-Adressen fest][setp2saddresses]. Wenn sich das Gateway nicht in der Basic-SKU befindet, muss IKEV2 in der Point-to-Site-Konfiguration deaktiviert und SSTP ausgewählt werden. Der Adressraum muss sich in einem der folgenden Adressblöcke befinden:

* 10.0.0.0/8 – entspricht einem IP-Adressbereich von 10.0.0.0 bis 10.255.255.255
* 172.16.0.0/12 – entspricht einem IP-Adressbereich von 172.16.0.0 bis 172.31.255.255 
* 192.168.0.0/16 – entspricht einem IP-Adressbereich von 192.168.0.0 bis 192.168.255.255

Wenn Sie das Gateway nur für die Verwendung mit der App Service-VNET-Integration erstellen, müssen Sie kein Zertifikat hochladen. Das Erstellen des Gateways kann 30 Minuten dauern. Sie können Ihre App erst mit Ihrem VNET integrieren, nachdem das Gateway bereitgestellt wurde. 

### <a name="configure-vnet-integration-with-your-app"></a>Konfigurieren der VNET-Integration mit Ihrer App ###

So aktivieren Sie die VNET-Integration in Ihrer App 

1. Wechseln Sie im Azure-Portal zu Ihrer App, öffnen Sie die App-Einstellungen, und wählen Sie „Netzwerke > VNET-Integration“ aus. Sie müssen Ihre ASP auf eine Standard-SKU oder besser skalieren, bevor Sie die VNET-Integration konfigurieren können. 
 ![Benutzeroberfläche der VNET-Integration][1]

1. Wählen Sie **VNET hinzufügen** aus. 
 ![Hinzufügen der VNET-Integration][2]

1. Wählen Sie Ihr VNET aus. 
  ![Auswählen Ihres VNET][8]
  
Nach diesem letzten Schritt wird Ihre App neu gestartet.  

## <a name="how-the-system-works"></a>Wie das System funktioniert
Das VNET-Integrationsfeature basiert auf Point-to-Site-VPN-Technologie. Apps in Azure App Service werden in einem mehrinstanzenfähigen System gehostet, sodass die Bereitstellung einer App direkt in einem VNET nicht möglich ist. Die Point-to-Site-Technologie beschränkt den Netzwerkzugriff auf den virtuellen Computer, auf dem die App gehostet wird. Apps sind darauf beschränkt, nur Datenverkehr an das Internet über Hybrid Connections oder VNET-Integration zu senden. 

![Funktionsweise der VNET-Integration][3]

## <a name="managing-the-vnet-integrations"></a>Verwalten von VNET-Integrationen
Die Möglichkeit, eine Verbindung mit einem VNET herzustellen und zu trennen, ist auf der App-Ebene angesiedelt. Vorgänge, die sich auf die VNET-Integration über mehrere Apps auswirken können, werden auf Ebene des App Service-Plans ausgeführt. Der App-UID können Sie Details zu Ihrem VNET entnehmen. Die gleichen Informationen werden auch auf Ebene des App Service-Plans angezeigt. 

 ![Details zum VNET][4]

Auf der Seite „Netzwerkfunktionsstatus“ können Sie sehen, ob für die App eine Verbindung mit Ihrem VNET besteht. Falls Ihr VNET-Gateway ausgefallen ist, wird der Status „Nicht verbunden“ angezeigt. 

Die Informationen, die für Sie jetzt auf der Benutzeroberfläche für die VNET-Integration auf App-Ebene verfügbar sind, entsprechen den Detailinformationen, die Sie über den App Service-Plan erhalten. Dies sind die folgenden Elemente:

* VNET-Name: Link zur Benutzeroberfläche des virtuellen Netzwerks
* Standort: gibt den Standort Ihres VNET an. Die Integration mit einem VNET an einem anderen Standort kann Latenzprobleme für Ihre App verursachen. 
* Zertifikatstatus: gibt an, ob Ihre Zertifikate zwischen Ihrem App Service-Plan und dem VNET synchron sind.
* Gatewaystatus: Wenn die Gateways ausgefallen sind, kann Ihre App nicht auf Ressourcen im VNET zugreifen. 
* VNET-Adressraum: zeigt den IP-Adressraum für Ihr VNET an. 
* Punkt-zu-Standort-Adressraum: zeigt den Point-to-Site-IP-Adressraum für Ihr VNET an. Wenn Sie Aufrufe an Ihr VNET ausführen, ist die FROM-Adresse Ihrer App eine dieser Adressen. 
* Standort-zu-Standort-Adressraum: Sie können Site-to-Site-VPNs verwenden, um Ihr VNET mit lokalen Ressourcen oder den anderen VNETs zu verbinden. Die mit dieser VPN-Verbindung definierten IP-Adressbereiche werden hier dargestellt.
* DNS-Server: zeigt die mit Ihrem VNET konfigurierten DNS-Server.
* IP addresses routed to the VNet (An das VNET weitergeleitete IP-Adressen): zeigt die Adressblöcke, die zur Weiterleitung in Ihr VNET geroutet werden 

Der einzige Vorgang, den Sie in der App-Ansicht Ihrer VNET-Integration durchführen können, ist das Trennen Ihrer App von dem VNET, mit dem derzeit eine Verbindung besteht. Um Ihre App von einem VNET zu trennen, wählen Sie **Verbindung trennen** aus. Wenn Sie die Verbindung mit einem VNET trennen, wird Ihre App neu gestartet. Das Trennen der Verbindung führt nicht zu Änderungen in Ihrem VNET. Das VNET und dessen Konfiguration, einschließlich der Gateways, bleiben unverändert. Wenn Sie Ihr VNET dann löschen möchten, müssen Sie zuerst die darin enthaltenen Ressourcen löschen, z.B. auch die Gateways. 

Um die Benutzeroberfläche der ASP-VNET-Integration zu erreichen, öffnen Sie die ASP-Benutzeroberfläche, und wählen Sie **Netzwerk** aus.  Wählen Sie unter „VNET-Integration“ die Option **Zum Konfigurieren hier klicken** aus, um die Netzwerkfeaturestatus-Benutzeroberfläche zu öffnen.

![Informationen zur ASP-VNET-Integration][5]

Die Benutzeroberfläche der ASP-VNET-Integration zeigt alle VNETs an, die von den Apps in Ihrem ASP verwendet werden. Es können bis zu 5 VNETs mit einer beliebigen Anzahl von Apps in Ihrem App Service-Plan verbunden sein. Für jede App kann nur eine Integration konfiguriert sein. Klicken Sie auf das gewünschte VNET, um Informationen dazu anzuzeigen. Sie können hier zwei Aktionen durchführen.

* **Netzwerk synchronisieren**. Durch den Synchronisierungsvorgang wird sichergestellt, dass Ihre Zertifikate und Netzwerkinformationen synchronisiert sind. Wenn Sie das DNS des VNET hinzufügen oder ändern, müssen Sie eine **Netzwerksynchronisierung** durchführen. Bei diesem Vorgang werden alle Apps, die dieses VNET verwenden, neu gestartet.
* **Routen hinzufügen:** Durch das Hinzufügen von Routen wird ausgehender Datenverkehr in Ihr VNET geleitet.

**Routing:** Die in Ihrem VNET definierten Routen werden zum Leiten des Datenverkehrs aus der App in Ihr VNET verwendet. Wenn Sie weiteren ausgehenden Datenverkehr in das VNET senden möchten, können Sie diese Adressblöcke hier hinzufügen.   

**Zertifikate:** Wenn VNET-Integration aktiviert ist, müssen Zertifikate ausgetauscht werden, um die Sicherheit der Verbindung sicherzustellen. Zusammen mit den Zertifikaten werden die DNS-Konfiguration, Routen und anderen Elemente, mit denen das Netzwerk beschrieben wird, ausgetauscht.
Wenn Zertifikate oder Netzwerkinformationen geändert werden, müssen Sie auf „Netzwerk synchronisieren“ klicken. Wenn Sie auf „Netzwerk synchronisieren“ klicken, bewirkt dies einen kurzen Ausfall der Verbindung zwischen der App und dem VNET. Die App wird nicht neu gestartet, aber der Konnektivitätsverlust kann dazu führen, dass Ihre Website nicht richtig funktioniert. 

## <a name="accessing-on-premises-resources"></a>Zugriff auf lokale Ressourcen
Apps können durch Integration mit VNETs, die Site-to-Site-Verbindungen aufweisen, auf lokale Ressourcen zugreifen. Um auf lokale Ressourcen zuzugreifen, müssen Sie Ihre lokalen VPN-Gatewayrouten mit Ihren Point-to-Site-Adressblöcken aktualisieren. Verwenden Sie beim ersten Einrichten des Site-to-Site-VPN die Skripts für die Konfiguration. Die Routen sollten damit richtig eingerichtet werden. Wenn Sie die Point-to-Site-Adressen nach dem Erstellen des Site-to-Site-VPN hinzufügen, müssen Sie die Routen manuell aktualisieren. Die Details zur Vorgehensweise variieren je nach Gateway und sind hier nicht beschrieben. Sie können zudem nicht BGP mit einer Site-to-Site-VPN-Verbindung konfigurieren.

> [!NOTE]
> Das VNET-Integrationsfeature integriert eine VNET-App nicht mit einem ExpressRoute-Gateway. Selbst wenn das ExpressRoute-Gateway im [Koexistenzmodus][VPNERCoex] konfiguriert ist, funktioniert die VNET-Integration nicht. Wenn Sie über eine ExpressRoute-Verbindung auf Ressourcen zugreifen müssen, können Sie dazu eine [App Service-Umgebung][ASE] nutzen, die in Ihrem VNET ausgeführt werden kann. 
> 
> 

## <a name="peering"></a>Peering
Sie können mit der VNET-Integration auf Ressourcen in VNETs zugreifen, die mit dem VNET, mit dem Sie verbunden sind, mittels Peering verknüpft sind. So konfigurieren Sie Peering für Ihre App

1. Fügen Sie eine Peeringverbindung mit dem VNET hinzu, mit dem Ihre App eine Verbindung herstellt. Aktivieren Sie beim Hinzufügen der Peeringverbindung **Zugriff auf virtuelles Netzwerk zulassen** sowie **Weitergeleiteten Datenverkehr zulassen** und **Gatewaytransit zulassen**.
1. Fügen Sie in dem VNET, das mit dem VNET, mit dem Sie verbunden sind, mittels Peering verknüpft ist, eine Peeringverbindung hinzu. Aktivieren Sie beim Hinzufügen der Peeringverbindung im Ziel-VNET **Zugriff auf virtuelles Netzwerk zulassen**, und aktivieren Sie **Weitergeleiteten Datenverkehr zulassen** und **Allow remote gateways** (Remotegateways zulassen).
1. Wechseln Sie im Portal zu „App Service-Plan > Netzwerk > VNet Integration UI“ (Benutzeroberfläche der VNET-Integration).  Wählen Sie das VNET aus, mit dem Ihre App eine Verbindung herstellt. Fügen Sie den Adressbereich des VNET, das mit dem VNET, mit dem Ihre App verbunden ist, mittels Peering verknüpft ist, im Abschnitt „Routing“ hinzu.  


## <a name="pricing-details"></a>Preisübersicht
Es gibt drei Gebühren für die Verwendung des VNET-Integrationsfeatures:

* ASP-Tarifanforderungen
* Datenübertragungskosten
* Kosten für VPN Gateway

Ihre Apps müssen sich in einem der App Service-Pläne Standard, Premium oder PremiumV2 befinden. Weitere Informationen zu diesen Kosten finden Sie hier: [App Service – Preise][ASPricing]. 

Es gibt eine Gebühr für ausgehende Daten, auch wenn sich das VNET im selben Rechenzentrum befindet. Diese Kosten werden unter [Datenübertragung – Preisübersicht][DataPricing] beschrieben. 

Es gibt Kosten für das VNET-Gateway, das für das Point-to-Site-VPN erforderlich ist. Die Details finden Sie auf der Seite [VPN Gateway – Preise][VNETPricing].

## <a name="troubleshooting"></a>Problembehandlung
Die Funktion ist zwar einfach einzurichten, aber dies bedeutet nicht, dass für Ihre Benutzeroberfläche keinerlei Probleme auftreten. Falls beim Zugreifen auf den gewünschten Endpunkt Probleme auftreten, können Sie einige Hilfsprogramme verwenden, um die Verbindung über die App-Konsole zu testen. Sie können zwei Konsolen verwenden. Eine ist die Kudu-Konsole, und die andere ist die Konsole im Azure-Portal. Greifen Sie in der App auf „Tools -> Kudu“ zu, um zur Kudu-Konsole zu gelangen. Dies entspricht dem Zugreifen auf „[Websitename].scm.azurewebsites.net“. Wechseln Sie nach dem Öffnen zur Konsolenregisterkarte „Debuggen“. Um auf die über das Azure-Portal gehostete Konsole zuzugreifen, greifen Sie in der App auf „Tools“ -> „Konsole“ zu. 

#### <a name="tools"></a>Tools
Die Tools **ping**, **nslookup** und **tracert** funktionieren aufgrund von Sicherheitseinschränkungen nicht über die Konsole. Es wurden zwei separate Tools hinzugefügt, um diese Lücke zu füllen. Zum Testen der DNS-Funktionalität haben wir ein Tool mit dem Namen „nameresolver.exe“ hinzugefügt. Die Syntax ist:

    nameresolver.exe hostname [optional: DNS Server]

Sie können **nameresolver** verwenden, um die Hostnamen zu überprüfen, von denen Ihre App abhängig ist. So können Sie testen, ob für das DNS etwas falsch konfiguriert ist oder ob ggf. kein Zugriff auf Ihren DNS-Server besteht.

Mit dem nächsten Tool können Sie die TCP-Verbindung mit einer Host/Port-Kombination testen. Dieses Tool hat den Namen **tcpping** und die folgende Syntax:

    tcpping.exe hostname [optional: port]

Das **tcpping**-Hilfsprogramm teilt Ihnen mit, ob Sie einen bestimmten Host und Port erreichen können. Es kann nur unter folgenden Bedingungen erfolgreich ausgeführt werden: Eine Anwendung lauscht auf der Host- und Portkombination, und von Ihrer App aus ist Netzwerkzugriff auf den angegebenen Host und Port möglich.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Debuggen des Zugriffs auf im VNET gehostete Ressourcen
Es kann verschiedene Ursachen haben, warum Ihre App einen bestimmten Host und Port nicht erreichen kann. In den meisten Fällen liegt eine der drei folgenden Ursachen vor:

* **Eine Firewall.** Falls eine Firewall den Zugriff verhindert, wird das TCP-Timeout ausgelöst. Das TCP-Timeout entspricht hier 21 Sekunden. Überprüfen Sie die Verbindung mithilfe des **tcpping**-Tools. TCP-Timeouts können zwar auch zahlreiche andere Ursachen haben, es empfiehlt sich allerdings, bei der Firewall zu beginnen. 
* **Kein DNS-Zugriff.** Das DNS-Timeout beträgt drei Sekunden pro DNS-Server. Wenn Sie zwei DNS-Server besitzen, beträgt das Timeout 6 Sekunden. Überprüfen Sie mithilfe des nameresolver-Tools, ob DNS funktioniert. Zur Erinnerung: Das nslookup-Tool kann nicht verwendet werden, da es nicht das DNS verwendet, mit dem Ihr VNET konfiguriert ist.
* **Ungültiger P2S-Adressbereich.** Der Point-to-Site-IP-Adressbereich muss in den privaten IP-Adressbereichen gemäß RFC 1918 liegen (10.0.0.0–10.255.255.255/172.16.0.0–172.31.255.255/192.168.0.0–192.168.255.255). Wenn der Bereich außerhalb liegende IP-Adressen verwendet, funktioniert es nicht. 

Sollte das Problem weiterhin bestehen, prüfen Sie zunächst einige einfache Dinge wie etwa: 

* Wird im Portal angezeigt, dass das Gateway ausgeführt wird?
* Wird angezeigt, dass die Zertifikate synchron sind?
* Wurde die Netzwerkkonfiguration geändert, ohne für die betroffenen ASPs die Funktion „Netzwerk synchronisieren“ auszuführen? 

Fahren Sie das Gateway hoch, wenn es heruntergefahren ist. Falls Ihre Zertifikate nicht synchron sind, müssen Sie in der ASP-Ansicht Ihrer VNET-Integration die Funktion „Netzwerk synchronisieren“ ausführen. Wenn Sie vermuten, dass an Ihrer VNET-Konfiguration eine Änderung vorgenommen wurde, die nicht mit Ihren ASPs synchronisiert wurde, klicken Sie auf „Netzwerk synchronisieren“.  Bei einer Netzwerksynchronisation werden alle Apps im ASP, die mit diesem VNET integriert sind, neu gestartet. 

Wenn dies nicht die Lösung des Problems ist, müssen Sie weiterforschen:

* Sind andere Apps vorhanden, die die VNET-Integration verwenden, um Ressourcen im gleichen VNET zu erreichen? 
* Können Sie andere Ressourcen in Ihrem VNET mithilfe von „tcpping“ über die App-Konsole erreichen? 

Wenn Sie beide obigen Fragen mit „ja“ beantworten können, liegt für Ihre VNET-Integration kein Problem vor. Das Problem hat also eine andere Ursache. Dieser Fall ist etwas schwieriger zu lösen, weil Sie nicht leicht ermitteln können, warum eine host:port-Kombination nicht erreichbar ist. Mögliche Ursachen sind:

* Aktivierte Firewall auf Ihrem Host, die den Zugriff auf den Anwendungsport über Ihren Punkt-zu-Standort-IP-Bereich verhindert Gegebenenfalls erforderlicher öffentlicher Zugriff für die Durchquerung von Subnetzen
* Zielhost ist ausgefallen
* Anwendung ist nicht verfügbar
* Falsche IP-Adresse oder falscher Hostname
* Ihre Anwendung lauscht über andere Ports als von Ihnen erwartet. Sie können Ihre Prozess-ID auf den lauschenden Port festlegen, indem Sie auf dem Endpunkthost „netstat -aon“ verwenden. 
* Netzwerksicherheitsgruppen sind so konfiguriert, dass der Zugriff auf Ihren Anwendungshost und -port aus Ihrem Punkt-zu-Standort-IP-Bereich verhindert wird

Beachten Sie, dass Sie nicht wissen, welche IP-Adresse Ihres Point-to-Site-IP-Bereichs die App verwenden wird. Sie müssen also den Zugriff für den gesamten Bereich zulassen. 

Weitere Debugschritte:

* Stellen Sie eine Verbindung mit einer VM im VNET her, und versuchen Sie, die Ressource host:port von dort aus zu erreichen. Um den TCP-Zugriff zu testen, verwenden Sie den PowerShell-Befehl **test-netconnection**. Die Syntax ist:

      test-netconnection hostname [optional: -Port]

* Rufen Sie eine Anwendung auf einer VM auf, und testen Sie den Zugriff auf den Host und Port über die Konsole der App.

#### <a name="on-premises-resources"></a>Lokale Ressourcen ####

Wenn Ihre App eine Ressource lokal nicht erreichen kann, überprüfen Sie, ob Sie die Ressource über Ihr VNET erreichen können. Verwenden Sie den PowerShell-Befehl **test-netconnection**, um den TCP-Zugriff zu überprüfen. Wenn die VM Ihre lokale Ressource nicht erreichen kann, stellen Sie sicher, dass Ihre Standort-zu-Standort-VPN-Verbindung funktioniert. Wenn dies der Fall ist, sollten Sie die oben angegebenen Punkte sowie die Konfiguration und den Status des lokalen Gateways überprüfen. 

Wenn die im VNET gehostete VM auf ein lokales System zugreifen kann, die App jedoch nicht, trifft wahrscheinlich einer der folgenden Gründe zu:

* Die Routen des lokalen Gateways enthalten keine Punkt-zu-Standort-IP-Bereiche
* Die Netzwerksicherheitsgruppen blockieren den Zugriff auf den Point-to-Site-IP-Bereich.
* Die lokalen Firewalls blockieren den Datenverkehr des Point-to-Site-IP-Bereichs.


## <a name="powershell-automation"></a>PowerShell-Automatisierung

Sie können App Service mithilfe von PowerShell in ein virtuelles Azure-Netzwerk integrieren. Ein ausführungsbereites Skript finden Sie unter [Verbinden einer App in Azure App Service mit einem virtuellen Azure-Netzwerk](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).

## <a name="hybrid-connections-and-app-service-environments"></a>Hybridverbindungen und App Service-Umgebungen
Es gibt drei Funktionen, die den Zugriff auf im VNET gehostete Ressourcen ermöglichen. Sie lauten wie folgt:

* VNET-Integration
* Hybridverbindungen
* App Service-Umgebungen

Für Hybridverbindungen müssen Sie in Ihrem Netzwerk einen Relay-Agent installieren, und zwar den Hybrid Connection Manager (HCM). Für den HCM muss eine Verbindung mit Azure und auch mit Ihrer Anwendung hergestellt werden können. Hybrid Connections erfordern keinen für eingehenden Internetdatenverkehr zugänglichen Endpunkt für Ihr Remotenetzwerk, wie dies für eine VPN-Verbindung erforderlich ist. Der HCM kann nur unter Windows ausgeführt werden, und Sie können bis zu fünf Instanzen verwenden, um für Hochverfügbarkeit zu sorgen. Für Hybridverbindungen wird aber nur TCP unterstützt, und für jeden Endpunkt einer Hybridverbindung muss eine bestimmte host:port-Kombination verwendet werden. 

Mit dem Feature für die App Service-Umgebung können Sie eine einzelne Mandanteninstanz von Azure App Service in Ihrem VNET ausführen. Wenn sich Ihre Apps in einer App Service-Umgebung befinden, können sie ohne weitere Schritte auf Ressourcen in Ihrem VNET zugreifen. Mit einer App Service-Umgebung werden Ihre Apps auf leistungsfähigeren Workern ausgeführt und können auf bis zu 100 ASP-Instanzen skaliert werden. App Service-Umgebungen funktionieren mit allen Netzwerkfeatures, einschließlich ExpressRoute und Dienstendpunkten.  

Bei den Anwendungsfällen gibt es zwar einige Überschneidungen, aber diese Funktionen können sich nicht gegenseitig ersetzen. Das Wissen, welche Funktion verwendet werden sollte, richtet sich nach Ihren Anforderungen. Beispiel: 

* Wenn Sie Entwickler sind und einfach eine Website in Azure ausführen möchten, die auf eine Datenbank auf der Arbeitsstation unter Ihrem Schreibtisch zugreifen kann, ist es am einfachsten, Hybrid Connections zu verwenden. 
* Wenn Sie in einem großen Unternehmen arbeiten, das eine größere Zahl von Webeigenschaften in der Public Cloud anordnen und im eigenen Netzwerk verwalten möchte, ist es ratsam, die App Service-Umgebung zu verwenden. 
* Wenn Sie mehrere Apps haben, die Zugriff auf Ressourcen in Ihrem VNET benötigen, ist die VNET-Integration die beste Wahl. 

Wenn Ihr VNET bereits mit dem lokalen Netzwerk verbunden ist, stellen die VNET-Integration oder eine App Service-Umgebung eine einfache Möglichkeit zur Nutzung der lokalen Ressourcen dar. Wenn Ihr VNET nicht mit dem lokalen Netzwerk verbunden ist, bedeutet es gegenüber der Installation des HCM deutlich mehr Aufwand, ein Site-to-Site-VPN mit Ihrem VNET einzurichten. 

Über die Unterschiede bei den Funktionen hinaus gibt es auch Unterschiede bei den Preisen. Die Funktion „App Service-Umgebung“ ist ein Premium-Dienstangebot, umfasst zusätzlich zu anderen hervorragenden Features aber auch die meisten Möglichkeiten zur Netzwerkkonfiguration. Die VNET-Integration kann mit den App Service-Plänen „Standard“ oder „Premium“ verwendet werden und eignet sich ideal zum sicheren Nutzen von Ressourcen in Ihrem VNET per App Service mit Mehrinstanzenfähigkeit. Hybrid Connections sind derzeit von einem BizTalk-Konto abhängig. Die Preise reichen von der anfänglichen kostenlosen Nutzung bis zu den unterschiedlichen höheren Preisstufen, die sich nach der benötigten Menge richten. Was die Nutzung über viele Netzwerke hinweg betrifft, stehen aber Hybrid Connections an erster Stelle. Hiermit können Sie auf Ressourcen in deutlich mehr als 100 separaten Netzwerken zugreifen. 

## <a name="new-vnet-integration"></a>Neue VNET-Integration ##

Es gibt eine neue Version der VNET-Integrationsfunktion, die nicht von der Point-to-Site-VPN-Technologie abhängig ist. Im Gegensatz zu dem bereits vorhandenen Feature funktioniert die neue Vorschaufunktion mit ExpressRoute und Dienstendpunkten. 

Die neue Funktion ist nur für neuere Azure App Service-Skalierungseinheiten verfügbar. Wenn Sie auf PremiumV2 skalieren können, befinden Sie sich in einer neuen App Service-Skalierungseinheit. Auf der Benutzeroberfläche der VNET-Integration im Portal erfahren Sie, ob Ihre App mit dem neuen VNET-Integrationsfeature kompatibel ist. 

Die neue Version befindet sich in der Vorschau und weist folgende Merkmale auf.

* Kein Gateway ist erforderlich, um die neue VNET-Integrationsfunktion zu verwenden.
* Sie können ohne zusätzliche Konfiguration bis auf die Integration mit dem mit ExpressRoute verbundenen VNET über ExpressRoute-Verbindungen auf Ressourcen zugreifen.
* Die App und das VNET müssen sich in der gleichen Region befinden.
* Das neue Feature erfordert ein nicht verwendetes Subnetz im Resource Manager-VNET.
* Ihr App Service-Plan muss einem der Tarife Standard, Premium oder PremiumV2 entsprechen.
* Produktionsworkloads werden für das neue Feature, während es sich in der Vorschauphase befindet, nicht unterstützt.
* Ihre App muss sich in einer Azure App Service-Bereitstellung befinden, die auf PremiumV2 skaliert werden kann.
* Das neue Feature für VNET-Integration funktioniert nicht für Apps in einer App Service-Umgebung.
* Ein VNET mit einer integrierten App kann nicht gelöscht werden.  
* Routingtabellen und globales Peering sind mit der neuen VNET-Integration noch nicht verfügbar.  
* Eine Adresse wird für jede Instanz des App Service-Plans verwendet. Da die Subnetzgröße nach dem Zuweisen nicht mehr geändert werden kann, verwenden Sie ein Subnetz, das mehr als die maximale Skalierungsgröße abdecken kann. Empfohlen wird die Größe /27 mit 32 Adressen, da sie einen App Service-Plan unterstützt, der für 20 Instanzen skaliert ist.
* Mit der neuen VNET-Integrationsfunktion können Sie durch Dienstendpunkte gesicherte Ressourcen nutzen. Aktivieren Sie dazu Dienstendpunkte in dem für die VNET-Integration verwendeten Subnetz.

So verwenden Sie das neue Feature

1. Wechseln Sie zur Netzwerkbenutzeroberfläche im Portal. Wenn Ihre App das neue Feature verwenden kann, wird eine Funktion zum Verwenden des neuen Vorschaufeatures angezeigt.  

   ![Auswählen der neuen VNET-Integration (Vorschau)][6]

1. Wählen Sie **VNET hinzufügen (Vorschau)** aus.  

1. Wählen Sie das Resource Manager-VNET aus, das Sie integrieren möchten, und erstellen Sie dann ein neues Subnetz, oder wählen Sie ein vorhandenes leeres Subnetz aus. Die Integration dauert weniger als eine Minute. Während der Integration wird Ihre App neu gestartet.  Nach Abschluss der Integration werden Details für das VNET, mit dem Sie integriert sind, angezeigt, und ein Banner oben informiert Sie, dass sich das Feature in der Vorschau befindet.

   ![Auswählen von VNET und Subnetz][7]

Damit Ihre App den DNS-Server, für den Ihr VNET konfiguriert ist, verwenden kann, erstellen Sie für Ihre App eine Anwendungseinstellung mit dem Namen WEBSITE_DNS_SERVER und der IP-Adresse des Servers als Wert.  Wenn Sie über einen sekundären DNS-Server verfügen, erstellen Sie eine andere Anwendungseinstellung mit dem Namen WEBSITE_DNS_ALT_SERVER und der IP-Adresse des Servers als Wert. 

Um Ihre App vom VNET zu trennen, wählen Sie **Verbindung trennen** aus. Hiermit wird Ihre Web-App neu gestartet. 

Mit dem neuen VNET-Integrationsfeature können Sie Dienstendpunkte verwenden.  Um Dienstendpunkte mit Ihrer App zu verwenden, stellen Sie über die neue VNET-Integration eine Verbindung mit einem ausgewählten VNET her, und konfigurieren Sie dann Dienstendpunkte in dem für die Integration verwendeten Subnetz. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-details.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-aspdetails.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-newvnet.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-newvnetdetails.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-selectvnet.png

<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#gatewaysubnet
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
