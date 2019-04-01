---
title: Verwenden von Azure DNS für private Domänen
description: Übersicht über den privaten DNS-Hostingdienst in Microsoft Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 3/1/2019
ms.author: victorh
ms.openlocfilehash: 7f5f377f34a43dfb01ea516e023bb98f118d0dd4
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247221"
---
# <a name="use-azure-dns-for-private-domains"></a>Verwenden von Azure DNS für private Domänen

Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Dienstnamens in die IP-Adresse verantwortlich. Als Hostingdienst für DNS-Domänen bietet Azure DNS eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Zusätzlich zu DNS-Domänen mit Internetverbindung unterstützt Azure DNS als Vorschaufeature jetzt auch private DNS-Domänen.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Azure DNS bietet einen zuverlässigen, sicheren DNS-Dienst zum Verwalten und Auflösen von Domänennamen in einem virtuellen Netzwerk, ohne dass Sie eine benutzerdefinierte DNS-Lösung hinzufügen müssen. Durch die Verwendung privater DNS-Zonen können Sie anstelle der momentan verfügbaren von Azure bereitgestellten Namen Ihre eigenen benutzerdefinierten Domänennamen verwenden. Die Verwendung benutzerdefinierter Domänennamen erleichtert Ihnen die optimale Anpassung der Architektur Ihres virtuellen Netzwerks an die Anforderungen Ihres Unternehmens. Es wird eine Namensauflösung für virtuelle Computer in einem virtuellen Netzwerk und zwischen virtuellen Netzwerken bereitgestellt. Darüber hinaus können Sie Zonennamen mit einer Split-Horizon-Ansicht konfigurieren, sodass eine private und eine öffentliche DNS-Zone den Namen verwenden können.

Um eine private DNS-Zone in Ihrem virtuellen Netzwerk zu veröffentlichen, geben Sie die Liste mit den virtuellen Netzwerken an, für die das Auflösen von Einträgen in der Zone zulässig ist. Diese werden als *virtuelle Auflösungsnetzwerke* bezeichnet. Sie können auch ein virtuelles Netzwerk angeben, für das Azure DNS Hostnameneinträge verwaltet, wenn ein virtueller Computer erstellt wird, sich seine IP-Adresse ändert oder der virtuelle Computer gelöscht wird. Dies wird als *virtuelles Registrierungsnetzwerk* bezeichnet.

Wenn Sie ein virtuelles Netzwerk für die Registrierung angeben, können die DNS-Einträge für die VMs aus diesem virtuellen Netzwerk, die für die private Zone registriert sind, nicht über Azure PowerShell und Azure CLI-APIs angezeigt oder abgerufen werden. Die VM-Einträge werden tatsächlich jedoch registriert und erfolgreich aufgelöst.

![Übersicht über DNS](./media/private-dns-overview/scenario.png)

> [!NOTE]
> Als bewährte Methode sollten Sie keine Domäne vom Typ „.local“ für Ihre private DNS-Zone verwenden. Nicht alle Betriebssysteme unterstützen dies.

## <a name="benefits"></a>Vorteile

Azure DNS bietet folgende Vorteile:

* **Macht benutzerdefinierte DNS-Lösungen überflüssig**. Früher erstellten viele Kunden benutzerdefinierte DNS-Lösungen zum Verwalten von DNS-Zonen in ihrem virtuellen Netzwerk. Die Verwaltung von DNS-Zonen kann ab sofort mithilfe der nativen Azure-Infrastruktur durchgeführt werden, sodass das lästige Erstellen und Verwalten benutzerdefinierter DNS-Lösungen entfällt.

* **Verwenden aller gängigen DNS-Eintragstypen**. Azure DNS unterstützt A-, AAAA-, CNAME-, MX-, PTR-, SOA-, SRV- und TXT-Einträge.

