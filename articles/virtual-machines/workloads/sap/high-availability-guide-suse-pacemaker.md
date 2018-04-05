---
title: Einrichten von Pacemaker unter SUSE Linux Enterprise Server in Azure | Microsoft-Dokumentation
description: Einrichten von Pacemaker unter SUSE Linux Enterprise Server in Azure
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/20/2018
ms.author: sedusch
ms.openlocfilehash: 75615de523f1fba808f44fb1a1015138fb190edc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="setting-up-pacemaker-on-suse-linux-enterprise-server-in-azure"></a>Einrichten von Pacemaker unter SUSE Linux Enterprise Server in Azure

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md

Es gibt zwei Möglichkeiten zum Einrichten eines Pacemaker-Clusters in Azure. Sie können entweder einen Umgrenzungs-Agent verwenden, über den der Neustart eines fehlerhaften Knotens über die Azure-APIs erfolgt, oder Sie können ein SBD-Gerät verwenden.

Für das SBD-Gerät ist ein zusätzlicher virtueller Computer erforderlich, der als iSCSI-Zielserver dient und ein SBD-Gerät bereitstellt. Dieser iSCSI-Zielserver kann jedoch für andere Pacemaker-Cluster freigegeben werden. Die Verwendung eines SBD-Geräts bietet den Vorteil einer kürzeren Failoverzeit. Bei lokaler Verwendung von SBD-Geräten sind zudem keine Änderungen am Betrieb des Pacemaker-Clusters erforderlich. Bei der SBD-Umgrenzung kann der Azure Fence Agent dennoch als Sicherungsumgrenzungsmechanismus für den Fall verwendet werden, dass der iSCSI-Zielserver nicht verfügbar ist.

Wenn Sie in keinen zusätzlichen virtuellen Computer investieren möchten, können Sie auch den Azure Fence Agent verwenden. Der Nachteil dabei ist, dass ein Failover zwischen 10 und 15 Minuten dauern kann, wenn beim Beenden einer Ressource Fehler auftreten oder keine Kommunikation mehr zwischen den Clusterknoten möglich ist.

![Übersicht über Pacemaker unter SLES](./media/high-availability-guide-suse-pacemaker/pacemaker.png)

## <a name="sbd-fencing"></a>SBD-Umgrenzung

Gehen Sie folgendermaßen vor, wenn Sie ein SBD-Gerät zur Umgrenzung verwenden möchten.

### <a name="set-up-an-iscsi-target-server"></a>Einrichten eines iSCSI-Zielservers

Sie müssen zunächst einen virtuellen iSCSI-Zielcomputer erstellen, wenn noch keiner vorhanden ist. iSCSI-Zielserver können für mehrere Pacemaker-Cluster freigegeben werden.

1. Stellen Sie einen neuen virtuellen Computer unter SLES 12 SP1 oder höher bereit, und stellen Sie über SSH eine Verbindung mit dem Computer her. Der Computer muss nicht groß sein. Eine VM-Größe wie Standard_E2s_v3 oder Standard_D2s_v3 ist ausreichend.

1. Aktualisieren Sie SLES.

   <pre><code>
   sudo zypper update
   </code></pre>

1. Installieren Sie die iSCSI-Zielpakete.

   <pre><code>
   sudo zypper install targetcli-fb dbus-1-python
   </code></pre>

1. Aktivieren Sie den iSCSI-Zieldienst.

   <pre><code>   
   sudo systemctl enable target
   sudo systemctl enable targetcli
   sudo systemctl start target
   sudo systemctl start targetcli
   </code></pre>

### <a name="create-iscsi-device-on-iscsi-target-server"></a>Erstellen des iSCSI-Geräts auf dem iSCSI-Zielserver

