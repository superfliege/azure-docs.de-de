---
title: Multimasterunterstützung auf globaler Ebene mit Azure Cosmos DB | Microsoft-Dokumentation
description: ''
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 2446fac7526015d11737529c26d54e910643b750
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="multi-master-at-global-scale-with-azure-cosmos-db"></a>Multimasterunterstützung auf globaler Ebene mit Azure Cosmos DB 
 
Die Entwicklung von global verteilten Anwendungen, die mit lokalen Latenzen reagieren, bei gleichzeitiger Beibehaltung konsistenter Ansichten von Daten weltweit stellt eine große Herausforderung dar. Kunden verwenden global verteilte Datenbanken, da sie die Datenzugriffslatenz verbessern, hohe Datenverfügbarkeit erreichen und eine garantierte Notfallwiederherstellung sicherstellen sowie ihre Geschäftsanforderungen erfüllen möchten. Multimasterunterstützung in Azure Cosmos DB bietet Hochverfügbarkeit (99,999 %), Latenz im einstelligen Millisekundenbereich zum Schreiben von Daten und Skalierbarkeit mit integrierter umfassender und flexibler Unterstützung der Konfliktlösung. Diese Features erleichtern die Entwicklung von global verteilten Anwendungen erheblich. Für global verteilte Anwendungen ist Multimasterunterstützung entscheidend. 

![Multimasterarchitektur](./media/multi-region-writers/multi-master-architecture.png)

Mit der Multimasterunterstützung in Azure Cosmos DB können Sie Schreibvorgänge in auf der ganzen Welt verteilten Datencontainern (z.B. Sammlungen, Diagramme oder Tabellen) ausführen. Sie können Daten in jeder Region aktualisieren, die Ihrem Datenbankkonto zugeordnet ist. Diese Datenaktualisierungen können asynchron verteilt werden. Neben dem schnellen Zugriff und der Schreiblatenz für Ihre Daten bietet Multimasterunterstützung zudem eine praktische Lösung bei Problemen mit Failover und Lastenausgleich. Zusammenfassend erhalten Sie mit Azure Cosmos DB eine Schreiblatenz von unter 10 ms im 99. Perzentil auf der ganzen Welt, eine Schreib- und Leseverfügbarkeit von 99,999 % auf der ganzen Welt und die Möglichkeit, den Schreib- und Lesedurchsatz auf der ganzen Welt zu skalieren.   

## <a name="a-simple-multi-master-example--content-publishing"></a>Ein einfaches Multimasterbeispiel – Inhaltsveröffentlichung  

Sehen wir uns ein praxisnahes Szenario an, in dem die Verwendung der Multimasterunterstützung mit Azure Cosmos DB beschrieben wird. Stellen Sie sich eine Plattform für die Inhaltsveröffentlichung vor, die auf Azure Cosmos DB basiert. Hier sind einige Anforderungen aufgeführt, die von dieser Plattform erfüllt werden müssen, um sowohl für Herausgeber als auch für Verbraucher für eine hohe Benutzerfreundlichkeit zu sorgen. 

* Autoren und Abonnenten sind weltweit verteilt.  

* Autoren müssen Artikel in ihrer lokalen (nächstgelegenen) Region veröffentlichen (schreiben).  

* Autoren verfügen für ihre Artikel über Leser/Abonnenten, die weltweit verteilt sind.  

* Abonnenten sollten eine Benachrichtigung erhalten, wenn neue Artikel veröffentlicht werden.  

* Abonnenten müssen Artikel aus ihrer lokalen Region lesen können. Außerdem sollte es möglich sein, Rezensionen für diese Artikel hinzuzufügen.  

* Alle Benutzer, einschließlich des Autors der Artikel, sollten alle Rezensionen anzeigen können, die an Artikel einer lokalen Region angefügt sind.  

Wenn wir von Millionen von Verbrauchern und Herausgebern mit Milliarden von Artikeln ausgehen, wird schnell klar, dass wir die Skalierungsprobleme lösen und den lokalen Zugriff sicherstellen müssen. Ein solcher Anwendungsfall eignet sich perfekt für Multimasterunterstützung in Azure Cosmos DB. 

## <a name="benefits-of-having-multi-master-support"></a>Vorteile der Multimasterunterstützung 

