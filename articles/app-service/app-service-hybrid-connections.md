---
title: Azure App Service-Hybridverbindungen | Microsoft-Dokumentation
description: 'Gewusst wie: Erstellen und Verwenden von Hybridverbindungen für den Zugriff auf Ressourcen in unterschiedlichen Netzwerken'
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: ccompy
ms.openlocfilehash: 677642e4e97523ed71ff5857ae27263743dca535
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2017
ms.locfileid: "25990818"
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service-Hybridverbindungen #

Hybrid Connections ist sowohl ein Dienst in Azure als auch ein Feature in Azure App Service. Als Dienst gehen seine Einsatzmöglichkeiten und Funktionen über die von Azure App Service hinaus. Weitere Informationen zu Hybrid Connections und der Verwendung außerhalb von App Service finden Sie unter [Azure Relay Hybrid Connections][HCService].

In App Service kann mithilfe von Hybridverbindungen auf Anwendungsressourcen in anderen Netzwerken zugegriffen werden. Es ermöglicht den Zugriff von Ihrer App auf einen Anwendungsendpunkt. Die Funktion bietet keine alternative Möglichkeit für den Zugriff auf Ihre Anwendung. Bei der Verwendung in App Service entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port. Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, vorausgesetzt, der Zugriff erfolgt über einen TCP-Lauschport. Das Feature Hybrid Connections besitzt keine Informationen über das Anwendungsprotokoll oder den abzurufenden Inhalt und benötigt diese Informationen auch nicht. Sie ermöglichen lediglich den Netzwerkzugriff.  


## <a name="how-it-works"></a>So funktioniert's ##
Das Feature Hybrid Connections besteht aus zwei ausgehenden Aufrufen von Azure Service Bus Relay. Es gibt eine Verbindung von einer Bibliothek auf dem Host, auf dem die App in App Service ausgeführt wird. Außerdem besteht eine Verbindung vom Hybridverbindungs-Manager (HCM) zu Service Bus Relay. Der HCM ist ein Relaydienst, den Sie in dem Netzwerk bereitstellen, das die Ressource hostet, auf die Sie zugreifen möchten. 

Über die beiden verknüpften Verbindungen verfügt Ihre App über einen TCP-Tunnel zu einer festen Host:Port-Kombination auf der anderen Seite des HCM. Die Verbindung verwendet zum Schutz TLS 1.2 und zur Authentifizierung/Autorisierung SAS-Schlüssel.    

![Diagramm zum allgemeinen Ablauf bei Hybridverbindungen][1]

Wenn Ihre App eine DNS-Anforderung stellt, die mit einem konfigurierten Hybridverbindungsendpunkt übereinstimmt, wird der ausgehende TCP-Datenverkehr über die Hybridverbindung weitergeleitet.  

> [!NOTE]
> Sie sollten folglich versuchen, stets einen DNS-Namen für Ihre Hybridverbindung zu verwenden. Einige Clientsoftware führen keine DNS-Suche durch, wenn der Endpunkt stattdessen eine IP-Adresse verwendet.
>
>

Das Feature Hybrid Connections bietet zwei Typen: die neuen Hybrid Connections, die als Dienst im Rahmen von Azure Relay angeboten werden, und die älteren Hybrid Connections von BizTalk. Letztere werden im Portal als klassische Hybridverbindungen bezeichnet. Weiter unten in diesem Artikel finden Sie weitere Informationen hierzu.

### <a name="app-service-hybrid-connection-benefits"></a>Vorteile der App Service-Hybridverbindungen ###

Die Funktion für Hybridverbindungen bietet eine Reihe von Vorteilen wie etwa Folgende:

- Sie ermöglicht Apps den sicheren Zugriff auf lokale Systeme und Dienste.
- Für das Feature ist kein Endpunkt erforderlich, der über das Internet zugänglich ist.
- Sie lässt sich schnell und einfach einrichten. 
- Jede Hybridverbindung entspricht einer einzelnen Host-Port-Kombination, die auch einen Sicherheitsvorteil darstellt.
- Sie erfordert normalerweise keine Firewalllücken. Die Verbindungen sind alle ausgehend über Standardwebports.
- Da die Funktion auf Netzwerkebene ausgeführt wird, ist sie nicht von der Sprache, die von Ihrer App verwendet wird, und von der Technologie, die vom Endpunkt verwendet wird, abhängig.
- Sie kann verwendet werden, um über eine einzige App Zugriff in mehreren Netzwerken bereitzustellen. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Einschränkungen bei Hybridverbindungen ###

