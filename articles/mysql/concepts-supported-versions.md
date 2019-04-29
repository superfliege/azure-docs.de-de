---
title: Unterstützte Versionen in Azure Database for MySQL
description: Beschreibung der unterstützten Versionen in Azure-Datenbank für MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/07/2019
ms.openlocfilehash: ecd6466d8d7a7e4497d076ced0c9f2375d5dfb7f
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106035"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Unterstützte Azure-Datenbank für MySQL-Serverversionen
Azure Database for MySQL wurde basierend auf [MySQL Community Edition](https://www.mysql.com/products/community/) mithilfe der InnoDB-Engine entwickelt.

MySQL verwendet das Benennungsschema „X.Y.Z“. X ist die Hauptversion, Y die Nebenversion und Z die Version zur Fehlerbehebung. Weitere Informationen zum Schema finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

Azure Database for MySQL unterstützt derzeit die folgenden Versionen:

## <a name="mysql-version-56"></a>MySQL-Version 5.6

Version zur Fehlerbehebung: 5.6.39

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.6.39 finden Sie in den [Versionshinweisen](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) für MySQL.

## <a name="mysql-version-57"></a>MySQL-Version 5.7

Version zur Fehlerbehebung: 5.7.21

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.7.21 finden Sie in den [Versionshinweisen](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) für MySQL.

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MySQL an, nicht die tatsächliche Version, die auf Ihrer MySQL-Serverinstanz ausgeführt wird. Um die Version Ihrer MySQL-Serverinstanz zu ermitteln, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein.

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Versionsupdates zur Fehlerbehebung wird vom Dienst automatisch verwaltet. Beispiel: 5.7.20 bis 5.7.21.  

Zurzeit werden Upgrades von Neben- und Hauptversionen nicht unterstützt. Ein Upgrade von MySQL 5.6 auf MySQL 5.7 wird beispielsweise nicht unterstützt. Wenn Sie von 5.6 auf 5.7 aktualisieren möchten, führen Sie eine [Sicherung und Wiederherstellung](./concepts-migrate-dump-restore.md) auf einem Server aus, der mit der neuen Modulversion erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrer **Dienstebene** finden Sie unter [Dienstebenen](./concepts-pricing-tiers.md).
