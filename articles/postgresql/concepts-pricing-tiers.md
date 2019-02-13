---
title: Tarife für Azure Database for PostgreSQL
description: In diesem Artikel werden die Tarife für Azure Database for PostgreSQL beschrieben.
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 8cc7d7d93f483289ab7faa3f5a7193fc218b57bc
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55695026"
---
# <a name="azure-database-for-postgresql-pricing-tiers"></a>Azure Database for PostgreSQL – Tarife

Sie können eine Azure Database for PostgreSQL-Serverinstanz basierend auf drei unterschiedlichen Tarifen erstellen: „Basic“, „Allgemein“ und „Arbeitsspeicheroptimiert“. Die Tarife unterscheiden sich anhand der bereitstellbaren Menge an Rechenleistung in V-Kernen, des Arbeitsspeichers pro V-Kern und der zum Speichern der Daten verwendeten Speichertechnologie. Alle Ressourcen werden auf der PostgreSQL-Serverebene bereitgestellt. Ein Server kann über eine oder mehrere Datenbanken verfügen.

|    | **Basic** | **Allgemeiner Zweck** | **Arbeitsspeicheroptimiert** |
|:---|:----------|:--------------------|:---------------------|
| Computegeneration | Gen 4, Gen 5 | Gen 4, Gen 5 | Gen 5 |
| V-Kerne | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Arbeitsspeicher pro V-Kern | 2 GB | 5 GB | 10 GB |
| Speichergröße | 5 GB bis 1 TB | 5 GB bis 4 TB | 5 GB bis 4 TB |
| Speichertyp | Azure-Standardspeicher | Azure Storage Premium | Azure Storage Premium |
| Aufbewahrungszeitraum von Datenbanksicherungen | 7 bis 35 Tage | 7 bis 35 Tage | 7 bis 35 Tage |

Um einen Tarif auszuwählen, verwenden Sie die folgende Tabelle als Ausgangspunkt.

| Tarif | Zielworkloads |
|:-------------|:-----------------|
| Basic | Workloads mit geringen Anforderungen an Rechen- und E/A-Leistung. Beispiele hierfür sind Server, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| Allgemeiner Zweck | Geeignet für die meisten Unternehmensworkloads mit gängigen Compute- und Arbeitsspeicheranforderungen und skalierbarem E/A-Durchsatz. Hierzu zählen beispielsweise zum Hosten von Web- und mobilen Apps verwendete Server und andere Unternehmensanwendungen.|
| Arbeitsspeicheroptimiert | Geeignet für Hochleistungs-Datenbankworkloads, für die In-Memory-Leistung erforderlich ist, um eine schnellere Transaktionsverarbeitung und höhere Parallelität zu erzielen. Hierzu zählen beispielsweise Server für die Verarbeitung von Echtzeitdaten und leistungsstarke Transaktions- oder Analyse-Apps.|

Nach der Servererstellung können Sie die Anzahl von virtuellen Kernen, die Hardwaregeneration und den Tarif (mit Ausnahme eines Wechsels zu oder von Basic) innerhalb weniger Sekunden ändern. Außerdem haben Sie die Möglichkeit, die Speichermenge einzeln zu erhöhen und den Aufbewahrungszeitraum für Sicherungen zu erhöhen oder zu verringern, ohne dass bei der Anwendung Ausfallzeiten auftreten. Der Sicherungsspeichertyp kann nach der Servererstellung nicht mehr geändert werden. Weitere Informationen finden Sie im Abschnitt [Skalieren von Ressourcen](#scale-resources).


## <a name="compute-generations-and-vcores"></a>Computegenerationen und V-Kerne

Computeressourcen werden in Form von virtuellen Kernen bereitgestellt und repräsentieren die logische CPU der zugrunde liegenden Hardware. Derzeit können Sie aus zwei Computegenerationen wählen – Gen 4 und Gen 5. Logische CPUs der Generation 4 basieren auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) 2,4 GHz. Logische CPUs der Generation 5 basieren auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) 2,3 GHz. Gen 4 und 5 Gen stehen in den folgenden Regionen zur Verfügung („X“ steht für verfügbar). 

> [!IMPORTANT]
> Ab dem 12. Dezember 2018 können Neukunden keine Computeserver der vierten Generation mehr in den Regionen Brasilien (Süden), Kanada (Mitte), Kanada (Osten), Asien (Osten), USA (Osten 2), Indien (Mitte), Indien (Westen), Japan (Westen), USA (Norden-Mitte) und USA (Westen) bereitstellen. Zuvor erstellte Computeserver der vierten Generation werden ab dem 1. Februar 2019 in diesen Regionen zu Servern der fünften Generation migriert.

