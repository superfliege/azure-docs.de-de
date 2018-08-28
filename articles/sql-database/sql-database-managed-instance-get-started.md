---
title: 'Azure-Portal: Erstellen einer verwalteten SQL-Instanz | Microsoft-Dokumentation'
description: Erstellen einer verwalteten SQL-Instanz, einer Netzwerkumgebung und eines virtuellen Clientcomputers für den Zugriff
keywords: Tutorial zu SQL-Datenbank, Erstellen einer verwalteten SQL-Instanz
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: quickstart
ms.date: 08/13/2018
ms.author: jovanpop-msft
ms.openlocfilehash: cb378c2d2773096992ef688653fd77b2625f8754
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42023822"
---
# <a name="create-an-azure-sql-managed-instance"></a>Erstellen einer verwalteten Azure SQL-Instanz

In diesem Schnellstart wird erläutert, wie Sie eine verwaltete SQL-Instanz in Azure erstellen. Die verwaltete Azure SQL-Datenbank-Instanz ist eine SQL Server-Datenbank-Engine-PaaS-Instanz (Platform-as-a-Service), mit der Sie hoch verfügbare SQL Server-Datenbanken in der Azure-Cloud ausführen und skalieren können. In dieser Schnellstartanleitung wird gezeigt, wie Sie zum Einstieg eine verwaltete SQL-Instanz erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="prepare-network-environment"></a>Vorbereiten der Netzwerkumgebung

Die verwaltete SQL-Instanz ist ein sicherer Dienst, der sich in Ihrer eigenen Azure Virtual Network-Instanz (VNET) befindet. Zum Erstellen einer verwalteten müssen Sie die Azure-Netzwerkumgebung mit folgenden Komponenten vorbereiten:
 - Azure-VNET, in dem sich Ihre verwaltete Instanz befindet
 - Subnetz im Azure-VNET, in dem sich die verwaltete Instanz befindet
 - Benutzerdefinierte Route, die der verwalteten Instanz die Kommunikation mit den Azure-Diensten ermöglicht, die sie steuern und verwalten

Das Subnetz wird ausschließlich für verwaltete Instanzen verwendet, und Sie können keine anderen Ressourcen (z.B. Azure Virtual Machines-Instanzen) in diesem Subnetz erstellen. In diesem Schnellstart werden zwei Subnetze in Ihrem Azure-VNET erstellt, sodass Sie verwaltete Instanzen im dedizierten Subnetz für verwaltete Instanzen und andere Ressourcen im Standardsubnetz platzieren können.

1. Stellen Sie eine für die verwaltete Azure SQL-Datenbank-Instanz vorbereitete Azure-Netzwerkumgebung bereit, indem Sie auf die folgende Schaltfläche klicken:

    <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener"> <img src="http://azuredeploy.net/deploybutton.png"> </a>

    Mit dieser Schaltfläche wird im Azure-Portal ein Formular geöffnet, in dem Sie Ihre Netzwerkumgebung vor der Bereitstellung konfigurieren können.

