---
title: Grundlegendes zu ausgehenden Verbindungen in Azure | Microsoft Docs
description: "In diesem Artikel wird erläutert, wie Azure virtuellen Computern die Kommunikation mit öffentlichen Internetdiensten ermöglicht."
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: b8e225ba4374c73dbabac3dddab9ba37fa798a5a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Grundlegendes zu ausgehenden Verbindungen in Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Ein virtueller Computer (VM) in Azure kann mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren. Wenn eine VM einen ausgehenden Datenfluss in einen öffentlichen IP-Adressraum einleitet, ordnet Azure die private IP-Adresse der VM einer öffentlichen IP-Adresse zu und lässt zu, das zurückfließender Datenverkehr die VM erreicht.

Azure bietet drei Methoden, um ausgehende Verbindungen zu ermöglichen. Jede Methode weist eigene Merkmale und Einschränkungen auf. Überprüfen Sie jede Methode sorgfältig, um diejenige auszuwählen, die Ihren Anforderungen entspricht.

| Szenario | Methode | Hinweis |
| --- | --- | --- |
| Eigenständige VM (kein Lastenausgleich, keine öffentliche IP-Adresse auf Instanzebene) |Standardmäßiges SNAT |Azure ordnet eine öffentliche IP-Adresse für SNAT zu. |
| VM mit Lastenausgleich (keine öffentliche IP-Adresse auf Instanzebene auf VM) |SNAT mit Lastenausgleich |Azure verwendet eine öffentliche IP-Adresse des Lastenausgleichs für SNAT. |
| VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich) |SNAT nicht verwendet |Azure verwendet die öffentliche IP-Adresse, die der VM zugewiesen ist. |

