---
title: Serielle Konsole für virtuelle Azure-Computer | Microsoft-Dokumentation
description: Bidirektionale serielle Konsole für virtuelle Azure-Computer.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: 4e93e455e309771ed3e33382ee49cdc144036fb1
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782412"
---
# <a name="virtual-machine-serial-console-preview"></a>Serielle Konsole für virtuelle Computer (Vorschau) 


Die serielle Konsole für virtuelle Computer in Azure ermöglicht den Zugriff auf eine textbasierte Konsole für virtuelle Windows-Computer. Diese serielle Verbindung erfolgt mit dem seriellen COM1-Port des virtuellen Computers und ermöglicht einen Zugriff auf den virtuellen Computer, der nicht vom Zustand des Netzwerks oder Betriebssystems des virtuellen Computers abhängt. Der Zugriff auf die serielle Konsole für einen virtuellen Computer kann zurzeit nur über das Azure-Portal erfolgen und ist nur für Benutzer mit der Zugriffsberechtigung „VM-Mitwirkender“ oder höher für den virtuellen Computer möglich. 

Die Dokumentation zur seriellen Konsole für virtuelle Linux-Computer finden Sie [hier](../linux/serial-console.md).

> [!Note] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Ergänzende Microsoft Azure-Nutzungsbedingungen für Microsoft Azure-Vorschauen]. (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Zurzeit befindet sich dieser Dienst in der **öffentlichen Vorschau**, und der Zugriff auf die serielle Konsole für virtuelle Computer ist für globale Azure-Regionen verfügbar. Zu diesem Zeitpunkt ist die serielle Konsole in der Azure Government-, Azure Deutschland- und Azure China-Cloud nicht verfügbar.

 

## <a name="prerequisites"></a>Voraussetzungen 

* Sie müssen das Resource Manager-Bereitstellungsmodell verwenden. Klassische Bereitstellungen werden nicht unterstützt. 
* Für den virtuellen Computer MUSS die [Startdiagnose](boot-diagnostics.md) aktiviert sein. 

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Das Konto, das die serielle Konsole verwendet, muss die Rolle [Mitwirkender](../../role-based-access-control/built-in-roles.md) für den virtuellen Computer und das Speicherkonto [Startdiagnose](boot-diagnostics.md) aufweisen. 
* Auf dem virtuellen Computer, für den Sie auf die serielle Konsole zugreifen, muss auch ein kennwortbasiertes Konto vorhanden sein. Mit der Funktionalität [Kennwort zurücksetzen](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) der Erweiterungen für den Zugriff auf virtuelle Computer können Sie eines erstellen – siehe Screenshot unten.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

## <a name="get-started-with-serial-console"></a>Erste Schritte mit der seriellen Konsole
Auf die serielle Konsole für virtuelle Computer kann nur über das [Azure-Portal](https://portal.azure.com) zugegriffen werden. Im Folgenden werden die Schritte für den Zugriff auf die serielle Konsole für virtuelle Computer über das Portal beschrieben.

  1. Öffnen Sie das Azure-Portal.
  2. Wählen Sie im Menü auf der linken Seite die Option „Virtuelle Computer“ aus.
  3. Klicken Sie in der Liste auf den virtuellen Computer. Die Übersichtsseite für den virtuellen Computer wird geöffnet.
  4. Scrollen Sie nach unten zum Abschnitt „Support und Problembehandlung“, und klicken Sie auf die Option „Serielle Konsole (Vorschau)“. Ein neuer Bereich mit der seriellen Konsole wird geöffnet, und die Verbindung wird hergestellt.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

## <a name="enable-serial-console-in-custom-or-older-images"></a>Aktivieren der seriellen Konsole in benutzerdefinierten oder älteren Images
Für neuere Windows Server-Images in Azure ist [Spezielle Verwaltungskonsole](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (Special Administrative Console, SAC) standardmäßig aktiviert. SAC wird unter Serverversionen von Windows unterstützt, ist aber unter Clientversionen (wie Windows 10, Windows 8 oder Windows 7) nicht verfügbar. Gehen Sie zum Aktivieren der seriellen Konsole für virtuelle Windows-Computer, die vor Februar 2018 erstellt wurden, wie folgt vor: 

1. Verbinden Sie sich über Remotedesktop mit Ihrem virtuellen Windows-Computer.
2. Führen Sie an einer Administratoreingabeaufforderung die folgenden Befehle aus. 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Starten Sie das System neu, damit die SAC-Konsole aktiviert wird.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Wenn erforderliche, kann SAC auch offline aktiviert werden. 

1. Fügen Sie den gewünschten Windows-Datenträger an, für den SAC als Datenträger für den vorhandenen virtuellen Computer konfiguriert werden soll. 
2. Führen Sie an einer Administratoreingabeaufforderung die folgenden Befehle aus. 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled"></a>Wie kann ich feststellen, ob SAC aktiviert ist?

Wenn [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) nicht aktiviert ist, zeigt die serielle Konsole die SAC-Eingabeaufforderung nicht an. In einigen Fällen werden Integritätsinformationen zum virtuellen Computer angezeigt, in anderen Fällen ist sie leer.  

## <a name="enable-the-windows-boot-menu-in-serial-console"></a>Aktivieren des Windows-Startmenüs in der seriellen Konsole 

Wenn Sie Windows-Bootloader-Eingabeaufforderungen in der seriellen Konsole aktivieren müssen, können Sie den Startkonfigurationsdaten die folgenden zusätzlichen Optionen hinzufügen. Weitere Details finden Sie unter [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set).

1. Verbinden Sie sich über Remotedesktop mit Ihrem virtuellen Windows-Computer.
2. Führen Sie an einer Administratoreingabeaufforderung die folgenden Befehle aus: 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Starten Sie das System neu, damit das Startmenü aktiviert wird.

## <a name="use-serial-console-for-nmi-calls-in-windows-vms"></a>Verwenden der seriellen Konsole für NMI-Aufrufe in virtuellen Windows-Computern
Ein nicht maskierbarer Interrupt (NMI) dient dazu, ein Signal zu erstellen, das die Software auf einem virtuellen Computer nicht ignoriert. In der Vergangenheit wurden NMIs verwendet, um Hardwareprobleme auf Systemen zu überwachen, die bestimmte Antwortzeiten erforderten.  Heute verwenden Programmierer und Systemadministratoren NMIs häufig als Mechanismus zum Debuggen oder Beheben von Problemen in Systemen, die nicht mehr reagieren.

Sie können mit der seriellen Konsole einen NMI an einen virtuellen Azure-Computer senden. Verwenden Sie dazu das Tastatursymbol in der unten gezeigten Befehlsleiste. Nach der Übermittlung des NMI bestimmt die Konfiguration des virtuellen Computers, wie das System reagiert. Windows kann so konfiguriert werden, dass beim Empfang eines NMI ein Absturz erfolgt und ein Speicherabbild erstellt wird.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Weitere Informationen zum Konfigurieren von Windows zum Erstellen eines Absturzabbilds beim Empfang eines NMI finden Sie unter [How to generate a complete crash dump file or a kernel crash dump file by using an NMI on a Windows-based system](https://support.microsoft.com/en-us/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file) (Erstellen einer vollständigen Absturzabbilddatei oder einer Kernel-Absturzabbilddatei mit einem NMI auf einem Windows-System).

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
Es werden keine Zugriffskennwörter für die Konsole protokolliert. Wenn jedoch Befehle, die innerhalb der Konsole ausgeführt werden, Kennwörter, Geheimnisse, Benutzernamen oder eine andere Form von persönlich identifizierbaren Informationen (PII) enthalten oder ausgeben, werden diese zusammen mit dem anderen sichtbaren Text als Teil der Implementierung der Scrollbackfunktion der seriellen Konsole in die Startdiagnoseprotokolle des virtuellen Computers geschrieben. Diese Protokolle sind zirkulär, und nur Personen mit Leseberechtigungen für das Diagnosespeicherkonto besitzen Zugriff auf sie. Es wird jedoch empfohlen, bewährte Methoden bei der Verwendung des Remotedesktops für alle Aspekte zu befolgen, die Geheimnisse und/oder PII beinhalten können. 

### <a name="concurrent-usage"></a>Parallele Verwendung
Wenn ein Benutzer mit der seriellen Konsole verbunden ist und ein anderer Benutzer erfolgreich den Zugriff auf denselben virtuellen Computer anfordert, wird der erste Benutzer getrennt und der zweite Benutzer in einer Art und Weise verbunden, als würde der erste Benutzer aufstehen und die physische Konsole verlassen und ein neuer Benutzer dessen Platz einnehmen.

>[!CAUTION] 
Dies bedeutet, dass der Benutzer, der getrennt wird, nicht abgemeldet wird! Die Möglichkeit, eine Abmeldung beim Trennen der Verbindung (über SIGHUP oder einen ähnlichen Mechanismus) zu erzwingen, ist noch in der Planung begriffen. Für Windows ist in SAC ein automatisches Timeout aktiviert. Für Linux können Sie dagegen die Timeouteinstellung für das Terminal konfigurieren. 

## <a name="common-scenarios-for-accessing-serial-console"></a>Gängige Szenarien für den Zugriff auf die serielle Konsole 
Szenario          | Aktionen in der seriellen Konsole                
:------------------|:-----------------------------------------
Falsche Firewallregeln | Greifen Sie auf die serielle Konsole zu, und korrigieren Sie die Windows-Firewallregeln. 
Dateisystembeschädigung/-überprüfung | Zugreifen auf die serielle Konsole und Wiederherstellen des Dateisystems 
RDP-Konfigurationsprobleme | Zugreifen auf die serielle Konsole und Ändern von Einstellungen Wechseln Sie zum Einstieg zur [RDP-Dokumentation](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).
Netzwerksperrsystem| Zugreifen auf die serielle Konsole über das Portal zum Verwalten des Systems Einige Netzwerkbefehle finden Sie unter [Serielle Konsole > Dokumentation zu CMD- und PowerShell-Befehlen](./serial-console-cmd-ps-commands.md). 
Interaktion mit Bootloader | Greifen Sie über die serielle Konsole auf BCD zu. Wechseln Sie zum Einstieg zu [Aktivieren der Anzeige des Startmenüs in der seriellen Konsole](#enabling-boot-menu-to-show-in-the-serial-console). 

## <a name="accessibility"></a>Barrierefreiheit
Die Barrierefreiheit ist ein wichtiger Aspekt bei der seriellen Konsole in Azure. Zu diesem Zweck haben wir sichergestellt, dass auch Personen mit Seh- und Hörbehinderung sowie Personen, die möglicherweise keine Maus nutzen können, auf die serielle Konsole zugreifen können.

### <a name="keyboard-navigation"></a>Tastaturnavigation
Verwenden Sie die `tab`-Taste auf der Tastatur, um im Azure-Portal in der seriellen Konsolenschnittstelle zu navigieren. Auf dem Bildschirm wird hervorgehoben, wo Sie sich gerade befinden. Um den Fokus vom seriellen Konsolenbereich zu entfernen, drücken Sie `Ctrl + F6` auf der Tastatur.

### <a name="use-serial-console-with-a-screen-reader"></a>Verwenden der seriellen Konsole mit einer Sprachausgabe
Die serielle Konsole ist mit einer Unterstützung für die Sprachausgabe versehen. Beim Navigieren mit aktivierter Sprachausgabe kann der Alternativtext für die aktuell ausgewählte Schaltfläche von der Sprachausgabe vorgelesen werden.

## <a name="errors"></a>Errors
Die meisten Fehler sind vorübergehender Natur und können durch einen Wiederholungsversuch der Verbindung behoben werden. Die folgende Tabelle zeigt eine Liste von Fehlern und deren Behebung.

Error                            |   Lösung 
:---------------------------------|:--------------------------------------------|
Startdiagnoseeinstellungen für „<VMNAME>“ können nicht abgerufen werden. Damit Sie die serielle Konsole verwenden können, stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist. | Stellen Sie sicher, dass für den virtuellen Computer [Startdiagnose](boot-diagnostics.md) aktiviert ist. 
Der virtuelle Computer befindet sich in einem beendeten Zustand mit aufgehobener Zuordnung. Starten Sie den virtuellen Computer neu, und wiederholen Sie die Verbindung mit der seriellen Konsole. | Der virtuelle Computer muss den Zustand „Gestartet“ aufweisen, um auf die serielle Konsole zugreifen zu können.
Sie verfügen nicht über die erforderlichen Berechtigungen zum Verwenden der seriellen Konsole dieses virtuellen Computers. Stellen Sie sicher, dass Sie mindestens über Berechtigungen der Rolle „VM-Mitwirkender“ verfügen.| Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).
Die Ressourcengruppe für das Startdiagnose-Speicherkonto „<STORAGEACCOUNTNAME>“ kann nicht ermittelt werden. Stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist und Sie über Zugriff auf dieses Speicherkonto verfügen. | Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).
Beim Zugriff auf das Speicherkonto für die Startdiagnose dieses virtuellen Computers wurde eine „Forbidden“-Antwort empfangen. | Stellen Sie sicher, dass die Startdiagnose nicht über eine Kontofirewall verfügt. Damit die serielle Konsole funktioniert, ist Zugriff auf ein Startdiagnose-Speicherkonto erforderlich.
Das Websocket ist geschlossen oder konnte nicht geöffnet werden. | Möglicherweise müssen Sie der Whitelist den Eintrag `*.console.azure.com` hinzufügen. Ein detaillierterer, aber längerer Ansatz besteht darin, der Whitelist die [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/en-us/download/details.aspx?id=41653) hinzuzufügen, die sich jedoch relativ regelmäßig ändern.

## <a name="known-issues"></a>Bekannte Probleme 
Da wir uns noch in der Vorschauphase für den Zugriff auf die serielle Konsole befinden, arbeiten wir an einigen bekannten Problemen. Im Folgenden finden Sie die Liste dieser Probleme mit möglichen Problemumgehungen. 

Problem                             |   Lösung 
:---------------------------------|:--------------------------------------------|
Es gibt keine Option der seriellen Konsole für VM-Skalierungsgruppeninstanzen. | Zum Zeitpunkt der Vorschau wird der Zugriff auf die serielle Konsole für VM-Skalierungsgruppeninstanzen nicht unterstützt.
Nach dem Drücken der EINGABETASTE nach dem Verbindungsbanner wird keine Anmeldeeingabeaufforderung angezeigt. | Informieren Sie sich auf dieser Seite: [Nach dem Drücken der EINGABETASTE geschieht nichts](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Dies kann passieren, wenn Sie einen benutzerdefinierten virtuellen Computer, eine Appliance mit verstärkter Sicherheit oder eine GRUB-Konfiguration ausführen, und Windows aufgrund dessen keine ordnungsgemäße Verbindung mit dem seriellen Port herstellen kann.
Es werden nur Integritätsinformationen angezeigt, wenn eine Verbindung mit einem virtuellen Windows-Computer hergestellt wird.| Dieses Problem tritt auf, wenn die spezielle Verwaltungskonsole (Special Administrative Console, SAC) für Ihr Windows-Image nicht aktiviert wurde. Unter [Zugreifen auf die serielle Konsole für Windows](#access-serial-console-for-windows) finden Sie Anweisungen für das manuelle Aktivieren von SAC auf Ihrem virtuellen Windows Computer. Weitere Details finden Sie unter [Windows-Integritätssignale](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).
Bei aktiviertem Kerneldebugging ist an der SAC-Eingabeaufforderung keine Eingabe möglich. | Stellen Sie eine RDP-Verbindung mit dem virtuellen Computer her, und führen Sie `bcdedit /debug {current} off` an einer Eingabeaufforderung mit erhöhten Rechten aus. Falls Sie keine RDP-Verbindung herstellen können, fügen Sie stattdessen den Betriebssystemdatenträger an einen anderen virtuellen Azure-Computer an, bearbeiten Sie ihn mithilfe von `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, während er als Datenträger angefügt ist, und fügen Sie ihn anschließend wieder an den ursprünglichen virtuellen Computer an.
Das Einfügen in PowerShell in SAC führt zu einem dritten Zeichen, wenn der ursprüngliche Inhalt ein sich wiederholendes Zeichen enthielt. | Um dieses Problem zu umgehen, entfernen Sie das PSReadLine-Modul. `Remove-Module PSReadLine` entfernt das PSReadLine-Modul aus der aktuellen Sitzung.
Einige Tastatureingaben erzeugen seltsame SAC-Ausgaben (z.B. `[A`, `[3~`) | [VT100](https://aka.ms/vtsequences)-Escapesequenzen werden von der SAC-Eingabeaufforderung nicht unterstützt.
Beim Zugriff auf das Speicherkonto für die Startdiagnose dieses virtuellen Computers wurde eine „Forbidden“-Antwort empfangen. | Stellen Sie sicher, dass die Startdiagnose nicht über eine Kontofirewall verfügt. Damit die serielle Konsole funktioniert, ist Zugriff auf ein Startdiagnose-Speicherkonto erforderlich.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 
**F: Wie kann ich Feedback senden?**

A. Stellen Sie Feedback als Problem bereit, indem Sie zu https://aka.ms/serialconsolefeedback navigieren. Alternativ können Sie (weniger bevorzugt) Feedback über azserialhelp@microsoft.com oder in der Kategorie „Virtuelle Computer“ von http://feedback.azure.com senden.

**F: Ich bin nicht in der Lage, auf die serielle Konsole zugreifen. Wo kann ich eine Supportanfrage stellen?**

A. Dieses Vorschaufeature wird durch die Nutzungsbedingungen für die Vorschau abgedeckt. Unterstützung erfolgt am besten über die oben genannten Kanäle. 

## <a name="next-steps"></a>Nächste Schritte
* Eine ausführliche Anleitung zu CMD- und PowerShell-Befehlen, die Sie in der Windows-SAC verwenden können, finden Sie [hier](serial-console-cmd-ps-commands.md).
* Die serielle Konsole ist auch für [virtuelle Linux-Computer](../linux/serial-console.md) verfügbar.
* Erfahren Sie mehr zur [Startdiagnose](boot-diagnostics.md).
