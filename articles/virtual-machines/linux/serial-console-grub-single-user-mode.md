---
title: Serielle Azure-Konsole für den GRUB- und Einzelbenutzermodus | Microsoft-Dokumentation
description: Verwenden der seriellen Konsole für GRUB in Azure Virtual Machines.
services: virtual-machines-linux
documentationcenter: ''
author: alsin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 059cb0cbc7e62af16dbf95693be421feebcc1ee0
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "42144383"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>Verwenden der seriellen Konsole zum Zugreifen auf den GRUB- und Einzelbenutzermodus
Der Einzelbenutzermodus ist eine minimale Umgebung mit minimaler Funktionalität. Er kann bei der Untersuchung von Start- oder Netzwerkproblemen hilfreich sein, da weniger Dienste im Hintergrund ausgeführt werden können und je nach Ausführungsebene sogar ein Dateisystem nicht automatisch eingebunden werden kann. So können beispielsweise Probleme wie ein beschädigtes Dateisystem, eine fehlerhafte fstab-Datei oder eine Unterbrechung der Netzwerkkonnektivität (falsche iptables-Konfiguration) untersucht werden.

Bei einigen Distributionen wird automatisch der Einzelbenutzermodus oder Notfallmodus aktiviert, wenn die VM nicht gestartet werden. Andere erfordern jedoch eine zusätzliche Einrichtung, bevor der Einzelbenutzer- oder Notfallmodus automatisch aktiviert werden kann.

Damit Sie auf den Einzelbenutzermodus zugreifen können, müssen Sie sicherstellen, dass GRUB auf Ihrer VM aktiviert ist. Je nach Distribution müssen Sie möglicherweise einige Einrichtungsschritte durchführen, um sicherzustellen, dass GRUB aktiviert ist. 


## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Zugriff für Red Hat Enterprise Linux (RHEL)
RHEL aktiviert automatisch den Einzelbenutzermodus, wenn ein normaler Start nicht möglich ist. Wenn Sie keinen Stammzugriff für den Einzelbenutzermodus eingerichtet haben, haben Sie jedoch kein Stammkennwort und können sich nicht anmelden. Es ist eine Problemumgehung verfügbar (siehe „Manuelles Aktivieren des Einzelbenutzermodus“ unten), es wird jedoch empfohlen, zuerst den Stammzugriff einzurichten.

### <a name="grub-access-in-rhel"></a>GRUB-Zugriff in RHEL
In RHEL ist GRUB standardmäßig aktiviert. Starten Sie zum Aktivieren des GRUB-Modus Ihre VM mit `sudo reboot` neu, und drücken Sie eine beliebige Taste. Daraufhin wird der GRUB-Bildschirm angezeigt.