* **Automatische Verwaltung von Hostnameneinträgen**. Parallel zum Hosten Ihrer benutzerdefinierten DNS-Einträge verwaltet Azure automatisch Hostnameneinträge für die virtuellen Computer in den angegebenen virtuellen Netzwerken. In diesem Szenario können Sie die verwendeten Domänennamen optimieren, ohne benutzerdefinierte DNS-Lösungen erstellen oder die Anwendung ändern zu müssen.

* **Hostnamenauflösung zwischen virtuellen Netzwerken**. Im Gegensatz zu von Azure bereitgestellten Hostnamen können private DNS-Zonen von virtuellen Netzwerken gemeinsam genutzt werden. Diese Möglichkeit vereinfacht netzwerkübergreifende und Dienstermittlungsszenarien wie z.B. das Peering virtueller Netzwerke.

* **Vertraute Tools und Benutzerfreundlichkeit**. Zur Reduzierung der Lernkurve verwendet dieses neue Angebot die bewährten Azure DNS-Tools (PowerShell, Azure Resource Manager-Vorlagen, REST-API).

* **Split-Horizon-DNS-Unterstützung**. Mit Azure DNS können Sie Zonen mit gleichem Namen erstellen, die innerhalb eines virtuellen Netzwerks und im öffentlichen Internet unterschiedlich aufgelöst werden. Ein typisches Szenario für Split-Horizon-DNS ist die Bereitstellung einer bestimmte Version eines Diensts für die Verwendung in Ihrem virtuellen Netzwerk .

* **In allen Azure-Regionen verfügbar**. Das Azure DNS-Feature für private Zonen ist in allen Azure-Regionen in der öffentlichen Azure-Cloud verfügbar.

## <a name="capabilities"></a>Funktionen

Azure DNS bietet die folgenden Funktionen:

* **Automatische Registrierung von virtuellen Computern aus einem einzelnen virtuellen Netzwerk, das einer privaten Zone als virtuelles Netzwerk für die Registrierung zugeordnet ist**. Die virtuellen Computer werden für die private Zone als A-Einträge registriert (hinzugefügt), die auf die zugehörigen privaten IP-Adressen verweisen. Beim Löschen eines virtuellen Computers in einem virtuellen Netzwerk für die Registrierung entfernt Azure automatisch auch den entsprechenden DNS-Eintrag aus der verknüpften privaten Zone. 

  Standardmäßig fungiert ein virtuelles Netzwerk für die Registrierung insofern auch als virtuelles Netzwerk für die Auflösung, als dass die DNS-Auflösung für die Zone über jeden virtuellen Computer im virtuellen Netzwerk für die Registrierung funktioniert.

  > [!NOTE]
  > Wenn Sie ein virtuelles Netzwerk für die Registrierung angeben, können die DNS-Einträge für die VMs aus diesem virtuellen Netzwerk, die für die private Zone registriert sind, nicht über Azure PowerShell und Azure CLI-APIs angezeigt oder abgerufen werden. Die VM-Einträge werden tatsächlich jedoch registriert und erfolgreich aufgelöst.

* **Die Forward-DNS-Auflösung wird über virtuelle Netzwerke hinweg unterstützt, die mit der privaten Zone als virtuelles Netzwerk für die Auflösung verknüpft sind**. Für die DNS-Auflösung über virtuelle Netzwerke hinweg ist es nicht explizit erforderlich, ein Peering für die virtuellen Netzwerke zu konfigurieren. Möglicherweise möchten Kunden jedoch für andere Szenarien (z.B. HTTP-Datenverkehr) ein Peering für virtuelle Netzwerke einrichten.

* **Reverse-DNS-Lookups werden innerhalb des Bereichs eines virtuellen Netzwerks unterstützt**. Bei einem Reverse-DNS-Lookup für eine private IP-Adresse im virtuellen Netzwerk, das einer privaten Zone zugewiesen ist, wird der FQDN zurückgegeben, der den Host-/Eintragsnamen sowie den Zonennamen als Suffix enthält.

