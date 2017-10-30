---
title: "Exemplarische Vorgehensweise für die Connected Factory-Lösung der Azure IoT Suite | Microsoft-Dokumentation"
description: "Eine Beschreibung der vorkonfigurierten Connected Factory-Lösung von Azure IoT und ihrer Architektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 8f43196b88cf22aab66c913d0bd659b3d654cef0
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Vorkonfigurierte Connected Factory-Lösung – Exemplarische Vorgehensweise

Die [vorkonfigurierte Connected Factory-Lösung][lnk-preconfigured-solutions] der IoT Suite ist eine Komplettlösung für die Industrie, die:

* eine Verbindung mit simulierten Industriegeräten mit OPC UA-Servern in simulierten Fertigungsstrecken im Werk und mit echten OPC UA-Servergeräten herstellt. Weitere Informationen zu OPC UA finden Sie in den [häufig gestellten Fragen zur verbundenen Factory](iot-suite-faq-cf.md).
* betriebliche KPIs und OEE dieser Geräte und Fertigungsstrecken anzeigt.
* veranschaulicht, wie eine cloudbasierte Anwendung für die Interaktion mit OPC UA-Serversystemen verwendet werden kann.
* das Herstellen einer Verbindung mit eigenen OPC UA-Servergeräten ermöglicht.
* das Durchsuchen und Ändern der OPC UA-Serverdaten ermöglicht.
* in den Azure Time Series Insights-Dienst (TSI) integriert werden kann, um benutzerdefinierte Ansichten der Daten von den OPC UA-Servern bereitzustellen.

Sie können sie als Ausgangspunkt für Ihre Implementierung verwenden und [anpassen][lnk-customize], um Ihre eigenen speziellen Geschäftsanforderungen zu erfüllen.

In diesem Artikel werden einige wichtige Elemente der Connected Factory-Lösung beschrieben, um die Funktionsweise zu verdeutlichen. Außerdem erfahren Sie in diesem Artikel, wie Daten die Lösung durchlaufen. Dieses Wissen ist für folgende Zwecke hilfreich:

* Behandeln von Problemen in der Lösung
* Planen der Lösungsanpassung zur Erfüllung besonderer Anforderungen
* Entwerfen einer eigenen IoT-Lösung mit Verwendung von Azure-Diensten

Weitere Informationen finden Sie in den [häufig gestellten Fragen zur verbundenen Factory](iot-suite-faq-cf.md).

## <a name="logical-architecture"></a>Logische Architektur

Das folgende Diagramm beschreibt die logischen Komponenten der vorkonfigurierten Lösung:

![Logische Architektur von Connected Factorys][connected-factory-logical]

## <a name="communication-patterns"></a>Kommunikationsmuster

Die Lösung verwendet die [OPC UA-Pub/Sub-Spezifikation](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/), um OPC UA-Telemetriedaten im JSON-Format an IoT Hub zu senden. Zu diesem Zweck verwendet die Lösung das IoT Edge-Modul [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher).

