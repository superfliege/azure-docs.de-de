---
title: Includedatei
description: Includedatei
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 10/20/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: e33871f35613fbd5cdc5bf3162855b942056807f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50254515"
---
### <a name="what-is-azure-firewall"></a>Was ist Azure Firewall?

Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall als ein Dienst mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit. Sie können Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren.

### <a name="what-capabilities-are-supported-in-azure-firewall"></a>Welche Funktionen werden in Azure Firewall unterstützt?

* Zustandsbehaftete Firewall als Dienst
* Integrierte Hochverfügbarkeit mit uneingeschränkter Cloudskalierbarkeit
* FQDN-Filterung
* FQDN-Tags
* Filterregeln für den Netzwerkdatenverkehr
* SNAT-Unterstützung für ausgehenden Datenverkehr
* DNAT-Unterstützung für eingehenden Datenverkehr
* Zentrale Erstellung, Erzwingung und Protokollierung der Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und VNETs
* Vollständige Integration in Azure Monitor für Protokollierung und Analysen

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Was ist das typische Bereitstellungsmodell für Azure Firewall?

Sie können Azure Firewall in einem virtuellen Netzwerk bereitstellen, Kunden stellen Azure Firewall jedoch in der Regel in einem zentralen virtuellen Netzwerk bereit und verbinden andere virtuelle Netzwerke in einem Hub-and-Spoke-Modell damit. Sie können die Standardroute von den verbundenen virtuellen Netzwerken anschließend so festlegen, dass sie auf dieses zentrale virtuelle Firewall-Netzwerk verweist.

### <a name="how-can-i-install-the-azure-firewall"></a>Wie kann ich Azure Firewall installieren?

Sie können Azure Firewall über das Azure-Portal, PowerShell, die REST-API oder Vorlagen einrichten. Schrittanleitungen finden Sie im [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](../articles/firewall/tutorial-firewall-deploy-portal.md).

### <a name="what-are-some-azure-firewall-concepts"></a>Wie lauten einige der Azure Firewall-Konzepte?

Azure Firewall unterstützt-Regeln und Regelsammlungen. Eine Regelsammlung ist eine Liste von Regeln mit gleicher Reihenfolge und Priorität. Regelsammlungen werden in der Reihenfolge ihrer Priorität ausgeführt. Netzwerkregelsammlungen haben höhere Priorität als Anwendungsregelsammlungen, und alle Regeln werden beendet.

Es gibt zwei Arten von Regelsammlungen:

* *Anwendungsregeln*: Ermöglichen Ihnen das Konfigurieren vollqualifizierter Domänennamen (Fully Qualified Domain Names, FQDNs), auf die von einem Subnetz aus zugegriffen werden kann.
* *Netzwerkregeln*: Ermöglichen Ihnen das Konfigurieren von Regeln mit Quelladressen, Protokollen, Zielports und Zieladressen.

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Unterstützt Azure Firewall das Filtern des eingehenden Datenverkehrs?

Azure Firewall unterstützt Filter für eingehenden und ausgehenden Datenverkehr. Der Schutz des eingehenden Datenverkehrs ist für Nicht-HTTP/S-Protokolle bestimmt, z. B. RDP-, SSH- und FTP-Protokolle.

### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Welche Protokollierungs- und Analysedienste werden von Azure Firewall unterstützt?

