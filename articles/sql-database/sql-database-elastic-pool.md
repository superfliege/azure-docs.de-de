---
title: Verwalten mehrerer SQL-Datenbanken mit Pools für elastische Datenbanken – Azure | Microsoft-Dokumentation
description: Verwalten und skalieren Sie mehrere SQL-Datenbanken – Hunderte und Tausende – mit Pools für elastische Datenbanken. Ein Preis für Ressourcen, die Sie nach Bedarf verteilen können.
keywords: mehrere Datenbanken, Datenbankressourcen, Datenbankleistung
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 04/10/2018
ms.author: ninarn
ms.topic: article
ms.openlocfilehash: 33f4430baacbe50f3d4c7da857ee4345d4f74928
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Pools für elastische Datenbanken als Hilfe beim Verwalten und Skalieren mehrerer Azure SQL-Datenbank-Instanzen

Pools für elastische SQL-Datenbank-Instanzen sind eine einfache, kostengünstige Lösung zum Verwalten und Skalieren mehrerer Datenbanken mit variierenden und unvorhersehbaren Anforderungen. Die Datenbanken in einem Pool für elastische Datenbanken befinden sich auf einem einzelnen Azure SQL-Datenbank-Server und nutzen gemeinsam eine festgelegte Anzahl von Ressourcen zu einem festen Preis. Mit Pools für elastische Datenbanken in Azure SQL-Datenbanken sind SaaS-Entwickler in der Lage, das Preis-Leistungs-Verhältnis einer Gruppe von Datenbanken im Rahmen eines vorgegebenen Budgets zu optimieren und gleichzeitig eine flexible Leistung für jede Datenbank sicherzustellen.

## <a name="what-are-sql-elastic-pools"></a>Was sind Pools für elastische SQL-Datenbanken?

SaaS-Entwickler erstellen Anwendungen, die auf der obersten von umfangreichen Datenebenen aufsetzen, die wiederum aus zahlreichen Datenbanken bestehen können. In einem üblichen Anwendungsmuster wird jedem Kunden eine eigene Datenbank zur Verfügung gestellt. Verschiedene Kunden weisen jedoch oft unterschiedliche und unvorhersehbare Verwendungsmuster auf, und es ist schwierig, den Ressourcenbedarf jedes einzelnen Datenbankbenutzers vorherzusagen. Bisher hatten Sie zwei Optionen:

- Überdimensionierte Bereitstellung der Ressourcen, die sich an der Nutzung zu Spitzenzeiten orientiert (samt der damit verbundenen Kosten) oder
- Unterdimensionierte Bereitstellung, um Kosten zu Lasten von Leistung und Kundenzufriedenheit während Spitzenzeiten einzusparen.

