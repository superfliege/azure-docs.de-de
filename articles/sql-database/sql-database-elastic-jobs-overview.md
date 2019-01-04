---
title: Verwalten horizontal hochskalierter Clouddatenbanken | Microsoft Docs
description: Verwenden Sie den Auftragsdienst der elastischen Datenbank, um ein Skript für eine Gruppe von Datenbanken auszuführen.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 10/22/2018
ms.openlocfilehash: 9647522f4b3990d065f292f05934b8d19c691454
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865521"
---
# <a name="managing-scaled-out-cloud-databases"></a>Verwalten horizontal hochskalierter Clouddatenbanken

[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]

**Aufträge für die elastische Datenbank** ist ein vom Kunden gehosteter Azure-Clouddienst, der die geplante und die Ad-hoc-Ausführung von Verwaltungsaufgaben, sogenannten **Aufträgen**, ermöglicht. Mit Aufträgen lassen sich große Gruppen von Azure SQL-Datenbanken durch Ausführen von Transact-SQL-Skripts für administrative Vorgänge problemlos und zuverlässig verwalten.

Für die Verwaltung horizontal hochskalierter Sharddatenbanken ermöglicht Ihnen das Feature **Auftrag für die elastische Datenbank** (Vorschau) das zuverlässige Ausführen eines Transact-SQL-Skripts (T-SQL) für eine Gruppe von Datenbanken. Dies schließt Folgendes ein:

- Eine benutzerdefinierte Sammlung von Datenbanken (siehe Erläuterung weiter unten)
- Alle Datenbanken in einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md)
- Eine (mit der [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md)erstellte) Shard-Gruppe

