---
title: Einschränkungen von Azure Cloud Shell | Microsoft-Dokumentation
description: Übersicht über die Einschränkungen von Azure Cloud Shell
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: juluk
ms.openlocfilehash: 135496e17ae884db580922aa31f6824b2e7fd934
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855983"
---
# <a name="limitations-of-azure-cloud-shell"></a>Einschränkungen von Azure Cloud Shell

Für Azure Cloud Shell gelten die folgenden bekannten Einschränkungen:

## <a name="general-limitations"></a>Allgemeine Einschränkungen

### <a name="system-state-and-persistence"></a>Systemstatus und Persistenz

Der Computer mit Ihrer Cloud Shell-Sitzung besteht nur vorübergehend und wird recycelt, wenn die Sitzung 20 Minuten lang inaktiv ist. Für Cloud Shell muss eine Azure-Dateifreigabe bereitgestellt werden. Daher muss Ihr Abonnement Speicherressourcen für den Zugriff auf Cloud Shell einrichten können. Weitere Überlegungen:

* Mit eingebundenem Speicher werden nur Änderungen im `$Home`-Verzeichnis gespeichert.
* Azure-Dateifreigaben können nur innerhalb Ihrer [zugewiesenen Region](persisting-shell-storage.md#mount-a-new-clouddrive) eingebunden werden.
  * Führen Sie in Bash `env` aus, um für Ihre Region die Einstellung `ACC_LOCATION` zu ermitteln.

### <a name="browser-support"></a>Browserunterstützung

Cloud Shell unterstützt die aktuellen Versionen von Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox und Apple Safari. Safari im privaten Modus wird nicht unterstützt.

### <a name="copy-and-paste"></a>Kopieren und Einfügen

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Für einen bestimmten Benutzer kann nur eine Shell aktiv sein.

Benutzer können jeweils nur eine Art von Shell zu einem bestimmten Zeitpunkt starten, entweder **Bash** oder **PowerShell**. Möglicherweise führen Sie jedoch mehrere Instanzen von Bash oder PowerShell gleichzeitig aus. Der Wechsel zwischen Bash oder PowerShell führt dazu, dass Cloud Shell neu gestartet wird, wodurch bestehende Sitzungen beendet werden.

### <a name="usage-limits"></a>Usage limits (Nutzungseinschränkungen)

Cloud Shell ist für interaktive Anwendungsfälle konzipiert. Daher werden lange Sitzungen ohne Interaktion ohne Vorwarnung beendet.

## <a name="bash-limitations"></a>Bash-Einschränkungen

### <a name="user-permissions"></a>Benutzerberechtigungen

Berechtigungen werden als reguläre Benutzer ohne sudo-Zugriff festgelegt. Installationen außerhalb des Verzeichnisses `$Home` werden nicht gespeichert.

### <a name="editing-bashrc"></a>Bearbeiten von „.bashrc“

Gehen Sie bei der Bearbeitung von „.bashrc“ vorsichtig vor, da sonst unerwartete Fehler in Cloud Shell auftreten können.

## <a name="powershell-limitations"></a>PowerShell-Einschränkungen

### <a name="azuread-module-name"></a>`AzureAD`-Modulname

Der `AzureAD`-Modulname lautet derzeit `AzureAD.Standard.Preview`, das Modul bietet die gleiche Funktionalität.

### <a name="sqlserver-module-functionality"></a>`SqlServer`-Modulfunktionalität

Das in Cloud Shell enthaltene `SqlServer`-Modul bietet nur Unterstützung für die Vorabversion von PowerShell Core. Insbesondere ist `Invoke-SqlCmd` noch nicht verfügbar.

### <a name="default-file-location-when-created-from-azure-drive"></a>Standard-Dateispeicherort beim Erstellen vom Azure-Laufwerk:

Mithilfe von PowerShell-Cmdlets können Benutzer keine Dateien unter dem Azure-Laufwerk erstellen. Wenn Benutzer neue Dateien mit anderen Tools wie Vim oder nano erstellen, werden die Dateien standardmäßig in `$HOME` gespeichert. 

### <a name="gui-applications-are-not-supported"></a>GUI-Anwendungen werden nicht unterstützt.

Wenn der Benutzer einen Befehl (etwa `Connect-AzureAD` oder `Connect-AzureRmAccount`) ausführt, mit dem ein Windows-Dialogfeld erstellt wird, wird etwa folgende Fehlermeldung angezeigt: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-crashes-psreadline"></a>Bei TAB-Vervollständigung stürzt PSReadline ab.

Wenn der EditMode des Benutzers in PSReadline auf Emacs festgelegt ist, der Benutzer versucht, alle Möglichkeiten über die TAB-Vervollständigung anzuzeigen und die Größe des Fensters zu klein zum Darstellen aller Möglichkeiten ist, stürzt PSReadline ab.

### <a name="large-gap-after-displaying-progress-bar"></a>Große Lücke nach dem Darstellen der Statusanzeige

Wenn der Benutzer eine Aktion ausführt, mit der eine Statusanzeige dargestellt wird, etwa eine TAB-Vervollständigung für das Laufwerk `Azure:`, ist es möglich, dass der Cursor nicht ordnungsgemäß festgelegt wird und an der vorherigen Stelle der Statusanzeige eine Lücke angezeigt wird.

### <a name="random-characters-appear-inline"></a>In der Eingabe angezeigte zufällige Zeichen

Die Sequenzcodes für die Cursorposition, z.B. `5;13R`, können in der Benutzereingabe angezeigt werden.  Die Zeichen können manuell entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung für Cloud Shell](troubleshooting.md) <br>
[Schnellstart für Bash](quickstart.md) <br>
[Schnellstart für PowerShell](quickstart-powershell.md)
