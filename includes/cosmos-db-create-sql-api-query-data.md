---
title: Includedatei
description: Includedatei
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: e6e70da1f939547cdb589ae7bcb6ed1f6148f22e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733764"
---
Sie können jetzt zum Abrufen und Filtern Ihrer Daten Abfragen im Daten-Explorer verwenden.

1. Beachten Sie, dass die Abfrage standardmäßig auf `SELECT * FROM c` festgelegt ist. Dieser Standardabfrage ruft alle Dokumente in der Sammlung ab und zeigt sie an. 

    ![Die Standardabfrage im Daten-Explorer ist „SELECT * FROM c“.](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)

2. Bleiben Sie auf der Registerkarte **Dokumente**, und ändern Sie die Abfrage, indem Sie auf die Schaltfläche **Filter bearbeiten** klicken, `ORDER BY c._ts DESC` dem Abfrageprädikatfeld hinzufügen und anschließend auf **Filter anwenden** klicken.

    ![Ändern Sie die Standardabfrage, indem Sie ORDER BY c._ts DESC hinzufügen und auf „Filter anwenden“ klicken.](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

Mithilfe dieser geänderten Abfrage werden die Dokumente auf Basis ihrer Zeitstempel in absteigender Reihenfolge sortiert, damit Ihr zweites Dokument zuerst aufgeführt wird. Wenn Sie mit der SQL-Syntax vertraut sind, können Sie eine der unterstützten [SQL-Abfragen](../articles/cosmos-db/sql-api-sql-query.md) in dieses Feld eingeben. 

Damit ist unsere Arbeit im Daten-Explorer abgeschlossen. Bevor wir mit der Arbeit mit Code fortfahren, beachten Sie, dass der Daten-Explorer auch zum Erstellen von gespeicherten Prozeduren, UDFs und Triggern verwendet werden kann, um sowohl serverseitige Geschäftslogik als auch einen Skalierungsdurchsatz auszuführen. Der Daten-Explorer stellt den gesamten integrierten programmgesteuerten Datenzugriff in den APIs zur Verfügung, bietet jedoch einfachen Zugriff auf Ihre Daten im Azure-Portal.