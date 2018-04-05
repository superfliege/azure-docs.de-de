---
title: Serielle Konsole für virtuelle Azure-Computer | Microsoft-Dokumentation
description: Bidirektionale serielle Konsole für virtuelle Azure-Computer.
services: virtual-machines-linux
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: b7d6e48a6f34472bc38947fd70e850b1c3bf6f8a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-machine-serial-console-preview"></a>Serielle Konsole für virtuelle Computer (Vorschauversion) 


Die serielle Konsole für virtuelle Computer in Azure bietet Zugriff auf eine textbasierte Konsole für virtuelle Linux- und Windows-Computer. Diese serielle Verbindung erfolgt mit dem seriellen COM1-Port des virtuellen Computers und ermöglicht den Zugriff auf den virtuellen Computer. Sie steht nicht in Beziehung zum Zustand des Netzwerks/Betriebssystems des virtuellen Computers. Der Zugriff auf die serielle Konsole für einen virtuellen Computer kann zurzeit nur über das Azure-Portal erfolgen und ist nur für Benutzer mit der Zugriffsberechtigung „VM-Mitwirkender“ oder höher für den virtuellen Computer möglich. 

> [!Note] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen]. (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Zurzeit befindet sich dieser Dienst in der **Public Preview**, und der Zugriff auf die serielle Konsole für virtuelle Computer ist für globale Azure-Regionen verfügbar. Zu diesem Zeitpunkt ist die serielle Konsole in der Azure Government-, Azure Deutschland- und Azure China-Cloud nicht verfügbar.


## <a name="prerequisites"></a>Voraussetzungen 

* Für den virtuellen Computer MUSS die [Startdiagnose](boot-diagnostics.md) aktiviert sein. 
* Das Konto, das die serielle Konsole verwendet, muss die Rolle [Mitwirkender](../../active-directory/role-based-access-built-in-roles.md) für den virtuellen Computer und das Speicherkonto [Startdiagnose](boot-diagnostics.md) aufweisen. 
* Spezifische Einstellungen für die Linux-Distribution finden Sie unter [Zugriff auf die serielle Konsole für Linux](#accessing-serial-console-for-linux).


## <a name="open-the-serial-console"></a>Öffnen der seriellen Konsole
Auf die serielle Konsole für virtuelle Computer kann nur über das [Azure-Portal](https://portal.azure.com) zugegriffen werden. Im Folgenden werden die Schritte für den Zugriff auf die serielle Konsole für virtuelle Computer über das Portal beschrieben. 

  1. Öffnen Sie das Azure-Portal.
  2. Wählen Sie im Menü auf der linken Seite die Option „Virtuelle Computer“ aus.
  3. Klicken Sie in der Liste auf den virtuellen Computer. Die Übersichtsseite für den virtuellen Computer wird geöffnet.
  4. Scrollen Sie nach unten zum Abschnitt „Support und Problembehandlung“, und klicken Sie auf die Option „Serielle Konsole (Vorschau)“. Ein neuer Bereich mit der seriellen Konsole wird geöffnet, und die Verbindung wird hergestellt.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Für die serielle Konsole ist ein konfigurierter lokaler Benutzer mit einem Kennwort erforderlich. Derzeit haben virtuelle Computer, die nur mit einem öffentlichen SSH-Schlüssel konfiguriert sind, keinen Zugriff auf die serielle Konsole. Erstellen Sie einen lokalen Benutzer mit einem Kennwort. Befolgen Sie dazu die Anweisungen unter [VMAccess-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension).

### <a name="disable-feature"></a>Deaktivieren des Features
Die Funktionalität der seriellen Konsole kann für bestimmte virtuelle Computer deaktiviert werden, indem die Startdiagnoseeinstellung des jeweiligen virtuellen Computers deaktiviert wird.

## <a name="serial-console-security"></a>Sicherheit der seriellen Konsole 

### <a name="access-security"></a>Zugriffssicherheit 
Der Zugriff auf die serielle Konsole ist auf Benutzer eingeschränkt, die über die Zugriffsberechtigung [VM-Mitwirkende](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) oder höher für den virtuellen Computer verfügen. Wenn Ihr AAD-Mandant mehrstufige Authentifizierung erfordert, ist auch für den Zugriff auf die serielle Konsole mehrstufige Authentifizierung erforderlich, da der Zugriff über das [Azure-Portal](https://portal.azure.com) erfolgt.

### <a name="channel-security"></a>Kanalsicherheit
Alle gesendeten Daten werden bei der Übertragung verschlüsselt.

### <a name="audit-logs"></a>Überwachungsprotokolle
Alle Zugriffe auf die serielle Konsole werden zurzeit in den Protokollen der [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) des virtuellen Computers protokolliert. Der Zugriff auf diese Protokolle wird durch den Administrator des virtuellen Azure-Computers (der auch Besitzer dieser Protokolle ist) gesteuert.  

>[!CAUTION] 
Es werden keine Zugriffskennwörter für die Konsole protokolliert. Wenn jedoch Befehle, die innerhalb der Konsole ausgeführt werden, Kennwörter, Geheimnisse, Benutzernamen oder eine andere Form von persönlich identifizierbaren Informationen (PII) enthalten oder ausgeben, werden diese zusammen mit dem anderen sichtbaren Text als Teil der Implementierung der Scrollbackfunktionalität der seriellen Konsole in die Startdiagnoseprotokolle des virtuellen Computers geschrieben. Diese Protokolle sind zirkulär, und nur Personen mit Leseberechtigungen für das Diagnosespeicherkonto haben Zugriff auf sie. Es wird jedoch empfohlen, bewährte Methoden bei der Verwendung der SSH-Konsole für alle Aspekte zu befolgen, die Geheimnisse und/oder PII beinhalten können. 

### <a name="concurrent-usage"></a>Parallele Verwendung
Wenn ein Benutzer mit der seriellen Konsole verbunden ist und ein anderer Benutzer erfolgreich den Zugriff auf denselben virtuellen Computer anfordert, wird der erste Benutzer getrennt und der zweite Benutzer in einer Art und Weise verbunden, als würde der erste Benutzer aufstehen und die physische Konsole verlassen und ein neuer Benutzer dessen Platz einnehmen.

>[!CAUTION] 
Dies bedeutet, dass der Benutzer, der getrennt wird, nicht abgemeldet wird! Die Möglichkeit, eine Abmeldung beim Trennen der Verbindung (über SIGHUP oder einen ähnlichen Mechanismus) zu erzwingen, befindet sich noch in Planung. Für Windows ist ein automatisches Timeout in SAC aktiviert, für Linux können Sie jedoch die Terminaltimeouteinstellung konfigurieren. Fügen Sie dazu einfach `export TMOUT=600` in der Datei „.bash_profile“ oder „.profile“ für den Benutzer ein, mit dem Sie sich an der Konsole anmelden, sodass das Timeout für die Sitzung nach 10 Minuten erfolgt.

### <a name="disable-feature"></a>Deaktivieren des Features
Die Funktionalität der seriellen Konsole kann für bestimmte virtuelle Computer deaktiviert werden, indem die Startdiagnoseeinstellung des jeweiligen virtuellen Computers deaktiviert wird.

## <a name="common-scenarios-for-accessing-serial-console"></a>Gängige Szenarien für den Zugriff auf die serielle Konsole 
Szenario          | Aktionen in der seriellen Konsole                |  Anwendbarkeit im Betriebssystem 
:------------------|:-----------------------------------------|:------------------
Fehlerhafte fstab-Datei | Drücken der Eingabetaste, um fortzufahren, und Korrigieren der fstab-Datei in einem Text-Editor. Siehe Informationen zum [Beheben von fstab-Fehlern](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors). | Linux 
Falsche Firewallregeln | Zugriff auf die serielle Konsole, um iptables oder Windows-Firewallregeln zu korrigieren | Linux/Windows 
Dateisystembeschädigung/-überprüfung | Zugriff auf die serielle Konsole und Wiederherstellung des Dateisystems | Linux/Windows 
SSH/RDP-Konfigurationsprobleme | Zugriff auf die serielle Konsole und Ändern von Einstellungen | Linux/Windows 
Netzwerksperrsystem| Zugriff auf die serielle Konsole über das Portal zum Verwalten des Systems | Linux/Windows 
Interaktion mit Bootloader | Zugriff auf GRUB/BCD über die serielle Konsole | Linux/Windows 

## <a name="accessing-serial-console-for-linux"></a>Zugriff auf die serielle Konsole für Linux
Damit die serielle Konsole ordnungsgemäß ausgeführt wird, muss das Gastbetriebssystem so konfiguriert werden, dass Konsolenmeldungen über den seriellen Anschluss gelesen und geschrieben werden. In den meisten [von Azure unterstützten Linux-Distributionen](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) ist die serielle Konsole standardmäßig konfiguriert. Durch einfaches Klicken im Portal auf den Abschnitt „Serielle Konsole“ erhalten Sie Zugriff auf die Konsole. 

### <a name="access-for-redhat"></a>Zugriff für Red Hat 
Für in Azure verfügbare Red Hat-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. Für den Einzelbenutzermodus in Red Hat muss der Root-Benutzer aktiviert werden. Dieser ist standardmäßig deaktiviert. Wenn Sie den Einzelbenutzermodus aktivieren möchten, gehen Sie wie folgt vor:

1. Melden Sie sich über SSH beim Red Hat-System an.
2. Aktivieren Sie das Kennwort für den Root-Benutzer. 
 * `passwd root` (Legen Sie ein sicheres Root-Kennwort fest.)
3. Stellen Sie sicher, dass sich der Root-Benutzer nur über ttyS0 anmelden kann.
 * `edit /etc/ssh/sshd_config`, und stellen Sie sicher, dass PermitRootLog auf „no“ festgelegt ist.
 * `edit /etc/securetty file`, um die Anmeldung nur über ttyS0 zuzulassen. 

Wenn das System nun im Einzelbenutzermodus gestartet wird, können Sie sich über das Root-Kennwort anmelden.

Alternativ können Sie für RHEL 7.4+ oder 6.9+ den Einzelbenutzermodus in den GRUB-Eingabeaufforderungen aktivieren. Anweisungen dazu finden Sie [hier](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single).

### <a name="access-for-ubuntu"></a>Zugriff für Ubuntu 
Für in Azure verfügbare Ubuntu-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. Wenn das System im Einzelbenutzermodus gestartet wird, können Sie ohne zusätzliche Anmeldeinformationen darauf zugreifen. 

### <a name="access-for-coreos"></a>Zugriff für CoreOS
Für in Azure verfügbare CoreOS-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. Das System kann gegebenenfalls durch Ändern von GRUB-Parametern im Einzelbenutzermodus gestartet werden. Durch Hinzufügen von `coreos.autologin=ttyS0` kann sich der Benutzer anmelden und auf die serielle Konsole zugreifen. 

### <a name="access-for-suse"></a>Zugriff für SUSE
Für in Azure verfügbare SLES-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. Wenn Sie ältere Versionen von SLES in Azure verwenden, befolgen Sie die Anweisungen in [diesem KB-Artikel](https://www.novell.com/support/kb/doc.php?id=3456486), um die serielle Konsole zu aktivieren. Neuere Images von SLES 12 SP3+ ermöglichen zudem den Zugriff über die serielle Konsole für den Fall, dass das System im Notfallmodus gestartet wird.

### <a name="access-for-centos"></a>Zugriff für CentOS
Für in Azure verfügbare CentOS-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. Für den Einzelbenutzermodus sind Schritte entsprechend den oben für Red Hat-Images aufgeführten Anweisungen auszuführen. 

### <a name="access-for-oracle-linux"></a>Zugriff für Oracle Linux
Für in Azure verfügbare Oracle Linux-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. Für den Einzelbenutzermodus sind Schritte entsprechend den oben für Red Hat-Images aufgeführten Anweisungen auszuführen.

### <a name="access-for-custom-linux-image"></a>Zugriff für ein benutzerdefiniertes Linux-Image
Um die serielle Konsole für Ihr benutzerdefiniertes Linux-VM-Image zu aktivieren, aktivieren Sie den Konsolenzugriff in „/etc/inittab“, um ein Terminal auf ttyS0 auszuführen. Es folgt ein Beispiel, um dies in der Datei „inittab“ hinzuzufügen. 

`S0:12345:respawn:/sbin/agetty -L 115200 console vt102` 

## <a name="errors"></a>Errors
Die meisten Fehler sind vorübergehender Natur und können durch einen Wiederholungsversuch der Verbindung behoben werden. In der folgenden Tabelle sind einige Fehler und deren Behebung aufgeführt. 

Error                            |   Lösung 
:---------------------------------|:--------------------------------------------|
Startdiagnoseeinstellungen für „<VMNAME>“ können nicht abgerufen werden. Damit Sie die serielle Konsole verwenden können, stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist. | Stellen Sie sicher, dass für den virtuellen Computer [Startdiagnose](boot-diagnostics.md) aktiviert ist. 
Der virtuelle Computer befindet sich in einem beendeten Zustand mit aufgehobener Zuordnung. Starten Sie den virtuellen Computer, und stellen Sie erneut eine Verbindung mit der seriellen Konsole her. | Der virtuelle Computer muss den Zustand „Gestartet“ aufweisen, um auf die serielle Konsole zugreifen zu können.
Sie verfügen nicht über die erforderlichen Berechtigungen zum Verwenden der seriellen Konsole dieses virtuellen Computers. Stellen Sie sicher, dass Sie mindestens über Berechtigungen der Rolle „VM-Mitwirkender“ verfügen.| Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).
Die Ressourcengruppe für das Startdiagnose-Speicherkonto „<STORAGEACCOUNTNAME>“ kann nicht ermittelt werden. Stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist und Sie über Zugriff auf dieses Speicherkonto verfügen. | Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).

## <a name="known-issues"></a>Bekannte Probleme 
Da wir uns noch in der Vorschauphase für den Zugriff auf die serielle Konsole befinden, arbeiten wir an einigen bekannten Problemen. Im Folgenden finden Sie die Liste dieser Probleme mit möglichen Problemumgehungen. 

Problem                           |   Lösung 
:---------------------------------|:--------------------------------------------|
Es gibt keine Option der seriellen Konsole für VM-Skalierungsgruppeninstanzen. |  Zum Zeitpunkt der Vorschau wird der Zugriff auf die serielle Konsole für VM-Skalierungsgruppeninstanzen nicht unterstützt.
Nach dem Drücken der Eingabetaste nach dem Verbindungsbanner wird keine Anmeldeeingabeaufforderung angezeigt. | [Nach dem Drücken der Eingabetaste geschieht nichts](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md).


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 
**F: Wie kann ich Feedback senden?**

A. Stellen Sie Feedback als Problem bereit, indem Sie zu https://aka.ms/serialconsolefeedback navigieren. Alternativ können Sie (weniger bevorzugt) Feedback über azserialhelp@microsoft.com oder in der Kategorie „Virtuelle Computer“ von http://feedback.azure.com senden.

**F: Ich erhalte eine Fehlermeldung „Die vorhandene Konsole verursacht einen Konflikt des Betriebssystemtyps ‚Windows‘ mit dem angeforderten Betriebssystemtyp Linux“.**

A. Dies ist ein bekanntes Problem. Um dieses Problem zu beheben, öffnen Sie einfach die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) im Bash-Modus, und versuchen Sie es erneut.

**F: Ich bin nicht in der Lage, auf die serielle Konsole zugreifen. Wo kann ich eine Supportanfrage stellen?**

A. Dieses Vorschaufeature wird durch die Nutzungsbedingungen für die Azure-Vorschau abgedeckt. Unterstützung erfolgt am besten über die oben genannten Kanäle. 

## <a name="next-steps"></a>Nächste Schritte
* Die serielle Konsole ist auch für [virtuelle Windows-Computer](../windows/serial-console.md) verfügbar.
* Weitere Informationen zur [Startdiagnose](boot-diagnostics.md)