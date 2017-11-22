---
title: "Übersicht über den Azure DDoS-Schutz Standard| Microsoft-Dokumentation"
description: Informationen zum Azure DDoS-Schutzdienst
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 6b26108b000bac56fe7d49a3a634f2be9d7543a8
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="azure-ddos-protection-standard-overview"></a>Übersicht über den Azure DDoS-Schutz Standard

DDoS-Angriffe (Distributed Denial of Service) stellen eines der größten Verfügbarkeits- und Sicherheitsprobleme für Kunden dar, die ihre Anwendungen in die Cloud verschieben. Ein DDoS-Angriff hat das Ziel, die Ressourcen einer Anwendung zu verbrauchen, damit sie für berechtigte Benutzer nicht mehr verfügbar ist. Jeder Endpunkt, der öffentlich über das Internet erreichbar ist, kann Ziel von DDoS-Angriffen werden.

Der Azure DDoS-Schutz in Kombination mit bewährten Anwendungsentwurfsmethoden stellt den bestmöglichen Schutz gegen DDoS-Angriffe dar. Azure DDoS-Schutz bietet die folgenden Dienstebenen: 

- **Azure DDoS-Schutz Basic**: Wird automatisch als Teil der Azure-Plattform ohne Aufpreis aktiviert. Die stets verfügbare Überwachung des Datenverkehrs und die Abwehr von häufig vorkommenden Angriffen auf Netzwerkebene in Echtzeit bieten die gleichen Schutzmaßnahmen wie die Onlinedienste von Microsoft. Das gesamte weltweite Netzwerk von Azure steht für die Verteilung und Abwehr des regionsübergreifenden Angriffsdatenverkehrs zur Verfügung. Schutz wird für [öffentliche Azure-IP-Adressen](virtual-network-public-ip-address.md) mit IPv4 und IPv6 bereitgestellt.
- **Azure DDoS-Schutz Standard**: Stellt zusätzliche Funktionen zur Angriffsabwehr bereit, die speziell für virtuelle Azure-Netzwerkressourcen optimiert sind. Kann leicht aktiviert werden, und erfordert keine Änderung der Anwendung. Schutzrichtlinien werden durch dedizierte Datenverkehrsüberwachungen und Machine Learning-Algorithmen optimiert und auf öffentliche IP-Adressen angewendet, die in virtuellen Netzwerken bereitgestellten Ressourcen, wie z. B. Azure Load Balancer, Azure Application Gateway und Azure Service Fabric-Instanzen, zugeordnet sind. Über Azure Monitor-Ansichten steht Echtzeit-Telemetrie während eines Angriffs und für den Verlauf zur Verfügung. Mit der [Web Application Firewall für Application Gateway](https://azure.microsoft.com/services/application-gateway) können Sie Schutz auf der Anwendungsebene hinzufügen. Schutz wird für [öffentliche Azure-IP-Adressen](virtual-network-public-ip-address.md) mit IPv4 bereitgestellt. 

![Azure DDoS-Schutz Standard](./media/ddos-protection-overview/ddos-protection-overview-fig2.png)

> [!IMPORTANT]
> Der Azure DDoS-Schutz Standard befindet sich derzeit in der Vorschauphase. Schutz wird für jede Azure-Ressource bereitgestellt, der eine öffentliche Azure-IP-Adresse zugewiesen ist, wie z. B. virtuelle Computer, Lastenausgleichsmodule und Anwendungsgateways. Sie müssen sich für den Dienst [registrieren](http://aka.ms/ddosprotection), bevor Sie den DDoS-Schutz Standard für Ihr Abonnement aktivieren können. Nach der Registrierung setzt sich das DDoS-Team von Azure mit Ihnen in Verbindung und führt Sie durch den Aktivierungsprozess. Der DDoS-Schutz Standard ist nur in den Regionen USA (Osten), USA (Osten) 2, USA (Westen), USA (Westen-Mitte), Europa (Norden), Europa (Westen), Japan (Westen), Japan (Osten), Asien (Osten) und Asien (Südosten) verfügbar. Während der Vorschauphase entstehen für Sie für die Nutzung des Diensts keine Kosten.

Wir empfehlen Ihnen, den DDoS-Schutz Standard in einer Entwicklungs-, Test- oder Produktionsumgebung zu testen. Nutzen Sie die folgenden Ressourcen, um Ihr Feedback zu Ihren Erfahrungen mitzuteilen:
- [Azure DDoS-Schutz im Microsoft Azure-Forum](https://feedback.azure.com/forums/905032-azure-ddos-protection) 
- [Azure DDoS-Schutz im MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azureddosprotection)
- [Azure DDoS-Schutz in Stack Overflow](https://stackoverflow.com/tags/azure-ddos/info)

[Öffnen Sie ein Azure-Supportticket](../azure-supportability/how-to-create-azure-support-request.md), wenn Sie Supportprobleme haben. In der Vorschauversion wird Support für den DDoS-Schutz Standard nach bestem Bemühen bereitgestellt.

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS-Angriffstypen, die mit DDoS-Schutz Standard abgewehrt werden

Die folgenden Arten von Angriffen können mit dem DDoS-Schutz Standard abgewehrt werden:

- **Volumetrische Angriffe**: Ziel des Angriffs ist die Überflutung der Netzwerkebene mit einer beträchtlichen Menge scheinbar berechtigten Datenverkehrs. Dazu zählen UDP-Überflutungen, Verstärkungsüberflutungen und andere Überflutungen mit gefälschten Paketen. Der DDoS-Schutz Standard wehrt diese bis zu mehrere Gigabytes großen Angriffe ab, indem er sie mithilfe des weltweiten Netzwerks von Azure automatisch absorbiert und bereinigt. 
- **Protokollangriffe**: Diese Angriffe machen den Zugriff auf ein Ziel unmöglich, indem sie eine Schwachstelle in den Ebenen 3 und 4 des Protokollstapels ausnutzen. Dazu gehören SYN-Flutangriffe, Reflektionsangriffe und andere Protokollangriffe. Der DDoS-Schutz Standard wehrt diese Angriffe ab und unterscheidet dabei zwischen schädlichem und berechtigtem Datenverkehr. Nach Interaktion mit dem Client wird der schädliche Datenverkehr gesperrt. 
- **Angriffe auf Anwendungsebene**: Das Ziel dieser Art von Angriffen sind Webanwendungspakete, um die Datenübertragung zwischen Hosts zu unterbrechen. Dazu zählen Verletzungen des HTTP-Protokolls, die Einschleusung von SQL-Befehlen, XSS-Angriffe (Cross-Site Scripting) und andere Angriffe auf Ebene 7. Verwenden Sie die [Web Application Firewall von Azure Application Gateway](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) in Kombination mit dem DDoS-Schutz Standard zum Schutz vor diesen Angriffen. 

Mit dem DDoS-Schutz Standard werden Ressourcen in einem virtuellen Netzwerk geschützt, einschließlich öffentlicher IP-Adressen, die virtuellen Computern zugeordnet sind, interner Lastenausgleichsmodule und Anwendungsgateways. In Kombination mit der Web Application Firewall von Application Gateway stellt der DDoS-Schutz Standard eine vollständige Abwehrfunktion für Ebene 3 bis Ebene 7 bereit.

## <a name="ddos-protection-standard-features"></a>Funktionen von DDoS-Schutz Standard

![DDoS-Funktionen](./media/ddos-protection-overview/ddos-overview-fig1.png)

Die Funktionen von DDoS-Schutz Standard umfassen: 

- **Native Plattformintegration:** Ist standardmäßig in Azure integriert und enthält die Konfiguration über das Azure-Portal und PowerShell. Der DDoS-Schutz Standard erkennt Ihre Ressourcen und die Ressourcenkonfiguration.
- **Stets verfügbare Überwachung des Datenverkehrs:** Die Datenverkehrsmuster Ihrer Anwendungen werden 24 Stunden am Tag und 7 Tage die Woche nach Anzeichen für DDoS-Angriffe überwacht. Abwehrmaßnahmen werden bei Überschreitung der Schutzrichtlinien ausgeführt.
- **Sofort einsetzbarer Schutz:** Dank vereinfachter Konfiguration sind alle Ressourcen in einem virtuellen Netzwerk sofort geschützt, sobald der DDoS-Schutz Standard aktiviert wird. Es sind weder Benutzereingriffe noch Benutzerdefinitionen erforderlich. Der DDoS-Schutz Standard wehrt einen Angriff sofort automatisch ab, sobald er entdeckt wurde.
- **Adaptive Optimierung:** Dank einer intelligenten Profilerstellung lernt die Funktion den Datenverkehr Ihrer Anwendung kontinuierlich besser kennen. Auf dieser Basis wird das Profil ausgewählt und aktualisiert, das am besten zu Ihrem Dienst passt. Das Profil passt sich den Veränderungen des Datenverkehrs mit der Zeit an.
- **Schutz für Ebene 3 bis Ebene 7:** Bietet vollständigen DDoS-Stapelschutz bei Verwendung mit einem Anwendungsgateway.
- **Umfangreiche Angriffsabwehr:** Über 60 verschiedene Angriffstypen können mit einer weltweiten Kapazität zum Schutz vor den größten bekannten DDoS-Angriffen abgewehrt werden. 
- **Angriffsmetriken:** Mit Azure Monitor kann auf eine Zusammenfassung der Metriken für jeden Angriff zugegriffen werden.
- **Angriffswarnungen:** Mit integrierten Angriffsmetriken können Warnungen am Anfang und Ende eines Angriffs sowie währenddessen konfiguriert werden. Warnungen werden in Ihre Betriebssoftware, wie z. B. Microsoft Operations Management Suite, Splunk, Azure Storage, E-Mail, und das Azure-Portal integriert.
- **Kostengarantie:** Dienstguthaben für Datenübertragung und horizontale Anwendungsskalierung bei dokumentierten DDoS-Angriffen.

## <a name="ddos-protection-standard-mitigation"></a>Abwehrfunktion von DDoS-Schutz Standard

Der DDoS-Schutzdienst von Microsoft überwacht die tatsächliche Auslastung des Datenverkehrs und vergleicht sie ständig mit den Schwellenwerten der DDoS-Richtlinie. Bei Überschreitung des Schwellenwerts für den Datenverkehr wird die DDoS-Abwehr automatisch eingeleitet. Sinkt der Datenverkehr wieder unter den Schwellenwert, wird die Abwehr beendet.

Während des Abwehrvorgangs wird der an die geschützte Ressource gesendete Datenverkehr vom DDoS-Schutzdienst umgeleitet und mehrfach überprüft. Diese Überprüfungen umfassen in der Regel Folgendes:

- Sicherstellen, dass Pakete den Internetspezifikationen entsprechen und nicht falsch formatiert sind.
- Interaktion mit dem Client, um zu ermitteln, ob es sich bei dem Datenverkehr möglicherweise um ein gefälschtes Paket handelt (z. B. SYN-Authentifizierung, SYN-Cookie oder Löschen eines Pakets, damit die Quelle es erneut übermitteln muss).
- Übertragungsratenlimits für Pakete, wenn keine andere Erzwingungsmethode ausgeführt werden kann.

Der DDoS-Schutz sperrt Angriffsdatenverkehr und leitet den verbleibenden Datenverkehr an das vorgesehene Ziel weiter. Innerhalb weniger Minuten nach Angriffserkennung werden Sie mithilfe der Metriken von Azure Monitor benachrichtigt. Durch Konfigurieren der Protokollierung der DDoS-Schutztelemetrie können Sie für eine zukünftige Analyse die Protokolle in die verfügbaren Optionen schreiben. Metrische Daten werden derzeit in Azure Monitor für den DDoS-Schutz Standard 30 Tage lang beibehalten.

Es wird nicht empfohlen, eigene DDoS-Angriffe zu simulieren. Kunden können stattdessen über den Supportkanal einen simulierten DDoS-Angriff anfordern, der von Azure-Netzwerken ausgeführt wird. Ein Supporttechniker wird sich mit Ihnen wegen der Einzelheiten zum DDoS-Angriff in Verbindung setzen (Ports, Protokolle, Ziel-IP-Adressen) und einen Zeitpunkt für den Test verabreden.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die Verwaltung von DDoS-Schutz Standard mit [Azure PowerShell](ddos-protection-manage-ps.md) oder dem [Azure-Portal](ddos-protection-manage-portal.md).