> Hinweis: Red Hat bietet auch eine Dokumentation zum Start im Wiederherstellungsmodus, Notfallmodus, Debugmodus und Zurücksetzen des Stammkennworts. [Klicken Sie hier, um auf die Dokumentation zuzugreifen](https://aka.ms/rhel7grubterminal).

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>Einrichten des Stammzugriffs für den Einzelbenutzermodus in RHEL
Für den Einzelbenutzermodus in RHEL muss der Root-Benutzer aktiviert werden. Dieser ist standardmäßig deaktiviert. Wenn Sie den Einzelbenutzermodus aktivieren möchten, gehen Sie wie folgt vor:

1. Melden Sie sich über SSH beim Red Hat-System an.
1. Wechseln Sie zum Stamm.
1. Aktivieren Sie das Kennwort für den Root-Benutzer. 
    * `passwd root` (Legen Sie ein sicheres Root-Kennwort fest.)
1. Stellen Sie sicher, dass sich der Root-Benutzer nur über ttyS0 anmelden kann.
    * `edit /etc/ssh/sshd_config`, und stellen Sie sicher, dass „PermitRootLogIn“ auf „no“ festgelegt ist.
    * `edit /etc/securetty file`, um die Anmeldung nur über ttyS0 zuzulassen. 

Wenn das System nun im Einzelbenutzermodus gestartet wird, können Sie sich über das Root-Kennwort anmelden.

Alternativ können Sie für RHEL 7.4+ oder 6.9+ den Einzelbenutzermodus in den GRUB-Eingabeaufforderungen aktivieren. Anweisungen dazu finden Sie [hier](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single).

### <a name="manually-enter-single-user-mode-in-rhel"></a>Manuelles Aktivieren des Einzelbenutzermodus in RHEL
Wenn Sie GRUB und den Stammzugriff wie oben beschrieben eingerichtet haben, können Sie den Einzelbenutzermodus wie folgt aktivieren:

1. Drücken Sie während des Neustarts der VM die ESC-TASTE, um den GRUB-Modus zu aktivieren.
1. Drücken Sie auf dem GRUB-Bildschirm „e“, um das ausgewählte Betriebssystem zu bearbeiten, in das Sie starten möchten (in der Regel ist dies die erste Zeile).
1. Suchen Sie nach der Kernelzeile. In Azure beginnt diese mit `linux16`.
1. Drücken Sie STRG+E, um zum Ende der Zeile zu wechseln.
1. Fügen Sie am Ende der Zeile Folgendes hinzu: `systemd.unit=rescue.target`
    * Dadurch starten Sie im Einzelbenutzermodus. Wenn Sie den Notfallmodus verwenden möchten, fügen Sie anstelle von `systemd.unit=rescue.target` am Ende der Zeile `systemd.unit=emergency.target` hinzu.
1. Drücken Sie STRG+X, um den Modus zu beenden und einen Neustart mit den angewendeten Einstellungen durchzuführen.
1. Sie werden zur Eingabe des Administratorkennworts aufgefordert, bevor Sie in den Einzelbenutzermodus wechseln können. Dies ist das Kennwort, das Sie in den obigen Anweisungen erstellt haben.    

    ![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>Aktivieren des Einzelbenutzermodus ohne aktiviertes Stammkonto in RHEL
Wenn Sie die obigen Schritte zum Aktivieren des Stammbenutzers nicht ausgeführt haben, können Sie dennoch das Stammkennwort zurücksetzen. Gehen Sie folgendermaßen vor:

> Hinweis: Wenn Sie SELinux verwenden, stellen Sie bitte sicher, dass Sie beim Zurücksetzen des Stammkennworts die in der [Red Hat-Dokumentation](https://aka.ms/rhel7grubterminal) beschriebenen zusätzlichen Schritte ausgeführt haben.

1. Drücken Sie während des Neustarts der VM die ESC-TASTE, um den GRUB-Modus zu aktivieren.
1. Drücken Sie auf dem GRUB-Bildschirm „e“, um das ausgewählte Betriebssystem zu bearbeiten, in das Sie starten möchten (in der Regel ist dies die erste Zeile).
1. Suchen Sie nach der Kernelzeile. In Azure beginnt diese mit `linux16`.
1. Fügen Sie `rd.break` am Ende der Zeile hinzu, und achten Sie darauf, dass ein Leerzeichen vor `rd.break` vorhanden ist (siehe folgendes Beispiel).
    - Dadurch wird der Startprozess unterbrochen, bevor die Steuerung von `initramfs` an `systemd` übergeben wird (Informationen dazu finden Sie in der [Red Hat-Dokumentation](https://aka.ms/rhel7rootpassword)).
1. Drücken Sie STRG+X, um den Modus zu beenden und einen Neustart mit den angewendeten Einstellungen durchzuführen.
1. Nach dem Start wird der Notfallmodus mit einem schreibgeschützten Dateisystem aktiviert. Geben Sie in der Shell `mount -o remount,rw /sysroot` ein, um das Stammdateisystem mit Lese-/Schreibberechtigungen erneut einzubinden.
1. Geben Sie nach dem Start im Einzelbenutzermodus `chroot /sysroot` ein, um in das `sysroot`-Jail zu wechseln.
1. Sie sind jetzt der Root-Benutzer. Sie können Ihr Stammkennwort mit `passwd` zurücksetzen und anschließend wie oben beschrieben in den Einzelbenutzermodus wechseln. Geben Sie anschließend `reboot -f` ein, um neu zu starten.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> Hinweis: Wenn Sie den obigen Anweisungen folgen, gelangen Sie in die Notfall-Shell, sodass Sie auch Aufgaben wie das Bearbeiten von `fstab` ausführen können. Im Allgemeinen wird jedoch empfohlen, das Stammkennwort zurückzusetzen und damit den Einzelbenutzermodus zu aktivieren. 


## <a name="access-for-centos"></a>Zugriff für CentOS
Ähnlich wie in Red Hat Enterprise Linux erfordert der Einzelbenutzermodus in CentOS, dass GRUB und der Stammbenutzer aktiviert sind. 

### <a name="grub-access-in-centos"></a>GRUB-Zugriff in CentOS
In CentOS ist GRUB standardmäßig aktiviert. Starten Sie zum Aktivieren des GRUB-Modus Ihre VM mit `sudo reboot` neu, und drücken Sie eine beliebige Taste. Daraufhin wird der GRUB-Bildschirm angezeigt.

### <a name="single-user-mode-in-centos"></a>Einzelbenutzermodus in CentOS
Befolgen Sie die obigen Anweisungen für RHEL, um den Einzelbenutzermodus in CentOS zu aktivieren.

## <a name="access-for-ubuntu"></a>Zugriff für Ubuntu 
Ubuntu-Images erfordern kein Stammkennwort. Wenn das System im Einzelbenutzermodus gestartet wird, können Sie es ohne zusätzliche Anmeldeinformationen verwenden. 

### <a name="grub-access-in-ubuntu"></a>GRUB-Zugriff in Ubuntu
Halten Sie zum Zugreifen auf GRUB während des Starts der VM die ESC-TASTE gedrückt.

### <a name="single-user-mode-in-ubuntu"></a>Einzelbenutzermodus in Ubuntu
Ubuntu aktiviert automatisch den Einzelbenutzermodus, wenn ein normaler Start nicht möglich ist. Um den Einzelbenutzermodus manuell zu aktivieren, gehen Sie wie folgt vor:

1. Drücken Sie auf dem GRUB-Bildschirm „e“, um Ihren Starteintrag (Ubuntu-Eintrag) zu bearbeiten.
1. Suchen Sie nach der Zeile, die mit `linux` beginnt, und dann nach `ro`.
1. Fügen Sie `single` nach `ro` hinzu, und achten Sie darauf, dass vor und nach `single` ein Leerzeichen vorhanden ist.
1. Drücken Sie STRG+X, um einen Neustart mit diesen Einstellungen durchzuführen und den Einzelbenutzermodus zu aktivieren.

## <a name="access-for-coreos"></a>Zugriff für CoreOS
Der Einzelbenutzermodus in CoreOS erfordert, dass GRUB aktiviert ist. 

### <a name="grub-access-in-coreos"></a>GRUB-Zugriff in CoreOS
Drücken Sie zum Zugreifen auf GRUB während des Starts der VM eine beliebige Taste.

### <a name="single-user-mode-in-coreos"></a>Einzelbenutzermodus in CoreOS
CoreOS aktiviert automatisch den Einzelbenutzermodus, wenn ein normaler Start nicht möglich ist. Um den Einzelbenutzermodus manuell zu aktivieren, gehen Sie wie folgt vor:
1. Drücken Sie auf dem GRUB-Bildschirm „e“, um Ihren Starteintrag zu bearbeiten.
1. Suchen Sie nach der Zeile, die mit `linux$` beginnt. Es sollte 2, eingekapselt in verschiedene If-/Else-Klauseln, angezeigt werden.
1. Fügen Sie `coreos.autologin=ttyS0` an das Ende beider `linux$`-Zeilen an.
1. Drücken Sie STRG+X, um einen Neustart mit diesen Einstellungen durchzuführen und den Einzelbenutzermodus zu aktivieren.

## <a name="access-for-suse-sles"></a>Zugriff für SUSE SLES
Neuere Images von SLES 12 SP3 und höher ermöglichen den Zugriff über die serielle Konsole, wenn das System im Notfallmodus gestartet wird. 

### <a name="grub-access-in-suse-sles"></a>GRUB-Zugriff in SUSE SLES
Der GRUB-Zugriff in SLES erfordert eine Bootloaderkonfiguration über YaST. Gehen Sie dazu wie folgt vor:

1. Stellen Sie eine SSH-Verbindung mit Ihrer SLES-VM her, und führen Sie `sudo yast bootloader` aus. Navigieren Sie mit den Tasten `tab` und `enter` sowie den Pfeiltasten durch das Menü. 
1. Navigieren Sie zu `Kernel Parameters`, und überprüfen Sie `Use serial console`. 
1. Fügen Sie den Konsolenargumenten `serial --unit=0 --speed=9600 --parity=no` hinzu.

1. Drücken Sie zum Speichern Ihrer Einstellungen und Beenden F10.
1. Starten Sie zum Aktivieren des GRUB-Modus Ihre VM neu, und drücken Sie während der Startsequenz eine beliebige Taste, damit der GRUB-Bildschirm nicht geschlossen wird.
    - Das Standardzeitlimit für GRUB ist 1 Sekunde. Sie können diese Einstellung ändern, indem Sie die Variable `GRUB_TIMEOUT` in `/etc/default/grub` bearbeiten.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>Einzelbenutzermodus in SUSE SLES
Die Notfall-Shell wird automatisch geöffnet, wenn SLES nicht normal gestartet werden kann. Gehen Sie wie folgt vor, um die Notfall-Shell manuell zu öffnen:

1. Drücken Sie auf dem GRUB-Bildschirm „e“, um Ihren Starteintrag (SLES-Eintrag) zu bearbeiten.
1. Suchen Sie nach der Kernelzeile. Diese beginnt mit `linux`.
1. Fügen Sie `systemd.unit=emergency.target` an das Ende der Zeile an.
1. Drücken Sie STRG+X, um einen Neustart mit diesen Einstellungen durchzuführen und die Notfall-Shell zu öffnen.
> Beachten Sie, dass die Notfall-Shell mit einem _schreibgeschützten_ Dateisystem aktiviert wird. Wenn Sie Dateien bearbeiten möchten, müssen Sie das Dateisystem mit Lese-/Schreibberechtigungen erneut einbinden. Geben Sie dazu in der Shell `mount -o remount,rw /` ein.

## <a name="access-for-oracle-linux"></a>Zugriff für Oracle Linux
Ähnlich wie in Red Hat Enterprise Linux erfordert der Einzelbenutzermodus in Oracle Linux, dass GRUB und der Stammbenutzer aktiviert sind. 

### <a name="grub-access-in-oracle-linux"></a>GRUB-Zugriff in Oracle Linux
In Oracle Linux ist GRUB standardmäßig aktiviert. Starten Sie zum Aktivieren des GRUB-Modus Ihre VM mit `sudo reboot` neu, und drücken Sie die ESC-TASTE. Daraufhin wird der GRUB-Bildschirm angezeigt.

### <a name="single-user-mode-in-oracle-linux"></a>Einzelbenutzermodus in Oracle Linux
Befolgen Sie die obigen Anweisungen für RHEL, um den Einzelbenutzermodus in Oracle Linux zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte
* Die Hauptseite der Linux-Dokumentation für die serielle Konsole finden Sie [hier](serial-console.md).
* Verwenden der seriellen Konsole für [NMI- und SysRq-Aufrufe](serial-console-nmi-sysrq.md)
* Die serielle Konsole ist auch für [Windows](../windows/serial-console.md)-VMs verfügbar.
* Erfahren Sie mehr über die [Startdiagnose](boot-diagnostics.md).