| **Azure-Region** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| USA (Mitte) |  | X |
| USA (Ost) |  | X |
| USA (Ost) 2 | X | X |
| USA Nord Mitte | X | X |
| USA Süd Mitte | X | X |
| USA (Westen) | X | X |
| USA, Westen 2 |  | X |
| Brasilien Süd | X | X |
| Kanada, Mitte | X | X |
| Kanada, Osten | X | X |
| Nordeuropa | X | X |
| Europa, Westen |  | X |
| Frankreich, Mitte |  | X |
| UK, Süden |  | X |
| UK, Westen |  | X |
| Asien, Osten | X | X |
| Asien, Südosten | X | X |
| Australien (Osten) |  | X |
| Australien, Mitte |  | X |
| Australien, Mitte 2 |  | X |
| Australien, Südosten |  | X |
| Indien, Mitte | X | X |
| Indien (Süden) |  | X |
| Indien, Westen | X | X |
| Japan, Osten | X | X |
| Japan, Westen | X | X |
| Korea, Mitte |  | X |
| Korea, Süden |  | X |
| China, Osten 1 | X |  |
| China, Osten 2 |  | X |
| China, Norden 1 | X |  |
| China, Norden 2 |  | X |
| Deutschland, Mitte |  | X |
| US DoD, Mitte  | X |  |
| US DoD, Osten  | X |  |
| US Gov Arizona |  | X |
| US Gov Texas |  | X |
| US Government, Virginia |  | X |

## <a name="storage"></a>Storage

Der von Ihnen bereitgestellte Speicher definiert die Speicherkapazität, die für Ihren Azure Database for PostgreSQL-Server zur Verfügung steht. Der Speicher wird für die Datenbankdateien, temporären Dateien, Transaktionsprotokolle und PostgreSQL-Serverprotokolle verwendet. Außerdem wird durch die Gesamtmenge an bereitgestelltem Speicher die E/A-Kapazität Ihres Servers definiert.

|    | **Basic** | **Allgemeiner Zweck** | **Arbeitsspeicheroptimiert** |
|:---|:----------|:--------------------|:---------------------|
| Speichertyp | Azure-Standardspeicher | Azure Storage Premium | Azure Storage Premium |
| Speichergröße | 5 GB bis 1 TB | 5 GB bis 4 TB | 5 GB bis 4 TB |
| Speicherinkrementgröße | 1 GB | 1 GB | 1 GB |
| IOPS | Variable |3 IOPS/GB<br/>Min. 100 IOPS<br/>Max. 6.000 IOPS | 3 IOPS/GB<br/>Min. 100 IOPS<br/>Max. 6.000 IOPS |

Sie können während und nach der Erstellung des Servers zusätzliche Speicherkapazität hinzufügen. Der Tarif „Basic“ umfasst keine IOPS-Garantie. Für die Tarife „Allgemein“ und „Arbeitsspeicheroptimiert“ wird der IOPS-Wert gegenüber der bereitgestellten Speichergröße in einem Verhältnis von 3:1 skaliert.

