---
title: Mainframerehosting auf virtuellen Azure-Computern | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie für Mainframeworkloads wie IBM Z-basierte Systeme auf virtuellen Microsoft Azure-Computern ein Rehosting durchführen.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 8be0ebc486739f8826e8a1d5a5307a219ba71b6f
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192716"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Mainframerehosting auf virtuellen Azure-Computern

Durch das Migrieren von Workloads aus Mainframeumgebungen in die Cloud können Sie ihre Infrastruktur modernisieren und oft Kosten einsparen. Viele Workloads können mit nur geringfügigen Änderungen am Code, z.B. Aktualisieren der Namen der Datenbanken, in Azure übertragen werden.

Der Begriff *Mainframe* bezieht sich im Allgemeinen auf ein Großrechnersystem. Der Großteil der derzeit eingesetzten Mainframes sind jedoch IBM System Z-Server oder IBM-Plug-kompatible Systeme unter MVS, DOS, VSE, OS/390 oder z/OS.

Mit einem virtuellen Azure-Computer (VM) werden Ressourcen für eine bestimmte Anwendung auf einer einzigen Instanz isoliert und verwaltet. Mainframes wie IBM z/OS nutzen dafür logische Partitionen (LPARS). Ein Mainframe kann eine LPAR für eine CICS-Region mit zugeordneten COBOL-Programmen und eine separate LPAR für eine IBM Db2-Datenbank verwenden. Eine typische [n-schichtige Anwendung in Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) stellt Azure-VMs in einem virtuellen Netzwerk bereit, das für jede Schicht in Subnetze aufgeteilt werden kann.

Auf Azure-VMs können Mainframeemulationsumgebungen und Compiler ausgeführt werden, die Migrationen per Lift & Shift unterstützen. Die Entwicklungs- und Testumgebung werden häufig als erste Workloads von einem Mainframe zu Azure migriert. Zu den häufig eingesetzten Serverkomponenten, die Sie emulieren können, gehören Onlinetransaktionsverarbeitungs-, Batch- und Datenerfassungssysteme (siehe Abbildung).

![Mithilfe von Azure-Emulationsumgebungen können Sie z/OS-basierte Systeme ausführen.](media/01-overview.png)

