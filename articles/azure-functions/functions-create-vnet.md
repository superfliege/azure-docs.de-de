---
title: Integrieren von Azure Functions in ein virtuelles Azure-Netzwerk
description: Ein Tutorial in einzelnen Schritten, in dem erläutert wird, wie Sie eine Funktion mit einem virtuellen Azure-Netzwerk verbinden.
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 96ab479d3373eb6e575a00898f7007a4df252e39
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "60002788"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Integrieren einer Funktions-App in ein virtuelles Azure-Netzwerk

In diesem ausführlichen Tutorial wird erläutert, wie Sie mithilfe von Azure Functions eine Verbindung mit Ressourcen in einem virtuellen Azure-Netzwerk herstellen.

In diesem Tutorial stellen wir eine WordPress-Website auf einem virtuellen Computer in einem virtuellen Netzwerk bereit, das nicht über das Internet zugänglich ist. Wir stellen dann eine Funktion mit Zugriff auf das Internet sowie auf das virtuelle Netzwerk bereit. Wir verwenden diese Funktion, um auf Ressourcen der WordPress-Website zuzugreifen, die innerhalb des virtuellen Netzwerks bereitgestellt ist.

Weitere Informationen zur Funktionsweise des Systems, zur Problembehandlung und erweiterten Konfiguration finden Sie unter [Integrieren Ihrer App in ein Azure Virtual Network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). Azure Functions im Premium-Tarif hat dieselben Hostingfunktionen wie Web-Apps, sodass alle in diesem Artikel genannten Funktionen und Einschränkungen ebenfalls für Azure Functions gelten.

## <a name="topology"></a>Topologie

 ![Benutzeroberfläche für die Integration des virtuellen Netzwerks][1]

## <a name="create-a-vm-inside-a-virtual-network"></a>Erstellen eines virtuellen Computers in einem virtuellen Netzwerk

Zunächst erstellen wir einen vorkonfigurierten virtuellen Computer, auf dem WordPress in einem virtuellen Netzwerk ausgeführt wird. 

WordPress auf einem virtuellen Computer wurde ausgewählt, weil es eine der kostengünstigsten Ressourcen ist, die in einem virtuellen Netzwerk bereitgestellt werden kann. Beachten Sie, dass dieses Szenario auch mit einer Ressource in einem virtuellen Netzwerk verwendet werden kann, z. B. REST-APIs, App Service-Umgebungen und andere Azure-Dienste.

1. Öffnen Sie das Azure-Portal.
2. Fügen Sie eine neue Ressource hinzu, indem Sie das Blatt **Ressource erstellen** öffnen.
3. Suchen Sie nach [WordPress LEMP7 Max Performance on CentOS](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure), und öffnen Sie das Blatt für die Erstellung. 
4. Konfigurieren Sie auf der Registerkarte **Grundeinstellungen** den virtuellen Computer mit den folgenden Angaben:
    1. Erstellen Sie eine neue Ressourcengruppe für diesen virtuellen Computer, um die Bereinigung der Ressourcen am Ende des Tutorials zu vereinfachen. Hier wird „Function-VNET-Tutorial“ als Beispiel verwendet.
    1. Geben Sie dem virtuellen Computer einen eindeutigen Namen. Als Beispiel wird „VNET-Wordpress“ verwendet.
    1. Wählen Sie die Ihnen am nächsten gelegene Region aus.
    1. Wählen Sie die Größe als B1s (1 vCPU und 1 GB Arbeitsspeicher) aus.
    1. Wählen Sie für das Administratorkonto Kennwortauthentifizierung aus, und geben Sie einen eindeutigen Benutzernamen und ein Kennwort ein. Für dieses Tutorial müssen Sie sich nur bei dem virtuellen Computer anmelden, wenn Sie Probleme beheben müssen.
    
        ![Registerkarte „Grundeinstellungen“ zum Erstellen eines virtuellen Computers](./media/functions-create-vnet/create-vm-1.png)

