---
title: "Konfigurieren Ihrer Azure App Service-Umgebung für erzwungenes Tunneling"
description: "Ermöglichen der Funktion Ihrer App Service-Umgebung, wenn das Tunneling für ausgehenden Datenverkehr erzwungen ist"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: ccompy
ms.openlocfilehash: cd89bd23074dec1de6fa0e8784d42a5c539938b1
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurieren Ihrer App Service-Umgebung mit erzwungenem Tunneling

Die App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service im Azure Virtual Network (VNet) des Kunden. Viele Kunden konfigurieren ihre VNets als Erweiterungen ihrer lokalen Netzwerke mit VPNs oder ExpressRoute-Verbindungen. Aufgrund von Unternehmensrichtlinien oder anderen Sicherheitseinschränkungen konfigurieren sie Routen, um den gesamten ausgehenden Datenverkehr lokal zu senden, bevor er ins Internet übertragen werden kann. Wenn das Routing des VNet so geändert wird, dass der ausgehende Datenverkehr aus dem VNet über die VPN- oder ExpressRoute-Verbindung zu lokalen Standorten geleitet wird, bezeichnet man dies als erzwungenes Tunneling.  

Das erzwungene Tunneling kann für eine ASE zu Problemen führen. Die ASE weist eine Reihe von externen Abhängigkeiten auf, die in diesem Dokument zur [ASE-Netzwerkarchitektur][network] aufgeführt sind. Die ASE erfordert standardmäßig, dass die gesamte ausgehende Kommunikation über die VIP erfolgt, die mit der ASE bereitgestellt wird.

Routen sind ein wichtiger Aspekt hinsichtlich der Bedeutung des erzwungenen Tunnelings und dem entsprechenden Umgang damit. In einem Azure Virtual Network wird das Routing auf der Basis der längsten Präfixübereinstimmung (Longest Prefix Match, LPM) durchgeführt.  Wenn mehrere Routen mit identischer längster Präfixübereinstimmung vorhanden sind, wird die Route basierend auf ihrem Ursprung in der folgenden Reihenfolge ausgewählt:

1. Benutzerdefinierte Route
1. BGP-Route (bei Verwendung von ExpressRoute)
1. Systemroute

Weitere Informationen zum Routing in einem VNet finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung][routes]. 

Wenn Sie Ihre ASE in einem VNet mit erzwungenem Tunneling ausführen möchten, haben Sie zwei Möglichkeiten:

1. Ermöglichen Sie Ihrer ASE den direkten Zugang zum Internet.
1. Ändern Sie den Ausgangsendpunkt für Ihre App Service-Umgebung

## <a name="enable-your-ase-to-have-direct-internet-access"></a>Ermöglichen Sie Ihrer ASE den direkten Zugang zum Internet.

Sie haben folgenden Möglichkeiten, damit Ihre ASE funktioniert, wenn Ihr VNet mit einer ExpressRoute konfiguriert ist:

* Konfigurieren Sie ExpressRoute so, dass „0.0.0.0/0“ angekündigt wird. In der Standardeinstellung wird das Tunneling des gesamten ausgehenden Datenverkehrs lokal erzwungen.
* Erstellen Sie eine UDR. Wenden Sie diese in dem Subnetz an, das die ASE mit dem Adresspräfix „0.0.0.0/0“ dem nächsten Hoptyp „Internet“ enthält.

Wenn Sie diese beiden Änderungen vornehmen, wird der aus dem ASE-Subnetz stammende Datenverkehr ins Internet nicht zwingend über ExpressRoute geleitet, und die ASE ist funktionsfähig.

> [!IMPORTANT]
> Die in einer UDR definierten Routen müssen ausreichend spezifisch sein, damit sie Vorrang vor allen von der ExpressRoute-Konfiguration angekündigten Routen erhalten. Im vorhergehenden Beispiel wird der allgemeine Adressbereich „0.0.0.0/0“ verwendet. Er kann durch Routenankündigungen mit spezifischeren Adressbereichen versehentlich überschrieben werden kann.
>
> App Service-Umgebungen werden nicht mit ExpressRoute-Konfigurationen unterstützt, die Routen „über Kreuz“ vom öffentlichen Peeringpfad zum privaten Peeringpfad ankündigen. ExpressRoute-Konfigurationen, für die öffentliches Peering konfiguriert ist, erhalten Routenankündigungen von Microsoft. Die Ankündigungen enthalten zahlreiche Microsoft Azure-IP-Adressbereiche. Werden diese Adressbereiche über Kreuz auf dem privaten Peeringpfad angekündigt, werden alle ausgehenden Netzwerkpakete aus dem Subnetz der ASE zwangsweise zur lokalen Netzwerkinfrastruktur eines Kunden getunnelt. Dieser Netzwerkdatenfluss wird derzeit für App Service-Umgebungen nicht standardmäßig unterstützt. Eine Lösung für dieses Problem besteht darin, „Über-Kreuz-Ankündigungen“ von Routen vom öffentlichen Peeringpfad zum privaten Peeringpfad zu verhindern.  Die andere Lösung besteht darin, Ihre ASE in die Lage zu versetzen, in einer Konfiguration mit erzwungenem Tunneling zu arbeiten.

## <a name="change-the-egress-endpoint-for-your-ase"></a>Ändern Sie den Ausgangsendpunkt für Ihre App Service-Umgebung ##

