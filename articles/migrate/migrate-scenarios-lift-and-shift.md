---
title: Migrieren von lokalen Workloads mit Azure DMS und Site Recovery zu Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure mit Azure Database Migration Service und dem Azure Site Recovery-Dienst für die Migration von lokalen Computern vorbereiten.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/21/2018
ms.author: raynew
ms.openlocfilehash: d6fc1af3c5a587ab62b86dcd4189165d96e6e3bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182240"
---
# <a name="scenario-2-lift-and-shift-migration-to-azure"></a>Szenario 2: Lift & Shift-Migration zu Azure

Das Unternehmen Contoso zieht eine Migration zu Azure in Betracht. Um dies zu testen, soll auf eine kleine lokale Reise-App zugegriffen und diese zu Azure migriert werden. Es handelt sich um eine Reise-App mit zwei Ebenen, wobei eine Web-App auf der einen VM und eine SQL Server-Datenbank auf der zweiten VM ausgeführt wird. Die Anwendung wird in VMware bereitgestellt, und die Umgebung wird mit einem vCenter Server verwaltet. 

In [Szenario 1: Bewerten der Migration zu Azure](migrate-scenarios-assessment.md) wurde der Data Migration Assistant (DMA) verwendet, um die SQL Server-Datenbank für die App zu bewerten. Darüber hinaus wurde der Azure Migrate-Dienst zum Bewerten der App-VMs verwendet. Nach der erfolgreichen Bewertung sollen jetzt in diesem Szenario die Datenbank mit Azure Database Migration Service (DMS) zu einer verwalteten Azure SQL-Instanz und lokale Computer mit dem Azure Site Recovery-Dienst zu Azure-VMs migriert werden.

