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
ms.date: 05/18/2018
ms.openlocfilehash: 2d62cd693d7a67faf836c645f8bd33de9afca949
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266107"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Azure Database for MySQL – Gespeicherte Prozeduren für die Replikation eingehender Daten

Mithilfe der Replikation eingehender Daten können Sie Daten von einem MySQL-Server, der lokal, auf virtuellen Computern oder von Datenbankdiensten ausgeführt wird, die von anderen Cloudanbietern gehostet werden, mit dem Azure Database for MySQL-Dienst synchronisieren.

Mit den folgenden gespeicherten Prozeduren wird die Replikation eingehender Daten zwischen einem primären Computer und einem Replikat eingerichtet oder entfernt.

|**Name der gespeicherten Prozedur**|**Eingabeparameter**|**Ausgabeparameter**|**Hinweis zur Verwendung**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_primary*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/V|Um Daten im SSL-Modus zu übertragen, übergeben Sie im Parameter „master_ssl_ca“ den Kontext des Zertifizierungsstellenzertifikats. </br><br>Um Daten ohne SSL zu übertragen, übergeben Sie im Parameter „master_ssl_ca“ eine leere Zeichenfolge.|
|*mysql.az_replication _start*|N/V|N/V|Startet die Replikation.|
|*mysql.az_replication _stop*|N/V|N/V|Beendet die Replikation.|
|*mysql.az_replication _remove_primary*|N/V|N/V|Entfernt die Replikationsbeziehung zwischen dem primären Computer und dem Replikat.|
|*mysql.az_replication_skip_counter*|N/V|N/V|Überspringt einen Replikationsfehler.|

Informationen zum Einrichten der Replikation eingehender Daten zwischen einem primären Computer und einem Replikat in Azure Database for MySQL finden Sie unter [Konfigurieren der Replikation eingehender Daten](howto-data-in-replication.md).