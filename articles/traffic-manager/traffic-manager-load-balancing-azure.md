---
title: Verwendung des Lastenausgleichs Dienste in Azure | Microsoft Docs
description: 'Diesem Lernprogramm erfahren Sie, wie Sie ein Szenario erstellen Sie mithilfe des Azure-Lastenausgleich Portfolios: Traffic Manager Application Gateway und Lastenausgleich.'
services: traffic-manager
documentationcenter: 
author: liumichelle
manager: vitinnan
editor: 
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: ae9bd30b76786f94f0d836a39137da696fdb94a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="using-load-balancing-services-in-azure"></a>Verwenden des Lastenausgleichs-Diensten in Azure

## <a name="introduction"></a>Einführung

Microsoft Azure bietet mehrere Dienste für die Verwaltung der Verteilung des Netzwerkdatenverkehrs und Lastenausgleich. Sie können diese Dienste einzeln oder kombinieren deren Methoden je nach Bedarf, um die optimale Lösung zu erstellen.

In diesem Lernprogramm wir definieren Sie zunächst einen Kunde Anwendungsfall und wie sie eine robustere festgelegt werden kann und leistungsfähiger mithilfe der folgenden Azure-Lastenausgleich Portfolio angezeigt: Traffic Manager Application Gateway und Lastenausgleich. Klicken Sie dann bieten wir eine schrittweise Anleitung zum Erstellen einer Bereitstellung, die geografisch redundant und verteilt den Datenverkehr zu virtuellen Computern können Sie verschiedene Anforderungen verwalten.

Auf konzeptioneller Ebene spielt jeder dieser Dienste eine unterschiedliche Rolle in der Hierarchie des Lastenausgleichs.

* **Traffic Manager** enthält globale DNS des Lastenausgleichs. Es prüft auf eingehende DNS-Anforderungen und antwortet mit einem fehlerfreien Endpunkt, entsprechend der routing-Richtlinie, die der Kunde ausgewählt wurde. Optionen für Verteilermethoden lauten:
  * Leistung, routing, um den anforderer zum nächstgelegenen Endpunkt im Hinblick auf Latenz zu senden.
  * Priorität routing, um alle Leitung des Datenverkehrs an einen Endpunkt, mit anderen Endpunkten als Sicherung.
  * Gewichteter Roundrobin-routing, der verteilt den Datenverkehr basierend auf der Gewichtung, die an jeden Endpunkt zugewiesen wird.

  Der Client direkt eine Verbindung mit diesem Endpunkt. Azure Traffic Manager erkennt, wenn ein Endpunkt fehlerhaft ist und dann die Clients in eine andere fehlerfrei Instanz leitet. Verweisen auf [Azure Traffic Manager-Dokumentation](traffic-manager-overview.md) erfahren Sie mehr über den Dienst.
* **Application Gateway** Übermittlung Controller für die Anwendung (ADC) als ein Dienstangebot verschiedene Layer 7 Lastenausgleichsfunktionen für Ihre Anwendung enthält. Sie können Kunden Webfarm Produktivität zu optimieren, indem Sie die CPU-Intensive SSL-Tunnelabschluss an die Anwendungsgateway-Abladung. Andere Layer 7 Routingfunktionen enthalten roundrobinverteilung von eingehenden Datenverkehr, Cookie-basierte sitzungsaffinität, URL-routing pfadbasierten und die Möglichkeit, mehrere Websites hinter einem einzelnen Anwendungsgateway hosten. Application Gateway kann als ein Gateway Internetzugriff, ein Gateway nur intern oder eine Kombination aus beidem konfiguriert werden. Application Gateway wird vollständig Azure verwalteten, skalierbaren und hoch verfügbar. Er bietet einen umfangreichen Satz von Diagnose- und Protokollierungsfunktionen für bessere Verwaltbarkeit.
* **Load Balancer** ist ein wesentlicher Bestandteil des Azure-SDN-Stapel, hohe Leistung, niedrige Latenz Layer 4-Lastenausgleich Dienste für alle UDP und TCP-Protokolle. Er verwaltet eingehende und ausgehende Verbindungen. Sie können konfigurieren öffentliche und interne Endpunkte mit Lastenausgleich und definieren Regeln, um eingehende Verbindungen an Back-End-Pool Ziele zu ordnen, indem Sie TCP und HTTP durch Überprüfung der Integrität von Optionen zum Verwalten der Verfügbarkeit des Diensts.

## <a name="scenario"></a>Szenario

