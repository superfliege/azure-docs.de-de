---
title: Replizieren von Daten in Azure Database for MySQL
description: In diesem Artikel wird die Datenreplikation für Azure Database for MySQL beschrieben.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: 6135e4a0182f3af7db54eab974e4c307402185ab
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666075"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replizieren von Daten in Azure Database for MySQL

Mithilfe der Replikation eingehender Daten können Sie Daten von einem MySQL-Server, der lokal, auf virtuellen Computern oder von Datenbankdiensten ausgeführt wird, die von anderen Cloudanbietern gehostet werden, mit dem Azure Database for MySQL-Dienst synchronisieren. Die Replikation eingehender Daten basiert auf der nativen MySQL-Replikation, die wiederum auf der Position der binären Protokolldatei (binlog) basiert. Weitere Informationen zur binlog-Replikation finden Sie unter [Binary Log File Position Based Replication Configuration Overview](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html) (Konfiguration der auf der Position der binären Protokolldatei basierenden Replikation – Übersicht). 

## <a name="when-to-use-data-in-replication"></a>Szenarien für die Verwendung der Datenreplikation für Azure Database for MySQL
Zu den wichtigsten Szenarien, bei denen die Datenreplikation für Azure Database for MySQL infrage kommt, zählen Folgende:

- **Synchronisierung von Hybriddaten:** Mit der Datenreplikation für Azure Database for MySQL können Sie Daten zwischen Ihren lokalen Servern und Azure Database for MySQL synchron halten. Eine solche Synchronisierung ist für die Erstellung von Hybridanwendungen nützlich. Diese Methode ist optimal geeignet, wenn Sie über einen lokalen Datenbankserver verfügen, die Daten jedoch in eine Region verschieben möchten, die sich näher bei den Endbenutzern befindet.
- **Synchronisierung von Daten in verschiedenen Clouds:** Verwenden Sie bei komplexen Cloudlösungen die Datenreplikation für Azure Database for MySQL, um Daten zwischen Azure Database for MySQL und unterschiedlichen Cloudanbietern zu synchronisieren, einschließlich virtueller Computer und Datenbankdienste, die in diesen Clouds gehostet werden.

## <a name="limitations-and-considerations"></a>Einschränkungen und Aspekte

### <a name="data-not-replicated"></a>Nicht replizierte Daten
Die [*Systemdatenbank „mysql“*](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) auf dem Masterserver wird nicht repliziert. Änderungen an Konten und Berechtigungen auf dem Masterserver werden nicht repliziert. Wenn Sie ein Konto auf dem Masterserver erstellen und dieses Konto über Zugriff auf den Replikatserver verfügen muss, erstellen Sie dasselbe Konto manuell auf dem Replikatserver. Einen Überblick über die Tabellen, die in der Systemdatenbank enthalten sind, finden Sie im [Leitfaden zu MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Anforderungen
- Der Masterserver muss mindestens die MySQL-Version 5.6 aufweisen. 
- Die Versionen des Master- und des Replikatservers müssen identisch sein. Beispielsweise müssen beide die Version MySQL 5.6 oder beide MySQL 5.7 aufweisen.
- Jede Tabelle muss über einen Primärschlüssel verfügen.
- Der Masterserver sollte die MySQL InnoDB-Engine verwenden.
- Benutzer müssen über Berechtigungen zum Konfigurieren der binären Protokollierung und zum Erstellen neuer Benutzer auf dem Masterserver verfügen.

### <a name="other"></a>Andere
- Die Datenreplikation wird nur in den Tarifen Universell und Arbeitsspeicheroptimiert unterstützt.
- IDs globaler Transaktionen (GTIDs) werden nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [die Datenreplikation für Azure Database for MySQL einrichten](howto-data-in-replication.md).
