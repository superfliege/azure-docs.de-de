---
title: Einrichten der SAP HANA-Systemreplikation auf virtuellen Azure-Computern (VMs) | Microsoft-Dokumentation
description: Richten Sie die Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs) ein.
services: virtual-machines-linux
documentationcenter: ''
author: MSSedusch
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/24/2018
ms.author: sedusch
ms.openlocfilehash: b84b523f919e6b253462139b6888e5eb16248084
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs)

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

Lokal können Sie entweder die HANA-Systemreplikation oder freigegebenen Speicher verwenden, um Hochverfügbarkeit für SAP HANA einzurichten.
Die HANA-Systemreplikation in Azure ist bisher die einzige auf virtuellen Azure-Computern unterstützte Hochverfügbarkeitsfunktion. Die SAP HANA-Replikation umfasst primären Knoten und mindestens einen sekundären Knoten. Änderungen an den Daten auf dem primären Knoten werden synchron oder asynchron an den sekundären Knoten repliziert.

Dieser Artikel beschreibt das Bereitstellen der virtuellen Computer, ihre Konfiguration, das Installieren des Clusterframeworks sowie das Installieren und Konfigurieren der SAP HANA-Systemreplikation.
In den Beispielkonfigurationen, Installationsbefehlen usw. werden Instanznummer 03 und HANA-System-ID „HN1“ verwendet.

Lesen Sie zuerst die folgenden SAP Notes und Dokumente:

* SAP-Hinweis [1928533] mit folgenden Informationen:
  * Liste der Azure-VM-Größen, die für die Bereitstellung von SAP-Software unterstützt werden
  * Wichtige Kapazitätsinformationen für Größen von Azure-VMs
  * Unterstützte SAP-Software und Kombinationen aus Betriebssystem (OS) und Datenbank
  * Erforderliche SAP-Kernelversion für Windows und Linux in Microsoft Azure
* In SAP-Hinweis [2015553] sind die Voraussetzungen für Bereitstellungen von SAP-Software in Azure aufgeführt, die von SAP unterstützt werden.
* SAP-Hinweis [2205917] enthält empfohlene Betriebssystemeinstellungen für den SUSE Linux Enterprise Server for SAP Applications.
* SAP-Hinweis [1944799] enthält SAP HANA-Richtlinien für den SUSE Linux Enterprise Server for SAP Applications.
* SAP-Hinweis [2178632] enthält ausführliche Informationen zu allen Überwachungsmetriken, die für SAP in Azure gemeldet werden.
* SAP-Hinweis [2191498] enthält die erforderliche SAP Host Agent-Version für Linux in Azure.
* SAP-Hinweis [2243692] enthält Informationen zur SAP-Lizenzierung unter Linux in Azure.
* SAP-Hinweis [1984787] enthält allgemeine Informationen zu SUSE Linux Enterprise Server 12.
* SAP-Hinweis [1999351] enthält Informationen zur Problembehandlung für die Azure-Erweiterung zur verbesserten Überwachung für SAP.
* Das [WIKI der SAP-Community](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) enthält alle erforderlichen SAP-Hinweise für Linux.
* [SAP NetWeaver auf virtuellen Azure-Computern – Planungs- und Implementierungshandbuch][planning-guide]
* [Bereitstellung von Azure Virtual Machines für SAP unter Linux (dieser Artikel)][deployment-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – DBMS-Bereitstellungshandbuch][dbms-guide]
* [SAP HANA-SR – Leistungsoptimiertes Szenario][suse-hana-ha-guide] – das Handbuch enthält alle erforderlichen Informationen zum lokalen Einrichten der SAP HANA-Replikation. Verwenden Sie dieses Handbuch als Grundlage.

## <a name="overview"></a>Übersicht

Um hohe Verfügbarkeit zu erreichen, ist SAP HANA auf zwei virtuellen Computern installiert. Die Daten werden mit HANA-Systemreplikation repliziert.

