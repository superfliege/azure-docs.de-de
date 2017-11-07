---
title: Integrieren Ihrer ILB-ASE mit einem Azure Application Gateway
description: Exemplarische Vorgehensweise zum Integrieren einer App in Ihrer ILB-ASE mit Ihrem Azure Application Gateway
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a6a74f17-bb57-40dd-8113-a20b50ba3050
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: ccompy
ms.openlocfilehash: eedad8824add7fe425d34975dab640fbee82c2bc
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2017
---
# <a name="integrating-your-ilb-ase-with-an-application-gateway"></a>Integrieren Ihrer ILB-ASE mit einem Application Gateway #

Die [Azure App Service-Umgebung (ASE)](./intro.md) ist eine Bereitstellung des Azure App Service in einem Subnetz im Azure Virtual Network eines Kunden. Sie kann für den App-Zugriff über einen öffentlichen oder privaten Endpunkt bereitgestellt werden. Die Bereitstellung der ASE mit einem privaten Endpunkt wird als ILB-ASE bezeichnet.  
Azure Application Gateway ist eine virtuelle Anwendung, die den Lastenausgleich der Ebene 7, SSL-Abladung und WAF-Schutz bietet. Es kann eine öffentliche IP-Adresse überwachen und Datenverkehr an Ihren Anwendungsendpunkt routen. Die folgenden Informationen beschreiben die Vorgehensweise beim Integrieren eines WAF-konfigurierten Application Gateways mit einer App in einer ILB-ASE.  

Die Integration des Application Gateways mit der ILB-ASE erfolgt auf App-Ebene.  Wenn Sie das Application Gateway mit der ILB-ASE konfigurieren, erfolgt dies für bestimmte Apps in Ihrer ILB-ASE. Dadurch wird das Hosten sicherer mehrinstanzenfähiger Anwendungen in einer einzelnen ILB-ASE ermöglicht.  

![Application Gateway, das auf eine App in einer ILB-ASE verweist][1]

In dieser exemplarischen Vorgehensweise führen Sie folgende Aktionen aus:

* Erstellen eines Application Gateways
* Konfigurieren des Application Gateways zum Verweisen auf eine App in Ihrer ILB-ASE
* Konfigurieren Ihrer App, sodass der benutzerdefinierte Domänenname anerkannt wird
* Bearbeiten des öffentlichen DNS-Hostnamens, der auf Ihr Application Gateway verweist

Sie benötigen Folgendes, um Ihr Application Gateway mit der ILB-ASE zu integrieren:

* Eine ILB-ASE
* Eine App, die in der ILB-ASE ausgeführt wird
* Ein über das Internet routingfähiger Domänenname, der mit Ihrer App in der ILB-ASE verwendet wird
* Die von Ihrer ILB-ASE verwendete ILB-Adresse (Diese befindet sich im ASE-Portal unter **Einstellungen -> IP-Adressen**)

    ![Von Ihrer ILB-ASE verwendete IP-Adressen][9]
    
* Einen öffentlichen DNS-Namen, der später verwendet wird, um auf Ihr Application Gateway zu verweisen 

Informationen zum Erstellen einer ILB-ASE finden Sie im Dokument [Erstellen und Verwenden einer ILB-ASE][ilbase].

In dieser Anleitung wird davon ausgegangen, dass Sie ein Application Gateway in demselben Azure Virtual Network integrieren möchten, in dem die ASE bereitgestellt wird. Wählen oder erstellen Sie vor der Erstellung des Application Gateways ein Subnetz, das Sie als Host für das Application Gateway verwenden werden. Sie sollten ein anderes Subnetz als „GatewaySubnet“ oder das in der ILB-ASE verwendete Subnetz verwenden.
Wenn Sie das Application Gateway in „GatewaySubnet“ ablegen, können Sie später kein Virtual Network-Gateway erstellen. Außerdem können Sie es nicht in einem Subnetz ablegen, das von Ihrer ILB-ASE verwendet wird, da sich in ihrem Subnetz nur die ASE befinden darf.

## <a name="steps-to-configure"></a>Schritte zum Konfigurieren ##