Pools für elastische Datenbanken lösen dieses Problem, indem sie sicherstellen, dass die Datenbanken die Leistungsressourcen erhalten, die sie benötigen, und zwar genau zu dem Zeitpunkt, zu dem sie sie benötigen. Sie stellen einen einfachen Ressourcenzuordnungsmechanismus innerhalb eines vorhersagbaren Budgets bereit. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die Pools für elastische Datenbanken verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Mit Pools für elastische Datenbanken können Entwickler Ressourcen für einen Pool erwerben, der von mehreren Datenbanken genutzt wird, um unvorhersehbare Auslastungszeiten von einzelnen Datenbanken abzufedern. Sie können Ressourcen für den Pool entweder basierend auf dem [DTU-basierten Kaufmodell (Vorschauversion)](sql-database-service-tiers.md#dtu-based-purchasing-model) oder dem [V-Kern-basierten Kaufmodell (Vorschauversion)](sql-database-service-tiers.md#vcore-based-purchasing-model-preview) konfigurieren. Die Ressourcenanforderungen eines Pools werden anhand der zusammengefassten Auslastung der hierin befindlichen Datenbanken ermittelt. Die Anzahl der für den Pool verfügbaren Ressourcen wird vom Budget des Entwicklers gesteuert. Der Entwickler fügt dem Pool einfach Datenbanken hinzu, legt die minimalen und maximalen Ressourcen für die Datenbanken fest (je nach ausgewähltem Ressourcenmodell die minimale und maximale Anzahl von DTUs oder virtuellen Kernen), und legt dann basierend auf dem Budget die Ressourcen des Pools fest. Mithilfe von Pools können Entwickler ihre Dienste problemlos und kontinuierlich ausbauen, von schlanken Startups bis hin zu etablierten Unternehmen.

Im Pool können einzelne Datenbanken die automatische Skalierung innerhalb der angegebenen Parameter flexibel automatisch skalieren. Bei hoher Auslastung kann eine Datenbank mehr Ressourcen nutzen, um die Anforderungen zu erfüllen. Datenbanken verbrauchen bei geringerer Auslastung weniger Ressourcen und ohne Auslastung gar keine Ressourcen. Durch die Bereitstellung von Ressourcen für den gesamten Pool und nicht nur für einzelne Datenbanken vereinfachen Sie Ihre Verwaltungsaufgaben. Außerdem verfügen Sie über ein vorhersagbares Budget für den Pool. Einem vorhandenen Pool können zusätzliche Ressourcen hinzugefügt werden, ohne dass es zu Datenbankausfällen kommt. Es gilt jedoch die Ausnahme, dass die Datenbanken unter Umständen verschoben werden müssen, um die zusätzlichen Computeressourcen für die neue eDTU-Reservierung bereitzustellen. Ebenso können zusätzliche Ressourcen, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Und Sie können dem Pool Datenbanken hinzufügen oder Datenbanken aus dem Pool entfernen. Wenn die Ressourcen für eine Datenbank voraussichtlich nicht ausgeschöpft werden, sollten Sie sie entfernen.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Wann sollten Sie einen Pool für elastische SQL-Datenbank-Instanzen erwägen?

Pools eignen sich hervorragend für eine große Anzahl an Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine einzelne Datenbank wird dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet.

Je mehr Datenbanken Sie einem Pool hinzufügen können, desto größer werden Ihre Ersparnisse. Abhängig vom Auslastungsmuster Ihrer Anwendung können Sie bereits mit nur zwei S3-Datenbanken Ersparnisse erzielen.

Die folgenden Abschnitte helfen Ihnen einzuschätzen, ob Ihre Datenbanksammlung von einem Pool profitieren kann. In den Beispielen wird von Standardpools ausgegangen. Die Grundlagen gelten jedoch auch für Basic- und Premiumpools.

### <a name="assessing-database-utilization-patterns"></a>Bewerten der Datenbankauslastungsmuster

In der folgenden Abbildung finden Sie ein Beispiel für eine Datenbank, die sich in der Regel im Leerlauf befindet, bei der es allerdings auch regelmäßig zu Auslastungsspitzen kommt. Dieses Auslastungsmuster eignet sich für einen Pool:

   ![eine einzelne, für einen Pool geeignete Datenbank](./media/sql-database-elastic-pool/one-database.png)

Im dargestellten Zeitraum von fünf Minuten schnellt DB1 auf bis zu 90 DTUs. Die mittlere Auslastung liegt jedoch bei unter 5 DTUs. Zum Ausführen dieser Workload in einer einzelnen Datenbank ist eine S3-Leistungsstufe erforderlich. Allerdings bewirkt dies, dass die meisten Ressourcen in Zeiten geringer Aktivität nicht verwendet werden.

Mit einem Pool können diese ungenutzten DTUs auf mehrere Datenbanken verteilt werden, wodurch sich die erforderlichen DTUs und damit auch die Gesamtkosten verringern.

Ausgehend vom vorherigen Beispiel nehmen wir einmal an, dass es weitere Datenbanken mit ähnlichen Auslastungsmustern wie DB1 gibt. In den beiden folgenden Abbildungen wird die Auslastung von einmal vier, und von weiteren 20 Datenbanken in jeweils einem Graphen dargestellt. Dies soll anhand des DTU-basierten Kaufmodells veranschaulichen, dass sich die Auslastung der Datenbanken zeitlich nicht überlappt:

   ![vier Datenbanken mit einem für einen Pool geeigneten Auslastungsmuster](./media/sql-database-elastic-pool/four-databases.png)

   ![20 Datenbanken mit einem für einen Pool geeigneten Auslastungsmuster](./media/sql-database-elastic-pool/twenty-databases.png)

Die zusammengefasste DTU-Auslastung aller 20 Datenbanken wird in der vorstehenden Abbildung durch die schwarze Linie dargestellt. Diese zeigt, dass die zusammengefasste DTU-Auslastung niemals 100 DTUs übersteigt, und gibt an, dass die 20 Datenbanken in diesem Zeitraum 100 eDTUs gemeinsam nutzen können. Dies führt zu einer 20-fachen Verringerung im Hinblick auf die DTUs und einer 13-fachen Preisreduzierung im Vergleich zur Positionierung jeder Datenbank in S3-Leistungsstufen für Einzeldatenbanken.

Dieses Beispiel ist aus den folgenden Gründen ideal:

* Es gibt große Unterschiede zwischen der Spitzenauslastung und der mittleren Auslastung pro Datenbank.
* Die Spitzenauslastung jeder Datenbank ergibt sich zu jeweils unterschiedlichen Zeitpunkten.
* eDTUs werden von mehreren Datenbanken gemeinsam genutzt.

Der Preis eines Pools ergibt sich aus den eDTUs des Pools. Während der Preis pro eDTU für einen Pool 1,5 Mal höher als der Preis pro DTU für eine Einzeldatenbank ist, können hierbei **Pool-eDTUs von vielen Datenbanken gemeinsam genutzt werden, daher liegt der Preis unter der Gesamtanzahl der benötigten eDTUs**. Die Unterschiede bei der Preisgestaltung und die gemeinsame Nutzung von eDTUs sind die Basis des Einsparungspotenzials, das von Pools geboten wird.

Die folgenden Faustregeln hinsichtlich der Anzahl der Datenbanken und ihrer Auslastung sollen Ihnen helfen, sicherzustellen, dass Pools kosteneffizienter als Leistungsebenen für einzelne Datenbanken sind.

### <a name="minimum-number-of-databases"></a>Minimale Anzahl Datenbanken

Wenn die aggregierte Menge der Ressourcen für einzelne Datenbanken mehr als dem 1,5-fachen der für den Pool benötigten Ressourcen entspricht, ist ein Pool für elastische Datenbanken kostengünstiger.

***Beispiel für das DTU-basierte Kaufmodell***<br>
Es werden mindestens zwei S3-Datenbanken oder mindestens 15 S0-Datenbanken benötigt, damit ein Pool mit 100 eDTUs kosteneffizienter als die Verwendung von Leistungsebenen für einzelne Datenbanken ist.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximale Anzahl von gleichzeitig unter Spitzenlast laufenden Datenbanken

Bei der gemeinsamen Nutzung von Ressourcen können nicht alle Datenbanken in einem Pool gleichzeitig Ressourcen bis zum verfügbaren Grenzwert für einzelne Datenbanken verwenden. Je weniger Datenbanken gleichzeitig auf Spitzenlast genutzt werden, desto niedriger können die Poolressourcen festgelegt werden und desto kosteneffizienter wird der Pool. Im Allgemeinen sollten nicht mehr als zwei Drittel der Datenbanken (67 %) im Pool gleichzeitig den Ressourcengrenzwert erreichen.

***Beispiel für das DTU-basierte Kaufmodell***<br>
Um die Kosten für drei S3-Datenbanken in einem Pool mit 200 eDTUs zu senken, können höchstens zwei dieser Datenbanken gleichzeitig mit Spitzenauslastung ausgeführt werden. Andernfalls müsste der Pool auf mehr als 200 eDTUs ausgelegt werden, wenn mehr als zwei dieser vier S3-Datenbanken gleichzeitig mit Spitzenauslastung ausgeführt werden. Wenn die Größe des Pools auf mehr als 200 eDTUs geändert wird, müssten weitere S3-Datenbanken zum Pool hinzugefügt werden, damit die Kosten unterhalb der Leistungsstufe für einzelne Datenbanken bleiben.

Beachten Sie, dass in diesem Beispiel die Auslastung anderer Datenbanken im Pool nicht berücksichtigt wird. Wenn alle Datenbanken zu einem gegebenen Zeitpunkt eine Spitzenauslastung aufweisen, können weniger als zwei Drittel (67 %) der Datenbanken gleichzeitig eine Spitzenauslastung aufweisen.

### <a name="resource-utilization-per-database"></a>Ressourcenauslastung pro Datenbank
Ein großer Unterschied zwischen der mittleren und der Spitzenauslastung einer Datenbank weist darauf hin, dass es längere Zeiträume mit geringer Auslastung und kurze Zeiträume mit hoher Auslastung gibt. Dieses Auslastungsmuster eignet sich ideal für die gemeinsame, datenbankübergreifende Nutzung von Ressourcen. Eine Datenbank eignet sich für die Aufnahme in einen Pool, wenn die Spitzenauslastung etwa um das 1,5-fache höher als die mittlere Auslastung ist.

***Beispiel für das DTU-basierte Kaufmodell***<br>
Eine S3-Datenbank, die zu Spitzenzeiten 100 DTUs und im Durchschnitt 67 DTUs oder weniger benötigt, ist ein guter Kandidat für die gemeinsame Nutzung von eDTUs in einem Pool. Alternativ ist auch eine S1-Datenbank, die zu Spitzenzeiten 20 DTUs und im Durchschnitt 13 DTUs oder weniger benötigt, ein guter Kandidat für einen Pool.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Wie wähle ich die richtige Poolgröße?

Die optimale Größe eines Pools hängt von den zusammengefassten Ressourcen ab, die für alle Datenbanken im Pool benötigt werden. Dies setzt voraus, dass Sie die folgenden Mengen ermitteln:

* Maximale Ressourcen, die von allen Datenbanken im Pool (je nach ausgewähltem Ressourcenmodell die maximale Anzahl von DTUs oder virtuellen Kernen) verwendet werden.
* Maximale Speicherbytes, die von allen Datenbanken im Pool verwendet werden.

Verfügbare Dienstebenen für jedes Ressourcenmodell finden Sie im [DTU-basierten Kaufmodell](sql-database-service-tiers.md#dtu-based-purchasing-model) oder im [V-Kern-basierten Kaufmodell (Vorschauversion)](sql-database-service-tiers.md#vcore-based-purchasing-model-preview).

SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbank-Server und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Zusätzlich zu den Empfehlungen enthält die Datenbank eine integrierte Funktion, mit der die eDTU-Nutzung einer benutzerdefinierten Gruppe von Datenbanken auf dem Server geschätzt wird. Dadurch können Sie eine Was-wäre-wenn-Analyse durchführen, indem Sie interaktiv Datenbanken zum Pool hinzufügen und entfernen. So können Sie die Ressourcennutzung ermitteln und eine Orientierungshilfe für die Größenanpassung erhalten, bevor Sie Ihre Veränderungen committen. Eine Anleitung finden Sie unter [Überwachen, Verwalten und Skalieren eines Pools für elastische Datenbanken](sql-database-elastic-pool-manage-portal.md).

Wenn Sie keine Tools verwenden können, kann Ihnen die folgende Anleitung dabei helfen, einzuschätzen, ob ein Pool kostengünstiger als eine einzelne Datenbank ist:

1. Schätzen Sie die für den Pool benötigten eDTUs oder V-Kerne nach der folgenden Methode:

   Für DTU-basiertes Kaufmodell: MAX(<*Gesamtanzahl Datenbanken* x *durchschnittliche DTU-Nutzung pro Datenbank*>,<br>
   <*Anzahl gleichzeitiger Datenbanken mit Spitzenauslastung* x *DTU-Spitzenauslastung pro Datenbank*)

   Für V-Kern-basiertes Kaufmodell: MAX(<*Gesamtanzahl Datenbanken* x *durchschnittliche V-Kern-Nutzung pro Datenbank*>,<br>
   <*Anzahl gleichzeitiger Datenbanken mit Spitzenauslastung* x *V-Kern-Spitzenauslastung pro Datenbank*)

2. Schätzen Sie den für den Pool benötigten Speicherplatz, indem Sie die Menge der für alle Datenbanken im Pool benötigten Bytes addieren. Ermitteln Sie dann die eDTU-Poolgröße, die diese Menge an Speicher bietet.
3. Verwenden Sie für das DTU-basierte Kaufmodell die größere der eDTU-Schätzungen aus Schritt 1 und Schritt 2. Verwenden Sie für das V-Kern-basierte Kaufmodell die V-Kern-Schätzung aus Schritt 1.
4. Auf der [SQL-Datenbank Preisseite](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie die kleinste Poolgröße, die größer ist als die Schätzung aus Schritt 3.
5. Vergleichen Sie den Poolpreis aus Schritt 5 mit dem Preis der geeigneten Leistungsstufen für einzelne Datenbanken.

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Verwenden anderer SQL Datenbank-Funktionen mit Pools für elastische Datenbanken

### <a name="elastic-jobs-and-elastic-pools"></a>Elastische Aufträge und Pools für elastische Datenbanken

Mit einem Pool werden die Verwaltungsaufgaben vereinfacht, indem Skripts in **[elastischen Aufträgen](sql-database-elastic-jobs-overview.md)** ausgeführt werden. Durch einen elastischen Auftrag werden die meisten aufwändigen Schritte beseitigt, die bei der Verwendung einer großen Anzahl von Datenbanken anfallen. Lesen Sie sich [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) durch, bevor Sie beginnen.

Weitere Informationen zu anderen Datenbanktools für die Verwendung mit mehreren Datenbanken finden Sie unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Optionen für Geschäftskontinuität für Datenbanken in einem Pool für elastische Datenbanken
In einem Pool zusammengefasste Datenbanken unterstützen in der Regel die gleichen [Geschäftskontinuitätsfeatures](sql-database-business-continuity.md), die auch bei Einzeldatenbanken zur Verfügung stehen.

- **Point-in-Time-Wiederherstellung:** Bei der Point-in-Time-Wiederherstellung werden automatische Datenbanksicherungen verwendet, um den Status einer Datenbank in einem Pool zu einem bestimmten Zeitpunkt wiederherzustellen. Siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Geografische Wiederherstellung**: Die geografische Wiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Vorfalls in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Siehe [Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md)

- **Aktive Georeplikation**: Für Anwendungen, für die umfangreichere Wiederherstellungsanforderungen erforderlich sind, als die Geowiederherstellung bieten kann, konfigurieren Sie [aktive Georeplikation](sql-database-geo-replication-overview.md).

## <a name="manage-elastic-pools-and-databases-using-the-azure-portal"></a>Verwalten von Pools für elastische Datenbanken und von Datenbanken im Azure-Portal

### <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Erstellen eines neuen Pools für elastische SQL-Datenbank-Instanzen im Azure-Portal

Zur Erstellung eines Pools für elastische Datenbanken im Azure-Portal gibt es zwei Möglichkeiten.
1. Sie können einen Pool für elastische Datenbanken erstellen, indem Sie im **Marketplace** nach **Elastischer SQL-Pool** suchen oder auf dem Navigationsblatt „Elastische SQL-Pools“ auf **+Hinzufügen** klicken. Im Rahmen dieses Poolbereitstellungsworkflows können Sie einen neuen oder einen bereits vorhandenen Server angeben.
2. Sie können auch einen Pool für elastische Datenbanken erstellen, indem Sie zu einem vorhandenen SQL-Server navigieren und auf **Pool erstellen**, um direkt auf diesem Server einen Pool zu erstellen. Hier besteht einzige Unterschied darin, dass Sie während des Pool-Bereitstellungsworkflows den Schritt überspringen, in dem der Server angegeben wird.

> [!NOTE]
> Auf einem Server können mehrere Pools erstellt werden, es ist jedoch nicht möglich, Datenbanken von verschiedenen Servern im gleichen Pool zusammenzufassen.

Die Dienstebene des Pools bestimmt die Features für die elastischen Datenbanken im Pool sowie die für jede Datenbank verfügbare maximale Menge der Ressourcen. Ausführliche Informationen finden Sie in den Ressourceneinschränkungen für Pools für elastische Datenbanken im [DTU-Modell](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) und im [V-Kern-Modell](sql-database-vcore-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Zum Konfigurieren der Ressourcen und Preise des Pools klicken Sie auf **Pool konfigurieren**. Wählen Sie dann eine Dienstebene aus, fügen Sie den Pool Datenbanken hinzu, und konfigurieren Sie die Ressourceneinschränkungen für den Pool und seine Datenbanken.

Wenn Sie die Konfiguration des Pools abgeschlossen haben, können Sie auf „Anwenden“ klicken, dem Pool einen Namen geben und auf „OK“ klicken, um den Pool zu erstellen.

### <a name="monitor-an-elastic-pool-and-its-databases"></a>Überwachen eines Pools für elastische Datenbanken und der zugehörigen Datenbanken

Im Azure-Portal können Sie die Verwendung eines Pools für elastische Datenbanken sowie der darin enthaltenen Datenbanken überwachen. Außerdem können Sie einen Änderungssatz für Ihren Pool für elastische Datenbanken erstellen und alle Änderungen gleichzeitig übermitteln. So können Sie etwa Datenbanken hinzufügen oder entfernen, die Einstellungen des Pools für elastische Datenbanken ändern oder Ihre Datenbankeinstellungen anpassen.

Um mit der Überwachung Ihres Pools für elastische Datenbanken zu beginnen, suchen und öffnen Sie im Portal einen Pool für elastische Datenbanken. Zuerst sehen Sie einen Bildschirm mit einer Übersicht über den Status Ihres Pools für elastische Datenbanken. Dies umfasst:

* Überwachungsdiagramme, welche die Ressourcenverwendung des Pools für elastische Datenbanken zeigen
* Aktuelle Warnungen und Empfehlungen (falls vorhanden) für den Pool für elastische Datenbanken

Die folgende Abbildung zeigt ein Beispiel für einen Pool für elastische Datenbanken:

![Poolansicht](./media/sql-database-elastic-pool-manage-portal/basic.png)

Wenn Sie weitere Informationen zum Pool anzeigen möchten, klicken Sie auf eine der in dieser Übersicht verfügbaren Informationen. Wenn Sie auf das Diagramm **Ressourcenverwendung** klicken, werden Sie zur Azure-Überwachungsansicht weitergeleitet, in der Sie die im Diagramm angezeigten Metriken und das Zeitfenster anpassen können. Wenn Sie auf einer der verfügbaren Benachrichtigungen klicken, gelangen Sie auf ein Blatt, das die vollständigen Details der Warnung oder Empfehlung anzeigt.

Wenn Sie die Datenbanken in Ihrem Pool überwachen möchten, klicken Sie links im Ressourcenmenü im Abschnitt **Überwachung** auf **Datenbank-Ressourcennutzung**.

![Seite „Datenbank-Ressourcennutzung“](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

#### <a name="to-customize-the-chart-display"></a>So passen Sie die Darstellung des Diagramms an

Sie können das Diagramm und die Metrikseite bearbeiten, um andere Metriken wie etwa die prozentuale Verwendung von CPU, Daten-E/A und Protokoll-E/A anzuzeigen.

Auf dem Formular **Diagramm bearbeiten** können Sie einen festen Zeitbereich auswählen oder auf **benutzerdefiniert** klicken, um ein beliebiges 24-Stunden-Fenster in den vergangenen zwei Wochen auszuwählen, und dann die zu überwachenden Ressourcen auswählen.

#### <a name="to-select-databases-to-monitor"></a>So wählen Sie die zu überwachenden Datenbanken aus

Standardmäßig zeigt das Diagramm auf dem Blatt **Datenbank-Ressourcennutzung** die fünf wichtigsten Datenbanken nach DTU oder CPU (je nach Ihrer Dienstebene) an. Sie können die Anzeige der Datenbanken in diesem Diagramm verändern, indem Sie in der Liste unter dem Diagramm mithilfe der Kontrollkästchen auf der linken Seite Datenbanken auswählen oder deren Auswahl aufheben.

Sie können auch weitere Metriken zur parallelen Anzeige in dieser Datenbanktabelle auswählen, um einen umfassenderen Überblick über die Leistung Ihrer Datenbanken zu erhalten.

Weitere Informationen finden Sie unter [Erstellen von SQL-Datenbankwarnungen im Azure-Portal](sql-database-insights-alerts-portal.md).

### <a name="manage-an-elastic-pool-and-its-databases"></a>Verwalten eines Pools für elastische Datenbanken und der zugehörigen Datenbanken

Alle Pooleinstellungen befinden sich an einem Ort: dem Blatt **Pool konfigurieren**. Zum Aufrufen dieses Blatts suchen Sie im Portal einen Pool für elastische Datenbanken und klicken entweder oben auf dem Blatt oder im Ressourcenmenü auf der linken Seite auf **Pool konfigurieren**.

Hier können Sie eine beliebige Kombination aus den folgenden Änderungen vornehmen und alle in einem Batch speichern:
1. Ändern der Dienstebene des Pools
2. Herauf- oder Herunterskalieren der Leistung (DTU oder V-Kerne)
3. Hinzufügen oder Entfernen von Datenbanken im Pool
4. Festlegen einer minimalen (garantierten) und maximalen Leistungsgrenze für die Datenbanken in den Pools
5. Überprüfen der Kostenzusammenfassung, um alle Änderungen an Ihrer Rechnung anzuzeigen, die sich durch die neue Auswahl ergeben

![Konfigurationsblatt eines Pools für elastische Datenbanken](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="manage-elastic-pools-and-databases-using-powershell"></a>Verwalten von Pools für elastische Datenbanken und von Datenbanken mit PowerShell

Verwenden Sie zum Erstellen und Verwalten von Pools für elastische SQL-Datenbank-Instanzen mithilfe von Azure PowerShell die folgenden PowerShell-Cmdlets. Wenn Sie PowerShell installieren oder upgraden müssen, helfen Ihnen die Informationen unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) weiter. Informationen zum Erstellen und Verwalten von Datenbanken, Servern und Firewallregeln finden Sie unter [Erstellen und Verwalten von Azure SQL-Datenbankservern und -datenbanken mit PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell).

> [!TIP]
> PowerShell-Beispielskripts finden Sie unter [Erstellen von Pools für elastische Datenbanken und Verschieben von Datenbanken zwischen Pools und aus einem Pool heraus mit PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) und [Verwenden von PowerShell zum Überwachen und Skalieren eines Pools für elastische SQL-Datenbanken in der Azure SQL-Datenbank](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | BESCHREIBUNG |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Erstellt einen Pool für elastische Datenbanken für eine logische SQL Server-Instanz.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Ruft Pools für elastische Datenbanken und ihre Eigenschaftswerte für eine logische SQL Server-Instanz ab.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Ändert Eigenschaften eines Pools für elastische Datenbanken für eine logische SQL Server-Instanz. Verwenden Sie z.B. die **StorageMB**-Eigenschaft zum Ändern der maximalen Speicherkapazität eines Pools für elastische Datenbanken.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Löscht einen Pool für elastische Datenbanken aus einer logische SQL Server-Instanz.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Ruft den Status von Vorgängen für einen Pool für elastische Datenbanken für eine logische SQL Server-Instanz ab.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Erstellt eine neue Datenbank in einem vorhandenen Pool oder als einzelne Datenbank. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Ruft mindestens eine Datenbank ab.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Legt Eigenschaften für eine Datenbank fest oder verschiebt eine vorhandene Datenbank in, aus oder zwischen Pools für elastische Datenbanken.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Entfernt eine Datenbank.|


> [!TIP]
> Das Erstellen zahlreicher Datenbanken in einem Pool für elastische Datenbanken kann eine Weile dauern, wenn dies über das Portal oder über PowerShell-Cmdlets erfolgt, die jeweils nur eine Einzeldatenbank erstellen. Informationen zum Automatisieren der Erstellung in einem Pool für elastische Datenbanken finden Sie unter [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-elastic-pools-and-databases-using-the-azure-cli"></a>Verwalten von Pools für elastische Datenbanken und von Datenbanken mit der Azure-Befehlszeilenschnittstelle

Verwenden Sie zum Erstellen und Verwalten von Pools für elastische SQL-Datenbank-Instanzen mithilfe der [Azure CLI](/cli/azure) die folgenden [Azure CLI-SQL-Datenbank](/cli/azure/sql/db)-Befehle. Führen Sie die CLI mithilfe von [Cloud Shell](/azure/cloud-shell/overview) in Ihrem Browser aus, oder [installieren](/cli/azure/install-azure-cli) Sie sie unter macOS, Linux oder Windows.

> [!TIP]
> Azure CLI-Beispielskripts finden Sie unter [Verwenden der Azure CLI zum Verschieben einer Azure SQL-Datenbank in einen Pool für elastische SQL-Datenbanken ](scripts/sql-database-move-database-between-pools-cli.md) und [Verwenden der Azure CLI zum Skalieren eines Pools für elastische SQL-Datenbanken in Azure SQL-Datenbank](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | BESCHREIBUNG |
| --- | --- |
|[az sql elastic-pool create](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Erstellt einen Pool für elastische Datenbanken.|
|[az sql elastic-pool list](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Gibt eine Liste der Pools für elastische Datenbanken auf einem Server zurück.|
|[az sql elastic-pool list-dbs](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Gibt eine Liste der Datenbanken in einem Pool für elastische Datenbanken zurück.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Umfasst auch verfügbare DTU-Einstellungen, Speicherlimits und datenbankbezogene Einstellungen für den Pool. Zum Verringern der Ausführlichkeit sind zusätzliche Speicherlimits und datenbankbezogene Einstellungen standardmäßig ausgeblendet.|
|[az sql elastic-pool update](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|Aktualisiert einen Pool für elastische Datenbanken.|
|[az sql elastic-pool delete](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Löscht den Pool für elastische Datenbanken.|

## <a name="manage-databases-within-elastic-pools-using-transact-sql"></a>Verwalten von Datenbanken in Pools für elastische Datenbanken mit Transact-SQL

Verwenden Sie die folgenden T-SQL-Befehle zum Erstellen und Verschieben von Datenbanken in vorhandenen Pools für elastische Datenbanken oder zum Zurückgeben von Informationen zu einem Pool für elastische SQL-Datenbank-Instanzen mit Transact-SQL. Sie können diese Befehle mit dem Azure-Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs) oder einem beliebigen anderen Programm ausführen, mit dem eine Verbindung mit einem Azure SQL-Datenbankserver hergestellt und Transact-SQL-Befehle übergeben werden können. Informationen zum Erstellen und Verwalten von Datenbanken, Servern und Firewallregeln finden Sie unter [Erstellen und Verwalten von Azure SQL-Datenbankservern und -datenbanken mit Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Mit Transact-SQL können Sie einen Pool für elastische Azure SQL-Datenbank-Instanzen nicht erstellen, aktualisieren oder löschen. Sie können Datenbanken einem Pool für elastische Datenbanken hinzufügen oder daraus entfernen und mithilfe von DMVs Informationen zu vorhandenen Pools für elastische Datenbanken zurückgeben.
>

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[CREATE DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/create-database-azure-sql-database)|Erstellt eine neue Datenbank in einem vorhandenen Pool oder als einzelne Datenbank. Sie müssen über eine Verbindung mit der Masterdatenbank verfügen, um eine neue Datenbank erstellen zu können.|
| [ALTER DATABASE (Azure SQL-Datenbank)](/sql/t-sql/statements/alter-database-azure-sql-database) |Dient zum Verschieben einer Datenbank in, aus oder zwischen Pools für elastische Datenbanken.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Löscht eine Datenbank.|
|[sys.elastic_pool_resource_stats (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Gibt Ressourcenverwendungsstatistiken für alle Pools für elastische Datenbanken in einem logischen Server zurück. Für jeden Pool für elastische Datenbanken gibt es für jedes 15-sekündige Berichtszeitfenster eine Zeile (vier Zeilen pro Minute). Dies umfasst CPU-, E/A-, Protokoll-, Speicher- und gleichzeitige Anforderungs-/Sitzungsauslastung durch alle Datenbanken im Pool.|
|[sys.database_service_objectives (Azure SQL-Datenbank)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Gibt die Edition (Dienstebene), das Dienstziel (Tarif) und den Namen des Pools für elastische Datenbanken, falls vorhanden, für eine Azure SQL-Datenbank oder ein Azure SQL Data Warehouse zurück. Wenn eine Anmeldung an der Masterdatenbank in einem Azure SQL-Datenbankserver besteht, werden Informationen zu allen Datenbanken zurückgegeben. Für Azure SQL Data Warehouse müssen Sie über eine Verbindung mit der Masterdatenbank verfügen.|

## <a name="manage-elastic-pools-and-databases-using-the-rest-api"></a>Verwalten von Pools für elastische Datenbanken und von Datenbanken mit der REST-API

Verwenden Sie zum Erstellen und Verwalten von Pools für elastische SQL-Datenbank-Instanzen diese REST-API-Anforderungen.

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[Pools für elastische Datenbanken – Erstellen oder Aktualisieren](/rest/api/sql/elasticpools/createorupdate)|Erstellt einen neuen Pool für elastische Datenbanken oder aktualisiert einen vorhandenen elastischen Pool für elastische Datenbanken|
|[Pools für elastische Datenbanken – Löschen](/rest/api/sql/elasticpools/delete)|Löscht den Pool für elastische Datenbanken.|
|[Pools für elastische Datenbanken – Abrufen](/rest/api/sql/elasticpools/get)|Ruft einen Pool für elastische Datenbanken ab|
|[Pools für elastische Datenbanken – Auflisten nach Server](/rest/api/sql/elasticpools/listbyserver)|Gibt eine Liste der Pools für elastische Datenbanken auf einem Server zurück.|
|[Pools für elastische Datenbanken – Aktualisieren](/rest/api/sql/elasticpools/update)|Aktualisiert einen vorhandenen Pool für elastische Datenbanken|
|[Empfohlene Pools für elastische Datenbanken – Abrufen](/rest/api/sql/recommendedelasticpools/get)|Ruft einen empfohlenen Pool für elastische Datenbanken ab.|
|[Empfohlene Pools für elastische Datenbanken – Auflisten nach Server](/rest/api/sql/recommendedelasticpools/listbyserver)|Gibt empfohlene Pools für elastische Datenbanken zurück|
|[Empfohlene Pools für elastische Datenbanken – Auflisten von Metriken](/rest/api/sql/recommendedelasticpools/listmetrics)|Gibt Metriken zu empfohlenen Pools für elastische Datenbanken zurück.|
|[Pool für elastische Datenbanken – Aktivitäten](/rest/api/sql/elasticpoolactivities)|Gibt Aktivitäten des Pools für elastische Datenbanken zurück|
|[Pool für elastische Datenbanken – Datenbankaktivitäten](/rest/api/sql/elasticpooldatabaseactivities)|Gibt Aktivitäten in Datenbanken in einem Pool für elastische Datenbanken zurück|
|[Datenbanken – Erstellen oder Aktualisieren](/rest/api/sql/databases/createorupdate)|Erstellt eine neue Datenbank oder aktualisiert eine bereits vorhandene Datenbank|
|[Datenbanken – Abrufen](/rest/api/sql/databases/get)|Ruft eine Datenbank ab|
|[Datenbanken – Abrufen nach Pool für elastische Datenbanken](/rest/api/sql/databases/getbyelasticpool)|Ruft eine Datenbank in einem Pool für elastische Datenbanken ab|
|[Datenbanken – Abrufen nach empfohlenem Pool für elastische Datenbanken](/rest/api/sql/databases/getbyrecommendedelasticpool)|Ruft eine Datenbank in einem empfohlenen Pool für elastische Datenbanken ab.|
|[Datenbanken – Auflisten nach Pool für elastische Datenbanken](/rest/api/sql/databases/listbyelasticpool)|Gibt eine Liste der Datenbanken in einem Pool für elastische Datenbanken zurück.|
|[Datenbanken – Auflisten nach empfohlenem Pool für elastische Datenbanken](/rest/api/sql/databases/listbyrecommendedelasticpool)|Gibt eine Liste von Datenbanken in einem empfohlenen Pool für elastische Datenbanken zurück.|
|[Datenbanken – Auflisten nach Server](/rest/api/sql/databases/listbyserver)|Gibt eine Liste der Datenbanken auf einem Server zurück|
|[Datenbanken – Aktualisieren](/rest/api/sql/databases/update)|Aktualisiert eine vorhandene Datenbank|

## <a name="next-steps"></a>Nächste Schritte

* Ein Video finden Sie unter [Microsoft Virtual Academy video course on Azure SQL Database elastic capabilities](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554) (Microsoft Virtual Academy-Videokurs zu Funktionen elastischer Datenbanken in Azure SQL-Datenbank).
* Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die Pools für elastische Datenbanken verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Ein SaaS-Tutorial, in dem Pools für elastische Datenbanken verwendet werden, finden Sie in der [Einführung in die SaaS-Anwendung Wingtip](sql-database-wtp-overview.md).