In diesem Beispielszenario verwenden wir eine einfache Website, die zwei Typen von Inhalten dient: Bilder und dynamisch gerenderte Webseiten. Die Website muss geografisch redundant sein und sollte es die Benutzern über den nächstgelegenen (niedrigste Latenz) dienen Speicherort Ihnen. Der Anwendungsentwickler hat entschieden, die alle URLs, die das Muster/Images/entsprechen * von VMs, die sich vom Rest der Webfarm einen dedizierten Pool verarbeitet werden.

Darüber hinaus muss der VM-Standardpool, die für den dynamischen Inhalt sprechen Sie mit einer Back-End-Datenbank, die gehostet wird, in einem Cluster mit hoher Verfügbarkeit. Die gesamte Bereitstellung wird durch Azure Resource Manager eingerichtet.

Verwenden von Traffic Manager Application Gateway und Lastenausgleich ermöglicht diese Website, um diese Ziele zu erreichen:

* **Multi-Geo-Redundanz**: fällt eine Region aus, Traffic Manager Datenverkehr weiterleitet nahtlos in die nächstgelegene Region ohne jegliches Eingreifen des aus der Besitzer der Anwendung.
* **Reduziert die Latenz**: Da Traffic Manager weist automatisch der Kunde die nächstgelegene Region, kundenmeinungen geringere Latenz beim Anfordern der Inhalt der Webseite.
* **Unabhängige Skalierbarkeit**: Da die Arbeitslast der Web-Anwendung vom Typ des Inhalts getrennt ist, kann Anwendungsbesitzer die Anforderung Arbeitslasten, die unabhängig voneinander skalieren. Application Gateway wird sichergestellt, dass der Datenverkehr an den richtigen Pools basierend auf den angegebenen Vergleichsregeln und die Integrität der Anwendung weitergeleitet wird.
* **Interner Lastenausgleich**: Da Lastenausgleich befindet sich vor dem Cluster für hohe Verfügbarkeit, nur die aktive und fehlerfreie Endpunkt für eine Datenbank für die Anwendung verfügbar gemacht wird. Darüber hinaus kann ein Datenbankadministrator die arbeitsauslastung optimieren, indem Sie das Verteilen von aktiven und passiven Replikate auf dem Cluster, die unabhängig von der Front-End-Anwendung. Load Balancer stellt Verbindungen mit dem Cluster für hohe Verfügbarkeit und stellt sicher, dass nur fehlerfreie Datenbanken verbindungsanforderungen erhalten.

Das folgende Diagramm zeigt die Architektur dieses Szenarios:

