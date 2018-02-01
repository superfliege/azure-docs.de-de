---
title: Migration nach Azure Database for MySQL mit minimaler Ausfallzeit | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt die Durchführung einer Migration einer MySQL-Datenbank nach Azure Database for MySQL mit minimaler Ausfallzeit und die Einrichtung der anfänglichen Last und der kontinuierlichen Datensynchronisierung von der Quelldatenbank zur Zieldatenbank mithilfe von Attunity Replicate for Microsoft Migrations."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: d23628fd8446f6e7e0e5ed14b98da13c09b2d592
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
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