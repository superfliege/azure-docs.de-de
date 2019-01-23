---
title: Automatische Skalierung und zonenredundantes Application Gateway in Azure (öffentliche Vorschau)
description: Dieser Artikel enthält Informationen zu Anforderungsgrößenlimits und Ausschlusslisten für die Web Application Firewall (WAF) in Application Gateway mit dem Azure-Portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 1/10/2019
ms.author: victorh
ms.openlocfilehash: f5885fd2ac76550990c9a56a1d200bbe11555918
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54213755"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Automatische Skalierung und zonenredundantes Application Gateway (öffentliche Vorschau)

Application Gateway und Web Application Firewall (WAF) sind nun in der öffentlichen Vorschau unter einer neuen v2-SKU verfügbar, die eine bessere Leistung und Unterstützung für wichtige neue Funktionen wie automatische Skalierung, Zonenredundanz sowie Unterstützung statischer VIPs bietet. Vorhandene Features in der allgemein erhältlichen SKU werden in der neuen v2-SKU weiterhin unterstützt. Es gelten aber einige wenige Ausnahmen, die im Abschnitt zu den bekannten Einschränkungen aufgeführt sind. Die neuen v2-SKUs enthalten die folgenden Verbesserungen:

- **Automatische Skalierung**: Für Application Gateway- oder WAF-Bereitstellungen unter der SKU mit automatischer Skalierung ist nun das zentrale Hochskalieren und Herunterskalieren je nach Veränderung der Netzwerkdatenverkehr-Auslastungsmuster möglich. Durch die automatische Skalierung entfällt auch die Notwendigkeit, während des Bereitstellens eine Bereitstellungsgröße oder eine Anzahl von Instanzen auszuwählen. Somit bietet die SKU wahre Flexibilität. In der neuen SKU kann Application Gateway sowohl mit fester Kapazität (automatische Skalierung deaktiviert) als auch mit aktivierter automatischer Skalierung betrieben werden. Der Modus mit fester Kapazität empfiehlt sich für Szenarien mit einheitlichen und vorhersagbaren Workloads. Der Modus mit automatischer Skalierung empfiehlt sich für Anwendungen, bei denen der Anwendungsdatenverkehr großen Schwankungen unterworfen ist.
   
   > [!NOTE]
   > Die automatische Skalierung ist derzeit für die WAF-SKU nicht verfügbar. Konfigurieren Sie WAF mit dem Modus mit fester Kapazität und nicht mit dem Modus mit automatischer Skalierung.
- **Zonenredundanz**: Eine Application Gateway- oder WAF-Bereitstellung kann sich über mehrere Verfügbarkeitszonen erstrecken, sodass nicht mehr in jeder Zone mit einem Traffic Manager separate Application Gateway-Instanzen bereitgestellt und eingerichtet werden müssen. Sie können eine einzelne Zone oder mehrere Zonen mit bereitgestellten Application Gateway-Instanzen auswählen, um die Resilienz bei einem Zonenausfall zu gewährleisten. Der Back-End-Pool für Anwendungen kann auf ähnliche Weise auf Verfügbarkeitszonen verteilt werden.
- **Leistungsverbesserungen**: Die SKU mit automatischer Skalierung bietet eine bis zu fünf Mal bessere SSL-Auslagerungsleistung verglichen mit der allgemein verfügbaren SKU.
- **Schnellere Bereitstellung und Aktualisierung**: Die SKU mit automatischer Skalierung ermöglicht verglichen mit der allgemein verfügbaren SKU eine schnellere Bereitstellung und Aktualisierung.
- **Statische VIP**: Die Application Gateway-VIP unterstützt nun exklusiv den statischen VIP-Typ. Dadurch wird sichergestellt, dass die dem Anwendungsgateway zugeordnete VIP selbst nach einem Neustart unverändert bleibt.

> [!IMPORTANT]
> Die Application Gateway-SKU mit automatischer Skalierung und Zonenredundanz ist derzeit als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Unterstützte Regionen
Die SKU mit automatischer Skalierung ist in „USA, Osten 2“, „USA, Mitte“, „USA, Westen 2“, „USA, Norden-Mitte“, „USA, Westen“, „USA, Süden-Mitte“, „Frankreich, Mitte“, „Europa, Westen“, „Europa, Norden“, „Vereinigtes Königreich, Westen“, „Asien, Südosten“ und „Japan, Osten“ verfügbar.

## <a name="pricing"></a>Preise
Während der Vorschauphase fallen keine Kosten für das Anwendungsgateway an. Andere Ressourcen (Key Vault, virtuelle Computer und Ähnliches) werden Ihnen jedoch in Rechnung gestellt. 

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

|Problem|Details|
|--|--|
|Authentifizierungszertifikat|Nicht unterstützt.<br>Weitere Informationen finden Sie unter [Übersicht über End-to-End-SSL mit Application Gateway](ssl-overview.md#end-to-end-ssl-with-the-v2-sku).|
|Mischen von Standard_v2 und Standardanwendungsgateway in demselben Subnetz|Nicht unterstützt.<br>Wenn die automatische Skalierung aktiviert ist, kann ein Subnetz außerdem nur über ein Anwendungsgateway verfügen.|
|Benutzerdefinierte Route im Application Gateway-Subnetz|Nicht unterstützt|
|NSG für Eingangsportbereich| - 65200 bis 65535 für Standard_v2-SKU<br>- 65503 bis 65534 für Standard-SKU.<br>Weitere Informationen finden Sie in den [häufig gestellten Fragen](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Leistungsprotokolle in Azure-Diagnose|Nicht unterstützt.<br>Es sollten Azure-Metriken verwendet werden.|
|Abrechnung|Derzeit findet keine Abrechnung statt.|
|FIPS-Modus, WebSocket|Diese werden derzeit nicht unterstützt.|
|Reiner ILB-Modus|Dies wird derzeit nicht unterstützt. Öffentlicher und ILB-Modus kombiniert werden unterstützt.|
|Automatische Skalierung für Web Application Firewall|WAF unterstützt derzeit keine automatische Skalierung. Der Modus mit fester Kapazität wird unterstützt.|

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen Sie mit Azure PowerShell ein zonenredundantes Application Gateway mit automatischer Skalierung und einer reservierten virtuellen IP-Adresse.](tutorial-autoscale-ps.md)
- Erfahren Sie mehr über [Application Gateway](overview.md).
- Erfahren Sie mehr über [Azure Firewall](../firewall/overview.md). 

