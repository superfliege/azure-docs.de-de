---
title: Migration nach Azure Database for MySQL mit minimaler Ausfallzeit
description: Dieser Artikel beschreibt die Durchführung einer Migration einer MySQL-Datenbank nach Azure Database for MySQL mit minimaler Ausfallzeit und die Einrichtung der anfänglichen Last und der kontinuierlichen Datensynchronisierung von der Quelldatenbank zur Zieldatenbank mithilfe von Attunity Replicate for Microsoft Migrations.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 99add55188615debdc96b6cfc8b21e34552fd9d4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267253"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migration nach Azure Database for MySQL mit minimaler Ausfallzeit
Sie können Ihre vorhandene MySQL-Datenbank mithilfe von Attunity Replicate for Microsoft Migrations nach Azure Database for MySQL migrieren. Attunity Replicate ist ein gemeinsames Angebot von Attunity und Microsoft. Es wird Microsoft-Kunden in Verbindung mit Azure Database Migration Service ohne Zusatzkosten zur Verfügung gestellt. 

Attunity Replicate trägt zur Minimierung von Ausfallzeiten bei Datenbankmigrationen bei und sorgt dafür, dass die Quelldatenbank während des Prozesses weiter verwendet werden kann.

Das Attunity Replicate-Datenreplikationstool ermöglicht die Datensynchronisierung zwischen verschiedenen Quellen und Zielen. Es verteilt das Schemaerstellungsskript und die Daten, die den einzelnen Datenbanktabellen zugeordnet sind. Attunity Replicate verteilt keine anderen Artefakte (wie etwa SP, Trigger oder Funktionen) und konvertiert auch beispielsweise nicht den in solchen Artefakten gehosteten PL-/SQL-Code in T-SQL.

> [!NOTE]
> Attunity Replicate unterstützt zwar ein breites Spektrum von Migrationsszenarien, im Mittelpunkt steht jedoch die Unterstützung einer spezifischen Teilmenge von Quell-/Zielpaaren.

Der Prozess für die Durchführung einer Migration mit minimaler Ausfallzeit umfasst Folgendes:

* **Migrieren des MySQL-Quellschemas** zu einem verwalteten Azure Database for MySQL-Datenbankdienst mithilfe von [MySQL Workbench](https://www.mysql.com/products/workbench/)

* **Einrichten des ersten Ladevorgangs und die fortlaufende Synchronisierung von der Quelldatenbank zur Zieldatenbank** mithilfe von Attunity Replicate for Microsoft Migrations. Auf diese Weise wird die Zeit minimiert, die die Quelldatenbank als schreibgeschützt festgelegt werden muss, während Sie die Überführung Ihrer Anwendungen in die MySQL-Zieldatenbank in Azure vorbereiten.

Weitere Informationen zum Attunity Replicate for Microsoft Migrations-Angebot finden Sie in den folgenden Ressourcen:
 - Rufen Sie die Webseite zu [Attunity Replicate for Microsoft Migrations](https://aka.ms/attunity-replicate) auf.
 - Laden Sie [Attunity Replicate for Microsoft Migrations](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) herunter.
 - Besuchen Sie die [Community von Attunity Replicate](https://aka.ms/attunity-community), um Schnellstarthandbücher, Tutorials und Support zu erhalten.
 - Eine ausführliche Anleitung zur Verwendung von Attunity Replicate zum Migrieren von MySQL-Datenbanken zu Azure Database for MySQL finden Sie im [Handbuch zu Datenbankmigrationen](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).