![Diagramm des Lastenausgleichs Architektur](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> In diesem Beispiel ist nur eine von vielen möglichen Konfigurationen der Netzwerklastenausgleich Dienste, die Azure bietet. Traffic Manager, Application Gateway und Lastenausgleich können gemischt werden, und am besten entsprechen Ihren Anforderungen Lastenausgleich zugeordnet. Beispielsweise können SSL auslagern oder Layer 7-Verarbeitung ist nicht notwendig, Load Balancer anstelle von Application Gateway verwendet werden.

## <a name="setting-up-the-load-balancing-stack"></a>Einrichten der Stapel des Lastenausgleichs

### <a name="step-1-create-a-traffic-manager-profile"></a>Schritt 1: Erstellen eines Traffic Manager-Profils

1. Klicken Sie im Azure-Portal auf **neu**, und suchen Sie den Marketplace nach "Traffic Manager-Profil".
2. Auf der **erstellen Traffic Manager-Profil** Blatt, geben Sie die folgenden grundlegende Informationen:

  * **Namen**: Traffic Manager-Profil ein DNS-Präfixnamen erteilen.
  * **Routingmethode**: Wählen Sie die Richtlinie für die datenverkehrrouting Methode. Weitere Informationen zu den Methoden finden Sie unter [zu Traffic Manager Datenverkehr Verteilermethoden](traffic-manager-routing-methods.md).
  * **Abonnement**: Wählen Sie das Abonnement, die das Profil enthält.
  * **Ressourcengruppe**: Wählen Sie die Ressourcengruppe, die das Profil enthält. Es kann eine neue oder vorhandene Ressourcengruppe sein.
  * **Speicherort der Ressourcengruppe**: Traffic Manager-Dienst ist ein global und nicht an einen Speicherort gebunden. Allerdings müssen Sie eine Region für die Gruppe angeben, in dem sich die Traffic Manager-Profil zugeordnete Metadaten befindet. Dieser Standort hat keine Auswirkungen auf die laufzeitverfügbarkeit des Profils.

3. Klicken Sie auf **erstellen** Traffic Manager-Profils zu generieren.

  ![Blatt "Erstellen von Traffic Manager"](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-the-application-gateways"></a>Schritt 2: Erstellen der Anwendungsgateways

1. Klicken Sie im Azure-Portal im linken Bereich auf **neu** > **Networking** > **Anwendungsgateway**.
2. Geben Sie die folgenden grundlegende Informationen über das Application Gateway:

  * **Namen**: der Name des Anwendungsgateways.
  * **SKU-Größe**: die Größe der Anwendung-Gateways als klein, Mittel oder groß.
  * **Instanzanzahl**: die Anzahl der Instanzen, die einen Wert zwischen 2 und 10.
  * **Ressourcengruppe**: die Ressourcengruppe, die das Anwendungsgateway enthält. Sie können eine vorhandene Ressourcengruppe oder einen neuen sein.
  * **Speicherort**: die Region für das Anwendungsgateway, die am gleichen Speicherort wie die Ressourcengruppe ist. Der Speicherort ist wichtig, da das virtuelle Netzwerk und die öffentliche IP-Adresse am gleichen Speicherort wie das Gateway sein muss.
3. Klicken Sie auf **OK**.
4. Definieren Sie das virtuelle Netzwerk, Subnetz, Front-End-IP und Listenerkonfigurationen für das Anwendungsgateway. In diesem Szenario ist die Front-End-IP-Adresse **öffentlichen**, womit als Endpunkt Traffic Manager-Profil einem späteren Zeitpunkt hinzugefügt werden.
5. Konfigurieren Sie den Listener mit einem der folgenden Optionen aus:
    * Wenn Sie HTTP verwenden, wird nichts zu konfigurieren. Klicken Sie auf **OK**.
    * Wenn Sie HTTPS verwenden, sind weiteren Konfigurationsschritte erforderlich. Verweisen auf [erstellen Sie einen vorhandenes Anwendungsgateway](../application-gateway/application-gateway-create-gateway-portal.md), beginnend bei Schritt 9. Wenn Sie die Konfiguration abgeschlossen haben, klicken Sie auf **OK**.

#### <a name="configure-url-routing-for-application-gateways"></a>Konfigurieren Sie die URL-routing für Anwendungsgateways

Bei der Auswahl eines Back-End-Pools wird ein vorhandenes Anwendungsgateway, der mit einem Pfad basierenden Regel konfiguriert ist ein pfadmuster zum der Anforderungs-URL neben Round-Robin-Verteilung. In diesem Szenario wir sind das Hinzufügen einer pfadbasierten Regel zum Weiterleiten von beliebigen URL mit "/ Images /\*" zum Serverpool Bild. Weitere Informationen zum Konfigurieren der URL für einen Anwendungsgateway routing pfadbasierten finden Sie in [erstellen Sie eine Regel pfadbasierten für ein vorhandenes Anwendungsgateway](../application-gateway/application-gateway-create-url-route-portal.md).

![Application Gateway Webebene Diagramm](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Wechseln Sie zu der Instanz des Application-Gateways, das Sie im vorherigen Abschnitt erstellt haben, aus der Ressourcengruppe.
2. Klicken Sie unter **Einstellungen**Option **Back-End-Pools**, und wählen Sie dann **hinzufügen** die virtuellen Computer hinzufügen, die die Webebene Back-End-Adresspools zugeordnet werden sollen.
3. Auf der **hinzufügen Back-End-Pools** Blatt, geben Sie den Namen des Back-End-Pool und die IP-Adressen der Computer, die im Pool befinden. In diesem Szenario sind wir zwei Back-End-Server-Pools von virtuellen Computern verbunden.

  ![Application Gateway "Back-End-Pool hinzufügen" Blatt "](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Klicken Sie unter **Einstellungen** wählen Sie das Anwendungsgateway **Regeln**, und klicken Sie dann auf die **Pfadbasierter** Schaltfläche, um eine Regel hinzuzufügen.

  ![Gateway-Regeln "Pfad basiert" Anwendungsschaltfläche](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. Auf der **pfadbasierten Regel hinzufügen** Blatt, konfigurieren Sie die Regel, indem Sie die folgende Informationen bereitstellen.

   Grundlegende Einstellungen:

   + **Namen**: der Anzeigename der Regel, die im Portal zugegriffen werden kann.
   + **Listener**: den Listener, der für die Regel verwendet wird.
   + **Standard-Back-End-Pool**: der Back-End-Pool mit der Standardregel verwendet werden soll.
   + **Standardeinstellungen für die HTTP-**: der HTTP-Einstellungen mit der Standardregel verwendet werden soll.

   Pfad-Regeln:

   + **Namen**: der Anzeigename der Regel pfadbasierten.
   + **Pfade**: der Pfadregel an, die für die Weiterleitung von Datenverkehr verwendet wird.
   + **Back-End-Pool**: der Back-End-Pool mit dieser Regel verwendet werden soll.
   + **HTTP-Einstellung**: der HTTP-Einstellungen, die mit dieser Regel verwendet werden.

   > [!IMPORTANT]
   > Pfade: Gültige Pfade müssen beginnen mit "/". Der Platzhalter "\*" ist nur am Ende zulässig. Beispiele für gültige sind /xyz /xyz\*, oder /xyz/\*.

   ![Application Gateway "Pfadbasierten Regel hinzufügen" Blatt "](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-to-the-traffic-manager-endpoints"></a>Schritt 3: Hinzufügen von Anwendungsgateways an die Traffic Manager-Endpunkte

In diesem Szenario ist die Traffic Manager mit Anwendungsgateways (wie in den vorherigen Schritten konfiguriert) verbunden, die sich in unterschiedlichen Regionen befinden. Nun, dass die Anwendungsgateways konfiguriert sind, besteht der nächste Schritt zum Verbinden der Computer mit Traffic Manager-Profil.

1. Öffnen Sie Ihr Traffic Manager-Profil ein. Zu diesem Zweck zu suchen, in der Ressourcengruppe oder suchen Sie nach den Namen des Traffic Manager-Profils aus **alle Ressourcen**.
2. Wählen Sie im linken Bereich **Endpunkte**, und klicken Sie dann auf **hinzufügen** zum Hinzufügen eines Endpunkts.

  ![Traffic Manager Endpunkten "Add"-Schaltfläche](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Auf der **Endpunkt hinzufügen** Blatt, erstellen Sie einen Endpunkt durch Eingabe der folgenden Informationen:

  * **Typ**: Wählen Sie den Typ des Endpunkts für den Lastenausgleich. Wählen Sie in diesem Szenario **Azure-Endpunkt** da wir es mit den Anwendungsinstanzen für das Gateway eine Verbindung herstellen, die zuvor konfiguriert wurden.
  * **Namen**: Geben Sie den Namen des Endpunkts.
  * **Ressourcentyp "" als Ziel**: Wählen Sie **öffentliche IP-Adresse** und dann unter **Zielressource**, wählen Sie die öffentliche IP-Adresse des Anwendungsgateways, die zuvor konfiguriert wurde.

   ![Traffic Manager "Hinzufügen von Endpunkt" Blatt "](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Nun können Sie das Setup darauf zugreifen, mit dem DNS-Namen des Traffic Manager-Profil testen (in diesem Beispiel: TrafficManagerScenario.trafficmanager.net). Sie können Anforderungen senden, Hochfahren oder VMs und Webservern, die in unterschiedlichen Regionen erstellt wurden heruntergefahren und ändern die Einstellungen für das Traffic Manager-Profil zum Testen Ihrer Einrichtung.

### <a name="step-4-create-a-load-balancer"></a>Schritt 4: Erstellen Sie einen Lastenausgleich

In diesem Szenario verteilt Load Balancer Verbindungen zwischen der Webebene und die Datenbanken innerhalb eines Clusters hohe Verfügbarkeit.

Wenn Ihre hohe Verfügbarkeit Datenbankcluster SQL Server AlwaysOn verwendet wird, finden Sie in [konfigurieren Sie eine oder mehrere immer auf Verfügbarkeitsgruppenlistener](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) für-Schritt-Anweisungen.

Weitere Informationen zum Konfigurieren von einem internen Lastenausgleich finden Sie unter [erstellen Sie einen internen Lastenausgleich im Azure-Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. Klicken Sie im Azure-Portal im linken Bereich auf **neu** > **Networking** > **Load Balancer**.
2. Auf der **Lastenausgleich erstellen** Blatt, wählen Sie einen Namen für Ihr Lastenausgleichsmodul.
3. Legen Sie die **Typ** auf **intern**, und wählen Sie die entsprechenden virtuellen Netzwerk und Subnetz für das Lastenausgleichsmodul in befinden.
4. Klicken Sie unter **IP-Adresszuweisung**, wählen Sie entweder **dynamische** oder **statische**.
5. Klicken Sie unter **Ressourcengruppe**, wählen Sie die Ressourcengruppe für das Lastenausgleichsmodul.
6. Klicken Sie unter **Speicherort**, wählen Sie die entsprechende Region für das Lastenausgleichsmodul.
7. Klicken Sie auf **erstellen** der Load Balancer zu generieren.

#### <a name="connect-a-back-end-database-tier-to-the-load-balancer"></a>Verbinden Sie einen Back-End-Datenbankschicht zum Lastenausgleich

1. Suchen Sie aus der Ressourcengruppe der Load Balancer, die in den vorherigen Schritten erstellt wurde.
2. Klicken Sie unter **Einstellungen**, klicken Sie auf **Back-End-Pools**, und klicken Sie dann auf **hinzufügen** an einen Back-End-Adresspool hinzuzufügen.

  ![Laden des Lastenausgleichsmoduls "Back-End-Pool hinzufügen" Blatt "](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. Auf der **hinzufügen Back-End-Pools** Blatt, geben Sie den Namen des Back-End-Pools.
4. Fügen Sie einzelne Computer oder einer verfügbarkeitsgruppe, die an den Back-End-Pool.

#### <a name="configure-a-probe"></a>Konfigurieren Sie eine Überprüfung

1. In Ihr Lastenausgleichsmodul unter **Einstellungen**Option **Prüfpunkte**, und klicken Sie dann auf **hinzufügen** So fügen Sie einen Prüfpunkt hinzu.

 ![Laden des Lastenausgleichsmoduls "Add-Test" Blatt "](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. Auf der **Add Prüfpunkt** Blatt, geben Sie den Namen für den Prüfpunkt.
3. Wählen Sie die **Protokoll** für den Prüfpunkt. Bei einer Datenbank sollten Sie eine HTTP-Test, anstatt eine TCP-Überprüfung. Finden Sie weitere Informationen zum Lastenausgleich Prüfpunkte [verstehen lastenausgleichsüberprüfungen](../load-balancer/load-balancer-custom-probe-overview.md).
4. Geben Sie die **Port** Ihrer Datenbank für den Zugriff auf die Überprüfung verwendet werden.
5. Klicken Sie unter **Intervall**, angeben, wie häufig die Anwendung zu suchen.
6. Klicken Sie unter **Fehlerschwellenwert**, geben Sie die Anzahl der kontinuierlichen Testfehler, die auftreten, muss für die Back-End-VM "fehlerhaft" angesehen.
7. Klicken Sie auf **OK** zum Erstellen des Prüfpunkts.

#### <a name="configure-the-load-balancing-rules"></a>Konfigurieren Sie die Regeln des Lastenausgleichs

1. Klicken Sie unter **Einstellungen** wählen Sie von Ihrem Lastenausgleichsmodul **Load Balancer-Regeln**, und klicken Sie dann auf **hinzufügen** zum Erstellen einer Regel.
2. Auf der **lastenausgleichsregel hinzufügen** Blatt, geben Sie die **Namen** für die Regel den Lastenausgleich.
3. Wählen Sie die **Front-End-IP-Adresse** des Load Balancers **Protokoll**, und **Port**.
4. Klicken Sie unter **Back-End-Port**, geben Sie den Port in der Back-End-Pool verwendet werden.
5. Wählen Sie die **Back-End-Pool** und **Prüfpunkt** , die in den vorherigen Schritten anzuwendende die Regel erstellt wurden.
6. Klicken Sie unter **Sitzungspersistenz**, wählen Sie, wie Sie die Sitzungen beibehalten werden soll.
7. Klicken Sie unter **Leerlauftimeouts**, geben Sie die Anzahl der Minuten, bevor ein Timeout im Leerlauf.
8. Klicken Sie unter **Floating IP**, wählen Sie entweder **deaktiviert** oder **aktiviert**.
9. Klicken Sie auf **OK** zum Erstellen der Regel.

### <a name="step-5-connect-web-tier-vms-to-the-load-balancer"></a>Schritt 5: Verbinden Webebene VMs mit der Load balancer

Nachdem wir die IP-Adresse und eines Lastenausgleichs Front-End-Port in der Anwendung, die auf der Webebene VMs für alle Datenbankverbindungen ausgeführt werden konfigurieren. Diese Konfiguration ist spezifisch für die Anwendungen, die auf diesen virtuellen Computern ausgeführt werden. Um die Ziel-IP-Adresse und Port zu konfigurieren, finden Sie in der Dokumentation der Anwendung. Um die IP-Adresse des Front-End in der Azure-Portal suchen, finden Sie in der Front-End-IP-Adresspool auf dem **Lastenausgleichseinstellungen** Blatt.

![Load Balancer "Frontend-IP-Adresspool" Navigationsbereich](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Traffic Manager](traffic-manager-overview.md)
* [Gateway-Anwendungsübersicht](../application-gateway/application-gateway-introduction.md)
* [Übersicht über Azure Load Balancer](../load-balancer/load-balancer-overview.md)
