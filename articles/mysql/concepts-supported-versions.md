---
title: Unterstützte Versionen in Azure Database for MySQL
description: Beschreibung der unterstützten Versionen in Azure-Datenbank für MySQL
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: f2a9348e267ad6a5929fda64cc08dcd837243e71
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093339"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Unterstützte Azure-Datenbank für MySQL-Serverversionen
Azure Database for MySQL wurde basierend auf [MySQL Community Edition](https://www.mysql.com/products/community/) mithilfe der InnoDB-Engine entwickelt. Azure Database for MySQL unterstützt derzeit die folgenden Versionen:

## <a name="mysql-version-5639"></a>MySQL Version 5.6.39
Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.6.39 finden Sie in der [Dokumentation zu MySQL](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html).

## <a name="mysql-version-5721"></a>MySQL Version 5.7.21
Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.7.21 finden Sie in der [Dokumentation zu MySQL](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html).

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MySQL an, nicht die tatsächliche Version, die auf Ihrer MySQL-Serverinstanz ausgeführt wird. Um die Version Ihrer MySQL-Serverinstanz zu ermitteln, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein.

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Versionsupdates zur Fehlerbehebung wird vom Dienst automatisch verwaltet. Derzeit werden Upgrades von Nebenversionen nicht unterstützt. Beispielsweise wird das Upgrade von MySQL 5.6 auf MySQL 5.7 nicht unterstützt. Wenn Sie auf die nächste Nebenversion upgraden möchten, führen Sie eine [Sicherung und Wiederherstellung](./concepts-migrate-dump-restore.md) auf einem Server aus, der mit der neuen Engine-Version erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrem **Diensttarif** finden Sie unter [Diensttarife](./concepts-pricing-tiers.md).