Die Multimasterunterstützung ist entscheidend für global verteilte Anwendungen. Multimasterunterstützung setzt sich aus [mehreren Masterregionen](distribute-data-globally.md) zusammen, die gleichmäßig in ein Modell mit globalem Schreibzugriff (Aktiv/Aktiv-Muster) einbezogen sind. Gleichzeitig wird sichergestellt, dass Daten jederzeit dort verfügbar sind, wo Sie sie benötigen. Die für eine einzelne Region vorgenommenen Aktualisierungen werden asynchron an alle anderen Regionen verteilt (die wiederum selbst Masterregionen sind). In Azure Cosmos DB-Regionen, die als Masterregionen in einer Multimasterkonfiguration fungieren, werden die Daten aller Replikate automatisch konvergiert. Zudem wird die [globale Konsistenz und Datenintegrität ](consistency-levels.md) sichergestellt. In der folgenden Abbildung ist die Lese-/Schreibreplikation für einen einzelnen Master und Multimaster dargestellt.

![Einzelner Master und Multimaster](./media/multi-region-writers/single-vs-multi-master.png)

Die selbstständige Implementierung von Multimasterunterstützung stellt für Entwickler eine zusätzliche Belastung dar. Großkunden, die versuchen, Multimasterunterstützung selbstständig zu implementieren, können Hunderte von Stunden damit verbringen, eine Multimasterkonfiguration weltweit einzurichten und zu testen. Zudem verfügen viele Kunden über zahlreiche Techniker, deren einzige Aufgabe darin besteht, die Multimasterreplikation zu überwachen und zu verwalten. Die Erstellung und Verwaltung der Multimastereinrichtung beansprucht Zeit und Ressourcen, die für die innovative Entwicklung der Anwendung selbst eingesetzt werden könnten, und führt zu wesentlich höheren Kosten. Azure Cosmos DB bietet eine sofort verfügbare Unterstützung für Multimasterunterstützung und entlastet Entwickler.  

Zusammenfassend bietet Multimasterunterstützung die folgenden Vorteile:

* **Bessere Notfallwiederherstellung und Schreibverfügbarkeit und besseres Failover:** Mit mehreren Mastern kann die Hochverfügbarkeit einer unternehmenskritischen Datenbank in größerem Ausmaß beibehalten werden. Beispielsweise kann eine Multimaster-Datenbank Daten von einer Region in eine Failoverregion replizieren, wenn die primäre Region aufgrund eines Ausfalls oder eines regionalen Notfalls nicht mehr verfügbar ist. Eine solche Failoverregion dient als voll funktionsfähige Masterregion zur Unterstützung der Anwendung. Multimasterunterstützung bietet einen größeren Schutz der „Überlebensfähigkeit“ im Hinblick auf Naturkatastrophen, Stromausfälle oder Sabotage, da die übrigen Regionen sich in mehreren geografisch unterschiedlichen Mastern mit einer garantierten Schreibverfügbarkeit von über 99,999 % befinden können. 

* **Verbesserte Schreiblatenz für Endbenutzer:** Je näher sich Ihre Daten (die Sie bereitstellen) an den Endbenutzern befinden, desto besser ist die Benutzerfreundlichkeit. Wenn Sie beispielsweise über Benutzer in Europa verfügen, sich Ihre Datenbank jedoch in den USA oder Australien befindet, ergibt sich eine zusätzliche Latenz von 140 ms bzw. 300 ms für die jeweilige Region. Verzögerungen sind für viele gängige Spiele, Anforderungen im Bankgeschäft oder interaktive Anwendungen (Web- oder Mobilanwendungen) nicht akzeptabel. Latenz spielt eine große Rolle bei der Wahrnehmung der Kunden einer hochwertigen Erfahrung und beeinflusst das Verhalten von Benutzern nachweisbar in nennenswertem Ausmaß. Mit der beständigen Optimierung der Technologie und insbesondere durch die Einführung von AR, VR und MR, die eine immer immersivere und realistischere Erfahrung erforderlich machen, müssen Entwickler jetzt Softwaresysteme mit stringenten Latenzanforderungen erstellen. Daher werden lokal verfügbare Anwendungen und Daten (Inhalte für die Anwendungen) zunehmend wichtiger. Mit Multimasterunterstützung in Azure Cosmos DB ist die Leistung so schnell wie bei normalen Lese- und Schreibvorgängen und wird global durch geografische Verteilung verbessert.  

* **Verbesserte Schreibskalierbarkeit und optimierter Schreibdurchsatz:** Multimasterunterstützung bietet einen höheren Durchsatz und eine größere Auslastung bei gleichzeitiger Bereitstellung mehrerer Konsistenzmodelle mit Garantien für Richtigkeit sowie Sicherung durch SLAs. 

  ![Skalierung des Schreibdurchsatzes mit Multimasterunterstützung](./media/multi-region-writers/scale-write-throughput.png)

