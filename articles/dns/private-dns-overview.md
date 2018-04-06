---
title: Verwenden von Azure DNS für private Domänen | Microsoft-Dokumentation
description: Übersicht über private DNS-Hostingdienste in Microsoft Azure.
services: dns
documentationcenter: na
author: KumudD
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: 7f1bd8cdcab7bdd61b3f006acf6090c53db8eda6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="using-azure-dns-for-private-domains"></a>Verwenden von Azure DNS für private Domänen
Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Dienstnamens in die IP-Adresse verantwortlich. Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt.  Zusätzlich zu DNS-Domänen mit Internetverbindung unterstützt Azure DNS jetzt auch private DNS-Domänen als Vorschaufunktion.  
 
Azure DNS bietet einen zuverlässigen, sicheren DNS-Dienst zum Verwalten und Auflösen von Domänennamen in einem virtuellen Netzwerk, ohne dass Sie eine benutzerdefinierte DNS-Lösung hinzufügen müssen. In privaten DNS-Zonen können Sie anstatt der heute verfügbaren von Azure bereitgestellten Namen Ihre eigenen benutzerdefinierten Domänennamen verwenden.  Die Verwendung benutzerdefinierter Domänennamen erleichtert Ihnen die optimale Anpassung der Architektur Ihres virtuellen Netzwerks an die Anforderungen Ihres Unternehmens. Sie bietet Namensauflösung für virtuelle Computer in einem virtuellen Netzwerk und zwischen virtuellen Netzwerken. Darüber hinaus können Sie Zonennamen mit einer Split-Horizon-Ansicht konfigurieren, sodass eine private und eine öffentliche DNS-Zone den gleichen Namen verwenden können.