Wenn Sie [Szenario 1](migrate-scenarios-assessment.md) und anschließend dieses Szenario mit der anschaulichen Reise-App ausprobieren möchten, können Sie es von [GitHub](https://github.com/Microsoft/SmartHotel360) herunterladen.



**Dienst** | **Beschreibung** | **Kosten**
--- | --- | ---
[Datenbankverwaltungsdienst](https://docs.microsoft.com/azure/dms/dms-overview) | DMS ermöglicht die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen bei minimaler Ausfallzeit. | Der Dienst ist derzeit als öffentliche Vorschau (April 2018) verfügbar und kann während der Vorschauphase kostenlos verwendet werden.<br/><br/> Beachten Sie, dass die öffentliche Vorschauversion von DMS auf eine [Anzahl von Regionen](https://docs.microsoft.com/azure/dms/dms-overview) beschränkt ist.
[Verwaltete Azure SQL-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Die verwaltete Instanz ist ein verwalteter Datenbankdienst, der eine vollständig verwaltete SQL Server-Instanz in der Azure-Cloud darstellt. Sie basiert auf dem gleichen Code wie die neueste Version der SQL Server-Datenbank-Engine und verfügt über die neuesten Features, Leistungsverbesserungen und Sicherheitspatches. | Bei Verwendung verwalteter Azure SQL-Datenbank-Instanzen, die in Azure ausgeführt werden, fallen Gebühren je nach Kapazität an. [Weitere Informationen](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Der Dienst orchestriert und verwaltet die Migration und Notfallwiederherstellung für Azure-VMs sowie lokale virtuelle Computer und physische Server.  | Während der Replikation in Azure fallen Gebühren für Azure Storage an.  Es werden Azure-VMs erstellt, und Gebühren fallen an, sobald ein Failover erfolgt. [Weitere Informationen](https://azure.microsoft.com/pricing/details/site-recovery/) zu Gebühren und Preisen.

In diesem Szenario richten wir ein Site-to-Site-VPN ein, damit DMS eine Verbindung mit der lokalen Datenbank herstellen kann, erstellen eine verwaltete Azure SQL-Instanz in Azure und migrieren die Datenbank. Für Site Recovery bereiten wir die lokale VMware-Umgebung vor, richten die Replikation ein und migrieren die VMs zu Azure.  


> [!IMPORTANT]
> Sie müssen für die eingeschränkte öffentliche Vorschau der verwalteten SQL-Instanz registriert sein. Sie benötigen ein Azure-Abonnement, um sich [registrieren](https://portal.azure.com#create/Microsoft.SQLManagedInstance) zu können. Da sich die Registrierung über einige Tage erstrecken kann, stellen Sie sicher, dass Sie diesen Vorgang durchführen, bevor Sie mit der Bereitstellung dieses Szenarios beginnen.

## <a name="architecture"></a>Architektur

### <a name="site-recovery"></a>Site Recovery

![Site Recovery](media/migrate-scenarios-lift-and-shift/asr-architecture.png)  

### <a name="dms"></a>DMS
![DMS](media/migrate-scenarios-lift-and-shift/dms-architecture.png)  

Szenario:

- Contoso ist ein fiktiver Name für ein typisches Unternehmen. Contoso möchte seine zweistufige lokale Reise-App untersuchen und migrieren.
- Contoso verfügt über ein lokales Rechenzentrum (contoso-datacenter) mit einem lokalen Domänencontroller (**contosodc1**).
- Die interne Reise-App ist auf zwei VMs (WEBVM und SQLVM) verteilt und befindet sich auf dem VMware ESXi-Host (**contosohost1.contoso.com**).
- Die VMware-Umgebung wird mit einem vCenter Server (**vcenter.contoso.com**) verwaltet, der auf einer VM ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie dieses Szenario ausführen möchten, sind hier die entsprechenden Anforderungen aufgeführt.

Anforderungen | Details
--- | ---
**Azure-Abonnement** | Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.<br/><br/> Wenn Sie ein kostenloses Konto erstellen, sind Sie der Administrator Ihres Abonnements und können alle Aktionen durchführen.<br/><br/> Falls Sie ein vorhandenes Abonnement verwenden und nicht der Administrator sind, müssen Sie mit dem Administrator zusammenarbeiten, damit er Ihnen Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ zuweist.<br/><br/> Wenn Sie detailliertere Berechtigungen benötigen, lesen Sie [diesen Artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site Recovery (lokal)** | Einen lokalen vCenter-Server mit Version 5.5, 6.0 oder 6.5.<br/><br/> Einen ESXi-Host mit Version 5.5, 6.0 oder 6.5.<br/><br/> Mindestens eine VMware-VM auf dem ESXi-Host.<br/><br/> VMs müssen die [Azure-Anforderungen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements) erfüllen.<br/><br/> Unterstützte Konfiguration für [Netzwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) und [Speicher](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage).
**DMS** | Für DMS benötigen Sie ein [kompatibles lokales VPN-Gerät](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Sie müssen das lokale VPN-Gerät konfigurieren können. Es muss über eine externe öffentliche IPv4-Adresse verfügen, und die Adresse darf sich nicht hinter einem NAT-Gerät befinden.<br/><br/> Stellen Sie sicher, dass Sie Zugriff auf Ihre lokale SQL Server-Datenbank haben.<br/><br/> Die Windows-Firewall sollte auf die Quelldatenbank-Engine zugreifen können. [Weitere Informationen](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Wenn sich eine Firewall vor dem Datenbankcomputer befindet, fügen Sie Regeln hinzu, um den Zugriff auf die Datenbank und auf Dateien über SMB-Port 445 zuzulassen.<br/><br/> Die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldeinformationen müssen Mitglieder der Serverrolle „sysadmin“ sein.<br/><br/> Sie benötigen eine Netzwerkfreigabe in der lokalen Datenbank, die von DMS zum Sichern der Quelldatenbank verwenden werden kann.<br/><br/> Stellen Sie sicher, dass das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für die Netzwerkfreigabe verfügt.<br/><br/> Notieren Sie sich einen Windows-Benutzer (mit dem Kennwort), der eine Berechtigung für Vollzugriff auf die Netzwerkfreigabe besitzt. Azure Database Migration Service verwendet diese Benutzeranmeldeinformationen, um Sicherungsdateien in den Azure-Speichercontainer hochzuladen.<br/><br/> Bei der SQL Server Express-Installation wird das TCP/IP-Protokoll standardmäßig auf **Deaktiviert** gesetzt. Stellen Sie sicher, dass es aktiviert ist.


# <a name="scenario-overview"></a>Übersicht über das Szenario

Wir gehen wie folgt vor:

> [!div class="checklist"]
> * **Schritt 1: Einrichten einer Site-to-Site-VPN-Verbindung**: Über eine Site-to-Site-VPN-Verbindung stellt DMS eine Verbindung mit Ihrer lokalen Datenbank her. Wir erstellen ein virtuelles Netzwerk für die VPN-Verbindung, und später verwenden wir dieses Netzwerk für Site Recovery. Das von Ihnen erstellte Netzwerk muss sich in der Region befinden, in der Sie einen Recovery Services-Tresor erstellen.
> * **Schritt 2: Einrichten einer verwalteten Azure SQL-Instanz**: Sie benötigen eine bereits erstellte verwaltete Instanz, zu der die lokale SQL Server-Datenbank migriert wird.
> * **Schritt 3: Einrichten eines SAS-URI für DMS**: Ein SAS-URI (Shared Access Signature Uniform Resource Identifier) bietet delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto, damit Sie eingeschränkte Berechtigungen für Speicherobjekte erteilen können. Richten Sie einen SAS-URI so ein, dass DMS auf den Speicherkontocontainer zugreifen kann, in den der Dienst die SQL Server-Sicherungsdateien hochlädt.
> * **Schritt 4: Vorbereiten von DMS**: Sie registrieren den Database Migration-Anbieter, erstellen eine Instanz und erstellen dann ein DMS-Projekt.
> * **Schritt 5: Vorbereiten von Azure für Site Recovery**: Sie erstellen ein Azure-Speicherkonto zum Speichern replizierter Daten.
> * **Schritt 6: Vorbereiten der lokalen VMware für Site Recovery**: Sie bereiten Konten für die VM-Ermittlung und Agentinstallation sowie eine Verbindung mit Azure-VMs nach dem Failover vor. 
> * **Schritt 7: Replizieren von VMs**: Sie konfigurieren die Quell- und Zielumgebung für Site Recovery, richten eine Replikationsrichtlinie ein und starten die Replikation von VMs im Azure-Speicher.
> * **Schritt 8: Migrieren der Datenbank mit DMS**: Führen Sie eine Datenbankmigration aus.
> * **Schritt 9: Migrieren der VMs mit Site Recovery**: Führen Sie ein Failover aus, um die VMs zu Azure zu migrieren.


## <a name="step-1-set-up-site-to-site-vpn-connection"></a>Schritt 1: Einrichten einer Site-to-Site-VPN-Verbindung

Zum Vorbereiten der Site-to-Site-VPN-Verbindung richten Sie ein virtuelles Netzwerk und Subnetze ein, erstellen ein VPN-Gateway für das virtuelle Netzwerk und richten ein lokales Netzwerkgateway ein, damit Azure bekannt ist, wie eine Verbindung mit Ihrem lokalen VPN hergestellt wird. Anschließend erstellen und überprüfen Sie die Site-to-Site-Verbindung.

### <a name="set-up-a-virtual-network"></a>Einrichten eines virtuellen Netzwerks

Erstellen Sie ein virtuelles Azure-Netzwerk, um DMS eine Site-to-Site-Verbindung mit Ihrem lokalen SQL Server bereitzustellen.


1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressource erstellen**. Wählen Sie **Netzwerk** > **Virtuelles Netzwerk** aus.
2. Wählen Sie unter **Virtuelles Netzwerk** > **Bereitstellungsmodell auswählen** die Option **Resource Manager** > **Erstellen** aus.
3. Geben Sie unter **Virtuelles Netzwerk erstellen** > **Name** einen eindeutigen Netzwerknamen ein. In diesem Szenario **ContosoVNET**.
4. Fügen Sie unter **Adressbereich** den IP-Adressbereich hinzu. Der Bereich darf nicht mit Ihrem lokalen Adressbereich überlappen.
5. Erstellen Sie unter **Ressourcengruppe** eine neue Gruppe, oder wählen Sie eine vorhandene Gruppe aus. In diesem Szenario verwenden wir **ContosoRG**.
6. Wählen Sie unter **Standort** die Region aus, in der das virtuelle Netzwerk erstellt werden soll. Hier verwenden wir **USA, Osten 2**.
7. Fügen Sie unter **Subnetz** den Namen und Adressbereich des ersten Subnetzes hinzu. Wir haben das Subnetz **Apps** erstellt.
8. Deaktivieren Sie Dienstendpunkte.

    ![Erstellen eines virtuellen Netzwerks](media/migrate-scenarios-lift-and-shift/vnet-create-network.png)

9. Wählen Sie **An Dashboard anheften** aus, damit Sie das Netzwerk in Zukunft leichter finden können, und klicken Sie dann auf **Erstellen**.
10. Die Erstellung des virtuellen Netzwerks dauert ein paar Sekunden. Prüfen Sie es nach der Erstellung auf dem Dashboard im Azure-Portal.
11. Stellen Sie sicher, dass die folgenden Kommunikationsports in Ihrem virtuellen Netzwerk zulässig sind: 443, 53, 9354, 445, 12000.


### <a name="set-up-subnets"></a>Einrichten von Subnetzen

Es müssen vier Subnetze in unserem Azure-Netzwerk vorhanden sein:

![Subnetzliste](media/migrate-scenarios-lift-and-shift/vnet-subnet-list.png)

Sie haben das erste Subnetz (Apps) beim Erstellen des Netzwerks eingerichtet. Erstellen Sie jetzt die übrigen Subnetze. Das Gatewaysubnetz wird von der VPN-Gatewayverbindung verwendet, damit Azure Datenverkehr über eine VPN-Verbindung an Ihren lokalen Standort senden kann.

1. Klicken Sie im virtuellen Netzwerk unter **Einstellungen** auf **Subnetz** > **+Subnetz**.
2. Richten Sie ein Subnetz **Datenmigration** mit diesem Adressbereich ein, und klicken Sie dann auf **OK**.

    ![Datensubnetz](media/migrate-scenarios-lift-and-shift/vnet-subnet-data.png)  


3. Richten Sie unter **+Subnetz** ein Subnetz mit dem Namen **Identity** mit dem folgenden Adressbereich ein, und klicken Sie dann auf **OK**.
    ![Subnetz „Identity“](media/migrate-scenarios-lift-and-shift/vnet-subnet-identity.png)

4. Klicken Sie unter **Subnetz** auf **Gatewaysubnetz**, und klicken Sie dann auf **OK**.
    - Dieses Subnetz enthält die IP-Adressen, die von Ihrem VPN-Gateway für VPN-Verbindungen verwendet werden.
    - Der Name dieses Subnetzes wird automatisch festgelegt, damit es von Azure erkannt wird.
    - Passen Sie den automatisch eingetragenen Adressbereich entsprechend Ihrem lokalen Subnetz an. 

    ![Gatewaysubnetz](media/migrate-scenarios-lift-and-shift/vnet-subnet-gateway.png)
    
### <a name="set-up-a-virtual-network-gateway"></a>Einrichten eines Gateways für virtuelle Netzwerke

1. Klicken Sie im Portal auf der linken Seite auf **+**, und geben Sie „Gateway für virtuelle Netzwerke“ für die Suche ein. Klicken Sie unter **Ergebnisse** auf **Gateway für virtuelle Netzwerke**.
2. Klicken Sie unten auf der Seite „Gateway für virtuelle Netzwerke“ auf **Erstellen**.
3. Geben Sie unter **Gateway für virtuelle Netzwerke erstellen** >  **Name** einen Namen für das Gatewayobjekt an.
4. Wählen Sie unter **Gatewaytyp** die Option **VPN** aus.
5. Wählen Sie unter **VPN-Typ** die Option **Routenbasiert** aus.
6. Wählen Sie unter **SKU** die Gateway-SKU in der Dropdownliste aus. Welche SKUs in der Dropdownliste aufgeführt werden, hängt vom ausgewählten VPN-Typ ab. Aktivieren Sie nicht den Aktiv/Aktiv-Modus. [Weitere Informationen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku) zu SKUs.
7. Fügen Sie unter **Virtuelles Netzwerk** > **Virtuelles Netzwerk auswählen** das von Ihnen erstellte Netzwerk hinzu. Das Gateway wird diesem Netzwerk hinzugefügt.
8. Geben Sie unter **Erste IP-Konfiguration** eine öffentliche IP-Adresse an.  Ein VPN-Gateway muss über eine öffentliche IP-Adresse verfügen. Diese Adresse wird bei der Erstellung des VPN-Gateways der Ressource dynamisch zugewiesen. Derzeit werden nur dynamische Adressen unterstützt, doch ändert sich die IP-Adresse nach dem Zuweisen nicht.
    - Klicken Sie auf **Gateway-IP-Konfiguration erstellen**. Klicken Sie unter **Öffentliche IP-Adresse auswählen** auf **+Neu erstellen**.
    - Geben Sie unter **Öffentliche IP-Adresse erstellen**, einen Namen für die öffentliche IP-Adresse (Contoso-Gateway) ein. Behalten Sie für die SKU die Einstellung **Basis** bei, und klicken Sie auf **OK**, um die Änderungen zu speichern.
        
    ![Gateway des virtuellen Netzwerks](media/migrate-scenarios-lift-and-shift/vnet-create-vpn-gateway2.png) 

9. Klicken Sie auf **Erstellen**, um das VPN-Gateway zu erstellen. Die Einstellungen werden überprüft, und auf dem Dashboard wird angezeigt, dass das Gateway des virtuellen Netzwerks bereitgestellt wird.

    ![Überprüfen des Gateways für virtuelle Netzwerke](media/migrate-scenarios-lift-and-shift/vnet-vpn-gateway-validate.png)
    
Die Erstellung eines Gateways kann bis zu 45 Minuten dauern. Unter Umständen müssen Sie die Portalseite aktualisieren, um den Status „Abgeschlossen“ anzuzeigen.

### <a name="set-up-a-local-network-gateway"></a>Einrichten eines lokalen Netzwerkgateways

Richten Sie in Azure ein lokales Netzwerkgateway zur Darstellung Ihres lokalen Standorts ein.

1. Klicken Sie im Portal auf **+Ressource erstellen**.
2. Geben Sie im Suchfeld den Text **Lokales Netzwerkgateway** ein, und drücken Sie die**** EINGABETASTE. Klicken Sie in den Ergebnissen auf **Lokales Netzwerkgateway** > **Erstellen**.
3. Geben Sie unter **Lokales Netzwerkgateway erstellen** > **Name** einen Namen ein, der von Azure für das Objekt Ihres lokalen Netzwerkgateways verwendet wird.
4. Geben Sie unter **IP-Adresse** die öffentliche IP-Adresse des lokalen VPN-Geräts ein, mit dem Azure eine Verbindung herstellt. Die IP-Adresse sollte nicht der NAT unterzogen sein und muss für Azure erreichbar sein.
5. Geben Sie unter **Adressbereich** die Adressbereiche des lokalen Netzwerks ein, die über das VPN-Gateway an das lokale Gerät weitergeleitet werden. Stellen Sie sicher, dass sich diese nicht mit Bereichen im virtuellen Azure-Netzwerk überschneiden.
6. **BGP-Einstellungen konfigurieren** ist für dieses Szenario nicht relevant.
7. Vergewissern Sie sich, dass unter **Abonnement:** das richtige Abonnement angegeben ist.
8. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe aus, die Sie zum Erstellen des Netzwerks (ContosoRG) verwendet haben.
9. Wählen Sie unter **Standort** die Region aus, in der Sie das virtuelle Netzwerk erstellt haben.

    ![Lokales Gateway](media/migrate-scenarios-lift-and-shift/vnet-create-local-gateway.png)

4. Klicken Sie auf **Erstellen**, um das lokale Gateway zu erstellen.

### <a name="configure-your-on-premises-vpn-device"></a>Konfigurieren Ihres lokalen VPN-Geräts

Auf Ihrem lokalen VPN-Gerät muss Folgendes konfiguriert werden:

- Die öffentliche IPv4-Adresse des von Ihnen soeben erstellten Gateways für das virtuelle Netzwerk in Azure
- Der vorinstallierte Schlüssel (der Schlüssel, den Sie beim Erstellen der Site-to-Site-VPN-Verbindung verwendet haben)  

Hilfe beim Einrichten Ihres lokalen VPN-Geräts:

- Abhängig von Ihrem lokalen VPN-Gerät können Sie ggf. ein VPN-Gerätekonfigurationsskript herunterladen. [Weitere Informationen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-download-vpndevicescript).
- Sehen Sie sich [weitere hilfreiche Ressourcen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#VPNDevice) an.

### <a name="create-the-vpn-connection"></a>Erstellen der VPN-Verbindung

Nun erstellen Sie die Site-to-Site-VPN-Verbindung zwischen dem Gateway des virtuellen Netzwerks und Ihrem lokalen VPN-Gerät.

1. Öffnen Sie die Seite für das virtuelle Netzwerk über **Virtuelle Netzwerke** > **Übersicht** > **Verbundene Geräte**. 
2. Klicken Sie auf **Verbindungen** > **+Hinzufügen**.
3. Geben Sie unter **Verbindung hinzufügen** > **Name** einen Namen für die Verbindung an.
4. Wählen Sie unter **Verbindungstyp** die Option **Site-to-Site (IPsec)** aus.
5. Der Wert für **Gateway für virtuelle Netzwerke** ist festgelegt, da Sie von diesem Gateway aus die Verbindung herstellen.
6. Geben Sie unter **Lokales Netzwerkgateway** das von Ihnen erstellte lokale Netzwerkgateway an.
7. Geben Sie unter **Gemeinsam verwendeter Schlüssel** den Schlüssel an, der dem Wert entspricht, den Sie für Ihr lokales VPN-Gerät verwenden. Im Beispiel wird „abc123“ verwendet. Sie können (und sollten) allerdings einen komplexeren Wert verwenden. Entscheidend ist Folgendes: Der Wert, den Sie hier angeben, muss dem Wert entsprechen, den Sie beim Konfigurieren Ihres VPN-Geräts angegeben haben.
8. Die Werte für **Abonnement**, **Ressourcengruppe** und **Standort** sind festgelegt.

    ![VPN-Verbindung](media/migrate-scenarios-lift-and-shift/vpn-connection.png) 

4. Klicken Sie auf **OK**, um die Verbindung zu erstellen. Auf dem Bildschirm blinkt der Hinweis, dass die Verbindung erstellt wird.
5. Nachdem die Verbindung erstellt wurde, können Sie diese auf der Seite **Verbindungen** des Gateways für virtuelle Netzwerke anzeigen. Der Status wechselt von „Unbekannt“ zu „Verbindung wird hergestellt“ und dann zu „Erfolgreich“.

### <a name="verify-the-vpn-connection"></a>Überprüfen der VPN-Verbindung

Im Azure-Portal können Sie zur gewünschten Verbindung navigieren und den Verbindungsstatus eines Resource Manager-VPN-Gateways anzeigen. 

1. Klicken Sie auf **Alle Ressourcen**, und navigieren Sie zu Ihrem Gateway für virtuelle Netzwerke.
2. Klicken Sie auf der Seite mit dem Gateway für virtuelle Netzwerke auf Verbindungen. Der Status der einzelnen Verbindungen wird angezeigt.
3. Klicken Sie auf den Namen der Verbindung, um weitere Informationen unter **Zusammenfassung** anzuzeigen. Der Status lautet „Erfolgreich“ und „Verbunden“, wenn die Verbindung erfolgreich hergestellt wurde.

Stellen Sie nun sicher, dass Sie über VPN eine Verbindung mit der SQL Server-VM herstellen können. Verwenden Sie eine Remotedesktopverbindung, und stellen Sie eine Verbindung mit der VM-IP-Adresse her.



## <a name="step-2-prepare-an-azure-sql-managed-instance"></a>Schritt 2: Vorbereiten einer verwalteten Azure SQL-Instanz

### <a name="set-up-a-virtual-network"></a>Einrichten eines virtuellen Netzwerks

Für dieses Szenario müssen Sie ein weiteres virtuelles Netzwerk erstellen, das für die verwaltete Azure SQL-Instanz bereitgestellt wird.  Beachten Sie folgende Punkte:

- Zum Erstellen der verwalteten Instanz benötigen Sie ein dediziertes Subnetz.
- Das Subnetz muss leer sein, darf keinen anderen Clouddienst enthalten und darf kein Gatewaysubnetz sein. Nachdem die verwaltete Instanz erstellt wurde, dürfen Sie dem Subnetz keine Ressourcen hinzufügen.
- Dem Subnetz darf keine NSG zugeordnet sein.
- Das Subnetz muss eine benutzerdefinierte Routingtabelle (User Route Table, UDR) aufweisen, der die Route vom Typ 0.0.0.0/0, nächster Hop „Internet“ als einzige Route zugewiesen ist. 
- Optionales benutzerdefiniertes DNS: Wenn ein benutzerdefiniertes DNS für das VNET angegeben ist, muss die IP-Adresse des rekursiven Azure-Resolvers (z.B. 168.63.129.16) zur Liste hinzugefügt werden. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Dem Subnetz darf kein Dienstendpunkt (Speicher oder SQL) zugeordnet sein. Dienstendpunkte sollten im virtuellen Netzwerk deaktiviert sein.
- Das Subnetz muss mindestens 16 IP-Adressen aufweisen. [Weitere Informationen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) zum Festlegen der Größe des Subnetzes für die verwaltete Instanz.

Richten Sie das virtuelle Netzwerk folgendermaßen ein:

1.  Klicken Sie im Azure-Portal auf **Ressource erstellen**. 
2. Wählen Sie **Netzwerk** > **Virtuelles Netzwerk** aus.
3. Wählen Sie unter **Virtuelles Netzwerk** > **Bereitstellungsmodell auswählen** die Option **Resource Manager** > **Erstellen** aus.
4. Geben Sie unter **Virtuelles Netzwerk erstellen** > **Name** einen eindeutigen Netzwerknamen ein. In diesem Szenario **ContosoVNETSQLMI**.
5. Fügen Sie unter **Adressbereich** den nachfolgend gezeigten IP-Adressbereich hinzu. Der Bereich darf nicht mit Ihrem lokalen Adressbereich und dem ContosoVNET-Adressbereich überlappen.
6. Erstellen Sie unter **Ressourcengruppe** eine neue Gruppe, oder wählen Sie eine vorhandene Gruppe aus. In diesem Szenario verwenden wir **ContosoRG**.
7. Wählen Sie unter **Standort** die Region aus, in der das virtuelle Netzwerk erstellt werden soll. Hier verwenden wir **USA, Osten 2**.
8. Fügen Sie unter **Subnetz** den Namen und Adressbereich des ersten Subnetzes hinzu. Wir haben das Subnetz **ManagedInstances** erstellt.
9. Deaktivieren Sie Dienstendpunkte.

    ![Netzwerk für verwaltete Instanz](media/migrate-scenarios-lift-and-shift/network-mi.png)

10. Nachdem Sie das Netzwerk bereitgestellt haben, müssen Sie die DNS-Einstellungen ändern. In diesem Szenario verweist das DNS zunächst mit einer statischen privaten IP-Adresse (172.16.3.4) auf einen Domänencontroller im Subnetz „Identity“ und dann auf den Azure DNS-Resolver 168.63.129.16.

    ![Netzwerk für verwaltete Instanz](media/migrate-scenarios-lift-and-shift/network-mi2.png)


### <a name="set-up-routing"></a>Einrichten des Routings

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**. Klicken Sie auf **Routingtabelle** > **In Routingtabelle erstellen**.
2. Geben Sie unter **Routingtabelle erstellen** > **Name** einen Namen für die Tabelle an.
3. Wählen Sie Ihr Abonnement, die Ressourcengruppe und den Standort aus. Lassen Sie BGP deaktiviert, und klicken Sie auf **Erstellen**.
    ![Routingtabelle](media/migrate-scenarios-lift-and-shift/route-table.png)

4. Öffnen Sie die Routingtabelle nach dem Erstellen, und klicken Sie auf **Routen** > **+Hinzufügen**.
5. Geben Sie unter **Route hinzufügen** > **Name** einen Routennamen an.
6. Geben Sie unter **Adresspräfix** die Zeichenfolge „0.0.0.0/0“ an.
7. Wählen Sie unter **Typ des nächsten Hops** die Option **Internet** aus. Klicken Sie dann auf **OK**.

     ![Routingtabelle](media/migrate-scenarios-lift-and-shift/route-table2.png)


### <a name="apply-the-route-to-the-managed-instance-subnet"></a>Anwenden der Route auf das Subnetz für die verwaltete Instanz

1. Öffnen Sie das von Ihnen erstellte virtuelle Netzwerk. Klicken Sie auf **Subnetze** > Name Ihres Subnetzes.
2. Klicken Sie auf **Routingtabelle** > Name Ihrer Tabelle. Klicken Sie anschließend auf **Speichern**.

     ![Routingtabelle](media/migrate-scenarios-lift-and-shift/route-table3.png)

### <a name="create-a-managed-instance"></a>Erstellen einer verwalteten Instanz

1. Klicken Sie auf **Ressource erstellen**, suchen Sie nach **Verwaltete Instanz**, und wählen Sie die Option **Verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion)** aus.
2. Klicken Sie auf **Erstellen**.
3. Wählen Sie unter **Verwaltete SQL-Instanz** Ihr Abonnement aus, und stellen Sie sicher, dass für **Nutzungsbedingungen für die Vorschau** der Status **Akzeptiert** angezeigt wird.
4. Geben Sie unter **Name der verwalteten Instanz** einen Namen an. 
5. Geben Sie einen Benutzernamen und ein Kennwort als Administrator für die Instanz an.
6. Wählen Sie die Ressourcengruppe, den Standort und das virtuelle Netzwerk für die Instanz aus.
7. Klicken Sie auf **Tarif**, um die Größe für Compute und Speicher festzulegen. Standardmäßig erhält die Instanz 32 GB kostenlosen Speicherplatz (April 2018).
8. Geben Sie den Speicher und die Anzahl virtueller Kerne an.
9. Klicken Sie auf **Übernehmen**, um die Einstellungen zu speichern, und auf **Erstellen**, um die verwaltete Instanz bereitzustellen. Zum Anzeigen des Bereitstellungsstatus klicken Sie auf **Benachrichtigungen** und **Die Bereitstellung wird ausgeführt**.

    ![Verwaltete Instanz](media/migrate-scenarios-lift-and-shift/mi-1.png)

Sobald die Bereitstellung der verwalteten Instanz abgeschlossen ist, werden zwei neue Ressourcen in der Ressourcengruppe „ContosoRG“ angezeigt: „Virtueller Cluster“ für die verwalteten Instanzen und „Verwaltete SQL-Instanz“. Beide befinden sich am Standort „USA, Osten 2“.

![Verwaltete Instanz](media/migrate-scenarios-lift-and-shift/managed-instance2.png)


## <a name="step-3-get-a-sas-uri-for-dms"></a>Schritt 3: Abrufen eines SAS-URI für DMS

Rufen Sie einen SAS-URI ab, damit DMS auf Ihren Speicherkontocontainer zugreifen kann, um die Sicherungsdateien hochzuladen, die zum Migrieren von Datenbanken zu einer verwalteten Azure SQL-Datenbank-Instanz verwendet werden. 

1. Öffnen Sie den Speicher-Explorer (Vorschau).
2. Erweitern Sie im linken Bereich das Speicherkonto mit dem Blobcontainer, für den Sie die SAS abrufen möchten. Erweitern Sie die Option **Blobcontainer**des Speicherkontos.
3. Klicken Sie mit der rechten Maustaste auf den Container, und wählen Sie **Shared Access Signature abrufen** aus.

     ![SAS](media/migrate-scenarios-lift-and-shift/sas-1.png)

4. Geben Sie unter **Shared Access Signature** die Richtlinie, das Start- und Ablaufdatum, die Zeitzone und die Zugriffsebenen für die Ressource an. Klicken Sie dann auf **Erstellen**.

    ![SAS](media/migrate-scenarios-lift-and-shift/sas-2.png)

5. In einem zweiten Dialogfeld wird der Blobcontainer zusammen mit der URL und den Abfragezeichenfolgen angezeigt, die Sie zum Zugreifen auf die Speicherressource verwenden können. Wählen Sie **Kopieren** aus, um die Werte zu kopieren. Bewahren Sie diese an einem sicheren Ort auf. Sie benötigen diese beim Einrichten von DMS.

      ![SAS](media/migrate-scenarios-lift-and-shift/sas-3.png)  

## <a name="step-4-prepare-dms"></a>Schritt 4: Vorbereiten von DMS

Sie registrieren den Database Migration-Anbieter und erstellen eine Instanz

### <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“

1. Wählen Sie unter Ihrem Abonnement die Option **Ressourcenanbieter** aus. 
2. Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option Registrieren aus. 


### <a name="create-an-instance"></a>Erstellen einer Instanz

1. Wählen Sie **+ Ressource erstellen** aus, suchen Sie nach „Azure Database Migration Service“, und wählen Sie dann **Azure Database Migration Service** aus der Dropdownliste aus.
2. Klicken Sie im Bildschirm „Azure Database Migration Service (Vorschau)“ auf **Erstellen**.
3. Geben Sie einen Namen für den Dienst, das Abonnement, die Ressourcengruppe, ein virtuelles Netzwerk und den Tarif an.
4. Wählen Sie **Erstellen**, um den Dienst zu erstellen.

    ![DMS](media/migrate-scenarios-lift-and-shift/dms-instance.png)  




## <a name="step-5-prepare-azure-for-the-site-recovery-service"></a>Schritt 5: Vorbereiten von Azure für den Site Recovery-Dienst

### <a name="set-up-a-virtual-network"></a>Einrichten eines virtuellen Netzwerks

Sie benötigen ein Azure-Netzwerk, in dem die Azure-VMs beim Erstellen nach einem Failover gespeichert werden, und ein Azure-Speicherkonto, in dem replizierte Daten gespeichert werden.

- Für dieses Szenario verwenden wir das zuvor für DMS erstellte Azure-Netzwerk.
- Beachten Sie, dass sich das verwendete Netzwerk in der gleichen Region wie der Site Recovery-Tresor befinden muss.


### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Site Recovery repliziert VM-Daten in den Azure-Speicher. Azure-VMs werden aus dem Speicher erstellt, wenn Sie ein Failover von einem lokalen Standort nach Azure ausführen.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im Menü auf **Neu** > **Speicher** > **Speicherkonto**.
2. Geben Sie unter **Speicherkonto erstellen** einen Namen für das Konto ein. Verwenden Sie für diese Tutorials den Namen **contosovmsacct1910171607**. Der Name muss in Azure eindeutig und zwischen 3 und 24 Zeichen lang sein, wobei nur Ziffern und Kleinbuchstaben zulässig sind.
3. Wählen Sie unter **Bereitstellungsmodell** die Option **Resource Manager**.
4. Wählen Sie unter **Kontoart** die Option **Allgemein**. Wählen Sie unter **Leistung** die Option **Standard**. Wählen Sie keinen Blobspeicher aus.
5. Wählen Sie unter **Replikation** für die Speicherredundanz die Standardoption **Georedundanter Speicher mit Lesezugriff**.
6. Wählen Sie unter **Abonnement** das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.
7. Geben Sie unter **Ressourcengruppe** eine neue Ressourcengruppe ein. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Verwenden Sie für diese Tutorials den Namen **ContosoRG**.
8. Wählen Sie unter **Standort** den geografischen Standort für das Speicherkonto aus. Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden. Für dieses Szenario verwenden wir die Region **USA, Osten 2**.

   ![Speicherkonto erstellen](media/migrate-scenarios-lift-and-shift/create-storageacct.png)

9. Wählen Sie **Erstellen**, um das Speicherkonto zu erstellen.

### <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

1. Wählen Sie im Azure-Portal die Option **Ressource erstellen** > **Überwachung und Verwaltung** > **Backup und Site Recovery**.
2. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird. Verwenden Sie für dieses Tutorial den Namen **ContosoVMVault**.
3. Wählen Sie unter **Ressourcengruppe** die vorhandene Ressourcengruppe mit dem Namen **contosoRG** aus.
4. Geben Sie unter **Standort** die Azure-Region **USA, Osten 2** ein.
5. Wählen Sie **An Dashboard anheften** > **Erstellen**, um über das Dashboard schnell auf den Tresor zuzugreifen.
Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf der Hauptseite **Recovery Services-Tresore** angezeigt.

## <a name="step-6-prepare-on-premises-vmware-for-site-recovery"></a>Schritt 6: Vorbereiten der lokalen VMware für Site Recovery

Zum Vorbereiten der VMware für Site Recovery ist Folgendes erforderlich:

- Vorbereiten eines Kontos auf dem vCenter-Server oder dem vSphere ESXi-Host zum Automatisieren der VM-Ermittlung
- Vorbereiten eines Kontos für die automatische Installation des Mobility Service auf VMware-VMs
- Vorbereiten lokaler VMs, wenn Sie nach dem Failover eine Verbindung mit Azure-VMs herstellen möchten


### <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung

Site Recovery benötigt Zugriff auf VMware-Server, um folgende Aufgaben durchzuführen:

- Automatisches Ermitteln von VMs. Dafür ist mindestens ein Konto mit Lesezugriff erforderlich.
- Orchestrieren von Replikation, Failover und Failback. Sie benötigen ein Konto, das berechtigt ist, Vorgänge wie das Erstellen und Entfernen von Datenträgern sowie das Einschalten virtueller Computer durchzuführen.

Erstellen Sie das Konto wie folgt:

1. Erstellen Sie zum Verwenden eines dedizierten Kontos eine Rolle auf vCenter-Ebene. Geben Sie der Rolle einen Namen wie **Azure_Site_Recovery**.
2. Weisen Sie der Rolle die Berechtigungen, die in der folgenden Tabelle aufgeführt sind, zu.
3. Erstellen Sie einen Benutzer auf dem vCenter-Server oder vSphere-Host. Weisen Sie dem Benutzer die Rolle zu.

**Aufgabe** | **Rolle/Berechtigungen** | **Details**
--- | --- | ---
**VM-Ermittlung** | Mindestens ein Benutzer mit Lesezugriff<br/><br/> Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=schreibgeschützt | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **Kein Zugriff** mit **An untergeordnetes Objekt weitergeben** zu.
**Vollständige Replikation, Failover, Failback** |  Erstellen Sie eine Rolle („Azure_Site_Recovery“) mit den erforderlichen Berechtigungen, und weisen Sie die Rolle dann einem VMware-Benutzer oder einer VMware-Gruppe zu.<br/><br/> Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=Azure_Site_Recovery<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Aufgaben -> Aufgabe erstellen, Aufgabe aktualisieren<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtueller Computer -> Momentaufnahmen -> Momentaufnahmen entfernen | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **Kein Zugriff** mit **An untergeordnetes Objekt weitergeben** zu.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Vorbereiten eines Kontos für die Installation des Mobility Services

Der Mobility Service muss auf der VM installiert sein, die Sie replizieren möchten.

- Site Recovery kann eine automatische Pushinstallation dieser Komponente durchführen, wenn Sie die Replikation für die VM aktivieren.
- Für die automatische Pushinstallation müssen Sie ein Konto vorbereiten, mit dem Site Recovery auf die VM zugreift.
- Sie geben dieses Konto an, wenn Sie die Replikation in der Azure-Konsole einrichten.
- Sie benötigen ein Domänen- oder lokales Konto mit Berechtigungen zum Installieren auf der VM.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Nach einem Failover zu Azure können Sie von Ihrem lokalen Netzwerk aus eine Verbindung mit Ihren replizierten VMs in Azure herstellen.

Gehen Sie wie folgt vor, wenn Sie die Verbindung mit Windows-VMs nach dem Failover per RDP herstellen möchten:

1. Aktivieren Sie vor dem Failover RDP auf dem lokalen virtuellen Computer, um über das Internet darauf zuzugreifen. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.
2. Für den Zugriff über ein Site-to-Site-VPN aktivieren Sie RDP auf dem lokalen Computer. RDP sollte unter **Windows-Firewall** -> **Zugelassene Apps und Feature** für **private und Domänennetzwerke** zugelassen werden.
3. Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135).
4. Auf dem virtuellen Computer sollten keine ausstehenden Windows-Updates vorhanden sein, wenn Sie ein Failover auslösen. Andernfalls können Sie sich nach Abschluss des Updates nicht mehr auf dem virtuellen Computer anmelden.
5. Aktivieren Sie nach dem Failover auf der Windows-Azure-VM die **Startdiagnose**, um einen Screenshot des virtuellen Computers anzuzeigen. Wenn Sie keine Verbindung herstellen können, überprüfen Sie, ob der virtuelle Computer ausgeführt wird, und sehen sich dann diese [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) an.



## <a name="step-7-replicate-vms-with-site-recovery"></a>Schritt 7: Replizieren von VMs mit Site Recovery

### <a name="select-a-replication-goal"></a>Auswählen eines Replikationsziels

1. Wählen Sie unter **Recovery Services-Tresore** den Tresornamen **ContosoVMVault** aus.
2. Wählen Sie unter **Erste Schritte** die Option „Site Recovery“ aus. Klicken Sie anschließend auf **Infrastruktur vorbereiten**.
3. Wählen Sie in **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie in **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie in **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus. Wählen Sie dann **OK**aus.

    ![OVF-Vorlage](./media/migrate-scenarios-lift-and-shift/replication-goal.png)


### <a name="confirm-deployment-planning"></a>Bestätigen der Bereitstellungsplanung

Klicken Sie unter **Bereitstellungsplanung** in der Dropdownliste auf **Ja, ist abgeschlossen**.

### <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung


Zum Einrichten der Quellumgebung benötigen Sie einen einzelnen, hochverfügbaren, lokalen Computer, um lokale Site Recovery-Komponenten zu hosten. Die Komponenten umfassen den Konfigurationsserver und den Prozessserver.

- Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- Der Prozessserver fungiert als Replikationsgateway. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.
- Der Prozessserver installiert auch Mobility Service auf virtuellen Computern, die Sie replizieren möchten, und führt auf lokalen VMware-VMs eine automatische Ermittlung durch.


So richten Sie den Konfigurationsserver als hoch verfügbare VMware-VM ein:

- Laden Sie eine vorbereitete OVF-Vorlage (Open Virtualization Format) herunter.
- Importieren Sie die Vorlage in VMware, um den virtuellen Computer zu erstellen.
- Richten Sie den Konfigurationsserver ein, und registrieren Sie ihn im Tresor. 

Nach der Registrierung ermittelt Site Recovery lokale virtuelle VMware-Computer.



#### <a name="download-the-vm-template"></a>Herunterladen der Vorlage

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**.

    ![Herunterladen der Vorlage](./media/migrate-scenarios-lift-and-shift/new-cs.png)

4. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
2. Laden Sie die OVF-Vorlage für den Konfigurationsserver herunter.

    ![Herunterladen der OVF-Vorlage](./media/migrate-scenarios-lift-and-shift/add-cs.png)

  > [!TIP]
  Die neueste Version der Konfigurationsservervorlage können Sie direkt aus dem [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.

#### <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware

1. Melden Sie sich beim VMware vCenter-Server über den VMWare vSphere-Client an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den Assistenten zum Bereitstellen von OVF-Vorlagen zu starten. 

     ![OVF-Vorlage](./media/migrate-scenarios-lift-and-shift/vcenter-wizard.png)

3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie unter **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie unter **Speicher auswählen** für **Select virtual disk format** (Format für virtuellen Datenträger auswählen) die Option **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus, um eine optimale Leistung zu erzielen.
7. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
8. Gehen Sie unter **Bereit für den Abschluss** wie folgt vor:

    * Wählen Sie zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.

    * Wenn Sie eine weitere Netzwerkschnittstelle hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten). Klicken Sie dann auf **Fertig stellen**. Standardmäßig wird die Konfigurationsservervorlage mit einer einzelnen NIC bereitgestellt. Weitere NICs können nach der Bereitstellung hinzugefügt werden.



#### <a name="register-the-configuration-server"></a>Registrieren des Konfigurationsservers 

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Benutzeroberfläche für die Installation von Windows Server 2016 hochgefahren. Akzeptieren Sie den Lizenzvertrag, und geben Sie ein Administratorkennwort ein.
3. Melden Sie sich nach Abschluss der Installation als Administrator am virtuellen Computer an.
4. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool gestartet.
5. Geben Sie einen Namen ein, der für die Registrierung des Konfigurationsservers bei Site Recovery verwendet wird. Klicken Sie anschließend auf **Weiter**.

    ![OVF-Vorlage](./media/migrate-scenarios-lift-and-shift/config-server-register1.png)

6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.

    ![OVF-Vorlage](./media/migrate-scenarios-lift-and-shift/config-server-register2.png)

7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.

#### <a name="configure-settings-and-add-the-vmware-server"></a>Konfigurieren der Einstellungen und Hinzufügen des VMware-Servers

1. Klicken Sie im Assistenten für die Konfigurationsserververwaltung auf **Konnektivität einrichten**, und wählen Sie anschließend die NIC aus, die den Replikationsdatenverkehr empfangen soll. Klicken Sie dann auf **Speichern**. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
2. Wählen Sie unter **Recovery Services-Tresor auswählen** Ihr Azure-Abonnement, die entsprechende Ressourcengruppe und den entsprechenden Tresor aus.

    ![Tresor](./media/migrate-scenarios-lift-and-shift/cswiz1.png) 

3. Akzeptieren Sie unter **Drittanbietersoftware installieren** den Lizenzvertrag. Klicken Sie auf **Herunterladen und installieren**, um MySQL Server zu installieren.
4. Klicken Sie auf **VMware PowerCLI installieren**. Stellen Sie sicher, dass alle Browserfenster geschlossen sind, bevor Sie diesen Schritt durchführen. Klicken Sie anschließend auf **Weiter**.
5. Unter **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor der Vorgang fortgesetzt wird.
6. Geben Sie unter **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port ein, über den der Server lauscht. Geben Sie einen Anzeigenamen ein, der für den VMware-Server im Tresor verwendet werden soll.
7. Geben Sie die Anmeldeinformationen ein, die der Konfigurationsserver beim Herstellen der Verbindung mit dem VMware-Server verwenden soll. Site Recovery verwendet diese Anmeldeinformationen für die automatische Erkennung von VMware-VMs, die für die Replikation verfügbar sind. Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter**.

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

8. Geben Sie unter **Anmeldeinformationen für virtuelle Computer konfigurieren** den Benutzernamen und das Kennwort an, die für die automatische Installation von Mobility Service auf Computern verwendet werden sollen, wenn die Replikation aktiviert ist. Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten. 

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

7. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen. 
8. Überprüfen Sie nach Abschluss der Registrierung im Azure-Portal, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Klicken Sie dann auf **OK**, um die Einstellungen für das Ziel zu konfigurieren.
9. Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt virtuelle Computer.

> [!NOTE]
> Es kann 15 Minuten oder länger dauern, bis der Kontoname im Portal angezeigt wird. Klicken Sie zur sofortigen Aktualisierung auf **Konfigurationsserver** > ***Servername*** > **Server aktualisieren**.

### <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie Zielressourcen aus, und überprüfen Sie sie.

1. Wählen Sie unter **Infrastruktur vorbereiten** > **Ziel** das gewünschte Azure-Abonnement aus.
2. Wählen Sie für das Zielbereitstellungsmodell die Option **Azure Resource Manager** aus.

3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.


### <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

1. Klicken Sie unter **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **Replikationsrichtlinie** auf **Erstellen und zuordnen**.
1. Geben Sie unter **Replikationsrichtlinie erstellen** den Richtliniennamen **VMwareRepPolicy** ein.
2. Übernehmen Sie in **RPO-Schwellenwert** den Standardwert von 60 Minuten. Mit diesem Wert wird festgelegt, wie oft Wiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
3. Übernehmen Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** den Standardwert von 24 Stunden für die Dauer des Aufbewahrungszeitfensters für jeden Wiederherstellungspunkt. Verwenden Sie für dieses Tutorial 72 Stunden. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden.
4. Übernehmen Sie unter **Häufigkeit der Momentaufnahmen für App-Konsistenz** den Standardwert von 60 Minuten für die Häufigkeit, mit der Momentaufnahmen für die App-Konsistenz erstellt werden. Wählen Sie **OK** aus, um die Richtlinie zu erstellen.

    ![Erstellen einer Replikationsrichtlinie](./media/migrate-scenarios-lift-and-shift/replication-policy.png)

5. Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. 

    ![Zuordnen der Replikationsrichtlinie](./media/migrate-scenarios-lift-and-shift/replication-policy2.png)



### <a name="enable-replication"></a>Aktivieren der Replikation

Starten Sie nun die Replikation der VMs. Site Recovery installiert Mobility Service auf jeder VM, wenn Sie die Replikation dafür aktivieren. 


1. Klicken Sie auf **Anwendung replizieren** > **Quelle**.
2. Wählen Sie unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer**.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus.
5. Wählen Sie den Prozessserver (Konfigurationsserver) aus. Wählen Sie dann **OK**aus.

    ![Aktivieren der Replikation](./media/migrate-scenarios-lift-and-shift/enable-replication1.png)

1. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. in der Sie die virtuellen Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).
2. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten.
3. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen sollen.
4. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen.
5. Wählen Sie das Subnetz im virtuellen Netzwerk aus. Wählen Sie dann **OK**aus.

    ![Aktivieren der Replikation](./media/migrate-scenarios-lift-and-shift/enable-replication2.png)

6. Wählen Sie unter **Virtuelle Computer** > **Virtuelle Computer auswählen** die gewünschte VM (WEBVM) aus. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. 

    ![Aktivieren der Replikation](./media/migrate-scenarios-lift-and-shift/enable-replication3.png)

7. Überprüfen Sie zur Überwachung der hinzugefügten virtuellen Computer den Zeitpunkt der letzten Ermittlung für virtuelle Computer unter **Konfigurationsserver** > **Letzter Kontakt um**. Wenn Sie virtuelle Computer hinzufügen möchten, ohne auf die geplante Ermittlung zu warten, markieren Sie den Konfigurationsserver (wählen Sie ihn aber nicht aus), und klicken Sie auf **Aktualisieren**. Es kann länger als 15 Minuten dauern, bis die Änderungen wirksam und im Portal angezeigt werden.
8. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Konto aus, das der Prozessserver zum automatischen Installieren von Mobility Service auf dem Computer verwenden soll. 

    ![Aktivieren der Replikation](./media/migrate-scenarios-lift-and-shift/enable-replication4.png)

9. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist.
10. Klicken Sie auf **Replikation aktivieren**.

    ![Aktivieren der Replikation](./media/migrate-scenarios-lift-and-shift/enable-replication5.png)

11. Verfolgen Sie den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge**. 
12. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover und wird unter **Übersicht** > **Zusammenfassung** angezeigt.

    ![Zusammenfassung](./media/migrate-scenarios-lift-and-shift/essentials.png)


## <a name="step-8-migrate-the-database-with-dms"></a>Schritt 8: Migrieren der Datenbank mit DMS

Erstellen Sie ein DMS-Projekt, und führen Sie die Migration aus.


### <a name="create-a-database-migration-project"></a>Erstellen eines Datenbankmigrationsprojekts

1. Suchen Sie die DMS-Ressource im Azure-Portal, und öffnen Sie sie.
2. Wählen Sie **+ Neues Migrationsprojekt** aus.
3. Geben Sie unter **Neues Migrationsprojekt** einen Namen für das Projekt an.
4. Wählen Sie unter **Typ des Quellservers** die Option **SQL Server**. Wählen Sie unter **Typ des Zielservers** die Option **Verwaltete Azure SQL-Datenbank-Instanz** aus.
5. Klicken Sie auf **Erstellen** , um das Projekt zu erstellen.
6. Geben Sie unter **Quelldetails** die Verbindungsdetails für die lokale SQL Server-Quellinstanz an. Klicken Sie auf **Speichern**.
7. Geben Sie unter **Zieldetails** die Verbindungsdetails für das Ziel an, d.h. für die zuvor bereitgestellte verwaltete Azure SQL-Datenbank-Instanz, zu der die lokale Datenbank migriert wird. Klicken Sie anschließend auf **Speichern**.
8. Überprüfen Sie unter **Projektzusammenfassung** die dem Migrationsprojekt zugeordneten Details.

    ![DMS-Projekt](./media/migrate-scenarios-lift-and-shift/dms-project.png)

### <a name="migrate-the-database"></a>Migrieren der Datenbank

1. Nach dem Erstellen des Projekts wird der Migrations-Assistent angezeigt.
2. Geben Sie die Anmeldeinformationen für die Quell- und Zielserver an, und klicken Sie auf **Speichern**. Der Assistent versucht, sich bei Ihrem lokalen SQL Server anzumelden.

    ![DMS-Assistent](./media/migrate-scenarios-lift-and-shift/dms-wiz1.png)

3. Wählen Sie unter **Zu Zieldatenbanken zuordnen** die zu migrierende Quelldatenbank aus. Klicken Sie anschließend auf **Speichern**.

    ![DMS-Assistent](./media/migrate-scenarios-lift-and-shift/dms-wiz2.png)

4. Wählen Sie unter **Zieldetails** die Option **Ich kenne die Zieldetails** aus. Geben Sie den DNS-Namen Ihrer verwalteten SQL-Instanzen zusammen mit den Anmeldeinformationen an. Klicken Sie anschließend auf **Speichern**.

    ![DMS-Assistent](./media/migrate-scenarios-lift-and-shift/dms-wiz3.png)

5. Wählen Sie im gespeicherten Projekt die Option **+Neue Aktivität** aus. Wählen Sie anschließend **Migration ausführen** aus.
6. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für die Quell- und Zielserver ein. Klicken Sie anschließend auf **Speichern**.
7. Wählen Sie unter **Zu Zieldatenbanken zuordnen** die zu migrierende Quelldatenbank aus. Klicken Sie dann auf **Speichern**
8. Geben Sie unter **Migrationseinstellungen konfigurieren** > **Speicherort für die Sicherung** die Netzwerkfreigabe an, die Sie auf dem lokalen Computer erstellt haben. DMS speichert Quellsicherungen in dieser Freigabe.  Denken Sie daran, dass das Dienstkonto, mit dem die SQL Server-Quellinstanz ausgeführt wird, über Schreibberechtigungen für diese Freigabe verfügen muss.
9. Geben Sie den Benutzernamen und das Kennwort an, die von DMS zum Hochladen der Sicherungsdateien in den Azure-Speichercontainer für die Wiederherstellung verwendet werden kann.
10. Geben Sie den SAS-URI an, der DMS Zugriff auf Ihren Speicherkontocontainer gewährt, in den der Dienst die Sicherungsdateien hochlädt und der für die Migration zur verwalteten Azure SQL-Datenbank-Instanz verwendet wird.  Klicken Sie anschließend auf **Speichern**.
11. Geben Sie unter **Migrationszusammenfassung** einen Namen für die Migrationsaktivität an, und****  führen Sie die Migration aus.
12. Überwachen Sie im Projekt unter **Übersicht** den Migrationsstatus. Stellen Sie nach Abschluss der Migration sicher, dass die Zieldatenbanken in der verwalteten Instanz vorhanden sind.


## <a name="step-9-migrate-vms-with-site-recovery"></a>Schritt 9: Migrieren von VMs mit Site Recovery

Es wird empfohlen, anhand eines Testfailovers sicherzustellen, dass alles wie erwartet funktioniert, bevor Sie eine vollständige Migration durchführen. Beim Durchführen eines Testfailovers geschieht Folgendes:

1. Eine Überprüfung der erforderlichen Komponenten wird ausgeführt, um sicherzustellen, dass alle Bedingungen für eine Migration erfüllt sind.
2. Durch das Failover werden die Daten verarbeitet, sodass eine Azure-VM erstellt werden kann. Wenn Sie den letzten Wiederherstellungspunkt auswählen, wird ein Wiederherstellungspunkt auf der Grundlage der Daten erstellt.
3. Eine Azure-VM wird anhand der im vorherigen Schritt verarbeiteten Daten erstellt.

### <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente** > VM.
2. Klicken Sie unter den VM-Eigenschaften auf **+Testfailover**.

    ![Testfailover](./media/migrate-scenarios-lift-and-shift/test-failover.png)

3. Wählen Sie **Letzte Verarbeitung** aus, um für die VM ein Failover auf den letzten verfügbaren Zeitpunkt durchzuführen. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten aufgewendet, und der RTO-Wert (Recovery Time Objective) wird niedrig gehalten.
2. Wählen Sie unter **Testfailover** das Azure-Zielnetzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
3. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt im Tresor unter **Einstellungen** > **Aufträge** > **Testfailover** verfolgen.
4. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Vergewissern Sie sich, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird. Sie sollten nun eine Verbindung mit der replizierten VM in Azure herstellen können.
5. Klicken Sie zum Löschen der während des Testfailovers erstellten Azure-VMs auf **Testfailover bereinigen** für die VM. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.

### <a name="migrate-the-machines"></a>Migrieren der Computer

Nachdem Sie sichergestellt haben, dass das Testfailover wie erwartet funktioniert, erstellen Sie einen Wiederherstellungsplan für die Migration zu Azure.

### <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

1. Wählen Sie im Tresor nacheinander **Wiederherstellungspläne (Site Recovery)** > **+Wiederherstellungsplan** aus.
2. Geben Sie unter **Wiederherstellungsplan erstellen** einen Namen für den Plan an.
3. Wählen Sie den Quellkonfigurationsserver und Azure als Ziel aus. Wählen Sie für das Bereitstellungsmodell die Option **Resource Manager** aus. Der Quellort muss Computer aufweisen, die für Failover und Wiederherstellung aktiviert sind. 
4. Fügen Sie unter **Elemente auswählen** die Computer (WEBVM) zum Plan hinzu. Klicken Sie dann auf **OK**.
5. Klicken Sie auf **OK**, um den Plan zu erstellen.

    ![Wiederherstellungsplan](./media/migrate-scenarios-lift-and-shift/recovery-plan1.png)

### <a name="run-a-failover"></a>Ausführen eines Failovers

1. Klicken Sie unter **Wiederherstellungspläne** auf den Plan und dann auf **Failover**.
2. Wählen Sie unter **Failover** > **Wiederherstellungspunkt** den letzten Wiederherstellungspunkt aus.
3. Die Einstellung für den Verschlüsselungsschlüssel ist für dieses Szenario nicht relevant.
4. Wählen Sie **Computer vor Beginn des Failovers herunterfahren** aus. Site Recovery versucht, virtuelle Quellcomputer herunterzufahren, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover1.png)

5. Überprüfen Sie, ob die Azure-VM wie erwartet in Azure angezeigt wird.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover2.png)

6. Klicken Sie in **Replizierte Elemente** mit der rechten Maustaste auf die VM > **Migration abschließen**. Dadurch wird der Migrationsvorgang abgeschlossen, die Replikation für die VM wird beendet, und die Site Recovery-Abrechnung für die VM wird eingestellt.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover3.png)

### <a name="update-the-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Der letzte Schritt im Migrationsprozess ist das Aktualisieren der Verbindungszeichenfolge der Anwendung, um auf die migrierte Datenbank zu verweisen, die auf der verwalteten SQL-Instanz ausgeführt wird.

1. Um nach der neuen Verbindungszeichenfolge im Azure-Portal zu suchen, klicken Sie auf **Einstellungen** > **Verbindungszeichenfolgen**.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover4.png)  

2. Sie müssen diese Zeichenfolge mit dem Benutzernamen und dem Kennwort für die verwaltete SQL-Instanz aktualisieren.
3. Nachdem die Zeichenfolge konfiguriert wurde, müssen Sie die aktuelle Verbindungszeichenfolge in der Datei "web.config" Ihrer Anwendung ersetzen.
4. Nachdem Sie die Datei aktualisiert und gespeichert haben, starten Sie IIS auf WEBVM neu. Dazu können Sie IISRESET /RESTART in einer Eingabeaufforderung verwenden.
5. Nachdem IIS neu gestartet wurde, verwendet Ihre Anwendung die Datenbank, die auf der verwalteten SQL-Instanz ausgeführt wird.
6. An diesem Punkt kann der lokale SQLVM-Computer heruntergefahren werden, und die Migration ist abgeschlossen.



## <a name="conclusion"></a>Zusammenfassung

In diesem Szenario haben wir Folgendes durchgeführt:

> [!div class="checklist"]
> * Migrieren unserer lokalen Datenbank zu einer verwalteten Azure SQL-Instanz mit DMS
> * Migrieren unserer lokalen VMs zu Azure-VMs mit dem Azure Site Recovery-Dienst
