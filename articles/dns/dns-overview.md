---
title: Was ist Azure DNS?
description: Übersicht über DNS-Hostingdienste in Microsoft Azure. Hosten Ihrer Domäne in Microsoft Azure.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: f68281f71ed525798410fb80b0d0175d38050c40
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829918"
---
# <a name="what-is-azure-dns"></a>Was ist Azure DNS?

Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.

Es ist nicht möglich, Azure DNS zum Erwerben eines Domänennamens zu verwenden. Für eine jährliche Gebühr können Sie einen Domänennamen erwerben, indem Sie [Azure-Web-Apps](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) oder die Domänennamen-Registrierungsstelle eines Drittanbieters nutzen. Ihre Domänen können dann in Azure DNS für die Verwaltung von Einträgen gehostet werden. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md) .

Die folgenden Features sind in Azure DNS enthalten:

## <a name="reliability-and-performance"></a>Zuverlässigkeit und Leistung

DNS-Domänen in Azure DNS werden im globalen Azure-Netzwerk von DNS-Servern gehostet. Für Azure DNS werden Anycast-Netzwerke verwendet, sodass jede DNS-Abfrage jeweils vom am nächsten liegenden verfügbaren DNS-Server beantwortet wird. So erzielen Sie für Ihre Domäne sowohl eine schnelle Leistung als auch eine hohe Verfügbarkeit.

## <a name="security"></a>Sicherheit

Der Azure DNS-Dienst basiert auf Azure Resource Manager und verfügt beispielsweise über die folgenden Features:

* [Rollenbasierte Zugriffssteuerung](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control): Hiermit können Sie steuern, wer Zugriff auf spezielle Aktionen für Ihre Organisation hat.

* [Aktivitätsprotokolle](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs): Dienen zum Nachverfolgen, welche Änderungen an einer Ressource ein Benutzer in Ihrer Organisation vorgenommen hat, oder zum Ermitteln eines Fehlers während der Problembehandlung.

* [Ressourcensperren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources): Sie können ein Abonnement, eine Ressourcengruppe oder eine Ressource sperren, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern.

Weitere Informationen finden Sie unter [Schützen von DNS-Zonen und -Einträgen](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Einfache Bedienung

Der Azure DNS-Dienst kann DNS-Einträge für Ihre Azure-Dienste verwalten und außerdem DNS für Ihre externen Ressourcen bereitstellen. Azure DNS ist in das Azure-Portal integriert und verwendet die gleichen Anmeldeinformationen, den gleichen Supportvertrag und die gleiche Abrechnung wie Ihre anderen Azure-Dienste. 

Die DNS-Abrechnung basiert auf der Anzahl von DNS-Zonen, die in Azure gehostet werden, und der Anzahl von empfangenen DNS-Abfragen. Weitere Informationen zu Preisen finden Sie in den [Preisangaben zu Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

Ihre Domänen und Einträge können über das Azure-Portal, Azure PowerShell-Cmdlets und die plattformübergreifende Azure-Befehlszeilenschnittstelle verwaltet werden. Anwendungen, für die eine automatisierte DNS-Verwaltung erforderlich ist, können über die REST-API und SDKs mit dem Dienst zusammenarbeiten.

## <a name="customizable-virtual-networks-with-private-domains"></a>Anpassbare virtuelle Netzwerke mit privaten Domänen

Azure DNS unterstützt auch private DNS-Domänen (jetzt in der öffentlichen Vorschauphase). Sie können also Ihre eigenen benutzerdefinierten Domänennamen in Ihren privaten virtuellen Netzwerken anstelle der von Azure bereitgestellten Namen verwenden, die derzeit verfügbar sind.

Weitere Informationen finden Sie unter [Verwenden von Azure DNS für private Domänen](private-dns-overview.md).

## <a name="alias-records"></a>Aliaseinträge

Azure DNS unterstützt Aliasdatensätze. Sie können einen Aliasdatensatz zum Verweisen auf eine Azure-Ressource, z.B. eine öffentliche Azure-IP-Adresse oder ein Traffic Manager-Profil, verwenden. Wenn sich die IP-Adresse der zugrunde liegenden Ressource ändert, wird der Aliasdatensatz während der DNS-Auflösung nahtlos automatisch aktualisiert. Der Aliasdatensatz verweist auf die Dienstinstanz, und der Dienstinstanz ist eine IP-Adresse zugeordnet. 

Darüber hinaus kann Ihre Apex- oder Naked-Domäne (z.B. contoso.com) jetzt auf ein Traffic Manager-Profil mit einem Aliaseintrag zeigen.

Weitere Informationen finden Sie in der [Übersicht über Azure-DNS-Aliaseinträge](dns-alias.md).


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu DNS-Zonen und -Einträgen: [Übersicht über DNS-Zonen und -Einträge](dns-zones-records.md).

* Informationen zur Erstellung einer Zone in Azure DNS: [Erstellen einer DNS-Zone](./dns-getstarted-create-dnszone-portal.md).

* Häufig gestellte Fragen zu Azure DNS finden Sie unter der [Häufig gestellte Fragen zu Azure DNS](dns-faq.md).

