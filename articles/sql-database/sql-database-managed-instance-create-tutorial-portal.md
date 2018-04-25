---
title: 'Azure-Portal: Erstellen einer verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation'
description: Erstellen Sie eine verwaltete Azure SQL-Datenbank-Instanz in einem VNet.
keywords: Tutorial zu SQL-Datenbank, Erstellen einer verwalteten SQL-Datenbank-Instanz
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 04/10/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: 1e04b12241121d3894e2d71df423bae4f000a60d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="create-an-azure-sql-database-managed-instance-in-the-azure-portal"></a>Erstellen einer verwalteten Azure SQL-Datenbank-Instanz im Azure-Portal

In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal eine verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion) in einem dedizierten Subnetz eines virtuellen Netzwerks (VNet) erstellen und anschließend unter Verwendung von SQL Server Management Studio auf einem virtuellen Computer im gleichen VNet eine Verbindung mit der verwalteten Instanz herstellen.

> [!div class="checklist"]
> * Whitelist Ihres Abonnements
> * Konfigurieren eines virtuellen Netzwerks (VNET)
> * Erstellen einer neuen Routingtabelle und einer Route
> * Anwenden der Routingtabelle auf das Subnetz für die verwaltete Instanz
> * Erstellen einer verwalteten Instanz
> * Erstellen eines neuen Subnetzes im VNET für einen virtuellen Computer
> * Erstellen eines virtuellen Computers im neuen Subnetz im VNET
> * Herstellen der Verbindung mit dem virtuellen Computer
> * Installieren von SSMS und Herstellen einer Verbindung mit der verwalteten Instanz


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

