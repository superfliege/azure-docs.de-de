---
title: Testen von SAP NetWeaver auf Microsoft Azure SUSE-Linux-VMs | Microsoft Docs
description: Testen von SAP NetWeaver in Microsoft Azure SUSE-Linux-VMs
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/14/2017
ms.author: hermannd
ms.openlocfilehash: f7dd532e96540fa297cac8fa3736f9f4a6ccd82f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Ausführen von SAP NetWeaver auf Microsoft Azure SUSE-Linux-VMs
Dieser Artikel beschreibt verschiedene Aspekte, die Sie berücksichtigen müssen, wenn Sie SAP NetWeaver auf virtuellen Microsoft Azure SUSE-Linux-Computern (VMs) ausführen. Ab 19. Mai 2016 wird SAP NetWeaver offiziell auf virtuellen SUSE-Linux-Computern in Azure unterstützt. Alle Informationen zu Linux-Versionen, SAP-Kernelversionen und sonstigen Voraussetzungen finden Sie im SAP-Hinweis 1928533 „SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen“.
Eine ausführlichere Dokumentation zu SAP auf virtuellen Linux-Computern finden Sie hier: [Verwenden von SAP auf virtuellen Linux-Computern](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Die folgenden Informationen sollten Ihnen helfen, einige potenzielle Probleme zu vermeiden.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE-Images auf Azure zum Ausführen von SAP
Verwenden Sie SUSE Linux Enterprise Server SLES 12 (SPx) oder SLES for SAP, um SAP NetWeaver in Azure auszuführen (siehe auch SAP-Hinweis 1928533). Ein spezielles SUSE-Image befindet sich im Azure Marketplace („SLES 11 SP3 for SAP CAL“). Dieses Image ist jedoch nicht für die allgemeine Nutzung vorgesehen. Verwenden Sie dieses Image nicht, da es für die [SAP Cloud Appliance Library](https://cal.sap.com/) vorgesehen ist.  

Für alle Installationen in Azure muss das Azure Resource Manager-Bereitstellungsframework verwendet werden. Verwenden Sie die weiter unten angegebenen Befehle, um mit Azure PowerShell oder der Azure Befehlszeilenschnittstelle (CLI) nach SUSE SLES-Images und -Versionen zu suchen. Die Ausgabe kann dann z. B. verwendet werden, um das OS-Image in einer JSON-Vorlage für die Bereitstellung einer neuen SUSE Linux-VM zu definieren.
Die folgenden PowerShell-Befehle gelten für Azure PowerShell Version 1.0.1 und höher.

Sie können für SAP-Installationen zwar weiterhin die SLES-Standardimages verwenden, es wird jedoch empfohlen, die neuen SLES for SAP-Images zu verwenden. Diese Images stehen jetzt im Azure-Imagekatalog zur Verfügung. Weitere Informationen zu diesen Images erhalten Sie auf der entsprechenden [Azure Marketplace-Seite]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) oder auf der Seite [Häufig gestellte Fragen: SUSE Linux Enterprise Server für SAP-Anwendungen]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* Suchen nach vorhandenen Herausgebern einschließlich SUSE:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* Suchen nach vorhandenen Angeboten von SUSE:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* Suchen nach SUSE SLES-Angeboten:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* Suchen nach einer bestimmten Version einer SLES-SKU:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Installieren von WALinuxAgent in einer SUSE-VM
Der Agent namens „WALinuxAgent“ ist Teil der SLES-Images im Azure Marketplace. Informationen zur manuellen Installation (z. B. beim Hochladen einer SLES OS-VHD [Virtual Hard Disk, virtuelle Festplatte] vom lokalen Standort) finden Sie unter:

* [OpenSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>„Erweiterte Überwachung“ von SAP
Die „erweiterte Überwachung“ von SAP ist eine obligatorische Voraussetzung für die Ausführung von SAP in Azure. Einzelheiten dazu finden Sie im SAP-Hinweis 2191498 „SAP on Linux with Azure: Enhanced Monitoring“ (SAP unter Linux mit Azure: erweiterte Überwachung).

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Anfügen von Azure-Datenträgern an eine Azure Linux-VM
Binden Sie Azure-Datenträger niemals mithilfe der Geräte-ID auf einem virtuellen Azure-Linux-Computer ein. Verwenden Sie stattdessen den Universally Unique Identifier (UUID). Seien Sie vorsichtig, wenn Sie grafische Tools z. B. für das Einbinden von Azure-Datenträgern verwenden. Überprüfen Sie die Einträge in "/etc/fstab".

Das Problem mit der Geräte-ID besteht darin, dass diese sich ändern kann und die Azure-VM sich dann möglicherweise beim Startvorgang aufhängt. Sie könnten das Problem dann durch Hinzufügen des Parameters „nofail“ in „/etc/fstab“ beheben. Beachten Sie jedoch, dass Anwendungen bei Verwendung von „nofail“ den Bereitstellungspunkt wie zuvor verwenden könnten, und für den Fall, dass ein externer Azure-Datenträger während des Startvorgangs nicht eingebunden wurde, in das Stammdateisystem schreiben könnten.

Die einzige Ausnahme für die Einbindung per UUID ist das Anfügen eines Betriebssystemdatenträgers zu Problembehandlungszwecken, wie im folgenden Abschnitt beschrieben.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Problembehandlung bei SUSE-VM, auf die kein Zugriff mehr möglich ist
Es kann zu Situationen kommen, in denen eine SUSE-VM unter Azure beim Startvorgang hängen bleibt (z. B. aufgrund eines Fehlers bei der Einbindung von Datenträgern). Sie können dieses Problem mithilfe der Startdiagnosefunktion für Azure Virtual Machines v2 im Azure-Portal überprüfen. Weitere Informationen finden Sie unter [Boot Diagnostics](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) (Startdiagnose).

Eine Möglichkeit zur Lösung des Problems ist, den Betriebssystem-Datenträger aus der beschädigten VM einer anderen SUSE-VM in Azure anzufügen. Nehmen Sie dann entsprechende Änderungen wie die Bearbeitung von „/etc/fstab“ oder das Entfernen von Netzwerk-Udev-Regeln vor, wie im nächsten Abschnitt beschrieben.

Es ist aber wichtig, einen Aspekt zu berücksichtigen. Beim Bereitstellen mehrerer SUSE-VMs über das gleiche Azure Marketplace-Image (z. B. SLES 11 SP4) wird der Betriebssystem-Datenträger immer mit der gleichen UUID eingebunden. Somit entstehen zwei identische UUIDs, wenn die UUID zum Anfügen eines Betriebssystemdatenträgers eines anderen virtuellen Computers verwendet wird, der mit dem gleichen Azure Marketplace-Image bereitgestellt wurde. Zwei identische UUIDs führen dazu, dass der für die Problembehandlung verwendete virtuelle Computer vom angefügten und beschädigten Betriebssystemdatenträger und nicht vom ursprünglichen Betriebssystemdatenträger startet.

Probleme können auf zwei Arten vermieden werden:

* Verwenden Sie für den virtuellen Problembehandlungscomputer ein anderes Azure Marketplace-Image (beispielsweise SLES 11 SPx anstelle von SLES 12).
* Vermeiden Sie das Anfügen des fehlerhaften Betriebssystem-Datenträgers von einer anderen VM per UUID, und verwenden Sie etwas anderes.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Hochladen einer SUSE-VM vom lokalen Standort nach Azure
Eine Beschreibung der Schritte zum Hochladen einer SUSE-VM aus einem lokalen System nach Azure finden Sie unter [Vorbereiten eines virtuellen SLES- oder openSUSE-Computers für Azure](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Wenn Sie eine VM hochladen möchten, ohne die Bereitstellung zum Schluss aufzuheben (z. B., um eine vorhandene SAP-Installation sowie den Hostnamen zu behalten), müssen Sie Folgendes beachten:

* Vergewissern Sie sich, dass der Betriebssystem-Datenträger über die UUID und nicht über die Geräte-ID eingebunden wurde. Das alleinige Ändern der UUID in „etc/fstab“ ist für die Betriebssystemfestplatte nicht ausreichend. Zusätzlich dürfen Sie nicht vergessen, den Bootloader über „YaST“ oder durch Bearbeiten von „/boot/grub/menu.lst“ anzupassen.
* Falls Sie für den SUSE-Betriebssystemdatenträger das VHDX-Format verwendet und es für den Upload an Azure in das VHD-Format konvertiert haben, ändert sich das Netzwerkgerät wahrscheinlich von „eth0“ in „eth1“. Zur Vermeidung von Problemen beim späteren Starten in Azure sollte das Netzwerkgerät wieder in „eth0“ umbenannt werden, wie in [Fixing eth0 in cloned SLES 11 VMware](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/)(Korrigieren von „eth0“ in geklonter SLES 11 VMware) beschrieben.

Neben den Erläuterungen in diesem Artikel empfehlen wir zudem, die folgende Datei zu entfernen:

   /lib/udev/rules.d/75-persistent-net-generator.rules

Sie können auch den Azure Linux Agent (waagent) installieren, um potenzielle Probleme zu vermeiden, solange es nicht mehrere Netzwerkkarten gibt.

## <a name="deploying-a-suse-vm-on-azure"></a>Bereitstellen einer SUSE-VM in Azure
Neue SUSE-VMs sollten Sie über JSON-Vorlagendateien im neuen Azure Resource Manager-Modell erstellen. Nach der Erstellung der JSON-Vorlagendatei können Sie die VM mithilfe des folgenden CLI-Befehls als Alternative zu PowerShell bereitstellen:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Weitere Informationen zu JSON-Vorlagendateien finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../../../resource-group-authoring-templates.md) und [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).

Weitere Informationen zu Befehlszeilenschnittstelle und Azure Resource Manager finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit Azure Resource Manager](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-Lizenz und Hardwareschlüssel
Für die offizielle SAP-Azure-Zertifizierung wurde ein neuer Mechanismus eingeführt, um den SAP-Hardwareschlüssel zu berechnen, der für die SAP-Lizenz verwendet wird. Zur Verwendung des neuen Algorithmus war eine Anpassung des SAP-Kernels erforderlich. Frühere SAP-Kernelversionen für Linux enthielten diese Codeänderung nicht. Daher konnte es in bestimmten Situationen vorkommen (z.B. bei einer Änderung der Größe der Azure-VM), dass sich der SAP-Hardwareschlüssel änderte und die SAP-Lizenz ihre Gültigkeit verlor. Eine Lösung mit neueren SAP-Linux-Kernels ist verfügbar.  Die detaillierten SAP-Kernel-Patches sind im SAP-Hinweis 1928533 dokumentiert.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE-Paket „sapconf“/tuned-adm
SUSE stellt ein Paket namens „sapconf“ bereit, das einen Satz an SAP-spezifischen Einstellungen verwaltet. Weitere Informationen zur Funktionsweise dieses Pakets sowie zur Installation und Verwendung finden Sie unter [Using sapconf to prepare a SUSE Linux Enterprise Server to run SAP systems](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) (Verwendung von „sapconf“ zur Vorbereitung einer SUSE Linux Enterprise Server-Instanz zum Ausführen von SAP-Systemen) und [What is sapconf or how to prepare a SUSE Linux Enterprise Server for running SAP systems?](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems) (Was ist „sapconf“ – oder wie wird eine SUSE Linux Enterprise Server-Instanz für die Ausführung von SAP-Systemen vorbereitet?).

Inzwischen ist ein Ersatz für „sapconf“ erschienen: tuned-adm. Weitere Informationen zu diesem Tool finden Sie unter den beiden folgenden Links:

- Die SLES-Dokumentation zum tuned-adm-Profil „sap-hana“ finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html). 

- Informationen zur Systemoptimierung für SAP-Workloads mit „tuned-adm“ finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) in Kapitel 7.2.

