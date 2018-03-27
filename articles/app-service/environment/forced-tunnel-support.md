---
title: Konfigurieren Ihrer Azure App Service-Umgebung für die Tunnelerzwingung
description: Ermöglichen der Funktion Ihrer App Service-Umgebung, wenn das Tunneling für ausgehenden Datenverkehr erzwungen wird
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
ms.date: 3/6/2018
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 92073cd29f29c1ddf5863e23c4a12dfdf8e21598
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurieren Ihrer App Service-Umgebung mit erzwungenem Tunneling

Die App Service-Umgebung (ASE) ist eine Bereitstellung von Azure App Service im Azure Virtual Network des Kunden. Viele Kunden konfigurieren ihre virtuellen Azure-Netzwerke als Erweiterungen ihrer lokalen Netzwerke mit VPNs oder Azure ExpressRoute-Verbindungen. Bei der Tunnelerzwingung leiten Sie für das Internet bestimmten Datenverkehr stattdessen an Ihr VPN oder ein virtuelles Gerät um. Dies erfolgt häufig im Rahmen von Sicherheitsanforderungen, bei denen der gesamte ausgehende Datenverkehr überprüft und überwacht wird. 

Die ASE weist eine Reihe von externen Abhängigkeiten auf, die im Dokument zur [Netzwerkarchitektur für App Service-Umgebungen][network] beschrieben werden. Normalerweise muss der gesamte ausgehende Abhängigkeitsdatenverkehr der ASE über die VIP verlaufen, die für die ASE bereitgestellt wurde. Wenn Sie das Routing für den Datenverkehr zur bzw. aus der ASE ändern, ohne die unten angegebenen Informationen zu beachten, funktioniert Ihre ASE nicht mehr.

In einem virtuellen Azure-Netzwerk wird das Routing auf der Basis der längsten Präfixübereinstimmung (Longest Prefix Match, LPM) durchgeführt. Wenn mehrere Routen mit identischer längster Präfixübereinstimmung vorhanden sind, wird die Route in der folgenden Reihenfolge beruhend auf ihrem Ursprung ausgewählt:

* Benutzerdefinierte Route
* BGP-Route (bei Verwendung von ExpressRoute)
* Systemroute

Weitere Informationen zum Routing in einem virtuellen Netzwerk finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung][routes]. 

Wenn Sie Ihren ausgehenden Datenverkehr der ASE nicht direkt ins Internet, sondern an einen anderen Ort leiten möchten, haben Sie folgende Optionen:

* Ermöglichen Sie Ihrer ASE den direkten Zugang zum Internet.
* Konfigurieren des ASE-Subnetzes für die Verwendung von Dienstendpunkten für Azure SQL und Azure Storage
* Hinzufügen Ihrer eigenen IPs zur Azure SQL-Firewall der ASE

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Aktivieren Ihrer App Service-Umgebung, sodass sie direkten Zugang zum Internet hat

Um für Ihre ASE den direkten Weg ins Internet auch dann zu ermöglichen, wenn das virtuelle Azure-Netzwerk mit ExpressRoute konfiguriert wurde, können Sie wie folgt vorgehen:

* Konfigurieren Sie ExpressRoute so, dass „0.0.0.0/0“ angekündigt wird. In der Standardeinstellung wird der gesamte ausgehende Datenverkehr an lokale Speicherorte geleitet.
* Erstellen Sie eine UDR mit dem Adresspräfix 0.0.0.0/0 und dem Typ „Internet“ für den nächsten Hop, und wenden Sie sie auf das ASE-Subnetz an.

Wenn Sie diese beiden Änderungen vornehmen, wird der aus dem Subnetz des App Service stammende Datenverkehr ins Internet nicht zwingend über die ExpressRoute-Verbindung geleitet.

> [!IMPORTANT]
> Die in einer UDR definierten Routen müssen ausreichend spezifisch sein, damit sie Vorrang vor allen von der ExpressRoute-Konfiguration angekündigten Routen erhalten. Im vorhergehenden Beispiel wird der allgemeine Adressbereich „0.0.0.0/0“ verwendet. Er kann durch Routenankündigungen mit spezifischeren Adressbereichen versehentlich überschrieben werden kann.
>
> App Service-Umgebungen werden nicht mit ExpressRoute-Konfigurationen unterstützt, die Routen „über Kreuz“ vom öffentlichen Peeringpfad zum privaten Peeringpfad ankündigen. ExpressRoute-Konfigurationen, für die öffentliches Peering konfiguriert ist, erhalten Routenankündigungen von Microsoft. Die Ankündigungen enthalten zahlreiche Microsoft Azure-Adressbereiche. Wenn diese Adressbereiche über Kreuz auf dem privaten Peeringpfad angekündigt werden, werden alle ausgehenden Netzwerkpakete aus dem Subnetz der App Service-Umgebung in die lokale Netzwerkinfrastruktur eines Kunden geleitet. Dieser Netzwerkdatenfluss wird standardmäßig nicht für App Service-Umgebungen unterstützt. Eine Lösung für dieses Problem besteht darin, „Über-Kreuz-Ankündigungen“ von Routen vom öffentlichen Peeringpfad zum privaten Peeringpfad zu verhindern. Eine andere Lösung besteht darin, Ihre App Service-Umgebung in die Lage zu versetzen, in einer Konfiguration mit erzwungenem Tunneling zu arbeiten.

