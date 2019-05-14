---
title: Azure Resource Manager-Vorlagen für Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: c907de019b64a6a9d03206514a1147fd43c78106
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080576"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Resource Manager-Vorlagen für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Azure Cosmos DB:

|**API-Typ** | **Link zum Beispiel**| **Beschreibung** |
|---|---| ---|
|Core-API (SQL)| [Erstellen eines Azure Cosmos DB-Kontos (Multimaster)](manage-sql-with-resource-manager.md) | Diese Vorlage erstellt ein SQL-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Container, die den Durchsatz auf Datenbankebene gemeinsam nutzen. |
| MongoDB-API | [Erstellen eines Azure Cosmos DB-Kontos (Multimaster)](manage-mongodb-with-resource-manager.md) | Diese Vorlage erstellt mithilfe der Azure Cosmos DB-API für MongoDB ein Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Container, die den Durchsatz auf Datenbankebene gemeinsam nutzen. |
| Cassandra-API | [Erstellen eines Azure Cosmos DB-Kontos (Multimaster)](manage-cassandra-with-resource-manager.md) | Diese Vorlage erstellt ein Cassandra-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Tabellen, die den Durchsatz auf Schlüsselbereichsebene gemeinsam nutzen. |
| Gremlin-API| [Erstellen eines Azure Cosmos DB-Kontos (Multimaster)](manage-gremlin-with-resource-manager.md) | Diese Vorlage erstellt ein Gremlin-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Graphen, die den Durchsatz auf Datenbankebene gemeinsam nutzen. |
| Tabelle-API | [Erstellen eines Azure Cosmos DB-Kontos (Multimaster)](manage-table-with-resource-manager.md) | Diese Vorlage erstellt ein Tabellen-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält eine einzelne Tabelle. |

> [!TIP]
> Um bei Verwendung der Tabellen-API die gemeinsame Nutzung des Durchsatzes zu aktivieren, aktivieren Sie im Azure-Portal den Durchsatz auf Kontoebene.

Auf der Seite [ARM-Referenz für Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) finden Sie die Referenzdokumentation.