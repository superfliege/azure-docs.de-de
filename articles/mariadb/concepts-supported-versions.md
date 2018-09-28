---
title: Unterstützte Versionen in Azure Database for MariaDB
description: Beschreibung der unterstützten Versionen in Azure Database for MariaDB
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: aa83b9955839aaa03aa2ebf46c9e464dc75c8d3a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977501"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Unterstützte Serverversionen von Azure Database for MariaDB
Azure Database for MariaDB wurde mithilfe der InnoDB-Engine auf der Grundlage des [MariaDB-Servers](https://downloads.mariadb.org/) entwickelt, einem Open Source-Produkt. Azure Database for MariaDB unterstützt derzeit die folgende Version:

## <a name="mariadb-version-10217"></a>MariaDB Version 10.2.17
Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MariaDB 10.2.17 finden Sie in der [Dokumentation zu MariaDB](https://downloads.mariadb.org/mariadb/10.2.17/).

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MariaDB an, nicht die tatsächliche Version, die auf Ihrer MariaDB-Serverinstanz ausgeführt wird. Wenn Sie die Version Ihrer MariaDB-Serverinstanz ermitteln möchten, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein.

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Updates zu Nebenversionen wird vom Dienst automatisch verwaltet.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrem **Diensttarif** finden Sie unter [Diensttarife](./concepts-pricing-tiers.md).