Einige Mainframeworkloads lassen sich relativ leicht zu Azure migrieren, während für andere ein Rehosting in Azure mit einer Partnerlösung durchgeführt werden kann. Bei der Wahl einer Partnerlösung unterstützt Sie das [Azure-Migrationscenter für Mainframes](https://azure.microsoft.com/migration/mainframe/) mit einem ausführlichen Leitfaden.

## <a name="set-up-devtest-environment-using-a-micro-focus-rehosting-platform"></a>Einrichten einer Entwicklungs-/Testumgebung mit einer Micro Focus-Rehostingplattform

Micro Focus Enterprise Server ist eine der größten verfügbaren Mainframerehostingplattformen. Mit dieser können Sie z/OS-Workloads auf einer günstigeren x86-Plattform in Azure ausführen.

Hinweise zu den ersten Schritten finden Sie in den folgenden Artikeln:

- [Install Micro Focus Enterprise Server 4.0 and Enterprise Developer 4.0 on Azure (Installieren von Micro Focus Enterprise Server 4.0 und Enterprise Developer 4.0 in Azure)](./microfocus/set-up-micro-focus-on-azure.md)
- [Set up Micro Focus CICS BankDemo for Micro Focus Enterprise Developer 4.0 in Azure (Einrichten von Micro Focus CICS BankDemo für Micro Focus Enterprise Developer 4.0 in Azure)](./microfocus/demo.md)

## <a name="tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame in Azure

TmaxSoft OpenFrame ist eine beliebte Mainframerehostinglösung, mit der vorhandene Mainframeressourcen leicht per Lift & Shift zu Azure migriert werden können. Eine OpenFrame-Umgebung in Azure eignet sich für Entwicklungs-, Demo-, Test- und Produktionsworkloads.

Laden Sie das E-Book [Install TmaxSoft OpenFrame on Azure (Installieren von TmaxSoft OpenFrame in Azure)](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-on-azure/) herunter, um sich einen Überblick über die ersten Schritte zu verschaffen.

## <a name="set-up-a-devtest-environment-using-ibm-z-devtest-120"></a>Einrichten einer Entwicklungs-/Testumgebung mithilfe von IBM ZD&T 12.0

IBM Z Development and Test Environment (IBM ZD&T) richtet eine Umgebung in Azure ein, die nicht für den Produktionsbetrieb, sondern für das Entwickeln, Testen und Demonstrieren von z/OS-basierten Anwendungen verwendet werden kann.

In der Azure-Emulationsumgebung können mehrere Z-Instanzen mithilfe von Application Developers Controlled Distributions (ADCDs) gehostet werden. Sie können in Azure und Azure Stack ZD&T Personal Edition, ZD&T Parallel Sysplex und ZD&T Enterprise Edition ausführen.

Hinweise zu den ersten Schritten finden Sie in den folgenden Artikeln:

-   [Set up IBM zD&T 12.0 on Azure (Einrichten von IBM ZD&T 12.0 in Azure)](./ibm/install-ibm-z-environment.md)
-   [Set up ADCD on zD&T (Einrichten von ADCD auf ZD&T)](./ibm/demo.md)

## <a name="migrate-ibm-db2-purescale-to-azure"></a>Migrieren von IBM DB2 pureScale zu Azure

Die IBM DB2 pureScale-Umgebung bietet einen Datenbankcluster für Azure mit hoher Verfügbarkeit und Skalierbarkeit unter Linux-Betriebssystemen. Zwar ist dies nicht mit der ursprünglichen Umgebung identisch, doch bietet IBM DB2 pureScale unter Linux ähnliche Hochverfügbarkeit und Skalierbarkeitsfeatures wie IBM DB2 für z/OS, das in einer Parallel Sysplex-Konfiguration auf dem Mainframe ausgeführt wird.

Hinweise zu den ersten Schritten finden Sie unter [IBM DB2 pureScale on Azure (IBM DB2 pureScale in Azure)](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure).

## <a name="considerations"></a>Überlegungen

Wenn Sie Mainframeworkloads zu Azure-IaaS (Infrastructure-as-a-Service) migrieren, können Sie zwischen mehreren Typen von bedarfsgesteuerten, skalierbaren Computeressourcen einschließlich Azure-VMs auswählen. Azure stellt eine Reihe von [Linux-](https://docs.microsoft.com/azure/virtual-machines/linux/overview) und [Windows-VMs](https://docs.microsoft.com/azure/virtual-machines/windows/overview) bereit.

### <a name="high-availability-and-failover"></a>Hochverfügbarkeit und Failover

Azure bietet SLAs (Vereinbarungen zum Servicelevel) an, bei denen eine Verfügbarkeit mit mehreren Neunen Standard ist. Optimiert wird diese zudem durch eine lokale Replikation oder Georeplikation von Diensten. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

Für Azure-IaaS, z. B. für VMs, ist ein Failover von bestimmten Systemfunktionen wie Failoverclusteringinstanzen und [Verfügbarkeitsgruppen](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets) abhängig. Wenn Sie Azure-PaaS-Ressourcen (Platform-as-a-Service) wie [Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) und [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) verwenden, kann die Plattform automatisch ein Failover ausführen.

### <a name="scalability"></a>Skalierbarkeit

Mainframes werden in der Regel zentral hochskaliert, Cloudumgebungen horizontal. Azure bietet für [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) und [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) mehrere VM-Größen für unterschiedliche Anforderungen an. Auch die Cloud kann entsprechend der genauen Benutzeransprüche zentral hoch- oder herunterskaliert werden. Computeleistung, Speicher und Dienste werden in einem nutzungsbasierten Abrechnungsmodell nach Bedarf [skaliert](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling).

### <a name="storage"></a>Storage

In der Cloud stehen mehrere Speicheroptionen zur Verfügung, die flexibel und skalierbar sind. Die Kosten richten sich nach der tatsächlichen Nutzung. [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) bietet einen massiv skalierbaren Objektspeicher für Datenobjekte, einen Dateisystemdienst für die Cloud, einen zuverlässigen Messagingspeicher und einen NoSQL-Speicher. Für VMs bieten verwaltete und nicht verwaltete Datenträger einen dauerhaften und sicheren Speicher.

### <a name="backup-and-recovery"></a>Sicherung und Wiederherstellung

Die Verwaltung eines eigenen Notfallwiederherstellungsstandorts kann teuer sein. Azure bietet daher kostengünstige und leicht implementierbare Optionen für die [Sicherung](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup), [Wiederherstellung](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) und [Redundanz](https://docs.microsoft.com/azure/storage/common/storage-redundancy) auf lokaler oder regionaler Ebene oder mittels Georedundanz an.

## <a name="azure-government-for-mainframe-migrations"></a>Azure Government für Mainframemigrationen

Viele öffentliche Stellen würden ihre Mainframeanwendungen gerne zu einer moderneren und flexibleren Plattform migrieren. Microsoft Azure Government ist eine physisch getrennte Clouddiensteinstanz, die zwar auf der Microsoft Azure-Plattform basiert, jedoch an Behördensysteme auf staatlicher, bundesstaatlicher und kommunaler Ebene angepasst ist. Dieser Dienst stellt hervorragende Sicherheits-, Schutz- und Compliancedienste speziell für US-Regierungsbehörden und ihre Partner bereit.

Azure Government verfügt über eine Provisional Authority to Operate (P-ATO) auf FedRAMP High Impact-Stufe, die für bestimmte Systeme erforderlich ist. 

Laden Sie [Microsoft Azure Government cloud for mainframe applications (Microsoft Azure Government-Cloud für Mainframeanwendungen)](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/) herunter, um sich einen Überblick über die ersten Schritte zu verschaffen.

## <a name="learn-more"></a>Weitere Informationen

Wenn Sie eine Mainframemigration in Betracht ziehen, steht Ihnen das umfassende [Partnerökosystem](partner-workloads.md) zur Seite. Ausführliche Anleitungen zur Auswahl einer Partnerlösung finden Sie in der [Platform Modernization Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

Weitere Informationen:

-   [Mainframemigration](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
-   [Problembehandlung](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
-   [Demystifying mainframe to Azure migration (Mainframemigration zu Azure im Überblick)](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