1. Wechseln Sie im Azure-Portal zu **Neu > Netzwerk > Application Gateway**. 
    1. Stellen Sie Folgendes bereit:
        1. Name des Application Gateways
        1. WAF auswählen
        1. Dasselbe Abonnement wie für das ASE VNet auswählen
        1. Ressourcengruppe erstellen oder auswählen
        1. Speicherort für das ASE VNet auswählen

    ![Grundlagen zur Erstellung eines neuen Application Gateways][2]   
    1. Legen Sie im Einstellungsbereich Folgendes fest:
        1. Das ASE VNet
        1. Das Subnetz, in dem das Application Gateway bereitgestellt werden muss. Verwenden Sie nicht „GatewaySubnet“, da es die Erstellung von VPN-Gateways verhindert.
        1. „Öffentlich“ auswählen
        1. Wählen Sie eine öffentliche IP-Adresse aus. Wenn keine verfügbar ist, erstellen Sie eine IP-Adresse zu diesem Zeitpunkt.
        1. Konfigurieren Sie für HTTP oder HTTPS. Bei der Konfiguration für HTTPS müssen Sie ein PFX-Zertifikat bereitstellen.
        1. Wählen Sie die Web Application Firewall-Einstellungen aus. Hier können Sie die Firewall aktivieren und nach Bedarf entweder die Erkennung oder die Prävention für die Firewall festlegen.

    ![Einstellungen zur Erstellung eines neuen Application Gateways][3]
    
    1. Wählen Sie bei der Überprüfung des Abschnitts „Zusammenfassung“ die Option **OK** aus. Es kann etwas mehr als 30 Minuten dauern, bis Setup für Ihr Application Gateway abgeschlossen ist.  

2. Nachdem Ihr Application Gateway eingerichtet wurde, wechseln Sie zum Application Gateway-Portal. Wählen Sie **Back-End-Pool** aus.  Fügen Sie die ILB-Adresse für Ihre ILB-ASE hinzu.

    ![Konfigurieren des Back-End-Pools][4]

3. Wählen Sie nach Abschluss der Konfiguration Ihres Back-End-Pools die Option **Integritätstests** aus. Erstellen Sie einen Integritätstest für den Domänennamen, den Sie für Ihre App verwenden möchten. 

    ![Konfigurieren von Integritätstests][5]
    
4. Wählen Sie nach Abschluss der Konfiguration Ihrer Integritätstests die Option **HTTP-Einstellungen** aus.  Bearbeiten Sie dort die vorhandene Einstellung, wählen Sie die Option **Benutzerdefinierten Test verwenden** und dann den von Ihnen konfigurierten Test aus.

    ![Konfigurieren von HTTP-Einstellungen][6]
    
5. Wechseln Sie zum Application Gateway **Übersicht**, und kopieren Sie die öffentliche IP-Adresse, die für Ihr Application Gateway verwendet wurde.  Legen Sie diese IP-Adresse als A-Datensatz für Ihren App-Domänennamen fest, oder verwenden Sie den DNS-Namen für diese Adresse im CNAME-Datensatz.  Es ist einfacher, die öffentliche IP-Adresse auszuwählen und sie von der Benutzeroberfläche „Öffentlichen IP-Adresse“ als über den Link im Abschnitt „Application Gateway-Übersicht“ zu kopieren. 

    ![Application Gateway-Portal][7]

6. Legen Sie den benutzerdefinierten Domänennamens für Ihre App in Ihrer ILB-ASE fest.  Wechseln Sie zu Ihrer App im Portal, und wählen Sie unter „Einstellungen“ die Option **Benutzerdefinierte Domänen** aus.

![Festlegen des benutzerdefinierten Domänennamens für die App][8]

Informationen zum Festlegen der benutzerdefinierten Domänennamen für Ihre Web-Apps finden Sie unter [Festlegen benutzerdefinierter Domänennamen für Ihre Web-App][custom-domain]. Der Unterschied zwischen einer App in einer ILB-ASE und diesem Dokument ist, dass keine Überprüfung des Domänennamens erfolgt.  Da Sie Besitzer des DNS sind, mit dem die App-Endpunkte verwaltet werden, können Sie dort ablegen, was Sie möchten. Der benutzerdefinierte Domänenname, den Sie in diesem Fall hinzufügen, muss sich nicht in Ihrem DNS befinden, aber er muss weiterhin mit Ihrer App konfiguriert werden. 

Nachdem Setup abgeschlossen ist und Sie eine kurze Zeit gewartet haben, bis die DNS-Änderungen verbreitet wurden, können Sie über den von Ihnen erstellten benutzerdefinierten Domänennamen auf Ihre App zugreifen. 


<!--IMAGES-->
[1]: ./media/integrate-with-application-gateway/appgw-highlevel.png
[2]: ./media/integrate-with-application-gateway/appgw-createbasics.png
[3]: ./media/integrate-with-application-gateway/appgw-createsettings.png
[4]: ./media/integrate-with-application-gateway/appgw-backendpool.png
[5]: ./media/integrate-with-application-gateway/appgw-healthprobe.png
[6]: ./media/integrate-with-application-gateway/appgw-httpsettings.png
[7]: ./media/integrate-with-application-gateway/appgw-publicip.png
[8]: ./media/integrate-with-application-gateway/appgw-customdomainname.png
[9]: ./media/integrate-with-application-gateway/appgw-iplist.png

<!--LINKS-->
[appgw]: http://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
