---
title: Bewährte Methoden für Kostenermittlung und Größenanpassung von Workloads, die zu Azure migriert werden | Microsoft-Dokumentation
description: Sie erhalten bewährte Methoden für Kostenermittlung und Größenanpassung von Workloads, die zu Azure migriert werden.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: raynew
ms.openlocfilehash: 48ce99bd830d2c35e5cb9703d2ef754a602d534b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926145"
---
# <a name="best-practices-for-costing-and-sizing-workloads-migrated-to-azure"></a>Bewährte Methoden für Kostenermittlung und Größenanpassung von Workloads, die zu Azure migriert werden

Wenn sie sich bei der Planung und dem Entwurf einer Migration auf die Kosten konzentrieren, sichert dies den langfristigen Erfolg Ihrer Azure-Migration. Während eines Migrationsprojekts ist es entscheidend, dass alle Teams (Finanzen, Verwaltung, App-Teams usw.) die damit verbundenen Kosten kennen und verstehen.
- Die Abschätzung Ihrer Migrationsausgaben vor der Migration, zusammen mit einer Baseline für die monatlichen, vierteljährlich und jährlichen Budgetziele, ist entscheidend für den Erfolg.
- Nach der Migration sollten Sie Kosten optimieren, Workloads kontinuierlich überwachen und bereits für zukünftige Verwendungsmuster planen. Migrierte Ressourcen können eventuell als eine Art von Workload beginnen, sich jedoch im Laufe der Zeit in einen anderen Typ entwickeln, basierend auf Nutzung, Kosten und sich verändernden Geschäftsanforderungen.

In diesem Artikel werden bewährte Methoden für Kostenermittlung und Größenanpassung vor und nach der Migration erläutert.  

> [!IMPORTANT]
> Die in diesem Artikel wiedergegebenen bewährten Methoden und Meinungen basieren auf Azure-Plattform- und --Dienstfunktionen, die zum Zeitpunkt der Niederschrift verfügbar sind. Features und Funktionen ändern sich im Laufe der Zeit. Nicht alle Empfehlungen sind notwendigerweise auf Ihre Bereitstellung anwendbar. Wählen Sie also aus, was für Sie funktioniert.


## <a name="before-migration"></a>Vor der Migration 

Bevor Sie Ihre Workloads in die Cloud verschieben, schätzen Sie die monatlichen Kosten für ihren Betrieb in Azure ab. Die proaktive Verwaltung von Cloudkosten hilft Ihnen dabei, Ihr Budget für Betriebsaufwendungen (OpEx) einzuhalten. Wenn das Budget beschränkt ist, berücksichtigen Sie dies vor der Migration.  Erwägen Sie, Workloads in serverlose Azure-Technologien zu konvertieren, falls möglich, um Kosten zu senken.

Die bewährten Methoden in diesem Abschnitt helfen Ihnen, Kosten abzuschätzen, die Größe von virtuellen Computern und Speicher richtig anzupassen, Azure-Hybridvorteile zu nutzen, reservierte virtuelle Computer zu verwenden und Cloudausgaben über Abonnements hinweg abzuschätzen.



## <a name="best-practice-estimate-monthly-workload-costs"></a>Bewährte Methode: Monatliche Workloadkosten abschätzen
 
Um eine Prognose für Ihre monatliche Rechnung für migrierte Workloads aufzustellen, gibt es eine Reihe von Tools, die Sie verwenden können.

- **Azure-Preisrechner**: Sie wählen die Produkte aus, die Sie schätzen möchten, z. B. virtuelle Computer und Speicher. Sie geben Kosten in den Preisrechner ein, um eine Schätzung zu erstellen.

  ![Azure-Preisrechner](./media/migrate-best-practices-costs/pricing.png) *Azure-Preisrechner*

- **Azure Migrate**: Um Kosten zu schätzen, müssen Sie alle Ressourcen überprüfen und berücksichtigen, die für die Ausführung Ihrer Workloads in Azure erforderlich sind. Um diese Daten zu sammeln, erstellen Sie ein Inventar Ihrer Ressourcen, einschließlich Servern, virtueller Computer, Datenbanken und Speicher. Sie können Azure Migrate verwenden, um diese Informationen zu sammeln.

  - Azure Migrate ermittelt und bewertet Ihre lokale Umgebung, um ein Inventar aufzustellen.
  - Azure Migrate kann Abhängigkeiten zwischen VMs zuordnen und Ihnen aufzeigen, damit Sie ein vollständiges Bild erhalten.
  - Eine Azure Migrate-Bewertung enthält geschätzte Kosten.
    - Compute-Kosten: Wenn Sie eine Bewertung erstellen, berechnet Azure Migrate anhand der empfohlenen Azure-VM-Größe mithilfe der Abrechnungs-API die geschätzten monatlichen Kosten für den virtuellen Computer. Bei der Schätzung werden Betriebssystem, Software Assurance, reservierte Instanzen, VM-Betriebszeit, Standort und Währungseinstellungen berücksichtigt. Die Kosten aller virtuellen Computer werden in der Bewertung zusammengefasst, um so die monatlichen Compute-Gesamtkosten zu berechnen.
    - Speicherkosten: Azure Migrate berechnet die monatlichen Speichergesamtkosten durch Aggregieren der Speicherkosten aller virtuellen Computer in einer Bewertung. Sie können die monatlichen Speicherkosten für einen bestimmten Computer berechnen, indem Sie die monatlichen Kosten aller an den Computer angefügten Datenträger zusammenfassen. 

    ![Azure Migrate](./media/migrate-best-practices-costs/assess.png)
    *Azure Migrate-Bewertung*

