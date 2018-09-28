---
title: Automatische Skalierung und zonenredundantes Application Gateway in Azure (öffentliche Vorschau) | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zu Anforderungsgrößenlimits und Ausschlusslisten für die Web Application Firewall (WAF) in Application Gateway mit dem Azure-Portal.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: victorh
ms.openlocfilehash: 5d051ed049964af708056e2963f04cb478c15906
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951507"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Automatische Skalierung und zonenredundantes Application Gateway (öffentliche Vorschau)

Application Gateway und Web Application Firewall (WAF) sind nun in der öffentlichen Vorschau unter einer neuen SKU verfügbar, die eine bessere Leistung und Unterstützung für wichtige neue Funktionen wie automatische Skalierung, Zonenredundanz sowie Unterstützung statischer VIPs bietet. Vorhandene Features in der allgemein erhältlichen SKU werden in der neuen SKU weiterhin unterstützt. Es gelten jedoch einige wenige Ausnahmen, die im Abschnitt zu bekannten Einschränkungen aufgeführt sind. Die neuen SKUs enthalten die folgenden Verbesserungen:

- **Automatische Skalierung**: Für Application Gateway- oder WAF-Bereitstellungen unter der SKU mit automatischer Skalierung ist nun das zentrale Hochskalieren und Herunterskalieren je nach sich ändernden Netzwerkdatenverkehr-Auslastungsmustern möglich. Durch die automatische Skalierung entfällt auch die Notwendigkeit, während des Bereitstellens eine Bereitstellungsgröße oder eine Anzahl von Instanzen auszuwählen. Somit bietet die SKU wahre Flexibilität. In der neuen SKU kann Application Gateway sowohl mit fester Kapazität (automatische Skalierung deaktiviert) als auch mit aktivierter automatischer Skalierung betrieben werden. Der Modus mit fester Kapazität empfiehlt sich für Szenarien mit einheitlichen und vorhersagbaren Workloads. Der Modus mit automatischer Skalierung empfiehlt sich für Anwendungen, bei denen der Anwendungsdatenverkehr großen Schwankungen unterworfen ist.
   
   > [!NOTE]
   > Die automatische Skalierung ist derzeit für die WAF-SKU nicht verfügbar. Konfigurieren Sie WAF mit dem Modus mit fester Kapazität und nicht mit dem Modus mit automatischer Skalierung.
- **Zonenredundanz**: Eine Application Gateway- oder WAF-Bereitstellung kann sich über mehrere Verfügbarkeitszonen erstrecken, sodass nicht mehr in jeder Zone mit einem Traffic Manager separate Application Gateway-Instanzen bereitgestellt und eingerichtet werden müssen. Sie können eine einzelne Zone oder mehrere Zonen mit bereitgestellten Application Gateway-Instanzen auswählen, um die Resilienz bei einem Zonenausfall zu gewährleisten. Der Back-End-Pool für Anwendungen kann auf ähnliche Weise auf Verfügbarkeitszonen verteilt werden.
- **Leistungsverbesserungen**: Die SKU mit automatischer Skalierung bietet eine bis zu fünf Mal bessere SSL-Auslagerungsleistung verglichen mit der allgemein verfügbaren SKU.
- **Schnellere Bereitstellung und Aktualisierung**: Die SKU mit automatischer Skalierung ermöglicht verglichen mit der allgemein verfügbaren SKU eine schnellere Bereitstellung und Aktualisierung.
- **Statische VIP:** Die Application Gateway-VIP unterstützt nun exklusiv den statischen VIP-Typ. Dadurch wird sichergestellt, dass die dem Anwendungsgateway zugeordnete VIP selbst nach einem Neustart unverändert bleibt.

> [!IMPORTANT]
> Die Application Gateway-SKU mit automatischer Skalierung und Zonenredundanz ist derzeit als öffentliche Vorschau verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Unterstützte Regionen
Die SKU mit automatischer Skalierung ist verfügbar in USA, Osten 2, USA, Mitte, USA Westen 2, Frankreich, Mitte, Europa, Westen und Asien, Südosten.

## <a name="pricing"></a>Preise
Während der Vorschauphase fallen keine Kosten für das Anwendungsgateway an. Andere Ressourcen (Key Vault, virtuelle Computer und Ähnliches) werden Ihnen jedoch in Rechnung gestellt. 

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

|Problem|Details|
|--|--|
|Abrechnung|Derzeit findet keine Abrechnung statt.|
|FIPS-Modus, WebSocket|Diese werden derzeit nicht unterstützt.|
|Reiner ILB-Modus|Dies wird derzeit nicht unterstützt. Öffentlicher und ILB-Modus kombiniert werden unterstützt.|
|Automatische Skalierung für Web Application Firewall|WAF unterstützt derzeit keine automatische Skalierung. Der Modus mit fester Kapazität wird unterstützt.|

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Application Gateway](overview.md).
- Erfahren Sie mehr über [Azure Firewall](../firewall/overview.md). 

