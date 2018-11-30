---
title: Serielle Konsole für Azure-VMs unter Linux | Microsoft-Dokumentation
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
ms.date: 10/31/2018
ms.author: harijay
ms.openlocfilehash: 0c47600082a2c633116d1e85e9f31324544c2c57
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52261750"
---
# <a name="virtual-machine-serial-console-for-linux"></a>Serielle Konsole für virtuelle Computer für Linux

Die serielle Konsole für virtuelle Computer (VMs) im Azure-Portal ermöglicht den Zugriff auf eine textbasierte Konsole für virtuelle Linux-Computer. Diese serielle Verbindung erfolgt über den seriellen COM1-Port des virtuellen Computers und ermöglicht Zugriff auf diesen, unabhängig vom Zustand des Netzwerks oder Betriebssystems des virtuellen Computers. Der Zugriff auf die serielle Konsole für einen virtuellen Computer kann nur über das Azure-Portal erfolgen. Er ist nur für Benutzer zulässig, die über eine Zugriffsrolle als Mitwirkender für virtuelle Computer oder höher für den virtuellen Computer verfügen. 

Die Dokumentation zur seriellen Konsole für Windows-VMs finden Sie unter [Serielle Konsole für virtuelle Computer für Windows](../windows/serial-console.md).

> [!NOTE] 
> Die serielle Konsole für virtuelle Computer ist in globalen Azure-Regionen allgemein verfügbar. Sie ist noch nicht in den Clouds „Azure Government“ und „Azure China“ verfügbar.


## <a name="prerequisites"></a>Voraussetzungen 

- Die VM, auf der Sie auf eine serielle Konsole zugreifen, muss das Ressourcenverwaltungs-Bereitstellungsmodell verwenden. Klassische Bereitstellungen werden nicht unterstützt. 