## <a name="nfs-share-in-distributed-sap-installations"></a>NFS-Freigabe in verteilten SAP-Installationen
Im Falle einer verteilten Installation, in der z. B. die Datenbank und die SAP-Anwendungsserver in separaten VMs installiert werden sollen, können Sie das Verzeichnis „/sapmnt“ über das Network File System (NFS) freigeben. Wenn nach der Erstellung der NFS-Freigabe für „/sapmnt“ Probleme mit den Installationsschritten auftreten, prüfen Sie, ob „no_root_squash“ für die Freigabe festgelegt wurde.

## <a name="logical-volumes"></a>Logische Datenträger (LVMs)
Wenn Sie in der Vergangenheit ein großes logisches Volume benötigt haben, das sich über mehrere Azure-Datenträger erstreckt (beispielsweise für die SAP-Datenbank), wurde die Verwendung des RAID-Verwaltungstools MDADM empfohlen, da Linux Logical Volume Manager (LVM) in Azure noch nicht vollständig geprüft war. Unter [Konfigurieren von Software-RAID unter Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erfahren Sie, wie Sie mithilfe von „mdadm“ ein Linux-RAID in Azure einrichten. Seit Anfang Mai 2016 wird auch Linux Logical Volume Manager (LVM) vollständig in Azure unterstützt und kann als Alternative zu MDADM verwendet werden. Weitere Informationen zu LVM in Azure finden Sie unter dem folgenden Link:  
[Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Azure SUSE Repository
Im Falle eines Problems mit dem Zugriff auf das Azure SUSE-Standardrepository können Sie es per Befehl zurücksetzen. Probleme dieser Art können auftreten, wenn Sie ein privates Betriebssystemimage in einer Azure-Region erstellen und das Image dann in eine andere Azure-Region kopieren, in der Sie neue virtuelle Computer auf der Grundlage dieses privaten Betriebssystemimages bereitstellen möchten. Führen Sie auf dem virtuellen Computer den folgenden Befehl aus:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome Desktop
Wenn Sie mithilfe von Gnome Desktop ein vollständiges SAP-Demosystem auf einem einzelnen virtuellen Computer installieren möchten (einschließlich SAP-Benutzeroberfläche, Browser und SAP-Verwaltungskonsole), beachten Sie den folgenden Tipp zur Installation für Azure SLES-Images:

   Für SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Für SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-Unterstützung für Oracle unter Linux in der Cloud
Es gibt eine Supporteinschränkung von Oracle unter Linux in virtualisierten Umgebungen. Diese Supporteinschränkung ist zwar kein Azure-spezifisches Thema, sollte Ihnen aber bekannt sein. In öffentlichen Clouds wie Azure unterstützt SAP Oracle unter SUSE oder Red Hat nicht. Um dieses Thema zu diskutieren, wenden Sie sich bitte direkt an Oracle.

