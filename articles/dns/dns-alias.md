---
title: Azure DNS-Aliaseinträge – Übersicht
description: Übersicht über die Unterstützung für Aliaseinträge in Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 05a6a1700de2b8b334b40d9efd9b8d79e9e7241f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957571"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS-Aliaseinträge – Übersicht

Azure DNS-Aliaseinträge sind Qualifizierungen für einen DNS-Ressourceneintragssatz, die es Ihnen ermöglichen, aus Ihrer DNS-Zone auf andere Azure-Ressourcen zu verweisen. Sie können beispielsweise einen Aliaseintragssatz erstellen, der auf eine öffentliche Azure-IP-Adresse anstatt auf einen A-Datensatz verweist. Da Ihr Aliaseintragssatz dynamisch auf eine Dienstinstanz einer öffentlichen Azure-IP-Adresse verweist, aktualisiert sich der Aliaseintragssatz während der DNS-Auflösung reibungslos selbst.

Ein Aliasdatensatz wird für die folgenden Datensatztypen in einer Azure DNS-Zone unterstützt: A, AAAA und CNAME. 

> [!NOTE]
> Aliaseinträge für die Datensatztypen A oder AAAA für Traffic Manager werden nur für externe Endpunkttypen unterstützt. Sie müssen die IPv4- oder IPv6-Adresse (idealerweise statische IPs) nach Bedarf für externe Endpunkte in Traffic Manager angeben.

## <a name="capabilities"></a>Funktionen

- **Verweisen auf eine öffentliche IP-Ressource aus einem DNS-A/AAAA-Eintragssatz**. Sie können einen A/AAAA-Eintragssatz erstellen und zu einem Aliaseintragssatz machen, um auf eine öffentliche IP-Ressource zu verweisen.
- **Verweisen auf ein Traffic Manager-Profil aus einem DNS-A/AAAA/CNAME-Eintragssatz**. Zusätzlich zur Möglichkeit, von einem DNS-CNAME-Eintragssatz auf den CNAME eines Traffic Manager-Profils (z.B.: contoso.trafficmanager.net) zu verweisen, können Sie jetzt auch von einem A- oder AAAA-Eintragssatz in Ihrer DNS-Zone auf ein Traffic Manager-Profil verweisen, das über externe Endpunkte verfügt.
   > [!NOTE]
   > Aliaseinträge für die Eintragstypen A oder AAAA für Traffic Manager werden nur für externe Endpunkttypen unterstützt. Sie müssen die IPv4- oder IPv6-Adresse (idealerweise statische IPs) nach Bedarf für externe Endpunkte in Traffic Manager angeben.
- **Verweisen auf einen anderen DNS-Ressourceneintragssatz innerhalb derselben Zone**. Aliaseinträge können auf andere Eintragssätze desselben Typs verweisen. So kann beispielsweise ein DNS-CNAME-Eintragssatz ein Alias für einen anderen CNAME-Eintragsatz desselben Typs sein. Dies ist hilfreich, wenn Sie möchten, dass einige Eintragssätze in Bezug auf ihr Verhalten Aliase und andere keine Aliase sind.

## <a name="scenarios"></a>Szenarien
Für Aliaseinträge gibt es einige häufige Szenarien:

### <a name="prevent-dangling-dns-records"></a>Vermeiden verwaister DNS-Einträge
Innerhalb von Azure DNS-Zonen können Aliaseinträge verwendet werden, um den Lebenszyklus von Azure-Ressourcen wie etwa einer öffentlichen IP oder eines Traffic Manager-Profils nachzuverfolgen. Eines der häufigsten Probleme mit herkömmlichen DNS-Einträgen sind „verwaiste Einträge“, insbesondere bei A/AAAA- oder CNAME-Eintragstypen. 

Wenn die Ziel-IP oder der CNAME bei einem traditionellen DNS-Zoneneintrag nicht mehr vorhanden ist, weiß der DNS-Zoneneintrag nichts davon und muss manuell aktualisiert werden. In einigen Organisationen wird diese manuelle Aktualisierung möglicherweise nicht rechtzeitig durchgeführt oder kann aufgrund der Trennung von Rollen und zugeordneten Berechtigungsebenen problematisch sein.