![Direkter Internetzugriff][1]

## <a name="configure-your-ase-with-service-endpoints"></a>Konfigurieren Ihrer ASE mit Dienstendpunkten

Führen Sie die folgenden Schritte aus, um das Routing für den gesamten ausgehenden Datenverkehr Ihrer ASE einzurichten, mit Ausnahme des Datenverkehrs für Azure SQL und Azure Storage:

1. Erstellen Sie eine Routingtabelle, und weisen Sie sie Ihrem ASE-Subnetz zu. Die Adressen für Ihre Region finden Sie unter [App Service-Umgebung Management-Adressen][management]. Erstellen Sie Routen für diese Adressen mit „Internet“ als nächstem Hop. Dies ist erforderlich, weil für eingehenden Verwaltungsdatenverkehr der App Service-Umgebung von derselben Adresse geantwortet werden muss, an die er gesendet wurde.   

2. Aktivieren von Dienstendpunkten mit Azure SQL und Azure Storage über Ihr ASE-Subnetz

Dienstendpunkte ermöglichen Ihnen das Beschränken des Zugriffs auf mehrinstanzenfähige Dienste auf eine Gruppe von virtuellen Azure-Netzwerken und Subnetzen. Weitere Informationen zu Dienstendpunkten finden Sie in der Dokumentation [Dienstendpunkte im virtuellen Netzwerk][serviceendpoints]. 

Wenn Sie die Dienstendpunkte auf einer Ressource aktivieren, werden Routen erstellt, die eine höhere Priorität als alle anderen Routen haben. Bei Verwendung von Dienstendpunkten mit einer ASE mit Tunnelerzwingung wird für den Azure SQL- und Azure Storage-Verwaltungsdatenverkehr kein Tunneling erzwungen. Für den restlichen ASE-Abhängigkeitsdatenverkehr wird das Tunneling erzwungen, und er darf nicht verloren gehen, da die ASE ansonsten nicht richtig funktioniert.

Wenn Dienstendpunkte in einem Subnetz mit einer Azure SQL-Instanz aktiviert werden, müssen für alle Azure SQL-Instanzen, mit denen aus diesem Subnetz Verbindungen hergestellt werden, Dienstendpunkte aktiviert sein. Falls Sie aus demselben Subnetz auf mehrere Azure SQL-Instanzen zugreifen möchten, ist es nicht möglich, dass Sie Dienstendpunkte nur auf einer Azure SQL-Instanz aktivieren, aber nicht auf einer anderen Instanz.  Azure Storage weist ein anderes Verhalten als Azure SQL auf.  Wenn Sie Dienstendpunkte mit Azure Storage aktivieren, sperren Sie den Zugriff auf diese Ressource aus Ihrem Subnetz. Der Zugriff auf andere Azure Storage-Konten ist aber auch dann möglich,wenn dafür keine Dienstendpunkte aktiviert wurden.  

Wenn Sie die Tunnelerzwingung mit einer Netzwerkfilter-Appliance konfigurieren, sollten Sie bedenken, dass die ASE zusätzlich zu Azure SQL und Azure Storage noch über einige andere Abhängigkeiten verfügt. Sie müssen diesen Datenverkehr zulassen, da die ASE ansonsten nicht richtig funktioniert.

![Tunnelerzwingung mit Dienstendpunkten][2]

## <a name="add-your-own-ips-to-the-ase-azure-sql-firewall"></a>Hinzufügen Ihrer eigenen IPs zur Azure SQL-Firewall der ASE ##

Führen Sie die folgenden Schritte aus, um das Tunneling für den gesamten ausgehenden Datenverkehr Ihrer ASE einzurichten, mit Ausnahme des Datenverkehrs für Azure Storage:

