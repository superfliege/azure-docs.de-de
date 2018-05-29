---
title: SQL-Hostserver in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SQL-Instanzen für die Bereitstellung über den SQL-Adapter-Ressourcenanbieter hinzufügen.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.openlocfilehash: a89e5bf48c24abf72f18ee98f2dcb0eda6db35cd
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33202591"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Hinzufügen von Hostservern für den SQL-Ressourcenanbieter
Sie können SQL Server-Instanzen auf virtuellen Computern in Ihrer [Azure Stack](azure-stack-poc.md)-Umgebung oder eine Instanz außerhalb Ihrer Azure Stack-Umgebung verwenden, sofern der Ressourcenanbieter eine Verbindung mit dieser herstellen kann. Allgemeine Anforderungen:

* Die SQL-Instanz muss für die Verwendung durch den Ressourcenanbieter und durch Benutzerworkloads vorgesehen sein. Sie können keine SQL-Instanz verwenden, die durch andere Consumer, einschließlich App Services, verwendet wird.
* Die VM eines SQL-Ressourcenanbieters ist nicht in eine Domäne eingebunden und kann nur per SQL-Authentifizierung eine Verbindung herstellen.
* Sie müssen ein Konto mit entsprechenden Berechtigungen für die Verwendung durch den Ressourcenanbieter konfigurieren.
* Der Ressourcenanbieter und die Benutzer (z.B. Web-Apps) verwenden das Benutzernetzwerk, sodass in diesem Netzwerk Konnektivität mit der SQL-Instanz erforderlich ist. Diese Anforderung bedeutet normalerweise, dass sich die IP-Adressen der SQL-Instanzen in einem öffentlichen Netzwerk befinden müssen.
* Sie sind für die Verwaltung der SQL-Instanzen und deren Hosts verantwortlich. Der Ressourcenanbieter installiert weder Patches, noch führt er Sicherungen oder Wechsel von Anmeldeinformationen o.Ä. durch.
* Mithilfe von SKUs können unterschiedliche Klassen von SQL-Funktionen, z.B. die Leistung, Always On usw., erstellt werden.

Einige Images für SQL-IaaS-VMs sind über das Marketplace-Verwaltungsfeature verfügbar. Achten Sie darauf, immer die neueste Version der **SQL-IaaS-Erweiterung** herunterzuladen, bevor Sie eine VM mit einem Marketplace-Artikel bereitstellen. Die SQL-Images sind mit den in Azure verfügbaren SQL-VMs identisch. Für virtuelle SQL-Computer, die mit diesen Images erstellt werden, stellen die IaaS-Erweiterung und die zugehörigen Portalerweiterungen Features wie das automatische Patchen und Sicherungsfunktionen bereit.

