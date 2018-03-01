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
ms.date: 02/05/2018
ms.author: kumud
ms.openlocfilehash: e1a2b4c281194ec4c70e4d9fe1bc97c5aa61436e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Grundlegendes zu ausgehenden Verbindungen in Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


In Azure wird die Konnektivität in ausgehender Richtung für Benutzerbereitstellungen mit mehreren unterschiedlichen Mechanismen erzielt.  In diesem Artikel wird beschrieben, welche Szenarien es gibt, wann sie zutreffen, wie sie funktionieren und wie sie verwaltet werden.

Eine Bereitstellung in Azure kann mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren. Wenn eine Instanz einen ausgehenden Datenfluss an ein Ziel im öffentlichen IP-Adressbereich initiiert, ordnet Azure die private IP-Adresse dynamisch einer öffentlichen IP-Adresse zu.  Nachdem diese Zuordnung erstellt wurde, kann der zurückgegebene Datenverkehr für diesen ursprünglich ausgehenden Datenfluss auch die private IP-Adresse erreichen, von der der Datenfluss stammt.

Azure verwendet für diese Aufgabe das Verfahren „Übersetzung der Quellnetzwerkadresse“ (Source Network Address Translation, SNAT).  Wenn mehrere private IP-Adressen durch eine einzelne öffentliche IP-Adresse maskiert sind, wird in Azure die [Portadressenübersetzung](#pat) (Port Address Translation, PAT) genutzt, um private IP-Adressen zu maskieren.  Kurzlebige Ports werden für PAT verwendet und sind je nach Poolgröße [vorab zugeordnet](#preallocatedports).

Es gibt mehrere [Szenarien für die ausgehende Richtung](#scenarios). Diese Szenarien können je nach Bedarf kombiniert werden. Prüfen Sie sie sorgfältig, um die Funktionen, Einschränkungen und Muster sowie die damit verbundenen Auswirkungen auf Ihr Bereitstellungsmodell und Anwendungsszenario zu verstehen.  Sehen Sie sich die Anleitung zur [Verwaltung dieser Szenarien](#snatexhaust) an.

## <a name="scenarios"></a>Übersicht über das Szenario

Azure verfügt über zwei Hauptmodelle für die Bereitstellung: Azure Resource Manager und klassisch. Der Lastenausgleich und die dazugehörigen Ressourcen werden explizit definiert, wenn [Azure Resource Manager-Ressourcen](#arm) verwendet werden.  Bei klassischen Bereitstellungen wird das Konzept eines Lastenausgleichs abstrahiert und eine ähnliche Funktion ausgedrückt, indem die Endpunkte eines [Clouddiensts](#classic) definiert werden. Die zutreffenden [Szenarien](#scenarios) für Ihre Bereitstellung hängen davon ab, welches Bereitstellungsmodell verwendet wird.

### <a name="arm"></a>Azure Resource Manager

In Azure gibt es derzeit drei verschiedene Methoden, wie die ausgehende Konnektivität für Azure Resource Manager-Ressourcen erreicht werden kann.  [Klassische](#classic) Bereitstellungen weisen eine Teilmenge dieser Szenarien auf.

| Szenario | Methode | Beschreibung |
| --- | --- | --- |
| [1. VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich)](#ilpip) | SNAT, keine Portmaskierung |In Azure wird die öffentliche IP-Adresse verwendet, die der IP-Konfiguration der NIC einer Instanz zugewiesen ist.  Für die Instanz sind alle kurzlebigen Ports verfügbar. |
| [2. Öffentlicher Lastenausgleich, der einer VM zugewiesen ist (keine öffentliche IP-Adresse auf Instanzebene für die Instanz)](#lb) | SNAT mit Portmaskierung (PAT) per Front-End(s) des Lastenausgleichs |In Azure werden die öffentliche IP-Adresse der öffentlichen Front-Ends des Lastenausgleichs mit mehreren privaten IP-Adressen gemeinsam genutzt und kurzlebige Ports der Front-Ends für PAT verwendet. |
| [3. Eigenständige VM (kein Lastenausgleich, keine öffentliche IP-Adresse auf Instanzebene)](#defaultsnat) | SNAT mit Portmaskierung (PAT) | Azure weist für SNAT automatisch eine öffentliche IP-Adresse zu, nutzt diese öffentliche IP-Adresse gemeinsam mit mehreren privaten IP-Adressen der Verfügbarkeitsgruppe und verwendet kurzlebige Ports dieser öffentlichen IP-Adresse.  Dieses Szenario ist ein Fallbackszenario für die vorherigen Szenarien 1 und 2 und nicht zu empfehlen, falls Sie Sichtbarkeit und Kontrolle benötigen. |

Wenn eine VM mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren soll, können Sie je nach Bedarf zum Blockieren des Zugriffs Netzwerksicherheitsgruppen verwenden.  Die Verwendung von Netzwerksicherheitsgruppen wird unter [Verhindern der ausgehenden Konnektivität](#preventoutbound) ausführlicher beschrieben.  Die Anleitung für den Entwurf und die Implementierung mit einer Beschreibung, wie ein virtuelles Netzwerk ohne ausgehenden Zugriff entworfen und verwaltet wird, würde den Rahmen dieses Artikels sprengen.

### <a name="classic"></a>Klassisch (Clouddienste)

Die für klassische Bereitstellungen verfügbaren Szenarien sind eine Teilmenge von Szenarien, die für [Azure Resource Manager](#arm)-Bereitstellungen und Load Balancer Basic verfügbar sind.

Ein klassischer virtueller Computer verfügt über die gleichen drei grundlegenden Szenarien, die für Azure Resource Manager-Ressourcen beschrieben werden ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Eine klassische Webworkerrolle weist nur zwei Szenarien auf ([2](#lb), [3](#defaultsnat)).  Für [Lösungsstrategien](#snatexhaust) gelten die gleichen Unterschiede.

Der Algorithmus zum [Vorabzuweisen von kurzlebigen Ports](#ephemeralprots) für PAT-Zwecke für klassische Bereitstellungen entspricht dem Algorithmus für Azure Resource Manager-Ressourcenbereitstellungen.  

### <a name="ilpip"></a>Szenario 1: VM mit einer öffentlichen IP-Adresse auf Instanzebene

In diesem Szenario ist der VM eine öffentliche IP-Adresse auf Instanzebene (Instance Level Public IP, ILPIP) zugewiesen. Bei ausgehenden Verbindungen spielt es keine Rolle, ob für die VM ein Lastenausgleich durchgeführt wird.  Dieses Szenario hat Vorrang vor den anderen Szenarien. Wenn eine öffentliche IP-Adresse auf Instanzebene (ILPIP) verwendet wird, nutzt die VM die ILPIP für alle ausgehenden Datenflüsse.  

Es wird keine Portmaskierung (PAT) verwendet, und für die VM sind alle kurzlebigen Ports verfügbar.

Wenn Ihre Anwendung viele ausgehende Datenflüsse initiiert und es zu einer SNAT-Portüberlastung kommt, können Sie das [Zuweisen einer öffentlichen IP-Adresse auf Instanzebene erwägen, um SNAT-Engpässe zu mindern](#assignilpip). Lesen Sie sich den Abschnitt [Verwalten der SNAT-Auslastung](#snatexhaust) ganz durch.

### <a name="lb"></a>Szenario 2: VM mit Lastenausgleich ohne öffentliche IP-Adresse auf Instanzebene

Bei diesem Szenario ist die VM Teil eines Back-End-Pools für den öffentlichen Lastenausgleich.  Der VM ist keine öffentliche IP-Adresse zugewiesen. Die Lastenausgleichsressource muss mit einer Lastenausgleichsregel zum Erstellen einer Verknüpfung der öffentlichen Front-End-IP mit dem Back-End-Pool konfiguriert werden. Wenn Sie diese Regelkonfiguration nicht abschließen, ergibt sich das Verhalten wie oben unter [Eigenständige VM ohne öffentliche IP-Adresse auf Instanzebene](#defaultsnat) beschrieben.  Die Regel muss nicht über einen funktionierenden Listener im Back-End-Pool oder Integritätstest verfügen, um erfolgreich durchgeführt zu werden.

Wenn die dem Lastenausgleich unterliegende VM einen ausgehenden Datenfluss erstellt, übersetzt Azure die private IP-Quelladresse des ausgehenden Datenflusses in die öffentliche IP-Adresse des öffentlichen Lastenausgleichs-Frond-End. In Azure wird die „Übersetzung der Quellnetzwerkadresse“ (Source Network Address Translation, SNAT) verwendet, um diese Funktion durchzuführen, sowie die [Portadressübersetzung](#pat) (Port Address Translation, PAT), um mehrere private IP-Adressen hinter einer öffentlichen IP-Adresse zu maskieren. Kurzlebige Ports der öffentlichen IP-Adresse des Front-Ends für den Lastenausgleich werden verwendet, um einzelne Datenflüsse zu unterscheiden, die von der VM stammen. Beim Erstellen ausgehender Datenflüsse werden für SNAT [vorab zugewiesene kurzlebige Ports](#preallocatedports) verwendet. In diesem Kontext werden die kurzlebigen für SNAT verwendeten Ports als SNAT-Ports bezeichnet.

SNAT-Ports werden vorab zugeordnet, wie im Abschnitt [Grundlagen von SNAT und PAT](#snat) beschrieben, und stellen eine begrenzte Ressource mit einer Maximalauslastung dar. Es ist wichtig zu verstehen, wie sie [genutzt](#pat) werden. Lesen Sie sich den Abschnitt [Verwalten der SNAT-Auslastung](#snatexhaust) durch, um zu verstehen, wie Sie dies beim Entwurf berücksichtigen und je nach Bedarf das Fehlerrisiko verringern können.

Wenn [Load Balancer Basic mehrere (öffentliche) IP-Adressen zugeordnet sind](load-balancer-multivip-overview.md), sind diese öffentlichen IP-Adressen [Kandidaten für ausgehende Datenflüsse (und eine davon wird ausgewählt)](#multivipsnat).  

Sie können [Log Analytics für Azure Load Balancer](load-balancer-monitor-log.md) und [Warnungsereignisprotokolle](load-balancer-monitor-log.md#alert-event-log) zum Überwachen der Integrität ausgehender Verbindungen mit Load Balancer Basic verwenden.

### <a name="defaultsnat"></a>Szenario 3: Eigenständige VM ohne öffentliche IP-Adresse auf Instanzebene

Hierbei gehört die VM nicht zu einem Azure Load Balancer-Pool, und ihr ist keine öffentliche IP-Adresse auf Instanzebene zugewiesen. Wenn der virtuelle Computer einen ausgehenden Datenfluss einleitet, übersetzt Azure die private IP-Quelladresse für den ausgehenden Datenfluss in eine öffentliche IP-Quelladresse. Die für diesen ausgehenden Datenfluss verwendete öffentliche IP-Adresse ist nicht konfigurierbar und wird nicht auf die Ressourcengrenze des Abonnements für öffentliche IP-Adressen angerechnet. In Azure wird das Verfahren zur „Übersetzung der Quellnetzwerkadresse“ (Source Network Address Translation, SNAT) mit Portmaskierung ([PAT](#pat)) für diese Aufgabe verwendet. Dieses Szenario ähnelt [Szenario 2](#lb). Der Unterschied besteht darin, dass keine Steuerung der IP-Adresse verwendet wird.  Dies ist ein Fallbackszenario für den Fall, dass Szenario 1 und Szenario 2 nicht vorhanden sind.  Dieses Szenario wird nicht empfohlen, wenn eine Kontrolle über die ausgehende Adresse gewünscht wird.

SNAT-Ports werden vorab zugeordnet, wie im Abschnitt [Grundlagen von SNAT und PAT](#snat) beschrieben, und stellen eine begrenzte Ressource mit einer Maximalauslastung dar. Es ist wichtig zu verstehen, wie sie [genutzt](#pat) werden. Lesen Sie sich den Abschnitt [Verwalten der SNAT-Auslastung](#snatexhaust) durch, um zu verstehen, wie Sie dies beim Entwurf berücksichtigen und je nach Bedarf das Fehlerrisiko verringern können.

### <a name="combinations"></a>Mehrere kombinierte Szenarien

Die in den vorherigen Abschnitten beschriebenen Szenarien können kombiniert werden, um ein bestimmtes Ergebnis zu erzielen.  Wenn mehrere Szenarien vorhanden sind, gilt eine Prioritätsreihenfolge: [Szenario 1](#ilpip) hat Vorrang vor [Szenario 2](#lb) und [3](#defaultsnat) (nur Azure Resource Manager), und [Szenario 2](#lb) hat Vorrang vor [Szenario 3](#defaultsnat) (Azure Resource Manager und klassisch).

Ein Beispiel hierfür ist eine Azure Resource Manager-Bereitstellung, bei der die Anwendung stark von ausgehenden Verbindungen mit einer begrenzten Anzahl von Zielen abhängig ist, aber auch eingehende Datenflüsse über ein Front-End des Lastenausgleichs empfängt. In diesem Fall können Sie die Szenarien 1 und 2 kombinieren, um dies zu lösen.  Lesen Sie sich den Abschnitt [Verwalten der SNAT-Auslastung](#snatexhaust) durch, um Informationen zu zusätzlichen Mustern zu erhalten.

### <a name="multivipsnat"></a>Mehrere Front-Ends für ausgehende Datenflüsse

Für Load Balancer Basic wird ein einzelnes Front-End ausgewählt, das für ausgehende Datenflüsse verwendet wird, wenn [mehrere (öffentliche) IP-Front-Ends](load-balancer-multivip-overview.md) Kandidaten für ausgehende Datenflüsse sind.  Diese Auswahl ist nicht konfigurierbar, und der Auswahlalgorithmus sollte als zufällig angesehen werden.  Sie können für die ausgehende Richtung eine bestimmte IP-Adresse festlegen, wie unter [Kombinierte Szenarien](#combinations) beschrieben.

## <a name="snat"></a>Grundlagen von SNAT und PAT

### <a name="pat"></a>Portmaskierung mit SNAT (PAT)

Wenn eine öffentliche Load Balancer-Ressource mit VM-Instanzen verknüpft ist, wird jede ausgehende Verbindungsquelle erneut geschrieben. Die Quelle wird aus dem Adressraum der privaten Adresse des Front-Ends in die öffentliche IP-Adresse des Front-Ends des Lastenausgleichsmoduls erneut geschrieben. Im öffentlichen IP-Adressraum müssen die fünf Tupel des Datenflusses (IP-Quelladresse, Quellport, IP-Transportprotokoll, IP-Zieladresse, Zielport) eindeutig sein.  Hierfür werden kurzlebige Ports verwendet, nachdem die private IP-Quelladresse umgeschrieben wurde, da mehrere Datenflüsse von einer einzelnen öffentlichen IP-Adresse stammen.  Diese kurzlebigen Ports werden als SNAT-Ports bezeichnet. 

Pro Datenfluss zu einer einzelnen IP-Zieladresse, einem Port und einem Protokoll wird ein SNAT-Port genutzt. Bei mehreren Datenflüssen zur gleichen IP-Zieladresse bzw. zum Port und Protokoll belegt jeder Datenfluss einen einzelnen SNAT-Port. Hierdurch wird sichergestellt, dass die Datenflüsse eindeutig sind, wenn sie von der gleichen öffentlichen IP-Adresse stammen und die gleiche IP-Zieladresse, den Port und das Protokoll aufweisen. 

Mehrere Datenflüsse mit jeweils anderen Angaben für IP-Zieladresse, Port und Protokoll haben einen gemeinsamen SNAT-Port. Die IP-Adresse, der Port und das Protokoll für das Ziel bewirken, dass der Datenfluss eindeutig ist, ohne dass zusätzliche Quellports verwendet werden müssen, um Datenflüsse in Ihrem öffentlichen IP-Adressraum zu unterscheiden.

Sobald SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, nachdem SNAT-Ports von vorhandenen Datenflüssen freigegeben wurden. Der Load Balancer gibt die SNAT-Ports wieder frei, wenn der Datenflussvorgang abgeschlossen ist. Es wird ein [Leerlauftimeout von 4 Minuten](#idletimeout) verwendet, um SNAT-Ports für im Leerlauf befindliche Datenflüsse wieder freizugeben.

Im Abschnitt [Verwalten der SNAT-Auslastung](#snatexhaust) werden Muster für das Lösen von Bedingungen beschrieben, die häufig zu einer vollständigen SNAT-Portauslastung führen.

### <a name="preallocatedports"></a>Vorabzuordnung von kurzlebigen Ports für die Portmaskierung per SNAT (PAT)

In Azure wird ein Algorithmus verwendet, um basierend auf der Größe des Back-End-Pools bei der Portmaskierung per SNAT ([PAT](#pat)) die Anzahl von verfügbaren vorab zugeordneten SNAT-Ports zu ermitteln.  SNAT-Ports sind kurzlebige Ports, die für eine bestimmte öffentliche IP-Quelladresse verfügbar sind.

In Azure werden SNAT-Ports vorab der IP-Konfiguration der NIC jeder VM zugeordnet. Wenn dem Pool eine IP-Konfiguration hinzugefügt wird, werden die SNAT-Ports für diese IP-Konfiguration basierend auf der Größe des Back-End-Pools vorab zugeordnet. Für klassische Webworkerrollen erfolgt die Zuordnung pro Rolleninstanz.  Bei Erstellung von ausgehenden Datenflüssen werden diese Ports von [PAT](#pat) dynamisch genutzt (bis zum vorab festgelegten Grenzwert) und dann freigegeben, wenn der Datenfluss geschlossen wird oder ein [Leerlauftimeout](#ideltimeout) eintritt.

In der folgenden Tabelle sind die SNAT-Port-Vorabzuordnungen für Ebenen der Back-End-Poolgrößen angegeben:

| Poolgröße (VM-Instanzen) | Vorab zugeordnete SNAT-Ports pro IP-Konfiguration|
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1.000 | 32 |

Es ist wichtig, zu beachten, dass die Anzahl der verfügbaren SNAT-Ports nicht direkt in die Anzahl der Verbindungen übersetzt wird. Ein einzelner SNAT-Port kann für mehrere eindeutige Ziele wiederverwendet werden. Ports werden nur genutzt, wenn es erforderlich ist, dass Datenflüsse eindeutig sind. Eine Anleitung für den Entwurf und die Risikominderung finden Sie im Abschnitt zur [Verwaltung dieser begrenzten Ressource](#snatexhaust) und im Abschnitt mit der Beschreibung von [PAT](#pat).

Eine Änderung der Größe Ihres Back-End-Pools kann sich auf einige Ihrer eingerichteten Datenflüsse auswirken. Wenn die Größe des Back-End-Pools zunimmt und der Übergang auf die nächste Ebene vollzogen wird, wird die Hälfte Ihrer vorab zugeordneten SNAT-Ports während des Übergangs zur nächstgrößeren Back-End-Poolebene freigegeben. Für Datenflüsse, die einem wieder freigegebenen SNAT-Port zugeordnet sind, tritt ein Timeout auf, und sie müssen neu eingerichtet werden. Neue Verbindungsversuche sind sofort erfolgreich, solange vorab zugeordnete Ports verfügbar sind.

Wenn sich die Back-End-Poolgröße verringert und in eine niedrigere Dienstebene übergeht, nimmt die Anzahl der verfügbaren SNA-Ports zu. In diesem Fall sind vorhandene zugeordnete SNAT-Ports und die entsprechenden Datenflüsse nicht betroffen.

## <a name="snatexhaust"></a>Verwalten der SNAT-Auslastung (PAT)

Für [PAT](#pat) verwendete [kurzlebige Ports](#preallocatedports) sind eine begrenzte Ressource, wie unter [Eigenständige VM ohne öffentliche IP-Adresse auf Instanzebene](#defaultsnat) und [VM mit Lastenausgleich ohne öffentliche IP-Adresse auf Instanzebene](#lb) beschrieben.

Wenn Sie wissen, dass Sie viele ausgehende TCP- oder UDP-Verbindungen zu derselben IP-Zieladresse und zu demselben Port initiieren und Fehler bei ausgehenden Verbindungen feststellen oder vom Support darauf hingewiesen werden, dass Sie zu viele SNAT-Ports (vorab zugeordnete [kurzlebige Ports](#preallocatedports), die für [PAT](#pat) verwendet werden) in Anspruch nehmen, stehen Ihnen mehrere Abhilfemaßnahmen zur Verfügung.  Überprüfen Sie diese Optionen, und entscheiden Sie, welche für Ihr Szenario verfügbar und am besten geeignet sind.  Möglicherweise erleichtern eine oder mehrere Ihnen die Verwaltung dieses Szenarios.

Falls Sie Probleme haben, das Verhalten von ausgehenden Verbindungen zu verstehen, können Sie IP-Stapelstatistiken (netstat) verwenden. Es kann auch hilfreich sein, das Verbindungsverhalten durch Paketerfassungen zu beobachten.  Sie können diese Paketerfassungen im Gastbetriebssystem Ihrer Instanz durchführen oder [Network Watcher für die Paketerfassung](../network-watcher/network-watcher-packet-capture-manage-portal.md) verwenden.

### <a name="connectionreuse"></a>Ändern der Anwendung für die Wiederverwendung von Verbindungen 
Sie können den Bedarf an kurzlebigen Ports, die für SNAT verwendet werden, durch Wiederverwenden von Verbindungen in der Anwendung reduzieren.  Dies gilt insbesondere für Protokolle wie HTTP/1.1, bei denen die Wiederverwendung von Verbindungen der Standard ist.  Andere Protokolle, die HTTP als Transport verwenden (z.B. REST) können davon wiederum profitieren.  Wiederverwendung ist immer besser als einzelne atomische TCP-Verbindungen für jede Anforderung. Sie führt zu TCP-Transaktionen mit besserer Leistung und deutlich höherer Effizienz.

### <a name="connection pooling"></a>Ändern der Anwendung für die Verwendung von Verbindungspooling
Sie können in Ihrer Anwendung ein Verbindungspoolingschema verwenden, bei dem Anforderungen intern auf einen festen Satz von Verbindungen verteilt werden (wobei die Verbindungen möglichst wiederverwendet werden).  Dadurch wird die Anzahl der verwendeten kurzlebigen Ports eingeschränkt und eine besser vorhersagbare Umgebung hergestellt.  Dies kann auch den Durchsatz von Anforderungen erhöhen, indem mehrere gleichzeitige Vorgänge zugelassen werden, wenn eine einzelne Verbindung durch die Antwort eines Vorgangs blockiert wird.  Verbindungspooling ist möglicherweise innerhalb des Frameworks, das Sie zum Entwickeln Ihrer Anwendung oder der Konfigurationseinstellungen für Ihre Anwendung verwenden, bereits vorhanden.  Sie können dies mit der Wiederverwendung von Verbindungen kombinieren. Dann greifen Ihre Anforderungen auf eine feste und vorhersehbare Anzahl von Ports an derselben IP-Zieladresse und denselben Port zu und können gleichzeitig die Vorteile einer sehr effizienten Verwendung von TCP-Transaktionen mit geringeren Latenzen und geringerer Ressourcenauslastung nutzen.  UDP-Transaktionen können hiervon auch profitieren, da durch das Verwalten der Anzahl von UDP-Datenflüssen wiederum Zustände mit voller Auslastung vermieden und die SNAT-Portnutzung verwaltet werden kann.

### <a name="retry logic"></a>Ändern der Anwendung für die Verwendung weniger aggressiver Wiederholungslogik
Wenn die für [PAT](#pat) verwendeten [vorab zugeordneten kurzlebigen Ports](#preallocatedports) erschöpft sind oder Anwendungsfehler auftreten, führen aggressive oder Brute-Force-Wiederholungsversuche ohne Verfalls- und Backofflogik zu Erschöpfung oder gar anhaltender Erschöpfung. Sie können den Bedarf an kurzlebigen Ports durch Verwendung einer weniger aggressiven Wiederholungslogik reduzieren.   Für kurzlebige Ports ist ein (nicht veränderbarer) 4-Minuten-Leerlauftimeout festgelegt, und wenn die Wiederholungen zu aggressiv sind, ist kein Regenerieren von der Erschöpfung von selbst mehr möglich.  Die Überlegung, wie und wie oft Ihre Anwendung Transaktionen wiederholt, ist daher ein wichtiger Aspekt des Entwurfs.

### <a name="assignilpip"></a>Zuweisen einer öffentlichen IP-Adresse auf Instanzebene für jeden virtuellen Computer
So erhalten Sie das Szenario [VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich)](#ilpip).  Alle kurzlebigen Ports der für jede VM verwendeten öffentlichen IP-Adresse sind für die VM verfügbar (im Gegensatz zu Szenarien, in denen kurzlebige Ports einer öffentlichen IP-Adresse gemeinsam mit allen VMs genutzt werden, die dem jeweiligen Back-End-Pool zugeordnet sind).  Es müssen auch Nachteile berücksichtigt werden – etwa zusätzliche Kosten für öffentliche IP-Adressen und mögliche Auswirkungen des Whitelistings einer großen Anzahl von einzelnen IP-Adressen.

>[!NOTE] 
>Diese Option ist für Webworkerrollen nicht verfügbar.

### <a name="idletimeout"></a>Verwenden von Keepalives zum Zurücksetzen des Leerlauftimeouts für die ausgehende Richtung

Ausgehende Verbindungen haben einen 4-Minuten-Leerlauftimeout. Dieser ist nicht veränderbar. Sie können aber Keepalives auf der Transportschicht (TCP-Keepalives) oder Anwendungsschicht verwenden, um einen im Leerlauf befindlichen Datenfluss zu aktualisieren und den Leerlauftimeout bei Bedarf zurückzusetzen.

## <a name="discoveroutbound"></a>Ermitteln der von einer bestimmten VM verwendeten öffentlichen IP-Adresse
Es gibt viele Möglichkeiten, die öffentliche IP-Quelladresse einer ausgehenden Verbindung zu bestimmen. OpenDNS bietet einen Dienst, der die öffentliche IP-Adresse Ihrer VM anzeigen kann. Mithilfe des Befehls „nslookup“ können Sie eine DNS-Abfrage für den Namen „myip.opendns.com“ an den OpenDNS-Resolver senden. Der Dienst gibt die IP-Quelladresse zurück, die zum Senden der Abfrage verwendet wurde. Wenn Sie die folgende Abfrage auf Ihrer VM ausführen, ist die Antwort die öffentliche IP-Adresse, die für diese VM verwendet wird.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Verhindern der Konnektivität in ausgehender Richtung
Mitunter ist es nicht wünschenswert, dass eine VM einen ausgehenden Datenfluss erstellen darf, oder ggf. besteht die Anforderung, zu verwalten, welche Ziele mit ausgehenden Datenflüsse erreicht werden können, oder bei welchen Zielen eingehende Datenflüsse beginnen können. In diesem Fall verwenden Sie [Netzwerksicherheitsgruppen (NSG)](../virtual-network/virtual-networks-nsg.md) sowohl zum Verwalten der Ziele, die die VM erreichen kann, als auch des öffentlichen Ziels, das eingehende Datenflüsse initiieren kann. Wenn Sie eine Netzwerksicherheitsgruppe einer VM mit Lastenausgleich zuordnen, müssen Sie die [Standardtags](../virtual-network/virtual-networks-nsg.md#default-tags) und [Standardregeln](../virtual-network/virtual-networks-nsg.md#default-rules) beachten.

Sie müssen sicherstellen, dass die VM Anforderungen von Integritätstests von Azure Load Balancer empfangen kann. Wenn eine Netzwerksicherheitsgruppe Anforderungen von Integritätstests vom Standardtag AZURE_LOADBALANCER blockiert, misslingt Ihr VM-Integritätstests, weshalb die VM mit „Außer Betrieb“ markiert wird. Der Lastenausgleich beendet das Senden neuer Datenflüsse an diese VM.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Load Balancer Basic](load-balancer-overview.md).
- Weitere Informationen zu [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).
- Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) in Azure.