Fügen Sie einen neuen Datenträger an den virtuellen iSCSI-Zielcomputer an, der für den Cluster verwendet werden kann. Der Datenträger muss eine Größe von mindestens 1 GB aufweisen und in einem Storage Premium-Konto oder auf einem verwalteten Premium-Datenträger platziert werden, damit die [SLA für virtuelle Computer](https://azure.microsoft.com/support/legal/sla/virtual-machines) genutzt werden kann.

Führen Sie den folgenden Befehl auf dem **virtuellen iSCSI-Zielcomputer** aus, um einen iSCSI-Datenträger für den neuen Cluster zu erstellen. Im folgenden Beispiel wird **cl1** zum Identifizieren des neuen Clusters verwendet, und **prod-cl1-0** und **prod-cl1-1** sind die Hostnamen der Clusterknoten. Ersetzen Sie sie durch die Hostnamen Ihrer Clusterknoten.

<pre><code>
# List all data disks with the following command
sudo ls -al /dev/disk/azure/scsi1/

# total 0
# drwxr-xr-x 2 root root  80 Mar 26 14:42 .
# drwxr-xr-x 3 root root 160 Mar 26 14:42 ..
# lrwxrwxrwx 1 root root  12 Mar 26 14:42 lun0 -> ../../../<b>sdc</b>
# lrwxrwxrwx 1 root root  12 Mar 26 14:42 lun1 -> ../../../sdd

# Then use the disk name to list the disk id
sudo ls -l /dev/disk/by-id/scsi-* | grep sdc

# lrwxrwxrwx 1 root root  9 Mar 26 14:42 /dev/disk/by-id/scsi-14d53465420202020a50923c92babda40974bef49ae8828f0 -> ../../sdc
# lrwxrwxrwx 1 root root  9 Mar 26 14:42 <b>/dev/disk/by-id/scsi-360022480a50923c92babef49ae8828f0 -> ../../sdc</b>

# Use the data disk that you attached for this cluster to create a new backstore
sudo targetcli backstores/block create <b>cl1</b> <b>/dev/disk/by-id/scsi-360022480a50923c92babef49ae8828f0</b>

sudo targetcli iscsi/ create iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/luns/ create /backstores/block/<b>cl1</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/acls/ create iqn.2006-04.<b>prod-cl1-0.local:prod-cl1-0</b>
sudo targetcli iscsi/iqn.2006-04.<b>cl1</b>.local:<b>cl1</b>/tpg1/acls/ create iqn.2006-04.<b>prod-cl1-1.local:prod-cl1-1</b>

# save the targetcli changes
sudo targetcli saveconfig
sudo systemctl restart target
</code></pre>

### <a name="set-up-sbd-device"></a>Einrichten des SBD-Geräts

Stellen Sie im Cluster eine Verbindung mit dem iSCSI-Gerät her, das im letzten Schritt erstellt wurde.
Führen Sie die folgenden Befehle für die zu erstellenden Knoten des neuen Clusters aus.
Die folgenden Elemente sind mit einem der folgenden Präfixe versehen: **[A]** – gilt für alle Knoten, **[1]** – gilt nur für Knoten 1, oder **[2]** – gilt nur für Knoten 2.

1. **[A]** Stellen Sie eine Verbindung mit den iSCSI-Geräten her.

   Aktivieren Sie zunächst die iSCSI- und SBD-Dienste.

   <pre><code>
   sudo systemctl enable iscsid
   sudo systemctl enable iscsi
   sudo systemctl enable sbd
   </code></pre>

1. **[1]** Ändern Sie den Initiatornamen auf dem ersten Knoten.

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Ändern Sie den Inhalt der Datei so, dass er den ACLs entspricht, die Sie beim Erstellen des iSCSI-Geräts auf dem iSCSI-Zielserver verwendet haben.

   <pre><code>   
   InitiatorName=<b>iqn.2006-04.prod-cl1-0.local:prod-cl1-0</b>
   </code></pre>

1. **[2]** Ändern Sie den Initiatornamen auf dem zweiten Knoten.

   <pre><code>
   sudo vi /etc/iscsi/initiatorname.iscsi
   </code></pre>

   Ändern Sie den Inhalt der Datei so, dass er den ACLs entspricht, die Sie beim Erstellen des iSCSI-Geräts auf dem iSCSI-Zielserver verwendet haben.

   <pre><code>
   InitiatorName=<b>iqn.2006-04.prod-cl1-1.local:prod-cl1-1</b>
   </code></pre>

1. **[A]** Starten Sie den iSCSI-Dienst neu.

   Starten Sie den iSCSI-Dienst neu, um die Änderung zu übernehmen.
   
   <pre><code>
   sudo systemctl restart iscsid
   sudo systemctl restart iscsi
   </code></pre>

   Stellen Sie eine Verbindung mit den iSCSI-Geräten her. Im Beispiel unten ist 10.0.0.17 die IP-Adresse des iSCSI-Zielservers und 3260 der Standardport. <b>iqn.2006-04.cl1.local:cl1</b> ist der Zielname, der beim Ausführen des ersten Befehls aufgeführt wird.

   <pre><code>
   sudo iscsiadm -m discovery --type=st --portal=<b>10.0.0.17:3260</b>
   
   sudo iscsiadm -m node -T <b>iqn.2006-04.cl1.local:cl1</b> --login --portal=<b>10.0.0.17:3260</b>
   sudo iscsiadm -m node -p <b>10.0.0.17:3260</b> --op=update --name=node.startup --value=automatic
   </code></pre>

   Vergewissern Sie sich, dass das iSCSI-Gerät verfügbar ist, und notieren Sie den Gerätenamen („/dev/sde“ im folgenden Beispiel).

   <pre><code>
   lsscsi
   
   # [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
   # [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
   # [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
   # [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
   # <b>[6:0:0:0]    disk    LIO-ORG  cl1              4.0   /dev/sde</b>
   </code></pre>

   Rufen Sie die ID des iSCSI-Geräts ab.

   <pre><code>
   ls -l /dev/disk/by-id/scsi-* | grep <b>sde</b>
   
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-1LIO-ORG_cl1:3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   # <b>lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df -> ../../sde</b>
   # lrwxrwxrwx 1 root root  9 Feb  7 12:39 /dev/disk/by-id/scsi-SLIO-ORG_cl1_3fe4da37-1a5a-4bb6-9a41-9a4df57770e4 -> ../../sde
   </code></pre>

   Mit dem Befehl werden drei Geräte-IDs aufgeführt. Es wird empfohlen, die ID zu verwenden, die mit „scsi-3“ beginnt, im Beispiel oben ist dies folgende ID:
   
   **/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df**

1. **[1]** Erstellen Sie das SBD-Gerät.

   Verwenden Sie die Geräte-ID des iSCSI-Geräts, um ein neues SBD-Gerät auf dem ersten Clusterknoten zu erstellen.

   <pre><code>
   sudo sbd -d <b>/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df</b> -1 10 -4 20 create
   </code></pre>

1. **[A]** Passen Sie die SDB-Konfiguration an.

   Öffnen Sie die SBD-Konfigurationsdatei.

   <pre><code>
   sudo vi /etc/sysconfig/sbd
   </code></pre>

   Ändern Sie die Eigenschaft des SBD-Geräts, aktivieren Sie die Pacemaker-Integration, und ändern Sie den Startmodus von SBD.

   <pre><code>
   [...]
   <b>SBD_DEVICE="/dev/disk/by-id/scsi-360014053fe4da371a5a4bb69a419a4df"</b>
   [...]
   <b>SBD_PACEMAKER="yes"</b>
   [...]
   <b>SBD_STARTMODE="always"</b>
   </code></pre>

   Erstellen Sie die Softdog-Konfigurationsdatei.

   <pre><code>
   echo softdog | sudo tee /etc/modules-load.d/softdog.conf
   </code></pre>

   Laden Sie nun das Modul.

   <pre><code>
   sudo modprobe -v softdog
   </code></pre>

## <a name="cluster-installation"></a>Clusterinstallation

Die folgenden Elemente sind mit einem der folgenden Präfixe versehen: **[A]** – gilt für alle Knoten, **[1]** – gilt nur für Knoten 1, oder **[2]** – gilt nur für Knoten 2.

1. **[A]** Aktualisieren Sie den SLES.

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]** Aktivieren Sie den SSH-Zugriff.

   <pre><code>
   sudo ssh-keygen
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

