---
title: Unterstützte Versionen in Azure Database for PostgreSQL
description: Beschreibung der unterstützten Versionen in Azure-Datenbank für PostgreSQL
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/07/2018
ms.openlocfilehash: f337679279f4a3eb6d01808f15a59fee4c8f3cd1
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630584"
---
# <a name="supported-postgresql-database-versions"></a>Unterstützte PostgreSQL-Datenbankversionen
Ziel von Microsoft ist die Unterstützung von n-2 Versionen der PostgreSQL-Engine im Azure Database for PostgreSQL-Dienst. Das bedeutet, dass die derzeit veröffentlichte Hauptversion (n) und die zwei vorherigen Hauptversionen (-2) unterstützt werden.

Azure-Datenbank für PostgreSQL unterstützt derzeit die folgenden Versionen:

## <a name="postgresql-version-104"></a>PostgreSQL-Version 10.4
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-4.html).

## <a name="postgresql-version-969"></a>PostgreSQL-Version 9.6.9
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-9.html).

## <a name="postgresql-version-9513"></a>PostgreSQL-Version 9.5.13
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-13.html).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Updates zu Nebenversionen wird von Azure-Datenbank für PostgreSQL automatisch verwaltet. Derzeit werden Upgrades von Hauptversionen nicht unterstützt. Beispielsweise wird das Upgrade von PostgreSQL 9.5 auf PostgreSQL 9.6 nicht unterstützt. Wenn Sie auf die nächste Hauptversion upgraden möchten, [stellen Sie ein Speicherabbild auf einem Server wieder her](./howto-migrate-using-dump-and-restore.md), der mit der neuen Engineversion erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Unterstützung der verschiedenen PostgreSQL-Erweiterungen finden Sie unter [PostgreSQL-Erweiterungen](concepts-extensions.md).