* **Bessere Unterstützung für nicht verbundene Umgebungen (z.B. Edgegeräte):** Mit Multimasterunterstützung können Benutzer alle oder eine Teilmenge der Daten von einem Edgegerät in eine nächstgelegene Region in einer nicht verbundenen Umgebung replizieren. Dieses Szenario ist typisch für Sales Force Automation-Systeme, bei denen eine Teilmenge der Daten auf dem Laptop der einzelnen Vertriebsmitarbeiter (einem nicht verbundenen Gerät) gespeichert werden. Masterregionen in der Cloud auf der ganzen Welt können als Ziel der Kopie von den Remoteedgegeräten dienen.  

* **Lastenausgleich:** Mit Multimasterunterstützung kann die Auslastung für die Anwendung ausgeglichen werden, indem Benutzer und Workloads von einer stark ausgelasteten Region in Regionen verschoben werden, in denen die Auslastung gleichmäßig verteilt ist. Die Schreibkapazität kann problemlos erweitert werden, indem eine neue Region hinzugefügt wird und dann einige Schreibvorgänge in die neue Region verschoben werden. 

* **Bessere Nutzung der bereitgestellten Kapazität:** Mit Multimasterunterstützung können Sie für schreibintensive und gemischte Workloads die bereitgestellte Kapazität in mehreren Regionen auslasten.  In einigen Fällen können Sie Lese- und Schreibvorgänge gleichmäßiger neu verteilen, sodass weniger Durchsatz bereitgestellt werden muss und somit für Kunden eine größere Kostenersparnis ermöglicht wird.  

* **Einfachere und stabilere App-Architekturen:** Anwendungen, die in eine Multimasterkonfiguration verschoben werden, erhalten eine garantierte Datenstabilität.  Aufgrund der in Azure Cosmos DB verborgenen Komplexität können das Anwendungsdesign und die Anwendungsarchitektur erheblich vereinfacht werden. 

* **Risikofreie Failovertests:** Failovertests führen zu keinerlei Beeinträchtigungen des Schreibdurchsatzes. In Multimasterszenarien sind alle anderen Regionen vollständige Master, sodass ein Failover keine großen Auswirkungen auf den Schreibdurchsatz hat.  

* **Niedrigere Gesamtkosten und DevOps:** Die Erfüllung der Ziele im Hinblick auf die Skalierbarkeit, Leistung, globale Verteilung und Wiederherstellungszeit ist häufig kostenintensiv aufgrund aufwendiger Add-Ons oder der Verwaltung einer Sicherungsinfrastruktur, die sich bis zum Eintreten eines Notfalls im Ruhezustand befindet. Da Multimasterunterstützung in Azure Cosmos DB durch branchenführende SLAs unterstützt wird, müssen Entwickler die Back-End-Verbindungslogik nicht mehr selbst erstellen und verwalten und können so ihre unternehmenswichtigen Workloads ausführen. 

## <a name="use-cases-where-multi-master-support-is-needed"></a>Anwendungsfälle mit erforderlicher Multimasterunterstützung

Es gibt zahlreiche Anwendungsfälle für Multimasterunterstützung in Azure Cosmos DB: 

* **IoT:** Multimasterunterstützung in Azure Cosmos DB ermöglicht eine einfachere verteilte Implementierung der IoT-Datenverarbeitung. In geografisch verteilten Edgebereitstellungen, die CRDT (Conflict-Free Replicated Data Types, konfliktfrei replizierte Datentypen) verwenden, müssen häufig Zeitreihendaten von mehreren Standorten nachverfolgt werden. Jedes Gerät kann in einer der nächstgelegenen Regionen verwaltet werden. Zudem kann ein Gerät bewegt (z.B. in einem Fahrzeug) und dann dynamisch einer neuen Region zugewiesen werden, um in diese zu schreiben.  

* **E-Commerce:** Die Sicherstellung einer umfassenden Benutzerfreundlichkeit in E-Commerce-Szenarien erfordert Hochverfügbarkeit und Stabilität bei Fehlerszenarien. Für den Fall, dass in einer Region Fehler auftreten, müssen Benutzersitzungen, Warenkörbe und aktive Wunschlisten ohne Verlust von Statusdaten nahtlos von einer anderen Region übernommen werden. In der Zwischenzeit müssen die vom Benutzer vorgenommenen Aktualisierungen entsprechend verarbeitet werden (z.B. müssen Hinzufügungen und Löschungen aus dem Warenkorb übertragen werden). Mit Multimasterunterstützung können in Azure Cosmos DB derartige Szenarien mit einem nahtlosen Übergang zwischen aktiven Regionen korrekt verarbeitet werden, während gleichzeitig aus Sicht des Benutzers eine konsistente Ansicht der Daten beibehalten wird. 

