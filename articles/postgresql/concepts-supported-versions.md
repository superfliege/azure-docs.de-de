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
ms.date: 11/12/2018
ms.openlocfilehash: a03ead5e577b261b99e635addf6f9a98d8f3cadd
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619691"
---
# <a name="supported-postgresql-database-versions"></a>Unterstützte PostgreSQL-Datenbankversionen
Microsoft strebt im Azure Database for PostgreSQL-Dienst die Unterstützung von n-2 Versionen der PostgreSQL-Engine an. Das bedeutet, dass die aktuelle Hauptversion (n) und die beiden vorherigen Hauptversionen (-2) unterstützt werden.

Azure-Datenbank für PostgreSQL unterstützt derzeit die folgenden Versionen:

## <a name="postgresql-version-105"></a>PostgreSQL-Version 10.5
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-5.html).

## <a name="postgresql-version-9610"></a>PostgreSQL-Version 9.6.10
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-10.html).

## <a name="postgresql-version-9514"></a>PostgreSQL-Version 9.5.14
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-14.html).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Patches für Nebenversionen werden von Azure Database for PostgreSQL automatisch verwaltet. Derzeit werden Upgrades von Hauptversionen nicht unterstützt. Beispielsweise wird das Upgrade von PostgreSQL 9.5 auf PostgreSQL 9.6 nicht unterstützt. Wenn Sie auf die nächste Hauptversion upgraden möchten, erstellen Sie eine Datenbank, und führen Sie für diese eine [Sicherung und Wiederherstellung](./howto-migrate-using-dump-and-restore.md) auf einem Server aus, der mit der neuen Engine-Version erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Unterstützung der verschiedenen PostgreSQL-Erweiterungen finden Sie unter [PostgreSQL-Erweiterungen](concepts-extensions.md).