Bei Hybridverbindungen sind bestimmte Vorgänge und Funktionen nicht möglich. Hierzu zählt beispielsweise Folgendes:

- Einbindung eines Laufwerks
- Verwendung von UDP
- Zugriff auf TCP-basierte Dienste, die dynamische Ports verwenden, wie der passive Modus für FTP oder der erweiterte passive Modus
- Unterstützung von LDAP, da UDP manchmal erforderlich ist
- Unterstützung von Active Directory

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Hinzufügen und Erstellen von Hybridverbindungen in Ihrer App ##

Hybridverbindungen können über Ihre App Service-App im Azure-Portal oder über Azure Relay im Azure-Portal erstellt werden. Es wird empfohlen, Hybridverbindungen über die App Service-App zu erstellen, die Sie mit der Hybridverbindung verwenden möchten. Navigieren Sie zum Erstellen einer Hybridverbindung zum [Azure-Portal][portal], und wählen Sie Ihre App aus. Wählen Sie **Netzwerk** > **Endpunkte der Hybridverbindung konfigurieren** aus. In dieser Ansicht können Sie die Hybridverbindungen sehen, die für Ihre App konfiguriert sind.  

![Screenshot der Liste der Hybridverbindungen][2]

Um eine neue Hybridverbindung hinzuzufügen, wählen Sie **Hybridverbindung hinzufügen** aus.  Es wird eine Liste der Hybridverbindungen, die Sie bereits erstellt haben, angezeigt. Um Ihrer App eine oder mehrere dieser Hybridverbindungen hinzuzufügen, markieren Sie diese und wählen anschließend **Ausgewählte Hybridverbindung hinzufügen** aus.  

![Screenshot des Portals für Hybridverbindungen][3]

Wenn Sie eine neue Hybridverbindung erstellen möchten, wählen Sie **Neue Hybridverbindung erstellen** aus. Geben Sie Folgendes an: 

- Endpunktname
- Endpunkt-Hostname
- Endpunktport
- Zu verwendender Service Bus-Namespace

![Screenshot des Dialogfelds „Neue Hybridverbindung erstellen“][4]

Jede Hybridverbindung ist mit einem Service Bus-Namespace verbunden. Jeder Service Bus-Namespace befindet sich in einer Azure-Region. Es ist wichtig, einen Service Bus-Namespace in derselben Region wie Ihre App zu verwenden, um durch das Netzwerk verursachte Latenzen zu vermeiden.

Wenn Sie Ihre Hybridverbindung aus Ihrer App entfernen möchten, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Verbindung trennen** aus.  

Nachdem Ihrer App eine Hybridverbindung hinzugefügt wurde, können Sie die Details zu dieser anzeigen, indem Sie sie einfach auswählen. 