* **Betrugs- und Anomalieerkennung:** Häufig sind Anwendungen, die Benutzer- oder Kontoaktivitäten überwachen, global verteilt und müssen mehrere Ereignisse parallel nachverfolgen. Beim Erstellen und Verwalten von Ergebnissen für einen Benutzer müssen die Ergebnisse bei Aktionen aus verschiedenen geografischen Regionen parallel aktualisiert werden, um die Risikometriken überwachen zu können. Mit Azure Cosmos DB kann sichergestellt werden, dass Entwickler keine Konfliktszenarien auf Anwendungsebene behandeln müssen. 

* **Kollaboration:** für Anwendungen, für die eine Rangfolge basierend auf der Beliebtheit von Artikeln (z.B. Waren im Verkauf oder zu verwendende Medien) erstellt wird. Das Nachverfolgen der Beliebtheit in verschiedenen geografischen Regionen kann komplex werden, vor allem wenn Abgaben gezahlt oder Werbeentscheidungen in Echtzeit getroffen werden müssen. Mit Azure Cosmos DB können Entwickler ohne großen Aufwand und ohne Kompromisse bei der Latenz Features im Hinblick auf Rangfolge, Sortierung und Berichterstellung in vielen verschiedenen Regionen weltweit in Echtzeit bereitstellen. 

* **Messung:** Mithilfe der Multimasterunterstützung in Azure Cosmos DB können Zählung und Regulierung der Verwendung (z.B. API-Aufrufe, Transaktionen pro Sekunde oder in Anspruch genommene Minuten) mühelos global implementiert werden. Die integrierte Konfliktlösung gewährleistet die Genauigkeit der Zählungen sowie die Regulierung in Echtzeit. 

* **Personalisierung:** Ob Sie nun geografisch verteilte Indikatoren verwalten, die Aktionen auslösen (z.B. Prämien für Treuepunkte), oder personalisierte Ansichten für Benutzersitzungen implementieren, ermöglichen die von Azure Cosmos DB bereitgestellte Hochverfügbarkeit und die vereinfachte geografisch verteilte Zählung Anwendungen mit hoher Leistung und einfacher Verwaltung. 

## <a name="conflict-resolution-with-multi-master"></a>Konfliktlösung mit Multimasterunterstützung 

Bei Multimasterunterstützung besteht die Herausforderung häufig darin, dass zwei (oder mehr) Replikate desselben Datensatzes gleichzeitig von verschiedenen Writern in mehreren unterschiedlichen Regionen aktualisiert werden können. Gleichzeitige Schreibvorgänge können zu zwei verschiedenen Versionen desselben Datensatzes ohne vorhandene integrierte Konfliktlösung führen. Die Anwendung muss dann die Konfliktlösung ausführen, um diese Inkonsistenz zu beheben.  

**Beispiel:** Angenommen, Sie verwenden Azure Cosmos DB als persistenten Speicher für eine Warenkorb-Anwendung, die in den zwei Regionen „USA, Osten“ und „USA, Westen“ bereitgestellt wird.  Ein Benutzer in San Francisco fügt seinem Warenkorb zu einem bestimmten Zeitpunkt einen Artikel hinzu (z.B. ein Buch), während ungefähr zum gleichen Zeitpunkt durch einen Bestandsverwaltungsprozess in der Region „USA, Osten“ ein anderer Artikel im Warenkorb dieses Benutzers (z.B ein neues Telefon) aufgrund einer Lieferantenbenachrichtigung, laut der sich die Markteinführung verzögert, annulliert wird. Zum Zeitpunkt T1 unterscheiden sich dann die Warenkorbdatensätze in den beiden Regionen. In der Datenbank werden die internen Replikations- und Konfliktlösungsmechanismen verwendet, um diese Inkonsistenz zu beheben, und schließlich wird eine der beiden Versionen des Warenkorbs ausgewählt. Aufgrund der Verwendung der Konfliktlösungsheuristiken, die am häufigsten von Multimaster-Datenbanken angewandt werden (z.B. Last-Write-Wins; letzter Schreibzugriff hat Priorität), kann der Benutzer oder die Anwendung nicht vorhersagen, welche Version ausgewählt wird. In beiden Fällen gehen Daten verloren, oder es tritt ein unerwartetes Verhalten auf. Wenn die Version der Region „USA, Osten“ ausgewählt wird, geht die Auswahl des Benutzers eines neuen Kaufartikels (d.h. das Buch) verloren. Wenn die Version der Region „USA, Westen“ ausgewählt wird, befindet sich der zuvor ausgewählte Artikel (d.h. das Telefon) weiterhin im Warenkorb. In beiden Fällen gehen Informationen verloren. Schließlich wird durch einen anderen Prozess zur Überprüfung des Warenkorbs zwischen dem Zeitpunkt T1 und dem Zeitpunkt T2 auch das nicht deterministische Verhalten festgestellt. Beispielsweise werden bei einem Hintergrundprozess, durch den das entsprechende Warenlager ausgewählt wird und die Lieferkosten des Warenkorbs aktualisiert werden, Ergebnisse generiert, die in Konflikt mit den Inhalten des Warenkorbs stehen. Wenn der Prozess in der Region „USA, Westen“ ausgeführt wird und Alternative 1 zur Realität wird, werden die Lieferkosten für zwei Artikel berechnet, auch wenn der Warenkorb bald nur einen Artikel (das Buch) enthält. 