Wenn eine VM mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren soll, können Sie zum Blockieren des Zugriffs Netzwerksicherheitsgruppen verwenden. Netzwerksicherheitsgruppen werden unter [Verhindern öffentlicher Verbindungen](#preventing-public-connectivity) detaillierter behandelt.

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Eigenständige VM ohne öffentliche IP-Adresse auf Instanzebene

In diesem Szenario gehört die VM nicht zu einem Azure Load Balancer-Pool, und ihr ist keine öffentliche IP-Adresse auf Instanzebene zugewiesen. Wenn der virtuelle Computer einen ausgehenden Datenfluss einleitet, übersetzt Azure die private IP-Quelladresse für den ausgehenden Datenfluss in eine öffentliche IP-Quelladresse. Die für diesen ausgehenden Datenfluss verwendete öffentliche IP-Adresse ist nicht konfigurierbar und wird nicht auf die Ressourcengrenze des Abonnements für öffentliche IP-Adressen angerechnet. Azure verwendet das Verfahren „Source Network Address Translation (SNAT, Übersetzung der Quellnetzwerkadresse)“ für diese Aufgabe. Kurzlebige Ports der öffentlichen IP-Adresse werden verwendet, um einzelne Datenflüsse zu unterscheiden, die von der VM stammen. Beim Erstellen von Datenflüssen weist SNAT kurzlebige Ports dynamisch zu. In diesem Kontext werden die kurzlebigen für SNAT verwendeten Ports als SNAT-Ports bezeichnet.

SNAT Ports sind begrenzte Ressourcen, die sich erschöpfen können. Es ist wichtig, ihre Nutzung zu verstehen. Pro Datenfluss zu einer einzelnen IP-Zieladresse wird ein SNAT-Port genutzt. Für mehrere Datenflüsse zur gleichen IP-Zieladresse und zum selben Port belegt jeder Datenfluss einen einzelnen SNAT-Port. Dadurch wird sichergestellt, dass die Datenflüsse eindeutig sind, wenn sie von der gleichen öffentlichen IP-Adresse stammen und die gleiche IP-Zieladresse und denselben Zielport haben. Mehrere Datenflüsse mit jeweils einer anderen IP-Zieladresse haben einen gemeinsamen SNAT-Port. Die IP-Zieladresse und der Zielport sorgen für Eindeutigkeit der Datenflüsse.

Sie können [Log Analytics für Azure Load Balancer](load-balancer-monitor-log.md) und [Warnungsereignisprotokoll](load-balancer-monitor-log.md#alert-event-log) zum Überwachen der Integrität ausgehender Verbindungen verwenden. Sobald SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, nachdem SNAT-Ports von vorhandenen Datenflüssen freigegeben wurden. Der Lastenausgleich verwendet ein vierminütiges Leerlauftimeout für die Freigabe von SNAT-Ports.  Lesen Sie [VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich)](#vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer) im folgenden Abschnitt sowie [Verwalten der SNAT-Auslastung](#snatexhaust).

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>VM mit Lastenausgleich ohne öffentliche IP-Adresse auf Instanzebene

In diesem Szenario gehört die VM zum Azure Load Balancer-Pool.  Der VM ist keine öffentliche IP-Adresse zugewiesen. Die Lastenausgleichsressource muss mit einer Lastenausgleichsregel zum Erstellen einer Verknüpfung der öffentlichen Front-End-IP mit dem Back-End-Pool konfiguriert werden. Wenn Sie diese Konfiguration nicht abschließen, ergibt sich das Verhalten wie im vorhergehenden Abschnitt für [Eigenständige VM ohne öffentliche IP-Adresse auf Instanzebene](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address) beschrieben.

Wenn die dem Lastenausgleich unterliegende VM einen ausgehenden Datenfluss erstellt, übersetzt Azure die private IP-Quelladresse des ausgehenden Datenflusses in die öffentliche IP-Adresse des öffentlichen Lastenausgleichs-Frond-End. Azure verwendet das Verfahren „Source Network Address Translation (SNAT, Übersetzung der Quellnetzwerkadresse)“ für diese Aufgabe. Kurzlebiger Ports der öffentlichen IP-Adresse des Lastenausgleichs werden verwendet, um einzelne Datenflüsse zu unterscheiden, die von der VM stammen. Beim Erstellen ausgehender Datenflüsse weist SNAT kurzlebige Ports dynamisch zu. In diesem Kontext werden die kurzlebigen für SNAT verwendeten Ports als SNAT-Ports bezeichnet.

SNAT Ports sind begrenzte Ressourcen, die sich erschöpfen können. Es ist wichtig, ihre Nutzung zu verstehen. Pro Datenfluss zu einer einzelnen IP-Zieladresse und einem Port wird ein SNAT-Port genutzt. Für mehrere Datenflüsse zur gleichen IP-Zieladresse und zum selben Port belegt jeder Datenfluss einen einzelnen SNAT-Port. Dadurch wird sichergestellt, dass die Datenflüsse eindeutig sind, wenn sie von der gleichen öffentlichen IP-Adresse stammen und die gleiche IP-Zieladresse und denselben Zielport haben. Mehrere Datenflüsse mit jeweils einer anderen IP-Zieladresse haben einen gemeinsamen SNAT-Port. Die IP-Zieladresse und der Zielport sorgen für Eindeutigkeit der Datenflüsse.

Sie können [Log Analytics für Azure Load Balancer](load-balancer-monitor-log.md) und [Warnungsereignisprotokoll](load-balancer-monitor-log.md#alert-event-log) zum Überwachen der Integrität ausgehender Verbindungen verwenden. Sobald SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, nachdem SNAT-Ports von vorhandenen Datenflüssen freigegeben wurden. Der Lastenausgleich verwendet ein vierminütiges Leerlauftimeout für die Freigabe von SNAT-Ports.  Lesen Sie den folgenden Abschnitt sowie [Verwalten der SNAT-Auslastung](#snatexhaust).

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich)

In diesem Szenario ist der VM eine öffentliche IP-Adresse auf Instanzebene (Instance Level Public IP, ILPIP) zugewiesen. Es ist unerheblich, ob der virtuelle Computer einen Lastenausgleich hat oder nicht. Wenn eine öffentliche IP-Adresse auf Instanzebene verwendet wird, kommt SNAT nicht zum Einsatz. Die VM nutzt die öffentliche IP-Adresse auf Instanzebene für alle ausgehenden Datenflüsse. Wenn Ihre Anwendung viele ausgehende Datenflüsse auslöst und SNAT überlastet sein sollte, sollten Sie das Zuweisen einer öffentlichen IP-Adresse auf Instanzebene erwägen, um SNAT-Engpässe zu mindern.

## <a name="discovering-the-public-ip-used-by-a-given-vm"></a>Ermitteln der von einer bestimmten VM verwendeten öffentlichen IP-Adresse

Es gibt viele Möglichkeiten, die öffentliche IP-Quelladresse einer ausgehenden Verbindung zu bestimmen. OpenDNS bietet einen Dienst, der die öffentliche IP-Adresse Ihrer VM anzeigen kann. Mithilfe des Befehls „nslookup“ können Sie eine DNS-Abfrage für den Namen „myip.opendns.com“ an den OpenDNS-Resolver senden. Der Dienst gibt die IP-Quelladresse zurück, die zum Senden der Abfrage verwendet wurde. Wenn Sie die folgende Abfrage auf Ihrer VM ausführen, ist die Antwort die öffentliche IP-Adresse, die für diese VM verwendet wird.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Verhindern öffentlicher Verbindungen

Mitunter ist es nicht wünschenswert, dass eine VM einen ausgehenden Datenfluss erstellen darf, oder ggf. besteht die Anforderung, zu verwalten, welche Ziele mit ausgehenden Datenflüsse erreicht werden können, oder bei welchen Zielen eingehende Datenflüsse beginnen können. In diesem Fall verwenden Sie [Netzwerksicherheitsgruppen (NSG)](../virtual-network/virtual-networks-nsg.md) sowohl zum Verwalten der Ziele, die die VM erreichen kann, als auch des öffentlichen Ziels, das eingehende Datenflüsse initiieren kann. Wenn Sie eine Netzwerksicherheitsgruppe einer VM mit Lastenausgleich zuordnen, müssen Sie die [Standardtags](../virtual-network/virtual-networks-nsg.md#default-tags) und [Standardregeln](../virtual-network/virtual-networks-nsg.md#default-rules) beachten.

Sie müssen sicherstellen, dass die VM Anforderungen von Integritätstests von Azure Load Balancer empfangen kann. Wenn eine Netzwerksicherheitsgruppe Anforderungen von Integritätstests vom Standardtag AZURE_LOADBALANCER blockiert, misslingt Ihr VM-Integritätstests, weshalb die VM mit „Außer Betrieb“ markiert wird. Der Lastenausgleich beendet das Senden neuer Datenflüsse an diese VM.

## <a name="snatexhaust"></a>Verwalten der SNAT-Auslastung

Für SNAT verwendete kurzlebige Ports sind eine erschöpfbare Ressource, wie in [Eigenständige VM ohne öffentliche IP-Adresse auf Instanzebene](#standalone-vm-with-no-instance-level-public-ip-address) und [VM mit Lastenausgleich ohne öffentliche IP-Adresse auf Instanzebene](#standalone-vm-with-no-instance-level-public-ip-address) beschrieben.

Wenn Sie wissen, dass Sie viele ausgehende TCP- oder UDP-Verbindungen zur selben IP-Zieladresse und demselben Port initiieren, Fehler bei ausgehenden Verbindungen feststellen oder vom Support darauf hingewiesen werden, dass Sie zu viele SNAT-Ports in Anspruch nehmen, stehen Ihnen mehrere Abhilfemaßnahmen zur Verfügung.  Überprüfen Sie diese Optionen, und entscheiden Sie, welche für Ihr Szenario am besten geeignet ist.  Möglicherweise erleichtern eine oder mehrere Ihnen die Verwaltung dieses Szenarios.

### <a name="modify-application-to-reuse-connections"></a>Ändern der Anwendung für die Wiederverwendung von Verbindungen 
Sie können den Bedarf an kurzlebigen Ports, die für SNAT verwendet werden, durch Wiederverwenden von Verbindungen in der Anwendung reduzieren.  Dies gilt insbesondere für Protokolle wie HTTP/1.1, bei denen die Wiederverwendung von Verbindungen der Standard ist.  Andere Protokolle, die HTTP als Transport verwenden (z.B. REST) können davon wiederum profitieren.  Wiederverwendung ist immer besser als einzelne atomische TCP-Verbindungen für jede Anforderung. Sie führt zu TCP-Transaktionen mit besserer Leistung und deutlich höherer Effizienz.

### <a name="modify-application-to-use-connection-pooling"></a>Ändern der Anwendung für die Verwendung von Verbindungspooling
Sie können in Ihrer Anwendung ein Verbindungspoolingschema verwenden, bei dem Anforderungen intern auf einen festen Satz von Verbindungen verteilt werden (wobei die Verbindungen möglichst wiederverwendet werden).  Dadurch wird die Anzahl der verwendeten kurzlebigen Ports eingeschränkt und eine besser vorhersagbare Umgebung hergestellt.  Dies kann auch den Durchsatz von Anforderungen erhöhen, indem mehrere gleichzeitige Vorgänge zugelassen werden, wenn eine einzelne Verbindung durch die Antwort eines Vorgangs blockiert wird.  Verbindungspooling ist möglicherweise innerhalb des Frameworks, das Sie zum Entwickeln Ihrer Anwendung oder der Konfigurationseinstellungen für Ihre Anwendung verwenden, bereits vorhanden.  Sie können dies mit der Wiederverwendung von Verbindungen kombinieren. Dann greifen Ihre Anforderungen auf eine feste und vorhersehbare Anzahl von Ports an derselben IP-Zieladresse und denselben Port zu und können gleichzeitig die Vorteile einer sehr effizienten Verwendung von TCP-Transaktionen mit geringeren Latenzen und geringerer Ressourcenauslastung nutzen.

### <a name="modify-application-to-use-less-aggressive-retry-logic"></a>Ändern der Anwendung für die Verwendung weniger aggressiver Wiederholungslogik
Wenn die für SNAT verwendeten kurzlebigen Ports erschöpft sind oder Anwendungsfehler auftreten, führen aggressive oder Brute-Force-Wiederholungsversuche ohne Verfalls- und Backofflogik zu Erschöpfung oder gar anhaltender Erschöpfung. Sie können den Bedarf an kurzlebigen Ports durch Verwendung einer weniger aggressiven Wiederholungslogik reduzieren.   Für kurzlebige Ports ist ein (nicht veränderbarer) 4-Minuten-Leerlauftimeout festgelegt, und wenn die Wiederholungen zu aggressiv sind, ist kein Regenerieren von der Erschöpfung von selbst mehr möglich.  Die Überlegung, wie und wie oft Ihre Anwendung Transaktionen wiederholt, ist daher ein wichtiger Aspekt des Entwurfs.

### <a name="assign-an-instance-level-public-ip-to-each-vm"></a>Zuweisen einer öffentlichen IP-Adresse auf Instanzebene an jeden virtuellen Computer
So erhalten Sie das Szenario [VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich)](#vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer).  Alle kurzlebigen Ports der für jede VM verwendeten öffentlichen IP-Adresse sind für die VM verfügbar (im Gegensatz zu Szenarien, in denen kurzlebige Ports einer öffentlichen IP-Adresse gemeinsam mit allen VMs genutzt werden, die dem jeweiligen Back-End-Pool zugeordnet sind).  Es müssen auch Nachteile berücksichtigt werden – etwa zusätzliche Kosten für IP-Adressen und mögliche Auswirkungen des Whitelistings einer großen Anzahl von einzelnen IP-Adressen.

## <a name="limitations"></a>Einschränkungen

Wenn [einem Lastenausgleich mehrere (öffentliche) IP-Adressen zugeordnet sind](load-balancer-multivip-overview.md), sind diese öffentlichen IP-Adressen Kandidaten für ausgehende Datenflüsse.

Azure verwendet einen Algorithmus, um basierend auf der Größe des Pools die Anzahl der verfügbaren SNAT-Ports zu bestimmen.  Dies ist zu diesem Zeitpunkt nicht konfigurierbar.

Ausgehende Verbindungen haben einen 4-Minuten-Leerlauftimeout.  Dieser ist nicht veränderbar.

Es ist wichtig, zu beachten, dass die Anzahl der verfügbaren SNAT-Ports nicht direkt in die Anzahl der Verbindungen übersetzt wird. In den vorhergehenden Abschnitten finden Sie Angaben darüber, wann und wie SNAT-Ports zugeordnet werden und wie [diese begrenzte Ressource verwaltet](#snatexhaust) wird.