Sie können Ihren E/A-Verbrauch im Azure-Portal oder mit Azure CLI-Befehlen überwachen. Die wichtigen zu überwachenden Metriken sind das [Speicherlimit, der Speicherprozentsatz, der genutzte Speicher und der E/A-Prozentsatz](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Erreichen der Speicherbegrenzung

Der Server ist als schreibgeschützt gekennzeichnet, wenn die Menge des freien Speichers kleiner als 5 GB oder 5 % des bereitgestellten Speicher ist, je nachdem, welcher Wert kleiner ist. Wenn Sie beispielsweise 100 GB des Speichers bereitgestellt haben und die tatsächliche Auslastung über 95 GB hinausgeht, wird der Server als schreibgeschützt gekennzeichnet. Wenn Sie 5 GB des Speichers bereitgestellt haben, wird der Server ebenfalls als schreibgeschützt markiert, sofern sich der freie Speicher auf weniger als 250 MB beläuft.  

Wenn der Server als schreibgeschützt festgelegt ist, werden alle bestehenden Sitzungen getrennt, und für Transaktionen ohne Commit wird ein Rollback ausgeführt. Nachfolgende Schreibvorgänge und Transaktionscommits ergeben einen Fehler. Alle nachfolgenden Abfragen werden ununterbrochen fortgesetzt.  

Sie können die Menge des bereitgestellten Speichers für Ihren Server erhöhen oder eine neue Sitzung im Lese-/ Schreibmodus starten und Daten löschen, um Speicherplatz freizugeben. Durch Ausführen von `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` wird die aktuelle Sitzung in den Lese-/Schreibmodus versetzt. Um eine Datenbeschädigung zu verhindern, führen Sie keine Schreibvorgänge durch, solange der Server noch den schreibgeschützten Status aufweist.

Sie sollten eine Benachrichtigung einrichten, damit Sie informiert werden, wenn sich der Serverspeicher dem Schwellenwert nähert. So können Sie vermeiden, in den schreibgeschützten Zustand zu geraten. Weitere Informationen finden Sie in der Dokumentation zum [Einrichten einer Benachrichtigung](howto-alert-on-metric.md).

## <a name="backup"></a>Backup

Der Dienst erstellt automatisch Sicherungen Ihres Servers. Die Mindestaufbewahrungsdauer für Sicherungen beträgt sieben Tage. Sie können eine Aufbewahrungsdauer von bis zu 35 Tagen festlegen. Die Aufbewahrungsdauer kann während der Lebensdauer des Servers jederzeit angepasst werden. Sie können zwischen lokal redundanten und georedundanten Sicherungen wählen. Georedundante Sicherungen werden auch in der [geografisch gepaarten Region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) gespeichert, die zur Erstellungsregion Ihres Servers gehört (Regionspaar). Diese Redundanz sorgt in einem Notfall für Schutz. Sie haben auch die Möglichkeit, Ihren Server in einer beliebigen anderen Azure-Region, in der der Dienst verfügbar ist, mit georedundanten Sicherungen wiederherzustellen. Es ist nicht möglich, zwischen den beiden Sicherungsspeicheroptionen zu wechseln, sobald der Server erstellt ist.

## <a name="scale-resources"></a>Skalieren von Ressourcen

Nachdem Sie Ihren Server erstellt haben, können Sie die virtuellen Kerne, die Hardwaregeneration, den Tarif (mit Ausnahme eines Wechsels zu oder von Basic), die Speichermenge und den Aufbewahrungszeitraum für Sicherungen einzeln ändern. Der Sicherungsspeichertyp kann nach der Servererstellung nicht mehr geändert werden. Die Anzahl virtueller Kerne kann zentral hoch- oder herunterskaliert werden. Die Aufbewahrungsdauer für Sicherungen kann von 7 bis zu 35 Tagen zentral hoch- oder herunterskaliert werden. Die Speichergröße kann nur erhöht werden. Die Skalierung der Ressourcen kann entweder über das Portal oder per Azure CLI durchgeführt werden. Ein Beispiel für die Skalierung über die Azure CLI finden Sie unter [Überwachen und Skalieren eines einzelnen PostgreSQL-Servers mit der Azure CLI](scripts/sample-scale-server-up-or-down.md).

Beim Ändern der Anzahl von virtuellen Kernen, der Hardwaregeneration oder des Tarifs wird eine Kopie des ursprünglichen Servers mit der neuen Computezuteilung erstellt. Sobald der neue Server betriebsbereit ist und ausgeführt wird, werden die Verbindungen auf den neuen Server verschoben. Während des Moments, in dem das System den Wechsel zum neuen Server durchführt, können keine neuen Verbindungen hergestellt werden, und für alle Transaktionen ohne Commit erfolgt ein Rollback. Die Länge dieses Zeitfensters variiert, aber normalerweise dauert der Vorgang nicht länger als eine Minute.

Das Skalieren des Speichers und das Ändern der Aufbewahrungsdauer für Sicherungen sind reine Onlinevorgänge. Es gibt keine Ausfallzeit, und Ihre Anwendung wird nicht beeinträchtigt. Da der IOPS-Wert in Abhängigkeit der Größe des bereitgestellten Speichers skaliert wird, können Sie die IOPS-Menge, die für Ihren Server verfügbar ist, durch das zentrale Hochskalieren des Speichers erhöhen.

## <a name="pricing"></a>Preise

Aktuelle Preisinformationen finden Sie auf der Seite [Azure-Datenbank für MySQL – Preise](https://azure.microsoft.com/pricing/details/PostgreSQL/). Informationen zu den Kosten der gewünschten Konfiguration können Sie im [Azure-Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) anzeigen. Die monatlichen Kosten für die von Ihnen ausgewählten Optionen werden auf der Registerkarte **Tarif** angegeben. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie den Azure-Preisrechner verwenden, um einen geschätzten Preis zu erhalten. Wählen Sie auf der Website des [Azure-Preisrechners](https://azure.microsoft.com/pricing/calculator/) die Option **Elemente hinzufügen** aus, erweitern Sie die Kategorie **Datenbanken**, und wählen Sie **Azure Database for PostgreSQL** aus, um die Optionen anzupassen.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich, wie Sie [im Portal eine PostgreSQL-Serverinstanz erstellen](tutorial-design-database-using-azure-portal.md).
- Weitere Informationen zu [Dienstgrenzwerten](concepts-limits.md). 
- Weitere Informationen zum [horizontalen Hochskalieren mit Lesereplikaten](howto-read-replicas-portal.md).