2. **[2]** Aktivieren Sie den SSH-Zugriff.

   <pre><code>
   sudo ssh-keygen
   
   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_rsa): -> Press ENTER
   # Enter passphrase (empty for no passphrase): -> Press ENTER
   # Enter same passphrase again: -> Press ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_rsa.pub
   </code></pre>

1. **[1]** Aktivieren Sie den SSH-Zugriff.

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]** Installieren Sie die HA-Erweiterung.
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]** Richten Sie die Hostnamensauflösung ein.   

   Sie können entweder einen DNS-Server verwenden oder „/etc/hosts“ auf allen Knoten ändern. In diesem Beispiel wird die Verwendung der /etc/hosts-Datei veranschaulicht.
   Ersetzen Sie die IP-Adresse und den Hostnamen in den folgenden Befehlen.

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Fügen Sie „/etc/hosts“ die folgenden Zeilen hinzu. Ändern Sie die IP-Adresse und den Hostnamen Ihrer Umgebung entsprechend.   
   
   <pre><code>
   # IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[1]** Installieren Sie den Cluster.
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> Press ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> Press ENTER
   # Multicast port [5405] -> Press ENTER
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]** Fügen Sie dem Cluster Knoten hinzu.
   
   <pre><code> 
   sudo ha-cluster-join
   
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.14
   # /root/.ssh/id_rsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]** Ändern Sie das Kennwort „hacluster“ in das gleiche Kennwort.

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]** Konfigurieren Sie „corosync“ zur Verwendung einer anderen Transportart, und fügen Sie „nodelist“ hinzu. Andernfalls funktioniert der Cluster nicht.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   Fügen Sie der Datei den im Folgenden fett formatierten Inhalt hinzu.
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-cl1-0</b>
      ring0_addr:10.0.0.6
     }
     node {
      # IP address of <b>prod-cl1-1</b>
      ring0_addr:10.0.0.7
     } 
   }</b>
   logging {
     [...]
   }
   quorum {
        # Enable and configure quorum subsystem (default: off)
        # see also corosync.conf.5 and votequorum.5
        provider: corosync_votequorum
        <b>expected_votes: 2</b>
        <b>two_node: 1</b>
   }
   </code></pre>

   Starten Sie den corosync-Dienst dann neu.

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[1]** Ändern Sie die Pacemaker-Standardeinstellungen.

   <pre><code>
   sudo crm configure rsc_defaults resource-stickiness="1"   
   </code></pre>