![Übersicht zur Hochverfügbarkeit von SAP HANA](./media/sap-hana-high-availability/ha-suse-hana.png)

Das SAP HANA-SR-Setup verwendet einen dedizierten virtuellen Hostnamen und virtuelle IP-Adressen. Für die Verwendung einer virtuellen IP-Adresse ist in Azure ein Lastenausgleich erforderlich. Die folgende Liste zeigt die Konfiguration des Lastenausgleichs.

* Frontendkonfiguration
  * IP-Adresse 10.0.0.13 für „hn1-db“
* Backendkonfiguration
  * Mit primären Netzwerkschnittstellen von allen virtuellen Computern verbunden, die Teil der HANA-Systemreplikation sein sollen.
* Testport
  * Port 62503
* Lastenausgleichsregeln
  * 30313 TCP
  * 30315 TCP
  * 30317 TCP

## <a name="deploying-linux"></a>Bereitstellen von Linux

Der Ressourcen-Agent für SAP HANA ist im SUSE Linux Enterprise Server für SAP-Anwendungen enthalten.
Der Azure Marketplace enthält ein Image für den SUSE Linux Enterprise Server for SAP Applications 12, das Sie zum Bereitstellen neuer virtueller Computer verwenden können.

### <a name="deploy-with-template"></a>Bereitstellen mit Vorlage
Sie können eine der Schnellstartvorlagen auf Github verwenden, um alle erforderlichen Ressourcen bereitzustellen. Die Vorlage stellt die virtuellen Computer, den Load Balancer, die Verfügbarkeitsgruppe etc. bereit. Führen Sie diese Schritte aus, um die Vorlage bereitzustellen:

1. Öffnen Sie die [Datenbankvorlage][template-multisid-db] oder die [konvergierte Vorlage][template-converged] im Azure-Portal. 
   Die Datenbankvorlage erstellt nur die Regeln des Lastenausgleichs für eine Datenbank, während die konvergierte Vorlage auch die Regeln des Lastenausgleichs für eine ASCS/SCS- und ERS-Instanz (nur Linux) erstellt. Wenn Sie ein SAP NetWeaver-basiertes System installieren und auch die ASCS/SCS-Instanz auf dem gleichen Computer installieren möchten, verwenden Sie die [konvergierte Vorlage][template-converged].
