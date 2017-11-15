---
title: Azure App Service-Hybridverbindungen | Microsoft-Dokumentation
description: "Gewusst wie: Erstellen und Verwenden von Hybridverbindungen für den Zugriff auf Ressourcen in unterschiedlichen Netzwerken"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: ccompy
ms.openlocfilehash: f0b148cb9c304c54b47be9601740e7634462d59b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service-Hybridverbindungen #

## <a name="overview"></a>Übersicht ##

Hybridverbindungen sind sowohl ein Dienst in Azure als auch eine Funktion in Azure App Service.  Als Dienst sind Einsatzmöglichkeiten und Funktionen möglich, die über die von Azure App Service hinausgehen.  Weitere Informationen über Hybridverbindungen und dessen Verwendung außerhalb von Azure App Service finden Sie hier: [Azure-Relayhybridverbindungen][HCService]

In Azure App Service kann mithilfe von Hybridverbindungen auf Anwendungsressourcen in anderen Netzwerken zugegriffen werden. Es ermöglicht den Zugriff VON Ihrer App AUF einen Anwendungsendpunkt.  Die Funktion bietet keine alternative Möglichkeit für den Zugriff auf Ihre Anwendung.  Bei der Verwendung in App Service entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port.  Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, vorausgesetzt, dass Sie einen TCP-Überwachungsport festlegen. Hybridverbindungen besitzen keine Informationen über das Anwendungsprotokoll oder den abzurufenden Inhalt und benötigen diese Informationen auch nicht.  Sie ermöglichen lediglich den Netzwerkzugriff.  


## <a name="how-it-works"></a>So funktioniert's ##
Das Feature für Hybridverbindungen besteht aus zwei ausgehenden Aufrufen von Service Bus Relay.  Es besteht eine Verbindung mit der Bibliothek auf dem Host, auf dem Ihre App in App Service ausgeführt wird, sowie eine Verbindung zwischen dem Hybridverbindungs-Manager (HCM) und Service Bus Relay.  Der HCM ist ein Relaydienst, den Sie in dem Netzwerk bereitstellen, das die Ressource hostet, auf die Sie zugreifen möchten. 

Über die beiden verknüpften Verbindungen verfügt Ihre App über einen TCP-Tunnel zu einer festen Host:Port-Kombination auf der anderen Seite des HCM.  Die Verbindung verwendet zum Schutz TLS 1.2 und zur Authentifizierung/Autorisierung SAS-Schlüssel.    

![Allgemeiner Ablauf bei Hybridverbindungen][1]

Wenn Ihre App eine DNS-Anforderung stellt, die mit einem konfigurierten Hybridverbindungsendpunkt übereinstimmt, wird der ausgehende TCP-Datenverkehr über die Hybridverbindung weitergeleitet.  

> [!NOTE]
> Sie sollten folglich versuchen, stets einen DNS-Namen für Ihre Hybridverbindung zu verwenden.  Einige Clientsoftware führen keine DNS-Suche durch, wenn der Endpunkt stattdessen eine IP-Adresse verwendet.
>
>

Es gibt zwei Arten von Hybridverbindungen: die neuen Hybridverbindungen, die als Dienst im Rahmen von Azure Relay angeboten werden, und die älteren Hybridverbindungen von BizTalk.  Die älteren BizTalk-Hybridverbindungen werden als klassische Hybridverbindungen im Portal bezeichnet.  Weiter unten in diesem Dokument finden Sie weitere Informationen hierzu.

### <a name="app-service-hybrid-connection-benefits"></a>Vorteile der App Service-Hybridverbindungen ###

Die Funktion für Hybridverbindungen bietet eine Reihe von Vorteilen wie etwa Folgende:

- Sie ermöglicht Apps den sicheren Zugriff auf lokale Systeme und Dienste.
- Für das Feature ist kein Endpunkt erforderlich, der über das Internet zugänglich ist.
- Sie lässt sich schnell und einfach einrichten. 
- Jede Hybridverbindung entspricht einer einzelnen Host:Port-Kombination, was auch ein hervorragender Sicherheitsvorteil ist.
- Es sind normalerweise keine Firewallöffnungen erforderlich, da alle Verbindungen über die standardmäßige Webports ausgehen.
- Da die Funktion auf Netzwerkebene ausgeführt wird, ist sie nicht von der Sprache, die von Ihrer App verwendet wird, und von der Technologie, die vom Endpunkt verwendet wird, abhängig.
- Sie kann verwendet werden, um über eine einzige App Zugriff in mehreren Netzwerken bereitzustellen. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Einschränkungen bei Hybridverbindungen ###