## <a name="create-stonith-device"></a>Erstellen des STONITH-Geräts

Das STONITH-Gerät verwendet einen Dienstprinzipal zur Autorisierung bei Microsoft Azure. Führen Sie die folgenden Schritte aus, um einen Dienstprinzipal zu erstellen.

1. Besuchen Sie <https://portal.azure.com>.
1. Öffnen Sie das Blatt „Azure Active Directory“.  
   Wechseln Sie zu „Eigenschaften“, und notieren Sie sich die Verzeichnis-ID. Dies ist die **Mandanten-ID**.
1. Klicken Sie auf „App-Registrierungen“.
1. Klicken Sie auf "Hinzufügen".
1. Geben Sie einen Namen ein, wählen Sie den Anwendungstyp „Web-App/API“ aus, geben Sie eine Anmelde-URL ein (z.B. http://localhost)), und klicken Sie auf „Erstellen“.
1. Die Anmelde-URL wird nicht verwendet und kann eine beliebige gültige URL sein.
1. Wählen Sie die neue App aus, und klicken Sie auf der Registerkarte „Einstellungen“ auf „Schlüssel“.
1. Geben Sie eine Beschreibung für einen neuen Schlüssel ein, wählen Sie „Läuft nie ab“, und klicken Sie auf „Speichern“.
1. Notieren Sie sich den Wert. Er dient als **Kennwort** für den Dienstprinzipal.
1. Notieren Sie sich die Anwendungs-ID. Sie wird als Benutzername (**Anmelde-ID** in den folgenden Schritten) des Dienstprinzipals verwendet.

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]** Erstellen einer benutzerdefinierten Rolle für den Fence Agent.

