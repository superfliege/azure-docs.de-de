---
title: Was ist Azure DNS?
description: Übersicht über DNS-Hostingdienste in Microsoft Azure. Hosten Ihrer Domäne in Microsoft Azure.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 9535ad73300406408a94a6f38f001e45ba7409a2
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093094"
---
# <a name="what-is-azure-dns"></a>Was ist Azure DNS?

Azure DNS ist ein Hostingdienst für DNS-Domänen, der eine Namensauflösung mittels Microsoft Azure-Infrastruktur bietet. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge unter Verwendung der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung verwalten wie bei Ihren anderen Azure-Diensten.

Es ist nicht möglich, Azure DNS zum Erwerben eines Domänennamens zu verwenden. Ein Domänenname kann für eine Jahresgebühr über [Azure-Web-Apps](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) oder über die Domänennamen-Registrierungsstelle eines Drittanbieters erworben werden. Ihre Domänen können dann in Azure DNS für die Verwaltung von Einträgen gehostet werden. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md).

Azure DNS bietet folgende Features.

## <a name="reliability-and-performance"></a>Zuverlässigkeit und Leistung

DNS-Domänen in Azure DNS werden im globalen Azure-Netzwerk von DNS-Servern gehostet. Azure DNS verwendet Anycast-Netzwerke. Jede DNS-Abfrage wird vom jeweils nächstgelegenen verfügbaren DNS-Server beantwortet, um für Ihre Domäne eine hohe Leistung und Verfügbarkeit zu erzielen.

## <a name="security"></a>Sicherheit

 Azure DNS basiert auf Azure Resource Manager, wodurch beispielsweise folgende Features zur Verfügung stehen:

* [Rollenbasierte Zugriffssteuerung](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control), um zu steuern, wer Zugriff auf bestimmte Aktionen für Ihre Organisation hat.

* [Aktivitätsprotokolle](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs), um nachzuverfolgen, wie ein Benutzer in Ihrer Organisation eine Ressource geändert hat, oder um im Rahmen der Problembehandlung Fehler zu finden.

* [Ressourcensperre](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources), um ein Abonnement, eine Ressourcengruppe oder eine Ressource zu sperren. Eine Sperre verhindert, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern.

Weitere Informationen finden Sie unter [Schützen von DNS-Zonen und -Einträgen](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Einfache Bedienung

 Azure DNS kann DNS-Einträge für Ihre Azure-Dienste verwalten und außerdem DNS für Ihre externen Ressourcen bereitstellen. Azure DNS ist in das Azure-Portal integriert und verwendet die gleichen Anmeldeinformationen, den gleichen Supportvertrag und die gleiche Abrechnung wie Ihre anderen Azure-Dienste. 

Die DNS-Abrechnung basiert auf der Anzahl von DNS-Zonen, die in Azure gehostet werden, und der Anzahl von empfangenen DNS-Abfragen. Weitere Informationen zu Preisen finden Sie in den [Preisangaben zu Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

Ihre Domänen und Einträge können über das Azure-Portal, mithilfe von Azure PowerShell-Cmdlets sowie über die plattformübergreifende Azure-Befehlszeilenschnittstelle verwaltet werden. Anwendungen, die eine automatisierte DNS-Verwaltung erfordern, können über die REST-API und SDKs mit dem Dienst zusammenarbeiten.

## <a name="customizable-virtual-networks-with-private-domains"></a>Anpassbare virtuelle Netzwerke mit privaten Domänen

Azure DNS unterstützt auch private DNS-Domänen mit einem Feature, das sich jetzt in der Public Preview-Phase befindet. Mit diesem Feature können Sie in Ihren privaten virtuellen Netzwerken anstelle der derzeit von Azure bereitgestellten Namen Ihre eigenen benutzerdefinierten Domänennamen verwenden.

Weitere Informationen finden Sie unter [Verwenden von Azure DNS für private Domänen](private-dns-overview.md).

## <a name="alias-records"></a>Aliaseinträge

Azure DNS unterstützt Aliaseintragssätze. Sie können einen Aliaseintragssatz verwenden, um auf eine Azure-Ressource zu verweisen (beispielsweise eine öffentliche Azure-IP-Adresse oder ein Azure Traffic Manager-Profil). Wenn sich die IP-Adresse der zugrunde liegenden Ressource ändert, wird der Aliasdatensatz während der DNS-Auflösung nahtlos automatisch aktualisiert. Der Aliasdatensatz verweist auf die Dienstinstanz, und der Dienstinstanz ist eine IP-Adresse zugeordnet. 

Darüber hinaus kann Ihre Apex- oder Naked-Domäne jetzt über einen Aliaseintrag auf ein Traffic Manager-Profil verweisen. Ein Beispiel wäre etwa „contoso.com“.

Weitere Informationen finden Sie in der [Übersicht über Azure-DNS-Aliaseinträge](dns-alias.md).


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu DNS-Zonen und -Einträgen finden Sie in der [Übersicht über DNS-Zonen und -Einträge](dns-zones-records.md).

* Informationen zur Erstellung einer Zone in Azure DNS finden Sie unter [Erstellen einer DNS-Zone](./dns-getstarted-create-dnszone-portal.md).

* Häufig gestellte Fragen zu Azure DNS finden Sie unter der [Häufig gestellte Fragen zu Azure DNS](dns-faq.md).