In diesem Abschnitt wird beschrieben, wie eine ASE in einer Konfiguration mit erzwungenem Tunneling betrieben werden kann, indem der von der ASE verwendete Ausgangsendpunkt geändert wird. Wenn der ausgehende Datenverkehr der ASE über erzwungenes Tunneling zu einem lokalen Netzwerk geleitet wird, müssen Sie zulassen, dass dieser Datenverkehr von anderen IP-Adressen als der VIP-Adresse der ASE stammt.

Eine ASE weist nicht nur externe Abhängigkeiten auf, sondern muss auch auf eingehenden Datenverkehr zum Verwalten der ASE achten. Die ASE muss auf derartigen Datenverkehr reagieren können, und die Antworten können nicht von einer anderen Adresse zurückgesendet werden, da dies zu TCP-Problemen führt.  Daher sind drei Schritte erforderlich, um den Ausgangsendpunkt für die ASE zu ändern.

1. Legen Sie eine Routentabelle fest, um sicherzustellen, dass eingehender Datenverkehr über dieselbe IP-Adresse zurückgesendet werden kann.
1. Fügen Sie Ihre IP-Adressen, die für ausgehenden Datenverkehr verwendet werden, zur ASE-Firewall hinzu.
1. Legen Sie für die Routen für den ausgehenden Datenverkehr von der ASE das Tunneling fest.

![Netzwerkdatenstrom für erzwungenes Tunneling][1]

Sie können die ASE mit verschiedenen Ausgangsadressen konfigurieren, nachdem die ASE bereits einsatzbereit ist, oder sie können während der ASE-Bereitstellung festgelegt werden.  

### <a name="changing-the-egress-address-after-the-ase-is-operational"></a>Ändern der Ausgangsadresse, nachdem die ASE einsatzbereit ist ###
1. Rufen Sie die IP-Adressen ab, die Sie als ausgehende IP-Adressen für Ihre ASE verwenden möchten. Wenn Sie das erzwungene Tunneling anwenden, wären dies die IP-Adressen für Ihre NAT oder für Ihr Gateway.  Wenn Sie den ausgehenden Datenverkehr der ASE über eine NVA weiterleiten möchten, würde die Ausgangsadresse die öffentliche IP-Adresse der NVA darstellen.
2. Legen Sie die Ausgangsadressen in den ASE-Konfigurationsinformationen fest. Wechseln Sie zu „resource.azure.com“ und navigieren Sie zu: Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Dort können Sie die JSON-Datei anzeigen, die Ihre ASE beschreibt.  Stellen Sie sicher, dass am Anfang „read/write“ angezeigt wird.  Klicken Sie auf „Bearbeiten“. Scrollen Sie zum unteren Rand und ändern Sie „UserWhitelistedIpRanges“ von  

       "userWhitelistedIpRanges": null 
      
  in etwas, das dem Folgenden ähnelt. Verwenden Sie die Adressen, die Sie als Ausgangsadressbereich festlegen möchten. 

      "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

  Klicken Sie oben auf „PUT“. Dadurch wird ein Skalierungsvorgang für die ASE ausgelöst und die Firewall angepasst.
   
3. Erstellen oder bearbeiten Sie eine Routentabelle, und füllen Sie die Regeln aus, um den Zugriff auf/von Verwaltungsadressen zu gestatten, die Ihrem ASE-Standort zugeordnet sind.  Die Verwaltungsadressen befinden sich hier: [App Service-Umgebung – Verwaltungsadressen][management]. 

4. Passen Sie die Routen, die auf das ASE-Subnetz angewendet werden, mit einer Routentabelle oder mit BGP-Routen an.  

Wenn die ASE vom Portal aus nicht reagiert, dann gibt es ein Problem mit Ihren Änderungen.  Es kann sein, dass Ihre Liste der Ausgangsadressen unvollständig war, der Datenverkehr verloren ging oder blockiert wurde.  

### <a name="create-a-new-ase-with-a-different-egress-address"></a>Erstellen einer neuen ASE mit einer anderen Ausgangsadresse  ###

Für den Fall, dass Ihr VNet bereits so konfiguriert ist, dass das erzwungene Tunneling auf den gesamten Datenverkehr angewendet wird, müssen Sie einige zusätzliche Schritte durchführen, um Ihre ASE derart zu erstellen, dass sie erfolgreich ausgeführt werden kann. Das bedeutet, dass Sie während der ASE-Erstellung die Verwendung eines anderen Ausgangsendpunkts aktivieren müssen.  Dazu müssen Sie die ASE mit einer Vorlage erstellen, die die zulässigen Ausgangsadressen angibt.

1. Ermitteln Sie die IP-Adressen, die als Ausgangsadressen für Ihre ASE verwendet werden sollen.
1. Erstellen Sie vorab das Subnetz, das von der ASE verwendet werden soll. Dies ist notwendig, um Routen festlegen zu können und es ist zudem für die Vorlage erforderlich.  
1. Erstellen einer Routentabelle mit den Verwaltungs-IP-Adressen, die Ihrem ASE-Standort zugeordnet sind und Zuweisen dieser Adressen zu Ihrer ASE
1. Befolgen Sie die Anweisungen unter [Erstellen einer ASE mit einer Vorlage][template] und ziehen Sie die entsprechende Vorlage herunter.
1. Bearbeiten Sie den Abschnitt „resources“ der Date „azuredeploy.json“. Fügen Sie eine Zeile für **userWhitelistedIpRanges** mit Ihren Werten hinzu. Beispiel:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Wenn dies ordnungsgemäß konfiguriert ist, sollte die ASE problemlos starten.  


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