Beispielsweise die Rolle, die über Berechtigungen zum Löschen des CNAME oder der IP-Adresse einer Anwendung verfügen, haben unter Umständen keine ausreichenden Berechtigungen, um den DNS-Eintrag zu aktualisieren, der auf diese Ziele verweist. Daher gibt es möglicherweise eine zeitliche Verzögerung zwischen dem Löschen der IP-Adresse oder dem CNAME und dem Entfernen des DNS-Eintrags, der darauf verweist, was unter Umständen einen Ausfall für Endbenutzer verursacht.

Aliaseinträge verringern die Komplexität dieses Szenarios und helfen dabei, solche verwaisten Verweise zu verhindern. Wenn ein DNS-Eintrag als Aliaseintrag qualifiziert wird, um auf eine öffentliche IP-Adresse oder ein Traffic Manager-Profil zu verweisen, und diese zugrunde liegenden Ressourcen gelöscht werden, wird der DNS-Aliaseintrag zur gleichen Zeit ebenfalls entfernt. Dadurch wird sichergestellt, dass keine Ausfälle für die Endbenutzer auftreten.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Automatisches Aktualisieren von DNS-Zonen bei Änderungen von Anwendungs-IPs

Wenn eine Anwendung verschoben wird oder der zugrunde liegende virtuelle Computer neu gestartet wird, wird ein Aliaseintrag so ähnlich wie beim vorherigen Szenario automatisch aktualisiert, wenn sich die IP-Adresse für die zugrunde liegende öffentliche IP-Ressource ändert. Sie können potenzielle Sicherheitsrisiken vermeiden, wenn Endbenutzer zu einer anderen Anwendung mit der alten IP-Adresse weitergeleitet werden.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Hosten von Anwendungen mit Lastenausgleich an der Zonenspitze

Das DNS-Protokoll verhindert, dass an der Zonenspitze etwas anderes als ein A- oder AAAA-Eintrag zugewiesen wird (z.B.: contoso.com). Für Anwendungsbesitzer, die über Anwendungen mit Lastenausgleich hinter einem Traffic Manager verfügen, stellt dies ein Problem dar, da es nicht möglich war, vom Zonenspitzeneintrag auf das Traffic Manager-Profil zu verweisen. Daher waren Anwendungsbesitzer gezwungen, das Problem zu umgehen, beispielsweise durch eine Umleitung auf die Anwendungsschicht, um von der Zonenspitze auf eine andere Domäne (von contoso.com zu www.contoso.com) umzuleiten. Im Hinblick auf die Umleitungsfunktionen stellt dies einen Single Point of Failure dar.

Bei Aliaseinträgen besteht dieses Problem nicht mehr. Anwendungsbesitzer können jetzt ihren Zonenspitzeneintrag auf ein Traffic Manager-Profil verweisen, das über externe Endpunkte verfügt. Mit dieser Funktion können Anwendungsbesitzer auf dasselbe Traffic Manager-Profil verweisen, das für anderen Domänen innerhalb ihrer DNS-Zone verwendet wird. Beispielsweise können contoso.com und www.contoso.com beide auf dasselbe Traffic Manager-Profil verweisen, solange für das Traffic Manager-Profil nur externe Endpunkte konfiguriert wurden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Aliaseinträgen finden Sie in den folgenden Artikeln:

- [Tutorial: Configure an alias record to refer to an Azure Public IP address](tutorial-alias-pip.md) (Tutorial: Konfigurieren eines Aliaseintrags zum Verweisen auf eine öffentliche Azure-IP-Adresse)
- [Tutorial: Configure an alias record to support apex domain names with Traffic Manager](tutorial-alias-tm.md) (Tutorial: Konfigurieren eines Aliaseintrags zur Unterstützung von Apex-Domänennamen mit Traffic Manager)
- [Häufig gestellte Fragen zu DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
