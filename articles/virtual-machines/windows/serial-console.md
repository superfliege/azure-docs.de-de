---
title: Serielle Konsole für virtuelle Azure-Computer | Microsoft Docs
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
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 9c16114b4f8d335dc750b1beef1fb6204ae20e0f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-machine-serial-console-preview"></a>Serielle Konsole für virtuelle Azure-Computer (Vorschau) 


Die serielle Konsole für virtuelle Computer in Azure bietet Zugriff auf eine textbasierte Konsole für virtuelle Linux- und Windows-Computer. Diese serielle Verbindung erfolgt mit dem seriellen COM1-Port des virtuellen Computers und ermöglicht den Zugriff auf den virtuellen Computer. Sie steht nicht in Beziehung zum Zustand des Netzwerks/Betriebssystems des virtuellen Computers. Der Zugriff auf die serielle Konsole für einen virtuellen Computer kann zurzeit nur über das Azure-Portal erfolgen und ist nur für Benutzer mit der Zugriffsberechtigung „VM-Mitwirkender“ oder höher für den virtuellen Computer möglich. 

> [!Note] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Ergänzende Microsoft Azure-Nutzungsbedingungen für Microsoft Azure-Vorschauen]. (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Zurzeit befindet sich dieser Dienst in der **öffentlichen Vorschau**, und der Zugriff auf die serielle Konsole für virtuelle Computer ist für globale Azure-Regionen verfügbar. Zu diesem Zeitpunkt ist die serielle Konsole in der Azure Government-, Azure Deutschland- und Azure China-Cloud nicht verfügbar.

 

## <a name="prerequisites"></a>Voraussetzungen 

* Für den virtuellen Computer MUSS die [Startdiagnose](boot-diagnostics.md) aktiviert sein. 
* Das Konto, das die serielle Konsole verwendet, muss die Rolle [Mitwirkender](../../active-directory/role-based-access-built-in-roles.md) für den virtuellen Computer und das Speicherkonto [Startdiagnose](boot-diagnostics.md) aufweisen. 

## <a name="open-the-serial-console"></a>Öffnen der seriellen Konsole
Auf die serielle Konsole für virtuelle Computer kann nur über das [Azure-Portal](https://portal.azure.com) zugegriffen werden. Im Folgenden werden die Schritte für den Zugriff auf die serielle Konsole für virtuelle Computer über das Portal beschrieben. 

  1. Öffnen Sie das Azure-Portal.
  2. Wählen Sie im Menü auf der linken Seite die Option „Virtuelle Computer“ aus.
  3. Klicken Sie in der Liste auf den virtuellen Computer. Die Übersichtsseite für den virtuellen Computer wird geöffnet.
  4. Scrollen Sie nach unten zum Abschnitt „Support und Problembehandlung“, und klicken Sie auf die Option „Serielle Konsole (Vorschau)“. Ein neuer Bereich mit der seriellen Konsole wird geöffnet, und die Verbindung wird hergestellt.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

### <a name="disable-feature"></a>Deaktivieren des Features
Die Funktionalität der seriellen Konsole kann für bestimmte virtuelle Computer deaktiviert werden, indem die Startdiagnoseeinstellung dieses virtuellen Computers deaktiviert wird.

## <a name="serial-console-security"></a>Sicherheit der seriellen Konsole 

### <a name="access-security"></a>Zugriffssicherheit 
Der Zugriff auf die serielle Konsole ist auf Benutzer eingeschränkt, die über die Zugriffsberechtigung [VM-Mitwirkende](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) oder höher für den virtuellen Computer verfügen. Wenn Ihr AAD-Mandant mehrstufige Authentifizierung erfordert, ist auch für den Zugriff auf die serielle Konsole mehrstufige Authentifizierung erforderlich, da der Zugriff über das [Azure-Portal](https://portal.azure.com) erfolgt.

### <a name="channel-security"></a>Kanalsicherheit
Alle gesendeten Daten werden bei der Übertragung verschlüsselt.

### <a name="audit-logs"></a>Überwachungsprotokolle
Alle Zugriffe auf die serielle Konsole werden zurzeit in den Protokollen [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) des virtuellen Computers protokolliert. Der Zugriff auf diese Protokolle wird durch den Administrator des virtuellen Azure-Computers (der auch Besitzer dieser Protokolle ist) gesteuert.  

>[!CAUTION] 
Es werden keine Zugriffskennwörter für die Konsole protokolliert. Wenn jedoch Befehle, die innerhalb der Konsole ausgeführt werden, Kennwörter, Geheimnisse, Benutzernamen oder eine andere Form von persönlich identifizierbaren Informationen (PII) enthalten oder ausgeben, werden diese zusammen mit dem anderen sichtbaren Texten als Teil der Implementierung der Scrollbackfunktionalität der seriellen Konsole in die Startdiagnoseprotokolle des virtuellen Computers geschrieben. Diese Protokolle sind zirkulär, und nur Personen mit Leseberechtigungen für das Diagnosespeicherkonto besitzen Zugriff auf sie. Es wird jedoch empfohlen, bewährte Methoden bei der Verwendung des Remotedesktops für alle Aspekte zu befolgen, die Geheimnisse und/oder PII beinhalten können. 

### <a name="concurrent-usage"></a>Parallele Verwendung
Wenn ein Benutzer mit der seriellen Konsole verbunden ist und ein anderer Benutzer erfolgreich den Zugriff auf denselben virtuellen Computer anfordert, wird der erste Benutzer getrennt und der zweite Benutzer in einer Art und Weise verbunden, als würde der erste Benutzer aufstehen und die physische Konsole verlassen und ein neuer Benutzer dessen Platz einnehmen.

>[!CAUTION] 
Dies bedeutet, dass der Benutzer, der getrennt wird, nicht abgemeldet wird! Die Möglichkeit, eine Abmeldung beim Trennen der Verbindung (über SIGHUP oder einen ähnlichen Mechanismus) zu erzwingen, ist noch in der Planung begriffen. Für Windows ist ein automatisches Timeout in SAC aktiviert, für Linux können Sie jedoch die Terminaltimeouteinstellung konfigurieren. 


## <a name="accessing-serial-console-for-windows"></a>Zugreifen auf die serielle Konsole für Windows 
Für neuere Windows Server-Images in Azure ist [Spezielle Verwaltungskonsole](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (Special Administrative Console, SAC) standardmäßig aktiviert. SAC wird unter Serverversionen von Windows unterstützt, ist aber unter Clientversionen (z.B. Windows 10, Windows 8 oder Windows 7) nicht verfügbar. Führen Sie zum Aktivieren der seriellen Konsole für virtuelle Windows-Computer, die mit Feb2018-Images oder früher erstellt wurden, die folgenden Schritte aus: 

1. Verbinden Sie sich über Remotedesktop mit Ihrem virtuellen Windows-Computer.
2. Führen Sie an einer Administratoreingabeaufforderung die folgenden Befehle aus: 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Starten Sie das System neu, damit die SAC-Konsole aktiviert wird.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Wenn erforderliche, kann SAC auch offline aktiviert werden. 

1. Fügen Sie den gewünschten Windows-Datenträger an, für den SAC als Datenträger für den vorhandenen virtuellen Computer konfiguriert werden soll. 
2. Führen Sie an einer Administratoreingabeaufforderung die folgenden Befehle aus: 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>Wie kann ermittelt werden, ob SAC aktiviert ist? 

Wenn [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) nicht aktiviert ist, zeigt die serielle Konsole die SAC-Eingabeaufforderung nicht an. Sie kann in einigen Fällen Integritätsinformationen zum virtuellen Computers anzeigen oder leer sein.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Aktivieren der Anzeige des Startmenüs in der seriellen Konsole 

Wenn Sie Windows-Bootloader-Eingabeaufforderungen in der seriellen Konsole aktivieren müssen, können Sie die folgenden zusätzlichen Optionen zum Windows-Bootloader hinzufügen.

1. Verbinden Sie sich über Remotedesktop mit Ihrem virtuellen Windows-Computer.
2. Führen Sie an einer Administratoreingabeaufforderung die folgenden Befehle aus: 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Starten Sie das System neu, damit das Startmenü aktiviert wird.

> [!NOTE] 
> An diesem Punkt ist Unterstützung für Funktionstasten nicht aktiviert. Wenn Sie erweiterte Startoptionen benötigen, verwenden Sie bcdedit /set {current} onetimeadvancedoptions on. Weitere Details finden Sie unter [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set).

## <a name="common-scenarios-for-accessing-windows-serial-console"></a>Gängige Szenarien für den Zugriff auf die serielle Windows-Konsole 
Szenario          | Aktionen in der seriellen Konsole                
:------------------|:----------------------------------------
Falsche Firewallregeln | Zugriff auf die serielle Konsole, um iptables oder Windows-Firewallregeln zu korrigieren 
Dateisystembeschädigung/-überprüfung | Zugriff auf die serielle Konsole und Wiederherstellen des Dateisystems nach der Anmeldung bei SAC CMD
RDP-Konfigurationsprobleme | Greifen Sie auf die serielle Konsole zu, und melden Sie sich am CMD-Kanal an. Überprüfen Sie die Integrität der Terminaldienste, und führen Sie bei Bedarf einen Neustart aus.
Netzwerksperrsystem| Greifen Sie auf die serielle Konsole zu, und melden Sie sich am CMD-Kanal an. Überprüfen Sie den Firewallstatus über die Befehlszeile [netsh](https://docs.microsoft.com/windows-server/networking/technologies/netsh/netsh-contexts). 

## <a name="errors"></a>Fehler
Die meisten Fehler sind vorübergehender Natur und können durch einen Wiederholungsversuch der Verbindung behoben werden. Die Tabelle unten zeigt eine Liste von Fehlern und deren Behebung. 

Fehler                            |   Lösung 
:---------------------------------|:--------------------------------------------|
Startdiagnoseeinstellungen für „<VMNAME>“ können nicht abgerufen werden. Damit Sie die serielle Konsole verwenden können, stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist. | Stellen Sie sicher, dass für den virtuellen Computer [Startdiagnose](boot-diagnostics.md) aktiviert ist. 
Der virtuelle Computer befindet sich in einem beendeten Zustand mit aufgehobener Zuordnung. Starten Sie den virtuellen Computer neu, und wiederholen Sie die Verbindung mit der seriellen Konsole. | Der virtuelle Computer muss den Zustand „Gestartet“ aufweisen, um auf die serielle Konsole zugreifen zu können.
Sie verfügen nicht über die erforderlichen Berechtigungen zum Verwenden der seriellen Konsole dieses virtuellen Computers. Stellen Sie sicher, dass Sie mindestens über Berechtigungen der Rolle „VM-Mitwirkender“ verfügen.| Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).
Die Ressourcengruppe für das Startdiagnose-Speicherkonto „<STORAGEACCOUNTNAME>“ kann nicht ermittelt werden. Stellen Sie sicher, dass die Startdiagnose für diesen virtuellen Computer aktiviert ist und Sie über Zugriff auf dieses Speicherkonto verfügen. | Für den Zugriff auf die serielle Konsole sind bestimmte Berechtigungen erforderlich. Details finden Sie unter [Zugriffsanforderungen](#prerequisites).

## <a name="known-issues"></a>Bekannte Probleme 
Da wir uns noch in der Vorschauphase für den Zugriff auf die serielle Konsole befinden, arbeiten wir an einigen bekannten Problemen. Im Folgenden finden Sie die Liste dieser Probleme mit möglichen Problemumgehungen. 

Problem                           |   Lösung 
:---------------------------------|:--------------------------------------------|
Es gibt keine Option der seriellen Konsole für VM-Skalierungsgruppeninstanzen. | Zum Zeitpunkt der Vorschau wird der Zugriff auf die serielle Konsole für VM-Skalierungsgruppeninstanzen nicht unterstützt.
Nach dem Drücken der EINGABETASTE nach dem Verbindungsbanner wird keine Anmeldeeingabeaufforderung angezeigt. | [Nach dem Drücken der EINGABETASTE geschieht nichts.](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Es werden nur Integritätsinformationen angezeigt, wenn eine Verbindung mit einem virtuellen Windows-Computer hergestellt wird.| [Windows-Integritätssignale](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 
**F: Wie kann ich Feedback bereitstellen?**

A: Stellen Sie Feedback als Problem bereit, indem Sie zu https://aka.ms/serialconsolefeedback navigieren. Alternativ können Sie (weniger bevorzugt) Feedback über azserialhelp@microsoft.com oder in der Kategorie „Virtuelle Computer“ von http://feedback.azure.com senden. 

**F: Ich erhalte eine Fehlermeldung „Die vorhandene Konsole verursacht einen Konflikt des Betriebssystemtyps "Windows" mit dem angeforderten Betriebssystemtyp Linux“.**

A: Dies ist ein bekanntes Problem. Um dieses Problem zu beheben, öffnen Sie einfach die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) im bash-Modus, und versuchen Sie es erneut.

**F: Ich bin nicht in der Lage, auf die serielle Konsole zugreifen. Wo kann ich eine Supportanfrage stellen?**

A: Dieses Vorschaufeature wird durch die Nutzungsbedingungen für die Vorschau abgedeckt. Unterstützung erfolgt am besten über die oben genannten Kanäle. 

## <a name="next-steps"></a>Nächste Schritte
* Die serielle Konsole ist auch für [virtuelle Linux-Computer](../linux/serial-console.md) verfügbar.
* Weitere Informationen zur [Startdiagnose](boot-diagnostics.md)
