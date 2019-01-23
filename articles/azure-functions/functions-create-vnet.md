---
title: Integrieren von Azure Functions in ein Azure Virtual Network
description: Eine schrittweises Tutorial, das Ihnen zeigt, wie Sie eine Funktion mit einem virtuellen Azure-Netzwerk herstellen.
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 12/03/2018
ms.author: alkarche
ms.openlocfilehash: 1140c23a0aa5344119c35434316ec73cc3918f90
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198370"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Integrieren einer Funktions-App in ein Azure Virtual Network
Dieses schrittweise Tutorial zeigt Ihnen, wie Sie Azure Functions verwenden, um eine Verbindung mit Ressourcen in einem Azure VNET herzustellen. 

In diesem Tutorial stellen wir eine Wordpress-Website auf einem virtuellen Computer in einem privaten VNET ohne Internetzugriff bereit. Wir stellen dann eine Funktion bereit, die Zugriff auf beides hat, das Internet und das VNET. Wir verwenden diese Funktion, um auf Ressourcen der Wordpress-Website zuzugreifen, die innerhalb des VNET bereitgestellt ist.

Weitere Informationen dazu, wie das System funktioniert, zur Problembehandlung und erweiterten Konfiguration finden Sie im Dokument [Integrieren Ihrer App in ein Azure Virtual Network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). Azure Functions in dem dedizierten Plan besitzt dieselben Hostingfunktionen wie Web-Apps, sodass alle in diesem Dokument genannten Funktionalitäten und Einschränkungen ebenfalls für Azure Functions gelten.

## <a name="topology"></a>Topologie
 ![Benutzeroberfläche der VNET-Integration][1]

## <a name="creating-a-vm-inside-of-a-vnet"></a>Erstellen eines virtuellen Computers in einem VNET

Zu Beginn erstellen wir eine vorkonfigurierte VM, auf der Wordpress in einem VNET ausgeführt wird. 

Wordpress auf einem virtuellen Computer wurde gewählt, weil es eine der kostengünstigsten Ressourcen ist, die in einem VNET bereitgestellt werden kann. Beachten Sie, dass dieses Szenario auch mit allen anderen Ressourcen in einem VNET arbeiten kann, einschließlich REST-APIs, anderer Azure-Dienste, App Service-Umgebungen usw.

1.  Wechseln Sie zum Azure-Portal.
2.  Fügen Sie eine neue Ressource hinzu, indem Sie das Blatt „Ressource erstellen“ öffnen.
3.  Suchen Sie nach „[Wordpress LEMP7 Max Performance on CentOS](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure)“, und öffnen Sie das Blatt für die Erstellung. 
4.  Konfigurieren Sie auf dem Blatt für die Erstellung die VM mit den folgenden Informationen:
    1.  Erstellen Sie eine neue Ressourcengruppe für diesen virtuellen Computer, um die Bereinigung der Ressourcen am Ende des Tutorials zu vereinfachen. Ich habe meine Ressourcengruppe „Function-VNET-Tutorial“ genannt.
    1.  Geben Sie dem virtuellen Computer einen eindeutigen Namen. Ich habe meinen „VNET-Wordpress“ genannt.
    1.  Wählen Sie die Ihnen am nächsten gelegene Region aus.
    1.  Wählen Sie die Größe als B1s (1 vCPU und 1 GB Arbeitsspeicher).
    1.  Wählen Sie für das Administratorkonto Kennwortauthentifizierung aus, und geben Sie einen eindeutigen Benutzernamen und ein Kennwort ein. Für dieses Tutorial müssen Sie sich nur bei dem virtuellen Computer anmelden, wenn Sie Probleme behandeln müssen.
    
        <img src="./media/functions-create-vnet/create-VM-1.png" width="700">