Azure Firewall ist zum Anzeigen und Analysieren von Firewallprotokollen in Azure Monitor integriert. Protokolle können an Log Analytics, Azure Storage oder Event Hubs gesendet werden. Sie können in Log Analytics oder von anderen Tools, wie Excel und Power BI, analysiert werden. Weitere Informationen finden Sie unter [Tutorial: Überwachen von Azure Firewall-Protokollen](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Wie unterscheidet sich die Funktionsweise von Azure Firewall von vorhandenen Diensten wie NVAs im Marketplace?

Azure Firewall ist ein grundlegender Firewalldienst für verschiedene Kundenszenarios. Es wird erwartet, dass Sie über eine Mischung aus Drittanbieter-NVAs und Azure Firewall verfügen. Eine bessere Zusammenarbeit hat höchste Priorität.

### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Was ist der Unterschied zwischen Application Gateway-WAF und Azure Firewall?

Die Web Application Firewall (WAF) ist ein Feature von Application Gateway, das zentralisierten Schutz des eingehenden Datenverkehrs für Ihre Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken bietet. Azure Firewall bietet Schutz für eingehenden Datenverkehr für Nicht-HTTP/S-Protokolle (z.B. RDP, SSH, FTP), Schutz auf Netzwerkebene für eingehenden Datenverkehr für alle Ports und Protokolle sowie Schutz auf Anwendungsebene für ausgehenden HTTP/S-Datenverkehr.

### <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Was ist der Unterschied zwischen Netzwerksicherheitsgruppen (Network Security Groups, NSGs) und Azure Firewall?

Der Azure Firewall-Dienst ergänzt die Funktionen der Netzwerksicherheitsgruppen. Zusammen bieten sie eine bessere „Defense-in-Depth“-Netzwerksicherheit. Netzwerksicherheitsgruppen bieten verteilte Datenverkehrsfilterung auf Netzwerkebene, um den Datenverkehr auf Ressourcen in virtuellen Netzwerken in jedem Abonnement zu beschränken. Azure Firewall ist eine vollständig zustandsbehaftete, zentrale Netzwerkfirewall als Dienst, die Schutz auf Netzwerk- und Anwendungsebene für verschiedene Abonnements und virtuelle Netzwerke bietet.

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Wie richte ich Azure Firewall für meine Dienstendpunkte ein?

Für den sicheren Zugriff auf PaaS-Dienste werden Dienstendpunkte empfohlen. Sie können Dienstendpunkte im Azure Firewall-Subnetz aktivieren und diese im verbundenen virtuellen Spoke-Netzwerk deaktivieren. Auf diese Weise profitieren Sie von beiden Features: von der Dienstendpunktsicherheit und der zentralen Protokollierung des gesamten Datenverkehrs.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Wie sieht das Preismodell für Azure Firewall aus?

Azure Firewall setzt sich aus festen und variablen Gebühren zusammen:

* Feste Gebühr: 1,25 USD/Firewall/Stunde
* Variable Gebühr: 0,03 USD/von der Firewall verarbeiteten GB (ein- oder ausgehend)

Für eine Firewall, deren Zuordnung aufgehoben wurde, fallen keine Kosten an.

Weitere Informationen finden Sie unter [Azure Firewall – Preise](https://azure.microsoft.com/pricing/details/azure-firewall/).

### <a name="how-can-i-stop-and-start-azure-firewall"></a>Wie kann ich Azure Firewall beenden und starten?

Sie können Azure PowerShell verwenden, um Methoden zuzuordnen und die Zuordnung dafür aufzuheben (*Zuordnen* und *Zuordnung aufheben*).

Beispiel: 

```azurepowershell
# Stop an exisitng firewall

$azfw = Get-AzureRmFirewall -Name "FW Name” -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzureRmFirewall -AzureFirewall $azfw
```

```azurepowershell
#Start a firewall

$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzureRmPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzureRmFirewall -AzureFirewall $azfw
```

> [!NOTE]
> Sie müssen der ursprünglichen Ressourcengruppe und dem Abonnement eine Firewall und eine öffentliche IP-Adresse neu zuordnen.

### <a name="what-are-the-known-service-limits"></a>Welche Dienstgrenzwerte sind bekannt?

Die Einschränkungen des Azure Firewall-Diensts finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md#azure-firewall-limits).

### <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Kann Azure Firewall in einem virtuellen Hubnetzwerk den Netzwerkdatenverkehr zwischen zwei virtuellen Spoke-Netzwerken weiterleiten und filtern?

Ja, Sie können Azure Firewall in einem virtuellen Hubnetzwerk verwenden, um den Datenverkehr zwischen zwei virtuellen Spoke-Netzwerken weiterzuleiten und zu filtern. Subnetze in jedem der virtuellen Spoke-Netzwerke müssen über eine UDR verfügen, die auf die Azure Firewall als Standardgateway verweist, damit dieses Szenario ordnungsgemäß funktioniert.

### <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network"></a>Kann Azure Firewall den Netzwerkdatenverkehr zwischen Subnetzen im selben virtuellen Netzwerk weiterleiten und filtern?

Datenverkehr zwischen Subnetzen im selben virtuellen Netzwerk oder in einem direkt mittels Peering verknüpften virtuellen Netzwerk wird direkt weitergeleitet, selbst wenn die UDR auf Azure Firewall als Standardgateway verweist. Die empfohlene Methode für die interne Netzwerksegmentierung ist die Verwendung von Netzwerksicherheitsgruppen. Um in diesem Szenario Subnetz-zu-Subnetz-Datenverkehr an die Firewall zu senden, muss die UDR explizit den Zielsubnetzwerk-Präfix in beiden Subnetzen enthalten.

### <a name="are-there-any-firewall-resource-group-restrictions"></a>Gibt es Einschränkungen bei Ressourcengruppen?

Ja. Firewall, Subnetz, VNET und die öffentliche IP-Adresse müssen sich in der gleichen Ressourcengruppe befinden.
