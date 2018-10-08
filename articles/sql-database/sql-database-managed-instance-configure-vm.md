---
title: Herstellen einer Verbindung mit der Client-VM – Verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Stellen Sie von einer Azure-VM aus über SQL Server Management Studio eine Verbindung mit einer verwalteten Azure SQL-Datenbank-Instanz her.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, srbozovi, bonova
manager: craigg
ms.date: 09/13/2018
ms.openlocfilehash: 08b8e37493ea7bc549a2352aaa6714ef7c65bfdb
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159515"
---
# <a name="configure-azure-vm-to-connect-to-an-azure-sql-database-managed-instance"></a>Konfigurieren einer Azure-VM für das Herstellen einer Verbindung mit einer verwalteten Azure SQL-Datenbank-Instanz

In diesem Schnellstart wird gezeigt, wie Sie eine Azure-VM für das Herstellen einer Verbindung mit einer verwalteten Azure SQL-Datenbank-Instanz über SQL Server Management Studio (SSMS) konfigurieren. Eine Schnellstartanleitung, die zeigt, wie Sie von einem lokalen Clientcomputer über eine Point-to-Site-Verbindung eine Verbindung herstellen, finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung](sql-database-managed-instance-configure-p2s.md). 

## <a name="prerequisites"></a>Voraussetzungen

Als Ausgangspunkt in diesem Schnellstart werden die Ressourcen verwendet, die in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md) erstellt wurden.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-new-subnet-in-the-managed-instance-vnet"></a>Erstellen eines neuen Subnetzes im VNET der verwalteten Instanz

In den folgenden Schritten wird im VNET der verwalteten Instanz ein neues Subnetz erstellt, damit ein virtueller Azure-Computer eine Verbindung mit der verwalteten Instanz herstellen kann. Das Subnetz der verwalteten Instanz wird ausschließlich für verwaltete Instanzen verwendet, und Sie können keine anderen Ressourcen (z.B. Azure Virtual Machines-Instanzen) in diesem Subnetz erstellen. 

1. Öffnen Sie die Ressourcengruppe für die verwaltete Instanz, die Sie in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md) erstellt haben, und klicken Sie auf das virtuelle Netzwerk für Ihre verwaltete Instanz. Klicken Sie dann auf **Subnetze**.

   ![Ressourcen der verwalteten Instanz](./media/sql-database-managed-instance-configure-vm/resources.png)

2. Klicken Sie neben **Subnetz** auf das **+**-Zeichen, um ein neues Subnetz erstellen.

   ![Subnetze einer verwalteten Instanz](./media/sql-database-managed-instance-configure-vm/subnets.png)

3. Füllen Sie das Formular mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | Beschreibung |
   | ---------------- | ----------------- | ----------- | 
   | **Name** | Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   | **Adressbereich (CIDR-Block)** | Ein gültiger Bereich | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Netzwerksicherheitsgruppe** | Keine | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Routingtabelle** | Keine | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Dienstendpunkte** | 0 ausgewählt | Der Standardwert reicht für diesen Schnellstart aus.|
   | **Subnetzdelegierung** | Keine | Der Standardwert reicht für diesen Schnellstart aus.|
 
   ![Neues Subnetz der verwalteten Instanz für die Client-VM](./media/sql-database-managed-instance-configure-vm/new-subnet.png)

4. Klicken Sie auf **OK**, um dieses zusätzliche Subnetz im VNET der verwalteten Instanz zu erstellen.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Erstellen eines virtuellen Computers im neuen Subnetz im VNET

In den folgenden Schritten wird veranschaulicht, wie Sie einen virtuellen Computer im neuen Subnetz erstellen, um eine Verbindung mit der verwalteten Instanz herzustellen. 

## <a name="prepare-the-azure-virtual-machine"></a>Vorbereiten des virtuellen Azure-Computers

Da sich die verwaltete SQL-Instanz in Ihrer privaten Virtual Network-Instanz befindet, müssen Sie einen virtuellen Azure-Computer erstellen, auf dem ein SQL-Clienttool wie SQL Server Management Studio oder Azure Data Studio installiert ist, um eine Verbindung mit der verwalteten Instanz herzustellen und Abfragen auszuführen. In diesem Schnellstart wird SQL Server Management Studio verwendet.

Die einfachste Möglichkeit zum Erstellen eines virtuellen Clientcomputers mit allen erforderlichen Tools ist die Verwendung von Azure Resource Manager-Vorlagen.

