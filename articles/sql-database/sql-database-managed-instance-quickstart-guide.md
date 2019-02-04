---
title: 'Schnellstart: Verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation'
description: Hier finden Sie eine schnelle Einführung in die Verwendung verwalteter Azure SQL-Datenbank-Instanzen.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 77deed43c106a451d3de768989233c749e1280e1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468171"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Erste Schritte mit verwalteten Azure SQL-Datenbank-Instanzen

Eine [verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-index.yml) ist eine vollständig verwaltete PaaS-Version von SQL Server, die in der Azure-Cloud gehostet und in Ihrem eigenen VNET mit der privaten IP-Adresse platziert wird. In diesem Abschnitt erfahren Sie, wie Sie schnell eine verwaltete Instanz konfigurieren und erstellen und wie Sie Ihre Datenbanken migrieren.

## <a name="quickstart-overview"></a>Schnellstartübersicht

Dieser Abschnitt enthält eine Übersicht über die verfügbaren Artikel für den schnellen Einstieg in die Verwendung verwalteter Instanzen. Am einfachsten können Sie Ihre erste verwaltete Instanz über das [Azure-Portal](sql-database-managed-instance-get-started.md) erstellen. Dort können Sie die erforderlichen Parameter (Benutzername/Kennwort, Anzahl von Kernen, maximaler Speicher) konfigurieren und automatisch eine Azure-Netzwerkumgebung erstellen, ohne sich mit Netzwerkdetails oder Infrastrukturanforderungen zu befassen. Sie müssen lediglich darauf achten, dass Sie über einen geeigneten [Abonnementtyp](sql-database-managed-instance-resource-limits.md#supported-subscription-types) für die Instanzerstellung verfügen.

Wenn Sie Ihr eigenes Netzwerk verwenden oder anpassen möchten, erfahren Sie [hier](#configure-network-environment), wie Sie die Netzwerkumgebung für die verwaltete Instanz konfigurieren.

Wenn Sie Ihre verwaltete Instanz erstellen, müssen Sie mit einer der folgenden Methoden eine Verbindung mit der Instanz herstellen:

* Erstellen Sie in einem Subnetz des VNETs, in dem Ihre verwaltete Instanz platziert wird, einen [virtuellen Azure-Computer](sql-database-managed-instance-configure-vm.md) mit einer Installation von SQL Server Management Studio sowie mit anderen Apps, die verwendet werden können, um auf Ihre verwaltete Instanz zuzugreifen. Der virtuelle Computer darf sich nicht im gleichen Subnetz befinden wie Ihre verwalteten Instanzen.
* Richten Sie auf Ihrem Computer eine [Point-to-Site-Verbindung](sql-database-managed-instance-configure-p2s.md) ein. Dadurch können Sie Ihren Computer in das VNET einbinden, in dem die verwaltete Instanz platziert wird, und die verwaltete Instanz wie jede andere SQL Server-Instanz in Ihrem Netzwerk verwenden.

Sie können auch ExpressRoute oder eine Site-to-Site-Verbindung über Ihr lokales Netzwerk verwenden. Diese Methoden werden in den Schnellstartanleitungen jedoch nicht behandelt.

Wenn Sie eine verwaltete Instanz erstellen und den Zugriff konfigurieren, können Sie mit der Migration Ihrer Datenbanken beginnen, die sich in der lokalen SQL Server-Instanz oder auf virtuellen Azure-Computern befinden. Beachten Sie, dass die Migration nicht erfolgreich ist, wenn die zu migrierende Quelldatenbank nicht unterstützte Features enthält. Um Fehler zu vermeiden und die Kompatibilität zu überprüfen, können Sie den [Datenmigrations-Assistenten (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) installieren. Dieser analysiert Ihre Datenbanken in SQL Server und ermittelt sämtliche Probleme, die unter Umständen die Migration zur verwalteten Instanz verhindern (beispielsweise das Vorhandensein von FileStream oder mehreren Protokolldateien). Nachdem Sie diese Probleme behoben haben, sind Ihre Datenbanken bereit für die Migration zur verwalteten Instanz. Der [Assistent für Datenbankexperimente](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) ist ein nützliches Tool, das Ihre Workload in SQL Server aufzeichnen und in der verwalteten Instanz wiedergeben kann, um zu ermitteln, ob nach der Migration zur verwalteten Instanz Leistungsprobleme zu erwarten sind.

Nachdem Sie sich vergewissert haben, dass Sie Ihre Datenbank zur verwalteten Instanz migrieren können, können Sie die [native Wiederherstellungsfunktion](sql-database-managed-instance-get-started-restore.md) verwenden. Diese Funktion ermöglicht es Ihnen, per Transact-SQL-Befehl eine Sicherung Ihrer Datenbank zu erstellen, die Sicherung in Azure Blob Storage hochzuladen und die Datenbank per Transact-SQL-Befehl aus Blob Storage wiederherzustellen.

In diesen Schnellstartanleitungen erfahren Sie, wie Sie Ihre verwalteten Instanzen schnell konfigurieren, erstellen und ihnen Datenbanken hinzufügen. In einigen Szenarien müssen Sie die Bereitstellung der verwalteten Instanz und der erforderlichen Netzwerkumgebung anpassen oder automatisieren. Diese Szenarien werden im Anschluss beschrieben.

## <a name="customizing-network-environment"></a>Anpassen der Netzwerkumgebung

Wenn die Instanz über das [Azure-Portal](sql-database-managed-instance-get-started.md) erstellt wird, kann das VNET/Subnetz zwar automatisch konfiguriert werden, manchmal empfiehlt es sich jedoch, es vor der Erstellung verwalteter Instanzen zu erstellen, da Sie in diesem Fall die Parameter des VNETs/Subnetzes konfigurieren können. Die einfachste Möglichkeit zum Erstellen und Konfigurieren der Netzwerkumgebung ist die Verwendung der [Azure-Vorlage für die Ressourcenbereitstellung](sql-database-managed-instance-create-vnet-subnet.md). Diese erstellt und konfiguriert Ihr Netzwerk und das Subnetz, in dem die Instanz platziert wird. Sie müssen lediglich auf die Bereitstellungsschaltfläche von Azure Resource Manager klicken und das Formular mit Parametern ausfüllen. Alternativ können Sie die Erstellung des Netzwerks mit einem [PowerShell-Skript](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) automatisieren.

Wenn Sie bereits über ein VNET und ein Subnetz verfügen, in dem Sie Ihre verwaltete Instanz bereitstellen möchten, müssen diese die [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) erfüllen. Verwenden Sie [dieses PowerShell-Skript](sql-database-managed-instance-configure-vnet-subnet.md), um sich zu vergewissern, dass Ihr Subnetz ordnungsgemäß konfiguriert ist. Dieses Skript überprüft Ihr Netzwerk, meldet Probleme, informiert Sie über erforderliche Änderungen und bietet an, die nötigen Änderungen an Ihrem VNET/Subnetz vorzunehmen. Führen Sie dieses Skript aus, wenn Sie Ihr VNET/Subnetz nicht manuell konfigurieren möchten. Darüber hinaus empfiehlt es sich, das Skript nach jeder größeren Neukonfiguration Ihrer Netzwerkinfrastruktur auszuführen. Wenn Sie Ihr eigenes Netzwerk erstellen und konfigurieren möchten, lesen Sie die [Dokumentation zu verwalteten Instanzen](sql-database-managed-instance-connectivity-architecture.md) sowie [diese Anleitung](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01).

## <a name="automating-creation-of-managed-instance"></a>Automatisieren der Erstellung der verwalteten Instanz

 Falls Sie die Netzwerkumgebung noch nicht wie im vorherigen Schritt beschrieben erstellt haben, kann das Azure-Portal dies für Sie erledigen. Dabei wird die Umgebung allerdings mit einigen Standardparametern konfiguriert, die Sie später nicht mehr ändern können. Alternativ können Sie [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/), [PowerShell mit Resource Manager-Vorlage](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md) oder die [Azure-Befehlszeilenschnittstelle](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/) verwenden.

## <a name="migrating-to-managed-instance-with-minimal-downtime"></a>Migrieren zur verwalteten Instanz mit minimaler Downtime

In den Artikeln dieser Schnellstartanleitungen erfahren Sie, wie Sie schnell eine verwaltete Instanz einrichten und Ihre Datenbanken verschieben. Mit der nativen Wiederherstellung müssen Sie allerdings warten, bis die Datenbanken wiederhergestellt wurden, wodurch eine gewisse Downtime für Ihre Anwendung entsteht (insbesondere bei größeren Datenbanken). Für die Verschiebung Ihrer Produktionsdatenbank benötigen Sie wahrscheinlich eine bessere Migrationsmethode mit möglichst geringer Downtime bei der Migration. Der [Datenmigrationsdienst](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) kann Ihre Datenbank mit minimaler Downtime migrieren. Hierzu werden nach und nach die an der Quelldatenbank vorgenommenen Änderungen mittels Push an eine Datenbank übertragen, die Sie in der verwalteten Instanz wiederherstellen. Dadurch können Sie Ihre Anwendung schnell und mit minimaler Downtime von der Quell- auf die Zieldatenbank umstellen.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit einer [allgemeinen Liste unterstützter Features in verwalteten Instanzen](sql-database-features.md) sowie mit [Details und bekannten Problemen](sql-database-managed-instance-transact-sql-information.md) vertraut. 
* Informieren Sie sich über [technische Merkmale der verwalteten Instanz](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits). 
* Befassen Sie sich mit [komplexeren Tutorials](sql-database-howto-managed-instance.md). 
