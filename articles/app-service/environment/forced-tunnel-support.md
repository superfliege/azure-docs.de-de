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
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 4caaf0df3f1dd4b2cb9b76283a6beed897531c1c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurieren Ihrer App Service-Umgebung mit erzwungenem Tunneling

Die App Service-Umgebung ist eine Bereitstellung des Azure App Service in einer Instanz von Azure Virtual Network eines Kunden. Viele Kunden konfigurieren ihre virtuellen Netzwerke als Erweiterungen ihrer lokalen Netzwerke mit VPNs oder Azure ExpressRoute-Verbindungen. Aufgrund von Unternehmensrichtlinien oder anderen Sicherheitseinschränkungen konfigurieren sie Routen, um den gesamten ausgehenden Datenverkehr lokal zu senden, bevor er ins Internet übertragen werden kann. Wenn das Routing des virtuellen Netzwerks so geändert wird, dass der ausgehende Datenverkehr aus dem virtuellen Netzwerk über die VPN- oder ExpressRoute-Verbindung an lokale Standorte geleitet wird, wird dies als erzwungenes Tunneling bezeichnet. 

Das erzwungene Tunneling kann für eine App Service-Umgebung zu Problemen führen. Die App Service-Umgebung weist eine Reihe von externen Abhängigkeiten auf, die im Dokument zur [Netzwerkarchitektur für App Service-Umgebungen][network] aufgeführt sind. Die App Service-Umgebung erfordert standardmäßig, dass die gesamte ausgehende Kommunikation über die VIP erfolgt, die mit der App Service-Umgebung bereitgestellt wird.

Routen sind ein wichtiger Aspekt des erzwungenen Tunnelings und des entsprechenden Umgangs damit. In einem virtuellen Azure-Netzwerk wird das Routing auf der Basis der längsten Präfixübereinstimmung (Longest Prefix Match, LPM) durchgeführt. Wenn mehrere Routen mit identischer längster Präfixübereinstimmung vorhanden sind, wird die Route in der folgenden Reihenfolge beruhend auf ihrem Ursprung ausgewählt:

* Benutzerdefinierte Route
* BGP-Route (bei Verwendung von ExpressRoute)
* Systemroute

Weitere Informationen zum Routing in einem virtuellen Netzwerk finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung][routes]. 

Wenn Ihre App Service-Umgebung in einem virtuellen Netzwerk mit erzwungenem Tunnel ausgeführt werden soll, haben Sie zwei Optionen:

* Aktivieren Sie Ihre App Service-Umgebung, sodass sie direkten Zugang zum Internet hat.
* Ändern Sie den Endpunkt für ausgehenden Datenverkehr Ihrer App Service-Umgebung.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Aktivieren Ihrer App Service-Umgebung, sodass sie direkten Zugang zum Internet hat

Wenn Ihre App Service-Umgebung funktionieren soll, während das virtuelle Netzwerk mit einer ExpressRoute-Verbindung konfiguriert ist, haben Sie folgende Möglichkeiten:

* Konfigurieren Sie ExpressRoute so, dass „0.0.0.0/0“ angekündigt wird. In der Standardeinstellung wird das Tunneling des gesamten ausgehenden Datenverkehrs lokal erzwungen.
* Erstellen Sie eine UDR. Wenden Sie diese in dem Subnetz an, in dem die App Service-Umgebung liegt, und verwenden Sie hierbei das Adresspräfix 0.0.0.0/0 sowie „Internet“ als nächsten Hoptyp.

Wenn Sie diese beiden Änderungen vornehmen, wird der aus dem Subnetz des App Service stammende Datenverkehr ins Internet nicht zwingend über die ExpressRoute-Verbindung geleitet, und die App Service-Umgebung ist funktionsfähig.

> [!IMPORTANT]
> Die in einer UDR definierten Routen müssen ausreichend spezifisch sein, damit sie Vorrang vor allen von der ExpressRoute-Konfiguration angekündigten Routen erhalten. Im vorhergehenden Beispiel wird der allgemeine Adressbereich „0.0.0.0/0“ verwendet. Er kann durch Routenankündigungen mit spezifischeren Adressbereichen versehentlich überschrieben werden kann.
>
> App Service-Umgebungen werden nicht mit ExpressRoute-Konfigurationen unterstützt, die Routen „über Kreuz“ vom öffentlichen Peeringpfad zum privaten Peeringpfad ankündigen. ExpressRoute-Konfigurationen, für die öffentliches Peering konfiguriert ist, erhalten Routenankündigungen von Microsoft. Die Ankündigungen enthalten zahlreiche Microsoft Azure-IP-Adressbereiche. Werden diese Adressbereiche über Kreuz auf dem privaten Peeringpfad angekündigt, werden alle ausgehenden Netzwerkpakete aus dem Subnetz der App Service-Umgebung zwangsweise zur lokalen Netzwerkinfrastruktur eines Kunden getunnelt. Dieser Netzwerkdatenfluss wird derzeit standardmäßig nicht für App Service-Umgebungen unterstützt. Eine Lösung für dieses Problem besteht darin, „Über-Kreuz-Ankündigungen“ von Routen vom öffentlichen Peeringpfad zum privaten Peeringpfad zu verhindern. Eine andere Lösung besteht darin, Ihre App Service-Umgebung in die Lage zu versetzen, in einer Konfiguration mit erzwungenem Tunneling zu arbeiten.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Ändern des Endpunkts für ausgehenden Datenverkehr Ihrer App Service-Umgebung ##