1. Legen Sie die folgenden Parameter fest:
    1. SAP-System-ID  
       Geben Sie die SAP-System-ID des SAP-Systems ein, das Sie installieren möchten. Die ID wird als Präfix für die Ressourcen verwendet, die bereitgestellt werden.
    1. Stapeltyp (gilt nur, wenn Sie die konvergierte Vorlage verwenden).   
       Wählen Sie den SAP NetWeaver-Stapeltyp aus.
    1. Betriebssystemtyp  
       Wählen Sie eine der Linux-Distributionen aus. Wählen Sie für dieses Beispiel die Option „SLES 12“.
    1. DB-Typ  
       Wählen Sie „HANA“.
    1. SAP-Systemgröße  
       Die Anzahl der vom neuen System bereitgestellten SAPS. Wenn Sie nicht sicher sind, welche SAPS-Zahl für das System benötigt wird, können Sie sich an den SAP-Technologiepartner oder -Systemintegrator wenden.
    1. Systemverfügbarkeit  
       Wählen Sie „HA“ (hohe Verfügbarkeit).
    1. Administratorbenutzername und Administratorkennwort  
       Es wird ein neuer Benutzer erstellt, der sich am Computer anmelden kann.
    1. Neues oder vorhandenes Subnetz  
       Bestimmt, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Wenn Sie bereits über ein virtuelles Netzwerk verfügen, das mit dem lokalen Netzwerk verbunden ist, wählen Sie hier „vorhanden“ aus.
    1. Subnetz-ID  
    Die ID des Subnetzes, mit dem die virtuellen Computer eine Verbindung herstellen sollen. Wählen Sie das Subnetz des virtuellen VPN- oder Express Route-Netzwerks aus, um den virtuellen Computer mit dem lokalen Netzwerk zu verbinden. Die ID sieht in der Regel wie folgt aus: /subscriptions/`<subscription ID`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`>

### <a name="manual-deployment"></a>Manuelle Bereitstellung

1. Erstellen einer Ressourcengruppe
1. Erstellen eines virtuellen Netzwerks
1. Erstellen Sie eine Verfügbarkeitsgruppe.  
   Richten Sie die maximale Updatedomäne ein.
1. Erstellen Sie einen Load Balancer (intern)  
   Wählen Sie das im zweiten Schritt erstellte VNET aus.
1. Erstellen Sie den virtuellen Computer 1.  
   Verwenden Sie mindestens SLES4SAP 12 SP1 – in diesem Beispiel verwenden wir das SLES4SAP 12 SP2-Image https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM.  
   SLES für SAP 12 SP2 (Premium)  
   Wählen Sie die Verfügbarkeitsgruppe aus, die Sie zuvor erstellt haben.  
1. Erstellen Sie den virtuellen Computer 2.  
   Verwenden Sie mindestens SLES4SAP 12 SP1 – in diesem Beispiel verwenden wir das SLES4SAP 12 SP1 BYOS-Image https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM.  
   SLES für SAP 12 SP2 (Premium)  
   Wählen Sie die Verfügbarkeitsgruppe aus, die Sie zuvor erstellt haben.  
1. Fügen Sie die Datenträger hinzu.
1. Konfigurieren Sie den Load Balancer.
    1. Erstellen eines Front-End-IP-Pools
        1. Öffnen Sie den Lastenausgleich, wählen Sie den Front-End-IP-Pool aus und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen des neuen Front-End-IP-Pools ein (z.B. „hana-frontend“).
        1. Legen Sie die Zuweisung als statisch fest, und geben Sie die IP-Adresse ein (z.B. **10.0.0.13**).
        1. OK klicken
        1. Notieren Sie nach Erstellen des neuen Front-End-IP-Pools dessen IP-Adresse.
    1. Erstellen eines Back-End-Pools
        1. Öffnen Sie den Lastenausgleich, wählen Sie Back-End-Pools und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen des neuen Back-End-Pools ein (z.B. „hana-backend“).
        1. Klicken Sie auf „Virtuellen Computer hinzufügen“.
        1. Wählen Sie die Verfügbarkeitsgruppe aus, die Sie zuvor erstellt haben.
        1. Wählen Sie die virtuellen Computer des SAP HANA-Clusters aus.
        1. OK klicken
    1. Erstellen eines Integritätstests
        1. Öffnen Sie den Lastenausgleich, wählen Sie Integritätstests aus, und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen des neuen Integritätstests ein (z.B. „hana-hp“).
        1. Wählen Sie TCP als Protokoll, Port 625**03**, halten Sie „Intervall 5“ und „Fehlerschwellenwert 2“ bei.
        1. OK klicken
    1. Erstellen von Lastenausgleichsregeln
        1. Öffnen Sie den Load Balancer, wählen Sie das Laden von Lastenausgleichsregeln, und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen der neuen Lastenausgleichsregel ein (z.B. „hana-lb-3**03**15“).
        1. Wählen Sie die Front-End-IP-Adresse, den Back-End-Pool und den Integritätstest, die Sie zuvor erstellt haben (z.B. „hana-frontend“) aus.
        1. Behalten Sie TCP als Protokoll bei, und geben Sie Port 3**03**13 ein.
        1. Erhöhen Sie die Leerlaufzeitüberschreitung auf 30 Minuten.
        1. **Achten Sie darauf, dass Sie „Floating IP“ aktivieren.**
        1. OK klicken
        1. Wiederholen Sie die oben genannten Schritte für Port 3**03**15 und 3**03**17.

## <a name="create-pacemaker-cluster"></a>Erstellen des Pacemaker-Clusters

Führen Sie die Schritte in [Einrichten von Pacemaker auf SUSE Linux Enterprise Server in Azure](high-availability-guide-suse-pacemaker.md) zum Erstellen eines grundlegenden Pacemaker-Clusters für diesen HANA-Server aus. Sie können den gleichen Pacemaker-Cluster auch für SAP HANA und SAP NetWeaver (A) SCS verwenden.

## <a name="installing-sap-hana"></a>Installieren von SAP HANA

Die folgenden Elemente sind mit einem der folgenden Präfixe versehen: **[A]** – gilt für alle Knoten, **[1]** – gilt nur für Knoten 1, oder **[2]** – gilt nur für Knoten 2 des Pacemaker-Clusters.

1. **[A]** Richten Sie das Datenträgerlayout ein.
    1. LVM  
       
       Im Allgemeinen sollten Sie LVM für Volumes verwenden, die Daten- und Protokolldateien speichern. Im folgenden Beispiel wird davon ausgegangen, dass die virtuellen Computer über vier Datenträger verfügen, die zum Erstellen von zwei Volumes verwendet werden sollten.

       Auflisten aller verfügbaren Datenträger
       
       <pre><code>
       ls /dev/disk/azure/scsi1/lun*
       </code></pre>
       
       Beispielausgabe
       
       ```
       /dev/disk/azure/scsi1/lun0  /dev/disk/azure/scsi1/lun1  /dev/disk/azure/scsi1/lun2  /dev/disk/azure/scsi1/lun3
       ```
       
       Erstellen Sie physische Volumes für alle Datenträger, die Sie verwenden möchten.    
       <pre><code>
       sudo pvcreate /dev/disk/azure/scsi1/lun0
       sudo pvcreate /dev/disk/azure/scsi1/lun1
       sudo pvcreate /dev/disk/azure/scsi1/lun2
       sudo pvcreate /dev/disk/azure/scsi1/lun3
       </code></pre>

       Erstellen Sie eine Volumegruppe für Datendateien, eine Volumegruppe für die Protokolldateien und eine für das freigegebene Verzeichnis von SAP HANA.

       <pre><code>
       sudo vgcreate vg_hana_data_<b>HN1</b> /dev/disk/azure/scsi1/lun0 /dev/disk/azure/scsi1/lun1
       sudo vgcreate vg_hana_log_<b>HN1</b> /dev/disk/azure/scsi1/lun2
       sudo vgcreate vg_hana_shared_<b>HN1</b> /dev/disk/azure/scsi1/lun3
       </code></pre>
       
       Erstellen Sie die logischen Volumes.

       <pre><code>
       sudo lvcreate -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_log vg_hana_log_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared_<b>HN1</b>
       sudo mkfs.xfs /dev/vg_hana_data_<b>HN1</b>/hana_data
       sudo mkfs.xfs /dev/vg_hana_log_<b>HN1</b>/hana_log
       sudo mkfs.xfs /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       </code></pre>
       
       Erstellen Sie die Bereitstellungsverzeichnisse, und kopieren Sie die UUID aller logischen Volumes.
       
       <pre><code>
       sudo mkdir -p /hana/data/<b>HN1</b>
       sudo mkdir -p /hana/log/<b>HN1</b>
       sudo mkdir -p /hana/shared/<b>HN1</b>
       # write down the ID of /dev/vg_hana_data_<b>HN1</b>/hana_data, /dev/vg_hana_log_<b>HN1</b>/hana_log and /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       sudo blkid
       </code></pre>
       
       Erstellen Sie fstab-Einträge für die drei logischen Volumes.
       
       <pre><code>
       sudo vi /etc/fstab
       </code></pre>
       
       Fügen Sie diese Zeile in „/etc/fstab“ ein.
       
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_data_<b>HN1</b>-hana_data&gt;</b> /hana/data/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_log_<b>HN1</b>-hana_log&gt;</b> /hana/log/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_shared_<b>HN1</b>-hana_shared&gt;</b> /hana/shared/<b>HN1</b> xfs  defaults,nofail  0  2
       </code></pre>
       
       Stellen Sie die neuen Volumes bereit.
       
       <pre><code>
       sudo mount -a
       </code></pre>
    
    1. Einfache Datenträger  
       Für Demosysteme können Sie Ihre HANA-Daten- und Protokolldateien auf einem Datenträger platzieren. Die folgenden Befehle erstellen auf „/dev/disk/azure/scsi1/lun0“ eine Partition und formatieren sie mit XFS.

       <pre><code>
       sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
       sudo mkfs.xfs /dev/disk/azure/scsi1/lun0-part1
       
       # write down the ID of /dev/disk/azure/scsi1/lun0-part1
       sudo /sbin/blkid
       sudo vi /etc/fstab
       </code></pre>

       Fügen Sie diese Zeile in „/etc/fstab“ ein.
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
       </code></pre>

       Erstellen Sie das Zielverzeichnis und stellen Sie den Datenträger bereit.

       <pre><code>
       sudo mkdir /hana
       sudo mount -a
       </code></pre>

1. **[A]** Richten Sie die Hostnamensauflösung für alle Hosts ein.  
    Sie können entweder einen DNS-Server verwenden oder „/etc/hosts“ auf allen Knoten ändern. In diesem Beispiel wird die Verwendung der /etc/hosts-Datei veranschaulicht.
   Ersetzen Sie die IP-Adresse und den Hostnamen in den folgenden Befehlen.
    ```bash
    sudo vi /etc/hosts
    ```
    Fügen Sie „/etc/hosts“ die folgenden Zeilen hinzu. Ändern Sie die IP-Adresse und den Hostnamen Ihrer Umgebung entsprechend.    
    
   <pre><code>
   <b>10.0.0.5 hn1-db-0</b>
   <b>10.0.0.6 hn1-db-1</b>
   </code></pre>

1. **[A]** Installieren Sie die HANA-HA-Pakete.  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

Installieren Sie die SAP HANA-Systemreplikation gemäß Kapitel 4 des [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] (Handbuch zum Szenario der leistungsoptimierten SAP HANA-Systemreplikation).

1. **[A]** Führen Sie „hdblcm“ über die HANA-DVD aus.
    * Wählen Sie die Installation -> 1.
    * Wählen Sie weitere Komponenten für die Installation aus -> 1.
    * Geben Sie den Installationspfad ein [/hana/shared]: -> EINGABETASTE.
    * Geben Sie den Namen des lokalen Hosts ein [..]: -> EINGABETASTE.
    * Möchten Sie dem System weitere Hosts hinzufügen? (j/n) [n]: -> EINGABETASTE.
    * Geben Sie die SAP HANA-System-ID ein: <SID of HANA e.g. HN1>.
    * Geben Sie die Instanznummer ein [00]:   
  HANA Instanznummer. Verwenden Sie 03, wenn Sie die Azure-Vorlage verwendet oder die manuelle Bereitstellung durchgeführt haben.
    * Wählen Sie den Datenbankmodus / geben Sie den Index ein [1]: -> EINGABETASTE.
    * Wählen Sie die Systemnutzung / geben Sie den Index [ein 4]:  
  Wählen Sie die Systemnutzung.
    * Geben Sie den Speicherort der Datenvolumes ein [/hana/data/HN1]: -> EINGABETASTE.
    * Geben Sie den Speicherort der Protokollvolumes ein [/hana/log/HN1]: -> EINGABETASTE.
    * Möchten Sie die maximale Speicherbelegung beschränken? [n]: -> EINGABETASTE.
    * Geben Sie den Zertifikathostnamen für Host „'...' [...]“ ein, und drücken Sie die Eingabetaste.
    * Geben Sie das Kennwort für den SAP-Host-Agent-Benutzer ein (sapadm):
    * Bestätigen Sie das Kennwort für den SAP-Host-Agent-Benutzer (sapadm):
    * Geben Sie das Kennwort für den Systemadministrator ein (hdbadm):
    * Bestätigen Sie das Kennwort für den Systemadministrator (hdbadm):
    * Geben Sie das Basisverzeichnis für den Systemadministrator ein [/usr/sap/HN1/home]: -> EINGABETASTE.
    * Geben Sie die Anmelde-Shell für den Systemadministrator ein [/ bin/sh]: -> EINGABETASTE.
    * Geben Sie die Benutzer-ID für den Systemadministrator ein [1001]: -> EINGABETASTE.
    * Geben Sie die ID der Benutzergruppe ein (sapsys) [79]: -> EINGABETASTE.
    * Geben Sie das Datenbankbenutzer-Kennwort ein (SYSTEM):
    * Bestätigen Sie das Datenbankbenutzer-Kennwort (SYSTEM):
    * Soll das System nach dem Neustart des Computers neu starten? [n]: -> EINGABETASTE.
    * Möchten Sie fortfahren? (j/n):   
  Überprüfen Sie die Zusammenfassung, und geben Sie „j“ zum Fortfahren ein.

1. **[A]** Führen Sie ein Upgrade für den SAP-Host-Agent durch.  
  Laden Sie das aktuelle SAP-Hostagentarchiv vom [SAP Softwarecenter][sap-swcenter] herunter, und führen Sie den folgenden Befehl zum Aktualisieren des Agent aus. Ersetzen Sie den Pfad zum Archiv, um auf die Datei zu verweisen, die Sie heruntergeladen haben.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

## <a name="configure-sap-hana-20-system-replication"></a>Konfigurieren der SAP HANA 2.0-Systemreplikation

Die folgenden Elemente sind mit einem der folgenden Präfixe versehen: **[A]** – gilt für alle Knoten, **[1]** – gilt nur für Knoten 1, oder **[2]** – gilt nur für Knoten 2 des Pacemaker-Clusters.

1. **[1]** Erstellen Sie eine neue Mandantendatenbank.

   Wenn Sie SAP HANA 2.0 oder MDC verwenden, erstellen Sie eine Mandantendatenbank für Ihr SAP NetWeaver-System. Ersetzen Sie NW1 durch die SID des SAP-Systems.

   Melden Sie sich als „`<hanasid`>adm“ bei Azure an, und führen Sie den folgenden Befehl aus.

   <pre><code>
   hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]**  Konfigurieren Sie die Systemreplikation auf dem ersten Knoten.
   
   Melden Sie sich als „`<hanasid`>adm“ an, und sichern Sie die Datenbanken.

   <pre><code>
   hdbsql -d SYSTEMDB -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupSYS</b>')"
   hdbsql -d <b>HN1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupHN1</b>')"
   hdbsql -d <b>NW1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupNW1</b>')"
   </code></pre>

   Kopieren Sie die PKI-Systemdateien auf den sekundären Knoten.

   <pre><code>
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/SSFS_<b>HN1</b>.DAT <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/SSFS_<b>HN1</b>.KEY <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/
   </code></pre>

   Erstellen Sie den primären Standort.

   <pre><code>
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  Konfigurieren Sie die Systemreplikation auf dem zweiten Knoten.
    
    Registrieren Sie den zweiten Knoten zum Starten der Replikation. Melden Sie sich als „`<hanasid`>adm“ bei Azure an, und führen Sie den folgenden Befehl aus.

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>Konfigurieren der SAP HANA 1.0-Systemreplikation

1. **[1]** Erstellen Sie die erforderlichen Benutzer.

    Melden Sie sich als Stammbenutzer an, und führen Sie den folgenden Befehl aus. Stellen Sie sicher, dass Sie die fett formatierten Zeichenfolgen (HANA-System-ID „HN1“ und Instanzenanzahl „03“) durch die Werte Ihrer SAP HANA-Installation ersetzen.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. **[A]** Erstellen Sie einen Keystoreeintrag.
   
    Melden Sie sich als Stammbenutzer an, und führen Sie den folgenden Befehl aus, um einen neuen Eintrag für den Schlüsselspeicher zu erstellen.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>

1. **[1]** Sichern Sie die Datenbank.

   Melden Sie sich als Stammbenutzer an, und sichern Sie die Datenbanken.

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -d SYSTEMDB -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

   Wenn Sie eine Installation mit mehreren Mandanten verwenden, sichern Sie auch die Mandantendatenbank.

   <pre><code>   
   hdbsql -d <b>HN1</b> -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

1. **[1]**  Konfigurieren Sie die Systemreplikation auf dem ersten Knoten.
    
    Melden Sie sich als `<hanasid`>adm an, und erstellen Sie den primären Standort.

    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>

1. **[2]**  Konfigurieren Sie die Systemreplikation auf dem zweiten Knoten.

    Melden Sie sich als `<hanasid`>adm an, und erstellen Sie den sekundären Standort.

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="create-sap-hana-cluster-resources"></a>Erstellen von SAP HANA-Clusterressourcen

   Erstellen Sie zuerst die HANA-Topologie. Führen Sie die folgenden Befehle auf einem der Pacemaker-Clusterknoten aus.
   
   <pre><code>
   sudo crm configure property maintenance-mode=true

   # replace the bold string with your instance number and HANA system ID
   
   sudo crm configure primitive rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
     operations \$id="rsc_sap2_<b>HN1</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HN1</b>" InstanceNumber="<b>03</b>"
   
   sudo crm configure clone cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
   </code></pre>
   
   Erstellen Sie als Nächstes die HANA-Ressourcen.
   
   <pre><code>
   # replace the bold string with your instance number, HANA system ID and the frontend IP address of the Azure load balancer. 
      
   sudo crm configure primitive rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations \$id="rsc_sap_<b>HN1</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HN1</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
   
   sudo crm configure ms msl_SAPHana_<b>HN1</b>_HDB<b>03</b> rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
   
   sudo crm configure primitive rsc_ip_<b>HN1</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \
     meta target-role="Started" is-managed="true" \
     operations \$id="rsc_ip_<b>HN1</b>_HDB<b>03</b>-operations" \
     op monitor interval="10s" timeout="20s" \
     params ip="<b>10.0.0.13</b>"
   
   sudo crm configure primitive rsc_nc_<b>HN1</b>_HDB<b>03</b> anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   sudo crm configure group g_ip_<b>HN1</b>_HDB<b>03</b> rsc_ip_<b>HN1</b>_HDB<b>03</b> rsc_nc_<b>HN1</b>_HDB<b>03</b>
   
   sudo crm configure colocation col_saphana_ip_<b>HN1</b>_HDB<b>03</b> 2000: g_ip_<b>HN1</b>_HDB<b>03</b>:Started \
     msl_SAPHana_<b>HN1</b>_HDB<b>03</b>:Master  
   
   sudo crm configure order ord_SAPHana_<b>HN1</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     msl_SAPHana_<b>HN1</b>_HDB<b>03</b>
   
   # Cleanup the HANA resources. The HANA resources might have failed because of a known issue.
   sudo crm resource cleanup rsc_SAPHana_<b>HN1</b>_HDB<b>03</b>

   sudo crm configure property maintenance-mode=false
   </code></pre>

   Stellen Sie sicher, dass der Clusterstatus gültig ist und alle Ressourcen gestartet sind. Es ist nicht wichtig, auf welchem Knoten die Ressourcen ausgeführt werden.

   <pre><code>
   sudo crm_mon -r
   
   # Online: [ hn1-db-0 hn1-db-1 ]
   #
   # Full list of resources:
   #
   # stonith-sbd     (stonith:external/sbd): Started hn1-db-0
   # rsc_st_azure    (stonith:fence_azure_arm):      Started hn1-db-1
   # Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
   #     Started: [ hn1-db-0 hn1-db-1 ]
   # Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
   #     Masters: [ hn1-db-0 ]
   #     Slaves: [ hn1-db-1 ]
   # Resource Group: g_ip_HN1_HDB03
   #     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
   #     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

### <a name="test-cluster-setup"></a>Testen der Clustereinrichtung
In diesem Kapitel wird beschrieben, wie Sie Ihre Einrichtung testen können. Jeder Test setzt voraus, dass Sie der Stammbenutzer sind und der SAP HANA-Master auf dem virtuellen Computer „hn1-db-0“ ausgeführt wird.

#### <a name="fencing-test"></a>Umgrenzungstest

Sie können die Einrichtung des Umgrenzungs-Agent testen, indem Sie die Netzwerkschnittstelle auf Knoten „hn1-db-0“ deaktivieren.

<pre><code>
sudo ifdown eth0
</code></pre>

Der virtuelle Computer sollte jetzt abhängig von Ihrer Clusterkonfiguration neu gestartet oder beendet werden.
Wenn Sie die Stonith-Aktion auf „Aus“ festlegen, wird der virtuelle Computer beendet, und die Ressourcen werden zum ausgeführten virtuellen Computer migriert.

Sobald Sie den virtuellen Computer erneut starten, wird die SAP HANA-Ressource nicht als sekundär gestartet, wenn Sie AUTOMATED_REGISTER=„false“ festlegen. In diesem Fall konfigurieren Sie die HANA-Instanz als sekundär, indem Sie diesen Befehl ausführen:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Testen eines manuellen Failovers

Sie können ein manuelles Failover durch Beenden des Pacemaker-Diensts auf Knoten „hn1-db-0“ testen.
<pre><code>
service pacemaker stop
</code></pre>

Nach dem Failover können Sie den Dienst erneut starten. Wenn Sie AUTOMATED_REGISTER=„false“ festlegen, wird die SAP HANA-Ressource auf „hn1-db-0“ nicht als sekundär gestartet. In diesem Fall konfigurieren Sie die HANA-Instanz als sekundär, indem Sie diesen Befehl ausführen:

<pre><code>
service pacemaker start
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-migration"></a>Testen einer Migration

Sie können den SAP HANA-Masterknoten migrieren, indem Sie den folgenden Befehl ausführen.
<pre><code>
crm resource migrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
crm resource migrate g_ip_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
</code></pre>

Wenn Sie AUTOMATED_REGISTER=„false“ festlegen, sollte diese Befehlsfolge den SAP HANA-Masterknoten und die Gruppe, die die virtuelle IP-Adresse „hn1-db-1“ enthält, migrieren.
Die SAP HANA-Ressource auf „hn1-db-0“ wird nicht als sekundär gestartet. In diesem Fall konfigurieren Sie die HANA-Instanz als sekundär, indem Sie diesen Befehl ausführen:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

Die Migration erstellt Speicherorteinschränkungen, die erneut gelöscht werden müssen.

<pre><code>
crm configure edited

# Delete location constraints that are named like the following contraint. You should have two constraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HN1</b>_HDB<b>03</b> g_ip_<b>HN1</b>_HDB<b>03</b> role=Started inf: <b>hn1-db-1</b>
</code></pre>

Darüber hinaus müssen Sie auch den Status der sekundären Knotenressource bereinigen.

<pre><code>
# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

## <a name="next-steps"></a>Nächste Schritte
* [SAP NetWeaver auf virtuellen Azure-Computern – Planungs- und Implementierungshandbuch][planning-guide]
* [Bereitstellung von Azure Virtual Machines für SAP][deployment-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – DBMS-Bereitstellungshandbuch][dbms-guide]
* Informationen zur Erzielung von Hochverfügbarkeit und zur Planung der Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md). 