1. Wechseln Sie zur Registerkarte **Netzwerk**, und geben Sie Folgendes ein:
    1.  Erstellen Sie ein neues virtuelles Netzwerk.
    1.  Geben Sie einen privaten Adressbereich und ein Subnetz innerhalb dieses Adressbereichs ein. Die Größe des Subnetzes bestimmt, wie viele virtuelle Computer im App Service-Plan verwendet werden können. Wenn Sie mit IP-Adressenvergabe und dem Erstellen von Subnetzen nicht vertraut sind, finden Sie die entsprechenden Grundlagen in [diesem Dokument](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). IP-Adressenvergabe und Subnetze sind wichtig in diesem Szenario, daher wird empfohlen, einige Artikel dazu zu lesen und online einige Videos anzusehen, bis Sie ein grundlegendes Verständnis entwickelt haben. 
    
        Für dieses Beispiel wird das Netzwerk „10.10.0.0/16“ mit dem Subnetz „10.10.1.0/24“ verwendet. Es wird eine Überbereitstellung mit einem „/16“-Subnetz verwendet, da sich einfach berechnen lässt, welche Subnetze im Netzwerk „10.10.0.0/16“ verfügbar sind.
        
        <img src="./media/functions-create-vnet/create-vm-2.png" width="700">

1. Legen Sie auf der Registerkarte **Netzwerk** die öffentliche IP-Adresse auf **Keine** fest. Mit diesem Schritt wird der virtuelle Computer mit ausschließlichem Zugriff auf das virtuelle Netzwerk bereitgestellt.
       
    <img src="./media/functions-create-vnet/create-vm-2-1.png" width="700">

7. Erstellen Sie den virtuellen Computer. Der Vorgang dauert ungefähr 5 Minuten.
8. Navigieren Sie nach der Erstellung des virtuellen Computers zur zugehörigen Registerkarte **Netzwerk**, und notieren Sie sich die private IP-Adresse zur späteren Verwendung. Der virtuelle Computer sollte keine öffentliche IP besitzen.

    ![14]

Damit haben Sie eine WordPress-Website eingerichtet, die ausschließlich in Ihrem virtuellen Netzwerk bereitgestellt ist. Auf diese Site kann nicht aus dem öffentlichen Internet zugegriffen werden.

## <a name="create-a-function-app-in-a-premium-plan"></a>Erstellen einer Funktions-App in einem Premium-Tarif

Im nächsten Schritt wird eine Funktions-App in einem Premium-Tarif erstellt. Der Premium-Tarif bietet serverlose Skalierung mit allen Vorteilen eines dedizierten App Service-Plans. Funktions-Apps, die unter dem Verbrauchstarif erstellt werden, unterstützen die Integration des virtuellen Netzwerks nicht.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

## <a name="connect-your-function-app-to-your-virtual-network"></a>Verbinden Ihrer Funktions-App mit Ihrem virtuellen Netzwerk

Mit einer WordPress-Website, in der Dateien aus Ihrem virtuellen Netzwerk gehostet werden, können Sie nun die Funktions-App mit dem virtuellen Netzwerk verbinden.

1.  Wählen Sie im Portal für die Funktions-App aus dem vorherigen Schritt die Option **Plattformfeatures** aus. Wählen Sie dann **Netzwerk** aus.

    <img src="./media/functions-create-vnet/networking-0.png" width="850">

1.  Wählen Sie unter **VNET-Integration** die Option **Zum Konfigurieren hier klicken** aus.

    ![Status für das Konfigurieren einer Netzwerkfunktion](./media/functions-create-vnet/Networking-1.png)

1. Wählen Sie auf der Seite zur Integration des virtuellen Netzwerks die Option **VNET hinzufügen (Vorschau)** aus.

    <img src="./media/functions-create-vnet/networking-2.png" width="600"> 
    
1.  Erstellen Sie ein neues Subnetz für Ihre Funktion und den zu verwendenden App Service-Plan. Beachten Sie, dass die Größe des Subnetzes die Gesamtanzahl der virtuellen Computer einschränkt, die Sie dem App Service-Plan hinzufügen können. In Ihrem virtuellen Netzwerk wird Datenverkehr automatisch zwischen den Subnetzen weitergeleitet, sodass es keine Rolle spielt, dass sich die Funktion in einem anderen Subnetz als der virtuelle Computer befindet. 
    
    <img src="./media/functions-create-vnet/networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-virtual-network"></a>Erstellen einer Funktion mit Zugriff auf eine Ressource im virtuellen Netzwerk

