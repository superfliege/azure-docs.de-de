---
title: Häufig gestellte Fragen zu Azure DNS | Microsoft-Dokumentation
description: Häufig gestellte Fragen zu Azure DNS
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: kumud
ms.openlocfilehash: e0eb39ced1d88d2e0b6128493304f112f9c685fa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31395229"
---
# <a name="azure-dns-faq"></a>Häufig gestellte Fragen zu Azure DNS

## <a name="about-azure-dns"></a>Informationen zu Azure DNS

### <a name="what-is-azure-dns"></a>Was ist Azure DNS?

Das Domain Name System (DNS) ist für die Übersetzung (oder Auflösung) eines Website- oder Dienstnamens in die IP-Adresse verantwortlich. Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.

DNS-Domänen in Azure DNS werden im globalen Azure-Netzwerk von DNS-Servern gehostet. Hierzu werden Anycast-Netzwerke verwendet, sodass jede DNS-Abfrage vom nächsten verfügbaren DNS-Server beantwortet wird. Azure DNS bietet sowohl schnelle Leistung als auch Hochverfügbarkeit für Ihre Domäne.

Der Azure DNS-Dienst basiert auf Azure Resource Manager. Daher profitiert er von Resource Manager-Funktionen, wie z.B. die rollenbasierte Zugriffskontrolle, Überwachungsprotokolle und Ressourcensperren. Ihren Domänen und Einträge können über das Azure-Portal, Azure PowerShell-Cmdlets und die plattformübergreifende Azure-Befehlszeilenschnittstelle verwaltet werden. Anwendungen, die eine automatische DNS-Verwaltung erfordern, können über die REST-API und SDKs mit dem Dienst zusammenarbeiten.

### <a name="how-much-does-azure-dns-cost"></a>Wie viel kostet Azure DNS?

Das Abrechnungsmodell von Azure DNS basiert auf der Anzahl von DNS-Zonen, die in Azure DNS gehostet werden, und der Anzahl der von ihnen empfangenen DNS-Abfragen. Rabatte werden basierend auf der Nutzung angeboten.

