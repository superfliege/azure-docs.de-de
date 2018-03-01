---
title: Migration zu Azure Database for PostgreSQL mit minimaler Ausfallzeit
description: "In diesem Artikel wird beschrieben, wie Sie eine Migration mit minimaler Ausfallzeit durchführen. Hierzu extrahieren Sie zunächst eine PostgreSQL-Datenbank in eine Dumpdatei und stellen die PostgreSQL-Datenbank aus einer Archivdatei wieder her, die über „pg_dump“ in Azure Database for PostgreSQL erstellt wurde. Außerdem richten Sie unter Verwendung von Attunity Replicate for Microsoft Migrations den ersten Ladevorgang und die fortlaufende Synchronisierung von der Quelldatenbank zur Zieldatenbank ein."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 48cf460405ae3985553f9bff29f4fd7abb008196
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Migration zu Azure Database for PostgreSQL mit minimaler Ausfallzeit
Sie können Ihre vorhandene PostgreSQL-Datenbank mithilfe von Attunity Replicate for Microsoft Migrations zu Azure Database for PostgreSQL migrieren. Attunity Replicate ist ein gemeinsames Angebot von Attunity und Microsoft. Es wird Microsoft-Kunden in Verbindung mit Azure Database Migration Service ohne Zusatzkosten zur Verfügung gestellt. 

Attunity Replicate trägt zur Minimierung von Ausfallzeiten bei Datenbankmigrationen bei und sorgt dafür, dass die Quelldatenbank während des Prozesses weiter verwendet werden kann.

Das Attunity Replicate-Datenreplikationstool ermöglicht die Datensynchronisierung zwischen verschiedenen Quellen und Zielen. Es verteilt das Schemaerstellungsskript und die Daten, die den einzelnen Datenbanktabellen zugeordnet sind. Attunity Replicate verteilt keine anderen Artefakte (wie etwa SP, Trigger oder Funktionen) und konvertiert auch beispielsweise nicht den in solchen Artefakten gehosteten PL-/SQL-Code in T-SQL.

> [!NOTE]
> Attunity Replicate unterstützt zwar ein breites Spektrum von Migrationsszenarien, im Mittelpunkt steht jedoch die Unterstützung einer spezifischen Teilmenge von Quell-/Zielpaaren.

Der Prozess für die Durchführung einer Migration mit minimaler Ausfallzeit umfasst Folgendes:

* **Migrieren des PostgreSQL-Quellschemas** zu einer Azure Database for PostgreSQL-Datenbank unter Verwendung des Befehls [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) mit dem Parameter „-n“ und der anschließenden Verwendung des Befehls [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html).

* **Einrichten des ersten Ladevorgangs und die fortlaufende Synchronisierung von der Quelldatenbank zur Zieldatenbank** mithilfe von Attunity Replicate for Microsoft Migrations. Auf diese Weise wird der Zeitraum minimiert, für die die Quelldatenbank als schreibgeschützt festgelegt werden muss, während Sie die Überführung Ihrer Anwendungen in die PostgreSQL-Zieldatenbank in Azure vorbereiten.

Weitere Informationen zum Attunity Replicate for Microsoft Migrations-Angebot finden Sie in den folgenden Ressourcen:
 - Rufen Sie die Webseite zu [Attunity Replicate for Microsoft Migrations](https://aka.ms/attunity-replicate) auf.
 - Laden Sie [Attunity Replicate for Microsoft Migrations](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) herunter.
 - Besuchen Sie die [Community von Attunity Replicate](https://aka.ms/attunity-community/), um eine Schnellstartanleitung, Tutorials und Support zu erhalten.
 - Eine ausführliche Anleitung zur Verwendung von Attunity Replicate zum Migrieren von PostgreSQL zu Azure Database for PostgreSQL finden Sie im [Leitfaden zur Datenbankmigration](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).