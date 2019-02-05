---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 01/28/2019
ms.author: genemi
ms.openlocfilehash: edeaa932abe4d1882f957d0ed26aaddd97dabe3f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198060"
---
<!-- sql-database-connect-query-prerequisites-create-db-includes.md -->

- Azure SQL-Datenbank auf einem [logischen Server](https://docs.microsoft.com/azure/sql-database/sql-database-single-index) oder in einer [verwalteten Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index). Sie können eine der folgenden Methoden verwenden, um eine Datenbank zu erstellen:

| Logischer Server | Verwaltete Instanz |
| --- | --- |
| [Portal](../articles/sql-database/sql-database-get-started-portal.md) | [Portal](../articles/sql-database/sql-database-managed-instance-get-started.md) |
| [BEFEHLSZEILENSCHNITTSTELLE (CLI)](../articles/sql-database/sql-database-get-started-cli.md) | |
| [PowerShell](../articles/sql-database/sql-database-get-started-powershell.md) | |

- **Nur logischer Server**: Eine konfigurierte Firewallregel auf Serverebene, die das Herstellen einer Verbindung mit Ihrem logischen Server ermöglicht. Weitere Informationen finden Sie unter [Erstellen einer Firewallregel auf Serverebene](../articles/sql-database/sql-database-get-started-portal-firewall.md).
- **Nur verwaltete Instanz**: Konfigurierte Verbindung von dem Computer, der auf eine verwaltete Instanz zugreift. Sie können die folgenden Optionen verwenden:
  - [Virtueller Azure-Computer](../articles/sql-database/sql-database-managed-instance-configure-vm.md) im gleichen Azure-VNET wie die verwaltete Instanz, der auf die Instanz zugreifen kann
  - [Point-to-Site-Verbindung](../articles/sql-database/sql-database-managed-instance-configure-p2s.md) auf Ihrem Computer, die es Ihnen ermöglicht, Ihren Computer in das VNET einzubinden, in dem die verwaltete Instanz platziert wird, und die verwaltete Instanz wie jede andere SQL Server-Instanz in Ihrem Netzwerk zu verwenden.