Bei Hybridverbindungen sind bestimmte Vorgänge und Funktionen nicht möglich. Hierzu zählt beispielsweise Folgendes:

- Bereitstellung eines Laufwerks
- Verwendung von UDP
- Zugriff auf TCP-basierte Dienste, die dynamische Ports verwenden, wie der passive Modus für FTP oder der erweiterte passive Modus
- Unterstützung für LDAP, da gelegentlich UDP erforderlich ist
- Unterstützung für Active Directory

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Hinzufügen und Erstellen einer Hybridverbindung in Ihrer App ##

Hybridverbindungen können über Ihre App Service-App im Azure-Portal oder über Azure-Relay im Azure-Portal erstellt werden.  Es wird dringend empfohlen, Hybridverbindungen über die App Service-App zu erstellen, die Sie mit der Hybridverbindung verwenden möchten.  Navigieren Sie zum Erstellen einer Hybridverbindung zum [Azure-Portal][portal], und wählen Sie Ihre App aus.  Wählen Sie **Netzwerk > Endpunkte der Hybridverbindung konfigurieren** aus.  In dieser Ansicht können Sie die Hybridverbindungen sehen, die für Ihre App konfiguriert sind.  

![Liste der Hybridverbindungen][2]

Um eine neue Hybridverbindung hinzuzufügen, klicken Sie auf „Hybridverbindung hinzufügen“.  Auf der daraufhin geöffneten Benutzeroberfläche werden die Hybridverbindungen aufgeführt, die Sie bereits erstellt haben.  Um eine oder mehrere dieser Hybridverbindungen zu Ihrer App hinzuzufügen, klicken Sie auf diese und anschließend auf **Ausgewählte Hybridverbindung hinzufügen**.  

![Portal für Hybridverbindungen][3]

Wenn Sie eine neue Hybridverbindung erstellen möchten, klicken Sie auf **Neue Hybridverbindung erstellen**.  In dieser Ansicht können Sie Folgendes festlegen: 

- Endpunktname
- Endpunkthostname
- Endpunktport
- Zu verwendender Service Bus-Namespace

![Hybridverbindung erstellen][4]

Jede Hybridverbindung ist mit einem Service Bus-Namespace verbunden. Jeder Service Bus-Namespace befindet sich in einer Azure-Region.  Es ist wichtig, einen Service Bus-Namespace in derselben Region wie Ihre App zu verwenden, um durch das Netzwerk verursachte Latenzen zu vermeiden.

Wenn Sie Ihre Hybridverbindung aus Ihrer App entfernen möchten, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Verbindung trennen** aus.  

Sobald eine Hybridverbindung zu Ihrer App hinzugefügt wurde, können Sie die Details hierzu anzeigen, indem Sie einfach darauf klicken. 

![Details der Hybridverbindung][5]

### <a name="creating-a-hybrid-connection-in-the-azure-relay-portal"></a>Erstellen einer Hybridverbindung im Azure-Relay-Portal ###

Zusätzlich zur Portalumgebung über Ihre App besteht auch eine Möglichkeit zum Erstellen von Hybridverbindungen über das Azure-Relay-Portal. Damit eine Hybridverbindung von Azure App Service verwendet werden kann, muss sie zwei Kriterien erfüllen. Sie muss:

* eine Clientautorisierung erfordern
* ein Metadatenelement namens „endpoint“ aufweisen, das eine Host:Port-Kombination als Wert enthält

## <a name="hybrid-connections-and-app-service-plans"></a>Hybridverbindungen und App Service-Pläne ##

Hybridverbindungen sind nur in den Preis-SKUs Basic, Standard, Premium und Isolated verfügbar.  Es gelten Beschränkungen, die am Tarif geknüpft sind.  

> [!NOTE] 
> Sie können neue Hybridverbindungen nur basierend auf Azure-Relay erstellen. Sie können keine neuen BizTalk-Hybridverbindungen erstellen.
>

| Tarif | Anzahl der im Plan verwendbaren Hybridverbindungen |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Da es sich um Einschränkungen des App Service-Plans handelt, ist auch eine Benutzeroberfläche im App Service-Plan vorhanden, auf der angezeigt wird, wie viele Hybridverbindungen und von welchen Apps diese derzeit verwendet werden.  

![Eigenschaften von App Service-Planebenen][6]

