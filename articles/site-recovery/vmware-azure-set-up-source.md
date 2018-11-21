---
title: Einrichten der Quellumgebung und des Konfigurationsservers für die Notfallwiederherstellung von virtuellen VMware-Computern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre lokale Umgebung und den Konfigurationsserver für die Notfallwiederherstellung von virtuellen VMware-Computern in Azure mithilfe von Azure Site Recovery einrichten.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: e09e93ef85449c51e35b8da7ad93ee7088a0e7b4
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566311"
---
# <a name="set-up-the-source-environment-and-configuration-server"></a>Einrichten der Quellumgebung und des Konfigurationsservers

In diesem Artikel wird beschrieben, wie Sie Ihre lokale Quellumgebung für die Notfallwiederherstellung von virtuellen VMware-Computern in Azure mithilfe von [Azure Site Recovery](site-recovery-overview.md) einrichten. Er enthält die Schritte zum Einrichten eines lokalen Computers als Site Recovery-Konfigurationsserver und zum automatischen Ermitteln von lokalen virtuellen Computern. 

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie Folgendes bereits durchgeführt haben:

- Planen der Bereitstellung der Notfallwiederherstellung mit dem [Azure Site Recovery-Bereitstellungsplaner](site-recovery-deployment-planner.md). Mit dem Planer können Sie sicherstellen, dass Sie über ausreichende Ressourcen und Bandbreite für die Notfallwiederherstellung und die Recovery Point Objective (RPO) in Ihrer Organisation verfügen.
- [Einrichten von Azure-Ressourcen](tutorial-prepare-azure.md) im [Azure-Portal](http://portal.azure.com)
- [Einrichten von lokalen VMware-Ressourcen](vmware-azure-tutorial-prepare-on-premises.md), einschließlich eines dedizierten Kontos für die automatische Ermittlung von lokalen virtuellen VMs-Computern.
- Informationen zur Bereitstellung und Verwaltung des Konfigurationsservers finden Sie in den [allgemeinen Fragen](vmware-azure-common-questions.md#configuration-server).


## <a name="choose-protection-goals"></a>Auswählen der Schutzziele

1. Wählen Sie unter **Recovery Services-Tresore** den Tresornamen aus. 
2. Wählen Sie unter **Erste Schritte** die Option „Site Recovery“ aus. Klicken Sie anschließend auf **Infrastruktur vorbereiten**.
3. Wählen Sie in **Schutzziel** > **Wo befinden sich Ihre Computer?** die Option **Lokal** aus.
4. Wählen Sie in **Wohin möchten Sie Ihre Computer replizieren?** die Option **Nach Azure** aus.
5. Wählen Sie in **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus. Wählen Sie dann **OK**aus.

## <a name="about-the-configuration-server"></a>Informationen zum Konfigurationsserver

Beim Einrichten der Notfallwiederherstellung von virtuellen VMware-Computern in Azure stellen Sie einen lokalen Konfigurationsserver bereit.

- Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen VMware-Umgebung und Azure. Darüber hinaus verwaltet er die Datenreplikation.
- Sie stellen den Konfigurationsserver als virtuellen VMware-Computer bereit.
- Site Recovery stellt eine OVA-Vorlage bereit, die Sie aus dem Azure-Portal herunterladen und in vCenter Server importieren, um die Konfigurationsserver-VM einzurichten.
- [Erfahren Sie mehr](vmware-azure-architecture.md) zu den Komponenten und Prozessen von Konfigurationsservern.


>[!NOTE]
Verwenden Sie diese Anweisungen nicht, wenn Sie den Konfigurationsserver für die Notfallwiederherstellung von lokalen physischen Computern in Azure bereitstellen. Die Anweisungen für dieses Szenario finden Sie in [diesem Artikel](physical-azure-set-up-source.md).


## <a name="before-you-deploy-the-configuration-server"></a>Vor dem Bereitstellen des Konfigurationsservers

Wenn Sie den Konfigurationsserver mithilfe der OVA-Vorlage als virtuellen VMware-Computer installieren, wird der virtuelle Computer mit allen erforderlichen Komponenten installiert. Informationen zu den erforderlichen Komponenten finden Sie in den folgenden Artikeln.

- [Erfahren Sie mehr über](vmware-azure-configuration-server-requirements.md) die Hardware-, Software- und Kapazitätsanforderungen für den Konfigurationsserver.
- Wenn Sie mehrere virtuelle VMware-Computer replizieren, sollten Sie sich den Artikel [Planen der Kapazität und Skalierung der VMware-Notfallwiederherstellung für Azure](site-recovery-plan-capacity-vmware.md) durchlesen und das Tool [Azure Site Recovery-Bereitstellungsplaner](site-recovery-deployment-planner.md) für die VMWare-Replikation ausführen.
- Die Windows-Lizenz für die OVA-Vorlage ist eine Evaluierungslizenz, die 180 Tage lang gültig ist. Nach diesem Zeitraum müssen Sie Windows mit einer gültigen Lizenz aktivieren. 
- Mit der OVA-Vorlage kann der Konfigurationsserver auf einfache Weise als virtueller VMware-Computer eingerichtet werden. Wenn Sie aus einem bestimmten Grund den virtuellen VMware-Computer ohne die Vorlage einrichten möchten, informieren Sie sich über die erforderlichen Komponenten, und befolgen Sie die Anweisungen in [diesem Artikel](physical-azure-set-up-source.md).
- Für Ihr Azure-Konto sind Berechtigungen zum Erstellen von Azure AD-Apps erforderlich.


>[!IMPORTANT]
Der Konfigurationsserver muss wie in diesem Artikel beschrieben bereitgestellt werden. Das Kopieren oder Klonen eines vorhandenen Konfigurationsservers ist nicht möglich.

### <a name="set-up-azure-account-permissions"></a>Einrichten der Azure-Kontoberechtigungen

Ein globaler oder ein Mandantenadministrator kann dem Konto Berechtigungen zum Erstellen von Azure AD-Apps oder die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.


Der globale oder der Mandantenadministrator kann wie folgt Berechtigungen für das Konto zuweisen:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen.

 > [!NOTE]
 > Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Alternativ verfügt der globale oder der Mandantenadministrator über Berechtigungen zum Zuweisen der Rolle zum Konto. [Weitere Informationen](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

 


## <a name="download-the-ova-template"></a>Herunterladen der OVA-Vorlage

1. Navigieren Sie im Tresor zu **Infrastruktur vorbereiten** > **Quelle**.
2. Klicken Sie unter **Quelle vorbereiten** auf **+Konfigurationsserver**.
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver für VMware** angezeigt wird.
4. Laden Sie die OVA-Vorlage (Open Virtualization Application) für den Konfigurationsserver herunter.

  > [!TIP]
>Die neueste Version der Konfigurationsservervorlage können Sie auch vom [Microsoft Download Center](https://aka.ms/asrconfigurationserver) herunterladen.


## <a name="import-the-ova-template-in-vmware"></a>Importieren der OVA-Vorlage in VMware

1. Melden Sie sich mit dem VMWare vSphere-Client beim VMware vCenter-Server oder beim vSphere ESXi-Host an.
2. Wählen Sie im Menü **Datei** die Option **OVF-Vorlage bereitstellen** aus, um den Assistenten zum Bereitstellen von OVF-Vorlagen zu starten.

     ![OVF-Vorlage](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. Geben Sie unter **Quelle auswählen** den Speicherort der heruntergeladenen OVF-Vorlage ein.
4. Klicken Sie unter **Bewertungsdetails** auf **Weiter**.
5. Übernehmen Sie in **Name und Ordner auswählen** und **Konfiguration auswählen** die Standardeinstellungen.
6. Wählen Sie in **Speicher auswählen** unter **Select virtual disk format** (Format für virtuellen Datenträger auswählen) für eine optimale Leistung **Thick Provision Eager Zeroed** (Breite Bereitstellung gegen null) aus.
7. Übernehmen Sie auf den übrigen Seiten des Assistenten die Standardeinstellungen.
8. Wählen Sie unter **Zum Abschluss bereit** zum Einrichten des virtuellen Computers mit den Standardeinstellungen **Power on after deployment** (Nach Bereitstellung einschalten) > **Fertig stellen** aus.
9. Standardmäßig wird der virtuelle Computer mit einem einzelnen Netzwerkadapter bereitgestellt. Wenn Sie einen weiteren Netzwerkadapter hinzufügen möchten, deaktivieren Sie **Power on after deployment** (Nach Bereitstellung einschalten), und klicken Sie auf **Fertig stellen**. Führen Sie dann die nächsten Schritte aus.

## <a name="add-an-adapter-to-the-configuration-server"></a>Hinzufügen eines Adapters zum Konfigurationsserver

Wenn Sie dem Konfigurationsserver eine zusätzliche NIC hinzufügen möchten, führen Sie diesen Schritt aus, bevor Sie den Server im Tresor registrieren. Das Hinzufügen von zusätzlichen Adaptern nach der Registrierung wird nicht unterstützt.

1. Klicken Sie im vSphere-Client mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Einstellungen bearbeiten** aus.
2. Klicken Sie unter **Hardware** auf **Hinzufügen** > **Ethernet-Adapter**. Klicken Sie anschließend auf **Weiter**.
3. Wählen Sie einen Adaptertyp und ein Netzwerk aus.
4. Um die virtuelle NIC zu verbinden, wenn die VM Computer eingeschaltet wird, wählen Sie **Connect at power-on** (Beim Einschalten verbinden) aus. Klicken Sie anschließend auf **Weiter** > **Fertig stellen** > **OK**.

## <a name="register-the-configuration-server"></a>Registrieren des Konfigurationsservers 
Schalten Sie den virtuellen Computer ein, und registrieren Sie den Konfigurationsserver im Site Recovery-Tresor.

1. Schalten Sie den virtuellen Computer über die Konsole des VMware vSphere-Clients an.
2. Der virtuelle Computer wird mit der Windows Server 2016-Installationsoberfläche gestartet. Akzeptieren Sie den Lizenzvertrag, und geben Sie ein Administratorkennwort ein.
3. Melden Sie sich nach Abschluss der Installation als Administrator am virtuellen Computer an.



## <a name="set-up-the-configuration-server"></a>Einrichten des Konfigurationsservers

Als Bestandteil der Bereitstellung müssen Sie MySQL auf der Konfigurationsserver-VM installieren. Dazu haben Sie mehrere Möglichkeiten:

- Automatisches Herunterladen und Installieren von MySQL in Site Recovery: Wenn Sie diese Option verwenden möchten, müssen Sie vor dem Setup des Konfigurationsservers nichts weiter tun.
- Manuelles Herunterladen und Installieren von MySQL: Laden Sie vor dem Einrichten des Konfigurationsservers das MySQL-Installationsprogramm herunter, und speichern Sie es im Ordner **C:\Temp\ASRSetup**. Installieren Sie dann MySQL. 
- Manuelles Herunterladen und automatisches Installieren in Site Recovery: Laden Sie vor dem Einrichten des Konfigurationsservers das MySQL-Installationsprogramm herunter, und speichern Sie es im Ordner **C:\Temp\ASRSetup**.


1. Bei der ersten Anmeldung beim virtuellen Computer wird das Azure Site Recovery-Konfigurationstool gestartet.
2. Geben Sie einen Namen an, der für die Registrierung des Konfigurationsservers im Site Recovery-Tresor verwendet wird. Klicken Sie anschließend auf **Weiter**.
3. Das Tool überprüft, ob der virtuelle Computer eine Verbindung mit Azure herstellen kann. Klicken Sie nach der Verbindungsherstellung auf **Anmelden**, um sich bei Ihrem Azure-Abonnement anzumelden. Beachten Sie, dass das Konto über Zugriff auf den Tresor verfügen muss, in dem Sie den Konfigurationsserver registrieren möchten.
4. Das Tool führt einige Konfigurationsaufgaben und anschließend einen Neustart durch.
5. Melden Sie sich erneut am Computer an. Der Assistent für die Konfigurationsserververwaltung wird innerhalb weniger Sekunden automatisch gestartet.
6. Wählen Sie im Assistenten die Option **Konnektivität einrichten** aus.
7. Wählen Sie den Netzwerkadapter aus, über den der Verarbeitungsserver (der standardmäßig auf dem Konfigurationsserver ausgeführt wird) Replikationsdatenverkehr von virtuellen Computern empfängt.
8. Klicken Sie dann auf **Speichern**. Beachten Sie, dass Sie die Tresoreinstellungen nicht mehr ändern können, nachdem der Konfigurationsserver registriert wurde. 
9. Melden Sie sich unter **Recovery Services-Tresor auswählen** bei Microsoft Azure an, und wählen Sie Ihr Azure-Abonnement, die entsprechende Ressourcengruppe sowie den entsprechenden Tresor aus. 
10. Installieren Sie MySQL unter **Drittanbietersoftware installieren**:
     - Wenn der Download und die Installation von MySQL in Site Recovery automatisch durchgeführt werden, klicken Sie auf **Herunterladen und installieren**. Warten Sie, bis die Installation abgeschlossen ist, und fahren Sie dann mit dem Assistenten fort.
     - Wenn Sie MySQL heruntergeladen haben und es von Site Recovery automatisch installiert wird, klicken Sie auf **Herunterladen und installieren**. Warten Sie, bis die Installation abgeschlossen ist, und fahren Sie dann mit dem Assistenten fort.
     - Wenn Sie MySQL manuell heruntergeladen und installiert haben, klicken Sie auf **Herunterladen und installieren**. Die App wird als **Bereits installiert** angezeigt. Fahren Sie mit dem Assistenten fort.
11. Unter **Gerätekonfiguration überprüfen** werden die erforderlichen Komponenten überprüft, bevor der Vorgang fortgesetzt wird. 
12. Geben Sie in **vCenter-Server/vSphere ESXi-Server konfigurieren** den FQDN oder die IP-Adresse des vCenter-Servers oder vSphere-Hosts ein, auf dem sich die virtuellen Computer befinden, die repliziert werden sollen. Geben Sie den Port, an dem der Server lauscht, sowie einen Anzeigenamen für den VMware-Server im Tresor ein.
13. Geben Sie die Anmeldeinformationen ein, die auf dem Konfigurationsserver zum Herstellen der Verbindung mit dem VMware-Server und zum automatischen Ermitteln von virtuellen VMware-Computern verwendet werden, die zur Replikation verfügbar sind. Wählen Sie **Hinzufügen** >  **Weiter** aus. Die Anmeldeinformationen werden lokal gespeichert.
14. Geben Sie in **Anmeldeinformationen für virtuelle Computer konfigurieren** die Anmeldeinformationen an, die in Site Recovery für die automatische Installation des Mobility Service verwendet werden, wenn Sie die Replikation für einen virtuellen Computer aktivieren.
    - Für Windows-Computer benötigt das Konto lokale Administratorrechte auf den Computern, die Sie replizieren möchten.
    - Bei Linux geben Sie die Anmeldeinformationen für das root-Konto an.
15. Klicken Sie auf **Konfiguration abschließen**, um die Registrierung abzuschließen.
16. Nachdem die Registrierung abgeschlossen ist, öffnen Sie das Azure-Portal, und überprüfen Sie, ob der Konfigurationsserver und der VMware-Server unter **Recovery Services-Tresor** > **Verwalten** > **Site Recovery-Infrastruktur** > **Konfigurationsserver** aufgeführt sind.


## <a name="exclude-antivirus-on-the-configuration-server"></a>Ausschließen von Antivirensoftware auf dem Konfigurationsserver

Wenn auf der Konfigurationsserver-VM Antivirensoftware ausgeführt wird, stellen Sie sicher, dass die folgenden Ordner von Vorgängen der Antivirensoftware ausgeschlossen werden. Dadurch wird sichergestellt, dass die Replikation und Konnektivität wie erwartet funktionieren: 

- C:\Programme\Microsoft Azure Recovery Services Agent
- C:\Programme\Microsoft Azure Site Recovery Provider
- C:\Programme\Microsoft Azure Site Recovery Configuration Manager
- C:\Programme\Microsoft Azure Site Recovery Error Collection Tool
- C:\thirdparty
- C:\Temp
- C:\strawberry
- C:\ProgramData\MySQL
- C:\Programme (x86)\MySQL
- C:\ProgramData\ASR
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\inetpub
- Site Recovery-Installationsverzeichnis. Beispiel: E:\Programme (x86)\Microsoft Azure Site Recovery


## <a name="next-steps"></a>Nächste Schritte
- Wenn Probleme auftreten, finden Sie entsprechende Informationen in den [allgemeinen Fragen](vmware-azure-common-questions.md#configuration server) und unter [Problembehandlung](vmware-azure-troubleshoot-configuration-server.md) für den Konfigurationsserver.
- Nun können Sie in Azure [die Zielumgebung einrichten](./vmware-azure-set-up-target.md).
