---
title: 'Azure-Portal: Erstellen einer verwalteten SQL-Instanz | Microsoft-Dokumentation'
description: Erstellen einer verwalteten SQL-Instanz, einer Netzwerkumgebung und eines virtuellen Clientcomputers für den Zugriff
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: Carlrab
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 3eadc2d233fd1716716c323f4c7087ee8363c67c
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912321"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Schnellstart: Erstellen einer verwalteten Azure SQL-Datenbank-Instanz

Diese Schnellstartanleitung zeigt, wie Sie eine [verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md) im Azure-Portal erstellen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-managed-instance"></a>Erstellen einer verwalteten Instanz

Die folgenden Schritte zeigen, wie Sie eine verwaltete Instanz erstellen.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Suchen Sie nach **Verwaltete Instanz**, und wählen Sie dann **Verwaltete Azure SQL-Datenbank-Instanz** aus.
3. Klicken Sie auf **Create**.

   ![Erstellen einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Füllen Sie das Formular für die verwaltete Instanz mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | BESCHREIBUNG |
   | ------ | --------------- | ----------- |
   | **Abonnement** | Ihr Abonnement | Ein Abonnement mit der Berechtigung zum Erstellen neuer Ressourcen |
   |**Name der verwalteten Instanz**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Administratoranmeldung für verwaltete Instanz**|Beliebiger gültiger Benutzername|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Verwenden Sie nicht „serveradmin“. Hierbei handelt es sich um eine reservierte Rolle auf Serverebene.| 
   |**Kennwort**|Ein gültiges Kennwort|Das Kennwort muss mindestens 16 Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
   |**Ressourcengruppe**|Eine neue oder vorhandene Ressourcengruppe|Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen).|
   |**Location**|Der Speicherort, an dem Sie die verwaltete Instanz erstellen möchten|Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).|
   |**Virtuelles Netzwerk**|Wählen Sie entweder **Neues virtuelles Netzwerk erstellen** oder ein virtuelles Netzwerk aus, das Sie zuvor in der Ressourcengruppe erstellt haben, die bereits in diesem Formular angegeben wurde.| Eine Vorlage zum Konfigurieren eines virtuellen Netzwerks für eine verwaltete Instanz mit benutzerdefinierten Einstellungen finden Sie in Github unter [Configure SQL Managed Instance virtual network environment template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment). Informationen zu den Anforderungen für das Konfigurieren der Netzwerkumgebung für eine verwaltete Instanz finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-vnet-configuration.md). |

   ![Formular für die verwaltete Instanz](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Klicken Sie auf **Tarif**, um die Größe der Compute- und Speicherressourcen festzulegen und die Tarifoptionen zu prüfen. Der Tarif „Universell“ mit 32 GB Arbeitsspeicher und 16 V-Kernen ist der Standardwert.
6. Verwenden Sie die Schieberegler oder Textfelder, um die Speichermenge und die Anzahl von virtuellen Kernen anzugeben. 
7. Klicken Sie nach Abschluss des Vorgangs auf **Übernehmen**, um Ihre Auswahl zu speichern.  
8. Klicken Sie auf **Erstellen**, um die verwaltete Instanz bereitzustellen.
9. Klicken Sie auf das Symbol **Benachrichtigungen**, um den Status der Bereitstellung anzuzeigen.

    ![Fortschritt der Bereitstellung einer verwalteten Instanz](./media/sql-database-managed-instance-get-started/deployment-progress.png)

10. Klicken Sie auf **Die Bereitstellung wird ausgeführt**, um das Fenster für die verwaltete Instanz zu öffnen und den Bereitstellungsstatus weiter zu verfolgen. 

> [!IMPORTANT]
> Für die erste Instanz in einem Subnetz ist die Bereitstellungsdauer normalerweise deutlich länger als bei den nachfolgenden Instanzen. Brechen Sie den Bereitstellungsvorgang nicht ab, weil er länger als erwartet dauert. Das Erstellen der zweiten verwalteten Instanz im Subnetz dauert nur einige Minuten.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Überprüfen der Ressourcen und Abrufen des vollqualifizierten Servernamens

Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, überprüfen Sie die erstellten Ressourcen, und rufen Sie den vollqualifizierten Servernamen für die Verwendung in späteren Schnellstartanleitungen ab.

1. Öffnen Sie die Ressourcengruppe für Ihre verwaltete Instanz, und zeigen Sie die Ressourcen an, die in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md) erstellt wurden.

   ![Ressourcen der verwalteten Instanz](./media/sql-database-managed-instance-get-started/resources.png)Öffnen Sie die Ressource „Verwaltete Instanz“ im Azure-Portal.

2. Klicken Sie auf die verwaltete Instanz.
3. Suchen Sie auf der Registerkarte **Übersicht** nach der Eigenschaft **Host**, und kopieren Sie die vollständig qualifizierte Hostadresse für die verwaltete Instanz.


   ![Ressourcen der verwalteten Instanz](./media/sql-database-managed-instance-get-started/host-name.png)

   Der Name lautet etwa wie folgt: **Name_Ihres_Computers.neu15011648751ff.database.windows.net**.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verbinden mit einer verwalteten Instanz finden Sie wie folgt:
  - Eine Übersicht über die Verbindungsoptionen für Anwendungen finden Sie unter [Verbinden Sie Ihre Anwendungen mit der verwalteten Instanz](sql-database-managed-instance-connect-app.md).
  - Eine Schnellstartanleitung, die zeigt, wie Sie eine Verbindung von einem virtuellen Azure-Computer zu einer verwalteten Instanz herstellen, finden Sie unter [Konfigurieren einer Verbindung zu einem virtuellen Azure-Computer](sql-database-managed-instance-configure-vm.md).
  - Eine Schnellstartanleitung, die zeigt, wie Sie von einem lokalen Clientcomputer über eine Point-to-Site-Verbindung eine Verbindung zu einer verwalteten Instanz herstellen, finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung](sql-database-managed-instance-configure-p2s.md).
- Zur Wiederherstellung einer vorhandenen SQL Server-Datenbank von einer lokalen in eine verwaltete Instanz können Sie entweder [Azure Database Migration Service (DMS) für die Migration](../dms/tutorial-sql-server-to-managed-instance.md) oder den [T-SQL-Befehl „RESTORE“](sql-database-managed-instance-get-started-restore.md) verwenden, um die Wiederherstellung von einer Datenbanksicherungsdatei durchzuführen.
- Informationen zur erweiterten Überwachung der Datenbankleistung verwalteter Instanzen mit integrierten Problembehandlungsfunktionen finden Sie unter [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)](../log-analytics/log-analytics-azure-sql.md).
