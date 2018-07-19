---
title: Bereitstellen des Konfigurationsservers für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie einen Konfigurationsserver für die VMware-Notfallwiederherstellung in Azure Site Recovery bereitstellen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 240f5270d083fa5f4742f3ed2cd61feee2b635ec
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38718956"
---
# <a name="deploy-a-configuration-server"></a>Bereitstellen eines Konfigurationsservers

Sie stellen einen lokalen Konfigurationsserver bereit, wenn Sie [Azure Site Recovery](site-recovery-overview.md) für die Notfallwiederherstellung von VMware-VMs und physischen Servern in Azure verwenden. Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure. Darüber hinaus verwaltet er die Datenreplikation. In diesem Artikel werden die erforderlichen Schritte zum Bereitstellen des Konfigurationsservers beim Replizieren virtueller VMware-Computer in Azure beschrieben. Befolgen Sie die Schritte in [diesem Artikel](physical-azure-set-up-source.md), wenn Sie einen Konfigurationsserver für die Replikation physischer Server einrichten müssen.

>[!TIP]
Weitere Informationen zur Rolle des Konfigurationsservers als Bestandteil der Azure Site Recovery-Architektur finden Sie [hier](vmware-azure-architecture.md).

## <a name="deployment-of-configuration-server-through-ova-template"></a>Bereitstellung des Konfigurationsservers über die OVA-Vorlage

Der Konfigurationsserver muss als hochverfügbare VMware-VM mit bestimmten Mindestanforderungen an die Hardware und Größenanpassung eingerichtet werden. Um eine unkomplizierte und einfache Bereitstellung zu ermöglichen, bietet Site Recovery eine zum Download verfügbare OVA-Vorlage (Open Virtualization Application), mit der der Konfigurationsserver entsprechend aller unten aufgeführten obligatorischen Anforderungen eingerichtet werden kann.

## <a name="prerequisites"></a>Voraussetzungen

Die Mindesthardwareanforderungen für einen Konfigurationsserver werden in der folgenden Tabelle zusammengefasst:

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="capacity-planning"></a>Kapazitätsplanung

Die Größenanforderungen für den Konfigurationsserver richten sich nach der potenziellen Datenänderungsrate. Verwenden Sie diese Tabelle als Richtlinie.

| **CPU** | **Arbeitsspeicher** | **Größe des Cachedatenträgers** | **Datenänderungsrate** | **Geschützte Computer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 Sockets * 4 Kerne mit 2,5 GHz) |16 GB |300 GB |500 GB oder weniger |Weniger als 100 Computer replizieren. |
| 12 vCPUs (2 Sockets * 6 Kerne mit 2,5 GHz) |18 GB |600 GB |500 GB bis 1 TB |100 bis 150 Computer replizieren |
| 16 vCPUs (2 Sockets * 8 Kerne mit 2,5 GHz) |32 GB |1 TB |1 TB bis 2 TB |150 bis 200 Computer replizieren |

Wenn Sie mehr als eine VMware-VM replizieren, lesen Sie den Artikel [Überlegungen zur Kapazitätsplanung](/site-recovery-plan-capacity-vmware.md). Führen Sie das [Bereitstellungsplanertool](site-recovery-deployment-planner.md) für die VMware-Replikation aus.

## <a name="download-the-template"></a>Herunterladen der Vorlage

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
4. Laden Sie die OVA-Vorlage (Open Virtualization Application) für den Konfigurationsserver herunter.

  > [!TIP]
