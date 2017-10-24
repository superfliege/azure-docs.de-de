---
title: SQL-Hostserver in Azure Stack | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie SQL-Instanzen für die Bereitstellung über den SQL-Adapter-Ressourcenanbieter hinzufügen."
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 58c83b74041e0e2e82729f569c53aca59f3aed43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="add-hosting-servers-for-use-by-the-sql-adapter"></a>Hinzufügen von Hostservern für die Verwendung durch den SQL-Adapter

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können SQL Server-Instanzen auf virtuellen Computern in Ihrer [Azure Stack](azure-stack-poc.md)-Umgebung oder eine Instanz außerhalb Ihrer Azure Stack-Umgebung verwenden, sofern der Ressourcenanbieter eine Verbindung mit dieser herstellen kann. Allgemeine Anforderungen:

* Die SQL-Instanz muss für die Verwendung durch den Ressourcenanbieter und durch Benutzerworkloads vorgesehen sein. Sie können keine SQL-Instanz verwenden, die durch andere Consumer, einschließlich App Services, verwendet wird.
* Der Ressourcenanbieteradapter ist nicht in eine Domäne eingebunden und kann nur per SQL-Authentifizierung eine Verbindung herstellen.
* Sie müssen ein Konto mit entsprechenden Berechtigungen für die Verwendung durch den Ressourcenanbieter konfigurieren.
* Der Netzwerkdatenverkehr vom Ressourcenanbieter zu SQL erfolgt über den Port 1433. Dieser Port kann nicht geändert werden.
* Der Ressourcenanbieter und die Benutzer (z.B. Web-Apps) verwenden das Benutzernetzwerk, sodass in diesem Netzwerk Konnektivität mit der SQL-Instanz erforderlich ist. Diese Anforderung bedeutet normalerweise, dass sich die IP-Adressen der SQL-Instanzen in einem öffentlichen Netzwerk befinden müssen.
* Sie sind für die Verwaltung der SQL-Instanzen und deren Hosts verantwortlich. Der Ressourcenanbieter installiert weder Patches, noch führt er Sicherung oder Wechsel von Anmeldeinformationen o.Ä. durch.
* Mithilfe von SKUs können unterschiedliche Klassen von SQL-Funktionen, z.B. die Leistung, Always On usw., erstellt werden.



Einige Images für SQL-IaaS-VMs sind über das Marketplace-Verwaltungsfeature verfügbar. Achten Sie darauf, immer die neueste Version der SQL-IaaS-Erweiterung herunterzuladen, bevor Sie einen virtuellen Computer mit einem Marketplace-Artikel bereitstellen. Die SQL-Images sind mit den in Azure verfügbaren SQL-VMs identisch. Für virtuelle SQL-Computer, die mit diesen Images erstellt werden, stellen die IaaS-Erweiterung und die zugehörigen Portalerweiterungen Features wie das automatische Patchen und Sicherungsfunktionen bereit.

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

  > [!NOTE]
  > Sofern die SQL-Instanz für den Azure Resource Manager-Benutzer und -Administrator zugänglich ist, kann sie vom Ressourcenanbieter gesteuert werden. Die SQL-Instanz __muss__ ausschließlich dem Ressourcenanbieter zugeordnet sein.

