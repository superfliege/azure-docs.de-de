---
title: "Konfigurieren von Serverparametern in Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie MySQL-Serverparameter in Azure Database for MySQL mithilfe des Azure-Portals konfigurieren können."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/10/2017
ms.openlocfilehash: f3b32c1f6b33bc60b50f1496414a300db468dc92
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Konfigurieren von Serverparametern in Azure Database for MySQL mit dem Azure-Portal

Azure Database für MySQL unterstützt das Konfigurieren einiger Serverparameter. In diesem Thema wird beschrieben, wie diese Parameter mithilfe des Azure-Portals konfiguriert werden. Nicht alle Serverparameter können angepasst werden. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navigieren Sie im Azure-Portal zu „Serverparameter“.
1. Melden Sie sich beim Azure-Portal an, und suchen Sie dann nach Ihrem Azure Database for MySQL-Server.
2. Klicken Sie im Abschnitt **EINSTELLUNGEN** auf **Serverparameter**, um die Seite „Serverparameter“ für Azure Database for MySQL zu öffnen.
3. Suchen Sie die Einstellungen, die Sie anpassen möchten. Überprüfen Sie die Spalte **Beschreibung**, um den Zweck und die zulässigen Werte zu verstehen. 
4. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

![Blatt „Serverparameter“ im Azure-Portal](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Liste der konfigurierbaren Serverparameter

Die Liste der unterstützten Serverparameter wächst ständig. Verwenden Sie die Registerkarte mit den Serverparametern im Azure-Portal, um die Definition abzurufen und Serverparameter anhand Ihrer Anwendungsanforderungen zu konfigurieren. 

## <a name="nonconfigurable-server-parameters"></a>Nicht konfigurierbare Serverparameter
„InnoDB-Pufferpool“ und „Max. Anzahl von Verbindungen“ können nicht konfiguriert werden und sind an Ihren [Tarif](concepts-service-tiers.md) gebunden. 

| **Preisstufe** | **InnoDB-Pufferpool (MB)** | **Max. Anzahl von Verbindungen** |
| :------------------------ | :-------- | :----------- |
| Basic 50 | 1024 | 50 | 
| Basic 100  | 2.560 | 100 | 
| Standard 100 | 2.560 | 200 | 
| Standard 200 | 5120 | 400 | 
| Standard 400 | 10.240 | 800 | 
| Standard 800 | 20.480 | 1600 |

Diese zusätzlichen Serverparameter sind im System nicht konfigurierbar. <br>
 innodb_file_per_table in Basic tier: OFF<br>
 innodb_flush_log_at_trx_commit=1<br>
 sync_binlog=1<br>
 innodb_log_file_size=512MB<br>
 
Weitere Serverparameter, die hier nicht aufgeführt sind, werden für die Versionen [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) und [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html) auf ihre vordefinierten MySQL-Standardwerte festgelegt.

## <a name="next-steps"></a>Nächste Schritte
- [Datenverbindungsbibliotheken für Azure Database for MySQL](concepts-connection-libraries.md)