1. Erstellen Sie eine Routingtabelle, und weisen Sie sie Ihrem ASE-Subnetz zu. Die Adressen für Ihre Region finden Sie unter [App Service-Umgebung Management-Adressen][management]. Erstellen Sie Routen für diese Adressen mit „Internet“ als nächstem Hop. Dies ist erforderlich, weil für eingehenden Verwaltungsdatenverkehr der App Service-Umgebung von derselben Adresse geantwortet werden muss, an die er gesendet wurde. 

2. Aktivieren von Dienstendpunkten mit Azure Storage über Ihr ASE-Subnetz

3. Beschaffen Sie sich die Adressen, die für den gesamten ausgehenden Datenverkehr von Ihrer App Service-Umgebung ins Internet verwendet werden. Falls Sie den Datenverkehr an lokale Speicherorte weiterleiten, sind diese Adressen Ihre NATs oder Gateway-IPs. Wenn Sie den ausgehenden Datenverkehr der App Service-Umgebung über eine NVA weiterleiten möchten, ist die ausgehende Adresse die öffentliche IP-Adresse der NVA.

4. _Gehen Sie wie folgt vor, um die Ausgangsadressen in einer vorhandenen App Service-Umgebung festzulegen:_ Navigieren Sie zu „resource.azure.com“ und dann zu „Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>“. Dort sehen Sie die JSON-Datei, die Ihre App Service-Umgebung beschreibt. Vergewissern Sie sich, dass am Anfang **read/write** angezeigt wird. Wählen Sie **Bearbeiten** aus. Scrollen Sie ganz nach unten. Ändern Sie den Wert **userWhitelistedIpRanges** von **null** in einen Wert, der dem unten angegebenen Wert ähnelt. Verwenden Sie die Adressen, die Sie als Ausgangsadressbereich festlegen möchten. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Wählen Sie oben **PUT** aus. Diese Option löst einen Skalierungsvorgang für Ihre App Service-Umgebung aus und passt die Firewall an.

_Gehen Sie wie folgt vor, um Ihre ASE mit den Ausgangsadressen zu erstellen_: Befolgen Sie die Anleitung unter [Erstellen einer App Service-Umgebung mit einer Vorlage][template], und rufen Sie die entsprechende Vorlage ab.  Bearbeiten Sie den Abschnitt „resources“ in der Datei „azuredeploy.json“, aber nicht im Block „properties“, und fügen Sie eine Zeile für **userWhitelistedIpRanges** mit Ihren Werten ein.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/hostingEnvironments",
        "name": "[parameters('aseName')]",
        "kind": "ASEV2",
        "location": "[parameters('aseLocation')]",
        "properties": {
          "name": "[parameters('aseName')]",
          "location": "[parameters('aseLocation')]",
          "ipSslAddressCount": 0,
          "internalLoadBalancingMode": "[parameters('internalLoadBalancingMode')]",
          "dnsSuffix" : "[parameters('dnsSuffix')]",
          "virtualNetwork": {
            "Id": "[parameters('existingVnetResourceId')]",
            "Subnet": "[parameters('subnetName')]"
          },
        "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]
        }
      }
    ]

Durch diese Änderungen wird Datenverkehr an Azure Storage direkt aus der ASE gesendet und der Zugriff auf Azure SQL von zusätzlichen Adressen zur VIP der ASE ermöglicht.

   ![Tunnelerzwingung mit SQL-Whitelist][3]

## <a name="preventing-issues"></a>Verhindern von Problemen ##

Wenn die Kommunikation zwischen der ASE und ihren Abhängigkeiten unterbrochen ist, ist die ASE nicht mehr fehlerfrei.  Falls dieser Fehlerzustand zu lange anhält, wird die ASE angehalten. Befolgen Sie die Anleitung im ASE-Portal, um den angehaltenen Zustand für die ASE zu beenden.

Zusätzlich zur Unterbrechung der Kommunikation kann es auch zu einer negativen Beeinträchtigung Ihrer ASE kommen, wenn Sie zu viel Latenz zulassen. Die Latenz kann zu hoch sein, wenn sich Ihre ASE in zu weiter Entfernung von Ihrem lokalen Netzwerk befindet.  Beispiele für eine zu weite Entfernung zur Erreichung Ihres lokalen Netzwerks sind ein Ozean oder ein Kontinent, der überwunden werden muss. Latenz kann auch entstehen, wenn das Intranet überlastet ist oder Einschränkungen der ausgehenden Bandbreite vorliegen.


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/asedependencies.png
[2]: ./media/forced-tunnel-support/forcedtunnelserviceendpoint.png
[3]: ./media/forced-tunnel-support/forcedtunnelexceptstorage.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