1. Wechseln Sie zur Registerkarte „Netzwerk“, und geben Sie Folgendes ein:
    1.  Erstellen eines neuen virtuellen Netzwerks
    1.  Geben Sie in Ihren gewünschten privaten Adressraum und das Subnetz innerhalb dieses Adressbereichs ein. Die Größe des Subnetzes bestimmt, wie viele virtuelle Computer im App Service-Plan verwendet werden können. Wenn Sie sich mit IP-Adressenvergabe und dem Erstellen von Subnetzen nicht auskennen, gibt es ein [Dokument, das die Grundlagen behandelt](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). IP-Adressenvergabe und Subnetze sind wichtig in diesem Szenario, weshalb ich empfehlen würde, einige Artikel dazu zu lesen und online ein paar Videos anzuschauen, bis Sie ein grundlegendes Verständnis entwickelt haben. 
        1. Für dieses Beispiel habe ich mich entschieden, das Netzwerk „10.10.0.0/16“ mit dem Subnetz „10.10.1.0/24“ zu verwenden. Ich habe mich entschlossen, großzügig bereitzustellen, und verwende ein „/16“-Subnetz, weil sich einfach berechnen lässt, welche Subnetze im Netzwerk „10.10.0.0/16“ verfügbar sind.
        
        <img src="./media/functions-create-vnet/create-VM-2.png" width="700">

1. Zurück auf der Registerkarte „Netzwerk“ legen Sie die öffentliche IP auf „Keine“ fest. Dadurch wird der virtuelle Computer so bereitgestellt, dass er nur Zugriff auf das VNET hat.
       
    <img src="./media/functions-create-vnet/create-VM-2.1.png" width="700">

7. Erstellen Sie den virtuellen Computer. Dies dauert ca. 5 Minuten.
8. Sobald der virtuelle Computer erstellt ist, besuchen Sie seine Registerkarte „Netzwerk“, und notieren Sie sich die private IP-Adresse zur späteren Verwendung. Der virtuelle Computer sollte keine öffentliche IP besitzen.

    ![14]

Sie verfügen jetzt über eine Wordpress-Website, die ausschließlich in Ihrem virtuellen Netzwerk bereitgestellt ist. Auf diese Site kann nicht aus dem öffentlichen Internet zugegriffen werden.

## <a name="create-a-dedicated-function-app"></a>Erstellen einer dedizierten Funktions-App

Der nächste Schritt besteht darin, eine Funktions-App in einem Standard- oder höheren App Service-Plan zu erstellen. Beachten Sie, dass Funktions-Apps eines Verbrauchstarifs keine VNET-Integration unterstützen.

1. Wechseln Sie zum Azure-Portal.
2. Fügen Sie eine neue Ressource hinzu, indem Sie das Blatt „Ressource erstellen“ öffnen.
3. Wählen Sie „Serverlose Funktions-App“ aus.
4. Geben Sie alle Ihre normalen Informationen auf dem Blatt zur Erstellung ein, bis auf eine Ausnahme:
    1. Wählen Sie einen Standard- oder höheren App Service-Plan aus.

        <img src="./media/functions-create-vnet/Create-App-Service-Plan.PNG" width="300">
    
1. Mein fertiges Blatt zur Erstellung sieht wie folgt aus.

    <img src="./media/functions-create-vnet/Create-Function-App.png" width="300">


## <a name="connect-your-function-app-to-your-vnet"></a>Verbinden Ihrer Funktions-App mit Ihrem VNET

Nachdem wir nun eine Wordpress-Website haben, die viele Dateien aus einem VNET enthält, müssen wir die Funktions-App mit diesem VNET verbinden.

1.  Wählen Sie im Portal für die Funktions-App aus dem vorherigen Schritt **Plattformfeatures** und dann **Netzwerk** aus.
1.  Wählen Sie unter „VNET-Integration“ die Option **Zum Konfigurieren klicken** aus.

    <img src="./media/functions-create-vnet/Networking-1.png" width="450">

1. Wählen Sie im VNET-Integrationsbereich **VNet hinzufügen (Vorschau)** aus.

    <img src="./media/functions-create-vnet/Networking-2.png" width="600"> 
    
1.  Erstellen Sie ein neues Subnetz für Ihre Funktion und den zu verwendenden App Service-Plan. Beachten Sie, dass die Größe des Subnetzes die Gesamtzahl der VMs einschränkt, die Sie Ihrem App Service-Plan hinzufügen können. Ihr VNET leitet Datenverkehr automatisch zwischen den Subnetzen in Ihrem VNET weiter, weshalb es egal ist, dass sich Ihre Funktion in einem anderen Subnetz als Ihre VM befindet. 
    
    <img src="./media/functions-create-vnet/Networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-vnet"></a>Erstellen einer Funktion, die auf eine Ressource in Ihrem VNET zugreift

