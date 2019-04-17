---
title: Netzwerkoptionen von Azure Functions
description: Enthält eine Übersicht über alle Netzwerkoptionen, die in Azure Functions verfügbar sind.
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: conceptual
ms.date: 1/14/2019
ms.author: alkarche
ms.openlocfilehash: 10d7daa6da45c56e20c622fcbca9ee288e737dab
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358163"
---
# <a name="azure-functions-networking-options"></a>Netzwerkoptionen von Azure Functions

In diesem Dokument wird die Suite der Netzwerkfeatures beschrieben, die für die Hostingoptionen von Azure Functions zur Verfügung stehen. Alle folgenden Netzwerkoptionen verfügen über die Möglichkeit zum Zugreifen auf Ressourcen ohne Verwendung von Adressen, die über das Internet geroutet werden können, oder die Einschränkung des Internetzugriffs auf eine Funktions-App. Für die Hostingmodelle sind unterschiedliche Ebenen der Netzwerkisolation verfügbar, und wenn Sie die richtige Auswahl treffen, können Sie Ihre Anforderungen in Bezug auf die Netzwerkisolation erfüllen.

Funktions-Apps können auf unterschiedliche Arten gehostet werden.

* Es gibt eine Gruppe von Tarifoptionen, die in einer mehrinstanzenfähigen Infrastruktur ausgeführt werden und über unterschiedliche Ebenen von VNET-Konnektivitäts- und Skalierungsoptionen verfügen.
    1. Der Verbrauchstarif, der als Reaktion auf Last dynamisch skaliert wird und nur über Optionen für eine minimale Netzwerkisolation verfügt.
    1. Der Premium-Tarif, bei dem ebenfalls dynamisch skaliert wird, der aber eine umfassendere Netzwerkisolation ermöglicht.
    1. Der App Service-Plan, bei dem eine feste Skalierung verwendet wird und der eine ähnliche Netzwerkisolation wie der Premium-Tarif aufweist.
* Funktionen können auch in einer App Service-Umgebung (ASE) ausgeführt werden, in der Ihre Funktion in Ihrem VNET bereitgestellt wird und eine umfassende Netzwerksteuerung und -isolation möglich ist.

## <a name="networking-feature-matrix"></a>Matrix mit Netzwerkfeatures

