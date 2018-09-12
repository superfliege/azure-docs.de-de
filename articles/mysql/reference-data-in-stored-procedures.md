---
title: Azure Database for MySQL – Gespeicherte Prozeduren für die Replikation eingehender Daten
description: In diesem Artikel werden alle gespeicherten Prozeduren vorgestellt, die für die Replikation eingehender Daten verwendet werden.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: fb1a1b31d90df0022e5973de3ae2f55fb4c36701
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665945"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Azure Database for MySQL – Gespeicherte Prozeduren für die Replikation eingehender Daten

Mithilfe der Replikation eingehender Daten können Sie Daten von einem MySQL-Server, der lokal, auf virtuellen Computern oder von Datenbankdiensten ausgeführt wird, die von anderen Cloudanbietern gehostet werden, mit dem Azure Database for MySQL-Dienst synchronisieren.

Mit den folgenden gespeicherten Prozeduren wird die Datenreplikation zwischen einem Master und einem Replikat eingerichtet oder entfernt.

|**Name der gespeicherten Prozedur**|**Eingabeparameter**|**Ausgabeparameter**|**Hinweis zur Verwendung**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/V|Um Daten im SSL-Modus zu übertragen, übergeben Sie im Parameter „master_ssl_ca“ den Kontext des Zertifizierungsstellenzertifikats. </br><br>Um Daten ohne SSL zu übertragen, übergeben Sie im Parameter „master_ssl_ca“ eine leere Zeichenfolge.|
|*mysql.az_replication _start*|N/V|N/V|Startet die Replikation.|
|*mysql.az_replication _stop*|N/V|N/V|Beendet die Replikation.|
|*mysql.az_replication _remove_master*|N/V|N/V|Entfernt die Replikationsbeziehung zwischen dem Master und dem Replikat|
|*mysql.az_replication_skip_counter*|N/V|N/V|Überspringt einen Replikationsfehler.|

Informationen zum Einrichten der Datenreplikation zwischen einem Master und einem Replikat in Azure Database for MySQL finden Sie unter [Konfigurieren der Replikation eingehender Daten](howto-data-in-replication.md).