Die Funktions-App kann jetzt auf das VNET mit unserer Wordpress-Website zugreifen, weshalb wir die Funktion verwenden werden, um auf diese Datei zuzugreifen und sie wiederum dem Benutzer bereitzustellen. In diesem Beispiel verwenden eine Wordpress-Website als API und einen Funktionsproxy als aufrufende Funktion, weil sich beide leicht einrichten und visualisieren lassen. Genau so einfach könnten Sie jede andere API verwenden, die innerhalb eines VNET bereitgestellt ist, sowie eine andere Funktion mit Code, der API-Aufrufe an die in Ihrem VNET bereitgestellte API ausführt. Ein in Ihrem VNET bereitgestellter SQL-Server ist ein perfektes Beispiel.

1. Öffnen Sie im Portal die Funktions-App aus dem vorherigen Schritt.
1. Erstellen Sie einen Proxy, indem Sie **Proxys** > **+** auswählen.

    <img src="./media/functions-create-vnet/New-Proxy.png" width="250">

1. Konfigurieren Sie den Namen und die Route des Proxys. Ich habe „/plant“ als meine Route ausgewählt.
1. Tragen Sie Ihre Wordpress-Website-IP von vorher ein, und legen Sie die Back-End-URL auf `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg` fest.
    
    <img src="./media/functions-create-vnet/Create-Proxy.png" width="900">

Wenn Sie nun versuchen, Ihre Back-End-URL direkt zu besuchen, indem Sie sie in eine neue Browserregisterkarte einfügen, sollte es zu einem Timeout der Seite kommen. Dies ist zu erwarten, da Ihre Wordpress-Website nur mit Ihrem VNET, und nicht mit dem Internet verbunden ist. Wenn Sie Ihre Webproxy-URL in den Browser einfügen, sollte ein ansprechende Pflanzenbild angezeigt werden, das mithilfe von Pull aus Ihrer Wordpress-Website in Ihr VNET abgerufen wird. 

Ihre Funktions-App ist sowohl mit dem Internet als auch mit Ihrem VNET verbunden. Der Proxy empfängt einer Anforderung über das öffentliche Internet, und leitet diese Anforderung dann, indem er als einfacher HTTP-Proxy fungiert, weiter in das virtuelle Netzwerk hinein. Der Proxy leitet dann die Antwort über das öffentliche Internet zurück an Sie weiter. 

<img src="./media/functions-create-vnet/Plant.png" width="900">

## <a name="next-steps"></a>Nächste Schritte

Azure Functions, die in App Service-Plänen ausgeführt werden, werden im selben Dienst wie Web-Apps ausgeführt, sodass die gesamte Dokumentation für Web-Apps ebenfalls für dedizierte Funktionen gilt.

1. [Weitere Informationen zur VNET-Integration mit App Service/Functions finden Sie hier](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
1. [Weitere Informationen zu VNETs in Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)
1. [Aktivieren von Netzwerkfeatures und Steuerung mit App Service-Umgebungen](https://docs.microsoft.com/azure/app-service/environment/intro)
1. [Verbinden mit einzelnen lokalen Ressourcen ohne Änderungen an der Firewall mithilfe von Hybrid Connections](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)
1. [Weitere Informationen zu Funktionsproxys](https://review.docs.microsoft.com/azure/azure-functions/functions-proxies)

<!--Image references-->
[1]: ./media/functions-create-vnet/topology.png
[2]: ./media/functions-create-vnet/Create-Function-App.png
[3]: ./media/functions-create-vnet/Create-App-Service-Plan.PNG
[4]: ./media/functions-create-vnet/configure-VNET.png
[5]: ./media/functions-create-vnet/create-VM-1.png
[6]: ./media/functions-create-vnet/create-VM-2.png
[7]: ./media/functions-create-vnet/create-VM-2.1.png
[8]: ./media/functions-create-vnet/Networking-1.png
[9]: ./media/functions-create-vnet/Networking-2.png
[10]: ./media/functions-create-vnet/Networking-3.png
[11]: ./media/functions-create-vnet/New-Proxy.png
[12]: ./media/functions-create-vnet/Create-Proxy.png
[14]: ./media/functions-create-vnet/VM-Networking.png