> [!IMPORTANT]
> Eine Liste mit den Regionen, in denen die verwaltete Instanz derzeit verfügbar ist, finden Sie unter [Migrieren Ihrer Datenbanken zu einem vollständig verwalteten Dienst mit einer verwalteten Instanz von Azure SQL-Datenbank](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/#create/Microsoft.SQLManagedInstance)an.

## <a name="whitelist-your-subscription"></a>Whitelist Ihres Abonnements

Die verwaltete SQL-Datenbank-Instanz wird anfänglich als geschlossene öffentliche Vorschauversion veröffentlicht, für die Ihr Abonnement auf die Whitelist gesetzt werden muss. Falls Ihr Abonnement nicht bereits auf die Whitelist gesetzt wurde, können Sie die folgenden Schritte verwenden, damit die Nutzungsbedingungen für die Vorschauversion angezeigt und akzeptiert werden können und eine Anforderung für das Setzen auf die Whitelist gesendet wird.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Suchen Sie nach **Verwaltete Instanz**, und wählen Sie dann die Option **Verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion)**.
3. Klicken Sie auf **Create**.

   ![Verwaltete Instanz – Erstellen](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Wählen Sie Ihr Abonnement aus, klicken Sie auf **Nutzungsbedingungen für Vorschau**, und geben Sie dann die angeforderten Informationen an.

   ![Verwaltete Instanz – Nutzungsbedingungen für Vorschau](./media/sql-database-managed-instance-tutorial/preview-terms.png)

5. Klicken Sie auf **OK**, wenn Sie fertig sind.

   ![Verwaltete Instanz – Nutzungsbedingungen für Vorschau](./media/sql-database-managed-instance-tutorial/preview-approval-pending.png)

   > [!NOTE]
   > Während Sie auf die Genehmigung für die Vorschauversion warten, können Sie weiterarbeiten und die nächsten Abschnitte dieses Tutorials durcharbeiten.

## <a name="configure-a-virtual-network-vnet"></a>Konfigurieren eines virtuellen Netzwerks (VNET)

In den folgenden Schritten wird gezeigt, wie Sie ein neues [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md)-VNET zur Verwendung durch Ihre verwaltete Azure SQL-Datenbank-Instanz erstellen. Weitere Informationen zur VNET-Konfiguration finden Sie unter [Configure a VNet for Azure SQL Database Managed Instance](sql-database-managed-instance-vnet-configuration.md) (Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz).

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Klicken Sie auf die Option **Virtuelles Netzwerk**, vergewissern Sie sich, dass der **Resource Manager** als Bereitstellungsmodus ausgewählt ist, und klicken Sie dann auf **Erstellen**.

   ![Virtuelles Netzwerk – Erstellen](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Füllen Sie das Formular für das virtuelle Netzwerk mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | Beschreibung |
   | ------ | --------------- | ----------- |
   |**Name**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Adressraum**|Beliebiger gültiger Adressbereich, z.B. 10.14.0.0/24.|Der Adressname des virtuellen Netzwerks in CIDR-Notation.|
   |**Abonnement**|Ihr Abonnement|Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions).|
   |**Ressourcengruppe**|Beliebige gültige Ressourcengruppe (neu oder bereits vorhanden)|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Location**|Gültiger Standort| Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).|
   |**Subnetzname**|Beliebiger gültiger Subnetzname, z.B. „mi_subnet“.|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Subnetzadressbereich**|Beliebige gültige Subnetzadresse, z.B. 10.14.0.0/28. Verwenden Sie einen Subnetzadressraum, der kleiner als der eigentliche Adressraum ist. Auf diese Weise ist Platz für die Erstellung weiterer Subnetze in demselben VNET vorhanden, z.B. ein Subnetz zum Hosten von Test-/-Client-Apps oder Gatewaysubnetze zum Herstellen von Verbindungen aus der lokalen Umgebung oder anderen VNETs.|Der Adressbereich des Subnetzes in CIDR-Notation. Er muss innerhalb des Adressraums des virtuellen Netzwerks liegen.|
   |**Dienstendpunkte**|Deaktiviert|Aktivieren Sie mindestens einen Endpunkt für das Subnetz.|
   ||||

   ![Formular für die Erstellung des virtuellen Netzwerks](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Klicken Sie auf **Create**.

## <a name="create-new-route-table-and-a-route"></a>Erstellen einer neuen Routingtabelle und einer Route

In den folgenden Schritten wird veranschaulicht, wie Sie eine Route des folgenden Typs erstellen: 0.0.0.0/0, nächster Hop „Internet“.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Klicken Sie auf **Routingtabelle** und dann auf der Seite „Routingtabelle“ auf **Erstellen**. 

   ![Routingtabelle – Erstellen](./media/sql-database-managed-instance-tutorial/route-table-create.png)

3. Füllen Sie das Formular für die Routingtabelle mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | Beschreibung |
   | ------ | --------------- | ----------- |
   |**Name**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Abonnement**|Ihr Abonnement|Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions).|
   |**Ressourcengruppe**|Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben.|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Location**|Wählen Sie den Standort aus, den Sie im vorherigen Abschnitt angegeben haben.| Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).|
   |**BGP-Routenverteilung deaktivieren**|Deaktiviert||
   ||||

   ![Formular für die Erstellung einer Routingtabelle](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

4. Klicken Sie auf **Create**.
5. Öffnen Sie nach der Erstellung der Routingtabelle die neu erstellte Routingtabelle.

   ![Routingtabelle](./media/sql-database-managed-instance-tutorial/route-table.png)

6. Klicken Sie auf **Routen** und dann auf **Hinzufügen**.

   ![Routingtabelle – Hinzufügen](./media/sql-database-managed-instance-tutorial/route-table-add.png)

7.  Fügen Sie **0.0.0.0/0 Next Hop Internet route** (Route vom Typ 0.0.0.0/0, nächster Hop „Internet“) als **einzige** Route hinzu, indem Sie die Informationen in der folgenden Tabelle verwenden:

    | Einstellung| Empfohlener Wert | BESCHREIBUNG |
    | ------ | --------------- | ----------- |
    |**Routenname**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
    |**Adresspräfix**|0.0.0.0/0|Die IP-Zieladresse in CIDR-Notation, für die diese Route gilt.|
    |**Typ des nächsten Hops**|Internet|Der nächste Hop verarbeitet die passenden Pakete für diese Route.|
    |||

    ![Route](./media/sql-database-managed-instance-tutorial/route.png)

8. Klicken Sie auf **OK**.

## <a name="apply-the-route-table-to-the-managed-instance-subnet"></a>Anwenden der Routingtabelle auf das Subnetz für die verwaltete Instanz

In den folgenden Schritten wird veranschaulicht, wie Sie die neue Routingtabelle im Subnetz für die verwaltete Instanz festlegen.

1. Öffnen Sie das zuvor erstellte virtuelle Netzwerk, um die Routingtabelle im Subnetz für die verwaltete Instanz festzulegen.
2. Klicken Sie auf **Subnetze** und dann auf das Subnetz für die verwaltete Instanz (**mi_subnet** im folgenden Screenshot).

    ![Subnetz](./media/sql-database-managed-instance-tutorial/subnet.png)

11. Klicken Sie auf **Routingtabelle**, und wählen Sie **myMI_route_table** aus.

    ![Festlegen der Routingtabelle](./media/sql-database-managed-instance-tutorial/set-route-table.png)

12. Klicken Sie unten auf der Seite auf **Speichern**.

    ![Festlegen der Routingtabelle – Speichern](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)

## <a name="create-a-managed-instance"></a>Erstellen einer verwalteten Instanz

In den folgenden Schritten wird veranschaulicht, wie Sie Ihre verwaltete Instanz erstellen, nachdem die Vorschauversion genehmigt wurde.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Suchen Sie nach **Verwaltete Instanz**, und wählen Sie dann die Option **Verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion)**.
3. Klicken Sie auf **Create**.

   ![Verwaltete Instanz – Erstellen](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Wählen Sie Ihr Abonnement aus, und stellen Sie sicher, dass für die Nutzungsbedingungen für die Vorschau **Akzeptiert** angezeigt wird.

   ![Verwaltete Instanz – Vorschauversion akzeptiert](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

4. Füllen Sie das Formular für die verwaltete Instanz mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | BESCHREIBUNG |
   | ------ | --------------- | ----------- |
   |**Name der verwalteten Instanz**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Administratoranmeldung für verwaltete Instanz**|Beliebiger gültiger Benutzername|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).| 
   |**Kennwort**|Ein gültiges Kennwort|Das Kennwort muss mindestens 16 Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
   |**Ressourcengruppe**|Die zuvor erstellte Ressourcengruppe||
   |**Location**|Der zuvor ausgewählte Standort|Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).|
   |**Virtuelles Netzwerk**|Das zuvor erstellte virtuelle Netzwerk|

   ![Formular für die Erstellung der verwalteten Instanz](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

5. Klicken Sie auf **Tarif**, um die Größe der Compute- und Speicherressourcen festzulegen und die Tarifoptionen zu prüfen. Standardmäßig werden für Ihre Instanz 32 GB an kostenlosem Speicherplatz festgelegt. Dies reicht für Ihre Anwendungen unter Umständen nicht aus.
6. Verwenden Sie die Schieberegler oder Textfelder, um die Speichermenge und die Anzahl von virtuellen Kernen anzugeben. 
   ![Formular für die Erstellung der verwalteten Instanz](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

7. Klicken Sie nach Abschluss des Vorgangs auf **Übernehmen**, um Ihre Auswahl zu speichern.  
8. Klicken Sie auf **Erstellen**, um die verwaltete Instanz bereitzustellen.
9. Klicken Sie auf das Symbol **Benachrichtigungen**, um den Status der Bereitstellung anzuzeigen.
 
   ![Bereitstellungsstatus](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

9. Klicken Sie auf **Die Bereitstellung wird ausgeführt**, um das Fenster für die verwaltete Instanz zu öffnen und den Bereitstellungsstatus weiter zu verfolgen.
 
   ![Bereitstellungsstatus 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Fahren Sie während der Bereitstellung mit den nächsten Schritten fort.

> [!IMPORTANT]
> Für die erste Instanz in einem Subnetz ist die Bereitstellungsdauer normalerweise deutlich länger als bei den nachfolgenden Instanzen (in einigen Fällen mehr als 24 Stunden). Brechen Sie den Bereitstellungsvorgang nicht ab, weil er länger als erwartet dauert. Diese Dauer für die Bereitstellung Ihrer ersten Instanz ist eine vorübergehende Situation. Sie können damit rechnen, dass sich die Bereitstellungsdauer kurz nach Beginn der öffentlichen Vorschauversion deutlich reduziert.

## <a name="create-a-new-subnet-in-the-vnet-for-a-virtual-machine"></a>Erstellen eines neuen Subnetzes im VNET für einen virtuellen Computer

In den folgenden Schritten wird veranschaulicht, wie Sie ein zweites Subnetz im VNET für einen virtuellen Computer erstellen, in dem Sie SQL Server Management Studio installieren und eine Verbindung mit Ihrer verwalteten Instanz herstellen.

1. Öffnen Sie Ihre VNET-Ressource.
 
   ![VNet](./media/sql-database-managed-instance-tutorial/vnet.png)

2. Klicken Sie auf **Subnetze** und dann auf **+Subnetz**.
 
   ![Hinzufügen des Subnetzes](./media/sql-database-managed-instance-tutorial/add-subnet.png)

3. Füllen Sie das Formular für das Subnetz mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | Beschreibung |
   | ------ | --------------- | ----------- |
   |**Name**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Adressbereich (CIDR-Block)**|Beliebiger gültiger Adressbereich innerhalb des VNET (Standardeinstellung verwenden)||
   |**Netzwerksicherheitsgruppe**|Keine||
   |**Routingtabelle**|Keine||
   |**Dienstendpunkte**|Keine||

   ![Details zum VM-Subnetz](./media/sql-database-managed-instance-tutorial/vm-subnet-details.png)

4. Klicken Sie auf **OK**.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Erstellen eines virtuellen Computers im neuen Subnetz im VNET

In den folgenden Schritten wird veranschaulicht, wie Sie einen virtuellen Computer in demselben VNET erstellen, in dem die verwaltete Instanz erstellt wird. 

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter** oder **Windows 10**. In diesem Abschnitt des Tutorials wird Windows Server verwendet. Die Konfiguration von Windows 10 ist sehr ähnlich. 

   ![Compute](./media/sql-database-managed-instance-tutorial/compute.png)

3. Füllen Sie das Formular für den virtuellen Computer mit den geforderten Informationen aus, indem Sie die Angaben in der folgenden Tabelle verwenden:

   | Einstellung| Empfohlener Wert | Beschreibung |
   | ------ | --------------- | ----------- |
   |**Name**|Ein gültiger Name|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   | **VM-Datenträgertyp**|SSD|SSDs bieten das beste Preis-Leistungs-Verhältnis.|   
   |**Benutzername**|Beliebiger gültiger Benutzername|Gültige Namen finden Sie unter [Benennungskonventionen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).| 
   |**Kennwort**|Ein gültiges Kennwort|Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.| 
   |**Abonnement**|Ihr Abonnement|Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions).|
   |**Ressourcengruppe**|Die zuvor erstellte Ressourcengruppe||
   |**Location**|Der zuvor ausgewählte Standort||
   |**Already have a Windows license** (Windows-Lizenz bereits vorhanden)|Nein |Wenn Sie über Windows-Lizenzen mit aktiver Software Assurance (SA) verfügen, können Sie mit dem Azure-Hybridvorteil Computekosten sparen.|
   ||||

   ![Formular für die Erstellung des virtuellen Computers](./media/sql-database-managed-instance-tutorial/virtual-machine-create-form.png)

3. Klicken Sie auf **OK**.
4. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Für dieses Tutorial benötigen Sie nur einen kleinen virtuellen Computer.

    ![VM-Größen](./media/sql-database-managed-instance-tutorial/virtual-machine-size.png)  

5. Klicken Sie auf **Auswählen**.
6. Klicken Sie im Formular **Einstellungen** auf **Subnetz**, und wählen Sie **vm_subnet**. Wählen Sie nicht das Subnetz aus, in dem die verwaltete Instanz bereitgestellt wurde, sondern ein anderes Subnetz in demselben VNET.

    ![VM-Einstellungen](./media/sql-database-managed-instance-tutorial/virtual-machine-settings.png)  

7. Klicken Sie auf **OK**.
8. Sehen Sie sich auf der Seite mit der Zusammenfassung die Angebotsdetails an, und klicken Sie dann auf **Erstellen**, um mit der Bereitstellung des virtuellen Computers zu beginnen.
 
## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

In den folgenden Schritten wird veranschaulicht, wie Sie für Ihren neu erstellten virtuellen Computer eine Remotedesktopverbindung herstellen.

1. Navigieren Sie zur VM-Ressource, nachdem die Bereitstellung abgeschlossen wurde.

    ![VM](./media/sql-database-managed-instance-tutorial/vm.png)  

2. Klicken Sie in den Eigenschaften des virtuellen Computers auf die Schaltfläche **Verbinden**. Eine Remotedesktopprotokoll-Datei (RDP-Datei) wird erstellt und heruntergeladen.
3. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. 
4. Klicken Sie in der angezeigten Aufforderung auf **Verbinden**. Auf einem Macintosh benötigen Sie einen RDP-Client, z.B. diesen [Remotedesktopclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) aus dem Mac App Store.

5. Geben Sie den Benutzernamen und das Kennwort ein, den bzw. das Sie beim Erstellen des virtuellen Computers festgelegt haben, und klicken Sie anschließend auf **OK**.

6. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

Im Server-Manager-Dashboard wird eine Verbindung mit Ihrem virtuellen Computer hergestellt.

> [!IMPORTANT]
> Fahren Sie erst fort, nachdem die Bereitstellung Ihrer verwalteten Instanz erfolgreich abgeschlossen wurde. Rufen Sie den Hostnamen für Ihre Instanz nach Abschluss der Bereitstellung auf der Registerkarte **Übersicht** Ihrer verwalteten Instanz im Feld **Verwaltete Instanz** ab. Der Name hat in etwa das folgende Format: **drfadfadsfd.tr23.westus1-a.worker.database.windows.net**.

## <a name="install-ssms-and-connect-to-the-managed-instance"></a>Installieren von SSMS und Herstellen einer Verbindung mit der verwalteten Instanz

In den folgenden Schritten wird veranschaulicht, wie Sie SSMS herunterladen und installieren und anschließend eine Verbindung mit Ihrer verwalteten Instanz herstellen.

1. Klicken Sie im Server-Manager im linken Bereich auf **Lokaler Server**.

    ![Server-Manager-Eigenschaften](./media/sql-database-managed-instance-tutorial/server-manager-properties.png)  

2. Klicken Sie im Bereich **Eigenschaften** auf **Ein**, um die verstärkte Sicherheitskonfiguration für IE zu ändern.
3. Klicken Sie im Dialogfeld **Verstärkte Sicherheitskonfiguration für Internet Explorer** im Abschnitt „Administratoren“ des Dialogfelds auf **Aus** und dann auf **OK**.

    ![Verstärkte Sicherheitskonfiguration für Internet Explorer](./media/sql-database-managed-instance-tutorial/internet-explorer-security-configuration.png)  
4. Öffnen Sie über die Taskleiste **Internet Explorer**.
5. Wählen Sie die Option **Use recommended security and compatibility settings** (Empfohlene Sicherheits- und Kompatibilitätseinstellungen verwenden), und klicken Sie anschließend auf **OK**, um das Setup von Internet Explorer 11 abzuschließen.
6. Geben Sie https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms in das Adressfeld für die URL ein, und drücken Sie die**EINGABETASTE**. 
7. Laden Sie die aktuelle Version von SQL Server Management Studio herunter, und klicken Sie auf **Ausführen**, wenn Sie dazu aufgefordert werden.
8. Klicken Sie in der Aufforderung auf **Installieren**, um den Vorgang zu starten.
9. Klicken Sie nach Abschluss der Installation auf **Schließen**.
10. Öffnen Sie SSMS.
11. Geben Sie im Dialogfeld **Mit Server verbinden** den *Hostnamen* für Ihre verwaltete Instanz im Feld **Servername** ein, wählen Sie **SQL Server-Authentifizierung**, geben Sie Benutzername und Kennwort ein, und klicken Sie dann auf **Verbinden**.

    ![SSMS-Verbindung](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Nach der Verbindungsherstellung können Sie Ihre System- und Benutzerdatenbanken auf dem Knoten „Datenbanken“ und verschiedene Objekte auf den Knoten „Sicherheit“, „Serverobjekte, „Replikation“, „Verwaltung“, „SQL Server-Agent“ und „XEvent Profiler“ anzeigen.

> [!NOTE]
> Wenn Sie eine vorhandene SQL-­Datenbank in einer verwalteten Instanz wiederherstellen möchten, können Sie den [Azure Database Migration Service (DMS) für die Migration](../dms/tutorial-sql-server-to-managed-instance.md) oder den [T-SQL-Befehl „RESTORE“](sql-database-managed-instance-restore-from-backup-tutorial.md) verwenden, um eine Datenbanksicherungsdatei wiederherzustellen, oder eine [BACPAC-Datei importieren](sql-database-import.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie über das Azure-Portal eine verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion) in einem dedizierten Subnetz eines virtuellen Netzwerks (VNet) erstellen und anschließend unter Verwendung von SQL Server Management Studio auf einem virtuellen Computer im gleichen VNet eine Verbindung mit der verwalteten Instanz herstellen.  Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> * Whitelist Ihres Abonnements
> * Konfigurieren eines virtuellen Netzwerks (VNET)
> * Erstellen einer neuen Routingtabelle und einer Route
> * Anwenden der Routingtabelle auf das Subnetz für die verwaltete Instanz
> * Erstellen einer verwalteten Instanz
> * Erstellen eines neuen Subnetzes im VNET für einen virtuellen Computer
> * Erstellen eines virtuellen Computers im neuen Subnetz im VNET
> * Herstellen der Verbindung mit dem virtuellen Computer
> * Installieren von SSMS und Herstellen einer Verbindung mit der verwalteten Instanz

Im nächsten Tutorial erfahren Sie, wie Sie eine Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz wiederherstellen.

> [!div class="nextstepaction"]
>[Wiederherstellen einer Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-restore-from-backup-tutorial.md)