- Auf der VM, auf der Sie auf eine serielle Konsole zugreifen, muss [Startdiagnose](boot-diagnostics.md) aktiviert sein. 

    ![Einstellungen der Startdiagnose](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Ein Konto, das eine serielle Konsole verwendet, muss die Rolle [Mitwirkender für virtuelle Computer](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) für die VM und das Speicherkonto [Startdiagnose](boot-diagnostics.md) aufweisen: 

    - Die VM, auf der Sie auf eine serielle Konsole zugreifen, muss über ein kennwortbasiertes Konto verfügen. Mit der Funktion [Kennwort zurücksetzen](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) der Erweiterungen für den Zugriff auf virtuelle Computer können Sie eines erstellen. Wählen Sie im Abschnitt **Support + Problembehandlung** **Kennwort zurücksetzen** aus. 

    - Spezifische Einstellungen für Linux-Distributionen finden Sie unter [Verfügbarkeit der seriellen Konsole in Linux-Distributionen](#serial-console-linux-distribution-availability).



## <a name="get-started-with-the-serial-console"></a>Erste Schritte mit der seriellen Konsole
Auf die serielle Konsole für virtuelle Computer kann nur über das Azure-Portal zugegriffen werden:

  1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

  1. Wählen Sie im linken Menü **Virtual Machines** (Virtuelle Computer) aus.

  1. Wählen Sie in der Liste eine VM aus. Die Übersichtsseite für die VM wird geöffnet.

  1. Scrollen Sie nach unten zum Abschnitt **Support + Problembehandlung**, und wählen Sie **Serielle Konsole** aus. Ein neuer Bereich mit der seriellen Konsole wird geöffnet, und die Verbindung wird hergestellt.

     ![Fenster der seriellen Konsole unter Linux](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Die serielle Konsole erfordert einen lokalen Benutzer mit konfiguriertem Kennwort. VMs, die nur mit einem öffentlichen SSH-Schlüssel konfiguriert sind, können sich nicht bei der seriellen Konsole anmelden. Wenn Sie einen lokalen Benutzer mit Kennwort erstellen möchten, verwenden Sie die [Erweiterung für VM-Zugriff](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension), die im Azure-Portal durch Auswählen von **Kennwort zurücksetzen** verfügbar ist, und erstellen Sie einen lokalen Benutzer mit einem Kennwort.
> Sie können auch das Administratorkennwort in Ihrem Konto zurücksetzen, indem Sie [GRUB verwenden, um im Einzelbenutzermodus zu starten](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distribution-availability"></a>Verfügbarkeit der seriellen Konsole in Linux-Distributionen
Damit die serielle Konsole ordnungsgemäß ausgeführt wird, muss das Gastbetriebssystem so konfiguriert werden, dass Konsolenmeldungen über den seriellen Anschluss gelesen und geschrieben werden. In den meisten [von Azure unterstützten Linux-Distributionen](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) ist die serielle Konsole standardmäßig konfiguriert. Zugriff auf die serielle Konsole erhalten Sie, indem Sie im Azure-Portal im Bereich **Support + Problembehandlung****Serielle Konsole** auswählen. 

Distribution      | Zugriff auf die serielle Konsole
:-----------|:---------------------
Red Hat Enterprise Linux    | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert. 
CentOS      | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert. 
Ubuntu      | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
CoreOS      | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
SUSE        | Für die neueren SLES-Images, die auf Azure verfügbar sind, ist der Zugriff auf die serielle Konsole standardmäßig aktiviert. Wenn Sie in Azure ältere Versionen von SLES (10 oder niedriger) verwenden, lesen Sie die Anweisungen in [diesem KB-Artikel](https://www.novell.com/support/kb/doc.php?id=3456486), um die serielle Konsole zu aktivieren. 
Oracle Linux        | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
Benutzerdefinierte Linux-Images     | Um die serielle Konsole für Ihr benutzerdefiniertes Linux-VM-Image zu aktivieren, aktivieren Sie den Konsolenzugriff in der Datei */etc/inittab*, um ein Terminal auf `ttyS0` auszuführen. Beispiel: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Weitere Informationen zur ordnungsgemäßen Erstellung von benutzerdefinierten Images finden Sie unter [Erstellen und Hochladen einer Linux-VHD in Azure](https://aka.ms/createuploadvhd). Wenn Sie einen benutzerdefinierten Kernel erstellen, sollten Sie die Aktivierung dieser Kernelflags in Erwägung ziehen: `CONFIG_SERIAL_8250=y` und `CONFIG_MAGIC_SYSRQ_SERIAL=y`. Die Konfigurationsdatei befindet sich normalerweise im Pfad */boot/*.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Häufige Szenarien für den Zugriff auf die serielle Konsole 
Szenario          | Aktionen in der seriellen Konsole                
:------------------|:-----------------------------------------
Fehlerhafte *FSTAB*-Datei | Drücken Sie die **EINGABETASTE**, um fortzufahren, und verwenden Sie einen Text-Editor, um die Datei *FSTAB* zu korrigieren. Dazu müssen Sie sich möglicherweise im Einzelbenutzermodus befinden. Weitere Informationen finden Sie unter [Beheben von FSTAB-Fehlern](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) und [Verwenden der seriellen Konsole zum Zugreifen auf GRUB und den Einzelbenutzermodus](serial-console-grub-single-user-mode.md).
Falsche Firewallregeln | Greifen Sie auf die serielle Konsole zu, und korrigieren Sie iptables. 
Dateisystembeschädigung/-überprüfung | Greifen Sie auf die serielle Konsole zu, und stellen Sie das Dateisystem wieder her. 
SSH/RDP-Konfigurationsprobleme | Greifen Sie auf die serielle Konsole zu, und ändern Sie die Einstellungen. 
Netzwerksperrsystem| Greifen Sie aus dem Azure-Portal auf die serielle Konsole zu, um das System zu verwalten. 
Interaktion mit Bootloader | Greifen Sie aus der seriellen Konsole auf GRUB zu. Weitere Informationen finden Sie unter [Verwenden der seriellen Konsole zum Zugreifen auf den GRUB- und Einzelbenutzermodus](serial-console-grub-single-user-mode.md). 

## <a name="disable-the-serial-console"></a>Deaktivieren der seriellen Konsole
Standardmäßig haben alle Abonnements Zugriff auf die serielle Konsole, die für alle virtuellen Computer aktiviert ist. Sie können die serielle Konsole auf Abonnement- oder VM-Ebene deaktivieren.

> [!NOTE] 
> Um die serielle Konsole für ein Abonnement zu aktivieren oder zu deaktivieren, müssen Sie über Schreibberechtigungen für das Abonnement verfügen. Diese Berechtigungen schließen Administrator- oder Besitzerrollen ein. Benutzerdefinierte Rollen können ebenfalls über Schreibberechtigungen verfügen.

### <a name="subscription-level-disable"></a>Deaktivieren auf Abonnementebene
Die serielle Konsole kann über den [REST-API-Aufruf zum Deaktivieren der Konsole](/rest/api/serialconsole/console/disableconsole) für ein gesamtes Abonnement deaktiviert werden. Sie können die auf der Seite der API-Dokumentation verfügbare Funktion **Jetzt testen** verwenden, um die serielle Konsole für ein Abonnement zu deaktivieren und zu aktivieren. Geben Sie unter **subscriptionId** Ihre Abonnement-ID ein, geben Sie unter **default** **default** ein, und wählen Sie dann **Ausführen** aus. Azure CLI-Befehle sind noch nicht verfügbar.

![REST-API testen](./media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Alternativ können Sie den folgenden Satz von Bash-Befehlen in Cloud Shell verwenden, um die serielle Konsole für ein Abonnement zu deaktivieren, zu aktivieren oder den Deaktivierungsstatus der seriellen Konsole anzuzeigen: 

* So rufen Sie den Deaktivierungsstatus der seriellen Konsole für ein Abonnement ab:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* So deaktivieren Sie die serielle Konsole für ein Abonnement:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* So aktivieren Sie die serielle Konsole für ein Abonnement:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

### <a name="vm-level-disable"></a>Deaktivieren auf VM-Ebene
Die serielle Konsole kann für bestimmte virtuelle Computer deaktiviert werden, indem die Startdiagnoseeinstellung des jeweiligen virtuellen Computers deaktiviert wird. Deaktivieren Sie die Startdiagnose im Azure-Portal, um die serielle Konsole für die VM zu deaktivieren.

## <a name="serial-console-security"></a>Sicherheit der seriellen Konsole 

### <a name="access-security"></a>Zugriffssicherheit 
Der Zugriff auf die serielle Konsole ist auf Benutzer eingeschränkt, die über eine Zugriffsrolle als [Mitwirkender für virtuelle Computer](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) oder höher für den virtuellen Computer verfügen. Wenn für Ihren Azure Active Directory-Mandanten mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) erforderlich ist, ist MFA auch für den Zugriff auf die serielle Konsole erforderlich, weil der Zugriff auf die serielle Konsole über das [Azure-Portal](https://portal.azure.com) erfolgt.

### <a name="channel-security"></a>Kanalsicherheit
Alle gesendeten Daten werden bei der Übertragung verschlüsselt.

### <a name="audit-logs"></a>Überwachungsprotokolle
Alle Zugriffe auf die serielle Konsole werden zurzeit in den Protokollen [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) des virtuellen Computers protokolliert. Der Zugriff auf diese Protokolle wird durch den Administrator des virtuellen Azure-Computers (der auch Besitzer dieser Protokolle ist) gesteuert.  

>[!CAUTION] 
Zugriffskennwörter für die Konsole werden nicht protokolliert. Wenn jedoch innerhalb der Konsole Befehle ausgeführt werden, die Kennwörter, Geheimnisse, Benutzernamen oder personenbezogene Informationen anderer Art enthalten oder ausgeben, werden diese in den Protokollen der VM-Startdiagnose erfasst. Diese werden gemeinsam mit dem gesamten anderen sichtbaren Text als Teil der Implementierung der Scrollbackfunktion der seriellen Konsole geschrieben. Diese Protokolle sind zirkulär, und nur Personen mit Leseberechtigungen für das Speicherkonto der Diagnose haben auf sie Zugriff. Allerdings empfehlen wir die bewährte Methode, für alles, das Geheimnisse und oder personenbezogene Informationen beinhalten kann, den Remotedesktop zu verwenden. 

### <a name="concurrent-usage"></a>Parallele Verwendung
Wenn ein Benutzer mit der seriellen Konsole verbunden ist und ein anderer Benutzer erfolgreich den Zugriff auf denselben virtuellen Computer anfordert, wird der erste Benutzer getrennt und der zweite Benutzer mit der gleichen Sitzung verbunden.

>[!CAUTION] 
Dies bedeutet, dass ein Benutzer, der getrennt wird, nicht abgemeldet wird. Die Möglichkeit, bei der Trennung eine Abmeldung zu erzwingen (mithilfe von SIGHUP oder einem ähnlichen Mechanismus), ist noch nicht implementiert. Für Windows ist in SAC (Special Administrative Console, spezielle Verwaltungskonsole) ein automatisches Timeout aktiviert. Für Linux können Sie die Timeouteinstellung für das Terminal konfigurieren. Fügen Sie zu diesem Zweck Ihrer *.bash_profile*- oder *.profile*-Datei für den Benutzer, den Sie für die Anmeldung bei der Konsole verwenden, `export TMOUT=600` hinzu. Durch diese Einstellung erfolgt nach 10 Minuten ein Timeout der Sitzung.

## <a name="accessibility"></a>Barrierefreiheit
Die Barrierefreiheit ist ein wichtiger Aspekt bei der seriellen Konsole in Azure. Daher haben wir sichergestellt, dass die serielle Konsole vollständig barrierefrei ist.

### <a name="keyboard-navigation"></a>Tastaturnavigation
Verwenden Sie die **TAB**-Taste auf der Tastatur, um im Azure-Portal in der seriellen Konsolenschnittstelle zu navigieren. Auf dem Bildschirm wird hervorgehoben, wo Sie sich gerade befinden. Um den Fokus vom seriellen Konsolenbereich zu entfernen, drücken Sie **STRG**+**F6** auf der Tastatur.

### <a name="use-the-serial-console-with-a-screen-reader"></a>Verwenden der seriellen Konsole mit einer Sprachausgabe
Die serielle Konsole bietet integrierte Unterstützung für die Sprachausgabe. Beim Navigieren mit aktivierter Sprachausgabe kann der Alternativtext für die aktuell ausgewählte Schaltfläche von der Sprachausgabe vorgelesen werden.

## <a name="errors"></a>Errors
Da die meisten Fehler vorübergehend sind, können sie oftmals durch Wiederherstellen der Verbindung behoben werden. Die folgende Tabelle zeigt eine Liste von Fehlern und deren Behebung.

Error                            |   Lösung 
:---------------------------------|:--------------------------------------------|
Die Startdiagnoseeinstellungen für *&lt;VMNAME&gt;* können nicht abgerufen werden. Damit Sie die serielle Konsole verwenden können, stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist. | Stellen Sie sicher, dass für den virtuellen Computer [Startdiagnose](boot-diagnostics.md) aktiviert ist. 
Der virtuelle Computer befindet sich in einem beendeten Zustand mit aufgehobener Zuordnung. Starten Sie den virtuellen Computer neu, und wiederholen Sie die Verbindung mit der seriellen Konsole. | Die VM muss sich im gestarteten Zustand befinden, um auf die serielle Konsole zugreifen zu können.
Sie verfügen nicht über die erforderlichen Berechtigungen, um diese VM mit der seriellen Konsole zu verwenden. Achten Sie darauf, dass Sie mindestens über die Berechtigungen der Rolle „Mitwirkender für virtuelle Computer“ verfügen.| Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Weitere Informationen finden Sie unter [Voraussetzungen](#prerequisites).
Die Ressourcengruppe für das Startdiagnose-Speicherkonto *&lt;STORAGEACCOUNTNAME&gt;* kann nicht ermittelt werden. Stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist und Sie über Zugriff auf dieses Speicherkonto verfügen. | Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Weitere Informationen finden Sie unter [Voraussetzungen](#prerequisites).
Das Websocket ist geschlossen oder konnte nicht geöffnet werden. | Möglicherweise müssen Sie der Whitelist den Eintrag `*.console.azure.com` hinzufügen. Ein detaillierterer, aber längerer Ansatz besteht darin, der Whitelist die [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) hinzuzufügen, die sich jedoch relativ regelmäßig ändern.
Beim Zugriff auf das Speicherkonto für die Startdiagnose dieses virtuellen Computers wurde eine „Forbidden“-Antwort empfangen. | Stellen Sie sicher, dass die Startdiagnose nicht über eine Kontofirewall verfügt. Damit die serielle Konsole funktioniert, ist Zugriff auf ein Startdiagnose-Speicherkonto erforderlich.

## <a name="known-issues"></a>Bekannte Probleme 
Uns sind einige Probleme mit der seriellen Konsole bekannt. Hier finden Sie eine Liste dieser Probleme und Schritte zur Lösung.

Problem                           |   Lösung 
:---------------------------------|:--------------------------------------------|
Das Drücken der **EINGABETASTE** nach dem Verbindungsbanner führt nicht zur Anzeige einer Anmeldeaufforderung. | Weitere Informationen finden Sie unter [Hitting enter does nothing](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md) (Das Drücken der Eingabetaste bewirkt nichts). Dieses Problem kann auftreten, wenn Sie einen benutzerdefinierten virtuellen Computer, eine Appliance mit verstärkter Sicherheit oder eine GRUB-Konfiguration ausführen, und Linux aufgrund dessen keine ordnungsgemäße Verbindung mit dem seriellen Port herstellen kann.
Der Text in der seriellen Konsole nimmt nur einen Teil der Größe des Bildschirms in Anspruch (häufig nach Verwendung eines Text-Editors). | Serielle Konsolen unterstützen keine Aushandlung der Fenstergröße ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)). Daher wird kein SIGWINCH-Signal gesendet, um die Bildschirmgröße zu aktualisieren, und der virtuelle Computer erhält keine Informationen über die Größe Ihres Terminals. Installieren Sie xterm oder ein ähnliches Hilfsprogramm, das über einen `resize`-Befehl verfügt, und führen Sie dann `resize` aus.
Das Einfügen von langen Zeichenfolgen funktioniert nicht. | Die serielle Konsole begrenzt die Länge der Zeichenfolgen, die in das Terminal eingefügt werden können, auf 2048 Zeichen, um die Bandbreite am seriellen Port nicht zu überlasten.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

**F: Wie kann ich Feedback senden?**

A. Geben Sie Feedback, indem Sie ein GitHub-Problem unter https://aka.ms/serialconsolefeedback erstellen. Alternativ dazu können Sie Feedback über azserialhelp@microsoft.com oder in der Kategorie „Virtuelle Computer“ von http://feedback.azure.com senden (dies sind die weniger bevorzugten Methoden).

**F: Unterstützt die serielle Konsole das Kopieren und Einfügen?**

A. Ja. Verwenden Sie **STRG**+**UMSCHALT**+**C** und **STRG**+**UMSCHALT**+**V** zum Kopieren und Einfügen in das Terminal.

**F: Kann ich die serielle Konsole anstelle einer SSH-Verbindung verwenden?**

A. Diese Nutzung mag technisch möglich sein, doch ist die serielle Konsole in erster Linie als ein Tool zur Problembehandlung in Situationen gedacht, in denen keine Verbindung über SSH möglich ist. Aus den folgenden Gründen raten wir davon ab, die serielle Konsole als SSH-Ersatz zu verwenden:

- Die serielle Konsole hat nicht so viel Bandbreite wie SSH. Da es sich um eine reine Textverbindung handelt, sind eher GUI-lastige Interaktionen schwierig.
- Der Zugriff auf die serielle Konsole ist derzeit nur über Benutzername und Kennwort möglich. SSH-Schlüssel sind weitaus sicherer als Benutzername/Kennwort-Kombinationen. Daher ist aus Gründen der Anmeldesicherheit SSH der seriellen Konsole vorzuziehen.

**F: Wer kann die serielle Konsole für mein Abonnement aktivieren oder deaktivieren?**

A. Um die serielle Konsole auf Abonnementebene aktivieren oder deaktivieren zu können, müssen Sie über Schreibberechtigungen für das Abonnement verfügen. Die Rollen „Administrator“ und „Besitzer“ verfügen beispielsweise über Schreibberechtigungen. Benutzerdefinierte Rollen können ebenfalls über Schreibberechtigungen verfügen.

**F: Wer kann auf die serielle Konsole für meinen virtuellen Computer zugreifen?**

A. Sie müssen mindestens der Rolle „Mitwirkender für virtuelle Computer“ einer VM angehören, um auf die serielle Konsole der VM zuzugreifen. 

**F: Meine serielle Konsole zeigt nichts an, was muss ich tun?**

A. Ihr Image ist wahrscheinlich nicht richtig für den Zugriff auf die serielle Konsole konfiguriert. Informationen zum Konfigurieren Ihres Images für die Aktivierung der seriellen Konsole finden Sie unter [Verfügbarkeit der seriellen Konsole in Linux-Distributionen](#serial-console-linux-distribution-availability).

**F: Ist die serielle Konsole für VM-Skalierungsgruppen verfügbar?**

A. Derzeit wird der Zugriff auf die serielle Konsole für Instanzen von VM-Skalierungsgruppen nicht unterstützt.

**F: Ich habe meinen virtuellen Computer nur mit SSH-Schlüsselauthentifizierung eingerichtet. Kann ich trotzdem die serielle Konsole verwenden, um eine Verbindung mit meinem virtuellen Computer herzustellen?**

A. Ja. Da die serielle Konsole keine SSH-Schlüssel erfordert, brauchen Sie nur eine Kombination aus Benutzername und Kennwort einzurichten. Dazu können Sie im Azure-Portal **Kennwort zurücksetzen** auswählen und diese Anmeldeinformationen für die Anmeldung bei der seriellen Konsole verwenden.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden der seriellen Konsole zum [Zugreifen auf GRUB und den Einzelbenutzermodus](serial-console-grub-single-user-mode.md).
* Verwenden der seriellen Konsole für [NMI- und SysRq-Aufrufe](serial-console-nmi-sysrq.md).
* Erfahren Sie, wie Sie die serielle Konsole zum [Aktivieren von GRUB in verschiedenen Distributionen](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/) verwenden.
* Die serielle Konsole ist auch für [virtuelle Windows-Computer](../windows/serial-console.md) verfügbar.
* Erfahren Sie mehr zur [Startdiagnose](boot-diagnostics.md).