**Weitere Informationen**:
- [Verwenden](https://azure.microsoft.com/pricing/calculator/) des Azure-Preisrechners.
- [Verschaffen eines Überblicks](https://docs.microsoft.com/azure/migrate/migrate-overview) über Azure Migrate.
- [Lesen von Informationen zu](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation) Azure Migrate-Bewertungen.
- [Weitere Informationen](https://docs.microsoft.com/azure/dms/dms-overview) über den Datenbankmigrationsdienst (DMS).

## <a name="best-practice-right-size-vms"></a>Bewährte Methode: Richtige Größe von virtuellen Computern anpassen

Sie können eine Reihe von Optionen auswählen, wenn Sie virtuelle Azure-Computer bereitstellen, um Workloads zu unterstützen. Jeder VM-Typ verfügt über bestimmte Features und verschiedene Kombinationen aus CPU, Arbeitsspeicher und Datenträgern. VMs werden wie folgt gruppiert.

**Typ** | **Details** | **Verwenden Sie**
--- | --- | ---
**Allgemeiner Zweck** | Ausgewogenes Verhältnis von CPU zu Arbeitsspeicher. | Gut geeignet für Tests und Entwicklung, kleine bis mittlere Datenbanken, Webserver mit geringer bis mittlerer Auslastung.
**Compute-optimiert** | Hohes Verhältnis von CPU zu Arbeitsspeicher. | Gut geeignet für Webserver, Network Appliances, Stapelverarbeitungsvorgänge, Anwendungsserver.
**Arbeitsspeicheroptimiert** | Hohes Verhältnis von Speicher zu CPU. | Gut geeignet für relationale Datenbanken, mittlere bis große Caches, In-Memory-Analysen.
**Speicheroptimiert** | Datenträgerdurchsatz und -E/A auf hohem Niveau. | Gut geeignet für Big Data sowie SQL- und NoSQL-Datenbanken.
**GPU-optimiert** | Spezialisierte VMs. Einzelne oder mehrere GPUs. | Anspruchsvolle Grafik- und Videobearbeitung.
**Hohe Leistung** | Schnellste und leistungsfähigste CPU. Virtuelle Computer mit optionalen Netzwerkschnittstellen mit hohem Durchsatz (RDMA). | Kritische Hochleistungs-Apps.

- Es ist wichtig, die Preisunterschiede zwischen diesen virtuellen Computern zu verstehen sowie die langfristigen Auswirkungen auf das Budget.
- Jeder Typ umfasst eine bestimmte Anzahl darin enthaltener VM-Serien.
- Außerdem können Sie, wenn Sie eine VM innerhalb einer Serie auswählen, die VM nur innerhalb dieser Serie zentral hoch- oder herunterskalieren. Beispielsweise kann eine DSv2\_2 zentral zu DSv2\_4 hochskaliert werden, aber sie kann nicht in eine andere Serie wie Fsv2\_2 geändert werden.

**Weitere Informationen:**
- [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) zu VM-Typen und -Größenanpassung sowie zur Zuordnung von Größen zu Typen.
- [Planen der](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs) VM-Größenanpassung.
- [Lesen](https://docs.microsoft.com/azure/migrate/contoso-migration-assessment) einer Beispielbewertung für das fiktive Unternehmen Contoso.

## <a name="best-practice-select-the-right-storage"></a>Bewährte Methode: Den richtigen Speicher auswählen

Das Optimieren und Verwalten von lokalem Speicher (SAN oder NAS) sowie der Netzwerke, die diesen unterstützen, können teuer und zeitaufwendig sein. Datei(Speicher)daten werden häufig zur Cloud migriert, um Schwierigkeiten beim Betrieb und der Verwaltung zu beseitigen. Microsoft bietet verschiedene Optionen zum Verschieben von Daten in Azure, und Sie müssen sich hinsichtlich dieser Optionen entscheiden. Die Auswahl des richtigen Speichertyps für Daten kann Ihrer Organisation jeden Monat mehrere tausend Euro sparen. Ein paar Überlegungen:

- Daten, auf die nicht häufig zugegriffen wird und die nicht unternehmenskritisch sind, müssen nicht auf dem teuersten Speicher abgelegt werden.
- Umgekehrt sollten sich unternehmenskritische Daten auf Speicheroptionen eines höheren Tarifs befinden.
- Erstellen Sie während der Migrationsplanung ein Inventar der Daten, und klassifizieren Sie sie nach Wichtigkeit, um sie dem jeweils optimal geeigneten Speicher zuzuordnen. Berücksichtigen Sie hierbei Budget und Kosten sowie die Leistung. Kosten sollte nicht notwendigerweise der ausschlaggebende Entscheidungsfaktor sein. Die Entscheidung für die kostengünstigste Option könnte die Workload Leistungs- und Verfügbarkeitsrisiken aussetzen. 

### <a name="storage-data-types"></a>Speicherdatentypen

Azure bietet verschiedene Arten von Speicherdaten.

| **Datentyp** | **Details** | **Verwendung** |
|--- | --- |  --- |
|**Blobs** | Optimiert für die Speicherung großer Mengen unstrukturierter Objekte wie etwa Text- oder Binärdaten.<br/>Zugriff auf Daten von überall her über HTTP/HTTPS. | Verwendung für Szenarios mit Streaming und wahlfreiem Zugriff. Beispielsweise um einem Browser Bilder und Dokumente direkt bereitzustellen, Video und Audio zu streamen und Sicherungs- und Notfallwiederherstellungsdaten zu speichern.|
|**Dateien** | Verwaltete Dateifreigaben, auf die über SMB 3.0 zugegriffen wird. | Verwendung beim Migrieren lokaler Dateifreigaben sowie zur Bereitstellung von mehrfachem Zugriff bzw. mehreren Verbindungen, um Daten abzulegen.|
|**Datenträger** | Basierend auf Seitenblobs.<br/><br/> Datenträgertyp (Geschwindigkeit): Standard (HDD oder SSD) oder Premium (SSD)<br/><br/>Datenträgerverwaltung: Nicht verwaltet (Sie verwalten Datenträgereinstellungen und Speicher) oder verwaltet (Sie wählen den Datenträgertyp aus, und Azure verwaltet diesen Datenträger für Sie). | Verwendung von Premium-Datenträgern für VMs. Verwendung von verwalteten Datenträgern für einfache Verwaltung und Skalierung.|
|**Warteschlangen** | Speichern und Abrufen große Mengen von Nachrichten mittels Zugriff über authentifizierte Aufrufe (HTTP oder HTTPS). | Verbinden von App-Komponenten mit asynchronem Message Queuing.|
|**Tabellen** | Speichern von Tabellen. | Jetzt Teil der Azure Cosmos DB-Tabellen-API.|



### <a name="access-tiers"></a>Zugriffsebenen
Azure Storage bietet verschiedene Optionen für den Zugriff auf Blockblobdaten. Die Auswahl der richtigen Zugriffsebene hilft sicherzustellen, dass Sie Blockblobdaten auf möglichst kostengünstige Art und Weise speichern.

**Typ** | **Details** | **Verwendung**
--- | --- | ---
**Heiße Ebene** | Höhere Speicherkosten als kalte Ebene. Niedrigere Zugriffsgebühren als kalte Ebene.<br/><br/>Dies ist die Standardebene. | Verwendung für Daten, die aktiv und mit häufigem Zugriff verwendet werden.
**Kalte Ebene** | Niedrigere Speicherkosten als heiße Ebene. Höhere Zugriffsgebühren als heiße Ebene.<br/><br/> Speicherung für mindestens 30 Tage. | Kurzfristige Speicherung, Daten sind verfügbar, aber Zugriff erfolgt nur selten.
**Archivieren** | Verwendung für einzelne Blockblobs.<br/><br/> Kostengünstigste Option für Speicherung. Datenzugriff ist teurer als bei heißer oder kalter Ebene. | Verwendung für Daten, die mehrere Stunden Serverabrufwartezeit tolerieren und mindestens 180 Tage lang auf dieser Ebene verbleiben. 

### <a name="storage-account-types"></a>Speicherkontotypen

Azure bietet verschiedene Typen von Speicherkonten und Leistungsstufen.

**Kontotyp** | **Details** | **Verwendung**
--- | --- | ---
**Allgemein v2 Standard** | Unterstützt Blobs (Block, Seiten, Anfüge), Dateien, Datenträger, Warteschlangen und Tabellen.<br/><br/> Unterstützt die Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“. ZRS wird unterstützt. | Verwendung für die meisten Szenarios und die meisten Typen von Daten. Standardspeicherkonten können auf HDDs oder SSDs basieren.
**Allgemein v2 Premium** | Unterstützt die Blobspeicherdaten (Seitenblobs). Unterstützt die Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“. ZRS wird unterstützt.<br/><br/> Speicherung auf SSDs. | Microsoft empfiehlt die Verwendung für alle VMs.
**Allgemein v1** | Zugriffsebenen werden nicht unterstützt. Keine Unterstützung von ZRS. | Verwendung, wenn Apps das klassische Azure-Bereitstellungsmodell benötigen.
**Blob** | Spezialisiertes Speicherkonto zum Speichern unstrukturierter Objekte. Bietet nur Blockblobs und Anfügeblobs (keine Datei-, Warteschlangen-, Tabellen- oder Datenträgerspeicherdienste). Bietet dieselbe Dauerhaftigkeit, Verfügbarkeit, Skalierbarkeit und Leistung wie Allgemein v2. | Sie können in diesen Konten keine Seitenblobs speichern, weshalb auch keine VHD-Dateien gespeichert werden können. Sie können eine Zugriffsebene auf „Heiß“ oder „Kalt“ festlegen.

### <a name="storage-redundancy-options"></a>Redundanzoptionen für Storage

Speicherkonten können verschiedene Arten von Redundanz für Resilienz und Hochverfügbarkeit verwenden.

**Typ** | **Details** | **Verwendung**
--- | --- | ---
**Lokal redundanter Speicher (LRS)** | Schützt vor einem lokalen Ausfall durch Replikation in eine einzelne Speichereinheit in einer gesonderten Fehler- und Updatedomäne. Behält mehrere Kopien Ihrer Daten in einem Rechenzentrum. Bietet eine Dauerhaftigkeit von mindestens 99,999999999 % (11 Neunen) für Objekte in einem bestimmten Jahr. | Erwägen Sie, ob Ihre Anwendung Daten speichert, die problemlos wiederhergestellt werden können.
**Zonenredundanter Speicher (Zone Redundant Storage, ZRS)** | Schützt vor Rechenzentrumsausfällen, indem über drei Speichercluster in einer einzelnen Region hinweg repliziert wird. Jeder Speichercluster ist physisch unabhängig und befindet sich in einer eigenen Verfügbarkeitszone. Bietet eine Dauerhaftigkeit von mindestens 99,9999999999 % (12 Neunen) für Objekte für ein bestimmtes Jahr, indem mehrere Kopien Ihrer Daten in mehreren Rechenzentren oder mehreren Regionen gespeichert werden. | Überlegen Sie, ob Sie Konsistenz, Dauerhaftigkeit und Hochverfügbarkeit benötigen. Schützt eventuell nicht vor einem regionalen Notfall, wenn mehrere Zonen dauerhaft betroffen sind.
**Geografisch redundanter Speicher (GRS)** | Schützt vor dem Ausfall einer vollständigen Region, indem Daten in eine sekundäre Region repliziert werden, die mehrere hundert Kilometer vom primären Speicherort der Quelldaten entfernt ist. Bietet eine Dauerhaftigkeit von mindestens 99,99999999999999 % (16 Neunen) für Objekte in einem bestimmten Jahr. | Replikatdaten sind nur verfügbar, wenn Microsoft ein Failover in die sekundäre Region initiiert. Kommt es zu einem Failover, sind Lese- und Schreibzugriff verfügbar.
**Georedundanter Speicher mit Lesezugriff (RA-GRS)** | Ähnlich wie GRS. Bietet eine Dauerhaftigkeit von mindestens 99,99999999999999 % (16 Neunen) für Objekte in einem bestimmten Jahr. | Bietet außerdem 99,99 % Leseverfügbarkeit, indem Lesezugriff aus der zweiten für GRS verwendeten Region zugelassen wird.

**Weitere Informationen:**
- [Sehen Sie sich](https://azure.microsoft.com/pricing/details/storage/) die Preise für Azure Storage an.
- [Weitere Informationen zum](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) Azure Import/Export für die Migration großer Datenmengen zu Azure-Blobs und Azure Files. 
- [Vergleichen Sie](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Datentypen für Blobs, Dateien und Datenträgerspeicher.
- [Weitere Informationen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) zu Zugriffsebenen.
- [Informationen zu ](https://docs.microsoft.com/azure/storage/common/storage-account-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) verschiedene Typen von Speicherkonten.
- Informationen zu [Speicherredundanz](https://docs.microsoft.com/azure/storage/common/storage-redundancy), [LRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), [ZRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), [GRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) und [GRS mit Lesezugriff](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#read-access-geo-redundant-storage).
- [Weitere Informationen zu](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) Azure Files.



## <a name="best-practice-leverage-azure-hybrid-benefits"></a>Bewährte Methode: Azure-Hybridvorteile nutzen

Dank jahrelanger Investitionen in Softwaresysteme wie Windows Server und SQL Server befindet sich Microsoft in der einzigartigen Lage, Kunden Wertschöpfung in der Cloud mit beträchtlichen Rabatte anbieten zu können, die andere Cloudanbieter nicht unbedingt bereitstellen können. 

Eine integriertes Portfolio aus lokalen und Azure-Produkten von Microsoft generiert Wettbewerbs- und Kostenvorteile. Wenn Sie derzeit eine Betriebssystem- oder anderweitige Softwarelizenzierung durch Software Assurance (SA) besitzen, können Sie diese Lizenzen mit in die Cloud nehmen, um die Azure-Hybridvorteile zu nutzen.

**Weitere Informationen:**

- [Werfen Sie einen Blick auf den](https://azure.microsoft.com/pricing/hybrid-benefit/) Einsparungsrechner für den Azure-Hybridvorteil.
- [Weitere Informationen zum](https://azure.microsoft.com/pricing/hybrid-benefit/) Hybridvorteil für Windows Server.
- [Sehen Sie sich die](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) Preisinformationen für virtuelle Azure-Computer mit SQL Server an.


## <a name="best-practice-use-reserved-vm-instances"></a>Bewährte Methode: Reservierte VM-Instanzen verwenden

Die meisten Cloudplattformen sind mit nutzungsbasierter Bezahlung eingerichtet. Dieses Modell beinhaltet Nachteile, da Sie nicht unbedingt wissen, wie sich dynamische Arbeitslasten entwickeln. Wenn Sie also klare Absichten für eine Workload formulieren, tragen Sie damit zur Infrastrukturplanung bei.

Wenn Sie reservierte Azure-VM-Instanzen verwenden, zahlen Sie im Voraus für eine 1- oder 3-Jahres-Instanz einer VM. 

- Durch die Vorauszahlung erhalten Sie einen Rabatt für die verwendeten Ressourcen.
- Sie können die Kosten für virtuelle Computer, SQL-Datenbank-Compute, Azure Cosmos DB-Kapazitäten oder andere Ressourcen deutlich um bis zu 72 % gegenüber der nutzungsbasierten Bezahlung reduzieren. 
- Reservierungen bieten einen Abrechnungsrabatt und wirken sich nicht auf den Laufzeitstatus Ihrer Ressourcen aus.
- Sie können reservierte Instanzen kündigen.

![Reservierte Instanzen](./media/migrate-best-practices-costs/reserve.png)
*Reservierte Azure-VMs*

**Weitere Informationen:**
- [Informationen zu](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) Azure-Reservierungen.
- [Lesen Sie](https://azure.microsoft.com/pricing/reserved-vm-instances/#faq) die häufig gestellten Fragen zu reservierten Instanzen.
- [Erhalten Sie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) Preisinformationen für virtuelle Azure-Computer mit SQL Server.


## <a name="best-practice-aggregate-cloud-spend-across-subscriptions"></a>Bewährte Methode: Cloudausgaben über Abonnements hinweg akkumulieren

Es ist unvermeidlich, dass Sie letztendlich mehr als ein Azure-Abonnement haben werden. Beispielsweise könnten Sie ein zusätzliches Abonnement benötigen, um Entwicklungs- und Produktionsumgebungen voneinander zu trennen, oder Sie haben möglicherweise eine Plattform, die ein separates Abonnement für jeden Client erfordert. Die Möglichkeit zu besitzen, die Berichterstellung zu Daten über alle Abonnements hinweg in einer einzelnen Plattform zusammenzufassen, ist eine wertvolle Funktion.

Um dies zu erreichen, können Sie Azure Cost Management-APIs verwenden. Nachdem Sie dann Daten in eine einzelne Quelle aggregiert haben, z. B. Azure SQL, können Sie mit Tools wie Power BI die aggregierten Daten analysieren. Sie können aggregierte Abonnementberichte sowie detaillierte Berichte erstellen. Für Benutzer, die proaktive Einblicke in das Kostenmanagement benötigen, können Sie beispielsweise spezifische Sichten für Kosten erstellen, gegliedert nach Abteilung, Ressourcengruppe usw. Sie müssen ihnen dazu keinen Vollzugriff auf Azure-Abrechnungsdaten gewähren.

**Weitere Informationen:**

- [Erhalten Sie einen Überblick](https://docs.microsoft.com/azure/billing/billing-consumption-api-overview) über die Azure-Nutzungs-API.
- [Erfahren Sie mehr über](https://docs.microsoft.com/power-bi/desktop-connect-azure-consumption-insights) das Herstellen einer Verbindung mit Azure Consumption Insights in Power BI Desktop.
- [Erfahren Sie, wie Sie](https://docs.microsoft.com/azure/billing/billing-manage-access) den Zugriff auf Abrechnungsinformationen für Azure mithilfe der rollenbasierten Zugriffssteuerung (RBAC) verwalten.
 
## <a name="after-migration"></a>Nach der Migration

Im Anschluss an eine erfolgreiche Migration Ihrer Workloads und nach ein paar Wochen des Sammelns von Verbrauchsdaten haben Sie eine klare Vorstellung von den Kosten der Ressourcen.
- Parallel zur Analyse der Daten können Sie beginnen, eine Budgetbaseline für Azure-Ressourcengruppen und -Ressourcen zu generieren.
- Wenn Sie dann zunehmend herausfinden, wo Ihr Budget für die Cloud verbraucht wird, können Sie analysieren, wie Sie Ihre Kosten noch weiter senken können.

Bewährte Methoden in diesem Abschnitt umfassen die Verwendung von Azure Cost Management für die Budgetierung und Analyse von Kosten, die Überwachung von Ressourcen und die Implementierung von Budgets für Ressourcengruppen sowie die Optimierung der Überwachung, von Speicher und VMs.

## <a name="best-practice-use-azure-cost-management"></a>Bewährte Methode: Azure Cost Management verwenden

Microsoft bietet Azure Cost Management, um Ihnen bei der Nachverfolgung Ihrer Ausgaben wie folgt zu helfen:

- Unterstützt Sie bei der Überwachung und Kontrolle der Azure-Ausgaben sowie bei der Optimierung der Ressourcennutzung.
- Überprüft Ihr gesamtes Abonnement mit allen darin enthaltenen Ressourcen und gibt Empfehlungen.
- Bietet eine vollständige API, um externe Tools und Finanzsysteme für die Berichterstellung zu integrieren.
- Verfolgt die Ressourcennutzung und Verwaltet Cloudkosten mit einer einzigen, einheitlichen Ansicht.
- Bietet umfassende Erkenntnisse zu Betrieb und Finanzen, um Sie bei fundierten Entscheidungen zu unterstützen.

Mit Cost Management können Sie:


- **Budget erstellen**: Erstellen Sie ein Budget zur Wahrnehmung Ihrer finanziellen Verantwortung.
  - Sie können dabei Dienste berücksichtigen, die Sie für einen bestimmten Zeitraum verwenden oder abonnieren (monatlich, vierteljährlich, jährlich), sowie einen Bereich (Abonnements/Ressourcengruppen). Beispielsweise können Sie ein Azure-Abonnementbudget für den Zeitraum eines Monats, Quartals oder Jahres erstellen.
  - Nach der Erstellung eines Budgets wird es in der Kostenanalyse angezeigt. Die Betrachtung Ihres Budgets in Bezug auf Ihre aktuellen Ausgaben ist einer der ersten Schritte, die zur Analyse Ihrer Kosten und Ausgaben erforderlich sind.
  - Sie können E-Mail-Benachrichtigungen versenden lassen, wenn Budgetschwellenwerte erreicht werden.
  - Sie können Kostenverwaltungsdaten zur Analyse nach Azure Storage exportieren.

    ![Cost Management-Budget](./media/migrate-best-practices-costs/budget.png)
    *Azure Cost Management-Budget*

- **Kostenanalyse durchführen**: Stellen Sie eine Kostenanalyse auf, um Ihre Organisationskosten zu untersuchen und zu analysieren, damit Sie besser verstehen, wie Kosten anfallen, und Ausgabentrends erkennen können.
  - Die Kostenanalyse steht EA-Benutzern zur Verfügung.
  - Sie können Kostenanalysedaten für eine Reihe von Bereichen, einschließlich nach Abteilung, Konto, Abonnement oder Ressourcengruppe, anzeigen.
  - Sie können eine Kostenanalyse abrufen, die Gesamtkosten für den aktuellen Monat und die akkumulierten täglichen Kosten anzeigt. 

    ![Cost Management-Analyse](./media/migrate-best-practices-costs/analysis.png)
    *Azure Cost Management-Analyse*
- **Empfehlungen abrufen**: Erhalten Sie Advisor-Empfehlungen, die Ihnen zeigen, wie Sie die Effizienz optimieren und verbessern können.


**Weitere Informationen:**

- [Einen Überblick erhalten](https://docs.microsoft.com/azure/cost-management/overview) über Azure Cost Management.
- [Erfahren Sie, wie Sie](https://docs.microsoft.com/azure/cost-management/cost-mgt-best-practices) Ihre Cloudinvestitionen mit Azure Cost Management optimieren.
- [Erfahren Sie, wie Sie](https://docs.microsoft.com/azure/cost-management/use-reports) Azure Cost Management-Berichte verwenden.
- [Erhalten Sie ein Tutorial](https://docs.microsoft.com/azure/cost-management/tutorial-acm-opt-recommendations?toc=/azure/billing/TOC.json) zum Optimieren von Kosten mithilfe von Empfehlungen.
- [Sehen Sie sich ](https://docs.microsoft.com/rest/api/consumption/budgets) die Azure Consumption-API an.

## <a name="best-practice-monitor-resource-utilization"></a>Bewährte Methode: Ressourcennutzung überwachen

In Azure bezahlen Sie für die Nutzung, also wenn Ressourcen tatsächlich genutzt werden, und Sie bezahlen nicht, wenn Sie sie nicht nutzen. Bei virtuellen Computern erfolgt die Abrechnung, wenn ein virtueller Computer zugeordnet wird, und nachdem die Zuordnung eines virtuellen Computers aufgehoben wurde, fallen keine Kosten mehr an. Unter Berücksichtigung dieses Aspekts sollten Sie verwendete VMs überwachen und die Größenanpassung von VMs überprüfen.

- Bewerten Sie kontinuierlich Ihre VM-Workloads, um Baselines zu bestimmen.
- Wenn Ihre Workload beispielsweise montags bis freitags zwischen 8: 00 und 18 Uhr stark verwendet wird, außerhalb dieser Zeiten jedoch kaum, könnten Sie virtuelle Computer außerhalb der Spitzenzeiten herabstufen. Dies kann bedeuten, dass Sie VM-Größen ändern oder VM-Skalierungsgruppen zum automatischen Hoch- oder Herunterskalieren von VMs verwenden könnten.
- Einige Unternehmen lassen VMs „schlummern“, indem Sie sie nach einem Zeitplan betreiben, der angibt, wann sie verfügbar sein sollten und wann sie nicht benötigt werden. 
- Zusätzlich zur Überwachung virtueller Computer sollten Sie weitere Netzwerkressourcen wie ExpressRoute und Gateways für virtuelle Netzwerke auf zu niedrige und zu hohe Nutzung überwachen.
- Sie können die VM-Nutzung mithilfe einer Reihe von Microsoft-Tools überwachen, einschließlich Azure Cost Management, Azure Monitor und Azure Advisor. Drittanbietertools sind ebenfalls verfügbar.  

**Weitere Informationen:**
- Verschaffen Sie sich einen Überblick über [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) und [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview).
- [Erhalten Sie](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations) Advisor-Empfehlungen zu Kosten.
- [Erfahren Sie, wie Sie [Kosten für Empfehlungen optimieren](https://docs.microsoft.com/azure/cost-management/tutorial-acm-opt-recommendations?toc=/azure/billing/TOC.json) und [unerwartete Gebühren vermeiden](https://docs.microsoft.com/azure/billing/billing-getting-started).
- [Erfahren Sie mehr über](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit/) das Toolkit zur Azure-Ressourcenoptimierung (ARO).

## <a name="best-practice-implement-resource-group-budgets"></a>Bewährte Methode: Budgets für Ressourcengruppen implementieren

Häufig werden Ressourcengruppen verwendet, um Kostengrenzen darzustellen. Zusammen mit diesem Verwendungsmuster entwickelt das Azure-Team weiterhin neue und verbesserte Möglichkeiten zum Nachverfolgen und Analysieren der Ressourcenausgaben auf unterschiedlichen Ebenen, einschließlich der Möglichkeit zur Erstellung von Budgets für Ressourcengruppen und Ressourcen.  

- Ein Ressourcengruppenbudget hilft Ihnen bei der Nachverfolgung der zu einer Ressourcengruppe gehörigen Kosten.
- Sie können außerdem Benachrichtigungen auslösen und eine Vielzahl von Playbooks ausführen, wenn das Budget erreicht oder überschritten wird. 

**Weitere Informationen:**

- [Erfahren Sie, wie Sie](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario) Kosten mit Azure-Budgets verwalten.
- [Absolvieren Sie ein Tutorial](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?toc=/azure/billing/TOC.json), um ein Azure-Budget zu erstellen und zu verwalten.


## <a name="best-practice-optimize-azure-monitor-retention"></a>Bewährte Methode: Azure Monitor-Aufbewahrung optimieren

Wenn Sie Ressourcen in Azure verschieben und die Diagnoseprotokollierung für sie aktivieren, generieren Sie eine Menge Protokolldaten. In der Regel werden diese Protokolldaten an ein Speicherkonto gesendet, das einem Log Analytics-Arbeitsbereich zugeordnet ist.

- Je länger der Aufbewahrungszeitraum der Protokolldaten ist, desto mehr Daten laufen bei Ihnen auf.
- Nicht alle Protokolldaten sind gleich, und einige Ressourcen generieren mehr Protokolldaten, als andere.
- Aufgrund von Bestimmungen und Compliance ist es wahrscheinlich, dass Sie Protokolldaten für manche Ressourcen länger aufbewahren müssen, als für andere.
- Sie sollten sorgfältig abwägen zwischen der Optimierung Ihrer Kosten für die Protokollspeicherung und der Aufbewahrung der erforderlichen Protokolldaten.
- Wir empfehlen, dass Sie die Protokollierung sofort nach Abschluss einer Migration bewerten und einrichten, damit Sie kein Geld dafür aufwenden, unwichtige Protokolle aufzubewahren.

**Weitere Informationen:**

- [Informationen zum](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs) Überwachen der Nutzung und der geschätzten Kosten.
 
## <a name="best-practice-optimize-storage"></a>Bewährte Methode: Speicher optimieren

Wenn Sie vor der Migration bewährte Methoden für die Auswahl von Speicher befolgt haben, ernten Sie wahrscheinlich einige Vorteile. Es wird allerdings wahrscheinlich noch zusätzliche Speicherkosten geben, die Sie noch weiter optimieren können. Im Laufe der Zeit veralten Blobs und Dateien. Daten werden eventuell nicht mehr verwendet, doch die gesetzliche Anforderungen können verlangen, dass Sie sie noch für einen bestimmten Zeitraum beibehalten müssen. Solche Daten müssen Sie also nicht unbedingt auf Hochleistungsspeicher lagern, den Sie für die ursprüngliche Migration verwendet haben.

Das Identifizieren und Verschieben veralteter Daten in kostengünstigere Speicherbereiche kann große Auswirkungen auf Ihr monatliches Speicherbudget und damit einhergehende Kosteneinsparungen haben. Azure bietet viele Möglichkeiten, um Ihnen bei der Identifizierung und anschließenden Speicherung dieser veralteten Daten zu helfen.

- Profitieren Sie von Zugriffsebenen für „Allgemeinen v2“-Speicher, indem Sie weniger wichtige Daten von der heißen Speicherebene zur kalten bzw. „Archiv“-Speicherebene verschieben.
- Verwenden Sie StorSimple zur Unterstützung beim Verschieben veralteter Daten, basierend auf benutzerdefinierten Richtlinien. 

**Weitere Informationen:**
- [Weitere Informationen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) zu Zugriffsebenen.
- [Erhalten Sie eine Übersicht über](https://docs.microsoft.com/azure/azure-monitor/overview) StorSimple und [StorSimple-Preise](https://azure.microsoft.com/pricing/details/storsimple/).

## <a name="best-practice-automate-vm-optimization"></a>Bewährte Methode: VM-Optimierung automatisieren

Das Hauptziel für die Ausführung einer VM in der Cloud ist die Maximierung von CPU, Arbeitsspeicher und Datenträger, die davon verwendet werden. Wenn Sie VMs ermitteln, die nicht optimiert sind, oder wenn Sie häufige Zeiträume haben, in denen VMs nicht verwendet werden, ist es sinnvoll, diese entweder herunterzufahren oder sie mithilfe von VM-Skalierungsgruppen herunterzuskalieren.

Sie können einen virtuellen Computer mit Azure Automation, VM-Skalierungsgruppen, automatischem Herunterfahren und Skript- oder Drittanbieterlösungen optimieren. 

**Weitere Informationen:**

- [Erfahren Sie, wie Sie](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision) vertikale automatische Skalierung verwenden.
- [Planen](https://azure.microsoft.com/updates/azure-devtest-labs-schedule-vm-auto-start/) des automatischen Starts einer VM.
- [Informationen, wie Sie ](https://docs.microsoft.com/azure/automation/automation-solution-vm-management) VMs außerhalb von Nutzungszeiten in Azure Automation starten oder beenden.
- [Erfahren Sie mehr] über [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview) und das [Toolkit zur Azure-Ressourcenoptimierung (ARO)](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit/).

## <a name="best-practices-use-logic-apps-and-runbooks-with-budgets-api"></a>Bewährten Methoden: Logic Apps und Runbooks mit der API für Budgets verwenden

Azure bietet eine REST-API, die Zugriff auf die Abrechnungsinformationen Ihres Mandanten hat.

- Sie können die API für Budgets verwenden, um externe Systeme und Workflows zu integrieren, die von Metriken ausgelöst werden, die Sie aus den API-Daten erstellen.
- Sie können Nutzungs- und Ressourcendaten mittels Pull in Ihre bevorzugten Datenanalysetools abrufen.
- Mithilfe der Azure-Ressourcennutzungs- und -Gebührenkarten-APIs können Sie Ihre Kosten genau vorhersagen und verwalten.
- Die APIs werden als Ressourcenanbieter implementiert und sind in den APIs enthalten, die von Azure Resource Manager verfügbar gemacht werden.
- Die API für Budgets kann in Azure Logic Apps und Runbooks integriert werden.

**Weitere Informationen:**

- [Erfahren Sie mehr über die](https://docs.microsoft.com/rest/api/consumption/budgets) API für Budgets.
- [Gewinnen von Erkenntnissen](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) über die Azure-Nutzung mit der Abrechnungs-API.


## <a name="best-practice-implement-serverless-technologies"></a>Bewährte Methode: Serverlose Technologien implementieren

VM-Workloads werden häufig im Istzustand („wie besehen“) migriert, um Ausfallzeiten zu vermeiden. Häufig hosten virtuelle Computer Aufgaben, die nur zwischenzeitlich ausgeführt werden und dafür nur einen kurzen Zeitraum beanspruchen, oder alternativ dazu viele Stunden. Beispielsweise VMs, die geplante Aufgaben ausführen, wie die Windows-Aufgabenplanung oder PowerShell-Skripts. Wenn diese Aufgaben nicht ausgeführt werden, belegen Sie dennoch eine VM und verursachen Kosten für Datenträgerspeicher.

Nach der Migration und einer gründlichen Überprüfung dieser Arten von Aufgaben sollten Sie erwägen, diese zu serverlosen Technologien wie Azure Functions oder Azure Batch-Aufträgen zu migrieren. Mit einer solchen Lösung müssen Sie die VMs nicht mehr verwalten und pflegen, was zu weiteren Kosteneinsparungen führt. 

**Weitere Informationen:**
- [Informationen zu](https://azure.microsoft.com/services/functions/) Azure Functions.
- [Informationen zu](https://azure.microsoft.com/services/batch/) Azure Batch.
  
## <a name="next-steps"></a>Nächste Schritte 

Sehen Sie sich weitere bewährte Methoden an:

- [Bewährte Methoden](migrate-best-practices-security-management.md) für Sicherheit und Verwaltung nach der Migration.
- [Bewährte Methoden](migrate-best-practices-networking.md) für Netzwerke nach der Migration.

