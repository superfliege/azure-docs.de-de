---
title: Tarife in Azure Database for MySQL
description: In diesem Artikel werden die Tarife in Azure Database for MySQL beschrieben.
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: 9d4e42df3137108248a043bb0d9def181d766c7a
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Azure Database for MySQL – Tarife

Sie können eine Azure Database for MySQL-Serverinstanz basierend auf drei unterschiedlichen Tarifen erstellen: „Basic“, „Allgemein“ und „Arbeitsspeicheroptimiert“. Die Tarife unterscheiden sich anhand der bereitstellbaren Menge an Rechenleistung in V-Kernen, des Arbeitsspeichers pro V-Kern und der zum Speichern der Daten verwendeten Speichertechnologie. Alle Ressourcen werden auf der MySQL-Serverebene bereitgestellt. Ein Server kann über eine oder mehrere Datenbanken verfügen.

|    | **Basic** | **Allgemeiner Zweck** | **Arbeitsspeicheroptimiert** |
|:---|:----------|:--------------------|:---------------------|
| Computegeneration | Gen 4, Gen 5 | Gen 4, Gen 5 | Gen 5 |
| V-Kerne | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| Arbeitsspeicher pro V-Kern | Grundwert | 2x Basic | 2x Allgemein |
| Speichergröße | 5 GB bis 1 TB | 5 GB bis 1 TB | 5 GB bis 1 TB |
| Speichertyp | Azure-Standardspeicher | Azure Storage Premium | Azure Storage Premium |
| Aufbewahrungszeitraum von Datenbanksicherungen | 7 bis 35 Tage | 7 bis 35 Tage | 7 bis 35 Tage |

Um einen Tarif auszuwählen, verwenden Sie die folgende Tabelle als Ausgangspunkt.

| Tarif | Zielworkloads |
|:-------------|:-----------------|
| Basic | Workloads mit geringen Anforderungen an Rechen- und E/A-Leistung. Beispiele hierfür sind Server, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| Allgemeiner Zweck | Geeignet für die meisten Unternehmensworkloads mit gängigen Compute- und Arbeitsspeicheranforderungen und skalierbarem E/A-Durchsatz. Hierzu zählen beispielsweise zum Hosten von Web- und mobilen Apps verwendete Server und andere Unternehmensanwendungen.|
| Arbeitsspeicheroptimiert | Geeignet für Hochleistungs-Datenbankworkloads, für die In-Memory-Leistung erforderlich ist, um eine schnellere Transaktionsverarbeitung und höhere Parallelität zu erzielen. Hierzu zählen beispielsweise Server für die Verarbeitung von Echtzeitdaten und leistungsstarke Transaktions- oder Analyse-Apps.|

Nachdem Sie einen Server erstellt haben, kann die Anzahl von V-Kernen innerhalb weniger Sekunden erhöht oder verringert werden. Außerdem haben Sie die Möglichkeit, die Speichermenge einzeln zu erhöhen und den Aufbewahrungszeitraum für Sicherungen zu erhöhen oder zu verringern, ohne dass bei der Anwendung Ausfallzeiten auftreten. Weitere Informationen finden Sie im Abschnitt „Skalieren von Ressourcen“.

## <a name="compute-generations-vcores-and-memory"></a>Computegenerationen, V-Kerne und Arbeitsspeicher

Computeressourcen werden in Form von virtuellen Kernen bereitgestellt und repräsentieren die logische CPU der zugrunde liegenden Hardware. Derzeit können Sie aus zwei Computegenerationen wählen – Gen 4 und Gen 5. Logische CPUs der Generation 4 basieren auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) 2,4 GHz. Logische CPUs der Generation 5 basieren auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) 2,3 GHz. Gen 4 und 5 Gen stehen in den folgenden Regionen zur Verfügung („X“ steht für verfügbar). 

| **Azure-Region** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| USA (Mitte) |  | X |
| USA (Ost) | X | X |
| USA (Ost) 2 | X |  |
| USA Nord Mitte | X |  |
| USA Süd Mitte | X |  |
| USA (Westen) | X | X |
| USA, Westen 2 |  | X |
| Kanada, Mitte | X | X |
| Kanada, Osten | X | X |
| Brasilien Süd | X |  |
| Nordeuropa | X | X |
| Europa, Westen | X | X |
| UK, Westen |  | X |
| UK, Süden |  | X |
| Asien, Osten | X |  |
| Asien, Südosten | X |  |
| Australien (Osten) |  | X |
| Indien, Mitte | X |  |
| Indien, Westen | X |  |
| Japan, Osten | X |  |
| Japan, Westen | X |  |
| Korea, Süden |  | X |

Je nach Tarif wird jeder V-Kern mit einer bestimmten Menge an Speicher bereitgestellt. Wenn Sie die Anzahl von V-Kernen für Ihren Server erhöhen oder verringern, wird der Speicher proportional dazu ebenfalls erhöht oder verringert. Im Tarif „Allgemein“ erhalten Sie im Vergleich zum Tarif „Basic“ die doppelte Menge an Speicher pro V-Kern. Im Tarif „Arbeitsspeicheroptimiert“ erhalten Sie im Vergleich zum Tarif „Allgemein“ die doppelte Menge an Speicher.

## <a name="storage"></a>Speicher

Der von Ihnen bereitgestellte Speicher definiert die Speicherkapazität, die für Ihren Azure Database for MySQL-Server zur Verfügung steht. Der Speicher wird für die Datenbankdateien, temporären Dateien, Transaktionsprotokolle und MySQL-Serverprotokolle verwendet. Außerdem wird durch die Gesamtmenge an bereitgestelltem Speicher die E/A-Kapazität Ihres Servers definiert.

