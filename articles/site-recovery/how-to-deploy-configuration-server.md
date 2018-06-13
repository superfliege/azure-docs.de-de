---
title: Bereitstellen des Konfigurationsservers für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen Konfigurationsserver für die VMware-Notfallwiederherstellung in Azure Site Recovery bereitstellen.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 02/25/2018
ms.author: anoopkv
ms.openlocfilehash: f7ec02cb4723d8a4ea0847810b1a1822cf20e957
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
ms.locfileid: "29767765"
---
# <a name="deploy-a-configuration-server"></a>Bereitstellen eines Konfigurationsservers

Sie stellen einen lokalen Konfigurationsserver bereit, wenn Sie [Azure Site Recovery](site-recovery-overview.md) für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure verwenden. Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure. Darüber hinaus verwaltet er die Datenreplikation. In diesem Artikel werden die erforderlichen Schritte zum Bereitstellen des Konfigurationsservers beim Replizieren virtueller VMware-Computer in Azure beschrieben. Befolgen Sie die Schritte in [diesem Artikel](site-recovery-set-up-physical-to-azure.md), wenn Sie einen Konfigurationsserver für die Replikation physischer Server einrichten müssen.

## <a name="prerequisites"></a>Voraussetzungen

Es wird empfohlen, den Konfigurationsserver als hoch verfügbare VMware-VM bereitzustellen. Die Hardwareanforderungen werden in der folgenden Tabelle zusammengefasst.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Kapazitätsplanung

Die Größenanforderungen für den Konfigurationsserver richten sich nach der potenziellen Datenänderungsrate. Verwenden Sie diese Tabelle als Richtlinie.

| **CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz) |16 GB |300 GB |500 GB oder weniger |Weniger als 100 Computer replizieren. |
| 12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz) |18 GB |600 GB |500 GB bis 1 TB |100 bis 150 Computer replizieren |
| 16 vCPUs (2 Sockets * 8 Kerne mit 2,5 GHz) |32 GB |1 TB |1 TB bis 2 TB |150 bis 200 Computer replizieren |


Wenn Sie virtuelle VMware-Computer replizieren, lesen Sie mehr über [Überlegungen zur Kapazitätsplanung](/site-recovery-plan-capacity-vmware.md). Führen Sie das [Bereitstellungsplanertool](site-recovery-deployment-planner.md) für die VMware-Replikation aus.



## <a name="download-the-template"></a>Herunterladen der Vorlage

Site Recovery bietet eine herunterladbare Vorlage zum Einrichten des Konfigurationsservers als hoch verfügbare VMware-VM. 

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
4. Laden Sie die OVF-Vorlage (Open Virtualization Format) für den Konfigurationsserver herunter.

  > [!TIP]
  Die neueste Version der Konfigurationsservervorlage können Sie direkt aus dem [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.


## <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware


1. Melden Sie sich beim VMware vCenter-Server oder beim vSphere ESXi-Host über den VMWare vSphere-Client an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den Assistenten zum Bereitstellen von OVF-Vorlagen zu starten.

     ![OVF-Vorlage](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie in **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie in **Speicher auswählen** unter **Select virtual disk format** (Format für virtuellen Datenträger auswählen) für eine optimale Leistung **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus.
4. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
5. Unter **Bereit für den Abschluss**:

    * Wählen Sie zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.

    * Wenn Sie eine weitere Netzwerkschnittstelle hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten), und wählen Sie dann **Fertig stellen** aus. Standardmäßig wird die Konfigurationsservervorlage mit einer einzelnen NIC bereitgestellt. Weitere NICs können nach der Bereitstellung hinzugefügt werden.


## <a name="add-an-additional-adapter"></a>Hinzufügen zusätzlicher Adapter

Wenn Sie dem Konfigurationsserver eine zusätzliche NIC hinzufügen möchten, führen Sie diesen Schritt aus, bevor Sie den Server im Tresor registrieren. Das Hinzufügen von zusätzlichen Adaptern nach der Registrierung wird nicht unterstützt.

1. Klicken Sie im vSphere-Client mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Einstellungen bearbeiten** aus.
2. Klicken Sie unter **Hardware** auf **Hinzufügen** > **Ethernet-Adapter**. Klicken Sie anschließend auf **Weiter**.
3. Wählen Sie einen Adaptertyp und ein Netzwerk aus. 
4. Um die virtuelle NIC zu verbinden, wenn der virtuelle Computer eingeschaltet wird, wählen Sie **Connect at power on** (Beim Einschalten verbinden) aus. Klicken Sie anschließend auf **Weiter** > **Fertig stellen** > **OK**.
 

## <a name="register-the-configuration-server"></a>Registrieren des Konfigurationsservers 

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Benutzeroberfläche für die Installation von Windows Server 2016 hochgefahren. Akzeptieren Sie den Lizenzvertrag, und geben Sie ein Administratorkennwort ein.
3. Melden Sie sich nach Abschluss der Installation als Administrator am virtuellen Computer an.
4. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool gestartet.
5. Geben Sie einen Namen ein, der für die Registrierung des Konfigurationsservers bei Site Recovery verwendet wird. Klicken Sie anschließend auf **Weiter**.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.
7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird automatisch gestartet.

### <a name="configure-settings"></a>Konfigurieren von Einstellungen

1. Wählen Sie im Assistent für die Konfigurationsserververwaltung die Option **Konnektivität einrichten**. Wählen Sie die NIC zum Empfangen von Replikationsdatenverkehr aus, und klicken Sie dann auf **Speichern**. Sie können diese Einstellung nach der Konfiguration nicht mehr ändern.
2. Wählen Sie unter **Recovery Services-Tresor auswählen** Ihr Azure-Abonnement, die entsprechende Ressourcengruppe und den entsprechenden Tresor aus.
3. Akzeptieren Sie unter **Drittanbietersoftware installieren** den Lizenzvertrag. Klicken Sie auf **Herunterladen und installieren**, um MySQL-Server zu installieren.
4. Klicken Sie auf **VMware PowerCLI installieren**. Stellen Sie sicher, dass alle Browserfenster geschlossen sind, bevor Sie diesen Schritt ausführen. Klicken Sie anschließend auf **Weiter**.
5. Unter **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor der Vorgang fortgesetzt wird.
6. Geben Sie unter **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port, an dem der Server lauscht, sowie einen Anzeigenamen für den VMware-Server im Tresor ein.
7. Geben Sie die Anmeldeinformationen ein, die der Konfigurationsserver beim Herstellen der Verbindung mit dem VMware-Server verwenden soll. Site Recovery verwendet diese Anmeldeinformationen für die automatische Erkennung von VMware-VMs, die für die Replikation verfügbar sind. Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter**.
8. Geben Sie unter **Anmeldeinformationen für virtuelle Computer konfigurieren** den Benutzernamen und das Kennwort an, die für die automatische Installation von Azure Site Recovery Mobility Service auf Computern verwendet werden sollen, wenn die Replikation aktiviert ist. Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten. Bei Linux geben Sie die Anmeldeinformationen für das root-Konto an.
9. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen. 
10. Überprüfen Sie nach Abschluss der Registrierung im Azure-Portal, ob der Konfigurationsserver und der VMware-Server auf der Seite **Quelle** im Tresor aufgeführt werden. Klicken Sie dann auf **OK**, um die Einstellungen für das Ziel zu konfigurieren.


## <a name="troubleshoot-deployment-issues"></a>Problembehandlung bei Bereitstellungsproblemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Nächste Schritte

[Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs](tutorial-vmware-to-azure.md)
