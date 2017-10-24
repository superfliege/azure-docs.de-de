---
title: "Verwenden von Azure DNS für private Domänen | Microsoft-Dokumentation"
description: "Übersicht über private DNS-Hostingdienste in Microsoft Azure."
services: dns
documentationcenter: na
author: garbrad
manager: 
editor: 
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2017
ms.author: garbrad
ms.openlocfilehash: ed47a9d850995aaf9e78bfde8b6a5fd80dc36918
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="using-azure-dns-for-private-domains"></a>Verwenden von Azure DNS für private Domänen
Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Dienstnamens in die IP-Adresse verantwortlich. Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt.  Zusätzlich zu DNS-Domänen mit Internetverbindung unterstützt Azure DNS jetzt auch private DNS-Domänen als Vorschaufunktion.  
 
Azure DNS bietet einen zuverlässigen, sicheren DNS-Dienst zum Verwalten und Auflösen von Domänennamen in einem virtuellen Netzwerk, ohne dass Sie eine benutzerdefinierte DNS-Lösung hinzufügen müssen. In privaten DNS-Zonen können Sie anstatt der heute verfügbaren von Azure bereitgestellten Namen Ihre eigenen benutzerdefinierten Domänennamen verwenden.  Die Verwendung benutzerdefinierter Domänennamen erleichtert Ihnen die optimale Anpassung der Architektur Ihres virtuellen Netzwerks an die Anforderungen Ihres Unternehmens. Sie bietet Namensauflösung für virtuelle Computer in einem virtuellen Netzwerk und zwischen virtuellen Netzwerken. Darüber hinaus können Sie Zonennamen mit einer Split-Horizon-Ansicht konfigurieren, sodass eine private und eine öffentliche DNS-Zone den gleichen Namen verwenden können.

![Übersicht über DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="benefits"></a>Vorteile

* **Macht benutzerdefinierte DNS-Lösungen überflüssig.** Früher erstellten viele Kunden benutzerdefinierte DNS-Lösungen zum Verwalten von DNS-Zonen in ihrem virtuellen Netzwerk.  Die Verwaltung von DNS-Zonen kann jetzt mithilfe der nativen Azure-Infrastruktur durchgeführt werden, sodass das lästige Erstellen und Verwalten benutzerdefinierter DNS-Lösungen entfällt.

* **Verwenden aller gängigen DNS-Eintragstypen.**  Azure DNS unterstützt A-, AAAA-, CNAME-, MX-, NS-, PTR-, SOA-, SRV- und TXT-Einträge.

* **Automatische Hostnameneintrags-Verwaltung.** Parallel zum Hosten Ihrer benutzerdefinierten DNS-Einträge verwaltet Azure automatisch Hostnameneinträge für die virtuellen Computer in den angegebenen virtuellen Netzwerken.  So können Sie die Domänennamen optimieren, die Sie verwenden, ohne benutzerdefinierte DNS-Lösungen erstellen oder die Anwendung ändern zu müssen.

* **Hostnamenauflösung zwischen virtuellen Netzwerken.** Im Gegensatz zu von Azure bereitgestellten Hostnamen können private DNS-Zonen von virtuellen Netzwerken gemeinsam genutzt werden.  Diese Möglichkeit vereinfacht netzwerkübergreifende und Dienstermittlungsszenarien wie das Peering virtueller Netzwerke.

* **Vertraute Tools und Benutzerfreundlichkeit.** Zur Reduzierung der Lernkurve verwendet dieses neue Angebot die bereits gut eingeführten Azure DNS-Tools (PowerShell Resource Manager-Vorlagen, REST-API), und die Unterstützung von CLI und Portal folgt so bald wie möglich.

* **Split-Horizon-DNS-Unterstützung.** Azure DNS ermöglicht Ihnen, Zonen mit gleichem Namen zu erstellen, die innerhalb eines virtuellen Netzwerks und im öffentlichen Internet unterschiedlich aufgelöst werden.  Ein typisches Szenario für Split-Horizon-DNS ist die Bereitstellung einer bestimmte Version eines Diensts für die Verwendung in Ihrem virtuellen Netzwerk .


## <a name="pricing"></a>Preise

Private DNS-Zonen sind während der verwalteten Vorschau kostenlos. Bei Ankündigung der allgemeinen Verfügbarkeit gilt für dieses Feature ein nutzungsbasiertes Preismodell, das dem vorhandenen Azure DNS-Angebot ähnelt. 


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie in Azure DNS [eine private DNS-Zone erstellen](./private-dns-getstarted-powershell.md).

Erfahren Sie mehr zu DNS-Zonen und -Einträgen im folgenden Artikel: [DNS-Zonen und -Einträge: Übersicht](dns-zones-records.md).

Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.