Weitere Informationen finden Sie auf der [Seite mit den Preisen von Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Wie lautet die Vereinbarung zum Servicelevel (SLA) für Azure DNS?

Azure garantiert für gültige DNS-Anforderungen, dass in 99,99 % der Fälle eine Antwort von mindestens einem Azure DNS-Namenserver empfangen wird.

Weitere Informationen finden Sie auf der [Seite mit der SLA für Azure DNS](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Was ist eine „DNS-Zone“? Ist das dasselbe wie eine DNS-Domäne? 

Eine Domäne ist ein eindeutiger Name im Domain Name System, z.B. „contoso.com“.


Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Beispiel: Die Domäne „contoso.com“ kann eine Reihe von DNS-Einträgen wie „mail.contoso.com“ (für einen E-Mail-Server) und „www.contoso.com“ (für eine Website) enthalten. Diese Einträge würden in der DNS-Zone „contoso.com“ gehostet werden.

Ein Domänenname ist *nur ein Name*, während eine DNS-Zone eine Datenressource ist, die die DNS-Einträge für einen Domänennamen enthält. Azure DNS ermöglicht es Ihnen, eine DNS-Zone zu hosten und die DNS-Einträge für eine Domäne in Azure zu verwalten. Darüber hinaus stellt der Dienst DNS-Namenserver bereit, um DNS-Abfragen aus dem Internet zu beantworten.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Muss ich einen DNS-Domänennamen erwerben, um Azure DNS nutzen zu können? 

Nicht unbedingt.

Sie müssen keine Domäne erwerben, um eine DNS-Zone in Azure DNS zu hosten. Sie können jederzeit eine DNS-Zone erstellen, ohne einen Domänennamen zu besitzen. DNS-Abfragen für diese Zone werden nur aufgelöst, wenn sie an den Azure DNS-Namenserver weitergeleitet werden, der der Zone zugewiesen ist.

Sie müssen einen Domänennamen erwerben, wenn Sie Ihre DNS-Zone in der globalen DNS-Hierarchie verknüpfen möchten. Durch diese Verknüpfung ist Ihre DNS-Zone für DNS-Abfragen auf der ganzen Welt auffindbar, und die Abfragen können mit Ihren DNS-Einträgen beantwortet werden.

## <a name="azure-dns-features"></a>Azure DNS-Features

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Unterstützt Azure DNS die Weiterleitung von Datenverkehr oder Endpunktfailover auf Basis von DNS?

Die Weiterleitung von Datenverkehr und Endpunktfailover auf Basis von DNS werden von Azure Traffic Manager bereitgestellt. Azure Traffic Manager ist ein separater Azure-Dienst, der zusammen mit Azure DNS verwendet werden kann. Weitere Informationen finden Sie in der [Traffic Manager-Übersicht](../traffic-manager/traffic-manager-overview.md).

Azure DNS unterstützt nur das Hosten von „statischen“ DNS-Domänen, in denen jede DNS-Abfrage für einen bestimmten DNS-Eintrag immer die gleiche DNS-Antwort erhält.

### <a name="does-azure-dns-support-domain-name-registration"></a>Unterstützt Azure DNS die Registrierung von Domänennamen?

Nein. Azure DNS unterstützt derzeit nicht den Kauf von Domänennamen. Wenn Sie Domänen erwerben möchten, müssen Sie eine von einem Drittanbieter angebotene Registrierungsstelle für Domänennamen verwenden. Die Registrierungsstelle erhebt in der Regel eine geringe jährliche Gebühr. Die Domänen können dann in Azure DNS für die Verwaltung von DNS-Einträgen gehostet werden. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md) .

Der Domänenkauf ist ein Feature, das im Azure-Backlog nachverfolgt wird. Sie können auf der Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>Unterstützt Azure DNS DNSSEC?

Nein. Azure DNS unterstützt derzeit DNSSEC nicht.

DNSSEC ist ein Feature, das im Azure DNS-Backlog nachverfolgt wird. Sie können auf der Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Unterstützt Azure DNS Zonenübertragungen (AXFR/IXFR)?

Nein. Azure DNS unterstützt derzeit Zonenübertragungen nicht. DNS-Zonen können [mithilfe der Azure CLI in Azure DNS importiert werden](dns-import-export.md). DNS-Einträge lassen sich dann über das [Azure DNS-Verwaltungsportal](dns-operations-recordsets-portal.md), unsere [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), das [SDK](dns-sdk.md), [PowerShell-Cmdlets](dns-operations-recordsets.md) oder das [CLI-Tool](dns-operations-recordsets-cli.md) verwalten.

Die Zonenübertragung ist ein Feature, das im Azure DNS-Backlog nachverfolgt wird. Sie können auf der Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Unterstützt Azure DNS URL-Umleitungen?

Nein. Dienste für URL-Umleitungen sind eigentlich keine DNS-Dienste – sie arbeiten auf der HTTP-Ebene, statt auf der DNS-Ebene. Bei einigen DNS-Anbietern gehört ein Dienst für URL-Umleitungen zum Gesamtangebot. Dies wird derzeit von Azure DNS nicht unterstützt.

Das Feature zur URL-Umleitung wird im Azure DNS-Backlog nachverfolgt. Sie können auf der Feedbackwebsite [Ihre Unterstützung für dieses Feature registrieren](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-extended-ascii-encoding-8-bit-set-for-txt-recordset-"></a>Unterstützt Azure DNS den erweiterten ASCII-Codierungssatz (8-Bit) für TXT-Recordsets?

Ja. Azure DNS unterstützt erweiterte ASCII-Codierung für TXT-Recordsets, wenn Sie die neueste Version der Azure-REST-APIs, SDKs, von PowerShell und CLI verwenden (Versionen vor 2017-10-01 oder SDK 2.1 unterstützen den erweiterten ASCII-Satz nicht). Wenn der Benutzer beispielsweise eine Zeichenfolge als Wert für einen TXT-Eintrag mit dem erweiterten ASCII-Zeichen „\128“ (z.B. abcd\128efgh) angibt, verwendet Azure DNS in interner Darstellung den Bytewert dieses Zeichens (der 128 entspricht). Zum Zeitpunkt der DNS-Auflösung wird dieser Bytewert ebenfalls in der Antwort zurückgegeben. Beachten Sie auch, dass „abc“ und „\097\098\099“, was die Auflösung betrifft, austauschbar sind. 

Wir befolgen für TXT-Einträge die Escaperegeln für das Zonendatei-Masterformat in [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt). Zum Beispiel fungiert „\“ jetzt tatsächlich laut RFC als Escapezeichen für alles. Wenn Sie „A\B“ als TXT-Eintragswert angeben, wird dieser nur als „AB“ dargestellt und aufgelöst. Wenn Sie wirklich möchten, dass der TXT-Eintrag in „A\B“ aufgelöst wird, müssen Sie „\"“ wieder mit dem Escapezeichen versehen, d.h. als „A\\B“ angeben. 

Beachten Sie, dass diese Unterstützung derzeit nicht für TXT-Einträge gilt, die im Azure-Portal erstellt wurden. 

## <a name="using-azure-dns"></a>Verwenden von Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Kann ich eine Domäne mit Azure DNS und einem anderen DNS-Anbieter hosten?

Ja. Azure DNS unterstützt das gemeinsame Hosten von Domänen mit anderen DNS-Diensten.

Dazu müssen Sie die NS-Einträge für die Domäne (die steuern, welche Anbieter DNS-Abfragen für die Domäne erhalten) so ändern, dass sie auf die Namenserver für beide Anbieter verweisen. Sie können diese NS-Einträge an drei Stellen ändern: in Azure DNS, beim anderen Anbieter und in der übergeordneten Zone (wird in der Regel über die Domänennamen-Registrierungsstelle konfiguriert). Weitere Informationen zur DNS-Delegierung finden Sie unter [Delegieren von Domänen](dns-domain-delegation.md).

Darüber hinaus müssen Sie sicherstellen, dass die DNS-Einträge für die Domäne zwischen beiden DNS-Anbietern synchron sind. Azure DNS unterstützt derzeit DNS-Zonenübertragungen nicht. DNS-Einträge müssen über das [Azure DNS-Verwaltungsportal](dns-operations-recordsets-portal.md), die [REST-API](https://docs.microsoft.com/powershell/module/azurerm.dns), das [SDK](dns-sdk.md), [PowerShell-Cmdlets](dns-operations-recordsets.md) oder das [CLI-Tool](dns-operations-recordsets-cli.md) synchronisiert werden.

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Muss ich meine Domäne an alle vier Azure DNS-Namenserver delegieren?

Ja. Azure DNS weist jeder DNS-Zone vier Namenserver zu. Dies dient der Isolierung von Fehlern und einer verbesserten Stabilität. Zur Qualifikation für die Azure DNS-SLA müssen Sie Ihre Domäne an alle vier Namenserver delegieren.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Welche Nutzungsgrenzwerte gelten für Azure DNS?

Bei der Verwendung von Azure DNS gelten folgende Standardgrenzwerte:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Kann ich eine Azure DNS-Zone zwischen Ressourcengruppen oder Abonnements verschieben?

Ja. DNS-Zonen können zwischen Ressourcengruppen oder Abonnements verschoben werden.

Es hat keine Auswirkungen auf DNS-Abfragen, wenn eine DNS-Zone verschoben wird. Die Namenserver, die der Zone zugewiesen sind, bleiben unverändert, und DNS-Abfragen werden als normaler Durchsatz verarbeitet.

Weitere Informationen und Anweisungen zum Verschieben von DNS-Zonen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Wie lange dauert es, bis DNS-Änderungen wirksam werden?

Neue DNS-Zonen und DNS-Einträge werden in der Regel schnell (innerhalb weniger Sekunden) auf den Azure DNS-Namenservern widergespiegelt.

Änderungen an vorhandenen DNS-Einträgen können etwas länger dauern, sollten aber dennoch innerhalb von 60 Sekunden auf den Azure DNS-Namenservern wiedergegeben werden. Dabei kann die DNS-Zwischenspeicherung außerhalb von Azure DNS (durch DNS-Clients und rekursive DNS-Auflösungsdienste) auch Einfluss auf die Zeit haben, bis eine DNS-Änderung wirksam wird. Diese Dauer der Zwischenspeicherung kann mithilfe der Eigenschaft für die Gültigkeitsdauer (Time To Live, TTL) der einzelnen Ressourceneintragssätze gesteuert werden.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Wie kann ich meine DNS-Zonen vor versehentlichem Löschen schützen?

Azure DNS wird mit Azure Resource Manager verwaltet und profitiert von Features für die Zugriffssteuerung, die Azure Resource Manager bereitstellt. Mit der rollenbasierten Zugriffssteuerung kann gesteuert werden, welche Benutzer über Lese- oder Schreibzugriff auf DNS-Zonen und Ressourceneintragssätze verfügen. Ressourcensperren können angewendet werden, um eine versehentliche Änderung oder Löschung von DNS-Zonen und Ressourceneintragssätzen zu verhindern.

Weitere Informationen finden Sie unter [Schützen von DNS-Zonen und -Einträgen](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Wie richte ich SPF-Datensätze in Azure DNS ein?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6"></a>Wird bei Azure DNS-Namenservern über IPv6 aufgelöst? 

Ja. Azure DNS-Namenserver basieren auf beiden Protokollen (besitzen sowohl IPv4- als auch IPv6-Adressen). Zum Ermitteln der IPv6-Adresse für die Azure DNS-Namenserver, die Ihrer DNS-Zone zugewiesen sind, können Sie ein Tool wie „nslookup“ verwenden (z. B. `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Wie richte ich einen internationalen Domänennamen (IDN) in Azure DNS ein?

Internationale Domänennamen (IDNs) funktionieren durch die Codierung der einzelnen DNS-Namen mit [Punycode](https://en.wikipedia.org/wiki/Punycode). DNS-Abfragen erfolgen mithilfe dieser mit Punycode codierten Namen.

Sie können internationale Domänennamen (IDNs) in Azure DNS konfigurieren, indem Sie zunächst den Zonennamen oder den Namen des Ressourceneintragssatzes in Punycode konvertieren. Azure DNS unterstützt die integrierte Konvertierung in/aus Punycode gegenwärtig nicht.

## <a name="private-dns"></a>Privates DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>Unterstützt Azure DNS „private“ Domänen?
Unterstützung für „private“ Domänen wird mithilfe des Features für private Zonen implementiert.  Dieses Feature ist derzeit in der Public Preview-Version verfügbar.  Private Zonen werden mit den gleichen Tools wie Azure DNS-Zonen mit Internetverbindung verwaltet, aber sie können nur innerhalb der von Ihnen angegebenen virtuellen Netzwerke aufgelöst werden.  Weitere Informationen finden Sie in der [Übersicht](private-dns-overview.md).

Derzeit werden private Zonen im Azure-Portal nicht unterstützt. 

Informationen zu anderen internen DNS-Optionen in Azure finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="what-is-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>Was ist der Unterschied zwischen einem virtuellen Registrierungsnetzwerk und einem virtuellen Auflösungsnetzwerk im Kontext von privaten Zonen? 
Sie können virtuelle Netzwerke auf zwei Arten mit einer privaten DNS-Zone verknüpfen: als ein virtuelles Registrierungsnetzwerk oder als ein virtuelles Auflösungsnetzwerk. In beiden Fällen können virtuelle Computer im virtuellen Netzwerk eine erfolgreiche Auflösung anhand von Einträgen in der privaten Zone vornehmen. Wenn Sie ein virtuelles Netzwerk als ein virtuelles Registrierungsnetzwerk angeben, registriert Azure automatisch (dynamische Registrierung) DNS-Einträge in der Zone für die virtuellen Computer im virtuellen Netzwerk. Darüber hinaus wird beim Löschen eines virtuellen Computers in einem virtuellen Registrierungsnetzwerk von Azure automatisch auch der entsprechende DNS-Eintrag aus der verknüpften privaten Zone entfernt. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Funktioniert Azure DNS Private Zones übergreifend in allen Azure-Regionen?
Ja. Private Zonen werden für die DNS-Auflösung zwischen virtuellen Netzwerken in allen Azure-Regionen auch ohne explizites Peering der virtuellen Netzwerke unterstützt, solange alle virtuellen Netzwerke als virtuelle Lösungsnetzwerke für die private Zone angegeben sind. Kunden benötigen möglicherweise ein Peering der virtuellen Netzwerke, damit der TCP/HTTP-Datenverkehr von einer Region zu einer anderen weitergeleitet werden kann. 

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Ist für private Zonen eine Verbindung mit dem Internet aus virtuellen Netzwerken erforderlich?
Nein. Private Zonen werden in Verbindung mit virtuellen Netzwerken verwendet und ermöglichen Kunden die Verwaltung von Domänen für virtuelle Computer oder andere Ressourcen innerhalb und übergreifend für virtuelle Netzwerke. Für die Namensauflösung ist keine Internetverbindung erforderlich. 

### <a name="can-the-same-private-zone-be-used-for-multiple-virtual-networks-for-resolution"></a>Kann die gleiche private Zone für mehrere virtuelle Netzwerke zur Auflösung verwendet werden? 
Ja. Kunden können einer einzelnen privaten Zone bis zu zehn virtuelle Auflösungsnetzwerke zuordnen.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Kann ein virtuelles Netzwerk, das zu einem anderen Abonnement gehört, einer privaten Zone als virtuelles Auflösungsnetzwerk hinzugefügt werden? 
Ja, solange der Benutzer über Schreibberechtigung sowohl für die virtuellen Netzwerke als auch die private DNS-Zone verfügt. Beachten Sie, dass die Schreibberechtigung mehreren RBAC-Rollen zugeordnet sein kann. Beispielsweise weist die RBAC-Rolle „Mitwirkender von klassischem Netzwerk“ Schreibberechtigungen für virtuelle Netzwerke auf. Weitere Informationen zu RBAC-Rollen finden Sie unter [Rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md).

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>Werden die automatisch registrierten DNS-Einträge virtueller Computer in einer privaten Zone automatisch gelöscht, wenn die virtuellen Computer vom Kunden gelöscht werden?
Ja. Wenn Sie einen virtuellen Computer innerhalb eines virtuellen Registrierungsnetzwerks löschen, löschen wir automatisch die DNS-Einträge, die in der Zone aufgrund der Tatsache registriert wurden, dass es sich um ein virtuelles Registrierungsnetzwerk handelt. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>Kann ein automatisch registrierter Eintrag für einen virtuellen Computer in einer privaten Zone (von einem virtuellen Registrierungsnetzwerk) manuell gelöscht werden? 
Nein. Derzeit können die DNS-Einträge virtueller Computer, die in einer privaten Zone von einem virtuellen Registrierungsnetzwerk automatisch registriert werden, von Kunden nicht angezeigt oder bearbeitet werden. Sie können jedoch solche automatisch registrierten DNS-Einträge durch einen manuell erstellten DNS-Eintrag in der Zone ersetzen (überschreiben). Informationen dazu finden Sie in der folgenden Frage und Antwort zu diesem Thema.

### <a name="what-happens-when-we-attempt-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>Was geschieht bei dem Versuch, einen neuen DNS-Eintrag manuell in einer privaten Zone zu erstellen, die den gleichen Hostnamen wie ein (automatisch registrierter) vorhandener virtueller Computer in einem virtuellen Registrierungsnetzwerk aufweist? 
Wenn Sie versuchen, manuell einen neuen DNS-Eintrag in einer privaten Zone zu erstellen, die den gleichen Hostnamen wie ein vorhandener (automatisch registrierter) virtueller Computer in einem virtuellen Registrierungsnetzwerk aufweist, wird dem neuen DNS-Eintrag gestattet, den automatisch registrierten Eintrag für den virtuellen Computer zu überschreiben. Darüber hinaus wird bei dem anschließenden Versuch, diesen manuell erstellten DNS-Eintrag aus der Zone zu löschen, der Löschvorgang erfolgreich ausgeführt, und die automatische Registrierung erfolgt erneut (der DNS-Eintrag wird in der Zone automatisch neu erstellt), sofern der virtuelle Computer noch vorhanden ist und ihm eine private IP-Adresse angefügt ist. 

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone--would-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-as-well"></a>Was geschieht, wenn wir die Verknüpfung eines virtuellen Registrierungsnetzwerks mit einer privaten Zone aufheben? Würden auch die automatisch registrierten Einträge virtueller Computer vom virtuellen Netzwerk aus der Zone entfernt werden?
Ja. Wenn Sie die Verknüpfung eines virtuellen Registrierungsnetzwerks mit einer privaten Zone aufheben (die DNS-Zone aktualisieren, um das zugeordnete virtuelle Registrierungsnetzwerk zu entfernen), entfernt Azure alle automatisch registrierten Einträge virtueller Computer aus der Zone. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-that-is-linked-to-a-private-zone--do-we-have-to-manually-update-the-private-zone-to-un-link-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>Was geschieht, wenn wir ein virtuelles Registrierungsnetzwerk (oder Auflösungsnetzwerk) löschen, das mit einer privaten Zone verknüpft ist? Müssen wir die private Zone manuell aktualisieren, um die Verknüpfung des virtuellen Netzwerks mit der Zone als virtuelles Registrierungsnetzwerk (oder Auflösungsnetzwerk) aufzuheben?
Ja. Wenn Sie ein virtuelles Registrierungsnetzwerk (oder Auflösungsnetzwerk) löschen, ohne zuerst die Verknüpfung mit einer privaten Zone aufzuheben, ermöglicht Azure einen erfolgreichen Löschvorgang, doch wird die Verknüpfung des virtuellen Netzwerks mit der privaten Zone (falls vorhanden) nicht automatisch aufgehoben. Sie müssen die Verknüpfung des virtuellen Netzwerks mit der privaten Zone manuell aufheben. Aus diesem Grund wird empfohlen, zuerst die Verknüpfung Ihres virtuellen Netzwerks mit Ihrer privaten Zone aufzuheben, bevor Sie es löschen.

### <a name="would-dns-resolution-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-contosolocal-is-linked-to-a-virtual-network"></a>Würde die DNS-Auflösung mithilfe des Standard-FQDN (internal.cloudapp.net) auch funktionieren, wenn eine private Zone (z.B. „contoso.local“) mit einem virtuellen Netzwerk verknüpft ist? 
Ja. Das Feature für private Zonen ersetzt nicht die DNS-Standardauflösungen mithilfe der von Azure bereitgestellten Zone „internal.cloudapp.net“, sondern wird als eine zusätzliche Funktion oder Erweiterung angeboten. In beiden Fällen (bei Verwendung der von Azure bereitgestellten Zone „internal.cloudapp.net“ oder Ihrer eigenen privaten Zone) wird empfohlen, den FQDN der Zone zu verwenden, anhand der Auflösungen vorgenommen werden sollen. 

### <a name="would-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Würde das DNS-Suffix auf virtuellen Computern in einem verknüpften virtuellen Netzwerk in das der privaten Zone geändert? 
Nein. Derzeit wird weiterhin das von Azure bereitgestellte Standardsuffix (*.internal.cloudapp.net) als DNS-Suffix auf den virtuellen Computern in Ihrem verknüpften virtuellen Netzwerk verwendet. Sie können jedoch dieses DNS-Suffix auf Ihren virtuellen Computern manuell in das der privaten Zone ändern. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Gibt es Einschränkungen für private Zonen während dieser Preview-Phase?
Ja. Während der Public Preview-Phase gelten die folgenden Einschränkungen:
* Ein virtuelles Registrierungsnetzwerk pro privater Zone.
* Bis zu zehn virtuelle Auflösungsnetzwerke pro privater Zone.
* Ein bestimmtes virtuelles Netzwerk kann nur mit genau einer privaten Zone als ein virtuelles Registrierungsnetzwerk verknüpft werden.
* Ein bestimmtes virtuelles Netzwerk kann mit bis zu zehn privaten Zonen als ein virtuelles Auflösungsnetzwerk verknüpft werden.
* Wenn ein virtuelles Registrierungsnetzwerk angegeben wird, können die DNS-Einträge für die virtuellen Computer aus diesem virtuellen Netzwerk, die für die private Zone registriert sind, nicht über Powershell/CLI/APIs angezeigt oder abgerufen werden, doch werden die Einträge der virtuellen Computer registriert und erfolgreich aufgelöst.
* Reverse-DNS funktioniert nur für den privaten IP-Bereich im virtuellen Registrierungsnetzwerk.
* Reverse-DNS für eine private IP-Adresse, die nicht in der privaten Zone registriert ist (z.B. eine private IP-Adresse für einen virtuellen Computer in einem virtuellen Netzwerk, das als ein virtuelles Auflösungsnetzwerk mit einer privaten Zone verknüpft ist), gibt „internal.cloudapp.net“ als DNS-Suffix zurück, doch kann dieses Suffix nicht aufgelöst werden.   
* Das virtuelle Netzwerk muss leer sein (d.h. keine virtuellen Computer mit angefügter NIC) bei der anfänglichen (d.h. erstmaligen) Verknüpfung mit einer privaten Zone als virtuelles Registrierungs- oder Auflösungsnetzwerk. Das virtuelle Netzwerk braucht jedoch dann für zukünftige Verknüpfungen mit anderen privaten Zonen als virtuelles Registrierungs- oder Auflösungsnetzwerk nicht leer zu sein. 
* Derzeit wird die bedingte Weiterleitung nicht unterstützt, z.B. für eine Auflösung zwischen Azure und lokalen Netzwerken. Eine Dokumentation dazu, wie Kunden dieses Szenario mithilfe anderer Mechanismen umsetzen können, finden Sie unter [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Gibt es Kontingente oder Grenzwerte in Bezug auf Zonen oder Einträge für private Zonen?
Es gibt für private Zonen keine gesonderten Grenzwerte bezüglich der Anzahl zulässiger Zonen pro Abonnement oder der Anzahl von Eintragssätzen pro Zone. Sowohl für öffentliche als auch private Zonen gelten die allgemeinen DNS-Grenzwerte, die [hier](../azure-subscription-service-limits.md#dns-limits) beschrieben sind.

### <a name="is-there-portal-support-for-private-zones"></a>Besteht Portalunterstützung für private Zonen?
Private Zonen, die bereits über Mechanismen außerhalb des Portals (API/PowerShell/CLI/SDKs) erstellt wurden, werden im Azure-Portal angezeigt, doch können Kunden keine neuen privaten Zonen erstellen oder Zuordnungen zu virtuellen Netzwerken verwalten. Darüber hinaus sind für virtuelle Netzwerke, die als virtuelle Registrierungsnetzwerke zugeordnet werden, die automatisch registrierten Einträge für virtuelle Computer vom Portal aus nicht sichtbar. 

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen zu Azure DNS](dns-overview.md)
<br>
[Verwenden von Azure DNS für private Domänen](private-dns-overview.md)
<br>
[Weitere Informationen zu DNS-Zonen und -Einträgen](dns-zones-records.md)
<br>
[Erste Schritte mit Azure DNS](dns-getstarted-portal.md)