1. Klicken Sie zum Erstellen einer Client-VM auf die folgende Schaltfläche, und installieren Sie SQL Server Management Studio. (Stellen Sie sicher, dass Sie in einer anderen Browserregisterkarte beim Azure-Portal angemeldet sind.)

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Füllen Sie das Formular mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | BESCHREIBUNG |
   | ---------------- | ----------------- | ----------- |
   | **Abonnement** | Ein gültiges Abonnement | Hierbei muss es sich um ein Abonnement mit der Berechtigung zum Erstellen neuer Ressourcen handeln. |
   | **Ressourcengruppe** |Die Ressourcengruppe, die Sie im Schnellstart [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md) angegeben haben.|In dieser Ressourcengruppe muss das VNET enthalten sein.|
   | **Location** | Der Standort für die Ressourcengruppe | Dieser Wert wird basierend auf der ausgewählten Ressourcengruppe ausgefüllt. | 
   | **Name des virtuellen Computers**  | Ein gültiger Name | Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Benutzername des Administrators**|Beliebiger gültiger Benutzername|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Verwenden Sie nicht „serveradmin“. Hierbei handelt es sich um eine reservierte Rolle auf Serverebene.| 
   |**Kennwort**|Ein gültiges Kennwort|Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
   | **Größe des virtuellen Computers** | Eine beliebige gültige Größe | Für diesen Schnellstart genügt der Standardwert in dieser Vorlage von **Standard_B2s. |
   | **Location**|[resourceGroup().location].| Ändern Sie diesen Wert nicht. |
   | **Name des virtuellen Netzwerks**|Der zuvor ausgewählte Standort|Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).|
   | **Subnetzname**|Der Name des Subnetzes, das Sie im vorherigen Verfahren erstellt haben| Wählen Sie nicht das Subnetz aus, in dem Sie die verwaltete Instanz erstellt haben.|
   | **Artefaktspeicherort** | [deployment().properties.templateLink.uri]  Ändern Sie diesen Wert nicht. |
   | **SAS-Token des Artefaktspeicherorts** | Lassen Sie dieses Feld leer. | Ändern Sie diesen Wert nicht. |

   ![Erstellen des virtuellen Clientcomputers](./media/sql-database-managed-instance-configure-vm/create-client-sql-vm.png)

   Wenn Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md) den vorgeschlagenen VNET-Namen und das Standardsubnetz verwendet haben, müssen Sie die beiden letzten Parameter nicht ändern. Andernfalls müssen Sie diese Werte in die Werte ändern, die Sie beim Einrichten der Netzwerkumgebung eingegeben haben.

3. Aktivieren Sie das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
4. Klicken Sie auf **Kaufen**, um die Azure-VM in Ihrem Netzwerk bereitzustellen.
5. Klicken Sie auf das Symbol **Benachrichtigungen**, um den Status der Bereitstellung anzuzeigen.
   
   Fahren Sie erst fort, wenn die Azure-VM erstellt wurde. 

## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

In den folgenden Schritten wird veranschaulicht, wie Sie für Ihren neu erstellten virtuellen Computer eine Remotedesktopverbindung herstellen.

1. Navigieren Sie zur VM-Ressource, nachdem die Bereitstellung abgeschlossen wurde.

    ![VM](./media/sql-database-managed-instance-configure-vm/vm.png)  

2. Klicken Sie auf **Verbinden**. 
   
   Eine Formular für eine RDP-Datei (Remotedesktopprotokoll) wird mit der öffentlichen IP-Adresse und der Portnummer für den virtuellen Computer angezeigt. 

   ![RDP-Formular](./media/sql-database-managed-instance-configure-vm/rdp.png)  

3. Klicken Sie auf **RDP-Datei herunterladen**.
 
   > [!NOTE]
   > Sie können für die Verbindung mit Ihrem virtuellen Computer auch SSH verwenden.

4. Schließen Sie das Formular **Mit virtuellem Computer verbinden**.
5. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. 
6. Klicken Sie in der angezeigten Aufforderung auf **Verbinden**. Auf einem Macintosh benötigen Sie einen RDP-Client, z.B. diesen [Remotedesktopclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) aus dem Mac App Store.

6. Geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie beim Erstellen des virtuellen Computers festgelegt haben, und klicken Sie anschließend auf **OK**.

7. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

Im Server-Manager-Dashboard wird eine Verbindung mit Ihrem virtuellen Computer hergestellt.

## <a name="use-ssms-to-connect-to-the-managed-instance"></a>Verwenden von SSMS zum Verbinden mit der verwalteten Instanz

1. Öffnen Sie SQL Server Management Studio (SSMS) auf dem virtuellen Computer.
 
   Das Öffnen dauert einen Moment, weil die Konfiguration bei diesem ersten Start von SSMS abgeschlossen wird.
2. Geben Sie im Dialogfeld **Mit Server verbinden** im Feld **Servername** den vollqualifizierten **Hostnamen** für Ihre verwaltete Instanz ein, wählen Sie **SQL Server-Authentifizierung** aus, geben Sie Benutzername und Kennwort ein, und klicken Sie dann auf **Verbinden**.

    ![SSMS-Verbindung](./media/sql-database-managed-instance-configure-vm/ssms-connect.png)  

Nach der Verbindungsherstellung können Sie Ihre System- und Benutzerdatenbanken auf dem Knoten „Datenbanken“ und verschiedene Objekte auf den Knoten „Sicherheit“, „Serverobjekte, „Replikation“, „Verwaltung“, „SQL Server-Agent“ und „XEvent Profiler“ anzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Schnellstartanleitung, die zeigt, wie Sie von einem lokalen Clientcomputer über eine Point-to-Site-Verbindung eine Verbindung herstellen, finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung](sql-database-managed-instance-configure-p2s.md).
- Eine Übersicht über die Verbindungsoptionen für Anwendungen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connect-app.md).
- Zur Wiederherstellung einer vorhandenen SQL Server-Datenbank von einer lokalen in eine verwaltete Instanz können Sie entweder [Azure Database Migration Service (DMS) für die Migration](../dms/tutorial-sql-server-to-managed-instance.md) oder den [T-SQL-Befehl „RESTORE“](sql-database-managed-instance-get-started-restore.md) verwenden, um die Wiederherstellung von einer Datenbanksicherungsdatei durchzuführen.
