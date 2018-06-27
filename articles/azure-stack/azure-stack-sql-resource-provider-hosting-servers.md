---
title: SQL-Hostserver in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SQL-Instanzen für die Bereitstellung über den SQL-Adapterressourcenanbieter hinzufügen.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2018
ms.author: jeffgilb
ms.openlocfilehash: 183d9479ae18e557b00d0867cad79600145da7bd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265227"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Hinzufügen von Hostservern für den SQL-Ressourcenanbieter

Sie können eine SQL-Instanz auf einem virtuellen Computer (VM) in [Azure Stack](azure-stack-poc.md) oder auf einer VM außerhalb Ihrer Azure Stack-Umgebung hosten, solange sich der SQL-Ressourcenanbieter mit der Instanz verbinden kann.

## <a name="overview"></a>Übersicht

Es gelten die folgenden allgemeinen Anforderungen für SQL-Hostserver:

* Die SQL-Instanz muss für die Verwendung durch den Ressourcenanbieter und durch Benutzerworkloads vorgesehen sein. Sie können keine SQL-Instanz verwenden, die durch andere Consumer verwendet wird. Diese Einschränkung gilt auch für App Services.
* Die VM eines SQL-Ressourcenanbieters ist nicht in eine Domäne eingebunden und kann nur per SQL-Authentifizierung eine Verbindung herstellen.
* Sie müssen ein Konto mit entsprechenden Berechtigungen für die Verwendung durch den Ressourcenanbieter konfigurieren.
* Der Ressourcenanbieter und die Benutzer (z.B. Web-Apps) verwenden das Benutzernetzwerk, sodass in diesem Netzwerk Konnektivität mit der SQL-Instanz erforderlich ist. Diese Anforderung bedeutet normalerweise, dass sich die IP-Adressen der SQL-Instanzen in einem öffentlichen Netzwerk befinden müssen.
* Die Verwaltung der SQL-Instanzen und ihrer Hosts liegt bei Ihnen. Beispielsweise wendet der Ressourcenanbieter keine Updates an, führt keine Sicherungen aus oder rotiert Anmeldeinformationen.
* Sie können SKUs verwenden, die verschiedene Klassen von SQL-Fähigkeiten, z.B. Leistung und Hochverfügbarkeit, mit AlwaysOn unterstützen.

### <a name="sql-server-virtual-machine-images"></a>Images virtueller SQL Server-Computer

Images für SQL-IaaS-VMs sind über das Marketplace-Verwaltungsfeature verfügbar. Diese Images sind mit den in Azure verfügbaren SQL-VMs identisch.

Achten Sie darauf, immer die neueste Version der **SQL-IaaS-Erweiterung** herunterzuladen, bevor Sie eine VM mit einem Marketplace-Artikel bereitstellen.  Die IaaS-Erweiterung und die zugehörigen Portalerweiterungen stellen zusätzliche Features wie das automatische Patchen und Sicherungen bereit.

