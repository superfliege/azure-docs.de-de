---
title: Überlegungen zum Netzwerkbetrieb in einer App Service-Umgebung – Azure
description: In diesem Artikel wird der ASE-Netzwerkverkehr erläutert. Weiterhin wird darauf eingegangen, wie Sie mit Ihrer ASE NSGs und UDRs festlegen können
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 73175b326c25d5d9a78155d0d9d888b655da1bfd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58124132"
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Überlegungen zum Netzwerkbetrieb in einer App Service-Umgebung #

## <a name="overview"></a>Übersicht ##

 Die Azure [App Service-Umgebung][Intro] ist eine Bereitstellung des Azure App Service in einem Subnetz in Ihrem Azure Virtual Network (VNet). Es gibt zwei Bereitstellungstypen für eine App Service-Umgebung (ASE):

- **Externe ASE**: Macht die gehosteten Apps der ASE für eine über das Internet erreichbare IP-Adresse verfügbar. Weitere Informationen finden Sie unter [Erstellen einer externen ASE][MakeExternalASE].
- **ILB ASE**: Macht die gehosteten Apps der ASE für eine IP-Adresse in Ihrem VNet verfügbar. Der interne Endpunkt ist ein ILB (Internal Load Balancer, interner Lastenausgleich), daher der Name ILB-ASE. Weitere Informationen finden Sie unter [Erstellen und Verwenden einer ILB-ASE][MakeILBASE].

Es gibt zwei Versionen der App Service-Umgebung: ASEv1 und ASEv2. Informationen zur ASEv1 finden Sie unter [Einführung in die App Service-Umgebung v1][ASEv1Intro]. ASEv1 kann in einem klassischen oder Resource Manager-VNet bereitgestellt werden. ASEv2 kann nur in einem Ressourcen-Manager-VNet bereitgestellt werden.

Alle Aufrufe, die von einer ASE ins Internet gehen, verlassen das VNet über eine der ASE zugewiesene VIP-Adresse. Die öffentliche IP dieser VIP ist die Quell-IP für alle Aufrufe, die von der ASE ins Internet gehen. Wenn die Apps in Ihrer ASE Ressourcen in Ihrem VNet oder über ein VPN aufrufen, ist die Quell-IP eine der IPs in dem von Ihrer ASE verwendeten Subnetz. Da sich die ASE im VNet befindet, hat sie ohne zusätzliche Konfiguration auch Zugriff auf Ressourcen im VNet. Wenn das VNet mit Ihrem lokalen Netzwerk verbunden ist, verfügen die Apps in der ASE ohne zusätzliche Konfiguration über Zugriff auf die dort enthaltenen Ressourcen.

![Externe ASE][1] 

Wenn Sie über eine externe ASE verfügen, ist die öffentliche VIP auch der Endpunkt, auf den die ASE-Apps aufgelöst werden für:

* HTTP/S. 
* FTP/S. 
* Webbereitstellung.
* Remotedebuggen.

![ILB ASE][2]

Wenn Sie über eine ILB-ASE verfügen, ist die Adresse der ILB der Endpunkt für HTTP/S, FTP/S, Webbereitstellung und Remotedebuggen.

Die normalen App-Zugriffsports sind:

| Zweck | From | To |
|----------|---------|-------------|
|  HTTP/HTTPS  | Vom Benutzer konfigurierbar |  80, 443 |
|  FTP/FTPS    | Vom Benutzer konfigurierbar |  21, 990, 10001-10020 |
|  Remotedebuggen in Visual Studio  |  Vom Benutzer konfigurierbar |  4020, 4022, 4024 |
|  Web Deploy-Dienst | Vom Benutzer konfigurierbar | 8172 |

