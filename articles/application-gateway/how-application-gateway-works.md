---
title: Funktionsweise von Azure Application Gateway
description: Dieser Artikel enthält Informationen zur Funktionsweise des Application Gateways
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: ef07def377b74fb74d57372f471efcf48fcf7aa2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57881094"
---
# <a name="how-application-gateway-works"></a>Funktionsweise von Application Gateway

In diesem Artikel wird erläutert, wie das Application Gateway die eingehenden Anforderungen akzeptiert und sie an das Back-End routet.

![how-application-gateway-works](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-a-request-is-accepted"></a>Wie eine Anforderung akzeptiert wird

Bevor ein Client eine Anforderung an ihr Application Gateway sendet, löst er den Domänennamen des Application Gateways mithilfe eines DNS-Servers (Domain Name System) auf. Der DNS-Eintrag wird von Azure gesteuert, da sich Ihre Application Gateways in der azure.com-Domäne befinden. Azure DNS gibt die IP-Adresse an den Client zurück, dabei handelt es sich um die *Front-End-IP-Adresse* des Application Gateways. Das Application Gateway akzeptiert eingehenden Datenverkehr an einem oder mehreren *Listenern*. Ein Listener ist eine logische Entität, die auf eingehende Verbindungsanforderungen prüft. Sie wird mit einer Front-End-IP-Adresse, einem Protokoll und einer Portnummer für Verbindungen vom Client mit dem Application Gateway konfiguriert. Falls die Web Application Firewall (WAF) aktiviert ist, prüft Application Gateway die Anforderungsheader und den Textkörper (falls vorhanden) anhand der *WAF-Regeln*, um zu bestimmen, ob die Anforderung eine gültige Anforderung ist – in diesem Fall wird sie an das Back-End geroutet – oder ein Sicherheitsrisiko, in welchem Fall sie gesperrt wird.  

Application Gateway kann als ein interner Load Balancer für Anwendungen oder als ein vom Internet zugänglicher Load Balancer für Anwendungen verwendet werden. Ein Application Gateway mit Internetzugriff hat eine öffentliche IP-Adresse. Der DNS-Name eines Application Gateways mit Internetzugriff lässt sich öffentlich in seine öffentliche IP-Adresse auflösen. Daher können Application Gateways mit Internetzugriff Anforderungen von Clients über das Internet routen. Interne Application Gateway verfügen nur über private IP-Adressen. Der DNS-Name eines internen Application Gateways lässt sich öffentlich zu seiner privaten IP-Adresse auflösen. Daher können interne Load Balancer nur Anforderungen von Clients mit Zugriff auf das VNET für das Application Gateway routen. Sowohl Application Gateways mit Internetzugriff als auch interne Application Gateways routen Anforderungen mithilfe von privaten IP-Adressen an Ihre Back-End-Server weiter. Daher benötigen Ihre Back-End-Server keine öffentlichen IP-Adressen, um Anforderungen von einem internen Application Gateway oder einem Application Gateway mit Internetzugriff zu empfangen.

## <a name="how-a-request-is-routed"></a>Wie eine Anforderung geroutet wird

Wenn die Anforderung als gültig erkannt wird (oder keine WAF aktiviert ist), wird die *Routingregel für Anforderungen*, die dem *Listener* zugeordnet ist, ausgewertet, um den *Back-End-Pool* zu ermitteln, an den die Anforderung geroutet werden soll. Regeln werden in der Reihenfolge verarbeitet, in der sie im Portal aufgeführt sind. Auf der Grundlage der Konfiguration der *Routingregel für Anforderungen* entscheidet das Application Gateway, ob alle Anforderungen an den Listener an einen bestimmten Back-End-Pool, je nach dem URL-Pfad an verschiedene Back-End-Pools oder an *Umleitungsanforderungen* zu einem anderen Port oder einer externen Website geroutet werden sollen

Nachdem ein *Back-End*-*Pool* gewählt wurde, sendet das Application Gateway die Anforderung an einen der fehlerfreien Back-End-Server, die in dem Pool konfiguriert sind (y.y.y.y). Die Integrität des Servers wird mithilfe eines *Integritätstests* überprüft. Wenn der Back-End-Pool mehrere Server enthält, verwendet das Application Gateway den Roundrobin-Algorithmus, um die Anforderungen zwischen den fehlerfreien Servern zu routen, wodurch ein Lastenausgleich für die Anforderungen auf den Servern erfolgt.

Nachdem ein Back-End-Server bestimmt worden ist, öffnet das Application Gateway eine neue TCP-Sitzung mit dem Back-End-Server und verwendet dazu die Konfiguration in den *HTTP-Einstellungen*. Die *HTTP-Einstellungen* sind eine Komponente, die das Protokoll, den Port und andere routingbezogene Einstellungen angibt, die zum Herstellen einer neuen Sitzung mit dem Back-End-Server erforderlich sind. Die Angaben für Port und Protokoll in den HTTP-Einstellungen bestimmen, ob der Datenverkehr zwischen dem Application Gateway und den Back-End-Servern verschlüsselt wird, wodurch End-to-End-SSL erreicht werden kann, oder unverschlüsselt bleibt. Beim Senden der ursprünglichen Anforderung an den Back-End-Server berücksichtigt das Application Gateway alle benutzerdefinierten Konfigurationen in den HTTP-Einstellungen, die sich auf das Überschreiben von Hostnamen, Pfad oder Protokoll, auf die Aufrechterhaltung von cookiebasierter Sitzungsaffinität, Verbindungsausgleich, die Übernahme des Hostnamens vom Back-End usw. beziehen.

Ein internes Application Gateway verfügt nur über private IP-Adressen. Der DNS-Name eines internen Application Gateways lässt sich intern zu seiner privaten IP-Adresse auflösen. Daher können interne Load Balancer nur Anforderungen von Clients mit Zugriff auf das VNET für das Application Gateway routen.

Beachten Sie, dass sowohl interne Application Gateways als auch solche mit Internetzugriff Anforderungen mithilfe von privaten IP-Adressen an ihre Back-End-Server routen, falls Ihre Back-End-Poolressource eine private IP-Adresse, eine VM-NIC-Konfiguration oder einer intern auflösbare Adresse enthält. Falls ihr Back-End-Pool einen öffentlichen Endpunkt darstellt, verwendet Application Gateway dessen öffentliche Front-End-IP, um den Server zu erreichen. Wenn Sie keine öffentliche Front-End-IP-Adresse bereitgestellt haben, wird eine für die ausgehende externe Konnektivität zugewiesen.

### <a name="modifications-to-the-request"></a>Änderungen an der Anforderung

Application Gateway fügt 4 zusätzliche Header in alle Anforderungen ein, bevor es die Anforderungen an das Back-End weiterleitet. Diese Header sind X-forwarded-for, X-forwarded-proto, X-forwarded-port und X-original-host. Das Format für den x-forwarded-for-Header ist eine durch Trennzeichen getrennte Liste von IP:Port. Die gültigen Werte für x-forwarded-proto sind HTTP und HTTPS. X-Forwarded-Port gibt den Port an, an dem die Anforderung das Application Gateway erreicht hat. Der X-original-host-Header enthält den ursprünglichen Hostheader, mit dem die Anforderung eingetroffen ist. Dieser Header ist nützlich in Szenarien wie der Azure-Websiteintegration. Dabei wird der eingehende Hostheader modifiziert, bevor der Datenverkehr zum Back-End weitergeleitet wird. Bei optional aktivierter Sitzungsaffinität wird ein vom Gateway verwaltetes Affinitätscookie eingefügt. 

Zusätzlich können Sie das Application Gateway für das Ändern von Headern mithilfe von [Rewrite HTTP headers](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) (Erneutes Generieren von HTTP-Headern) oder das Ändern des URI-Pfads mithilfe der Einstellung für die Pfadüberschreibung konfigurieren. Sofern dies nicht konfiguriert ist, werden jedoch alle eingehenden Anforderungen unverändert an das Back-End weitergeleitet.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Application Gateway funktioniert, lesen Sie [Application Gateway-Komponenten](application-gateway-components.md), um ein detaillierteres Verständnis seiner Komponenten zu erwerben.
