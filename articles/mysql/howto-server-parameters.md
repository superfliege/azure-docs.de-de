---
title: Konfigurieren von Serverparametern in Azure Database for MySQL
description: In diesem Artikel wird beschrieben, wie Sie MySQL-Serverparameter in Azure Database for MySQL mithilfe des Azure-Portals konfigurieren können.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6865663bebc84df288f4c7e2564ddb4870667c6f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Konfigurieren von Serverparametern in Azure Database for MySQL mit dem Azure-Portal

Azure Database für MySQL unterstützt das Konfigurieren einiger Serverparameter. In diesem Artikel wird beschrieben, wie diese Parameter mithilfe des Azure-Portals konfiguriert werden. Nicht alle Serverparameter können angepasst werden. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navigieren Sie im Azure-Portal zu „Serverparameter“.
1. Melden Sie sich beim Azure-Portal an, und suchen Sie dann nach Ihrem Azure Database for MySQL-Server.
2. Klicken Sie im Abschnitt **EINSTELLUNGEN** auf **Serverparameter**, um die Seite „Serverparameter“ für Azure Database for MySQL zu öffnen.
![Seite „Serverparameter“ im Azure-Portal](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Suchen Sie die Einstellungen, die Sie anpassen möchten. Überprüfen Sie die Spalte **Beschreibung**, um den Zweck und die zulässigen Werte zu verstehen. 
![Dropdownliste für Enumerierung](./media/howto-server-parameters/3-toggle_parameter.png)
4. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
![Änderungen speichern oder verwerfen](./media/howto-server-parameters/4-save_parameters.png)
5. Wenn Sie neue Werte für die Parameter gespeichert haben, können Sie jederzeit alles zurück auf die Standardwerte setzen, indem Sie die Option **Alle auf Standard zurücksetzen** wählen.
![Alle auf Standard zurücksetzen](./media/howto-server-parameters/5-reset_parameters.png)


## <a name="list-of-configurable-server-parameters"></a>Liste der konfigurierbaren Serverparameter

Die Liste der unterstützten Serverparameter wächst ständig. Verwenden Sie die Registerkarte mit den Serverparametern im Azure-Portal, um die Definition abzurufen und Serverparameter anhand Ihrer Anwendungsanforderungen zu konfigurieren. 

## <a name="nonconfigurable-server-parameters"></a>Nicht konfigurierbare Serverparameter
„InnoDB-Pufferpool“ und „Max. Anzahl von Verbindungen“ können nicht konfiguriert werden und sind an Ihren [Tarif](concepts-service-tiers.md) gebunden. 

|**Tarif**| **Computegeneration**|**vCore(s)**|**InnoDB-Pufferpool (MB)**| **Max. Anzahl von Verbindungen**|
|---|---|---|---|--|
|Basic| Gen 4| 1| 1024| 50|
|Basic| Gen 4| 2| 2.560| 100|
|Basic| Gen 5| 1| 1024| 50|
|Basic| Gen 5| 2| 2.560| 100|
|Allgemeiner Zweck| Gen 4| 2| 3.584| 300|
|Allgemeiner Zweck| Gen 4| 4| 7.680| 625|
|Allgemeiner Zweck| Gen 4| 8| 15360| 1250|
|Allgemeiner Zweck| Gen 4| 16| 31.232| 2500|
|Allgemeiner Zweck| Gen 4| 32| 62.976| 5.000|
|Allgemeiner Zweck| Gen 5| 2| 3.584| 300|
|Allgemeiner Zweck| Gen 5| 4| 7.680| 625|
|Allgemeiner Zweck| Gen 5| 8| 15360| 1250|
|Allgemeiner Zweck| Gen 5| 16| 31.232| 2500|
|Allgemeiner Zweck| Gen 5| 32| 62.976| 5.000|
|Arbeitsspeicheroptimiert| Gen 5| 2| 7168| 600|
|Arbeitsspeicheroptimiert| Gen 5| 4| 15360| 1250|
|Arbeitsspeicheroptimiert| Gen 5| 8| 30720| 2500|
|Arbeitsspeicheroptimiert| Gen 5| 16| 62464| 5.000|

Diese zusätzlichen Serverparameter sind im System nicht konfigurierbar:

|**Parameter**|**Fester Wert**|
| :------------------------ | :-------- |
|innodb_file_per_table (im Tarif „Basic“)|OFF|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Weitere Serverparameter, die hier nicht aufgeführt sind, werden für die Versionen [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) und [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html) auf ihre vordefinierten MySQL-Standardwerte festgelegt.

## <a name="next-steps"></a>Nächste Schritte
- [Datenverbindungsbibliotheken für Azure Database for MySQL](concepts-connection-libraries.md)
