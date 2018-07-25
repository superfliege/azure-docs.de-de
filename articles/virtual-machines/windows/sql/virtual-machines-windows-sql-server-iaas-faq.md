---
title: SQL Server auf virtuellen Microsoft Azure-Computern – FAQ | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zur Ausführung von SQL Server auf Azure-VMs.
services: virtual-machines-windows
documentationcenter: ''
author: v-shysun
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: v-shysun
ms.openlocfilehash: 48df858095cb867954460ec858567e41ed330063
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39012068"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-azure-virtual-machines"></a>Häufig gestellte Fragen zu SQL Server in Microsoft Azure Virtual Machines

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [SQL Server auf virtuellen Microsoft Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> In diesem Artikel werden spezifische Probleme mit SQL Server auf virtuellen Windows-Computern behandelt. Wenn Sie SQL Server auf virtuellen Linux-Computern ausführen, finden Sie weitere Informationen unter [Linux – häufig gestellte Fragen](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Images

1. **Welche Images im SQL Server Virtual Machine-Katalog sind verfügbar?**

   Azure verwaltet Images virtueller Computer für alle unterstützten Hauptversionen von SQL Server in allen Editionen für Windows und Linux. Weitere Informationen finden Sie in der vollständigen Liste der [Windows-VM-Images](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) und [Linux-VM-Images](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Werden vorhandene Images im SQL Server Virtual Machine-Katalog aktualisiert?**

   SQL Server-Images im Katalog der virtuellen Computer werden alle zwei Monate mit den aktuellen Windows- und Linux-Updates aktualisiert. Für Windows-Images umfasst dies alle Updates, die in Windows Update als „wichtig“ gekennzeichnet sind, einschließlich wichtiger SQL Server-Sicherheitsupdates und Service Packs. Für Linux-Images umfasst dies die neuesten Systemupdates. Kumulative Updates für SQL Server werden für Linux und Windows unterschiedlich gehandhabt. Für Linux sind kumulative Updates für SQL Server ebenfalls in der Aktualisierung enthalten. Virtuelle Windows-Computer werden derzeit jedoch nicht mit kumulativen Updates für SQL Server oder Windows Server aktualisiert.

1. **Können Images von virtuellen SQL Server-Computern aus dem Katalog entfernt werden?**

   Ja. Azure behält nur ein Image pro Hauptversion und Edition bei. Wenn beispielsweise ein neues Service Pack für SQL Server veröffentlicht wird, fügt Azure dem Katalog ein neues Image für dieses Service Pack hinzu. Das SQL Server-Image für das vorherige Service Pack wird umgehend aus dem Azure-Portal entfernt. Es bleibt jedoch für drei weitere Monate zur Bereitstellung über PowerShell verfügbar. Nach drei Monaten ist das jeweils vorhergehende Service Pack-Image nicht mehr verfügbar. Diese Entfernungsrichtlinie gilt auch, wenn eine SQL Server-Version nach Ende ihres Lebenszyklus nicht mehr unterstützt wird.

1. **Kann ich ein VHD-Image auf der Grundlage einer SQL Server-VM erstellen?**

   Ja. Hierbei sind jedoch ein paar Punkte zu beachten: Wenn Sie diese VHD für einen neuen virtuellen Computer in Azure bereitstellen, steht der Abschnitt „SQL Server-Konfiguration“ im Portal nicht zur Verfügung. Die SQL Server-Konfigurationsoptionen müssen in diesem Fall über PowerShell verwaltet werden. Die Abrechnung erfolgt zudem auf der Grundlage des Tarifs der SQL-VM, auf der Ihr Image ursprünglich basiert. Dies gilt auch, wenn Sie SQL Server vor der Bereitstellung von der virtuellen Festplatte entfernen. 

1. **Ist es möglich, Konfigurationen einzurichten, die nicht im Katalog der virtuellen Computer gezeigt werden (z.B. Windows 2008 R2 + SQL Server 2012) ?**

   Nein. Für Katalogimages von virtuellen Computern, die SQL Server enthalten, müssen Sie eines der bereitgestellten Images auswählen.

## <a name="creation"></a>Erstellung

1. **Wie erstelle ich einen virtuellen Azure-Computer mit SQL Server?**

   Die einfachste Lösung ist die Erstellung eines virtuellen Computers, der SQL Server enthält. Ein Tutorial für die Anmeldung bei Azure und das Erstellen eines virtuellen SQL-Computers im Portal finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Sie können sich für ein VM-Image mit sekundengenau abgerechneter SQL Server-Lizenzierung entscheiden oder ein Image verwenden, das die Verwendung Ihrer eigenen SQL Server-Lizenz ermöglicht. Sie können SQL Server auch manuell auf einem virtuellen Computer mit einer frei lizenzierten Edition (Developer oder Express) oder einer wiederverwendeten lokalen Lizenz installieren. Wenn Sie Ihre eigene Lizenz verwenden, benötigen Sie die [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/). Weitere Informationen finden Sie unter [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Preisinformationen für virtuelle Azure-Computer unter SQL Server).

1. **Wie kann ich meine lokale SQL Server-Datenbank zur Cloud migrieren?**

   Erstellen Sie zuerst einen virtuellen Azure-Computer mit einer SQL Server-Instanz. Migrieren Sie dann Ihre lokalen Datenbanken zu dieser Instanz. Datenmigrationsstrategien finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Lizenzierung

1. **Wie kann ich meine lizenzierte Kopie von SQL Server auf einer Azure-VM installieren?**

   Es gibt hierbei zwei Möglichkeiten. Sie können eines der [VM-Images, die Lizenzen unterstützen](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), bereitstellen – dies ist auch bekannt als Bring-Your-Own-License (BYOL). Eine weitere Möglichkeit ist das Kopieren der SQL Server-Installationsmedien auf einen virtuellen Windows Server-Computer und das anschließende Installieren von SQL Server auf dem virtuellen Computer. Wenn Sie SQL Server jedoch manuell installieren, gibt es keine Portalintegration, und die SQL Server-IaaS-Agent-Erweiterung wird nicht unterstützt, sodass Features wie z.B. automatisierte Sicherung und automatisiertes Patchen in diesem Szenario nicht funktionieren. Aus diesem Grund sollten Sie eines der Images aus dem BYOL-Katalog verwenden. Um BYOL oder Ihre eigenen SQL Server-Medien auf einer Azure-VM zu verwenden, benötigen Sie [License Mobility durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/). Weitere Informationen finden Sie unter [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Preisinformationen für virtuelle Azure-Computer unter SQL Server).

1. **Kann ich einen virtuellen Computer so ändern, dass meine eigene SQL Server-Lizenz verwendet wird, wenn er mithilfe eines der Katalogimages mit nutzungsbasierter Bezahlung erstellt wurde?**

   Nein. Von der Lizenzierung mit sekundengenauer Abrechnung kann nicht zur Verwendung einer eigenen Lizenz gewechselt werden. Erstellen Sie mithilfe eines der [BYOL-Images](virtual-machines-windows-sql-server-iaas-overview.md#BYOL) einen neuen virtuellen Azure-Computer, und migrieren Sie dann Ihre Datenbanken mit dem standardmäßigen [Datenmigrationsverfahren](virtual-machines-windows-migrate-sql.md) zum neuen Server.

1. **Muss ich Lizenzgebühren für SQL Server auf einem virtuellen Azure-Computer bezahlen, wenn dieser nur für Standby/Failover verwendet wird?**

   Wenn Sie über Software Assurance verfügen und Lizenzmobilität gemäß der Beschreibung in [FAQ zur Lizenzierung von virtuellen Computern](http://azure.microsoft.com/pricing/licensing-faq/) verwenden, müssen Sie keine Lizenzgebühren für einen SQL Server bezahlen, wenn dieser als passives sekundäres Replikat in einer Bereitstellung mit hoher Verfügbarkeit fungiert. Andernfalls müssen Sie für die Lizenzierung bezahlen.


## <a name="administration"></a>Verwaltung

1. **Kann ich eine zweite Instanz von SQL Server auf dem gleichen virtuellen Computer installieren? Kann ich die installierten Funktionen der Standardinstanz ändern?**

   Ja. Die SQL Server-Installationsmedien befinden sich in einem Ordner auf dem Laufwerk **C** . Führen Sie **Setup.exe** von diesem Speicherort aus, um neue SQL Server-Instanzen hinzuzufügen oder andere installierte Features von SQL Server auf dem Computer zu ändern. Beachten Sie, dass einige Funktionen wie „Automatisierte Sicherung“, „Automatisiertes Patchen“ und „Azure Key Vault-Integration“ nur für die Standardinstanz ausgeführt werden können.

1. **Kann ich die Standardinstanz von SQL Server deinstallieren?**

   Ja. Dabei sind jedoch ein paar Punkte zu beachten: Wie bereits in der vorhergehenden Antwort erläutert, können Funktionen, die sich auf die [SQL Server-IaaS-Agenterweiterung](virtual-machines-windows-sql-server-agent-extension.md) stützen, nur für die Standardinstanz ausgeführt werden. Wenn Sie die Standardinstanz deinstallieren, sucht die Erweiterung weiterhin danach und generiert möglicherweise Fehler im Ereignisprotokoll. Diese Fehler stammen aus den folgenden zwei Quellen: **Microsoft SQL Server-Verwaltung von Anmeldeinformationen** und **Microsoft SQL Server-IaaS-Agent**. Einer der Fehler kann dem Folgenden ähneln:

      Netzwerkbezogener oder instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden, oder auf ihn kann nicht zugegriffen werden.

   Wenn Sie sich entscheiden, die Standardinstanz zu deinstallieren, deinstallieren Sie auch die [SQL Server-IaaS-Agenterweiterung](virtual-machines-windows-sql-server-agent-extension.md).

1. **Kann ich SQL Server vollständig von einer SQL-VM entfernen?**

   Ja, die Abrechnung basiert jedoch weiterhin auf Ihrer SQL-VM, wie in den [Preisinformationen für virtuelle Azure-Computer mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md) beschrieben. Wenn Sie SQL Server nicht mehr benötigen, können Sie einen neuen virtuellen Computer bereitstellen und die Daten und Anwendungen zu diesem neuen virtuellen Computer migrieren. Anschließend können Sie den virtuellen SQL Server-Computer entfernen.
   
## <a name="updating-and-patching"></a>Aktualisieren und Patchen

1. **Wie aktualisiere ich auf eine neue Version/Edition von SQL Server auf einer Azure-VM?**

   Derzeit ist kein direktes Upgrade für SQL Server auf einer Azure-VM verfügbar. Erstellen Sie einen neuen virtuellen Azure-Computer mit der gewünschten Version/Edition von SQL Server, und migrieren Sie dann Ihre Datenbanken mit dem standardmäßigen [Datenmigrationsverfahren](virtual-machines-windows-migrate-sql.md) zu dem neuen Server.

1. **Wie werden Updates und Servicepacks auf eine SQL Server-VM angewendet?**

   Virtuelle Computer bieten Ihnen die Kontrolle über die Hostcomputer, einschließlich des Zeitpunkts, zu dem Updates angewendet werden sollen, und der Art und Weise. Für das Betriebssystem können Sie manuell Windows-Updates anwenden oder einen Planungsdienst namens [Automatisiertes Patchen](virtual-machines-windows-sql-automated-patching.md) aktivieren. Automatisiertes Patchen installiert alle wichtigen Updates, einschließlich SQL Server-Updates in dieser Kategorie. Andere optionale Updates für SQL Server müssen manuell installiert werden.

## <a name="general"></a>Allgemein

1. **Werden SQL Server-Failoverclusterinstanzen (FCI) auf Azure-VMs unterstützt?**

   Ja. Sie können [einen Windows-Failovercluster unter Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) erstellen und für die Speicherung des Clusters das Feature „Direkte Speicherplätze“ (S2D) verwenden. Alternativ können Sie, wie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) beschrieben, Cluster- oder Speicherlösungen von Drittanbietern nutzen.

   > [!IMPORTANT]
   > Derzeit wird die [SQL Server-IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) nicht für SQL Server-Failoverclusterinstanzen (FCI) in Azure unterstützt. Sie sollten die Erweiterung auf virtuellen Computern deinstallieren, die an den FCI beteiligt sind. Diese Erweiterung unterstützt Features wie automatisierte Sicherung und automatisiertes Patchen sowie einige Portalfeatures für SQL. Diese Features sind nach der Deinstallation des Agents nicht für SQL-VMs einsetzbar.

1. **Was ist der Unterschied zwischen SQL-VMs und dem SQL-Datenbank-Dienst?**

   Im Prinzip unterscheidet sich die Ausführung von SQL Server auf virtuellen Azure-Computern nicht von der Ausführung in einem Remoterechenzentrum. Im Gegensatz dazu bietet [SQL-Datenbank](../../../sql-database/sql-database-technical-overview.md) Database-as-a-Service. Mit SQL-Datenbank haben Sie keinen Zugriff auf die Computer, die Ihre Datenbanken hosten. Einen vollständigen Vergleich finden Sie unter [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Wie installiere ich SQL Data-Tools auf meiner Azure-VM?**

    Laden Sie die SQL Data-Tools von [Microsoft SQL Server Data Tools – Business Intelligence für Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313) herunter, und installieren Sie sie.

## <a name="resources"></a>angeben

**Virtuelle Windows-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Bereitstellen eines virtuellen Windows-Computers mit SQL Server](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrieren einer Datenbank zu SQL Server auf einer Azure-VM](virtual-machines-windows-migrate-sql.md)
* [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md)
* [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md)
* [Anwendungsmuster und Entwicklungsstrategien für SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuelle Linux-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Linux-Computer](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Bereitstellen eines virtuellen Linux-Computers mit SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Häufig gestellte Fragen (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server unter Linux – Dokumentation](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