Der Dienstprinzipal hat standardmäßig keine Zugriffsberechtigungen für Ihre Azure-Ressourcen. Sie müssen dem Dienstprinzipal Berechtigungen zum Starten und Beenden (Freigeben) aller virtuellen Computer des Clusters gewähren. Wenn Sie noch keine benutzerdefinierte Rolle erstellt haben, können Sie sie mit [PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell#create-a-custom-role) oder der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli#create-a-custom-role) erstellen.

Verwenden Sie folgenden Inhalt für die Eingabedatei. Sie müssen den Inhalt an Ihre Abonnements anpassen, d.h., Sie müssen „c276fc76-9cd4-44c9-99a7-4fd71546436e“ und „e91d47c4-76f3-4271-a796-21b4ecfe3624“ durch die IDs Ihres Abonnements ersetzen. Wenn Sie nur über ein Abonnement verfügen, entfernen Sie den zweiten Eintrag in AssignableScopes.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to deallocate and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="1-assign-the-custom-role-to-the-service-principal"></a>**[1]** Weisen Sie dem Dienstprinzipal die benutzerdefinierte Rolle zu.

Weisen Sie dem Dienstprinzipal die benutzerdefinierte Rolle „Linux Fence Agent Role“ zu, die im letzten Abschnitt erstellt wurde. Verwenden Sie die Rolle „Owner“ nicht mehr.

1. Besuchen Sie https://portal.azure.com.
1. Öffnen Sie das Blatt „Alle Ressourcen“.
1. Wählen Sie den virtuellen Computer des ersten Clusterknotens aus.
1. Klicken Sie auf „Zugriffssteuerung (IAM)“.
1. Klicken Sie auf "Hinzufügen".
1. Wählen Sie die Rolle „Linux Fence Agent Role“ aus.
1. Geben Sie den Namen der Anwendung ein, die Sie zuvor erstellt haben.
1. OK klicken

Wiederholen Sie die oben genannten Schritte für den zweiten Clusterknoten.

### <a name="1-create-the-stonith-devices"></a>**[1]** Erstellen Sie die STONITH-Geräte.

Nachdem Sie die Berechtigungen für die virtuellen Computer bearbeitet haben, können Sie die STONITH-Geräte im Cluster konfigurieren.

<pre><code>
# replace the bold string with your subscription ID, resource group, tenant ID, service principal ID and password
sudo crm configure property stonith-timeout=900

sudo crm configure primitive rsc_st_azure stonith:fence_azure_arm \
   params subscriptionId="<b>subscription ID</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" login="<b>login ID</b>" passwd="<b>password</b>"

</code></pre>

### <a name="1-create-fence-topology-for-sbd-fencing"></a>**[1]** Erstellen Sie die Umgrenzungstopologie für die SBD-Umgrenzung.

Wenn Sie ein SBD-Gerät verwenden möchten, wird dennoch empfohlen, einen Azure Fence Agent als Sicherung für den Fall zu verwenden, dass der iSCSI-Zielserver nicht verfügbar ist.

<pre><code>
sudo crm configure fencing_topology \
  stonith-sbd rsc_st_azure

</code></pre>
### **[1]** Aktivieren Sie die Verwendung eines STONITH-Geräts.

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>


## <a name="next-steps"></a>Nächste Schritte
* [SAP NetWeaver auf virtuellen Azure-Computern – Planungs- und Implementierungshandbuch][planning-guide]
* [Bereitstellung von Azure Virtual Machines für SAP][deployment-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – DBMS-Bereitstellungshandbuch][dbms-guide]
* Informationen zur Erzielung von Hochverfügbarkeit und zur Planung der Notfallwiederherstellung für SAP HANA auf Azure-VMs finden Sie unter [Hochverfügbarkeit für SAP HANA auf Azure Virtual Machines (VMs)][sap-hana-ha].