|    | **Basic** | **Allgemeiner Zweck** | **Arbeitsspeicheroptimiert** |
|:---|:----------|:--------------------|:---------------------|
| Speichertyp | Azure-Standardspeicher | Azure Storage Premium | Azure Storage Premium |
| Speichergröße | 5 GB bis 1 TB | 5 GB bis 1 TB | 5 GB bis 1 TB |
| Speicherinkrementgröße | 1 GB | 1 GB | 1 GB |
| IOPS | Variable |3 IOPS/GB<br/>Min. 100 IOPS | 3 IOPS/GB<br/>Min. 100 IOPS |

Sie können während und nach der Erstellung des Servers zusätzliche Speicherkapazität hinzufügen. Der Tarif „Basic“ umfasst keine IOPS-Garantie. Für die Tarife „Allgemein“ und „Arbeitsspeicheroptimiert“ wird der IOPS-Wert gegenüber der bereitgestellten Speichergröße in einem Verhältnis von 3:1 skaliert.

Sie können Ihren E/A-Verbrauch im Azure-Portal oder mit Azure CLI-Befehlen überwachen. Die wichtigen zu überwachenden Metriken sind das [Speicherlimit, der Speicherprozentsatz, der genutzte Speicher und der E/A-Prozentsatz](concepts-monitoring.md).

## <a name="backup"></a>Sicherung

Der Dienst erstellt automatisch Sicherungen Ihres Servers. Die Mindestaufbewahrungsdauer für Sicherungen beträgt sieben Tage. Sie können eine Aufbewahrungsdauer von bis zu 35 Tagen festlegen. Die Aufbewahrungsdauer kann während der Lebensdauer des Servers jederzeit angepasst werden. Sie können zwischen lokal redundanten und georedundanten Sicherungen wählen. Georedundante Sicherungen werden auch in der [geografisch gepaarten Region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) gespeichert, die zur Erstellungsregion Ihres Servers gehört (Regionspaar). Diese Redundanz sorgt in einem Notfall für Schutz. Sie haben auch die Möglichkeit, Ihren Server in einer beliebigen anderen Azure-Region, in der der Dienst verfügbar ist, mit georedundanten Sicherungen wiederherzustellen. Es ist nicht möglich, zwischen den beiden Sicherungsspeicheroptionen zu wechseln, sobald der Server erstellt ist.

## <a name="scale-resources"></a>Skalieren von Ressourcen

Nachdem Sie Ihren Server erstellt haben, können Sie die virtuellen Kerne, die Speichermenge und den Aufbewahrungszeitraum für Sicherungen einzeln ändern. Den Tarif oder Sicherungsspeichertyp können Sie nach der Erstellung eines Servers nicht mehr ändern. V-Kerne und die Aufbewahrungsdauer für Sicherungen können zentral hoch- oder herunterskaliert werden. Die Speichergröße kann nur erhöht werden. Die Skalierung der Ressourcen kann entweder über das Portal oder per Azure CLI durchgeführt werden. Ein Beispiel für die Skalierung mit der Azure CLI finden Sie unter [Überwachen und Skalieren eines einzelnen MySQL-Servers mit der Azure CLI](scripts/sample-scale-server.md).

Beim Ändern der Anzahl von virtuellen Kernen wird eine Kopie des ursprünglichen Servers mit der neuen Computezuteilung erstellt. Sobald der neue Server betriebsbereit ist und ausgeführt wird, werden die Verbindungen auf den neuen Server verschoben. Während des Moments, in dem das System den Wechsel zum neuen Server durchführt, können keine neuen Verbindungen hergestellt werden, und für alle Transaktionen ohne Commit erfolgt ein Rollback. Die Länge dieses Zeitfensters variiert, aber normalerweise dauert der Vorgang nicht länger als eine Minute.

Das Skalieren des Speichers und das Ändern der Aufbewahrungsdauer für Sicherungen sind reine Onlinevorgänge. Es gibt keine Ausfallzeit, und Ihre Anwendung wird nicht beeinträchtigt. Da der IOPS-Wert in Abhängigkeit der Größe des bereitgestellten Speichers skaliert wird, können Sie die IOPS-Menge, die für Ihren Server verfügbar ist, durch das zentrale Hochskalieren des Speichers erhöhen.

## <a name="pricing"></a>Preise

Aktuelle Preisinformationen finden Sie auf der Seite [Azure-Datenbank für MySQL – Preise](https://azure.microsoft.com/pricing/details/mysql/). Informationen zu den Kosten der gewünschten Konfiguration können Sie im [Azure-Portal](https://portal.azure.com/#create/Microsoft.MySQLServer) anzeigen. Die monatlichen Kosten für die von Ihnen ausgewählten Optionen werden auf der Registerkarte **Tarif** angegeben. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie den Azure-Preisrechner verwenden, um einen geschätzten Preis zu erhalten. Wählen Sie auf der Website des [Azure-Preisrechners](https://azure.microsoft.com/pricing/calculator/) die Option **Elemente hinzufügen** aus, erweitern Sie die Kategorie **Datenbanken**, und wählen Sie **Azure Database for MySQL** aus, um die Optionen anzupassen.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich, wie Sie [im Portal einen MySQL-Server erstellen](howto-create-manage-server-portal.md).
- Erfahren Sie etwas über das [Überwachen und Skalieren eines Azure Database for MySQL-Servers mithilfe der Azure CLI](scripts/sample-scale-server.md).
- Informieren Sie sich über die [Beschränkungen](concepts-limits.md).