## <a name="limitations"></a>Einschränkungen

Für Azure DNS gelten die folgenden Einschränkungen:

* Für jede private Zone ist nur ein virtuelles Netzwerk für die Registrierung zulässig.
* Für jede private Zone sind bis zu 10 virtuelle Netzwerke für die Auflösung zulässig. Dieser Grenzwert wird entfernt, wenn dieses Feature allgemein verfügbar ist.
* Ein bestimmtes virtuelles Netzwerk kann nur mit genau einer privaten Zone als ein virtuelles Netzwerk für die Registrierung verknüpft werden.
* Ein bestimmtes virtuelles Netzwerk kann mit bis zu 10 privaten Zonen als virtuelles Netzwerk für die Auflösung verknüpft werden. Dieser Grenzwert wird entfernt, wenn dieses Feature allgemein verfügbar ist.
* Wenn Sie ein virtuelles Netzwerk für die Registrierung angeben, können die DNS-Einträge für die VMs aus diesem virtuellen Netzwerk, die für die private Zone registriert sind, nicht über Azure PowerShell und Azure CLI-APIs angezeigt oder abgerufen werden. Die VM-Einträge werden tatsächlich jedoch registriert und erfolgreich aufgelöst.
* Reverse-DNS funktioniert nur für den privaten IP-Bereich im virtuellen Netzwerk für die Registrierung.
* Ein Reverse-DNS-Lookup für eine private IP-Adresse, die nicht in der privaten Zone registriert ist (z.B. eine private IP-Adresse für eine VM in einem virtuellen Netzwerk, das als virtuelles Netzwerk für die Auflösung mit einer privaten Zone verknüpft ist), gibt *internal.cloudapp.net* als DNS-Suffix zurück. Dieses Suffix kann jedoch nicht aufgelöst werden.
* Das virtuelle Netzwerk muss vollständig leer sein, wenn Sie es zum ersten Mal als virtuelles Registrierungs- oder Auflösungsnetzwerk mit einer privaten Zone verknüpfen. Bei anschließenden Verknüpfungen mit anderen privaten Zonen als virtuelles Netzwerk für die Registrierung oder Auflösung ist es nicht erforderlich, dass das virtuelle Netzwerk leer ist.
* Derzeit wird die bedingte Weiterleitung nicht unterstützt (z.B. zum Aktivieren der Auflösung zwischen Azure und lokalen Netzwerken). Informationen dazu, wie Kunden dieses Szenario mithilfe anderer Mechanismen umsetzen können, finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Allgemeine Fragen und Antworten zu privaten Zonen in Azure DNS, einschließlich des spezifischen DNS-Registrierungs- und -Auflösungsverhaltens, finden Sie im Dokument [Häufig gestellte Fragen](./dns-faq.md#private-dns).  

## <a name="pricing"></a>Preise

Das Feature für private DNS-Zonen ist während der öffentlichen Vorschau kostenlos. Bei allgemeiner Verfügbarkeit gilt für dieses Feature ein nutzungsbasiertes Preismodell, das dem vorhandenen Azure DNS-Angebot ähnelt. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie über [Azure PowerShell](./private-dns-getstarted-powershell.md) oder [Azure CLI](./private-dns-getstarted-cli.md) eine private Zone in Azure DNS erstellen.

Informieren Sie sich über einige gängige [Szenarien zu privaten Zonen](./private-dns-scenarios.md), die mit privaten Zonen in Azure DNS realisiert werden können.

Allgemeine Fragen und Antworten zu privaten Zonen in Azure DNS, einschließlich des spezifischen Verhaltens für bestimmte Vorgänge, finden Sie im Dokument [Häufig gestellte Fragen](./dns-faq.md#private-dns). 

Weitere Informationen zu DNS-Zonen und -Einträgen finden Sie unter [Übersicht über DNS-Zonen und -Einträge](dns-zones-records.md).

Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.
