---
title: "SQL Server auf virtuellen Linux-Computern in Azure – FAQ | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Antworten auf häufig gestellte Fragen zur Ausführung von SQL Server auf virtuellen Linux-Computern in Azure."
services: virtual-machines-linux
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 10/05/2017
ms.author: jroth
ms.openlocfilehash: a001ae116e33e0b7be4431b0bc4c8bb319f4e801
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Häufig gestellte Fragen zu SQL Server auf virtuellen Linux-Computern in Azure

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

Dieses Thema bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [SQL Server auf virtuellen Linux-Computern in Azure](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> In diesem Thema werden spezifische Probleme mit SQL Server auf virtuellen Linux-Computern behandelt. Wenn Sie SQL Server auf virtuellen Windows-Computern ausführen, finden Sie weitere Informationen unter [Windows – häufig gestellte Fragen](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

1. **Wie erstelle ich einen virtuellen Linux-Computer in Azure mit SQL Server?**

   Die einfachste Lösung ist die Erstellung eines virtuellen Linux-Computers, der SQL Server enthält. Ein Tutorial für die Anmeldung bei Azure und das Erstellen einer SQL-VM im Portal finden Sie unter [Bereitstellen eines virtuellen Linux-SQL Server-Computers im Azure-Portal](provision-sql-server-linux-virtual-machine.md). Sie können SQL Server auch manuell auf einem virtuellen Computer mit einer frei lizenzierten Edition (Developer oder Express) oder einer wiederverwendeten lokalen Lizenz installieren. Wenn Sie Ihre eigene Lizenz verwenden, benötigen Sie die [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility).

1. **Wie aktualisiere ich auf eine neue Version/Edition von SQL Server auf einer Azure-VM?**

   Derzeit ist kein direktes Upgrade für SQL Server auf einer Azure-VM verfügbar. Erstellen Sie einen neuen virtuellen Azure-Computer mit der gewünschten Version/Edition von SQL Server, und migrieren Sie dann Ihre Datenbanken mit dem standardmäßigen [Datenmigrationsverfahren](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview) zu dem neuen Server.

1. **Wie kann ich meine lizenzierte Kopie von SQL Server auf einer Azure-VM installieren?**

   Erstellen Sie zunächst einen virtuellen Linux-Computer, auf dem sich nur das Betriebssystem befindet. Führen Sie dann die [SQL Server-Installationsschritte](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) für Ihre Linux-Distribution aus. Wenn Sie eine frei lizenzierten Edition von SQL Server installieren, müssen Sie außerdem eine SQL Server-Lizenz und [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/) besitzen.

1. **Gibt es Linux-VM-Images mit Bring-Your-Own-License (BYOL) für SQL Server?**

   Derzeit gibt es keine BYOL-Linux-VM-Images für SQL Server. Sie können SQL Server jedoch manuell auf einem virtuellen Nur-Linux-Computer installieren, wie in den vorangegangenen Fragen erläutert.

1. **Kann ich einen virtuellen Computer so ändern, dass meine eigene SQL Server-Lizenz verwendet wird, wenn er mithilfe eines der Katalogimages mit nutzungsbasierter Bezahlung erstellt wurde?**

   Nein. Der Wechsel von der Lizenzierung mit minutenbasierter Bezahlung zur Nutzung der eigenen Lizenz ist nicht möglich. Sie müssen eine neue Linux-VM erstellen, SQL Server installieren und Ihre Daten migrieren. Weitere Details zu BYOL finden Sie in der vorherigen Frage.

1. **Welche zugehörigen SQL Server-Pakete werden ebenfalls installiert?**

   Eine Liste der SQL Server-Pakete, die auf SQL Server-Linux-VMs standardmäßig installiert sind, finden Sie unter [installierte Pakete](sql-server-linux-virtual-machines-overview.md#packages).

1. **Werden SQL Server-Lösungen mit hoher Verfügbarkeit auf Azure-VMs unterstützt?**

   Derzeit leider nicht. Für AlwaysOn-Verfügbarkeitsgruppen und Failoverclustering wird eine Clusteringlösung unter Linux benötigt, z.B. Pacemaker. Die unterstützten Linux-Distributionen für SQL Server unterstützen keine Add-Ons für hohe Verfügbarkeit in der Cloud.

1. **Warum kann ich RHEL- und SLES-SQL Server-VMs nicht mit einem Azure-Abonnement bereitstellen, das ein Ausgabenlimit aufweist?**

   Virtuelle RHEL- und SLES-Computer erfordern ein Abonnement ohne Ausgabenlimit und mit einer dem Abonnement zugeordneten überprüften Zahlungsmethode (in der Regel eine Kreditkarte). Wenn Sie eine RHEL- oder SLES-VM bereitstellen, ohne das Ausgabenlimit zu entfernen, wird Ihr Abonnement deaktiviert, und alle virtuellen Computer/Dienste werden beendet. Wenn Sie in diese Situation kommen, [entfernen Sie das Ausgabenlimit](https://account.windowsazure.com/subscriptions), um das Abonnement wieder zu aktivieren. Das verbleibende Guthaben wird für den aktuellen Abrechnungszyklus wiederhergestellt, aber Ihre Kreditkarte wird mit einem Zuschlag für das RHEL- oder SLES-VM-Image belastet, wenn Sie es neu starten und weiterhin ausführen.

## <a name="resources"></a>Ressourcen

**Virtuelle Linux-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Linux-Computer](sql-server-linux-virtual-machines-overview.md)
* [Bereitstellen eines virtuellen Linux-Computers mit SQL Server](provision-sql-server-linux-virtual-machine.md)
* [SQL Server unter Linux – Dokumentation](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Virtuelle Windows-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Bereitstellen eines virtuellen Windows-Computers mit SQL Server](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Häufig gestellte Fragen (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)