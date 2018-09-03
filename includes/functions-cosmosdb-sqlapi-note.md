---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: bb915c18965a55c2b1fc7fe3cf1fa923d7f43fd6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43052518"
---
Azure Cosmos DB-Bindungen werden nur in Kombination mit der SQL-API unterstützt. Für alle anderen Azure Cosmos DB-APIs (einschließlich [MongoDB-API](../articles/cosmos-db/mongodb-introduction.md), [Cassandra-API](../articles/cosmos-db/cassandra-introduction.md), [Gremlin-API](../articles/cosmos-db/graph-introduction.md) und [Table-API](../articles/cosmos-db/table-introduction.md)) müssen Sie für den Datenbankzugriff in Ihrer Funktion den statischen Client für Ihre API verwenden.
