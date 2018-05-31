---
title: DNS in Azure Stack | Microsoft-Dokumentation
description: Verwenden von DNS in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: mabrigg
ms.openlocfilehash: 4e854a2751ce366e3ca3a353487f2c972401c248
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34196524"
---
# <a name="using-dns-in-azure-stack"></a>Verwenden von DNS in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack unterstützt die folgenden Domain Name System-Features (DNS):

* DNS-Hostnamenauflösung
* Erstellen und Verwalten von DNS-Zonen und -Einträgen mithilfe der API

## <a name="support-for-dns-hostname-resolution"></a>Unterstützung für DNS-Hostnamenauflösung

Sie können eine DNS-Domänennamenbezeichnung für öffentliche IP-Ressourcen angeben. Azure Stack verwendet „*domainnamelabel.location*.cloudapp.azurestack.external“ für den Bezeichnungsnamen und nimmt die Zuordnung zur öffentlichen IP-Adresse in von Azure Stack verwalteten DNS-Servern vor.

Wenn Sie beispielsweise eine öffentliche IP-Ressource mit **contoso** als Domänennamenbezeichnung im Speicherort „Lokaler Azure Stack“ erstellen, wird der [vollqualifizierte Domänenname](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN) **contoso.local.cloudapp.azurestack.external** in die öffentliche IP-Adresse der Ressource aufgelöst. Mit diesem FQDN können Sie einen benutzerdefinierten CNAME-Domäneneintrag erstellen, der auf die öffentliche IP-Adresse in Azure Stack verweist.