In diesem Abschnitt wird beschrieben, wie eine App Service-Umgebung in einer Konfiguration mit erzwungenem Tunneling betrieben werden kann, indem der von der App Service-Umgebung verwendete Ausgangsendpunkt geändert wird. Wenn der ausgehende Datenverkehr der App Service-Umgebung über erzwungenes Tunneling zu einem lokalen Netzwerk geleitet wird, müssen Sie zulassen, dass dieser Datenverkehr von anderen IP-Adressen als der VIP-Adresse der App Service-Umgebung stammt.

Eine App Service-Umgebung weist nicht nur externe Abhängigkeiten auf, sondern muss auch auf eingehenden Datenverkehr lauschen und entsprechend reagieren. Die Antworten können nicht von einer anderen Adresse zurückgesendet werden, da TCP damit nicht umgehen kann. Es sind drei Schritte erforderlich, um den ausgehenden Endpunkt für die App Service-Umgebung zu ändern:

1. Legen Sie ein Routing fest, um sicherzustellen, dass eingehender Datenverkehr über dieselbe IP-Adresse zurückgesendet werden kann.

2. Fügen Sie Ihre IP-Adressen, die für ausgehenden Datenverkehr verwendet werden, der Firewall für die App Service-Umgebung hinzu.

3. Legen Sie für die Routen für ausgehenden Datenverkehr von der App Service-Umgebung Tunneling fest.

   ![Netzwerkdatenstrom für erzwungenes Tunneling][1]

Sie können die App Service-Umgebung mit anderen Adressen für ausgehenden Datenverkehr konfigurieren, nachdem die App Service-Umgebung bereits betriebsbereit ist, oder sie können während der Bereitstellung der App Service-Umgebung festgelegt werden.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Ändern der Adresse für ausgehenden Datenverkehr, nachdem die App Service-Umgebung betriebsbereit ist ###
1. Rufen Sie die IP-Adressen ab, die Sie als ausgehende IP-Adressen für Ihre App Service-Umgebung verwenden möchten. Wenn Sie erzwungenes Tunneling verwenden, stammen diese IP-Adressen von Ihren NATs oder Gateways. Wenn Sie den ausgehenden Datenverkehr der App Service-Umgebung über eine NVA weiterleiten möchten, ist die ausgehende Adresse die öffentliche IP-Adresse der NVA.

2. Legen Sie die ausgehenden Adressen in den Konfigurationsinformationen Ihrer App Service-Umgebung fest. Wechseln Sie zu „resource.azure.com“, und navigieren Sie zu: „Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>“. Dort sehen Sie die JSON-Datei, die Ihre App Service-Umgebung beschreibt. Vergewissern Sie sich, dass am Anfang **read/write** angezeigt wird. Wählen Sie **Bearbeiten** aus. Scrollen Sie nach unten, und ändern Sie den Wert **userWhitelistedIpRanges** von **null** in einen Wert ähnlich den folgenden. Verwenden Sie die Adressen, die Sie als Ausgangsadressbereich festlegen möchten. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Wählen Sie oben **PUT** aus. Diese Option löst einen Skalierungsvorgang für Ihre App Service-Umgebung aus und passt die Firewall an.
 
3. Erstellen oder bearbeiten Sie ein Routing, und füllen Sie die Regeln aus, um den Zugriff auf und von Verwaltungsadressen zuzulassen, die dem Standort Ihrer App Service-Umgebung zugeordnet sind. Die Verwaltungsadressen finden Sie unter [App Service-Umgebung – Verwaltungsadressen][management].

4. Passen Sie die Routen, die auf das Subnetz Ihrer App Service-Umgebung angewendet werden, mit einem Routing oder mit BGP-Routen an. 

Wenn die App Service-Umgebung vom Portal aus nicht reagiert, liegt ein Problem mit Ihren Änderungen vor. Es kann sein, dass Ihre Liste der Ausgangsadressen unvollständig war oder der Datenverkehr verloren ging oder blockiert wurde. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Erstellen einer neuen App Service-Umgebung mit einer anderen Adresse für ausgehenden Datenverkehr ###

Wenn Ihr virtuelles Netzwerk bereits so konfiguriert ist, dass erzwungenes Tunneling auf den gesamten Datenverkehr angewendet wird, müssen Sie einige zusätzliche Schritte durchführen, um Ihre App Service-Umgebung derart zu erstellen, dass sie erfolgreich verwendet werden kann. Sie müssen bei der Erstellung der App Service-Umgebung die Verwendung eines anderen ausgehenden Endpunkts aktivieren. Dazu müssen Sie die App Service-Umgebung mit einer Vorlage erstellen, die die zulässigen ausgehenden Adressen angibt.

1. Ermitteln Sie die IP-Adressen, die als ausgehende Adressen für Ihre App Service-Umgebung verwendet werden sollen.

2. Erstellen Sie vorab das Subnetz, das von der App Service-Umgebung verwendet werden soll. Es ist erforderlich, damit Routen festgelegt werden können, und wird zudem von der Vorlage benötigt.

3. Erstellen Sie ein Routing mit den Verwaltungs-IP-Adressen, die Ihrer App Service-Umgebung zugeordnet sind. Weisen Sie sie Ihrer App Service-Umgebung zu.

4. Befolgen Sie die Anweisungen unter [Erstellen einer App Service-Umgebung mit einer Vorlage][template]. Rufen Sie die entsprechende Vorlage ab.

5. Bearbeiten Sie den Abschnitt „resources“ in der Datei „azuredeploy.json“. Fügen Sie eine Zeile für **userWhitelistedIpRanges** mit Ihren Werten wie folgt hinzu:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Wenn dieser Abschnitt ordnungsgemäß konfiguriert ist, sollte die App Service-Umgebung problemlos gestartet werden. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