Sie sehen die Details zu Ihrer Hybridverbindung, indem Sie darauf klicken.  In den Eigenschaften, die hier gezeigt werden, können Sie alle Informationen sehen, die in der App-Ansicht angezeigt wurden. Sie sehen jedoch auch, wie viele andere Apps im selben App Service-Plan diese Hybridverbindung verwenden.

Es gibt zwar einen Grenzwert für die Anzahl der Hybridverbindungsendpunkte, die in einem App Service-Plan verwendet werden können, jedoch kann jede verwendete Hybridverbindung über eine beliebige Anzahl von Apps in diesem App Service-Plan verwendet werden.  Eine einzelne Hybridverbindung, die in fünf separaten Apps in einem App Service-Plan verwendet wird, zählt also als eine Hybridverbindung.

Es fallen zusätzliche Kosten für die Verwendung von Hybridverbindungen.  Ausführliche Informationen zu den Preisen von Hybridverbindungen finden Sie unter: [Service Bus-Preise][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

Damit Hybridverbindungen funktionieren, ist ein Relay-Agent in dem Netzwerk erforderlich, das Ihren Hybridverbindungsendpunkt hostet.  Dieser Relay-Agent wird als Hybrid Connection Manager (HCM) bezeichnet.  Dieses Tool kann über die Benutzeroberfläche unter **Netzwerk > Endpunkte der Hybridverbindung konfigurieren** in Ihrer App im [Azure-Portal][portal] heruntergeladen werden.  

Dieses Tool wird auf Windows Server 2012 und höheren Windows-Versionen ausgeführt.  Nach der Installation wird der HCM als Dienst ausgeführt.  Dieser Dienst stellt basierend auf den konfigurierten Endpunkten eine Verbindung mit Azure Service Bus Relay her.  Bei den Verbindungen vom HCM handelt es sich um ausgehende Verbindungen mit Azure über Port 443.    

Der HCM verfügt über eine Benutzeroberfläche für die Konfiguration.  Nach der Installation des HCM können Sie die Benutzeroberfläche aufrufen, indem Sie „HybridConnectionManagerUi.exe“ im Installationsverzeichnis des Hybridverbindungs-Managers ausführen.  Er kann auch mühelos auf Windows 10 erreicht werden, indem Sie *Hybrid Connection Manager UI* in Ihr Suchfeld eingeben.  

![Portal für Hybridverbindungen][7]

Wenn die Benutzeroberfläche des HCM gestartet wird, wird als Erstes eine Tabelle angezeigt, in der alle Hybridverbindungen, die mit dieser Instanz des HCM konfiguriert sind, aufgeführt werden.  Wenn Sie Änderungen vornehmen möchten, müssen Sie sich bei Azure authentifizieren. 

So fügen Sie Ihrem HCM eine oder mehrere Hybridverbindungen hinzu:

1. Starten der Benutzeroberfläche des HCM
1. Klicken Sie auf „Andere Hybridverbindung konfigurieren“.![Hinzufügen einer Hybridverbindung in HCM][8]

1. Melden Sie sich mit Ihrem Azure-Konto an.
1. Wählen Sie ein Abonnement.
1. Klicken Sie auf die Hybridverbindungen, die dieser HCM weiterleiten soll.![Auswählen einer Hybridverbindung][9]

1. Klicken Sie auf Speichern.

An diesem Punkt werden die Hybridverbindungen angezeigt, die Sie hinzugefügt haben.  Sie können auch auf die konfigurierte Hybridverbindung klicken und ausführliche Informationen dazu anzeigen.

![Details zur Hybridverbindung][10]

Damit der HCM die bei ihm konfigurierten Hybridverbindungen unterstützen kann, ist Folgendes erforderlich:

- TCP-Zugriff auf Azure über die Ports 80 und 443
- TCP-Zugriff auf den Hybridverbindungsendpunkt
- Möglichkeit für DNS-Suchvorgänge auf dem Endpunkthost und im Azure Service Bus-Namespace

Der HCM unterstützt sowohl neue Hybridverbindungen als auch die älteren BizTalk-Hybridverbindungen.

> [!NOTE]
> Azure-Relay nutzt Websockets für die Konnektivität. Diese Funktion ist nur für Windows Server 2012 oder höher verfügbar. Aus diesem Grund wird der Hybridverbindungs-Manager von Versionen vor Windows Server 2012 nicht unterstützt.
>

### <a name="redundancy"></a>Redundanz ###

Jeder HCM kann mehrere Hybridverbindungen unterstützen.  Darüber hinaus kann eine bestimmte Hybridverbindung durch mehrere HCMs unterstützt werden.  Das Standardverhalten ist ein Roundrobin-Datenverkehr über die konfigurierten HCMs für einen bestimmten Endpunkt.  Wenn Sie eine hohe Verfügbarkeit für Ihre Hybridverbindungen über Ihr Netzwerk wünschen, instanziieren Sie einfach mehrere HCMs auf unterschiedlichen Computern. 

### <a name="manually-adding-a-hybrid-connection"></a>Manuelles Hinzufügen einer Hybridverbindung ###

Wenn ein Benutzer außerhalb Ihres Abonnements eine HCM-Instanz für eine bestimmte Hybridverbindung hosten soll, können Sie für ihn die Gatewayverbindungszeichenfolge für die Hybridverbindung freigeben.  Diese können Sie in den Eigenschaften einer Hybridverbindung im [Azure-Portal][portal] sehen. Um diese Zeichenfolge zu verwenden, klicken Sie im HCM auf die Schaltfläche **Manuell eingeben**, und fügen Sie die Gatewayverbindungszeichenfolge ein.


## <a name="troubleshooting"></a>Problembehandlung ##

Der Verbindungsstatus für eine Hybridverbindung bedeutet, dass mindestens ein HCM mit dieser Hybridverbindung konfiguriert ist und Azure erreichen kann.  Wenn der Status für die Hybridverbindung nicht **Verbunden** ist, wurde die Hybridverbindung nicht auf einem HCM konfiguriert, der auf Azure zugreifen kann.

Der Hauptgrund, warum Clients keine Verbindung zum jeweiligen Endpunkt herstellen können, ist, dass der Endpunkt mit einer IP-Adresse anstelle eines DNS-Namens angegeben wurde.  Wenn Ihre App den gewünschten Endpunkt nicht erreichen kann und Sie eine IP-Adresse verwendet haben, gehen Sie zur Verwendung eines DNS-Namens über, der auf dem Host, auf dem der HCM ausgeführt wird, gültig ist.  Darüber hinaus sollten Sie prüfen, ob der DNS-Name auf dem Host, auf dem der HCM ausgeführt wird, ordnungsgemäß aufgelöst wird, und eine Verbindung zwischen dem Host, auf dem der HCM ausgeführt wird, und dem Hybridverbindungsendpunkt besteht.  

In App Service ist das Tool „tcpping“ vorhanden, das über die Konsole „Erweiterte Tools“ (Kudu) aufgerufen werden kann.  Mit diesem Tool können Sie feststellen, ob Sie Zugriff auf einen TCP-Endpunkt haben, jedoch nicht, ob Sie Zugriff auf einen Hybridverbindungsendpunkt haben.  Bei Verwendung in der Konsole für einen Hybridverbindungsendpunkt stellt ein erfolgreicher Ping lediglich fest, ob eine Hybridverbindung für Ihre App, die diese Host:Port-Kombination verwendet, konfiguriert ist.  

## <a name="biztalk-hybrid-connections"></a>BizTalk-Hybridverbindungen ##

Die ältere Funktion der BizTalk-Hybridverbindungen wurde für neue BizTalk-Hybridverbindungen gesperrt.  Sie können weiterhin Ihre bereits vorhandenen BizTalk-Hybridverbindungen mit Ihren Apps verwenden, sollten sie jedoch in neue Hybridverbindungen migrieren, die Azure-Relay nutzen.  Der neue Dienst bringt gegenüber der BizTalk-Version folgende Vorteile mit sich:

- Kein zusätzliches BizTalk-Konto ist erforderlich.
- TLS 1.2 statt 1.0 wie bei BizTalk-Hybridverbindungen.
- Kommunikation über die Ports 80 und 443, um Azure mithilfe eines DNS-Namens anstelle von IP-Adressen und einer Reihe von zusätzlichen anderen Ports zu erreichen  

Um Ihrer App eine vorhandene BizTalk-Hybridverbindung hinzuzufügen, navigieren Sie im [Azure-Portal][portal] zu Ihrer App, und klicken Sie auf **Netzwerk > Endpunkte der Hybridverbindung konfigurieren**.  Klicken Sie in der Tabelle „Klassische Hybridverbindungen“ auf **Klassische Hybridverbindung hinzufügen**.  In dieser Ansicht sehen Sie eine Liste Ihrer BizTalk-Hybridverbindungen.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/
