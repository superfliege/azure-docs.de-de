---
title: "Einschränkungen von Azure Cloud Shell | Microsoft-Dokumentation"
description: "Übersicht über die Einschränkungen von Azure Cloud Shell"
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: juluk
ms.openlocfilehash: 08426b6142dd125a5981d65635ecc55336cb3d15
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="limitations-of-azure-cloud-shell"></a>Einschränkungen von Azure Cloud Shell

Für Azure Cloud Shell gelten die folgenden bekannten Einschränkungen:

## <a name="general-limitations"></a>Allgemeine Einschränkungen

### <a name="system-state-and-persistence"></a>Systemstatus und Persistenz

Der Computer mit Ihrer Cloud Shell-Sitzung besteht nur vorübergehend und wird recycelt, wenn die Sitzung 20 Minuten lang inaktiv ist. Für Cloud Shell muss eine Azure-Dateifreigabe bereitgestellt werden. Daher muss Ihr Abonnement Speicherressourcen für den Zugriff auf Cloud Shell einrichten können. Weitere Überlegungen:

* Mit eingebundenem Speicher werden nur Änderungen im `clouddrive`-Verzeichnis gespeichert. In Bash bleibt Ihr `$Home`-Verzeichnis ebenfalls erhalten.
* Azure-Dateifreigaben können nur innerhalb Ihrer [zugewiesenen Region](persisting-shell-storage.md#mount-a-new-clouddrive) eingebunden werden.
  * Führen Sie in Bash `env` aus, um für Ihre Region die Einstellung `ACC_LOCATION` zu ermitteln.
* Azure Files unterstützt nur Konten mit lokal redundantem Speicher und georedundantem Speicher.

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

### <a name="clouddrive-smb-limited-permissions"></a>Clouddrive SMB – eingeschränkte Berechtigungen
Bestimmte Befehle im Verzeichnis `clouddrive` (beispielsweise `git clone`) verfügen nicht über die erforderlichen Berechtigungen für das Lesen/Schreiben bestimmter Dateien. Wenn dieses Problem auftritt, wiederholen Sie den Vorgang aus Ihrem `$Home` Verzeichnis, für das keine SMB-Einschränkungen gelten.

### <a name="editing-bashrc"></a>Bearbeiten von „.bashrc“

Gehen Sie bei der Bearbeitung von „.bashrc“ vorsichtig vor, da sonst unerwartete Fehler in Cloud Shell auftreten können.

### <a name="bashhistory"></a>.bash_history

Der Verlauf der Bashbefehle ist möglicherweise aufgrund einer Unterbrechung der Cloud Shell-Sitzung oder gleichzeitiger Sitzungen inkonsistent.

## <a name="powershell-limitations"></a>PowerShell-Einschränkungen

### <a name="slow-startup-time"></a>Langsame Startzeit

Die Initialisierung von PowerShell in Azure Cloud Shell (Vorschauversion) kann in der Vorschauphase bis zu 60 Sekunden dauern.

### <a name="no-home-directory-persistence"></a>Keine $Home-Verzeichnispersistenz

Von beliebigen Anwendungen (z.B.: git, vim usw.) in `$Home` geschriebene Daten bleiben zwischen PowerShell-Sitzungen nicht erhalten. Informationen zu einer Problemumgehung [finden Sie hier](troubleshooting.md#powershell-resolutions).

### <a name="default-file-location-when-created-from-azure-drive"></a>Standard-Dateispeicherort beim Erstellen vom Azure-Laufwerk:

Mithilfe von PowerShell-Cmdlets können Benutzer keine Dateien unter dem Azure-Laufwerk erstellen. Wenn Benutzer neue Dateien mit anderen Tools wie vim oder nano erstellen, werden die Dateien standardmäßig im Ordner „C:\Users“ gespeichert. 

### <a name="gui-applications-are-not-supported"></a>GUI-Anwendungen werden nicht unterstützt.

Wenn der Benutzer einen Befehl (etwa `Connect-AzureAD` oder `Login-AzureRMAccount`) ausführt, mit dem ein Windows-Dialogfeld erstellt wird, wird etwa folgende Fehlermeldung angezeigt: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung für Cloud Shell](troubleshooting.md) <br>
[Schnellstart für Bash](quickstart.md) <br>
[Schnellstart für PowerShell](quickstart-powershell.md)