Weitere Informationen zur Namensauflösung finden Sie im Artikel [DNS-Auflösung](https://docs.microsoft.com/en-us/azure/dns/dns-for-azure-services?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Jede erstellte Domänennamenbezeichnung muss für den Azure Stack-Speicherort eindeutig sein.

Die nächste Bildschirmaufnahme zeigt das Dialogfeld **Öffentliche IP-Adresse erstellen** zum Erstellen einer öffentlichen IP-Adresse mithilfe des Portals.

![Erstellen einer öffentlichen IP-Adresse](media/azure-stack-whats-new-dns/image01.png)

**Beispielszenario**

Sie verfügen über einen Lastenausgleich, der Anforderungen einer Webanwendung verarbeitet. Im Hintergrund des Lastenausgleichs befindet sich eine Website, die auf einem oder mehreren virtuellen Computern ausgeführt wird. Jetzt können Sie auf die Website mit Lastenausgleich mit einem DNS-Namen zugreifen, statt mit einer IP-Adresse.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>Erstellen und Verwalten von DNS-Zonen und -Einträgen mit der API

Sie können DNS-Zonen und -Einträge in Azure Stack erstellen und verwalten.

Azure Stack stellt einen DNS-Dienst wie unter Azure bereit, und es werden APIs verwendet, die mit den DNS-APIs von Azure konsistent sind.  Durch das Hosten Ihrer Domänen in Azure Stack-DNS können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs und Tools verwalten. Sie können auch die gleiche Abrechnung und den gleichen Support wie für die anderen Azure-Dienste verwenden.

Die Azure Stack-DNS-Infrastruktur ist kompakter als die von Azure. Die Größe und der Speicherort einer Azure Stack-Bereitstellung wirken sich auf den Bereich, den Umfang und die Leistung von DNS aus. Dies bedeutet auch, dass Leistung, Verfügbarkeit, globale Verteilung und Hochverfügbarkeit von Bereitstellung zu Bereitstellung variieren können.

## <a name="comparison-with-azure-dns"></a>Vergleich mit dem Azure-DNS

DNS in Azure Stack ist DNS in Azure ähnlich, aber es gibt wichtige Ausnahmen, die Sie kennen sollten.

* **AAAA-Einträge werden nicht unterstützt**

    Azure Stack unterstützt KEINE AAAA-Einträge, weil Azure Stack keine Unterstützung von IPv6-Adressen aufweist.  Dies ist ein wichtiger Unterschied zwischen DNS in Azure und Azure Stack.
* **Nicht mehrinstanzenfähig**

    Der DNS-Dienst in Azure Stack ist nicht mehrinstanzenfähig. Es ist also nicht möglich, dass für jeden Mandanten die gleiche DNS-Zone erstellt wird. Dies ist nur für das erste Abonnement erfolgreich, mit dem versucht wird, die Zone zu erstellen. Nachfolgende Anforderungen schlagen fehl.  Es handelt sich hierbei um ein bekanntes Problem und einen wichtigen Unterschied zwischen Azure und dem Azure Stack-DNS. Dieses Problem wird in einer zukünftigen Version behoben sein.
* **Tags, Metadaten und ETags**

    Außerdem gibt es kleinere Unterschiede in Bezug darauf, wie Azure Stack Tags, Metadaten, ETags und Grenzwerte verarbeitet.

Weitere Informationen zu Azure DNS finden Sie unter [DNS-Zonen und -Einträge](../../dns/dns-zones-records.md).

### <a name="tags-metadata-and-etags"></a>Tags, Metadaten und ETags

**Tags**

Das Azure Stack-DNS unterstützt die Verwendung von Azure Resource Manager-Tags in DNS-Zonenressourcen. Tags für DNS-Ressourceneintragssätze werden nicht unterstützt, aber als Alternative werden „Metadaten“ für DNS-Ressourceneintragssätze wie unten beschrieben unterstützt.

**Metadaten**

Das Azure Stack-DNS unterstützt das Hinzufügen von Ressourceneintragssätzen mithilfe von „Metadaten“ als Alternative zu Tags in Ressourceneintragssätzen. Ähnlich wie Tags ermöglichen Metadaten das Zuordnen von Name-Wert-Paare zu jedem Ressourceneintragssatz. Dies kann beispielsweise hilfreich sein, um den Zweck jedes Ressourceneintragssatzes zu dokumentieren. Anders als Tags können Metadaten nicht verwendet werden, um Ihre Azure-Rechnung zu filtern, und sie können nicht in einer Richtlinie von Azure Resource Manager angegeben werden.

**ETags**

Angenommen, zwei Personen oder zwei Prozesse versuchen, einen DNS-Eintrag zur gleichen Zeit zu ändern. Welcher hat Vorrang? Und weiß die Person, die Vorrang erhalten hat, dass sie die Änderungen einer anderen Person überschrieben hat?

Das Azure Stack-DNS verwendet ETags, um gleichzeitige Änderungen an derselben Ressource sicher verarbeiten zu können. ETags unterscheiden sich von Azure Resource Manager-Tags. Jeder DNS-Ressourcen (Zone oder Datensatzgruppe) ist ein ETag zugeordnet. Sobald eine Ressource abgerufen wird, wird auch sein ETag abgerufen. Beim Aktualisieren einer Ressource können Sie auswählen, dass die ETags wieder zurückgegeben werden sollen, damit das Azure Stack-DNS überprüfen kann, ob das ETag auf dem Server übereinstimmt. Da jedes Update einer Ressource dazu führt, dass das ETag erneut generiert wird, weist eine Nichtübereinstimmung des ETags darauf hin, dass eine gleichzeitige Änderung vorgenommen wurde. ETags können auch beim Erstellen einer neuen Ressource verwendet werden, um sicherzustellen, dass die Ressource nicht bereits vorhanden ist.

Standardmäßig verwendet das PowerShell-Modul des Azure Stack-DNS ETags, um gleichzeitige Änderungen an Zonen und Ressourceneintragssätzen zu blockieren. Der optionale Switch *-Overwrite* kann verwendet werden, um ETag-Überprüfungen zu unterdrücken. In diesem Fall werden gleichzeitig vorgenommene Änderungen überschrieben.

Auf Ebene der REST-API des Azure Stack-DNS werden ETags mithilfe von HTTP-Headern angegeben. Ihr Verhalten ist in der folgenden Tabelle angegeben:

| Header | Verhalten|
|--------|---------|
| Keine   | PUT ist immer erfolgreich (keine Etag-Prüfung)|
| If-match| PUT ist nur erfolgreich, wenn die Ressource vorhanden ist und das Etag übereinstimmt.|
| If-match *| PUT ist nur erfolgreich, wenn eine Ressource vorhanden ist.|
| If-none-match *| PUT ist nur erfolgreich, wenn die Ressource nicht vorhanden ist.|

### <a name="limits"></a>Einschränkungen

Bei Verwendung des Azure Stack-DNS gelten folgende Standardgrenzwerte:

| Ressource| Standardlimit|
|---------|--------------|
| Zonen pro Abonnement| 100|
| Datensätze pro Zone| 5.000|
| Datensätze pro Datensatzgruppe| 20|

## <a name="next-steps"></a>Nächste Schritte

[Introducing iDNS for Azure Stack](azure-stack-understanding-dns.md) (Einführung in iDNS für Azure Stack)