4. Wenn Sie Server hinzufügen, müssen Sie diese einer neuen oder vorhandenen SKU hinzufügen, um Dienstangebote zu unterscheiden. Beispielsweise können Sie über eine SQL Enterprise-Instanz Folgendes bereitstellen:
  - Datenbankkapazität
  - Automatische Sicherungen
  - Reservieren von Hochleistungsservern für einzelne Abteilungen
  - usw.

  Der SKU-Name sollte die Eigenschaften widerspiegeln, damit Benutzer ihre Datenbanken entsprechend platzieren können. Alle Hostserver in einer SKU sollten die gleichen Funktionen aufweisen.

    Beispiel:

    ![SKUs](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
Es kann bis zu einer Stunde dauern, bis SKUs im Portal angezeigt werden. Sie können erst eine Datenbank erstellen, wenn die SKU vollständig erstellt wurde.

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


3. Füllen Sie das Formular mit den Verbindungsdetails Ihrer SQL Server-Instanz aus, und verwenden Sie dabei den FQDN oder die IPv4-Adresse des Always On-Listeners. Geben Sie die Kontoinformationen für das Konto, das Sie mit Systemadministratorrechten konfiguriert haben, an.

4. Aktivieren Sie dieses Kontrollkästchen, um Instanzen von SQL-AlwaysOn-Verfügbarkeitsgruppe zu unterstützen.

    ![Hostserver](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Fügen Sie die SQL-Always On-Instanz einer SKU hinzu. Sie können in derselben SKU nicht eigenständige Servern mit Always On-Instanzen kombinieren. Dies wird beim Hinzufügen des ersten Hostservers bestimmt. Der Versuch, die Typen im Nachhinein zu kombinieren, führt zu einem Fehler.


## <a name="making-sql-databases-available-to-users"></a>Verfügbarmachen von SQL-Datenbanken für Benutzer

Erstellen Sie Pläne und Angebote, um SQL-Datenbanken für Benutzer zur Verfügung zu stellen. Fügen Sie dem Plan den Dienst Microsoft.SqlAdapter hinzu. Außerdem müssen Sie ein vorhandenes Kontingent hinzufügen oder ein neues erstellen. Wenn Sie ein Kontingent erstellen, können Sie die Kapazität angeben, die Sie dem Benutzer gewähren möchten.

![Erstellen von Plänen und Angeboten, die Datenbanken umfassen](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="maintenance-of-the-sql-adapter-rp"></a>Warten des SQL-Adapter-Ressourcenanbieters

Die Wartung von SQL Server-Instanzen wird hier nicht behandelt. Sie finden hier lediglich Informationen zum Kennwortwechsel. Sie sind verantwortlich für das Patchen, Sichern und Wiederherstellen der Datenbankserver, die mit dem SQL-Adapter verwendet werden.

### <a name="patching-and-updating"></a>Patchen und Aktualisieren
 Der SQL-Adapter wird nicht als Teil von Azure Stack gewartet, da es sich um eine Add-On-Komponente handelt. Microsoft stellt bei Bedarf Updates für den SQL-Adapter bereit. Der SQL-Adapter wird auf einem virtuellen _Benutzercomputer_ im Standardabonnement des Anbieters instanziiert. Aus diesem Grund ist es erforderlich, Windows-Patches, Antivirensignaturen usw. bereitzustellen. Die Windows-Updatepakete werden im Rahmen des Patch- und Updatezyklus bereitgestellt und können zum Anwenden eines Updates auf die Windows-VM verwendet werden. Bei der Veröffentlichung eines aktualisierten Adapters wird ein Skript zum Anwenden des Updates bereitgestellt. Dieses Skript erstellt eine neue Ressourcenanbieter-VM und migriert jeden beliebigen bereits verfügbaren Zustand.

 ### <a name="backuprestoredisaster-recovery"></a>Sicherung/Wiederherstellung/Notfallwiederherstellung
 Der SQL-Adapter wird nicht als Teil des Sicherungs-/Notfallwiederherstellungsprozesses von Azure Stack gesichert, da es sich um eine Add-On-Komponente handelt. Skripts werden für Folgendes bereitgestellt:
- Sichern der erforderlichen Zustandsinformationen (in einem Azure Stack-Speicherkonto gespeichert)
- Wiederherstellen des Ressourcenanbieters bei einer vollständigen Stack-Wiederherstellung.
Vor dem Ressourcenanbieter müssen die Datenbankserver (sofern erforderlich) wiederhergestellt werden.

### <a name="updating-sql-credentials"></a>Aktualisieren von SQL-Anmeldeinformationen

Sie sind für das Erstellen und Verwalten von Systemadministratorkonten auf Ihren SQL Server-Instanzen verantwortlich. Der Ressourcenanbieter benötigt ein Konto mit diesen Berechtigungen zum Verwalten von Datenbanken im Auftrag von Benutzern – er benötigt keinen Zugriff auf die Daten in diesen Datenbanken. Wenn Sie die Systemadministratorkennwörter auf Ihren SQL Server-Instanzen ändern müssen, können Sie die Updatefunktion der Administratorschnittstelle des Ressourcenanbieters verwenden, um das gespeicherte Kennwort, das vom Ressourcenanbieter verwendet wird, zu ändern. Diese Kennwörter werden in einem Schlüsseltresor in Ihrer Azure Stack-Instanz gespeichert.

Um die Einstellungen zu ändern, klicken Sie auf **Durchsuchen** &gt; **VERWALTUNGSRESSOURCEN** &gt; **SQL-Hostserver** &gt; **SQL-Anmeldungen**, und wählen Sie einen Anmeldenamen aus. Die Änderung muss zuerst auf der SQL-Instanz vorgenommen werden (und ggf. auf allen Replikaten). Klicken Sie im Bereich **Einstellungen** auf **Kennwort**.

![Aktualisieren des Administratorkennworts](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)


## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen von Datenbanken](azure-stack-sql-resource-provider-databases.md)