Es gibt noch weitere Optionen für die Bereitstellung von SQL-VMs, einschließlich Vorlagen im [Schnellstartkatalog für Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Alle Hostserver, die in Azure Stack mit mehreren Knoten installiert werden, müssen mit einem Benutzerabonnement erstellt werden. Sie können nicht mit dem Standardabonnement des Anbieters erstellt werden. Sie müssen über das Benutzerportal oder eine PowerShell-Sitzung mit einer entsprechenden Anmeldung erstellt werden. Alle Hostserver sind abrechenbare VMs, die entsprechende SQL-Lizenzen benötigen. Der Dienstadministrator _kann_ der Besitzer dieses Abonnements sein.


### <a name="required-privileges"></a>Erforderliche Berechtigungen

Sie können einen neuen Administrator mit geringeren als den vollständigen sysadmin-Berechtigungen erstellen. Die folgenden Vorgänge müssen zugelassen sein:

- Datenbank: erstellen, ändern, mit Einschluss (nur Always On), ablegen, sichern
- Verfügbarkeitsgruppe: ändern, verknüpfen, Datenbank hinzufügen/entfernen
- Anmeldung: erstellen, auswählen, ändern, ablegen, widerrufen
- Select-Vorgänge: \[master\].\[sys\].\[availability_group_listeners\] (Always On), sys.availability_replicas (Always On), sys.databases, \[master\].\[sys\].\[dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[sys\].\[availability_groups\] (Always On), sys.master_files



## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Bereitstellen von Kapazität durch Herstellen einer Verbindung mit einem eigenständigen SQL-Hostserver
Sie können eigenständige SQL Server-Instanzen (ohne hohe Verfügbarkeit) mit einer beliebigen Edition von SQL Server 2014 oder SQL Server 2016 verwenden. Stellen Sie sicher, dass Sie über die Anmeldeinformationen für ein Konto mit Systemadministratorrechten verfügen.

Zum Hinzufügen eines eigenständigen Hostservers, der bereits bereitgestellt wurde, führen Sie die folgenden Schritte aus:

1. Anmelden beim Azure Stack-Administratorportal als Dienstadministrator

2. Klicken Sie auf **Durchsuchen** &gt; **VERWALTUNGSRESSOURCEN** &gt; **SQL-Hostserver**.

  ![](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

  Auf dem Blatt **SQL Hosting Servers** (SQL-Hostserver) können Sie den SQL-Server-Ressourcenanbieter mit tatsächlichen SQL-Serverinstanzen verbinden, die als Back-End des Ressourcenanbieters dienen.

  ![Hostserver](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Füllen Sie das Formular mit den Verbindungsdetails für Ihre SQL Server-Instanz aus.

  ![Neue Hostserver](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Sie können optional einen Instanznamen aufnehmen, und eine Portnummer kann angegeben werden, wenn die Instanz nicht dem Standardport 1433 zugewiesen ist.

  > [!NOTE]
  > Sofern die SQL-Instanz für den Azure Resource Manager-Benutzer und -Administrator zugänglich ist, kann sie vom Ressourcenanbieter gesteuert werden. Die SQL-Instanz __muss__ ausschließlich dem Ressourcenanbieter zugeordnet sein.

4. Wenn Sie Server hinzufügen, müssen Sie diese einer neuen oder vorhandenen SKU hinzufügen, um Dienstangebote zu unterscheiden. Beispielsweise können Sie über eine SQL Enterprise-Instanz Folgendes bereitstellen:
  - Datenbankkapazität
  - Automatische Sicherungen
  - Reservieren von Hochleistungsservern für einzelne Abteilungen
  - usw.

  Der SKU-Name sollte die Eigenschaften widerspiegeln, damit Benutzer ihre Datenbanken entsprechend platzieren können. Alle Hostserver in einer SKU sollten die gleichen Funktionen aufweisen.

> [!IMPORTANT]
> Sonderzeichen, einschließlich Leerzeichen und Interpunktion, werden in den Namen für die **Familie** oder **Ebene** nicht unterstützt, wenn Sie die SKU für die SQL- und MySQL-Ressourcenanbieter erstellen.

Beispiel:

![SKUs](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
> Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Benutzer können erst eine Datenbank erstellen, wenn die SKU vollständig erstellt wurde.

## <a name="provide-capacity-using-sql-always-on-availability-groups"></a>Angeben der Kapazität mithilfe von SQL-AlwaysOn-Verfügbarkeitsgruppen
Für das Konfigurieren von SQL-Always On-Instanzen sind zusätzliche Schritte und mindestens drei virtuelle (oder physische) Computer erforderlich.

> [!NOTE]
> Der SQL-Adapter-Ressourcenanbieter unterstützt für Always On _nur_ Instanzen mit SQL 2016 SP1 Enterprise oder höher, da hierfür neue SQL-Features wie z.B. automatisches Seeding erforderlich sind. Zusätzlich zu den allgemeinen Anforderungen ist Folgendes erforderlich:

* Sie müssen zusätzlich zu den SQL-Always On-Computern noch einen Dateiserver bereitstellen. Es gibt eine [Azure Stack-Schnellstartvorlage](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-ha), die diese Umgebung für Sie erstellen kann. Sie kann auch als Richtlinie für das Erstellen Ihrer eigenen Instanz dienen.

* Sie müssen die SQL Server-Instanzen einrichten. Insbesondere müssen Sie das [automatische Seeding](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) für jede Verfügbarkeitsgruppe in jeder Instanz von SQL Server aktivieren.

```
ALTER AVAILABILITY GROUP [<availability_group_name>]
    MODIFY REPLICA ON 'InstanceName'
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

Auf sekundären Instanzen
```
ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
GO

```



Um SQL-Always On-Hostserver hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich als Dienstadministrator beim Azure Stack-Portal an.

2. Klicken Sie auf **Durchsuchen** &gt; **VERWALTUNGSRESSOURCEN** &gt; **SQL-Hostserver** &gt; **+ Hinzufügen**.

    Auf dem Blatt **SQL Hosting Servers** (SQL-Hostserver) können Sie den SQL-Server-Ressourcenanbieter mit tatsächlichen SQL-Serverinstanzen verbinden, die als Back-End des Ressourcenanbieters dienen.


3. Füllen Sie das Formular mit den Verbindungsdetails Ihrer SQL Server-Instanz aus, und verwenden Sie dabei den FQDN oder die IPv4-Adresse des Always On-Listeners (und optional die Portnummer). Geben Sie die Kontoinformationen für das Konto, das Sie mit Systemadministratorrechten konfiguriert haben, an.

4. Aktivieren Sie dieses Kontrollkästchen, um Instanzen von SQL-AlwaysOn-Verfügbarkeitsgruppe zu unterstützen.

    ![Hostserver](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Fügen Sie die SQL-Always On-Instanz einer SKU hinzu. Sie können in derselben SKU nicht eigenständige Servern mit Always On-Instanzen kombinieren. Dies wird beim Hinzufügen des ersten Hostservers bestimmt. Der Versuch, die Typen im Nachhinein zu kombinieren, führt zu einem Fehler.


## <a name="making-sql-databases-available-to-users"></a>Verfügbarmachen von SQL-Datenbanken für Benutzer

Erstellen Sie Pläne und Angebote, um SQL-Datenbanken für Benutzer zur Verfügung zu stellen. Fügen Sie dem Plan den Dienst Microsoft.SqlAdapter hinzu. Außerdem müssen Sie entweder ein vorhandenes Kontingent hinzufügen oder ein neues erstellen. Wenn Sie ein Kontingent erstellen, geben Sie die Kapazität an, die Sie dem Benutzer gewähren möchten.

![Erstellen von Plänen und Angeboten, die Datenbanken umfassen](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)


## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Datenbanken](azure-stack-sql-resource-provider-databases.md)
