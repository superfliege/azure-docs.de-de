---
title: Azure Virtual WAN – Übersicht | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die per Virtual WAN automatisierte skalierbare Konnektivität zwischen Branches einrichten.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 07/18/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 67dd6ba9b94ed9d58d91fb644ce9ee9e44ae9e45
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159168"
---
# <a name="what-is-azure-virtual-wan-preview"></a>Was ist Azure Virtual WAN? (Vorschau)

Der Netzwerkdienst Azure Virtual WAN bietet optimierte und automatisierte Konnektivität zwischen Branches über Azure. Per Virtual WAN können Sie Branchgeräte für die Kommunikation mit Azure verbinden und konfigurieren. Dies ist entweder manuell oder durch die Nutzung von Geräten eines bevorzugten Anbieters über einen Virtual WAN-Partner möglich. Die Nutzung von Geräten eines bevorzugten Anbieters ermöglicht eine einfache Verwendung, die Vereinfachung der Verbindungsherstellung und die Konfigurationsverwaltung. Über das integrierte Azure WAN-Dashboard können Sie anhand der Problembehandlung sofort Erkenntnisse gewinnen, um Zeit zu sparen, und auf einfache Weise umfangreiche Site-to-Site-Verbindungen anzeigen.

> [!IMPORTANT]
> Azure Virtual WAN befindet sich derzeit in der verwalteten Vorschauphase (Public Preview). Für die Nutzung von Virtual WAN müssen Sie sich [für die Vorschauversion registrieren](#enroll).
>
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dieser Artikel enthält eine kurze Übersicht über die Netzwerkkonnektivität Ihrer Workloads innerhalb und außerhalb von Azure. Virtual WAN hat die folgenden Vorteile:

* **Integrierte Konnektivitätslösungen (Hub & Spoke):** Automatisieren Sie die Site-to-Site-Konfiguration und -Konnektivität zwischen lokalen Standorten und einem Azure-Hub über eine Vielzahl von Ressourcen, z.B. Virtual WAN-Partnerlösungen.
* **Automatisierte Spoke-Einrichtung und -Konfiguration:** Verbinden Sie Ihre virtuellen Netzwerke und Workloads nahtlos mit dem Azure-Hub.
* **Intuitive Problembehandlung:** Sie können den gesamten Datenfluss in Azure anzeigen und diese Informationen nutzen, um erforderliche Aktionen durchzuführen.

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwan.png)

## <a name="vendor"></a>Nutzen eines Virtual WAN-Partners

1. Der Controller des Branchgeräts (VPN/SDWAN) wird authentifiziert, um standortbezogene Informationen per Azure-Dienstprinzipal zu exportieren.
2. Der Controller des Branchgeräts (VPN/SDWAN) ruft die Azure-Konnektivitätskonfiguration ab und aktualisiert das lokale Gerät. So werden für das lokale VPN-Gerät der Konfigurationsdownload und die Bearbeitung und die Aktualisierung automatisiert.
3. Nachdem das Gerät über die richtige Azure-Konfiguration verfügt, wird eine Site-to-Site-Verbindung (zwei aktive Tunnel) mit dem Azure WAN eingerichtet. Für Azure muss der Branchcontroller (VPN/SDWAN) IKEv2 unterstützen. BGP ist optional.

## <a name="resources"></a>Virtual WAN-Ressourcen

Sie erstellen die folgenden Ressourcen, um ein virtuelles End-to-End-WAN zu konfigurieren:

* **virtualWAN:** Die Ressource „virtualWAN“ stellt eine virtuelle Überlagerung Ihres Azure-Netzwerks dar und umfasst eine Sammlung mit mehreren Ressourcen. Sie enthält Links zu allen virtuellen Hubs, die Teil des virtuellen WAN sein sollen. Virtual WAN-Ressourcen sind voneinander isoliert und können keinen gemeinsamen Hub enthalten. Virtuelle Hubs kommunizieren per Virtual WAN nicht miteinander.

* **Site:** Die Ressource „Site“, die als „vpnsite“ bezeichnet wird, steht für Ihr lokales VPN-Gerät und die zugehörigen Einstellungen. Durch die Zusammenarbeit mit einem Virtual WAN-Partner verfügen Sie über eine integrierte Lösung zum automatischen Exportieren dieser Informationen nach Azure.

* **Hub:** Ein virtueller Hub ist ein von Microsoft verwaltetes virtuelles Netzwerk. Der Hub enthält verschiedene Dienstendpunkte zum Aktivieren der Konnektivität über Ihr lokales Netzwerk (vpnsite). Der Hub ist der Kern Ihres Netzwerks in einer Region. Es kann nur ein Hub pro Azure-Region vorhanden sein. Wenn Sie einen Hub mit dem Azure-Portal erstellen, werden für den virtuellen Hub automatisch ein VNET und ein „vpngateway“ erstellt.

  Ein Hub-Gateway ist nicht das gleiche wie ein Gateway für virtuelle Netzwerke, das Sie für ExpressRoute und VPN Gateway verwenden. Bei der Verwendung von Virtual WAN erstellen Sie beispielsweise von Ihrer lokalen Site aus keine direkte Site-to-Site-Verbindung mit Ihrem VNET. Stattdessen erstellen Sie eine Site-to-Site-Verbindung mit dem Hub. Der Datenverkehr verläuft immer über das Hub-Gateway. Dies bedeutet, dass Ihre VNETs kein eigenes Gateway für virtuelle Netzwerke benötigen. Mit Virtual WAN können Sie für Ihre VNETs über den virtuellen Hub und das virtuelle Hub-Gateway leicht eine Skalierung durchführen. 

* **Virtuelle Netzwerkverbindung für Hub:** Die Ressource für die virtuelle Netzwerkverbindung für den Hub wird verwendet, um den Hub nahtlos mit Ihrem virtuellen Netzwerk zu verbinden. Derzeit können Sie nur eine Verbindung mit virtuellen Netzwerken herstellen, die sich in derselben Hub-Region befinden.

##<a name="enroll"></a>Registrieren für die Vorschauversion

Bevor Sie Virtual WAN konfigurieren können, müssen Sie Ihr Abonnement zunächst für die Vorschauversion registrieren. Andernfalls können Sie Virtual WAN im Portal nicht verwenden. Senden Sie für die Registrierung eine E-Mail mit Ihrer Abonnement-ID an <azurevirtualwan@microsoft.com>. Sie erhalten eine E-Mail zurück, nachdem Ihr Abonnement registriert wurde.

## <a name="faq"></a>Häufig gestellte Fragen

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="feedback"></a>Feedback zur Vorschauversion

Wir schätzen Ihr Feedback. Senden Sie eine E-Mail an <azurevirtualwan@microsoft.com>, um Probleme zu melden oder Feedback zu Virtual WAN zu geben (positiv oder negativ). Fügen Sie den Namen Ihres Unternehmens in eckigen Klammern in die Betreffzeile ein. Wenn Sie ein Problem melden, geben Sie auch Ihre Abonnement-ID an.

## <a name="next-steps"></a>Nächste Schritte

Zum Erstellen einer Site-to-Site-Verbindung mit Virtual WAN können Sie entweder einen [Virtual WAN-Partner](https://aka.ms/virtualwan) nutzen oder die Verbindung manuell erstellen. Informationen zur manuellen Erstellung der Verbindung finden Sie unter [Erstellen einer Site-to-Site-Verbindung per Virtual WAN](virtual-wan-site-to-site-portal.md).