2. Sie haben die Option, die Namen des VNETs und der Subnetze zu ändern und die Ihren Netzwerkressourcen zugeordneten IP-Adressbereiche anzupassen. Klicken Sie dann auf die Schaltfläche „Kaufen“, um Ihre Umgebung zu erstellen und zu konfigurieren:

    ![Erstellen einer Umgebung für die verwaltete Instanz](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

    > [!Note]
    > Diese Azure Resource Manager-Bereitstellung erstellt zwei Subnetze im VNET, eines für verwaltete Instanzen mit dem Namen **ManagedInstances**, und eine weiteres mit dem Namen **Default** für andere Ressourcen wie virtuelle Clientcomputer, die zum Herstellen der Verbindung mit der verwalteten Instanz verwendet werden können. Wenn Sie keine zwei Subnetze benötigen, können Sie später das Standard-Subnetz löschen. Allerdings können Sie dann Schritt 3 in diesem Schnellstarthandbuch – [Vorbereiten des Clientcomputers](#prepare-client-machine) – nicht durchführen.

    > [!Note]
    > Wenn Sie die Namen der VNETs und Subnetze ändern, achten Sie darauf, sich die neuen Namen zu merken, da sie in den folgenden Abschnitten benötigt werden. Im weiteren Verlauf des Tutorials wird angenommen, dass Sie ein VNET namens **MyNewVNet**, das Subnetz **ManagedInstances** für verwaltete SQL-Instanzen und das Subnetz **Default** für virtuelle Computer und andere Ressourcen erstellt haben.

## <a name="create-a-managed-instance"></a>Erstellen einer verwalteten Instanz

In den folgenden Schritten wird veranschaulicht, wie Sie Ihre verwaltete Instanz erstellen, nachdem die Vorschauversion genehmigt wurde.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Suchen Sie nach **Verwaltete Instanz**, und wählen Sie dann die Option **Verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion)**.
3. Klicken Sie auf **Create**.

   ![Erstellen einer verwalteten Instanz](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

4. Wählen Sie Ihr Abonnement aus, und stellen Sie sicher, dass für die Nutzungsbedingungen für die Vorschau **Akzeptiert** angezeigt wird.

   ![Verwaltete Instanz – Vorschauversion akzeptiert](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

5. Füllen Sie das Formular für die verwaltete Instanz mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | BESCHREIBUNG |
   | ------ | --------------- | ----------- |
   |**Name der verwalteten Instanz**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Administratoranmeldung für verwaltete Instanz**|Beliebiger gültiger Benutzername|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Verwenden Sie nicht „serveradmin“. Hierbei handelt es sich um eine reservierte Rolle auf Serverebene.| 
   |**Kennwort**|Ein gültiges Kennwort|Das Kennwort muss mindestens 16 Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
   |**Ressourcengruppe**|Die zuvor erstellte Ressourcengruppe||
   |**Location**|Der zuvor ausgewählte Standort|Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).|
   |**Virtuelles Netzwerk**|Das zuvor erstellte virtuelle Netzwerk| Wählen Sie das Element **MyNewVNet/ManagedInstances** aus, wenn Sie die Namen im vorherigen Schritt nicht geändert haben. Wählen Sie andernfalls den VNET-Namen und das Subnetz für die verwaltete Instanz aus, die Sie im vorherigen Abschnitt eingegeben haben. **Verwenden Sie nicht das Standardsubnetz, da es nicht als Host für verwaltete Instanzen konfiguriert ist**. |

   ![Formular für die Erstellung der verwalteten Instanz](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

6. Klicken Sie auf **Tarif**, um die Größe der Compute- und Speicherressourcen festzulegen und die Tarifoptionen zu prüfen. Standardmäßig werden für Ihre Instanz 32 GB an kostenlosem Speicherplatz festgelegt. **Dies reicht für Ihre Anwendungen unter Umständen nicht aus**.
7. Verwenden Sie die Schieberegler oder Textfelder, um die Speichermenge und die Anzahl von virtuellen Kernen anzugeben. 
   ![Tarif der verwalteten Instanz](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

8. Klicken Sie nach Abschluss des Vorgangs auf **Übernehmen**, um Ihre Auswahl zu speichern.  
9. Klicken Sie auf **Erstellen**, um die verwaltete Instanz bereitzustellen.
10. Klicken Sie auf das Symbol **Benachrichtigungen**, um den Status der Bereitstellung anzuzeigen.
 
   ![Bereitstellungsstatus](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

11. Klicken Sie auf **Die Bereitstellung wird ausgeführt**, um das Fenster für die verwaltete Instanz zu öffnen und den Bereitstellungsstatus weiter zu verfolgen.
 
   ![Bereitstellungsstatus 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Fahren Sie während der Bereitstellung mit den nächsten Schritten fort.

> [!IMPORTANT]
> Für die erste Instanz in einem Subnetz ist die Bereitstellungsdauer normalerweise deutlich länger als bei den nachfolgenden Instanzen (in einigen Fällen mehr als 24 Stunden). Brechen Sie den Bereitstellungsvorgang nicht ab, weil er länger als erwartet dauert. Diese Dauer für die Bereitstellung Ihrer ersten Instanz ist eine vorübergehende Situation. Sie können damit rechnen, dass sich die Bereitstellungsdauer kurz nach Beginn der öffentlichen Vorschauversion deutlich reduziert. Das Erstellen der zweiten verwalteten Instanz im Subnetz dauert einige Minuten.

## <a name="prepare-client-machine"></a>Vorbereiten des Clientcomputers

Da sich die verwaltete SQL-Instanz in Ihrer privaten Virtual Network-Instanz befindet, müssen Sie einen virtuellen Azure-Computer erstellen, auf dem ein SQL-Clienttool wie SQL Server Management Studio oder SQL Operations Studio installiert ist, um eine Verbindung mit der verwalteten Instanz herzustellen und Abfragen auszuführen.

> [!Note]
> Anstelle eines virtuellen Azure-Clientcomputers können Sie auch eine [Point-to-Site](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)-Verbindung konfigurieren und auf Ihrem lokalen Computer eine Verbindung mit der verwalteten Instanz herstellen.

Die einfachste Möglichkeit zum Erstellen eines virtuellen Clientcomputers mit allen erforderlichen Tools ist die Verwendung von Azure Resource Manager-Vorlagen.

1. Klicken Sie auf die folgende Schaltfläche (stellen Sie auf einer anderen Browserregisterkarte sicher, dass Sie bei Azure-Portal angemeldet sind):

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Geben Sie in das Formular, das geöffnet wird, den Namen des virtuellen Computers, den Benutzernamen des Administrators und das Kennwort ein, die Sie für die Verbindung mit dem virtuellen Computer verwenden möchten.

    ![Erstellen des virtuellen Clientcomputers](./media/sql-database-managed-instance-get-started/create-client-sql-vm.png)

    Wenn Sie den VNET-Namen und das Standardsubnetz nicht geändert haben, müssen Sie die beiden letzten Parameter nicht ändern. Andernfalls müssen Sie diese Werte in die Werte ändern, die Sie beim Einrichten der Netzwerkumgebung eingegeben haben.

3. Klicken Sie auf die Schaltfläche „Kaufen“, und der virtuelle Azure-Computer wird im vorbereiteten Netzwerk bereitgestellt.

4. Stellen Sie eine Remotedesktopverbindung mit Ihrem virtuellen Computer her, und suchen Sie SQL Server Management Studio oder SQL Operation Studio, die automatisch auf dem virtuellen Computer installiert sind.

5. Öffnen Sie SSMS, und geben Sie den **Hostnamen** für Ihre verwaltete Instanz im Feld **Servername** ein, wählen Sie **SQL Server-Authentifizierung** aus, geben Sie Benutzername und Kennwort im Feld **Mit Server verbinden** ein, und klicken Sie dann auf **Verbinden**.

    ![SSMS-Verbindung](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Nach der Verbindungsherstellung können Sie Ihre System- und Benutzerdatenbanken auf dem Knoten „Datenbanken“ und verschiedene Objekte auf den Knoten „Sicherheit“, „Serverobjekte, „Replikation“, „Verwaltung“, „SQL Server-Agent“ und „XEvent Profiler“ anzeigen.

## <a name="next-steps"></a>Nächste Schritte

 - [Verbinden Sie Ihre Anwendungen mit der verwalteten Instanz](sql-database-managed-instance-connect-app.md).
 - [Migrieren Sie Ihre Datenbanken vom lokalen Standort zur verwalteten Instanz](sql-database-managed-instance-migrate.md).