![Screenshot der Details von Hybridverbindungen][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Erstellen einer Hybridverbindung im Azure Relay-Portal ###

Sie können Hybridverbindungen nicht nur über das Portal Ihrer App erstellen, sondern auch über das Azure Relay-Portal. Damit eine Hybridverbindung von App Service verwendet werden kann, muss sie:

* eine Clientautorisierung erfordern
* ein Metadatenelement namens „endpoint“ aufweisen, das eine Host-Port-Kombination als Wert enthält

## <a name="hybrid-connections-and-app-service-plans"></a>Hybrid Connections und App Service-Pläne ##

Das Feature Hybrid Connections ist nur in den Preis-SKUs Basic, Standard, Premium und Isolated verfügbar. Es gelten Beschränkungen, die am Tarif geknüpft sind.  

> [!NOTE] 
> Sie können neue Hybridverbindungen nur basierend auf Azure-Relay erstellen. Sie können keine neuen BizTalk-Hybridverbindungen erstellen.
>

| Tarif | Anzahl der im Plan verwendbaren Hybridverbindungen |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Beachten Sie, dass der App Service-Plan auch zeigt, wie viele Hybridverbindungen verwendet werden und von welchen Apps.  

![Screenshot der Eigenschaften eines App Service-Plans][6]

Wählen Sie die Hybridverbindung aus, um Details anzuzeigen. Sie können alle Informationen sehen, die auch in der App-Ansicht angezeigt wurden. Außerdem können Sie sehen, wie viele andere Apps im gleichen Plan diese Hybridverbindung verwenden.

Es gilt eine Einschränkung für die Anzahl der Hybridverbindungs-Endpunkte, die in einem App Service-Plan verwendet werden können. Jede verwendete Hybridverbindung kann jedoch von einer beliebigen Anzahl von Apps in diesem Plan verwendet werden. Eine einzelne Hybridverbindung, die in fünf separaten Apps in einem App Service-Plan verwendet wird, zählt z.B. als eine Hybridverbindung.

Es fallen zusätzliche Kosten für die Verwendung von Hybridverbindungen. Weitere Informationen finden Sie unter [Service Bus – Preise][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

Damit das Feature Hybrid Connections funktioniert, ist ein Relay-Agent in dem Netzwerk erforderlich, das Ihren Hybridverbindungs-Endpunkt hostet. Dieser Relay-Agent wird als Hybrid Connection Manager (HCM) bezeichnet. Zum Herunterladen von HCM wählen Sie in Ihrer App im [Azure-Portal][portal] die Option **Netzwerk** > **Endpunkte der Hybridverbindung konfigurieren** aus.  

Dieses Tool wird unter Windows Server 2012 und höher ausgeführt. Nach der Installation wird HCM als Dienst ausgeführt, der basierend auf den konfigurierten Endpunkten eine Verbindung mit Azure Service Bus Relay herstellt. Bei den Verbindungen vom HCM handelt es sich um ausgehende Verbindungen mit Azure über Port 443.    

Sie können nach der Installation von HCM die Datei „HybridConnectionManagerUi.exe“ ausführen, um die Benutzeroberfläche für das Tool zu verwenden. Diese Datei befindet sich im Installationsverzeichnis des Hybridverbindungs-Managers. Unter Windows 10 können Sie auch einfach im Suchfeld nach *Hybridverbindungs-Manager-Benutzeroberfläche* suchen.  

![Screenshot von Hybridverbindungs-Manager][7]

Wenn Sie die Benutzeroberfläche des HCM starten, wird als Erstes eine Tabelle angezeigt, in der alle Hybridverbindungen aufgeführt werden, die mit dieser Instanz des HCM konfiguriert sind. Wenn Sie Änderungen vornehmen möchten, müssen Sie sich zunächst bei Azure authentifizieren. 

So fügen Sie Ihrem HCM eine oder mehrere Hybridverbindungen hinzu:

1. Starten Sie die Benutzeroberfläche des HCM.
1. Wählen Sie **Andere Hybridverbindung konfigurieren** aus.
![Screenshot der Konfiguration neuer Hybridverbindungen][8]

1. Melden Sie sich mit Ihrem Azure-Konto an.
1. Wählen Sie ein Abonnement aus.
1. Wählen Sie die Hybridverbindungen aus, die der HCM weiterleiten soll.
![Screenshot von Hybridverbindungen][9]

1. Wählen Sie **Speichern** aus.

Es werden nun die Hybridverbindungen angezeigt, die Sie hinzugefügt haben. Sie können die konfigurierte Hybridverbindung auch auswählen, um Details anzuzeigen.

![Screenshot von Details einer Hybridverbindung][10]

Damit der HCM die bei ihm konfigurierten Hybridverbindungen unterstützen kann, ist Folgendes erforderlich:

- TCP-Zugriff auf Azure über die Ports 80 und 443
- TCP-Zugriff auf den Hybridverbindungs-Endpunkt
- Möglichkeit für DNS-Suchvorgänge auf dem Endpunkthost und im Service Bus-Namespace

HCM unterstützt sowohl neue Hybridverbindungen als auch die älteren BizTalk-Hybridverbindungen.

> [!NOTE]
> Azure-Relay nutzt Websockets für die Konnektivität. Diese Funktion ist nur für Windows Server 2012 oder höher verfügbar. Aus diesem Grund wird der HCM von Versionen vor Windows Server 2012 nicht unterstützt.
>

### <a name="redundancy"></a>Redundanz ###

Jeder HCM kann mehrere Hybridverbindungen unterstützen. Darüber hinaus kann eine bestimmte Hybridverbindung durch mehrere HCMs unterstützt werden. Das Standardverhalten ist ein Weiterleiten von Datenverkehr über die konfigurierten HCMs für einen bestimmten Endpunkt. Wenn Sie Hochverfügbarkeit für Hybridverbindungen über Ihr Netzwerk wünschen, führen Sie mehrere HCMs auf unterschiedlichen Computern aus. 

### <a name="manually-add-a-hybrid-connection"></a>Manuelles Hinzufügen einer Hybridverbindung ###

Damit ein Benutzer außerhalb Ihres Abonnements eine HCM-Instanz für eine bestimmte Hybridverbindung hosten kann, geben Sie für diesen die Gateway-Verbindungszeichenfolge für die Hybridverbindung frei. Diese können Sie in den Eigenschaften einer Hybridverbindung im [Azure-Portal][portal] sehen. Um diese Zeichenfolge zu verwenden, wählen Sie im HCM **Manuell eingeben** aus, und fügen Sie die Gateway-Verbindungszeichenfolge ein.


## <a name="troubleshooting"></a>Problembehandlung ##

Der Status „Verbunden“ bedeutet, dass mindestens ein HCM mit dieser Hybridverbindung konfiguriert ist und Azure erreichen kann. Wenn der Status für die Hybridverbindung nicht **Verbunden** ist, wurde die Hybridverbindung nicht auf einem HCM konfiguriert, der auf Azure zugreifen kann.

Der Hauptgrund, warum Clients keine Verbindung mit dem jeweiligen Endpunkt herstellen können, ist, dass der Endpunkt mit einer IP-Adresse anstelle eines DNS-Namens angegeben wurde. Wenn Ihre App den gewünschten Endpunkt nicht erreichen kann und Sie eine IP-Adresse verwendet haben, gehen Sie zur Verwendung eines DNS-Namens über, der auf dem Host, auf dem der HCM ausgeführt wird, gültig ist. Überprüfen Sie außerdem, ob der DNS-Name auf dem Host, auf dem der HCM ausgeführt wird, ordnungsgemäß aufgelöst wird. Vergewissern Sie sich, dass zwischen dem Host, auf dem der HCM ausgeführt wird, und dem Hybridverbindungs-Endpunkt Konnektivität besteht.  

In App Service kann das Tool tcpping über die Konsole „Erweiterte Tools“ (Kudu) aufgerufen werden. Mit diesem Tool können Sie feststellen, ob Sie Zugriff auf einen TCP-Endpunkt haben, jedoch nicht, ob Sie Zugriff auf einen Hybridverbindungsendpunkt haben. Wenn Sie das Tool an der Konsole für einen Hybridverbindungs-Endpunkt verwenden, bestätigen Sie nur, dass eine Host-Port-Kombination verwendet wird.  

## <a name="biztalk-hybrid-connections"></a>BizTalk-Hybridverbindungen ##

Die ältere Funktion der BizTalk-Hybridverbindungen wurde für neue BizTalk-Hybridverbindungen gesperrt. Sie können weiterhin Ihre vorhandenen BizTalk-Hybridverbindungen mit Ihren Apps verwenden, sollten sie jedoch zu neuen Hybridverbindungen migrieren, die Azure Relay nutzen. Der neue Dienst bringt gegenüber der BizTalk-Version folgende Vorteile mit sich:

- Kein zusätzliches BizTalk-Konto ist erforderlich.
- Bei TLS wird Version 1.2 anstelle von Version 1.0 verwendet.
- Die Kommunikation erfolgt über die Ports 80 und 443, um Azure mithilfe eines DNS-Namens anstelle von IP-Adressen und einer Reihe von zusätzlichen Ports zu erreichen.  

Um Ihrer App eine vorhandene BizTalk-Hybridverbindung hinzuzufügen, navigieren Sie im [Azure-Portal][portal] zu Ihrer App, und wählen Sie **Netzwerk** > **Endpunkte der Hybridverbindung konfigurieren** aus. Wählen Sie in der Tabelle „Klassische Hybridverbindungen“ die Option **Klassische Hybridverbindung hinzufügen** aus. Sie sehen eine Liste Ihrer BizTalk-Hybridverbindungen.  


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