Die Lösung verfügt auch über einen in eine Webanwendung integrierten OPC UA-Client, um die Verbindungsherstellung mit lokalen OPC UA-Servern zu ermöglichen. Der Client verwendet einen [Reverseproxy](https://wikipedia.org/wiki/Reverse_proxy) und wird von IoT Hub unterstützt, um die Verbindung ohne geöffnete Ports in der lokalen Firewall herstellen zu können. Dieses Kommunikationsmuster wird als [dienstgestützte Kommunikation](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/) bezeichnet. Zu diesem Zweck verwendet die Lösung das IoT Edge-Modul [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/).


## <a name="simulation"></a>Simulation

Die simulierten Stationen und die simulierten MES (Manufacturing Execution Systems) bilden eine Fertigungsstrecke in einem Werk. Die simulierten Geräte und das OPC Publisher-Modul basieren auf dem [.NET-Standard für OPC UA][lnk-OPC-UA-NET-Standard], der von OPC Foundation veröffentlicht wurde.

OPC Proxy und OPC Publisher werden basierend auf [Azure IoT Edge][lnk-Azure-IoT-Gateway] als Module implementiert. An jede simulierte Fertigungsstrecke ist ein designiertes Gateway angefügt.

Alle in Docker-Containern ausgeführten Simulationskomponenten werden auf einem virtuellen Azure-Computer unter Linux gehostet. Die Simulation ist standardmäßig für die Ausführung von acht simulierten Fertigungsstrecken konfiguriert.

## <a name="simulated-production-line"></a>Simulierte Fertigungsstrecke

Auf einer Fertigungsstrecke werden Teile hergestellt. Sie besteht aus verschiedenen Stationen: Montagestation, Versuchsstand und Packstation.

Die Simulation wird ausgeführt und aktualisiert die Daten, die durch die OPC UA-Knoten verfügbar gemacht werden. Alle Stationen der simulierten Fertigungsstrecke werden vom MES über OPC UA orchestriert.

## <a name="simulated-manufacturing-execution-system"></a>Simuliertes Manufacturing Execution System

Das MES überwacht jede Station der Fertigungsstrecke über OPC UA, um Statusänderungen der Stationen zu ermitteln. Durch Aufrufe von OPC UA-Methoden steuert sie die Stationen und leitet ein Produkt bis zu seiner Fertigstellung von einer Station zur nächsten weiter.

## <a name="gateway-opc-publisher-module"></a>OPC Publisher-Gatewaymodul

Das OPC Publisher-Modul stellt eine Verbindung mit den OPC UA-Servern der Stationen her und abonniert die zu veröffentlichenden OPC-Knoten. Das Modul konvertiert die Knotendaten ins JSON-Format, verschlüsselt sie und sendet sie als OPC UA-Nachrichten für Veröffentlichung/Abonnement an IoT Hub.

Für das OPC Publisher-Modul ist nur ein ausgehender HTTPS-Port (443) erforderlich, und es kann mit der vorhandenen Unternehmensinfrastruktur verwendet werden.

## <a name="gateway-opc-proxy-module"></a>OPC Proxy-Gatewaymodul

Das OPC UA Proxy-Gatewaymodul tunnelt binäre Befehls- und Steuerungsnachrichten für OPC UA und erfordert lediglich einen ausgehenden HTTPS-Port (443). Es kann mit vorhandener Unternehmensinfrastruktur, einschließlich Webproxys, verwendet werden.

Es nutzt IoT Hub-Gerätemethoden zum Übertragen zusammengefasster TCP/IP-Daten auf Anwendungsebene und stellt somit die Endpunktvertrauensstellung, Datenverschlüsselung und Integrität mithilfe von SSL/TLS sicher.

Das über den Proxy selbst weitergeleitete OPC UA-Binärprotokoll nutzt UA-Authentifizierung und -Verschlüsselung.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Das OPC Publisher-Gatewaymodul abonniert OPC UA-Serverknoten, um Änderungen der Datenwerte zu ermitteln. Wenn in einem der Knoten eine Datenänderung erkannt wird, sendet dieses Modul Nachrichten an Azure IoT Hub.

IoT Hub stellt Azure TSI eine Ereignisquelle zur Verfügung. TSI speichert Daten auf der Grundlage der an die Nachrichten angefügten Zeitstempel 30 Tage lang. Diese Daten enthalten Folgendes:

* OPC UA-Anwendungs-URI (ApplicationUri)
* OPC UA-Knoten-ID (NodeId)
* Wert des Knotens
* Zeitstempel der Quelle
* OPC UA-Anzeigename (DisplayName)

Derzeit ermöglicht TSI Kunden nicht die Anpassung der Aufbewahrungsdauer für Daten.

TSI führt mithilfe eines Zeitraums (Time.From, Time.To) Abfragen anhand von Knotendaten aus und aggregiert sie nach OPC UA-Anwendungs-URI, OPC UA-Knoten-ID oder OPC UA-Anzeigename.

Zum Abrufen der Daten für die OEE- and KPI-Messgeräte sowie die Zeitreihendiagramme werden Daten anhand der Ereignisanzahl, Summe, des Durchschnittswerts und des Mindest- und Höchstwerts aggregiert.

Die Zeitreihen werden mit einem anderen Prozess erstellt. OEE und KPIs werden aus den Stationsbasisdaten berechnet und für die Topologie (Produktionslinien, Werke, Unternehmen) in der Anwendung hochgerechnet.

Darüber hinaus werden Zeitreihen für OEE- und KPI-Topologie in der App berechnet, wenn ein angezeigter Zeitraum verfügbar ist. Die Tagesansicht wird beispielsweise zu jeder vollen Stunde aktualisiert.

Die Zeitreihenansicht der Knotendaten wird direkt von TSI mit einer Aggregation für den Zeitraum bereitgestellt.

## <a name="iot-hub"></a>IoT Hub
Der [IoT Hub][lnk-IoT Hub] empfängt die vom OPC Publisher-Modul an die Cloud gesendeten Daten und macht sie für den Azure TSI-Dienst verfügbar. 

Zu den Aufgaben des IoT Hub in der Lösung zählen auch:
- Die Verwaltung einer Identitätsregistrierung, die die IDs für alle OPC Publisher- und alle OPC Proxy-Module speichert
- Verwendung als Transportkanal für die bidirektionale Kommunikation des OPC Proxy-Moduls

## <a name="azure-storage"></a>Azure Storage
Die Lösung nutzt Azure-Blobspeicher als Datenspeicher für den virtuellen Computer sowie zum Speichern von Bereitstellungsdaten.

## <a name="web-app"></a>Web-App
Die als Teil der vorkonfigurierten Lösung bereitgestellte Web-App setzt sich aus einem integrierten OPC UA-Client, der Wartungsverarbeitung und der Telemetrievisualisierung zusammen.

## <a name="telemetry-data-flow"></a>Telemetriedatenfluss

![Telemetriedatenfluss](media/iot-suite-connected-factory-walkthrough/telemetry_dataflow.png)

### <a name="flow-steps"></a>Schritte des Datenflusses

1. OPC Publisher liest die erforderlichen OPC UA-X.509-Zertifikate und IoT Hub-Sicherheitsanmeldeinformationen aus dem lokalen Zertifikatspeicher.
    - Bei Bedarf erstellt OPC Publisher fehlende Zertifikate oder Anmeldeinformationen und speichert sie im Zertifikatspeicher.

2. OPC Publisher registriert sich selbst bei IoT Hub.
    - Verwendet das konfigurierte Protokoll. Kann jedes vom IoT Hub-Client-SDK unterstützte Protokoll verwenden. Verwendet standardmäßig „MQTT“.
    - Die Protokollkommunikation wird mittels TLS geschützt.

3. OPC Publisher liest die Konfigurationsdatei.

4. OPC Publisher erstellt eine OPC-Sitzung mit den einzelnen konfigurierten OPC UA-Servern.
    - Verwendet eine TCP-Verbindung.
    - OPC Publisher und OPC UA-Server authentifizieren sich gegenseitig über X.509-Zertifikate.
    - Jeglicher weitere OPC UA-Datenverkehr wird mithilfe des konfigurierten OPC UA Verschlüsselungsmechanismus verschlüsselt.
    - OPC Publisher erstellt in der OPC-Sitzung für jedes konfigurierte Veröffentlichungsintervall ein OPC-Abonnement.
    - Erstellt überwachte OPC-Elemente für die OPC-Knoten zur Veröffentlichung im OPC-Abonnement.

5. Wenn sich der Wert eines überwachten OPC-Knotens ändert, sendet OPC UA-Server Updates an OPC Publisher.

6. OPC Publisher transcodiert den neuen Wert.
    - Bei aktivierter Batchverarbeitung werden mehrere Änderungen zu einem Batch zusammengefasst.
    - Erstellt eine IoT Hub-Nachricht.

7. OPC Publisher sendet eine Nachricht an IoT Hub.
    - Verwendet das konfigurierte Protokoll.
    - Kommunikation wird mittels TLS geschützt.

8. Time Series Insights (TSI) liest die Nachrichten von IoT Hub.
    - Verwendet AMQP über TCP/TLS.
    - Hierbei handelt es sich um einen internen Schritt innerhalb des Datencenters.

9. Ruhende Daten in TSI.

10. Verbundene Factory-Web-App in Azure App Service fragt erforderliche Daten aus TSI ab.
    - Verwendet durch TCP/TLS geschützte Kommunikation.
    - Hierbei handelt es sich um einen internen Schritt innerhalb des Datencenters.

11. Webbrowser stellt eine Verbindung mit der verbundenen Factory-Web-App her.
    - Rendert das Dashboard der verbundenen Factory.
    - Stellt eine HTTPS-Verbindung her.
    - Der Zugriff auf die verbundene Factory-App erfordert die Authentifizierung des Benutzers über Azure Active Directory.
    - An die verbundene Factory-App gerichtete WebApi-Aufrufe werden durch Fälschungssicherheitstoken geschützt.

12. Bei Datenaktualisierungen sendet die verbundene Factory-Web-App aktualisierte Daten an den Webbrowser.
    - Verwendet das SignalR-Protokoll.
    - Gesichert durch TCP/TLS.

## <a name="browsing-data-flow"></a>Datenfluss beim Browsen

![Datenfluss beim Browsen](media/iot-suite-connected-factory-walkthrough/browsing_dataflow.png)

### <a name="flow-steps"></a>Schritte des Datenflusses

1. OPC Proxy (Serverkomponente) wird gestartet.
    - Liest die SAS-Schlüssel aus einem lokalen Speicher.
    - Bei Bedarf werden fehlende Zugriffsschlüssel im Speicher gespeichert.

2. OPC Proxy (Serverkomponente) registriert sich bei IoT Hub.
    - Liest alle bekannten Geräte von IoT Hub.
    - Verwendet MQTT über TLS über Socket oder sicheren WebSocket.

3. Webbrowser stellt eine Verbindung mit der verbundenen Factory-Web-App her und rendert das Dashboard der verbundenen Factory.
    - Verwendet HTTPS.
    - Ein Benutzer wählt einen OPC UA-Server für die Verbindungsherstellung aus.

4. Die verbundene Factory-Web-App richtet eine OPC UA-Sitzung mit dem ausgewählten OPC UA-Server ein.
    - Verwendet den OPC UA-Stapel.

5. Der OPC Proxy-Transport empfängt eine Anforderung aus dem OPC UA-Stapel zur Herstellung einer TPC-Socketverbindung mit dem OPC UA-Server.
    - Er ruft nur die TCP-Nutzlast ab und verwendet sie unverändert.
    - Hierbei handelt es sich um einen internen Schritt innerhalb der verbundenen Factory-Web-App.

6. OPC Proxy (Clientkomponente) sucht das OPC Proxy-Gerät (Serverkomponente) in der IoT Hub-Geräteregistrierung. Danach wird eine Gerätemethode des OPC Proxy-Geräts (Serverkomponente) in IoT Hub aufgerufen.
    - Verwendet HTTPS über TCP/TLS, um OPC Proxy zu suchen.
    - Verwendet HTTPS über TCP/TLS, um eine TCP-Socketverbindung mit dem OPC UA-Server herzustellen.
    - Hierbei handelt es sich um einen internen Schritt innerhalb des Datencenters.

7. IoT Hub ruft eine Gerätemethode im OPC Proxy-Gerät (Serverkomponente) auf.
    - Verwendet eine eingerichtete Verbindung mit MQTT über TLS über Socket oder sicheren WebSocket, um eine TCP-Socketverbindung mit dem OPC UA-Server herzustellen.

8. OPC Proxy (Serverkomponente) sendet die TCP-Nutzlast an das Produktionsnetzwerk.

9. Der OPC UA-Server verarbeitet die Nutzlast und sendet die Antwort zurück.

10. Die Antwort wird vom Socket der OPC Proxy-Instanz (Serverkomponente) empfangen.
    - OPC Proxy sendet die Daten als Rückgabewert der Gerätemethode an IoT Hub und OPC Proxy (Clientkomponente).
    - Diese Daten werden an den OPC UA-Stapel in der verbundenen Factory-App geliefert.

11. Die verbundene Factory-Web-App gibt OPC-Browser-UX (angereichert mit den OPC UA-spezifischen Informationen des OPC UA-Servers) zum Rendern an den Webbrowser zurück.
    - Beim Browsen durch den OPC-Adressraum und Anwenden von Funktionen auf Knoten im OPC-Adressraum verwendet der OPC-Browser-UX-Clientteil mit Fälschungssicherheitstoken geschützte AJAX-Aufrufe über HTTPS, um Daten aus der verbundenen Factory-Web-App abzurufen.
    - Bei Bedarf verwendet der Client die in den Schritten 4 bis 10 beschriebene Kommunikation für den Informationsaustausch mit dem OPC UA-Server.

> [!NOTE]
> OPC Proxy (Serverkomponente) und OPC Proxy (Clientkomponente) führen die Schritte 4 bis 10 für den gesamten TCP-Datenverkehr im Zusammenhang mit der OPC UA-Kommunikation durch.

> [!NOTE]
> Für den OPC UA-Server und den OPC UA-Stapel innerhalb der verbundenen Factory-Web-App ist die OPC Proxy-Kommunikation transparent, und alle OPC UA-Sicherheitsfeatures für Authentifizierung und Verschlüsselung werden angewendet.

## <a name="next-steps"></a>Nächste Schritte

Sie können mit den ersten Schritten mit IoT Suite fortfahren. Lesen Sie dazu die folgenden Artikel:

* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]
* [Bereitstellen eines Gateways unter Windows oder Linux für die vorkonfigurierte Connected Factory-Lösung](iot-suite-connected-factory-gateway-deployment.md)
* [OPC Publisher-Referenzimplementierung](iot-suite-connected-factory-publisher.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
