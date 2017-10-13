---
title: "Übersicht über den Azure DDoS-Schutz Standard| Microsoft-Dokumentation"
description: Informationen zum Azure DDoS-Schutzdienst
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 0130823b6a6f5a4883ad640c9bcefc89b82b2cd1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ddos-protection-standard-overview"></a>Übersicht über den Azure DDoS-Schutz Standard

>[!IMPORTANT]
>Der Azure DDoS-Schutz Standard befindet sich derzeit in der Vorschauphase. Der DDoS-Schutz Standard wird von einer begrenzten Anzahl von Azure-Ressourcen und in ausgewählten Regionen unterstützt. Sie müssen sich [für den Dienst registrieren](http://aka.ms/ddosprotection), um während der eingeschränkten Vorschauphase den DDoS-Schutz Standard für Ihr Abonnement zu aktivieren. Nach der Registrierung setzt sich das DDoS-Team von Azure mit Ihnen in Verbindung, um Sie durch den Aktivierungsprozess zu leiten. Der DDoS-Schutz (Standard) ist in den Regionen „USA, Osten“, „USA, Westen“ und „USA, Westen-Mitte“ verfügbar. Während der Vorschauphase entstehen für Sie für die Nutzung des Diensts keine Kosten.

DDoS-Angriffe (Distributed Denial of Service, verteilte DDoS-Angriffe) stellen eines der größten Verfügbarkeits- und Sicherheitsprobleme für Kunden dar, die ihre Anwendungen in die Cloud verschieben. Ein DDoS-Angriff hat das Ziel, die Ressourcen einer Anwendung zu verbrauchen, damit sie für berechtigte Benutzer nicht mehr verfügbar ist. Jeder Endpunkt, der öffentlich über das Internet erreichbar ist, kann Ziel von DDoS-Angriffen werden.

Der Azure DDoS-Schutz in Kombination mit bewährten Anwendungsentwurfsmethoden stellt den bestmöglichen Schutz gegen diese Art von Angriffen dar. Die folgenden beiden Dienstebenen werden bereitgestellt: 

- **Azure DDoS-Schutz Basic**: Wird automatisch als Teil der Azure-Plattform ohne Aufpreis aktiviert. Die stets verfügbare Überwachung des Datenverkehrs und die Abwehr von häufig vorkommenden Angriffen auf Netzwerkebene in Echtzeit bieten die gleichen Schutzmaßnahmen wie die Onlinedienste von Microsoft.  Das gesamte weltweite Netzwerk von Azure steht für die Verteilung und Abwehr des regionsübergreifenden Angriffsdatenverkehrs zur Verfügung. 
- **Azure DDoS-Schutz Standard**: Stellt zusätzliche Funktionen zur Angriffsabwehr bereit, die speziell für virtuelle Netzwerkressourcen optimiert sind. Kann leicht aktiviert werden, und erfordert keine Änderung der Anwendung. Schutzrichtlinien werden durch dedizierte Datenverkehrsüberwachungen und Machine Learning-Algorithmen optimiert und auf öffentliche IP-Adressen angewendet, die virtuellen Netzwerkressourcen, wie z.B. Load Balancer, Application Gateway und Service Fabric-Instanzen, zugeordnet sind.  Über Azure Monitor-Ansichten steht Echtzeit-Telemetrie während eines Angriffs und für den Verlauf zur Verfügung. Mit der [Web Application Firewall für Application Gateway](https://azure.microsoft.com/services/application-gateway/) können Sie Schutz auf der Anwendungsebene hinzufügen. 

![Azure DDoS-Schutz Standard](./media/ddos-protection-overview/ddos-protection-overview-fig2.png)

Sie können den DDoS-Schutz für Entwicklungs- und Testszenarios testen. Verwenden Sie die folgenden Ressourcen für ein Feedback zu Ihren Erfahrungen:
- [Azure DDoS-Schutz im Microsoft Azure-Forum](https://feedback.azure.com/forums/905032-azure-ddos-protection) 
- [Azure DDoS-Schutz im MSDN-Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azureddosprotection)
- [Azure DDoS-Schutz in Stack Overflow](https://stackoverflow.com/tags/azure-ddos/info)

[Öffnen Sie ein Azure-Supportticket](../azure-supportability/how-to-create-azure-support-request.md), wenn Sie Supportprobleme haben.

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS-Angriffstypen, die mit DDoS-Schutz Standard abgewehrt werden

Die folgenden Arten von Angriffen können mit dem DDoS-Schutz Standard abgewehrt werden:

- **Volumetrische Angriffe**: Ziel des Angriffs ist die Überflutung der Netzwerkebene mit einer beträchtlichen Menge scheinbar berechtigten Datenverkehrs. Dazu zählen UDP-Überflutungen, Verstärkungsüberflutungen und andere Überflutungen mit gefälschten Paketen. Der DDoS-Schutz Standard wehrt diese bis zu mehrere Gigabytes großen Angriffe ab, indem es sie mithilfe des weltweiten Netzwerks von Azure automatisch absorbiert und bereinigt. 
- **Protokollangriffe**: Diese Angriffe machen den Zugriff auf ein Ziel unmöglich, indem sie eine Schwachstelle in den Ebenen 3 und 4 des Protokollstapels ausnutzen. Dazu gehören SYN-Flutangriffe, Reflektionsangriffe und andere Protokollangriffe. Der DDoS-Schutz Standard wehrt diese Angriffe ab und unterscheidet dabei zwischen schädlichem und berechtigtem Datenverkehr. Nach Interaktion mit dem Client wird der schädliche Datenverkehr gesperrt. 
- **Angriffe auf Anwendungsebene**: Das Ziel dieser Art von Angriffen sind Webanwendungspakete, um die Datenübertragung zwischen Hosts zu unterbrechen. Dazu zählen Verletzungen des HTTP-Protokolls, die Einschleusung von SQL-Befehlen, XSS-Angriffe (Cross-Site Scripting) und andere Angriffe auf Ebene 7. Die Verwendung von Application Gateway-WAF in Kombination mit dem DDoS-Schutz Standard bietet Schutz vor diesen Angriffen. 

Mit dem DDoS-Schutz Standard werden Ressourcen in einem virtuellen Netzwerk geschützt, einschließlich öffentlicher IP-Adressen, die virtuellen Computern zugeordnet sind, interner Lastenausgleichsmodule und Anwendungsgateways. In Kombination mit der SKU der Anwendungsgateway-WAF stellt der DDoS-Schutz Standard eine vollständige Abwehrfunktion für L3 bis L7 bereit.

## <a name="ddos-protection-standard-features"></a>Funktionen von DDoS-Schutz Standard

![DDoS-Funktionen](./media/ddos-protection-overview/ddos-overview-fig1.png)

Die Funktionen von DDoS-Schutz Standard umfassen: 

- **Native Plattformintegration:** Der DDoS-Schutz Standard ist standardmäßig in Azure integriert und enthält die Konfiguration über das Azure-Portal und PowerShell. Der DDoS-Schutz Standard erkennt Ihre Ressourcen und die Ressourcenkonfiguration.
- **Stets verfügbare Überwachung des Datenverkehrs:** Ihre Anwendungs-Datenverkehrsmuster werden rund um die Uhr nach Anzeichen für DDoS-Angriffe überwacht. Abwehrmaßnahmen werden bei Überschreitung der Schutzrichtlinien ausgeführt.
- **Sofort einsetzbarer Schutz:** Dank vereinfachter Konfiguration sind alle Ressourcen in einem virtuellen Netzwerk sofort geschützt, sobald der DDoS-Schutz Standard aktiviert wird. Es sind weder Benutzereingriffe noch Benutzerdefinitionen erforderlich. Der DDoS-Schutz Standard wehrt einen Angriff automatisch ab, sobald er entdeckt wurde.
- **Adaptive Optimierung:** Dank einer intelligenten Profilerstellung lernt die Funktion den Datenverkehr Ihrer Anwendung kontinuierlich besser kennen. Auf dieser Basis wird das Profil ausgewählt und aktualisiert, das am besten zu Ihrem Dienst passt. Das Profil passt sich den Veränderungen des Datenverkehrs mit der Zeit an.
- **L3- bis L7-Schutz mit einem Anwendungsgateway:** Die Funktionen der Anwendungsgateway-WAF stellen einen vollständigen DDoS-Stapelschutz bereit.
- **Umfangreiche Angriffsabwehr:** Über 60 verschiedene Angriffstypen können mit einer weltweiten Kapazität zum Schutz vor den größten bekannten DDoS-Angriffen abgewehrt werden. 
- **Angriffsmetriken:** Mit Azure Monitor kann auf eine Zusammenfassung der Metriken für jeden Angriff zugegriffen werden.
- **Angriffswarnungen:** Mit integrierten Angriffsmetriken können Warnungen am Anfang und Ende eines Angriffs sowie währenddessen konfiguriert werden. Warnungen werden in Ihre Betriebssoftware, wie z.B. OMS, Splunk, Azure Storage, E-Mail und Azure-Portal, integriert.
- **Kostengarantie:** Dienstguthaben für Datenübertragung und horizontale Anwendungsskalierung bei dokumentierten DDoS-Angriffen.

## <a name="ddos-protection-standard-mitigation"></a>Abwehrfunktion von DDoS-Schutz Standard

Der DDoS-Schutzdienst von Microsoft überwacht die tatsächliche Auslastung des Datenverkehrs und vergleicht sie ständig mit den Schwellenwerten der DDoS-Richtlinie. Bei Überschreitung des Schwellenwerts für den Datenverkehr wird die DDoS-Abwehr automatisch eingeleitet. Sinkt der Datenverkehr wieder unter den Schwellenwert, wird die Abwehr beendet.

Während des Abwehrvorgangs wird der Datenverkehr in Richtung der geschützten Ressource vom DDoS-Schutzdienst umgeleitet und mehrfach überprüft. Diese Überprüfungen umfassen in der Regel Folgendes:

- Sicherstellen, dass Pakete den Internetspezifikationen entsprechen und nicht falsch formatiert sind.
- Interaktion mit dem Client, um zu ermitteln, ob es sich möglicherweise um ein gefälschtes Paket handelt (z.B. SYN-Authentifizierung, SYN-Cookie oder Löschen eines Pakets, damit die Quelle es erneut übermitteln muss).
- Übertragungsratenlimits für Pakete, wenn keine andere Erzwingungsmethode ausgeführt werden kann.

Der DDoS-Schutz sperrt Angriffsdatenverkehr und leitet den verbleibenden Datenverkehr an das vorgesehene Ziel weiter. Innerhalb weniger Minuten nach Angriffserkennung werden Sie mithilfe der Metriken von Azure Monitor benachrichtigt. Durch Konfigurieren der Protokollierung der DDoS-Schutztelemetrie können Sie für eine zukünftige Analyse die Protokolle in die verfügbaren Optionen schreiben. Metrische Daten werden derzeit in Azure Monitor für den DDoS-Schutz Standard 30 Tage lang beibehalten.

Es wird nicht empfohlen, eigene DDoS-Angriffe zu simulieren. Kunden können stattdessen über den Supportkanal einen simulierten DDoS-Angriff anfordern, der von Azure-Netzwerken ausgeführt wird. Ein Supporttechniker wird sich mit Ihnen wegen der Einzelheiten zum DDoS-Angriff in Verbindung setzen (Ports, Protokolle, Ziel-IP-Adressen) und einen Zeitpunkt für den Test verabreden.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die Verwaltung von DDoS-Schutz Standard mit [Azure PowerShell](ddos-protection-manage-ps.md) oder dem [Azure-Portal](ddos-protection-manage-portal.md).