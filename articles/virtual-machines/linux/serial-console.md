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
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: 857998c73abed76c9e20d5b3422ce607fb9f733d
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782880"
---
# <a name="virtual-machine-serial-console-preview"></a>Serielle Konsole für virtuelle Computer (Vorschau) 


Die serielle Konsole für virtuelle Computer in Azure ermöglicht den Zugriff auf eine textbasierte Konsole für virtuelle Linux-Computer. Diese serielle Verbindung erfolgt mit dem seriellen COM1-Port des virtuellen Computers und ermöglicht einen Zugriff auf den virtuellen Computer, der nicht vom Zustand des Netzwerks oder Betriebssystems des virtuellen Computers abhängt. Der Zugriff auf die serielle Konsole für einen virtuellen Computer kann zurzeit nur über das Azure-Portal erfolgen und ist nur für Benutzer mit der Zugriffsberechtigung „VM-Mitwirkender“ oder höher für den virtuellen Computer möglich. 

[Klicken Sie hier](../windows/serial-console.md), um die Dokumentation zur seriellen Konsole für virtuelle Windows-Computer anzuzeigen.

> [!Note] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Ergänzende Microsoft Azure-Nutzungsbedingungen für Microsoft Azure-Vorschauen]. (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Zurzeit befindet sich dieser Dienst in der **öffentlichen Vorschau**, und der Zugriff auf die serielle Konsole für virtuelle Computer ist für globale Azure-Regionen verfügbar. Zu diesem Zeitpunkt ist die serielle Konsole in der Azure Government-, Azure Deutschland- und Azure China-Cloud nicht verfügbar.


## <a name="prerequisites"></a>Voraussetzungen 

* Sie müssen das Resource Manager-Bereitstellungsmodell verwenden. Klassische Bereitstellungen werden nicht unterstützt. 
* Für Ihren virtuellen Computer MUSS die [Startdiagnose](boot-diagnostics.md) aktiviert sein – siehe Screenshot.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Das Azure-Konto, das die serielle Konsole verwendet, muss die Rolle [Mitwirkender](../../role-based-access-control/built-in-roles.md) für den virtuellen Computer und das Speicherkonto [Startdiagnose](boot-diagnostics.md) aufweisen. 
* Auf dem virtuellen Computer, für den Sie auf die serielle Konsole zugreifen, muss auch ein kennwortbasiertes Konto vorhanden sein. Mit der Funktionalität [Kennwort zurücksetzen](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) der Erweiterungen für den Zugriff auf virtuelle Computer können Sie eines erstellen – siehe Screenshot unten.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

* Spezifische Einstellungen für Linux-Distributionen finden Sie unter [Zugreifen auf die serielle Konsole für Linux](#access-serial-console-for-linux).



## <a name="get-started-with-serial-console"></a>Erste Schritte mit der seriellen Konsole
Auf die serielle Konsole für virtuelle Computer kann nur über das [Azure-Portal](https://portal.azure.com) zugegriffen werden. Im Folgenden werden die Schritte für den Zugriff auf die serielle Konsole für virtuelle Computer über das Portal beschrieben. 

  1. Öffnen Sie das Azure-Portal.
  2. Wählen Sie im Menü auf der linken Seite die Option „Virtuelle Computer“ aus.
  3. Klicken Sie in der Liste auf den virtuellen Computer. Die Übersichtsseite für den virtuellen Computer wird geöffnet.
  4. Scrollen Sie nach unten zum Abschnitt „Support und Problembehandlung“, und klicken Sie auf die Option „Serielle Konsole (Vorschau)“. Ein neuer Bereich mit der seriellen Konsole wird geöffnet, und die Verbindung wird hergestellt.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Für die serielle Konsole ist ein konfigurierter lokaler Benutzer mit einem Kennwort erforderlich. Derzeit können nur mit einem öffentlichen SSH-Schlüssel konfigurierte virtuelle Computer nicht auf die serielle Konsole zugreifen. Wenn Sie einen lokalen Benutzer mit Kennwort erstellen möchten, verwenden Sie die [VMAccess-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) (auch im Portal verfügbar, indem Sie auf „Kennwort zurücksetzen“ klicken), und erstellen Sie einen lokalen Benutzer mit einem Kennwort.

## <a name="access-serial-console-for-linux"></a>Zugreifen auf die serielle Konsole für Linux
Damit die serielle Konsole ordnungsgemäß ausgeführt wird, muss das Gastbetriebssystem so konfiguriert werden, dass Konsolenmeldungen über den seriellen Anschluss gelesen und geschrieben werden. In den meisten [von Azure unterstützten Linux-Distributionen](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) ist die serielle Konsole standardmäßig konfiguriert. Sie müssen einfach nur im Azure-Portal auf den Abschnitt „Serielle Konsole“ klicken, um Zugriff auf die Konsole zu erhalten. 

Distribution      | Zugriff auf die serielle Konsole
:-----------|:---------------------
Red Hat Enterprise Linux    | Für in Azure verfügbare Red Hat Enterprise Linux-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. 
CentOS      | Für in Azure verfügbare CentOS-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. 
Ubuntu      | Für in Azure verfügbare Ubuntu-Images ist der Zugriff auf die Konsole standardmäßig aktiviert.
CoreOS      | Für in Azure verfügbare CoreOS-Images ist der Zugriff auf die Konsole standardmäßig aktiviert.
SUSE        | Für in Azure verfügbare neuere SLES-Images ist der Zugriff auf die Konsole standardmäßig aktiviert. Wenn Sie in Azure ältere Versionen von SLES (10 oder niedriger) verwenden, befolgen Sie die Anweisungen in [diesem KB-Artikel](https://www.novell.com/support/kb/doc.php?id=3456486), um die serielle Konsole zu aktivieren. 
Oracle Linux        | Für in Azure verfügbare Oracle Linux-Images ist der Zugriff auf die Konsole standardmäßig aktiviert.
Benutzerdefinierte Linux-Images     | Um die serielle Konsole für Ihr benutzerdefiniertes Linux-VM-Image zu aktivieren, aktivieren Sie den Konsolenzugriff in „/etc/inittab“, um ein Terminal auf ttyS0 auszuführen. Es folgt ein Beispiel, um dies in der Datei „inittab“ hinzuzufügen: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Weitere Informationen zur ordnungsgemäßen Erstellung von benutzerdefinierten Images finden Sie unter [Erstellen und Hochladen einer Linux-VHD in Azure](https://aka.ms/createuploadvhd).

## <a name="common-scenarios-for-accessing-serial-console"></a>Gängige Szenarien für den Zugriff auf die serielle Konsole 
Szenario          | Aktionen in der seriellen Konsole                
:------------------|:-----------------------------------------
Fehlerhafte fstab-Datei | Drücken Sie die EINGABETASTE (`Enter`), um fortzufahren, und korrigieren Sie die FSTAB-Datei in einem Text-Editor. Dazu müssen Sie sich möglicherweise im Einzelbenutzermodus befinden. Informationen für den Einstieg finden Sie unter [Beheben von FSTAB-Fehlern](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) und [Verwenden der serielle Konsole zum Zugreifen auf GRUB und den Einzelbenutzermodus](serial-console-grub-single-user-mode.md).
Falsche Firewallregeln | Greifen Sie auf die serielle Konsole zu, und korrigieren Sie iptables. 
Dateisystembeschädigung/-überprüfung | Zugreifen auf die serielle Konsole und Wiederherstellen des Dateisystems 
SSH/RDP-Konfigurationsprobleme | Zugreifen auf die serielle Konsole und Ändern von Einstellungen 
Netzwerksperrsystem| Zugreifen auf die serielle Konsole über das Portal zum Verwalten des Systems 
Interaktion mit Bootloader | Greifen Sie über die serielle Konsole auf GRUB zu. Informationen für den Einstieg finden Sie unter [Verwenden der serielle Konsole zum Zugreifen auf GRUB und den Einzelbenutzermodus](serial-console-grub-single-user-mode.md). 

## <a name="disable-serial-console"></a>Deaktivieren der seriellen Konsole
Standardmäßig haben alle Abonnements Zugriff auf die serielle Konsole, die für alle virtuellen Computer aktiviert ist. Sie können die serielle Konsole auf Abonnement- oder VM-Ebene deaktivieren.

### <a name="subscription-level-disable"></a>Deaktivieren auf Abonnementebene
Die serielle Konsole kann über den [REST-API-Aufruf zum Deaktivieren der Konsole](https://aka.ms/disableserialconsoleapi) für ein gesamtes Abonnement deaktiviert werden. Sie können die auf der Seite der API-Dokumentation verfügbare Funktion „Jetzt testen“ verwenden, um die serielle Konsole für ein Abonnement zu deaktivieren und zu aktivieren. Geben Sie Ihre `subscriptionId` und im Feld `default` das Wort „default“ ein, und klicken Sie auf „Ausführen“. Azure CLI-Befehle sind noch nicht verfügbar, werden aber zu einem späteren Zeitpunkt hinzugefügt. [Testen Sie den REST-API-Aufruf hier](https://aka.ms/disableserialconsoleapi).

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Alternativ können Sie den Befehlssatz unten in Cloud Shell (es werden Bashbefehle gezeigt) verwenden, um die serielle Konsole für ein Abonnement zu deaktivieren oder zu aktivieren und den Deaktivierungsstatus der seriellen Konsole anzuzeigen. 

* So rufen Sie den Deaktivierungsstatus der seriellen Konsole für ein Abonnement ab
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Deaktivieren der seriellen Konsole für ein Abonnement:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Aktivieren der seriellen Konsole für ein Abonnement:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

### <a name="vm-level-disable"></a>Deaktivieren auf VM-Ebene
Die serielle Konsole kann für bestimmte virtuelle Computer deaktiviert werden, indem die Startdiagnoseeinstellung des jeweiligen virtuellen Computers deaktiviert wird. Deaktivieren Sie einfach die Startdiagnose im Azure-Portal, und die serielle Konsole wird für den virtuellen Computer deaktiviert.

## <a name="serial-console-security"></a>Sicherheit der seriellen Konsole 

### <a name="access-security"></a>Zugriffssicherheit 
Der Zugriff auf die serielle Konsole ist auf Benutzer eingeschränkt, die über die Zugriffsberechtigung [VM-Mitwirkende](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) oder höher für den virtuellen Computer verfügen. Wenn Ihr AAD-Mandant mehrstufige Authentifizierung erfordert, ist auch für den Zugriff auf die serielle Konsole mehrstufige Authentifizierung erforderlich, da der Zugriff über das [Azure-Portal](https://portal.azure.com) erfolgt.

### <a name="channel-security"></a>Kanalsicherheit
Alle gesendeten Daten werden bei der Übertragung verschlüsselt.

### <a name="audit-logs"></a>Überwachungsprotokolle
Alle Zugriffe auf die serielle Konsole werden zurzeit in den Protokollen [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) des virtuellen Computers protokolliert. Der Zugriff auf diese Protokolle wird durch den Administrator des virtuellen Azure-Computers (der auch Besitzer dieser Protokolle ist) gesteuert.  

>[!CAUTION] 
Es werden keine Zugriffskennwörter für die Konsole protokolliert. Wenn jedoch Befehle, die innerhalb der Konsole ausgeführt werden, Kennwörter, Geheimnisse, Benutzernamen oder eine andere Form von persönlich identifizierbaren Informationen (PII) enthalten oder ausgeben, werden diese zusammen mit dem anderen sichtbaren Text als Teil der Implementierung der Scrollbackfunktionalität der seriellen Konsole in die Startdiagnoseprotokolle des virtuellen Computers geschrieben. Diese Protokolle sind zirkulär, und nur Personen mit Leseberechtigungen für das Diagnosespeicherkonto haben Zugriff auf sie. Es wird jedoch empfohlen, bewährte Methoden bei der Verwendung der SSH-Konsole für alle Aspekte zu befolgen, die Geheimnisse und/oder PII beinhalten können. 

### <a name="concurrent-usage"></a>Parallele Verwendung
Wenn ein Benutzer mit der seriellen Konsole verbunden ist und ein anderer Benutzer erfolgreich den Zugriff auf denselben virtuellen Computer anfordert, wird der erste Benutzer getrennt und der zweite Benutzer in einer Art und Weise verbunden, als würde der erste Benutzer aufstehen und die physische Konsole verlassen und ein neuer Benutzer dessen Platz einnehmen.

>[!CAUTION] 
Dies bedeutet, dass der Benutzer, der getrennt wird, nicht abgemeldet wird! Die Möglichkeit, eine Abmeldung beim Trennen der Verbindung (über SIGHUP oder einen ähnlichen Mechanismus) zu erzwingen, ist noch in der Planung begriffen. Für Windows ist ein automatisches Timeout in SAC aktiviert, für Linux können Sie jedoch die Terminaltimeouteinstellung konfigurieren. Fügen Sie dazu einfach `export TMOUT=600` in der Datei „.bash_profile“ oder „.profile“ für den Benutzer ein, mit dem Sie sich an der Konsole anmelden, sodass das Timeout für die Sitzung nach 10 Minuten erfolgt.

## <a name="accessibility"></a>Barrierefreiheit
Die Barrierefreiheit ist ein wichtiger Aspekt bei der seriellen Konsole in Azure. Zu diesem Zweck haben wir sichergestellt, dass auch Personen mit Seh- und Hörbehinderung sowie Personen, die möglicherweise keine Maus nutzen können, auf die serielle Konsole zugreifen können.

### <a name="keyboard-navigation"></a>Tastaturnavigation
Verwenden Sie die `tab`-Taste auf der Tastatur, um im Azure-Portal in der seriellen Konsolenschnittstelle zu navigieren. Auf dem Bildschirm wird hervorgehoben, wo Sie sich gerade befinden. Um den Fokus vom seriellen Konsolenbereich zu entfernen, drücken Sie `Ctrl + F6` auf der Tastatur.

### <a name="use-serial-console-with-a-screen-reader"></a>Verwenden der seriellen Konsole mit einer Sprachausgabe
Die serielle Konsole ist mit einer Unterstützung für die Sprachausgabe versehen. Beim Navigieren mit aktivierter Sprachausgabe kann der Alternativtext für die aktuell ausgewählte Schaltfläche von der Sprachausgabe vorgelesen werden.

## <a name="errors"></a>Errors
Die meisten Fehler sind vorübergehender Natur und können oft durch einen erneuten Versuch der Verbindungsherstellung mit der seriellen Konsole behoben werden. Die folgende Tabelle zeigt eine Liste von Fehlern und deren Behebung.

Error                            |   Lösung 
:---------------------------------|:--------------------------------------------|
Startdiagnoseeinstellungen für „<VMNAME>“ können nicht abgerufen werden. Damit Sie die serielle Konsole verwenden können, stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist. | Stellen Sie sicher, dass für den virtuellen Computer [Startdiagnose](boot-diagnostics.md) aktiviert ist. 
Der virtuelle Computer befindet sich in einem beendeten Zustand mit aufgehobener Zuordnung. Starten Sie den virtuellen Computer neu, und wiederholen Sie die Verbindung mit der seriellen Konsole. | Der virtuelle Computer muss den Zustand „Gestartet“ aufweisen, um auf die serielle Konsole zugreifen zu können.
Sie verfügen nicht über die erforderlichen Berechtigungen zum Verwenden der seriellen Konsole dieses virtuellen Computers. Stellen Sie sicher, dass Sie mindestens über Berechtigungen der Rolle „VM-Mitwirkender“ verfügen.| Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).
Die Ressourcengruppe für das Startdiagnose-Speicherkonto „<STORAGEACCOUNTNAME>“ kann nicht ermittelt werden. Stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist und Sie über Zugriff auf dieses Speicherkonto verfügen. | Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).
Das Websocket ist geschlossen oder konnte nicht geöffnet werden. | Möglicherweise müssen Sie der Whitelist den Eintrag `*.console.azure.com` hinzufügen. Ein detaillierterer, aber längerer Ansatz besteht darin, der Whitelist die [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/en-us/download/details.aspx?id=41653) hinzuzufügen, die sich jedoch relativ regelmäßig ändern.
## <a name="known-issues"></a>Bekannte Probleme 
Da wir uns noch in der Vorschauphase für den Zugriff auf die serielle Konsole befinden, arbeiten wir an einigen bekannten Problemen. Im Folgenden finden Sie die Liste dieser Probleme mit möglichen Problemumgehungen:

Problem                           |   Lösung 
:---------------------------------|:--------------------------------------------|
Es gibt keine Option der seriellen Konsole für VM-Skalierungsgruppeninstanzen. |  Zum Zeitpunkt der Vorschau wird der Zugriff auf die serielle Konsole für VM-Skalierungsgruppeninstanzen nicht unterstützt.
Nach dem Drücken der EINGABETASTE nach dem Verbindungsbanner wird keine Anmeldeeingabeaufforderung angezeigt. | Informieren Sie sich auf dieser Seite: [Nach dem Drücken der EINGABETASTE geschieht nichts](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Dies kann passieren, wenn Sie einen benutzerdefinierten virtuellen Computer, eine Appliance mit verstärkter Sicherheit oder eine GRUB-Konfiguration ausführen, und Linux aufgrund dessen keine ordnungsgemäße Verbindung mit dem seriellen Port herstellen kann.
Beim Zugriff auf das Speicherkonto für die Startdiagnose dieses virtuellen Computers wurde eine „Forbidden“-Antwort empfangen. | Stellen Sie sicher, dass die Startdiagnose nicht über eine Kontofirewall verfügt. Damit die serielle Konsole funktioniert, ist Zugriff auf ein Startdiagnose-Speicherkonto erforderlich.
Der Text in der seriellen Konsole nimmt nur einen Teil der Größe des Bildschirms in Anspruch (häufig nach Verwendung eines Text-Editors) | Dies ist ein bekanntes Problem mit unbekannter Bildschirmgröße bei seriellen Verbindungen. Es empfiehlt sich, xterm oder ein ähnliches Hilfsprogramm zu installieren, das es Ihnen ermöglicht, den Befehl „Größe ändern“ zu verwenden. Durch Ausführen des Befehls „Größe ändern“ wird dieses Problem behoben.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 
**F: Wie kann ich Feedback senden?**

A. Stellen Sie Feedback als Problem bereit, indem Sie zu https://aka.ms/serialconsolefeedback navigieren. Alternativ können Sie (weniger bevorzugt) Feedback über azserialhelp@microsoft.com oder in der Kategorie „Virtuelle Computer“ von http://feedback.azure.com senden.

**F: Ich bin nicht in der Lage, auf die serielle Konsole zugreifen. Wo kann ich eine Supportanfrage stellen?**

A. Dieses Vorschaufeature wird durch die Nutzungsbedingungen für die Vorschau abgedeckt. Unterstützung erfolgt am besten über die oben genannten Kanäle. 

**F: Kann ich die serielle Konsole anstelle einer SSH-Verbindung verwenden?**

A. Dies mag technisch möglich sein, doch ist die serielle Konsole in erster Linie als ein Tool zur Problembehandlung in Situationen gedacht, in denen keine Verbindung über SSH möglich ist. Aus zweierlei Gründen wird abgeraten, die serielle Konsole als SSH-Ersatz zu verwenden:

1. Die serielle Konsole verfügt über weniger Bandbreite als SSH und ist eine Nur-Text-Verbindung. Das erschwert GUI-intensivere Interaktionen in der seriellen Konsole.
1. Der Zugriff auf die serielle Konsole erfolgt derzeit nur über Benutzername und Kennwort. SSH-Schlüssel sind weitaus sicherer als Benutzername/Kennwort-Kombinationen. Daher ist aus Gründen der Sicherheit SSH der seriellen Konsole vorzuziehen.



## <a name="next-steps"></a>Nächste Schritte
* Verwenden der seriellen Konsole zum [Starten in GRUB und zum Wechseln in den Einzelbenutzermodus](serial-console-grub-single-user-mode.md)
* Verwenden der seriellen Konsole für [NMI- und SysRq-Aufrufe](serial-console-nmi-sysrq.md)
* Die serielle Konsole ist auch für [Windows](../windows/serial-console.md)-VMs verfügbar.
* Erfahren Sie mehr über die [Startdiagnose](boot-diagnostics.md).