Dies gilt für externe ASEs und ILB-ASEs. Wenn Sie eine externe ASE nutzen, erreichen Sie diese Ports über die öffentliche VIP. Wenn Sie eine ILB-ASE nutzen, erreichen Sie diese Ports über die ILB. Wenn Sie Port 443 sperren, hat dies Auswirkungen auf einige im Portal verfügbare Features. Weitere Informationen finden Sie unter [Portalabhängigkeiten](#portaldep).

## <a name="ase-subnet-size"></a>ASE-Subnetzgröße ##

Die Größe des Subnetzes, das zum Hosten einer ASE verwendet wird, kann nach der ASE-Bereitstellung nicht mehr geändert werden.  Die ASE verwendet eine Adresse für jede Infrastrukturrolle sowie für jede Instanz eines isolierten App Service-Plans.  Zusätzlich werden 5 Adressen für den Azure-Netzwerkbetrieb für jedes erstellte Subnetz verwendet.  Eine ASE ohne App Service-Pläne verwendet 12 Adressen, bevor Sie eine App erstellen.  Wenn es sich um eine ILB-ASE handelt, werden vor dem Erstellen einer App in dieser ASE 13 Adressen verwendet. Wenn Sie Ihre ASE horizontal hochskalieren, werden Infrastrukturrollen jedes Vielfache von 15 und 20 Ihrer App Service-Planinstanzen hinzugefügt.

   > [!NOTE]
   > Im Subnetz kann sich ausschließlich die ASE befinden. Achten Sie darauf, einen Adressbereich auszuwählen, der auf künftiges Wachstum ausgelegt ist. Diese Einstellung kann später nicht geändert werden. Es wird eine Größe von `/24` mit 256 Adressen empfohlen.

Wenn Sie zentral hoch- oder herunterskalieren, werden neue Rollen der entsprechenden Größe hinzugefügt und Ihre Workloads werden dann von der aktuellen Größe auf die Zielgröße migriert. Erst nach der Migration Ihrer Apps werden die ursprünglichen virtuellen Computer entfernt. Das bedeutet, wenn Sie über eine ASE mit 100 ASP-Instanzen verfügen, gäbe es einen Zeitraum, in dem Sie die doppelte Anzahl von virtuellen Computern benötigen.  Aus diesem Grund empfehlen wir die Verwendung von „/24“, um eventuelle Änderungen zu berücksichtigen.  

## <a name="ase-dependencies"></a>ASE-Abhängigkeiten ##

### <a name="ase-inbound-dependencies"></a>Eingehende ASE-Abhängigkeiten ###

Die eingehenden ASE-Zugriffsabhängigkeiten sind:

| Zweck | From | To |
|-----|------|----|
| Verwaltung | App Service-Verwaltungsadressen | ASE-Subnetz: 454, 455 |
|  Interne ASE-Kommunikation | ASE-Subnetz: Alle Ports | ASE-Subnetz: Alle Ports
|  Azure Load Balancer eingehend zulassen | Azure Load Balancer | ASE-Subnetz: Alle Ports
|  Von der App zugewiesene IP-Adressen | Von der App zugewiesene Adressen | ASE-Subnetz: Alle Ports

Zusätzlich zur Systemüberwachung ermöglicht der eingehende Verwaltungsdatenverkehr die Steuerung und Kontrolle der ASE. Die Quelladressen für diesen Datenverkehr sind im Dokument [App Service-Umgebung Management-Adressen][ASEManagement] aufgeführt. Die Netzwerksicherheitskonfiguration muss den Zugriff über alle IP-Adressen auf den Ports 454 und 455 zulassen. Wenn Sie den Zugriff von diesen Adressen blockieren, wird Ihr ASE fehlerhaft und dann entsprechend ausgesetzt.

Viele Ports im ASE-Subnetz werden für die Kommunikation zwischen internen Komponenten verwendet, und sie können sich ändern.  Deshalb muss auf alle Ports im ASE-Subnetz aus dem ASE-Subnetz zugegriffen werden können. 

Für die Kommunikation zwischen dem Azure Load Balancer und dem ASE-Subnetz müssen mindestens die Ports 454, 455 und 16001 geöffnet sein. Port 16001 wird für Keep-Alive-Datenverkehr zwischen dem Load Balancer und der ASE verwendet. Wenn Sie eine ILB-ASE verwenden, können Sie den Datenverkehr auf die Ports 454, 455 und 16001 beschränken.  Wenn Sie eine externe ASE verwenden, müssen Sie die normalen App-Zugriffsports berücksichtigen.  Wenn Sie von der App zugewiesene Adressen verwenden, müssen Sie sie für alle Ports öffnen.  Wenn eine Adresse einer bestimmten App zugewiesen ist, verwendet der Load Balancer zum Senden von HTTP- und HTTPS-Datenverkehr an die ASE Ports, die nicht im Voraus bekannt sind.

Wenn Sie von der App zugewiesene IP-Adressen verwenden, müssen Sie Datenverkehr von den IP-Adressen, die Ihren Apps zugewiesen sind, an das ASE-Subnetz zulassen.

Der TCP-Datenverkehr, der über die Ports 454 und 455 eintrifft, muss wieder vom gleichen VIP ausgehen, sonst haben Sie ein asymmetrisches Routingproblem. 

### <a name="ase-outbound-dependencies"></a>Ausgehende ASE-Abhängigkeiten ###

Eine ASE hängt beim ausgehenden Zugriff von mehreren externen Systemen ab. Viele dieser Systemabhängigkeiten werden über DNS-Namen festgelegt und lassen sich keiner festen IP-Adressengruppe zuordnen. Somit erfordert die ASE den ausgehenden Zugriff aus dem ASE-Subnetz auf alle externen IP-Adressen über eine Vielzahl von Ports. 

Die vollständige Liste der ausgehenden Abhängigkeiten ist in dem Dokument aufgeführt, das das [Sperren des ausgehenden Datenverkehrs der App Service-Umgebung](./firewall-integration.md) beschreibt. Wenn die ASE nicht mehr auf ihre Abhängigkeiten zugreifen kann, ist sie nicht mehr funktionsfähig. Nach einer gewissen Zeit wird die ASE angehalten. 

### <a name="customer-dns"></a>Kunden-DNS ###

Wenn das VNet mit einem kundendefinierten DNS-Server konfiguriert ist, wird es von den Workloads des Mandanten verwendet. Zu Verwaltungszwecken muss die ASE auch weiterhin mit dem Azure-DNS kommunizieren können. 

Wenn das VNet mit einem Kunden-DNS auf der anderen Seite eines VPN konfiguriert ist, muss der DNS-Server aus dem Subnetz erreichbar sein, das die ASE enthält.

Sie können die Auflösung Ihrer Web-App mit dem Konsolenbefehl *nameresolver* testen. Wechseln Sie zum Debugfenster in Ihrer SCM-Website für Ihre App oder zur App im Portal, und wählen Sie die Konsole aus. Über die Shell-Eingabeaufforderung können Sie den Befehl *nameresolver* zusammen mit der Adresse ausgeben, die Sie suchen möchten. Das Ergebnis, das Sie erhalten, ist identisch mit dem, das Ihre App bei gleicher Suche erhalten würde. Wenn Sie nslookup verwenden, werden Sie stattdessen eine Suche mit Azure DNS durchführen.

Wenn Sie die DNS-Einstellung des VNet ändern, in dem sich Ihre ASE befindet, müssen Sie Ihre ASE neu starten. Um einen Neustart Ihrer ASE zu vermeiden, wird dringend empfohlen, dass Sie Ihre DNS-Einstellungen für Ihr VNet konfigurieren, bevor Sie die ASE erstellen.  

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portalabhängigkeiten ##

Neben den funktionalen Abhängigkeiten der ASE gibt es einige weitere zu beachtende Aspekte im Zusammenhang mit der Portalverwendung. Einige der Funktionen im Azure-Portal hängen vom direkten Zugriff auf den _SCM-Standort_ ab. Zu jeder App in Azure App Service gibt es zwei URLs. Die erste URL dient dem Zugriff auf Ihre App. Die zweite URL dient dem Zugriff auf den SCM-Standort, der auch als _Kudu-Konsole_ bezeichnet wird. Funktionen, die den SCM-Standort nutzen:

-   Webaufträge
-   Functions
-   Protokollstreaming
-   Kudu
-   Erweiterungen
-   Prozess-Explorer
-   Konsole

Bei Verwendung einer ILB-ASE ist der SCM-Standort von außerhalb des VNet nicht aus dem Internet zugänglich. Wenn Ihre App auf einer ILB-ASE gehostet wird, funktionieren einige Funktionen vom Portal aus nicht.  

Viele dieser vom SCM-Standort abhängigen Funktionen sind auch direkt in der Kudu-Konsole verfügbar. Sie können sich direkt mit der Konsole verbinden, anstatt das Portal zu verwenden. Wenn Ihre App in einer ILB-ASE gehostet wird, melden Sie sich mit Ihren Veröffentlichungsanmeldeinformationen an. Die URL für den Zugriff auf den SCM-Standort einer in einer ILB-ASE gehosteten App besitzt das folgende Format: 

```
<appname>.scm.<domain name the ILB ASE was created with> 
```

Wenn Ihre ILB-ASE den Domänennamen *contoso.net* aufweist und der App-Name *testapp* lautet, wird die App unter *testapp.contoso.net* erreicht. Der entsprechende SCM-Standort ist unter *testapp.scm.contoso.net* zu erreichen.

## <a name="functions-and-web-jobs"></a>Funktionen und Webaufträge ##

Funktionen und Webaufträge hängen von der SCM-Website ab. Ihre Verwendung im Portal wird jedoch unterstützt, selbst wenn sich Ihre Apps in einer ILB-ASE befinden, solange Ihr Browser die SCM-Website erreichen kann.  Wenn Sie ein selbstsigniertes Zertifikat mit Ihrer ILB-ASE verwenden, müssen Sie Ihren Browser anweisen, diesem Zertifikat zu vertrauen.  Für Internet Explorer und Microsoft Edge bedeutet dies, dass sich das Zertifikat im Vertrauensspeicher des Computers befinden muss.  Wenn Sie Chrome verwenden, müssen Sie das Zertifikat vorab im Browser akzeptieren, indem Sie direkt zur SCM-Website browsen.  Die ideale Lösung stellt die Verwendung eines kommerziellen Zertifikats dar, das in der Vertrauenskette Ihres Browsers enthalten ist.  

## <a name="ase-ip-addresses"></a>ASE-IP-Adressen ##

Eine ASE muss einige IP-Adressen berücksichtigen. Sie lauten wie folgt:

- **Öffentliche IP-Adresse für eingehenden Datenverkehr**: Für den App-Datenverkehr in einer externen ASE und für den Verwaltungsdatenverkehr sowohl in einer externen ASE als auch in einer ILB-ASE.
- **Öffentliche IP-Adresse für ausgehenden Datenverkehr**: Als „Absender“ für von der ASE ausgehende und das VNet verlassende Verbindungen, die nicht durch ein VPN weitergeleitet werden.
- **ILB IP-Adresse**: Wenn Sie eine ILB-ASE verwenden.
- **Von der App zugewiesene IP-basierte SSL-Adressen**: Kann nur in einer externen ASE verwendet werden, wenn gleichzeitig auch IP-basiertes SSL konfiguriert ist.

Alle diese IP-Adressen sind von der ASE-Benutzeroberfläche aus auf einfache Weise in der ASEv2 im Azure-Portal sichtbar. Wenn Sie eine ILB-ASE verwenden, wird die IP der ILB angezeigt.

   > [!NOTE]
   > Diese IP-Adressen werden nicht geändert, solange Ihr ASE aktiv bleibt.  Wenn Ihre ASE ausgesetzt und wiederhergestellt wird, ändern sich die von Ihrer ASE verwendeten Adressen. Die normale Ursache für die Aussetzung einer ASE ist, wenn Sie den eingehenden Verwaltungszugriff oder den Zugriff auf eine ASE-Abhängigkeit blockieren. 

![IP-Adressen][3]

### <a name="app-assigned-ip-addresses"></a>Von der App zugewiesene IP-Adressen ###

Mit einer externen ASE können Sie einzelnen Apps IP-Adressen zuweisen. Dies ist mit einer ILB-ASE nicht möglich. Weitere Informationen zum Konfigurieren einer eigenen IP-Adresse für Ihre App finden Sie in [Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure App Service](../app-service-web-tutorial-custom-ssl.md).

Wenn eine App über eine eigene IP-basierte SSL-Adresse verfügt, reserviert die ASE zwei Ports für die Zuordnung zu dieser IP-Adresse. Ein Port wird für den HTTP-Datenverkehr verwendet, während der andere Port für den HTTPS-Datenverkehr bestimmt ist. Diese Ports werden im Bereich „IP-Adressen“ der ASE-Benutzeroberfläche angezeigt. Der Datenverkehr muss diese Ports von der VIP aus erreichen können, da sonst nicht auf die Apps zugegriffen werden kann. Diese Anforderung ist beim Konfigurieren von Netzwerksicherheitsgruppen (NSGs) unbedingt zu berücksichtigen.

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen ##

[Netzwerksicherheitsgruppen][NSGs] bieten die Möglichkeit, den Netzwerkzugriff innerhalb eines VNet zu steuern. Wenn Sie das Portal verwenden, gibt es auf der niedrigsten Prioritätsstufe eine implizite Ablehnungsregel, durch die alles abgelehnt wird. Sie erstellen also Ihre eigenen Zulassungsregeln.

In einer ASE haben Sie keinen Zugriff auf die VMs, die zum Hosten der eigentlichen ASE verwendet werden. Diese sind in einem Microsoft-Abonnement enthalten. Wenn Sie den Zugriff auf die Apps in der ASE beschränken möchten, legen Sie im ASE-Subnetz Netzwerksicherheitsgruppen fest. Achten Sie dabei sorgfältig auf die ASE-Abhängigkeiten. Wenn Sie Abhängigkeiten blockieren, ist die ASE nicht mehr funktionsfähig.

NSGs können über das Azure-Portal und über PowerShell konfiguriert werden. Die folgenden Informationen beziehen sich auf das Azure-Portal. NSGs können Sie unter **Netzwerk** im Portal als Ressource der obersten Ebene erstellen.

Wenn die Anforderungen in Bezug auf ein- und ausgehenden Datenverkehr berücksichtigt werden, sollten die Netzwerksicherheitsgruppen denen in diesem Beispiel ähneln. Der VNET-Adressbereich lautet _192.168.250.0/23_, und das Subnetz, in dem sich die ASE befindet, lautet _192.168.251.128/25_.

Die ersten beiden Anforderungen an den eingehenden Datenverkehr, ohne die die ASE nicht funktionsfähig ist, sind in diesem Beispiel in der Liste an oberster Stelle genannt. Sie ermöglichen die Verwaltung der ASE und die Kommunikation der ASE mit sich selbst. Die anderen Einträge sind alle vom Mandanten konfigurierbar und können den Netzwerkzugriff auf die in der ASE gehosteten Anwendungen steuern. 

![Eingangssicherheitsregeln][4]

Eine Standardregel ermöglicht den IPs im VNet die Kommunikation mit dem ASE-Subnetz. Eine weitere Standardregel ermöglicht dem Lastenausgleich (auch als öffentliche VIP-Adresse bezeichnet) die Kommunikation mit der ASE. Sie können die Standardregeln anzeigen lassen, indem Sie neben dem Symbol **Hinzufügen** auf **Standardregeln** klicken. Wenn Sie nach den angezeigten NSG-Regeln noch eine Ablehnungsregel für alles andere hinzufügen, unterbinden Sie den Datenverkehr zwischen der VIP und der ASE. Wenn Sie Datenverkehr aus dem VNet verhindern möchten, fügen Sie eine eigene Regel zum Zulassen von eingehendem Datenverkehr hinzu. Verwenden Sie eine auf AzureLoadBalancer festgelegte Quelle mit dem Ziel **Beliebig** und einem Portbereich von **\***. Da die NSG-Regel auf das ASE-Subnetz angewendet wird, müssen Sie kein spezifisches Ziel angeben.

Wenn Sie der App eine IP-Adresse zugewiesen haben, müssen Sie die Ports geöffnet halten. Sie können die Ports anzeigen lassen, indem Sie **App Service-Umgebung** > **IP-Adressen** auswählen.  

Alle Elemente in den folgenden Regeln für ausgehenden Datenverkehr sind mit Ausnahme des letzten Elements erforderlich. Durch sie ist der Netzwerkzugriff auf die weiter oben im vorliegenden Artikel beschriebenen ASE-Abhängigkeiten möglich. Wenn Sie diese sperren, funktioniert die ASE nicht mehr. Das letzte Listenelement ermöglicht der ASE die Kommunikation mit anderen Ressourcen in Ihrem VNet.

![Ausgangssicherheitsregeln][5]

Nach der Festlegung Ihrer NSGs müssen diese dem Subnetz zugewiesen werden, in dem sich Ihre ASE befindet. Wenn Sie das ASE-VNet oder -Subnetz nicht kennen, können Sie es sich auf der ASE-Portalseite anzeigen lassen. Um die NSG Ihrem Subnetz zuzuweisen, öffnen Sie die Benutzeroberfläche des Subnetzes, und wählen Sie die NSG aus.

## <a name="routes"></a>Routen ##

Bei der Tunnelerzwingung legen Sie Routen in Ihrem VNET so fest, dass der ausgehende Datenverkehr nicht direkt in das Internet, sondern beispielsweise an ein ExpressRoute-Gateway oder ein virtuelles Gerät geleitet wird.  Wenn Sie Ihre ASE auf diese Weise konfigurieren müssen, lesen Sie das Dokument unter [Konfigurieren Ihrer App Service-Umgebung mit erzwungenem Tunneling][forcedtunnel].  Dieses Dokument erläutert die verfügbaren Optionen zum Arbeiten mit ExpressRoute und Tunnelerzwingung.

Bei der Erstellung einer ASE im Portal erstellen wir auch einen Satz von Routentabellen im Subnetz, das mit der ASE erstellt wird.  Diese Routen weisen einfach an, ausgehenden Datenverkehr direkt an das Internet zu senden.  
Führen Sie diese Schritte aus, um die gleichen Routen manuell zu erstellen:

1. Öffnen Sie das Azure-Portal. Wählen Sie **Netzwerk** > **Routingtabellen** aus.

2. Erstellen Sie eine neue Routentabelle in derselben Region wie Ihr VNet.

3. Wählen Sie in der Benutzeroberfläche Ihrer Routentabelle **Routen** > **Hinzufügen**.

4. Legen Sie als **Typ des nächsten Hops** **Internet** und als **Adresspräfix** **0.0.0.0/0** fest. Wählen Sie **Speichern** aus.

    Folgendes sollte angezeigt werden:

    ![Funktionale Routen][6]

5. Wechseln Sie nach dem Erstellen der neuen Routentabelle in das Subnetz, in dem sich Ihre ASE befindet. Wählen Sie aus der Liste im Portal Ihre Routentabelle aus. Nachdem Sie die Änderung gespeichert haben, sollten die für Ihr Subnetz vermerkten NSGs und Routen angezeigt werden.

    ![NSGs und Routen][7]

## <a name="service-endpoints"></a>Dienstendpunkte ##

Dienstendpunkte ermöglichen Ihnen das Beschränken des Zugriffs auf mehrinstanzenfähige Dienste auf eine Gruppe von virtuellen Azure-Netzwerken und Subnetzen. Weitere Informationen zu Dienstendpunkten finden Sie in der Dokumentation [Dienstendpunkte im virtuellen Netzwerk][serviceendpoints]. 

Wenn Sie die Dienstendpunkte auf einer Ressource aktivieren, werden Routen erstellt, die eine höhere Priorität als alle anderen Routen haben. Bei Verwendung von Dienstendpunkten mit einer ASE mit Tunnelerzwingung wird für den Azure SQL- und Azure Storage-Verwaltungsdatenverkehr kein Tunneling erzwungen. 

Wenn Dienstendpunkte in einem Subnetz mit einer Azure SQL-Instanz aktiviert werden, müssen für alle Azure SQL-Instanzen, mit denen aus diesem Subnetz Verbindungen hergestellt werden, Dienstendpunkte aktiviert sein. Falls Sie aus demselben Subnetz auf mehrere Azure SQL-Instanzen zugreifen möchten, ist es nicht möglich, dass Sie Dienstendpunkte nur auf einer Azure SQL-Instanz aktivieren, aber nicht auf einer anderen Instanz. Azure Storage weist ein anderes Verhalten als Azure SQL auf. Wenn Sie Dienstendpunkte mit Azure Storage aktivieren, sperren Sie den Zugriff auf diese Ressource aus Ihrem Subnetz. Der Zugriff auf andere Azure Storage-Konten ist aber auch dann möglich,wenn dafür keine Dienstendpunkte aktiviert wurden.  

![Dienstendpunkte][8]

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png
[8]: ./media/network_considerations_with_an_app_service_environment/serviceendpoint.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
[forcedtunnel]: ./forced-tunnel-support.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
