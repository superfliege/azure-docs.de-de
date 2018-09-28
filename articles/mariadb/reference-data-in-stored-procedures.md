---
title: Azure Database for MariaDB – gespeicherte Prozeduren für die Datenreplikation
description: In diesem Artikel werden alle gespeicherten Prozeduren vorgestellt, die für die Replikation eingehender Daten verwendet werden.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: reference
ms.date: 09/24/2018
ms.openlocfilehash: 87c6fa783964c019841810ec38f342a5a44c0ee3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959086"
---
# <a name="azure-database-for-mariadb-data-in-replication-stored-procedures"></a>Azure Database for MariaDB – gespeicherte Prozeduren für die Datenreplikation

Mithilfe der Datenreplikation können Sie Daten von einem MariaDB-Server, der lokal, auf virtuellen Computern oder von Datenbankdiensten ausgeführt wird, die von anderen Cloudanbietern gehostet werden, mit dem Azure Database for MariaDB-Dienst synchronisieren.

Mit den folgenden gespeicherten Prozeduren wird die Datenreplikation zwischen einem Master und einem Replikat eingerichtet oder entfernt.

|**Name der gespeicherten Prozedur**|**Eingabeparameter**|**Ausgabeparameter**|**Hinweis zur Verwendung**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/V|Um Daten im SSL-Modus zu übertragen, übergeben Sie im Parameter „master_ssl_ca“ den Kontext des Zertifizierungsstellenzertifikats. </br><br>Um Daten ohne SSL zu übertragen, übergeben Sie im Parameter „master_ssl_ca“ eine leere Zeichenfolge.|
|*mysql.az_replication _start*|N/V|N/V|Startet die Replikation.|
|*mysql.az_replication _stop*|N/V|N/V|Beendet die Replikation.|
|*mysql.az_replication _remove_master*|N/V|N/V|Entfernt die Replikationsbeziehung zwischen dem Master und dem Replikat|
|*mysql.az_replication_skip_counter*|N/V|N/V|Überspringt einen Replikationsfehler.|

Informationen zum Einrichten der Datenreplikation zwischen einem Master und einem Replikat in Azure Database for MariaDB finden Sie unter [Konfigurieren der Replikation eingehender Daten](howto-data-in-replication.md).