>Die neueste Version der Konfigurationsservervorlage können Sie auch direkt vom [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.

>[!NOTE]
Bei der mit der OVA-Vorlage bereitgestellten Lizenz handelt es sich um eine Evaluierungslizenz mit einer Gültigkeit von 180 Tagen. Nach Ablauf dieses Zeitraums muss der Kunde die Windows-Version mit einer käuflich erworbenen Lizenz aktivieren.

## <a name="import-the-template-in-vmware"></a>Importieren der Vorlage in VMware

1. Melden Sie sich beim VMware vCenter-Server oder beim vSphere ESXi-Host über den VMWare vSphere-Client an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den Assistenten zum Bereitstellen von OVF-Vorlagen zu starten.

     ![OVF-Vorlage](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie in **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie in **Speicher auswählen** unter **Select virtual disk format** (Format für virtuellen Datenträger auswählen) für eine optimale Leistung **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus.
7. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
8. Unter **Bereit für den Abschluss**:

    * Wählen Sie zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.

    * Wenn Sie eine weitere Netzwerkschnittstelle hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten), und wählen Sie dann **Fertig stellen** aus. Standardmäßig wird die Konfigurationsservervorlage mit einer einzelnen NIC bereitgestellt. Weitere NICs können nach der Bereitstellung hinzugefügt werden.

## <a name="add-an-additional-adapter"></a>Hinzufügen zusätzlicher Adapter

Wenn Sie dem Konfigurationsserver eine zusätzliche NIC hinzufügen möchten, führen Sie diesen Schritt aus, bevor Sie den Server im Tresor registrieren. Das Hinzufügen von zusätzlichen Adaptern nach der Registrierung wird nicht unterstützt.

1. Klicken Sie im vSphere-Client mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Einstellungen bearbeiten** aus.
2. Klicken Sie unter **Hardware** auf **Hinzufügen** > **Ethernet-Adapter**. Klicken Sie anschließend auf **Weiter**.
3. Wählen Sie einen Adaptertyp und ein Netzwerk aus. 
4. Um die virtuelle NIC zu verbinden, wenn die VM Computer eingeschaltet wird, wählen Sie **Connect at power-on** (Beim Einschalten verbinden) aus. Klicken Sie anschließend auf **Weiter** > **Fertig stellen** > **OK**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Registrieren des Konfigurationsservers bei Azure Site Recovery-Diensten

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Benutzeroberfläche für die Installation von Windows Server 2016 hochgefahren. Akzeptieren Sie den Lizenzvertrag, und geben Sie ein Administratorkennwort ein.
3. Melden Sie sich nach Abschluss der Installation als Administrator am virtuellen Computer an.
4. Bei der ersten Anmeldung wird das Azure Site Recovery-Konfigurationstool innerhalb weniger Sekunden gestartet.
5. Geben Sie einen Namen ein, der für die Registrierung des Konfigurationsservers bei Site Recovery verwendet wird. Klicken Sie anschließend auf **Weiter**.
6. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden. Die Anmeldeinformationen müssen über Zugriff auf den Tresor verfügen, in dem Sie den Konfigurationsserver registrieren möchten.
7. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
8. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird innerhalb weniger Sekunden **automatisch** gestartet.

### <a name="configure-settings"></a>Konfigurieren von Einstellungen

1. Klicken Sie im Assistenten für die Konfigurationsserververwaltung auf **Konnektivität einrichten**, und wählen Sie anschließend die NIC aus, die der Verarbeitungsserver nutzt, um den Replikationsdatenverkehr zu empfangen. Klicken Sie dann auf **Speichern**. Diese Einstellung kann nach der Konfiguration nicht mehr geändert werden.
2. Melden Sie sich unter **Recovery Services-Tresor auswählen** bei Microsoft Azure an, und wählen Sie Ihr Azure-Abonnement, die entsprechende Ressourcengruppe sowie den entsprechenden Tresor aus.

    > [!NOTE]
    > Nach der Registrierung kann der Recovery Services-Tresor nicht geändert werden.
    
3. Gehen Sie unter **Drittanbietersoftware installieren** wie folgt vor:

    |Szenario   |Vorgehensweise  |
    |---------|---------|
    |Kann ich MySQL manuell herunterladen und installieren?     |  Ja. Laden Sie die Anwendung MySQL manuell herunter, platzieren Sie sie im Ordner **C:\Temp\ASRSetup**, und installieren Sie sie manuell. Wenn Sie mit den Bedingungen einverstanden sind, klicken Sie auf **Herunterladen und installieren**. Im Portal wird die Meldung *Bereits installiert* angezeigt. Sie können mit dem nächsten Schritt fortfahren.       |
    |Ist es möglich, das Herunterladen von MySQL über das Internet zu umgehen?     |   Ja. Platzieren Sie Ihr MySQL-Installationsprogramm im Ordner **C:\Temp\ASRSetup**. Akzeptieren Sie die Bedingungen, und klicken Sie auf **Herunterladen und installieren**. Das Portal verwendet dann das von Ihnen hinzugefügte Installationsprogramm und installiert die Anwendung. Sie können nach der Installation mit dem nächsten Schritt fortfahren.    |
    |Kann ich MySQL über Azure Site Recovery herunterladen und installieren?     |  Akzeptieren Sie den Lizenzvertrag, und klicken Sie auf **Herunterladen und installieren**. Sie können nach der Installation dann mit dem nächsten Schritt fortfahren.       |
4. Unter **Anwendungskonfiguration überprüfen** werden die Voraussetzungen überprüft, bevor der Vorgang fortgesetzt wird.
5. Geben Sie unter **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port ein, über den der Server lauscht. Geben Sie einen Anzeigenamen ein, der für den VMware-Server im Tresor verwendet werden soll.
6. Geben Sie die Anmeldeinformationen ein, die der Konfigurationsserver beim Herstellen der Verbindung mit dem VMware-Server verwenden soll. Site Recovery verwendet diese Anmeldeinformationen für die automatische Erkennung von VMware-VMs, die für die Replikation verfügbar sind. Klicken Sie auf **Hinzufügen** und anschließend auf **Weiter**. Die hier eingegebenen Anmeldeinformationen werden lokal gespeichert.
7. Geben Sie unter **Anmeldeinformationen für virtuelle Computer konfigurieren** den Benutzernamen und das Kennwort der virtuellen Computer an, um den Mobility Service während der Replikation automatisch installieren zu lassen. Für **Windows**-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten. Bei **Linux** geben Sie die Anmeldeinformationen für das Stammkonto an.
8. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen.
9. Nachdem die Registrierung abgeschlossen ist, öffnen Sie das Azure-Portal, und stellen Sie sicher, dass der Konfigurationsserver und der VMware-Server unter **Recovery Services-Tresor** > **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver** aufgeführt sind.

## <a name="faq"></a>Häufig gestellte Fragen

1. Kann ich die VM, auf der der Konfigurationsserver installiert ist, für andere Zwecke einsetzen?

    **Nein**. Es wird empfohlen, die VM ausschließlich als Konfigurationsserver einzusetzen. Vergewissern Sie sich, dass alle im [vorherigen Abschnitt](vmware-azure-deploy-configuration-server.md#Prerequisites) genannten Spezifikationen für eine effiziente Verwaltung von Notfallwiederherstellungen eingehalten werden.
2. Kann ich den Tresor, der bereits auf dem Konfigurationsserver registriert ist, mit einem neu erstellten Tresor austauschen?

    **Nein**, sobald ein Tresor beim Konfigurationsserver registriert ist, sind keine Änderungen daran möglich.
3. Kann ich den gleichen Konfigurationsserver zum Schutz von physischen und virtuellen Computern verwenden?

    **Ja**, ein Konfigurationsserver kann zum Replizieren von physischen und virtuellen Computern verwendet werden. Allerdings kann ein physischer Computer nur auf eine VMware-VM zurückgesetzt werden.
4. Welchen Zweck erfüllt ein Konfigurationsserver, und wo wird er eingesetzt?

    Weitere Informationen zum Konfigurationsserver und seinen Funktionen finden Sie im [Artikel zu unserer Azure Site Recovery-Architektur](vmware-azure-architecture.md).
5. Wo erhalte ich die neueste Version des Konfigurationsservers?

    Den Artikel mit den einzelnen Schritten zum Durchführen eines Upgrades für den Konfigurationsserver finden Sie [im Portal](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server). Sie können das Tool auch direkt aus dem [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.
6. Wo kann ich die Passphrase für den Konfigurationsserver herunterladen?

    Informationen zum Herunterladen der Passphrase finden Sie in [diesem Artikel](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase).
7. Wo kann ich die Tresorregistrierungsschlüssel herunterladen?

    Im **Recovery Services-Tresor**, **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver**. Klicken Sie unter „Server“ auf **Registrierungsschlüssel herunterladen**, um die Datei mit den Tresoranmeldeinformationen herunterzuladen.

## <a name="upgrade-the-configuration-server"></a>Aktualisieren Sie den Konfigurationsserver

Um den Konfigurationsserver auf die neueste Version zu aktualisieren, lesen Sie die [hier](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) aufgeführten Schritte.

## <a name="troubleshoot-deployment-issues"></a>Problembehandlung bei Bereitstellungsproblemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Nächste Schritte

[Einrichten der Notfallwiederherstellung in Azure für lokale VMware-VMs](vmware-azure-tutorial.md)