Es gibt noch weitere Optionen für die Bereitstellung von SQL-VMs, einschließlich Vorlagen im [Schnellstartkatalog für Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Alle Hostserver, die in Azure Stack mit mehreren Knoten installiert werden, müssen mit einem Benutzerabonnement erstellt werden. Sie können nicht mit dem Standardabonnement des Anbieters erstellt werden. Sie müssen über das Benutzerportal oder eine PowerShell-Sitzung mit einer entsprechenden Anmeldung erstellt werden. Alle Hostserver sind abrechenbare VMs, die entsprechende SQL-Lizenzen benötigen. Der Dienstadministrator _kann_ der Besitzer dieses Abonnements sein.

### <a name="required-privileges"></a>Erforderliche Berechtigungen

Sie können einen Administrator erstellen, der weniger Rechte als ein SQL-Systemadministrator hat. Der Benutzer benötigt lediglich Rechte für die folgenden Vorgänge:

- Datenbank: erstellen, ändern, mit Kapselung (nur AlwaysOn), ablegen, sichern
- Verfügbarkeitsgruppe: ändern, verknüpfen, Datenbank hinzufügen/entfernen
- Anmeldung: erstellen, auswählen, ändern, ablegen, widerrufen
- Select-Vorgänge: \[master\].\[sys\].\[availability_group_listeners\] (Always On), sys.availability_replicas (Always On), sys.databases, \[master\].\[sys\].\[dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[sys\].\[availability_groups\] (Always On), sys.master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Bereitstellen von Kapazität durch Herstellen einer Verbindung mit einem eigenständigen SQL-Hostserver

Sie können eigenständige SQL Server-Instanzen (ohne hohe Verfügbarkeit) mit einer beliebigen Edition von SQL Server 2014 oder SQL Server 2016 verwenden. Stellen Sie sicher, dass Sie über die Anmeldeinformationen für ein Konto mit Systemadministratorrechten verfügen.

Zum Hinzufügen eines eigenständigen Hostservers, der bereits eingerichtet wurde, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim Azure Stack-Operatorportal als Dienstadministrator an.

2. Wählen Sie **Durchsuchen** &gt; **VERWALTUNGSRESSOURCEN** &gt; **SQL-Hostserver** aus.

   ![SQL-Hostserver](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Unter **SQL-Hostserver** können Sie den SQL Server-Ressourcenanbieter mit tatsächlichen SQL Server-Instanzen verbinden, die als Back-End des Ressourcenanbieters dienen.

   ![Dashboard „SQL-Adapter“](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Füllen Sie das Formular mit den Verbindungsdetails für Ihre SQL Server-Instanz aus.

   ![Hinzufügen eines SQL-Hostservers](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Sie können optional einen Instanznamen einschließen und eine Portnummer angeben, wenn die Instanz nicht dem Standardport 1433 zugewiesen ist.

   > [!NOTE]
   > Sofern die SQL-Instanz für den Azure Resource Manager-Benutzer und -Administrator zugänglich ist, kann sie vom Ressourcenanbieter gesteuert werden. Die SQL-Instanz __muss__ ausschließlich dem Ressourcenanbieter zugeordnet sein.

4. Wenn Sie Server hinzufügen, müssen Sie diese einer neuen oder vorhandenen SKU hinzufügen, um Dienstangebote zu unterscheiden. Beispielsweise können Sie über eine SQL Enterprise-Instanz Folgendes bereitstellen:
  
   - Datenbankkapazität
   - Automatische Sicherungen
   - Reservieren von Hochleistungsservern für einzelne Abteilungen

   Alle Hostserver in einer SKU sollten die gleichen Funktionen aufweisen. Der **Name** sollte die Eigenschaften der SKU widerspiegeln, sodass Benutzer ihre Datenbanken für die entsprechende SKU bereitstellen können.

   > [!IMPORTANT]
   > Sonderzeichen, einschließlich Leerzeichen und Interpunktion, werden in den Namen für die **Familie** oder **Ebene** nicht unterstützt, wenn Sie die SKU für die SQL- und MySQL-Ressourcenanbieter erstellen.

   Beispiel: 

   ![Erstellen einer SKU](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

   >[!NOTE]
   > Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Benutzer können erst eine Datenbank erstellen, wenn die SKU vollständig erstellt wurde.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Bereitstellen von Hochverfügbarkeit mithilfe von SQL-AlwaysOn-Verfügbarkeitsgruppen

Für das Konfigurieren von SQL-AlwaysOn-Instanzen sind zusätzliche Schritte und mindestens drei virtuelle (oder physische) Computer erforderlich. In diesem Artikel wird davon ausgegangen, dass Sie bereits über solide Kenntnisse zu AlwaysOn-Verfügbarkeitsgruppen verfügen. Weitere Informationen finden Sie unter 

* [Einführung in SQL Server Always On-Verfügbarkeitsgruppen auf virtuellen Azure-Computern](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Always On-Verfügbarkeitsgruppen (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> Der SQL-Adapterressourcenanbieter unterstützt _ausschließlich_ SQL 2016 SP1 Enterprise oder spätere Instanzen für AlwaysOn. Diese Adapterkonfiguration erfordert neue SQL-Features wie z.B. automatisches Seeding.

Zusätzlich zur vorangehenden Liste der Anforderungen müssen Sie das [automatische Seeding](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) in jeder Verfügbarkeitsgruppe für jede Instanz von SQL Server aktivieren.

Um das automatische Seeding auf allen Instanzen zu aktivieren, bearbeiten Sie den folgenden SQL-Befehl für jede Instanz und führen ihn dann aus:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

Für die sekundären Instanzen bearbeiten Sie den folgenden SQL-Befehl für jede Instanz und führen ihn dann aus:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>Hinzufügen von SQL-AlwaysOn-Hostservern

1. Melden Sie sich als Dienstadministrator beim Azure Stack-Administratorportal an.

2. Wählen Sie **Durchsuchen** &gt; **VERWALTUNGSRESSOURCEN** &gt; **SQL-Hostserver** &gt; **+Hinzufügen** aus.

   Unter **SQL-Hostserver** können Sie den SQL Server-Ressourcenanbieter mit tatsächlichen SQL Server-Instanzen verbinden, die als Back-End des Ressourcenanbieters dienen.

3. Füllen Sie das Formular mit den Verbindungsdetails für Ihre SQL Server-Instanz aus. Stellen Sie sicher, dass Sie die FQDN-Adresse des AlwaysOn-Listeners (und die optionale Portnummer) verwenden. Geben Sie die Informationen für das Konto an, das Sie mit Systemadministratorrechten konfiguriert haben.

4. Aktivieren Sie das Kontrollkästchen „AlwaysOn-Verfügbarkeitsgruppe“, um Instanzen von SQL-AlwaysOn-Verfügbarkeitsgruppen zu unterstützen.

   ![Aktivieren von AlwaysOn](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Fügen Sie die SQL-Always On-Instanz einer SKU hinzu.

   > [!IMPORTANT]
   > Sie können in derselben SKU nicht eigenständige Server mit AlwaysOn-Instanzen kombinieren. Der Versuch, nach dem Hinzufügen des ersten Hostservers Typen zu kombinieren, führt zu einem Fehler.

## <a name="make-the-sql-databases-available-to-users"></a>Verfügbarmachen von SQL-Datenbanken für Benutzer

Erstellen Sie Pläne und Angebote, um SQL-Datenbanken für Benutzer zur Verfügung zu stellen. Fügen Sie dem Plan den Dienst **Microsoft.SqlAdapter** hinzu. Außerdem müssen Sie das Standardkontingent hinzufügen oder ein neues Kontingent erstellen.

![Erstellen von Plänen und Angeboten, die Datenbanken umfassen](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Datenbanken](azure-stack-sql-resource-provider-databases.md)
