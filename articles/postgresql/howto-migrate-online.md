---
title: Migration zu Azure Database for PostgreSQL mit minimaler Ausfallzeit | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie eine Migration mit minimaler Ausfallzeit durchführen. Hierzu extrahieren Sie zunächst eine PostgreSQL-Datenbank in eine Dumpdatei und stellen die PostgreSQL-Datenbank aus einer Archivdatei wieder her, die über „pg_dump“ in Azure Database for PostgreSQL erstellt wurde. Außerdem richten Sie unter Verwendung von Attunity Replicate for Microsoft Migrations den ersten Ladevorgang und die fortlaufende Synchronisierung von der Quelldatenbank zur Zieldatenbank ein."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 8c98c58042e7f1d1726eaad6ee03d1531b6c910e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Migration zu Azure Database for PostgreSQL mit minimaler Ausfallzeit
Sie können Ihre vorhandene PostgreSQL-Datenbank mithilfe von Attunity Replicate for Microsoft Migrations zu Azure Database for PostgreSQL migrieren. Dies ist ein gemeinsames Angebot von Attunity und Microsoft, das Microsoft-Kunden ohne Zusatzkosten zusammen mit dem Azure Database Migration Service zur Verfügung gestellt wird. Attunity Replicate for Microsoft Migrations sorgt auch für eine minimale Ausfallzeit während der Datenbankmigration, und die Quelldatenbank wird während des gesamten Migrationsvorgangs fortgesetzt.

Attunity Replicate ist ein Datentreplikationstool, das die Datensynchronisierung zwischen einer Vielzahl von Quellen und Zielen ermöglicht und das Schemaerstellungsskript und die Daten, die jeder Datenbanktabelle zugeordnet sind, weitergibt. Attunity Replicate gibt keine anderen Artefakte (wie z.B. SP, Trigger, Funktionen usw.) weiter, und das Tool konvertiert z.B. PL-/SQL-Code in solchen Artefakte nicht in T-SQL.

> [!NOTE]
> Auch wenn Attunity Replicate eine Vielzahl an Migrationsszenarien unterstützt, liegt der Fokus von Attunity Replicate for Microsoft Migrations auf der Unterstützung einer bestimmten Teilmenge von Quell-/Zielelementpaaren.

Der Prozess für die Durchführung einer Migration mit minimaler Ausfallzeit umfasst Folgendes:

1. **Migrieren des PostgreSQL-Quellschemas** zu einer Azure Database for PostgreSQL-Datenbank unter Verwendung des Befehls [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) mit dem Parameter „-n“ und der anschließenden Verwendung des Befehls [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html).

2. **Einrichten des ersten Ladevorgangs und die fortlaufende Synchronisierung von der Quelldatenbank zur Zieldatenbank** mithilfe von Attunity Replicate for Microsoft Migrations. Auf diese Weise wird der Zeitraum minimiert, für die die Quelldatenbank als schreibgeschützt festgelegt werden muss, während Sie die Überführung Ihrer Anwendungen in die PostgreSQL-Zieldatenbank in Azure vorbereiten.

Weitere Informationen zum Attunity Replicate for Microsoft Migrations-Angebot finden Sie in den folgenden Ressourcen:
 - [Webseite](https://aka.ms/attunity-replicate) zu Attunity Replicate for Microsoft Migrations
 - [Download](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) von Attunity Replicate for Microsoft Migrations
 - [Community](https://microsoft.attunity.com/) von Attunity Replicate mit einem Schnellstarthandbuch, Tutorials und Support
 - Eine Schrittanleitung zur Verwendung von Attunity zum Migrieren von PostgreSQL zu Azure Database for PostgreSQL finden Sie im [Leitfaden zur Datenbankmigration](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).