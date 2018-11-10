---
title: Azure DNS-Aliaseinträge – Übersicht
description: Übersicht über die Unterstützung für Aliaseinträge in Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 52b42e964e7abe207064aff49f7f8f27f8476ef4
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092841"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS-Aliaseinträge – Übersicht

Azure DNS-Aliaseinträge sind Qualifizierungen für einen DNS-Ressourceneintragssatz. Sie können aus Ihrer DNS-Zone heraus auf andere Azure-Ressourcen verweisen. So können Sie beispielsweise einen Aliaseintragssatz erstellen, der anstatt auf einen A-Datensatz auf eine öffentliche Azure-IP-Adresse verweist. Ihr Aliaseintragssatz verweist dynamisch auf eine Dienstinstanz einer öffentlichen Azure-IP-Adresse. Dadurch aktualisiert sich der Aliaseintragssatz während der DNS-Auflösung nahtlos selbst.

Aliaseintragssätze werden für die folgenden Eintragstypen in einer Azure DNS-Zone unterstützt: 

- A 
- AAAA 
- CNAME 

> [!NOTE]
> Aliaseinträge für die Eintragstypen A und AAAA für Azure Traffic Manager werden nur für externe Endpunkttypen unterstützt. Geben Sie je nach Bedarf die IPv4- oder IPv6-Adresse für externe Endpunkte in Traffic Manager an. Verwenden Sie nach Möglichkeit statische IP-Adressen für die Adresse.

## <a name="capabilities"></a>Funktionen

- **Verweisen auf eine öffentliche IP-Ressource über einen DNS-Eintragssatz vom Typ A oder AAAA:** Sie können einen A/AAAA-Eintragssatz erstellen und zu einem Aliaseintragssatz machen, um auf eine öffentliche IP-Ressource zu verweisen.

- **Verweisen auf ein Traffic Manager-Profil über einen DNS-Eintragssatz vom Typ A, AAAA oder CNAME:** Sie können über einen DNS-Eintragssatz vom Typ CNAME auf den CNAME eines Traffic Manager-Profils verweisen. Ein Beispiel hierfür ist „contoso.trafficmanager.net“. Außerdem können Sie nun auch auf ein Traffic Manager-Profil verweisen, das über externe Endpunkte eines A- oder AAAA-Eintragssatzes in Ihrer DNS-Zone verfügt.

   > [!NOTE]
   > Aliaseinträge für die Eintragstypen A und AAAA für Traffic Manager werden nur für externe Endpunkttypen unterstützt. Geben Sie je nach Bedarf die IPv4- oder IPv6-Adresse für externe Endpunkte in Traffic Manager an. Verwenden Sie nach Möglichkeit statische IP-Adressen für die Adresse.
   
- **Verweisen auf einen anderen DNS-Ressourceneintragssatz innerhalb derselben Zone:** Aliaseinträge können auf andere Eintragssätze desselben Typs verweisen. So kann beispielsweise ein DNS-Eintragssatz vom Typ CNAME ein Alias für einen anderen CNAME-Eintragssatz desselben Typs sein. Dies ist nützlich, wenn Sie möchten, dass einige Eintragssätze Aliase und andere keine Aliase sind.

## <a name="scenarios"></a>Szenarien
Aliaseinträge sind in einigen allgemeinen Szenarien hilfreich.

### <a name="prevent-dangling-dns-records"></a>Vermeiden verwaister DNS-Einträge
 Innerhalb von Azure DNS-Zonen lässt sich mithilfe von Aliaseinträgen der Lebenszyklus von Azure-Ressourcen nachverfolgen. Zu solchen Ressourcen zählen beispielsweise öffentliche IP-Adressen oder Traffic Manager-Profile. Ein verbreitetes Problem bei herkömmlichen DNS-Einträgen sind verwaiste Einträge. Dieses Problem tritt besonders bei A/AAAA- und CNAME-Eintragstypen auf. 