|                |[Verbrauchstarif](functions-scale.md#consumption-plan)|⚠ [Premium-Tarif](functions-scale.md##premium-plan-public-preview)|[App Service-Plan](functions-scale.md#app-service-plan)|[App Service-Umgebung](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[**IP-Einschränkungen für eingehenden Datenverkehr**](#inbound-ip-restrictions)|✅ Ja|✅ Ja|✅ Ja|✅ Ja|
|[**VNET-Integration**](#vnet-integration)|❌ Nein|⚠ Ja|✅ Ja|✅ Ja|
|[**Vorschauversion der VNET-Integration (ExpressRoute und Dienstendpunkte)**](#preview-vnet-integration)|❌ Nein|⚠ Ja|⚠ Ja|✅ Ja|
|[**Hybridverbindungen**](#hybrid-connections)|❌ Nein|❌ Nein|✅ Ja|✅ Ja|
|[**Privater Websitezugriff**](#private-site-access)|❌ Nein| ❌ Nein|❌ Nein|✅ Ja|

⚠ Previewfunktion, nicht für die Verwendung in der Produktion

## <a name="inbound-ip-restrictions"></a>IP-Einschränkungen für eingehenden Datenverkehr

Mit IP-Einschränkungen können Sie eine nach Priorität sortierte Zulassen/Ablehnen-Liste mit IP-Adressen definieren, die auf Ihre App zugreifen dürfen. Die Zulassen-Liste kann IPv4- und IPv6-Adressen enthalten. Wenn mindestens ein Eintrag vorhanden ist, enthält die Liste am Ende einen impliziten Eintrag vom Typ „Alle ablehnen“. Die Funktion für IP-Einschränkungen funktioniert mit allen Hostingoptionen für Funktionen.

> [!NOTE]
> Um den Azure-Portal-Editor verwenden zu können, muss das Portal direkt auf Ihre ausgeführte Funktions-App zugreifen können. Außerdem muss die IP-Adresse des Geräts, das Sie zum Zugreifen auf das Portal verwenden, in einer Whitelist aufgeführt sein. Wenn die Netzwerkeinschränkungen vorhanden sind, können Sie weiterhin auf die Features der Registerkarte **Plattformfeatures** zugreifen.

[Weitere Informationen](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)

## <a name="vnet-integration"></a>VNET-Integration

Die VNET-Integration ermöglicht Ihrer Funktions-App das Zugreifen auf Ressourcen eines VNET. Die VNET-Integration ist sowohl für den Premium-Tarif als auch für den App Service-Plan verfügbar. In einer App Service-Umgebung befindet sich Ihre App bereits in einem VNET und benötigt das VNET-Integrationsfeature nicht, um Ressourcen in demselben VNET zu erreichen.

Die VNET-Integration ermöglicht Ihrer Funktions-App den Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, gewährt aber keinen [privaten Websitezugriff](#private-site-access) auf Ihre Funktions-App aus dem virtuellen Netzwerk.

VNET-Integration wird häufig verwendet, um Zugriff von Apps auf Datenbanken und Webdienste in Ihrem VNET zu ermöglichen. Bei der VNET-Integration müssen Sie keinen öffentlichen Endpunkt für Anwendungen auf Ihrer VM verfügbar machen, sondern Sie können stattdessen die privaten nicht über das Internet routbaren Adressen verwenden.

Die allgemein verfügbare Version der VNET-Integration beruht auf einem VPN-Gateway zum Verbinden von Funktions-Apps mit einem virtuellen Netzwerk. Sie ist verfügbar, wenn Functions unter einem App Service-Plan gehostet wird. Informationen zur Konfiguration dieses Features finden Sie im [entsprechenden App Service-Dokument](../app-service/web-sites-integrate-with-vnet.md#enabling-vnet-integration).

### <a name="preview-vnet-integration"></a>Vorschauversion der VNET-Integration

Es gibt eine neue Version des VNET-Integrationsfeatures, das sich in der Vorschau befindet. Es ist nicht von Point-to-Site-VPN abhängig und unterstützt auch den Zugriff auf Ressourcen über ExpressRoute oder Dienstendpunkte. Dieses Feature ist unter dem Premium-Tarif und unter App Service-Plänen mit Skalierung auf PremiumV2 verfügbar.

Die neue Version der VNET-Integration, die sich derzeit in der Vorschauphase befindet, hat die folgenden Vorteile:

* Kein Gateway ist erforderlich, um die neue VNET-Integrationsfunktion zu verwenden.
* Sie können ohne zusätzliche Konfiguration bis auf die Integration mit dem mit ExpressRoute verbundenen VNET über ExpressRoute-Verbindungen auf Ressourcen zugreifen.
* Sie können per Dienstendpunkt geschützte Ressourcen über die ausgeführte Functions-Instanz nutzen. Aktivieren Sie dazu Dienstendpunkte in dem für die VNET-Integration verwendeten Subnetz.
* Sie können keine Trigger für die Nutzung von mit Dienstendpunkten geschützten Ressourcen konfigurieren, indem Sie die neue Funktion für die VNET-Integration verwenden. 
* Sowohl die Funktions-App als auch das VNET müssen sich in derselben Region befinden.
* Das neue Feature erfordert ein nicht verwendetes Subnetz im Resource Manager-VNET.
* Produktionsworkloads werden unter der neuen Version der VNET-Integration nicht unterstützt, während sich diese in der Vorschauphase befindet.
* Routingtabellen und globales Peering sind mit der neuen VNET-Integration noch nicht verfügbar.
* Für jede potenzielle Funktions-App-Instanz wird eine Adresse verwendet. Verwenden Sie ein Subnetz, das Ihre maximale Skalierungsgröße leicht unterstützen kann, da die Subnetzgröße nach dem Zuweisen nicht mehr geändert werden kann. Zur Unterstützung eines Premium-Tarifs, bei dem auf bis zu 80 Instanzen skaliert werden kann, empfehlen wir beispielsweise ein `/25`-Subnetz mit 126 Hostadressen.

Weitere Informationen zur Verwendung der Vorschauversion der VNET-Integration finden Sie unter [Integrieren einer Funktions-App in ein Azure Virtual Network](functions-create-vnet.md).

## <a name="hybrid-connections"></a>Hybridverbindungen

[Hybrid Connections](../service-bus-relay/relay-hybrid-connections-protocol.md) ist ein Feature von Azure Relay, das zum Zugreifen auf Anwendungsressourcen in anderen Netzwerken verwendet werden kann. Es ermöglicht den Zugriff von Ihrer App auf einen Anwendungsendpunkt. Es kann nicht genutzt werden, um auf Ihre Anwendung zuzugreifen. Hybrid Connections ist für Funktionen verfügbar, die unter einem [App Service-Plan](functions-scale.md#app-service-plan) und in einer [App Service-Umgebung](../app-service/environment/intro.md) ausgeführt werden.

Bei der Verwendung in Functions entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port. Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, vorausgesetzt, der Zugriff erfolgt über einen TCP-Lauschport. Das Feature Hybrid Connections besitzt keine Informationen über das Anwendungsprotokoll oder den abzurufenden Inhalt und benötigt diese Informationen auch nicht. Sie ermöglichen lediglich den Netzwerkzugriff.

Weitere Informationen finden Sie in der [App Service-Dokumentation zu Hybrid Connections](../app-service/app-service-hybrid-connections.md). Sowohl Functions als auch Web-Apps werden unterstützt.

## <a name="private-site-access"></a>Privater Websitezugriff

Privater Websitezugriff bezieht sich darauf, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z.B. aus einem virtuellen Azure-Netzwerk heraus. Der private Websitezugriff ist nur verfügbar, wenn eine ASE mit einem internen Lastenausgleichsmodul (Internal Load Balancer, ILB) konfiguriert ist. Weitere Informationen zur Verwendung einer ILB-ASE finden Sie unter [Erstellen und Verwenden einer ILB-ASE](../app-service/environment/create-ilb-ase.md).

Es gibt viele Möglichkeiten, um mit anderen Hostingoptionen auf VNET-Ressourcen zuzugreifen, aber nur über eine ASE kann zugelassen werden, dass Trigger für eine Funktion über ein VNET verwendet werden.