![Übersicht über DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Vorteile

* **Macht benutzerdefinierte DNS-Lösungen überflüssig.** Früher erstellten viele Kunden benutzerdefinierte DNS-Lösungen zum Verwalten von DNS-Zonen in ihrem virtuellen Netzwerk.  Die Verwaltung von DNS-Zonen kann jetzt mithilfe der nativen Azure-Infrastruktur durchgeführt werden, sodass das lästige Erstellen und Verwalten benutzerdefinierter DNS-Lösungen entfällt.

* **Verwenden aller gängigen DNS-Eintragstypen.**  Azure DNS unterstützt A-, AAAA-, CNAME-, MX-, NS-, PTR-, SOA-, SRV- und TXT-Einträge.

* **Automatische Hostnameneintrags-Verwaltung.** Parallel zum Hosten Ihrer benutzerdefinierten DNS-Einträge verwaltet Azure automatisch Hostnameneinträge für die virtuellen Computer in den angegebenen virtuellen Netzwerken.  So können Sie die Domänennamen optimieren, die Sie verwenden, ohne benutzerdefinierte DNS-Lösungen erstellen oder die Anwendung ändern zu müssen.

* **Hostnamenauflösung zwischen virtuellen Netzwerken.** Im Gegensatz zu von Azure bereitgestellten Hostnamen können private DNS-Zonen von virtuellen Netzwerken gemeinsam genutzt werden.  Diese Möglichkeit vereinfacht netzwerkübergreifende und Dienstermittlungsszenarien wie das Peering virtueller Netzwerke.

* **Vertraute Tools und Benutzerfreundlichkeit.** Zur Reduzierung der Lernkurve verwendet dieses neue Angebot die bereits gut eingeführten Azure DNS-Tools (PowerShell, Resource Manager-Vorlagen, REST-API).

* **Split-Horizon-DNS-Unterstützung.** Azure DNS ermöglicht Ihnen, Zonen mit gleichem Namen zu erstellen, die innerhalb eines virtuellen Netzwerks und im öffentlichen Internet unterschiedlich aufgelöst werden.  Ein typisches Szenario für Split-Horizon-DNS ist die Bereitstellung einer bestimmte Version eines Diensts für die Verwendung in Ihrem virtuellen Netzwerk .

* **In allen Azure-Regionen verfügbar** Azure DNS Private Zones ist in allen Azure-Regionen in der öffentlichen Azure-Cloud verfügbar. 


## <a name="capabilities"></a>Funktionen 
* Automatische Registrierung von virtuellen Computern aus einem einzelnen virtuellen Netzwerk, das einer privaten Zone zugeordnet ist, als virtuelles Registrierungsnetzwerk. Die virtuellen Computer werden für die private Zone als A-Datensätze registriert (hinzugefügt), die auf ihre privaten IP-Adressen verweisen. Darüber hinaus wird beim Löschen eines virtuellen Computers in einem virtuellen Registrierungsnetzwerk von Azure automatisch auch der entsprechende DNS-Eintrag aus der verknüpften privaten Zone entfernt. Beachten Sie, dass ein virtuelles Registrierungsnetzwerk auch standardmäßig insofern als virtuelles Auflösungsnetzwerks fungiert, als dass DNS-Auflösung für die Zone von jedem der virtuellen Computer im virtuellen Registrierungsnetzwerk funktioniert. 
* Weiterleiten von DNS-Auflösung wird über virtuelle Netzwerke hinweg unterstützt, die mit der privaten Zone als virtuelle Auflösungsnetzwerke verknüpft sind. Für DNS-Auflösung über die Grenzen von virtuellen Netzwerken hinweg gibt es keine explizite Abhängigkeit dahingehend, dass die virtuellen Netzwerke zum selben Peering gehören müssen. Es kann aber sein, dass Kunden virtuelle Netzwerke für andere Szenarien (z. B. HTTP-Datenverkehr) zu einem Peering zusammenfassen möchten.
* Reverse-DNS-Lookup wird im VNET-Bereich unterstützt. Reverse-DNS-Lookup für eine private IP-Adresse im virtuellen Netzwerk, das einer privaten Zone zugewiesen ist, gibt den FQDN zurück, der den Host-/Datensatznamen sowie den Zonenname als Suffix enthält. 


## <a name="limitations"></a>Einschränkungen
* Ein virtuelles Registrierungsnetzwerk pro privater Zone.
* Bis zu zehn virtuelle Auflösungsnetzwerke pro privater Zone.
* Ein bestimmtes virtuelles Netzwerk kann nur mit genau einer privaten Zone als ein virtuelles Registrierungsnetzwerk verknüpft werden.
* Ein bestimmtes virtuelles Netzwerk kann mit bis zu zehn privaten Zonen als ein virtuelles Auflösungsnetzwerk verknüpft werden.
* Wenn ein virtuelles Registrierungsnetzwerk angegeben wird, können die DNS-Einträge für die virtuellen Computer aus diesem virtuellen Netzwerk, die für die private Zone registriert sind, nicht über Powershell/CLI/APIs angezeigt oder abgerufen werden, doch werden die Einträge der virtuellen Computer registriert und erfolgreich aufgelöst.
* Reverse-DNS funktioniert nur für den privaten IP-Bereich im virtuellen Registrierungsnetzwerk.
* Reverse-DNS für eine private IP-Adresse, die nicht in der privaten Zone registriert ist (beispielsweise eine private IP-Adresse für einen virtuellen Computer in einem virtuellen Netzwerk, das als ein virtuelles Auflösungsnetzwerk mit einer privaten Zone verknüpft ist), gibt „internal.cloudapp.net“ als DNS-Suffix zurück, doch kann dieses Suffix nicht aufgelöst werden.   
* Das virtuelle Netzwerk muss leer sein (d. h. keine VM-Datensätze) bei der anfänglichen (d. h. erstmaligen) Verknüpfung mit einer privaten Zone als virtuelles Registrierungs- oder Auflösungsnetzwerk. Das virtuelle Netzwerk braucht jedoch dann für zukünftige Verknüpfungen mit anderen privaten Zonen als virtuelles Registrierungs- oder Auflösungsnetzwerk nicht leer zu sein. 
* Derzeit wird die bedingte Weiterleitung nicht unterstützt, z. B. für eine Auflösung zwischen Azure und lokalen Netzwerken. Eine Dokumentation dazu, wie Kunden dieses Szenario mithilfe anderer Mechanismen umsetzen können, finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Außerdem sollten Sie [Häufig gestellte Fragen](./dns-faq.md#private-dns) für einige allgemeine Fragen und Antworten zu private Zonen in Azure DNS lesen, einschließlich des speziellen DNS-Registrierungs- und -Auflösungsverhaltens, das Sie für bestimmte Arten von Vorgängen erwarten können. 


## <a name="pricing"></a>Preise

Private DNS-Zonen sind während der öffentlichen Vorschau kostenlos. Während der allgemeinen Verfügbarkeit gilt für dieses Feature ein nutzungsbasiertes Preismodell, das dem vorhandenen Azure DNS-Angebot ähnelt. 


## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen einer privaten Zone in Azure DNS über [PowerShell](./private-dns-getstarted-powershell.md) oder [CLI](./private-dns-getstarted-cli.md).

Informieren Sie sich über einige häufige [Szenarien zu privaten Zonen](./private-dns-scenarios.md), die mit privaten Zonen in Azure DNS realisiert werden können.

Lesen Sie [Häufig gestellte Fragen](./dns-faq.md#private-dns) für einige allgemeine Fragen und Antworten zu private Zonen in Azure DNS, einschließlich des speziellen Verhaltens, das Sie für bestimmte Arten von Vorgängen erwarten können. 

Erfahren Sie mehr zu DNS-Zonen und -Einträgen im folgenden Artikel: [DNS-Zonen und -Einträge: Übersicht](dns-zones-records.md).

Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.