Wenn bei einem traditionellen DNS-Zoneneintrag die Ziel-IP-Adresse oder der CNAME nicht mehr vorhanden ist, wird diese Information nicht an den DNS-Zoneneintrag weitergegeben. Der Datensatz muss also manuell aktualisiert werden. In manchen Organisationen findet diese manuelle Aktualisierung unter Umständen nicht rechtzeitig statt. Darüber hinaus kann sich die Trennung von Rollen und zugeordneten Berechtigungsebenen als problematisch erweisen.

So kann es beispielsweise sein, dass eine Rolle zum Löschen von CNAMEs oder IP-Adressen autorisiert ist, die zu einer Anwendung gehören. Sie verfügt aber möglicherweise nicht über die erforderlichen Berechtigungen, um den DNS-Eintrag zu aktualisieren, der auf diese Ziele verweist. Dadurch kommt es zu einer Verzögerung zwischen dem Löschen der IP-Adresse oder des CNAME und dem Entfernen des DNS-Eintrags, der darauf verweist. Diese Verzögerung kann ggf. zu einem Ausfall für Benutzer führen.

Aliaseinträge verringern die Komplexität dieses Szenarios. Sie tragen dazu bei, verwaiste Verweise zu verhindern. Stellen Sie sich beispielsweise einen DNS-Eintrag vor, der als Aliaseintrag qualifiziert wird, um auf eine öffentliche IP-Adresse oder auf ein Traffic Manager-Profil zu verweisen. Wenn die zugrunde liegenden Ressourcen gelöscht werden, wird gleichzeitig auch der DNS-Aliaseintrag entfernt. Durch diesen Prozess werden Ausfälle für Benutzer verhindert.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Automatisches Aktualisieren von DNS-Zonen bei Änderungen von Anwendungs-IPs

Dieses Szenario ähnelt dem vorherigen Szenario. Möglicherweise wird eine Anwendung verschoben, oder der zugrunde liegende virtuelle Computer wird neu gestartet. Ein Aliaseintrag wird automatisch aktualisiert, wenn sich die IP-Adresse für die zugrunde liegende öffentliche IP-Ressource ändert. Leiten Sie Benutzer zu einer anderen Anwendung mit der alten IP-Adresse weiter, um potenzielle Sicherheitsrisiken zu vermeiden.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Hosten von Anwendungen mit Lastenausgleich im Zonen-Apex

Das DNS-Protokoll sorgt dafür, dass im Zonen-Apex nur A- oder AAAA-Einträge zugewiesen werden können. Ein Beispiel wäre etwa „contoso.com“. Diese Einschränkung stellt Anwendungsbesitzer, die über Anwendungen mit Lastenausgleich hinter Traffic Manager verfügen, vor ein Problem. Über den Zonen-Apex-Eintrag kann nicht auf das Traffic Manager-Profil verwiesen werden. Anwendungsbesitzer müssen daher auf eine Problemumgehung zurückgreifen: eine Umleitung in der Anwendungsschicht vom Zonen-Apex zu einer anderen Domäne, beispielsweise von „contoso.com“ zu „www.contoso.com“. Dies stellt einen Single Point of Failure für die Umleitungsfunktion dar.

Bei Aliaseinträgen besteht dieses Problem nicht mehr. Nun können Anwendungsbesitzer für Ihren Zonen-Apex-Eintrag einen Verweis auf ein Traffic Manager-Profil erstellen, das über externe Endpunkte verfügt. Anwendungsbesitzer können auf das gleiche Traffic Manager-Profil verweisen, das auch für andere Domänen innerhalb ihrer DNS-Zone verwendet wird. So können beispielsweise „contoso.com“ und „www.contoso.com“ jeweils auf das gleiche Traffic Manager-Profil verweisen. Voraussetzung ist allerdings, dass für das Traffic Manager-Profil nur externe Endpunkte konfiguriert sind.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Aliaseinträgen finden Sie in den folgenden Artikeln:

- [Tutorial: Konfigurieren eines Aliaseintrags, um auf eine öffentliche Azure-IP-Adresse zu verweisen](tutorial-alias-pip.md)
- [Tutorial: Configure an alias record to support apex domain names with Traffic Manager](tutorial-alias-tm.md) (Tutorial: Konfigurieren eines Aliaseintrags zur Unterstützung von Apex-Domänennamen mit Traffic Manager)
- [Häufig gestellte Fragen zu DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