Die Funktions-App hat nun Zugriff auf das virtuelle Netzwerk mit der WordPress-Website. Mithilfe der Funktion greifen wir nun auf diese Datei zu und stellen sie für den Benutzer bereit. In diesem Beispiel werden eine WordPress-Website als API und ein Proxy als aufrufende Funktion verwendet. Beide lassen sich einfach einrichten und visualisieren. 

Genau so einfach können Sie jede andere API verwenden, die in einem virtuellen Netzwerk bereitgestellt ist. Auch können Sie eine andere Funktion mit Code verwenden, über den API-Aufrufe an die in Ihrem virtuellen Netzwerk bereitgestellte API ausgeführt werden. Ein Beispiel dafür ist eine SQL Server-Instanz, die in Ihrem virtuellen Netzwerk bereitgestellt ist.

1. Öffnen Sie im Portal die Funktions-App aus dem vorherigen Schritt.
1. Erstellen Sie einen Proxy, indem Sie **Proxys** > **+** auswählen.

    <img src="./media/functions-create-vnet/new-proxy.png" width="250">

1. Konfigurieren Sie den Namen und die Route des Proxys. In diesem Beispiel wird „/plant“ als Route verwendet.
1. Geben Sie die zuvor notierte IP der WordPress-Website ein, und legen Sie die **Back-End-URL** auf `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg` fest.
    
    <img src="./media/functions-create-vnet/create-proxy.png" width="900">

Wenn Sie nun versuchen, die Back-End-URL direkt aufzurufen, indem Sie sie in einer neuen Browserregisterkarte einfügen, sollte ein Timeout der Seite erfolgen. Dies ist darauf zurückzuführen, dass die WordPress-Website nur mit Ihrem virtuellen Netzwerk und nicht mit dem Internet verbunden ist. Wenn Sie die Proxy-URL im Browser einfügen, sollte ein (per Pull aus der WordPress-Website übertragenes) Pflanzenbild im virtuellen Netzwerk angezeigt werden. 

Die Funktions-App ist sowohl mit dem Internet als auch mit dem virtuellen Netzwerk verbunden. Der Proxy empfängt einer Anforderung über das öffentliche Internet, und leitet diese Anforderung dann, indem er als einfacher HTTP-Proxy fungiert, weiter in das virtuelle Netzwerk hinein. Der Proxy leitet dann die Antwort über das öffentliche Internet zurück an Sie weiter. 

<img src="./media/functions-create-vnet/plant.png" width="900">

## <a name="next-steps"></a>Nächste Schritte

Funktionen, die in einem Premium-Tarif ausgeführt werden, haben dieselbe zugrunde liegende App Service-Infrastruktur wie Web-Apps in PremiumV2-Tarifen. Die gesamte Dokumentation für Web-Apps gilt daher auch für Funktionen mit Premium-Tarif.

* [Weitere Informationen zu den Netzwerkoptionen in Functions](./functions-networking-options.md)
* [Häufig gestellte Fragen zu Netzwerken in Functions](./functions-networking-faq.md)
* [Weitere Informationen zu virtuellen Netzwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Aktivieren weiterer Netzwerkfunktionen und Steuerung mit App Service-Umgebungen](../app-service/environment/intro.md)
* [Verbinden mit einzelnen lokalen Ressourcen ohne Änderungen an der Firewall mithilfe von Hybrid Connections](../app-service/app-service-hybrid-connections.md)
* [Weitere Informationen zu Functions-Proxys](./functions-proxies.md)

<!--Image references -->
[1]: ./media/functions-create-vnet/topology.png
[2]: ./media/functions-create-vnet/create-function-app.png
[3]: ./media/functions-create-vnet/create-app-service-plan.png
[4]: ./media/functions-create-vnet/configure-vnet.png
[5]: ./media/functions-create-vnet/create-vm-1.png
[6]: ./media/functions-create-vnet/create-vm-2.png
[7]: ./media/functions-create-vnet/create-vm-2-1.png
[8]: ./media/functions-create-vnet/networking-1.png
[9]: ./media/functions-create-vnet/networking-2.png
[10]: ./media/functions-create-vnet/networking-3.png
[11]: ./media/functions-create-vnet/new-proxy.png
[12]: ./media/functions-create-vnet/create-proxy.png
[14]: ./media/functions-create-vnet/vm-networking.png
