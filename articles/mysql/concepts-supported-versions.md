---
title: Unterstützte Versionen in Azure Database for MySQL
description: Beschreibung der unterstützten Versionen in Azure-Datenbank für MySQL
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/22/2018
ms.openlocfilehash: cfebdbe7485f0ffaa15828803d72c2a3f97c118d
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Unterstützte Azure-Datenbank für MySQL-Serverversionen
Azure Database for MySQL wurde basierend auf [MySQL Community Edition](https://www.mysql.com/products/community/) mithilfe der InnoDB-Engine entwickelt.  Azure Database for MySQL unterstützt derzeit die folgenden Versionen:

## <a name="mysql-version-5638"></a>MySQL Version 5.6.38
Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.6.38 finden Sie in der [Dokumentation zu MySQL](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-38.html).

## <a name="mysql-version-5720"></a>MySQL Version 5.7.20
Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.7.20 finden Sie in der [Dokumentation zu MySQL](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-20.htmll).

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MySQL an, nicht die tatsächliche Version, die auf Ihrer MySQL-Serverinstanz ausgeführt wird. Um die Version Ihrer MySQL-Serverinstanz zu ermitteln, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein. 

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Updates zu Nebenversionen wird vom Dienst automatisch verwaltet. Upgrades von Hauptversionen werden nicht unterstützt (ausgenommen Upgrade von MySQL 5.6 auf MySQL 5.7).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrem **Diensttarif** finden Sie unter [Diensttarife](./concepts-pricing-tiers.md).
