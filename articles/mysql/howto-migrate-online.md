---
title: Migration zu Azure Database for MySQL mit minimaler Ausfallzeit | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt die Durchführung einer Migration einer MySQL-Datenbank zu Azure Database for MySQL mit minimaler Ausfallzeit und die Einrichtung der anfänglichen Last und der kontinuierlichen Datensynchronisierung von der Quelldatenbank in die Zieldatenbank mithilfe von Attunity Replicate for Microsoft Migrationen."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 429f5c9f939a802184a6513a63fb9115abf4b235
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migration zu Azure Database for MySQL mit minimaler Ausfallzeit
Sie können Ihre vorhandene MySQL-Datenbank mithilfe von Attunity Replicate for Microsoft Migrations zu Azure Database for MySQL migrieren. Dies ist ein gemeinsames Angebot von Attunity und Microsoft, das Microsoft-Kunden ohne Zusatzkosten zusammen mit dem Azure Database Migration Service zur Verfügung gestellt wird. Attunity Replicate for Microsoft Migrations sorgt auch für eine minimale Ausfallzeit während der Datenbankmigration, und die Quelldatenbank wird während des gesamten Migrationsvorgangs fortgesetzt.

Attunity Replicate ist ein Datentreplikationstool, das die Datensynchronisierung zwischen einer Vielzahl von Quellen und Zielen ermöglicht und das Schemaerstellungsskript und die Daten, die jeder Datenbanktabelle zugeordnet sind, weitergibt. Attunity Replicate gibt keine anderen Artefakte (wie z.B. SP, Trigger, Funktionen usw.) weiter, und das Tool konvertiert z.B. PL-/SQL-Code in solchen Artefakten nicht in T-SQL.

> [!NOTE]
> Auch wenn Attunity Replicate eine Vielzahl an Migrationsszenarien unterstützt, liegt der Fokus von Attunity Replicate for Microsoft Migrations auf der Unterstützung einer bestimmten Teilmenge von Quell-/Zielelementpaaren.

Der Prozess für die Durchführung einer Migration mit minimaler Ausfallzeit umfasst Folgendes:

1. **Migrieren des MySQL-Quellschemas** zu einer Azure Database for MySQL-Datenbank mithilfe von [MySQL Workbench](https://www.mysql.com/products/workbench/)

2. **Einrichten der anfänglichen Last und der kontinuierlichen Datensynchronisierung von der Quelldatenbank in die Zieldatenbank** mithilfe von Attunity Replicate for Microsoft Migrations. Auf diese Weise wird die Zeit minimiert, die die Quelldatenbank als schreibgeschützt festgelegt werden muss, während Sie die Überführung Ihrer Anwendungen in die MySQL-Zieldatenbank in Azure vorbereiten.

Weitere Informationen zum Attunity Replicate for Microsoft-Migrations-Angebot finden Sie in den folgenden Ressourcen:
 - [Webseite](https://aka.ms/attunity-replicate) zu Attunity Replicate for Microsoft Migrations
 - [Download](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) von Attunity Replicate for Microsoft Migrations
 - [Community](https://microsoft.attunity.com/) von Attunity Replicate mit einem Schnellstarthandbuch, Tutorials und Support
 - Eine ausführliche Anleitung zur Verwendung von Attunity zum Migrieren von MySQL zu Azure Database for MySQL finden Sie im [Datenbank-Migrationshandbuch](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).