Weitere Informationen zum Konzept einer Sharddatenbank finden Sie unter [Sharding a SQL Server Database](https://blog.pythian.com/sharding-sql-server-database/) (Durchführen des Shardings für eine SQL Server-Datenbank).

## <a name="documentation"></a>Dokumentation

- [Installieren von Komponenten der Aufträge für die elastische Datenbank](sql-database-elastic-jobs-service-installation.md)
- [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md)
- [Erstellen und Verwalten von Aufträgen über PowerShell](sql-database-elastic-jobs-powershell.md)
- [Erstellen und Verwalten von horizontal hochskalierten Azure SQL-Datenbanken](sql-database-elastic-jobs-getting-started.md)

![Dienst für elastische Datenbankaufträge][1]

## <a name="why-use-jobs"></a>Gründe für die Verwendung von Aufträgen

### <a name="manage"></a>Verwalten

Führen Sie Schemaänderungen, die Verwaltung von Anmeldeinformationen, Verweisdatenaktualisierungen, Leistungsdatensammlung oder Telemetrieerfassung für Mandanten (Kunden) problemlos durch.

### <a name="reports"></a>Berichte

Aggregation von Daten aus einer Sammlung von Azure SQL-Datenbanken in einer einzelnen Zieltabelle.

### <a name="reduce-overhead"></a>Verringern des Aufwands

Normalerweise müssen Sie unabhängige Verbindungen mit jeder einzelnen Datenbank herstellen, um Transact-SQL-Anweisungen oder andere Verwaltungsaufgaben auszuführen. Ein Auftrag nimmt Ihnen die Anmeldung bei den einzelnen Datenbanken in der Zielgruppe ab. Zudem können Sie Transact-SQL-Skripts zur Ausführung für eine Gruppe von Azure SQL-Datenbanken definieren, verwalten und speichern.

### <a name="accounting"></a>Buchhaltung

Aufträge führen das Skript aus und protokollieren den Ausführungsstatus für die einzelnen Datenbanken. Außerdem werden Vorgänge automatisch wiederholt, wenn Fehler auftreten.

### <a name="flexibility"></a>Flexibilität

Definition benutzerdefinierter Gruppen von Azure SQL-Datenbanken sowie von Zeitplänen für die Auftragsausführung.

> [!NOTE]
> Im Azure-Portal steht nur eine eingeschränkte Anzahl von Funktionen für Pools für elastische SQL Azure-Datenbanken zur Verfügung. Verwenden Sie die PowerShell-APIs, um auf den gesamten aktuell verfügbaren Funktionsumfang zuzugreifen.

## <a name="applications"></a>ANWENDUNGEN

- Führen Sie Verwaltungsaufgaben durch, wie z.B. die Bereitstellung eines neuen Schemas.
- Aktualisieren Sie Referenzdaten, z.B. allgemeine Produktinformationen, die für alle Datenbanken gelten. Sie können auch automatische Updates für jeden Arbeitstag nach Geschäftsschluss planen.
- Neuerstellung von Indizes zum Verbessern der Abfrageleistung. Für die Neuerstellung kann die Ausführung für eine gesamte Sammlung von Datenbanken auf wiederkehrender Basis konfiguriert werden, etwa in Zeiten mit geringer Auslastung.
- Sammlung von Abfrageergebnissen aus einer Menge von Datenbanken in einer zentralen Tabelle auf fortlaufender Grundlage. Leistungsabfragen können fortlaufend ausgeführt werden und für die Auslösung der Ausführung weiterer Aufgaben konfiguriert werden.
- Ausführung von Abfragen zur Datenverarbeitung mit längerer Laufzeit für eine große Anzahl von Datenbanken, z. B. bei der Sammlung von Kundentelemetrie. Die Ergebnisse werden zur weiteren Analyse in einer einzelnen Zieltabelle gesammelt.

## <a name="elastic-database-jobs-end-to-end"></a>Umfassender Überblick über Aufträge für die elastische Datenbank

1. Installieren Sie die Komponenten der **Aufträge für die elastische Datenbank** . Weitere Informationen finden Sie unter [Installieren von elastischen Datenbankaufträgen](sql-database-elastic-jobs-service-installation.md). Wenn die Installation fehlschlägt, lesen Sie die Informationen zum [Deinstallieren](sql-database-elastic-jobs-uninstall.md).
2. Verwenden Sie die PowerShell-APIs, um auf weitere Funktionen zuzugreifen, beispielsweise zum Erstellen von benutzerdefinierten Datenbanksammlungen, zum Hinzufügen von Zeitplänen und/oder dem Erfassen von Ergebnismengen. Verwenden Sie das Portal für die einfache Installation sowie die Erstellung und Überwachung von Aufträgen, die auf die Ausführung für einen **Pool für elastische Datenbanken** beschränkt sind.
3. Erstellen Sie verschlüsselte Anmeldeinformationen für die Auftragsausführung und [fügen Sie den Benutzer (oder die Rolle) jeder Datenbank in der Gruppe hinzu](sql-database-security-overview.md).
4. Erstellen Sie ein idempotentes T-SQL-Skript, das für jede Datenbank in der Gruppe ausgeführt werden kann.
5. Führen Sie die folgenden Schritte aus, um Aufträge im Azure-Portal zu erstellen: [Erstellen und Verwalten von Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-create-and-manage.md).
6. Sie können auch PowerShell-Skripts verwenden: [Erstellen und Verwalten von Aufträgen für die elastische SQL-Datenbank mithilfe von PowerShell (Vorschau)](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Idempotente Skripts

Die Skripts müssen [idempotent](https://en.wikipedia.org/wiki/Idempotence)sein. Einfach ausgedrückt bedeutet „idempotent“, dass die erneute Ausführung eines bereits erfolgreich ausgeführten Skripts zum gleichen Ergebnis führt. Es kann vorkommen, dass ein Skript aufgrund von vorübergehenden Netzwerkproblemen nicht erfolgreich ausgeführt wird. In diesem Fall führt der Auftrag automatisch eine bestimmte Anzahl von Wiederholungsversuchen für das Skript aus. Ein idempotentes Skript führt auch bei zweimaliger erfolgreicher Ausführung zum gleichen Ergebnis.

Vor dem Erstellen eines Objekts empfiehlt es sich, zu überprüfen, ob es bereits vorhanden ist.  

```sql
    IF NOT EXIST (some_object)
    -- Create the object
```

Analog dazu muss auch ein Skript erfolgreich ausgeführt werden können, indem logische Tests durchgeführt werden und angemessen auf die vorgefundenen Bedingungen reagiert wird.

## <a name="failures-and-logs"></a>Fehler und Protokolle

Wenn ein Skript auch nach mehreren Versuchen nicht erfolgreich ausgeführt werden kann, wird der Fehler protokolliert und der Auftrag fortgesetzt. Nach Beendigung eines Auftrags (also nachdem er für alle Datenbanken in der Gruppe ausgeführt wurde) können Sie sich die Liste mit den Fehlversuchen ansehen. Die Protokolle enthalten Details zum Debuggen der fehlerhaften Skripts.

## <a name="group-types-and-creation"></a>Gruppentypen und Erstellung

Es gibt zwei Arten von Gruppen:

1. Shard-Gruppen
2. Benutzerdefinierte Gruppen

Shardgruppen werden mithilfe der [Tools für elastische Datenbanken](sql-database-elastic-scale-introduction.md) erstellt. Wenn Sie eine Shard-Gruppe erstellen, werden der Gruppe automatisch Datenbanken hinzugefügt bzw. daraus entfernt. Beispielsweise ist ein neuer Shard automatisch in der Gruppe enthalten, wenn Sie ihn der Shardzuordnung hinzufügen. Ein Auftrag kann dann für die Gruppe ausgeführt werden.

Benutzerdefinierte Gruppen werden dagegen starr definiert. Sie müssen explizit Datenbanken zu benutzerdefinierten Gruppen hinzufügen oder daraus entfernen. Wenn eine Datenbank aus der Gruppe gelöscht wird, versucht der Auftrag, das Skript für die Datenbank auszuführen, was letztendlich zu einem Fehler führt. Mit dem Azure-Portal erstellte Gruppen sind derzeit benutzerdefinierte Gruppen.

## <a name="components-and-pricing"></a>Komponenten und Preise

Die folgenden Komponenten arbeiten zusammen, um einen Azure-Clouddienst zu erstellen, der eine Ad-hoc-Ausführung von Verwaltungsaufgaben ermöglicht. Die Komponenten werden installiert und automatisch während der Installation in Ihrem Abonnement konfiguriert. Sie können die Dienste am automatisch generierten identischen Namen erkennen. Der Name ist eindeutig und besteht aus dem Präfix "edj", gefolgt von 21 zufällig generierten Zeichen.

- Azure Cloud Service

  Elastische Datenbankaufträge (Vorschauversion) werden als vom Kunden gehosteter Azure-Clouddienst bereitgestellt, um die angeforderten Aufgaben auszuführen. Der Dienst wird im Portal bereitgestellt und im Microsoft Azure-Abonnement gehostet. Der standardmäßig bereitgestellte Dienst wird mit mindestens zwei Workerrollen für Hochverfügbarkeit ausgeführt. Die Standardgröße der einzelnen Workerrollen ("ElasticDatabaseJobWorker") wird in einer A0-Instanz ausgeführt. Die Preise finden Sie unter [Cloud Services Preise](https://azure.microsoft.com/pricing/details/cloud-services/).

- Azure SQL-Datenbank

  Der Dienst verwendet zum Speichern aller Auftragsmetadaten eine Azure SQL-Datenbank, die als **Verwaltungsdatenbank** bezeichnet wird. Die Standarddienstebene ist S0. Informationen zu den Preisen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/).

- Azure-Servicebus

  Eine Azure Service Bus-Instanz wird zum Koordinieren der Arbeit innerhalb von Azure Cloud Service genutzt. Siehe [Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/).

- Azure Storage

  Ein Azure Storage-Konto wird zum Speichern von Diagnoseausgabeprotokollen verwendet, falls ein aufgetretenes Problem weiteres Debuggen erfordert (siehe [Aktivieren der Diagnose in Azure Cloud Services und Virtual Machines](../cloud-services/cloud-services-dotnet-diagnostics.md)). Information zu den Preisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>Funktionsweise von Aufträgen für die elastische Datenbank

1. Einer Azure SQL-Datenbank wird eine **Steuerdatenbank** zugeordnet, die alle Meta- und Statusdaten enthält.
2. Auf die Steuerdatenbank wird durch den **Auftragsdienst** sowohl zum Starten als auch zum Nachverfolgen der auszuführenden Aufträge zugegriffen.
3. Zwei verschiedene Rollen kommunizieren mit der Steuerdatenbank:
   - Controller: Legt fest, welche Aufträge Aufgaben zum Durchführen des angeforderten Auftrags benötigen, und führt Wiederholungsversuche bei Aufträgen mit Fehlern durch Erstellen neuer Auftragsaufgaben aus.
   - Ausführung von Auftragsaufgaben: Führt die Auftragsaufgaben durch.

### <a name="job-task-types"></a>Typen von Auftragsaufgaben

Es gibt mehrere Typen von Auftragsaufgaben, die die Ausführung von Aufträgen abwickeln:

- ShardMapRefresh

  Fragt die Shardzuordnung ab, um alle als Shards verwendeten Datenbanken zu bestimmen
- ScriptSplit

  Teilt das Skript entlang der GO-Anweisungen in Batches auf
- ExpandJob

  Erstellt untergeordnete Aufträge für jede Datenbank aus einem Auftrag, der eine Gruppe von Datenbanken zum Ziel hat
- ScriptExecution

  Führt ein Skript für eine bestimmte Datenbank mithilfe definierter Anmeldeinformationen aus
- Dacpac

  Wendet ein DACPAC auf eine bestimmte Datenbank mithilfe bestimmter Anmeldeinformationen an

## <a name="end-to-end-job-execution-work-flow"></a>End-to-End-Arbeitsablauf bei der Auftragsausführung

1. Ein Auftrag wird entweder mithilfe des Portals oder der PowerShell-API in die **Verwaltungsdatenbank** eingefügt. Der Auftrag fordert die Ausführung eines Transact-SQL-Skripts für eine Gruppe von Datenbanken mithilfe bestimmter Anmeldeinformationen an.
2. Der Controller identifiziert den neuen Auftrag. Auftragsaufgaben werden erstellt und ausgeführt, um das Skript aufzuteilen und die Datenbanken der Gruppe zu aktualisieren. Abschließend wird ein neuer Auftrag erstellt und ausgeführt, um den Auftrag zu erweitern und neue untergeordnete Aufträge zu erstellen, von denen für jeden die Ausführung des Transact-SQL-Skripts für eine bestimmte Datenbank der Gruppe festgelegt ist.
3. Der Controller identifiziert die erstellten untergeordneten Aufträge. Für jeden Auftrag erstellt der Controller eine Auftragsaufgabe und löst sie aus, um das Skript für eine Datenbank auszuführen.
4. Nach dem Abschluss aller Auftragsaufgaben aktualisiert der Controller die Aufträge mit dem Status „abgeschlossen“.
   Während der Auftragsausführung kann die PowerShell-API verwendet werden, um den aktuellen Status der Auftragsausführung anzuzeigen. Alle von den PowerShell-APIs zurückgegebenen Uhrzeiten werden in UTC angegeben. Gegebenenfalls kann eine Abbruchanforderung eingeleitet werden, um einen Auftrag zu beenden.

## <a name="next-steps"></a>Nächste Schritte

[Installieren Sie die Komponenten](sql-database-elastic-jobs-service-installation.md). Danach [erstellen Sie ein Protokoll für jede Datenbank in der Gruppe der Datenbanken und fügen es hinzu](sql-database-manage-logins.md). Weitere Informationen zum Erstellen und Verwalten von Aufträgen finden Sie unter [Erstellen und Verwalten von elastischen Datenbankaufträgen](sql-database-elastic-jobs-create-and-manage.md). Informationen hierzu finden Sie auch unter [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->