Azure Cosmos DB implementiert die Logik zur Behandlung von Schreibkonflikten innerhalb der Datenbank-Engine. Azure Cosmos DB bietet eine **umfassende und flexible Unterstützung der Konfliktlösung** durch Bereitstellung verschiedener Konfliktlösungsmodelle, z.B. „Automatisch“ (CRDT – Conflict-Free Replicated Data Types), „Last-Write-Wins“ (LWW), „Benutzerdefiniert“ (Gespeicherte Prozedur) und „Manuell“ zur automatischen Konfliktlösung. Die Konfliktlösungsmodelle bieten Garantien für Richtigkeit und Konsistenz, sodass sich Entwickler nicht mehr mit Konsistenz, Verfügbarkeit, Leistung, Replikationslatenz und komplexen Kombinationen von Ereignissen bei geografischen Failovern und regionsübergreifenden Schreibkonflikten auseinandersetzen müssen.  

  ![Multimaster-Konfliktlösung](./media/multi-region-writers/multi-master-conflict-resolution-blade.png)

In Azure Cosmos DB werden drei Arten von Konfliktlösungsmodellen bereitgestellt. Die Konfliktlösungsmodelle weisen folgende Semantik auf: 

**Automatisch:** Dies ist die Standardrichtlinie zur Konfliktlösung. Bei Auswahl dieser Richtlinie löst Azure Cosmos DB die miteinander in Konflikt stehenden Aktualisierungen auf Serverseite automatisch auf und gewährleistet eine starke letztliche Konsistenz. Intern implementiert Azure Cosmos DB die automatische Konfliktlösung durch die Nutzung konfliktfrei replizierter Datentypen (CRDTs) innerhalb der Datenbank-Engine.  

**Last-Write-Wins (LWW):** Durch Auswahl dieser Richtlinie können Sie Konflikte basierend auf der systemdefinierten Eigenschaft für einen synchronisierten Zeitstempel oder auf einer benutzerdefinierten Eigenschaft auflösen, die für die in Konflikt stehende Version der Datensätze definiert wurde. Die Konfliktlösung erfolgt auf Serverseite, und die Version mit dem neuesten Zeitstempel gewinnt.  

**Benutzerdefiniert:** Sie können eine anwendungsdefinierte Logik zur Konfliktlösung registrieren, indem Sie eine gespeicherte Prozedur registrieren. Die gespeicherte Prozedur wird bei Erkennung eines Aktualisierungskonflikts im Rahmen einer Datenbanktransaktion auf Serverseite aufgerufen. Wenn Sie die Option auswählen, aber keine gespeicherte Prozedur registrieren (oder wenn die gespeicherte Prozedur zur Laufzeit eine Ausnahme auslöst), können Sie über den Konfliktfeed auf alle in Konflikt stehenden Versionen zugreifen und sie einzeln lösen.  

## <a name="next-steps"></a>Nächste Schritte  

In diesem Artikel haben Sie erfahren, wie Sie mehrere global verteilte Master mit Azure Cosmos DB verwenden. Sehen Sie sich als Nächstes folgende Artikel an: 

* [Erfahren Sie, wie Azure Cosmos DB die globale Verteilung unterstützt](distribute-data-globally.md).  

* [Informieren Sie sich über automatische und manuelle Failover in Azure Cosmos DB](regional-failover.md).  

* [Erfahren Sie mehr über die globale Konsistenz bei Azure Cosmos DB](consistency-levels.md).  

* Entwickeln Sie Ihre Umgebung mit mehreren Regionen mit der [SQL-API](tutorial-global-distribution-sql-api.md), [MongoDB-API](tutorial-global-distribution-mongodb.md) oder [Tabellen-API](tutorial-global-distribution-table.md) für Azure Cosmos DB.  