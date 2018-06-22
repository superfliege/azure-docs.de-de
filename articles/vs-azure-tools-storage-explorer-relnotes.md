---
title: Anmerkungen zu dieser Version vom Microsoft Azure Storage-Explorer
description: Anmerkungen zu dieser Version für den Microsoft Azure Storage-Explorer
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: ''
ms.assetid: ''
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 2878fb737f5daa875b91aefc77c6b8bc495f917e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657569"
---
# <a name="microsoft-azure-storage-explorer-release-notes"></a>Anmerkungen zu dieser Version vom Microsoft Azure Storage-Explorer

Dieser Artikel enthält die Versionshinweise für das Release von Azure Storage-Explorer 1.0.0 sowie die Versionshinweise für frühere Versionen.

Beim [Microsoft Azure Storage-Explorer](./vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine eigenständige App, über die Sie ganz einfach mit Azure Storage-Daten arbeiten können – unter Windows, macOS und Linux.

## <a name="version-110"></a>Version 1.1.0
09.05.2018

### <a name="download-azure-storage-explorer-110"></a>Herunterladen von Azure Storage-Explorer 1.1.0
- [Azure Storage-Explorer 1.1.0 für Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage-Explorer 1.1.0 für Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage-Explorer 1.1.0 für Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Neu
* Storage-Explorer unterstützt jetzt Azurite. Hinweis: Die Verbindung mit Azurite ist mit standardmäßigen Entwicklungsendpunkten hartcodiert.
* Storage-Explorer unterstützt jetzt Zugriffsebenen für Blob Storage-Konten und GPv2-Speicherkonten. [Hier finden Sie weitere Informationen zu Zugriffsebenen.](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-storage-tiers)
* Beim Generieren einer SAS ist keine Startzeit mehr erforderlich.

### <a name="fixes"></a>Fehlerbehebungen
* Beim Abrufen der Abonnements von US-Behördenkonten traten Fehler auf. Dies wurde korrigiert. [#61](https://github.com/Microsoft/AzureStorageExplorer/issues/61)
* Die Ablaufzeit für Zugriffsrichtlinien wurde nicht richtig gespeichert. Dies wurde korrigiert. [#50](https://github.com/Microsoft/AzureStorageExplorer/issues/50)
* Beim Generieren einer SAS-URL für ein Containerelement wurde der Name des Elements der URL nicht angefügt. Dies wurde korrigiert. [#44](https://github.com/Microsoft/AzureStorageExplorer/issues/44)
* Beim Erstellen einer SAS waren vergangene Ablaufzeiten gelegentlich die Standardwerte. Das war darauf zurückzuführen, dass Storage-Explorer die letzte Start- und Ablaufzeit als Standardwert verwendete. Jetzt wird jedes Mal, wenn Sie das SAS-Dialogfeld öffnen, ein neues Set von Standardwerten generiert. [#35](https://github.com/Microsoft/AzureStorageExplorer/issues/35)
* Beim Kopieren zwischen Speicherkonten wird eine 24 Stunden gültige SAS generiert. Nach diesen 24 Stunden schlugen Kopiervorgänge fehl. Jetzt ist die SAS eine Woche gültig. So treten weniger Fehler beim Kopiervorgang wegen einer abgelaufenen SAS auf. [#62](https://github.com/Microsoft/AzureStorageExplorer/issues/62)
* Bei einigen Aktivitäten löste das Klicken auf „Abbrechen“ nicht immer die entsprechende Aktion aus. Dies wurde korrigiert. [#125](https://github.com/Microsoft/AzureStorageExplorer/issues/125)
* Die Übertragungsgeschwindigkeit einiger Aktivitäten war falsch. Dies wurde korrigiert. [#124](https://github.com/Microsoft/AzureStorageExplorer/issues/124)
* In der englischen Version war „Previous“ im Menü „View“ (Ansicht) falsch geschrieben. Der Rechtschreibfehler wurde korrigiert. [#71](https://github.com/Microsoft/AzureStorageExplorer/issues/71)
* Auf der letzten Seite von Windows Installer befand sich die Schaltfläche „Weiter“. Diese Schaltfläche heißt nun „Fertig stellen“. [#70](https://github.com/Microsoft/AzureStorageExplorer/issues/70)
* Beim Verwenden des schwarzen Designs wurden auf der Registerkarte fokussierte Schaltflächen in Dialogfeldern nicht angezeigt. Jetzt sind sie sichtbar.[#64](https://github.com/Microsoft/AzureStorageExplorer/issues/64)
* In der englischen Version war die Groß-/Kleinschreibung von „Auto-Resolve“ (Automatisch auflösen) für Aktionen im Aktivitätsprotokoll falsch. Jetzt ist sie richtig. [#51](https://github.com/Microsoft/AzureStorageExplorer/issues/51)
* Beim Löschen einer Entität aus einer Tabelle, zeigte das Dialogfeld, das zum Bestätigen aufforderte, ein Fehlersymbol an. Das Dialogfeld verwendet jetzt ein Warnsymbol. [#148](https://github.com/Microsoft/AzureStorageExplorer/issues/148)

### <a name="known-issues"></a>Bekannte Probleme
* Wenn Sie VS für Mac verwenden und irgendwann eine benutzerdefinierte AAD-Konfiguration erstellt haben, können Sie sich möglicherweise nicht anmelden. Um das Problem zu umgehen, löschen Sie den Inhalt von ~/.IdentityService/AadConfigurations. Wenn der Fehler dadurch nicht behoben wird, schreiben Sie bitte einen Kommentar zu [diesem Problem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite hat noch nicht alle Speicher-APIs vollständig implementiert. Deswegen können unerwartete Fehler oder ein unerwartetes Verhalten auftreten, wenn Azurite als Entwicklungsspeicher verwendet wird.
* In seltenen Fällen kann der Fokus in der Struktur beim Schnellzugriff hängen bleiben. Klicken Sie auf „Alle aktualisieren“, um den Fokus zu lösen.
* Wegen eines NodeJS-Fehlers können keine Inhalte aus dem OneDrive-Ordner hochgeladen werden. Der Fehler wurde behoben, jedoch noch nicht in Electron integriert.
* Beim Hochladen bestimmter Dateien als Anfügeblobs für Azure Stack tritt möglicherweise ein Fehler auf.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Der Grund hierfür ist, dass wir die [hier](https://github.com/Azure/azure-storage-node/issues/317) beschriebene Problemumgehung für „Filter abbrechen“ verwenden. 
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Die von Storage-Explorer verwendete Electron-Shell hat Probleme mit einigen GPU-Hardwarebeschleunigern (Grafikprozessor). Wenn Storage-Explorer ein leeres Hauptfenster anzeigt, können Sie versuchen, Storage-Explorer über die Befehlszeile zu starten und die GPU-Beschleunigung durch Hinzufügen des Switches `--disable-gpu` zu deaktivieren:

```
./StorageExplorer.exe --disable-gpu
```

* Für Linux-Benutzer müssen Sie [.NET Core 2.0](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x) installieren.
* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="previous-releases"></a>Vorgängerversionen

* [Version 1.0.0](#version-100)
* [Version 0.9.6](#version-096)
* [Version 0.9.5](#version-095)
* [Version 0.9.4 und 0.9.3](#version-094-and-093)
* [Version 0.9.2](#version-092)
* [Version 0.9.1 und 0.9.0](#version-091-and-090)
* [Version 0.8.16](#version-0816)
* [Version 0.8.14](#version-0814)
* [Version 0.8.13](#version-0813)
* [Version 0.8.12, 0.8.11 und 0.8.10](#version-0812-and-0811-and-0810)
* [Version 0.8.9 und 0.8.8](#version-089-and-088)
* [Version 0.8.7](#version-087)
* [Version 0.8.6](#version-086)
* [Version 0.8.5](#version-085)
* [Version 0.8.4](#version-084)
* [Version 0.8.3](#version-083)
* [Version 0.8.2](#version-082)
* [Version 0.8.0](#version-080)
* [Version 0.7.20160509.0](#version-07201605090)
* [Version 0.7.20160325.0](#version-07201603250)
* [Version 0.7.20160129.1](#version-07201601291)
* [Version 0.7.20160105.0](#version-07201601050)
* [Version 0.7.20151116.0](#version-07201511160)

## <a name="version-100"></a>Version 1.0.0
16.04.2018

### <a name="download-azure-storage-explorer-100"></a>Herunterladen von Azure Storage-Explorer 1.0.0
- [Azure Storage-Explorer 1.0.0 für Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage-Explorer 1.0.0 für Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage-Explorer 1.0.0 für Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Neu
* Verbesserte Authentifizierung, durch die Storage-Explorer den gleichen Kontospeicher wie Visual Studio 2017 verwenden kann. Für die Verwendung dieses Features müssen Sie sich erneut bei Ihren Konten anmelden und Ihre gefilterten Abonnements neu festlegen.
* Bei Azure Stack-Konten mit Sicherung durch AAD ruft Storage-Explorer jetzt Azure Stack-Abonnements ab, wenn „Target Azure Stack“ (Ziel-Azure Stack) aktiviert ist. Sie müssen keine benutzerdefinierte Anmeldungsumgebung mehr erstellen.
* Es wurden mehrere Verknüpfungen für eine schnellere Navigation hinzugefügt. Dazu gehören das Umschalten verschiedener Bereiche und das Verschieben zwischen Editoren. Weitere Einzelheiten finden Sie im Menü „Ansicht“.
* Feedback zu Storage-Explorer ist jetzt bei GitHub zu finden. Sie erreichen unsere Seite „Probleme“, indem Sie unten links auf die Schaltfläche „Feedback“ klicken oder [https://github.com/Microsoft/AzureStorageExplorer/issues](https://github.com/Microsoft/AzureStorageExplorer/issues) aufrufen. Sie können gerne Vorschläge machen, Probleme melden, Fragen stellen oder auf andere Weise ein Feedback hinterlassen.
* Wenn Probleme mit Ihrem SSL-Zertifikat auftreten und Sie das betreffende Zertifikat nicht finden können, können Sie Storage-Explorer jetzt mit dem Flag `--ignore-certificate-errors` über die Befehlszeile starten. Wenn Storage-Explorer mit diesem Flag gestartet wird, ignoriert er SSL-Zertifikatfehler.
* Das Kontextmenü enthält jetzt die Option „Herunterladen“ für Blob- und Dateielemente.
* Die Barrierefreiheit und Unterstützung von Bildschirmlesegeräten wurden verbessert. Wenn Sie Barrierefreiheitsfeatures benötigen, finden Sie unter [Dokumentationen zu Barrierefreiheit](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-accessibility) weitere Informationen.
* Storage-Explorer verwendet jetzt Electron 1.8.3.

### <a name="breaking-changes"></a>Wichtige Änderungen
* Storage-Explorer hat zu einer neuen Authentifizierungsbibliothek gewechselt. Im Rahmen des Wechsels zu der Bibliothek müssen Sie sich erneut bei Ihren Konten anmelden und Ihre gefilterten Abonnements neu festlegen.
* Die Methode zum Verschlüsseln vertraulicher Daten wurde geändert. Dadurch müssen möglicherweise einige Ihrer Elemente für den Schnellzugriff erneut hinzugefügt und/oder einige Ihrer angefügten Ressourcen erneut angefügt werden.

### <a name="fixes"></a>Fehlerbehebungen
* Bei einigen Benutzern hinter Proxys wurden Uploads oder Downloads der Gruppe „Blob“ durch folgende Fehlermeldung unterbrochen: „Unable to resolve“ (Auflösung nicht möglich). Dies wurde korrigiert.
* Wenn bei der Verwendung eines direkten Links eine Anmeldung erforderlich war, wurde durch Klicken auf die Anmeldeaufforderung „Anmelden“ in einem Popupfenster ein leeres Dialogfeld geöffnet. Dies wurde korrigiert.
* Wenn Storage-Explorer nicht gestartet werden kann, da ein GPU-Prozess abgebrochen wird, werden Sie jetzt unter Linux darüber informiert und angewiesen, die Option „--disable-gpu“ zu verwenden. Anschließend wird Storage-Explorer mit der aktivierten Option automatisch neu gestartet.
* Ungültige Zugriffsrichtlinien konnten im Dialogfeld „Zugriffsrichtlinien“ nur schwer identifiziert werden. Ungültige Zugriffsrichtlinien-IDs werden jetzt für bessere Sichtbarkeit in Rot hervorgehoben.
* Zwischen den verschiedenen Bereichen einer Aktivität waren im Aktivitätsprotokoll manchmal viele Leerzeichen enthalten. Dies wurde korrigiert.
* Im Abfrage-Editor der Tabelle ist der Editor abgestürzt, wenn Sie eine Zeitstempelklausel in einem ungültigen Status belassen und anschließend versucht haben, eine andere Klausel zu ändern. Der Editor stellt die Zeitstempelklausel jetzt im letzten gültigen Zustand wieder her, wenn in einer anderen Klausel eine Änderung festgestellt wird.
* Wenn Sie während der Eingabe Ihrer Suchabfrage in die Strukturansicht eine Pause gemacht haben, wurde die Suche gestartet und der Fokus vom Textfeld gelenkt. Jetzt müssen Sie die Suche explizit durch Drücken der Eingabetaste oder durch Klicken auf die Schaltfläche „Suche starten“ starten.
* Der Befehl „Get Shared Access Signature“ (Shared Access Signature abrufen) wurde manchmal deaktiviert, wenn mit der rechten Maustaste auf eine Datei in einer Dateifreigabe geklickt wurde. Dies wurde korrigiert.
* Wenn der relevante Ressourcenstrukturknoten während der Suche herausgefiltert wurde, konnten Sie mit der Tabulatortaste nicht in die Ressourcenstruktur gelangen und mit den Pfeiltasten nicht durch die Ressourcenstruktur navigieren. Wenn der relevante Ressourcenstrukturknoten ausgeblendet wird, wird der erste Knoten in der Ressourcenstruktur jetzt automatisch als relevant angezeigt.
* In der Editor-Symbolleiste war gelegentlich ein zusätzliches Trennzeichen sichtbar. Dies wurde korrigiert.
* Das Breadcrumb-Textfeld war gelegentlich überfüllt. Dies wurde korrigiert.
* Der Blob-Editor und der Dateifreigabe-Editor wurden manchmal konstant aktualisiert, wenn viele Dateien gleichzeitig hochgeladen wurden. Dies wurde korrigiert.
* Das Feature „Ordnerstatistik“ hatte in der Ansicht „Verwaltung der Momentaufnahmen von Dateifreigaben“keinen Nutzen. Es wurde jetzt deaktiviert.
* Unter Linux wurde das Menü „Datei“ nicht angezeigt. Dies wurde korrigiert.
* Wenn Sie einen Ordner in eine Dateifreigabe hochgeladen haben, wurden standardmäßig nur die Inhalte des Ordners hochgeladen. Jetzt werden die Inhalte des Ordners standardmäßig in einen geeigneten Ordner in der Dateifreigabe hochgeladen.
* Die Reihenfolge von Schaltflächen wurde in mehreren Dialogfeldern umgekehrt. Dies wurde korrigiert.
* Verschiedene sicherheitsbezogene Fehlerbehebungen.

### <a name="known-issues"></a>Bekannte Probleme
* In seltenen Fällen kann der Fokus in der Struktur beim Schnellzugriff hängen bleiben. Klicken Sie auf „Alle aktualisieren“, um den Fokus zu lösen.
* Beim Hochladen bestimmter Dateien als Anfügeblobs für Azure Stack tritt möglicherweise ein Fehler auf.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Der Grund hierfür ist, dass wir die hier beschriebene Problemumgehung für „Filter abbrechen“ verwenden. 
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Die von Storage-Explorer verwendete Electron-Shell hat Probleme mit einigen GPU-Hardwarebeschleunigern (Grafikprozessor). Wenn Storage-Explorer ein leeres Hauptfenster anzeigt, können Sie versuchen, Storage-Explorer über die Befehlszeile zu starten und die GPU-Beschleunigung durch Hinzufügen des Switches `--disable-gpu` zu deaktivieren:

```
./StorageExplorer.exe --disable-gpu
```

* Für Linux-Benutzer müssen Sie [.NET Core 2.0](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x) installieren.
* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-096"></a>Version 0.9.6
28.02.2018

### <a name="download-azure-storage-explorer-096-preview"></a>Herunterladen des Azure Storage-Explorers 0.9.6 (Vorschauversion)
- [Azure Storage-Explorer 0.9.6 (Vorschauversion) für Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage-Explorer 0.9.6 (Vorschauversion) für Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage-Explorer 0.9.6 (Vorschauversion) für Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="fixes"></a>Fehlerbehebungen
* Ein Problem hat die Auflistung erwarteter Blobs/Dateien im Editor verhindert. Dies wurde korrigiert.
* Ein Problem hat dazu geführt, dass beim Wechseln zwischen Momentaufnahmeansichten Elemente falsch angezeigt wurden. Dies wurde korrigiert.

### <a name="known-issues"></a>Bekannte Probleme
* Storage-Explorer unterstützt keine ADFS-Konten.
* Beim Hochladen bestimmter Dateien als Anfügeblobs für Azure Stack tritt möglicherweise ein Fehler auf.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Der Grund hierfür ist, dass wir die [hier](https://github.com/Azure/azure-storage-node/issues/317) beschriebene Problemumgehung für „Filter abbrechen“ verwenden.
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Im Bereich mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Die von Storage-Explorer verwendete Electron-Shell hat Probleme mit einigen GPU-Hardwarebeschleunigern (Grafikprozessor). Wenn Storage-Explorer ein leeres Hauptfenster anzeigt, können Sie versuchen, Storage-Explorer über die Befehlszeile zu starten und die GPU-Beschleunigung durch Hinzufügen des Switches `--disable-gpu` zu deaktivieren:

```
./StorageExplorer.exe --disable-gpu
```

* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-095"></a>Version 0.9.5
06.02.2018

### <a name="download-azure-storage-explorer-095-preview"></a>Herunterladen des Azure Storage-Explorers 0.9.5 (Vorschauversion)
- [Azure Storage-Explorer 0.9.5 (Vorschauversion) für Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage-Explorer 0.9.5 (Vorschauversion) für Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage-Explorer 0.9.5 (Vorschauversion) für Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Neu

* Unterstützung für Momentaufnahmen von Dateifreigaben:
    * Erstellen und verwalten Sie Momentaufnahmen für Ihre Dateifreigaben.
    * Wechseln Sie beim Durchsuchen problemlos zwischen den Ansichten der Momentaufnahmen Ihrer Dateifreigaben.
    * Stellen Sie frühere Versionen Ihrer Dateien wieder her.
* Unterstützung der Vorschauversion für Azure Data Lake Store:
    * Stellen Sie Verbindungen mit Ihren ADLS-Ressourcen unter mehreren Konten her.
    * Stellen Sie mit ADL-URIs Verbindungen mit ADLS-Ressourcen her, und geben Sie diese frei.
    * Führen Sie grundlegende Vorgänge für Dateien und Ordner rekursiv aus.
    * Heften Sie einzelne Ordner an den Schnellzugriff an.
    * Zeigen Sie Ordnerstatistiken an.

### <a name="fixes"></a>Fehlerbehebungen
* Leistungsverbesserungen beim Starten
* Verschiedene Fehlerbehebungen

### <a name="known-issues"></a>Bekannte Probleme
* Storage-Explorer unterstützt keine ADFS-Konten.
* Beim Hochladen bestimmter Dateien als Anfügeblobs für Azure Stack tritt möglicherweise ein Fehler auf.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Der Grund hierfür ist, dass wir die hier beschriebene Problemumgehung für „Filter abbrechen“ verwenden.
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Im Bereich mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Die von Storage-Explorer verwendete Electron-Shell hat Probleme mit einigen GPU-Hardwarebeschleunigern (Grafikprozessor). Wenn Storage-Explorer ein leeres Hauptfenster anzeigt, können Sie versuchen, Storage-Explorer über die Befehlszeile zu starten und die GPU-Beschleunigung durch Hinzufügen des Switches `--disable-gpu` zu deaktivieren:

```
./StorageExplorer.exe --disable-gpu
```

* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>Version 0.9.4 und 0.9.3
01/21/2018

### <a name="download-azure-storage-explorer-094-preview"></a>Herunterladen von Azure Storage-Explorer 0.9.4 (Vorschauversion)
* [Azure Storage-Explorer 0.9.4 (Vorschauversion) für Windows herunterladen](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Azure Storage-Explorer 0.9.4 (Vorschauversion) für Mac herunterladen](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Azure Storage-Explorer 0.9.4 (Vorschauversion) für Linux herunterladen](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a>Neu
* Das vorhandene Storage-Explorer-Fenster wird in folgenden Fällen wiederverwendet:
    * Sie öffnen Links, die im Storage-Explorer generiert wurden.
    * Sie öffnen den Storage-Explorer vom Portal aus.
    * Sie öffnen den Storage-Explorer über die Azure Storage VS Code-Erweiterung (demnächst verfügbar).
* Es wurde die Möglichkeit hinzugefügt, aus dem Storage-Explorer heraus ein neues Storage-Explorer-Fenster zu öffnen.
    * Bei Windows befindet sich im Menü „Datei“ und im Kontextmenü der Taskleiste eine Option „Neues Fenster“.
    * Bei Mac befindet sich im App-Menü eine Option „Neues Fenster“.

### <a name="fixes"></a>Fehlerbehebungen
* Ein Sicherheitsproblem wurde behoben. Aktualisieren Sie baldmöglichst auf 0.9.4.
* Alte Aktivitäten wurden nicht ordnungsgemäß bereinigt. Dies wirkte sich auf die Leistung von Aufträgen mit langer Ausführungszeit aus. Diese Aktivitäten werden jetzt ordnungsgemäß bereinigt.
* Aktionen mit großen Mengen an Dateien und Verzeichnissen führten gelegentlich zu einem Einfrieren von Storage-Explorer. Anforderungen an Azure für Dateifreigaben werden jetzt gedrosselt, um die Nutzung von Systemressourcen zu begrenzen.

### <a name="known-issues"></a>Bekannte Probleme
* Storage-Explorer unterstützt keine ADFS-Konten.
* Tastenkombinationen für „Explorer anzeigen“ und „Kontenverwaltung anzeigen“ sollten entsprechend STRG/BEFEHL+UMSCHALT+E oder STRG/BEFEHL+UMSCHALT+A sein.
* Beim Hochladen bestimmter Dateien als Anfügeblobs für Azure Stack tritt möglicherweise ein Fehler auf.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Der Grund hierfür ist, dass wir die hier beschriebene Problemumgehung für „Filter abbrechen“ verwenden.
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Im Bereich mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Die von Storage-Explorer verwendete Electron-Shell hat Probleme mit einigen GPU-Hardwarebeschleunigern (Grafikprozessor). Wenn Storage-Explorer ein leeres Hauptfenster anzeigt, können Sie versuchen, Storage-Explorer über die Befehlszeile zu starten und die GPU-Beschleunigung durch Hinzufügen des Switches `--disable-gpu` zu deaktivieren:
```
./StorageExplorer --disable-gpu
```
* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Version 0.9.2
01.11.2017

### <a name="hotfixes"></a>Hotfixes
* Abhängig von der lokalen Zeitzone waren beim Bearbeiten von Edm.DateTime-Werten für Tabellenentitäten unerwartete Datenänderungen möglich. Der Editor verwendet nun ein Nur-Text-Feld, mit dem die Edm.DateTime-Werte präzise und einheitlich gesteuert werden können.
* Der Upload/Download einer Gruppe von Blobs wurde nicht gestartet, wenn ein Name und Schlüssel angefügt war. Dies wurde korrigiert.
* Zuvor wurden Sie von Storage-Explorer nur zur erneuten Authentifizierung eines veralteten Kontos aufgefordert, wenn mindestens ein Abonnement des Kontos ausgewählt wurde. Nun fordert Sie Storage-Explorer auch dazu auf, wenn das Konto vollständig herausgefiltert wurde.
* Die Endpunktdomäne für Azure US Government war falsch. Dies wurde korrigiert.
* Manchmal war es schwierig, im Bereich „Konten verwalten“ auf die Schaltfläche „Anwenden“ zu klicken. Dieses Problem sollte nicht mehr auftreten.

### <a name="new"></a>Neu
* Unterstützung der Vorschau für Azure Cosmos DB:
    * [Onlinedokumentation](./cosmos-db/storage-explorer.md)
    * Erstellen von Datenbanken und Sammlungen
    * Bearbeiten von Daten
    * Abfragen, Erstellen oder Löschen von Dokumenten
    * Aktualisieren von gespeicherten Prozeduren, benutzerdefinierten Funktionen oder Triggern
    * Verwenden von Verbindungszeichenfolgen zum Verbinden und Verwalten Ihrer Datenbanken
* Die Leistung beim Hochladen/Herunterladen zahlreicher kleiner Blobs wurde verbessert.
* Es wurde die Aktion „Alle wiederholen“ hinzugefügt, falls in einer Upload- oder Downloadgruppe für Blobs Fehler auftreten.
* Storage-Explorer hält jetzt die Iteration beim Hochladen/Herunterladen von Blobs an, wenn erkennt wird, dass Ihre Netzwerkverbindung unterbrochen ist. Sie können die Iteration dann fortsetzen, sobald die Verbindung wiederhergestellt wurde.
* Die Optionen „Alle schließen“, „Andere schließen“ und „Schließen“ wurden für Registerkarten zum Kontextmenü hinzugefügt.
* Storage-Explorer verwendet jetzt native Dialogfelder und native Kontextmenüs.
* Storage-Explorer ist jetzt leichter zugänglich. Die Verbesserungen umfassen:
    * Verbesserte Unterstützung für die Sprachausgabe für NVDA unter Windows und für VoiceOver unter Mac
    * Verbesserte Designs mit hohem Kontrast
    * Fehlerbehebungen für TAB-TASTE und Tastaturfokus

### <a name="fixes"></a>Fehlerbehebungen
* Wenn Sie versucht haben, einen Blob mit einem ungültigen Windows-Dateinamen zu öffnen oder herunterzuladen, ist bei dem Vorgang ein Fehler aufgetreten. Storage-Explorer erkennt jetzt, ob ein Blobname ungültig ist und fragt Sie, ob Sie den Blob codieren oder überspringen möchten. Storage-Explorer erkennt zudem, ob ein Dateiname codiert ist und fragt Sie, ob Sie den Dateinamen vor dem Hochladen decodieren möchten.
* Der Editor für den Blobzielcontainer würde beim Hochladen des Blobs in einigen Fällen nicht ordnungsgemäß aktualisiert werden. Dies wurde korrigiert.
* Die Unterstützung für verschiedene Typen von Verbindungszeichenfolgen und SAS-URIs ist rückläufig. Wir haben alle bekannten Probleme behandelt. Senden Sie jedoch weiterhin Feedback, wenn weitere Probleme auftreten.
* Die Updatebenachrichtigung hat für einige Benutzer in Version 0.9.0 nicht funktioniert. Dieses Problem wurde behoben. Die von diesem Fehler betroffenen Benutzer können die neueste Version von Storage-Explorer [hier](https://azure.microsoft.com/features/storage-explorer/) herunterladen.

### <a name="known-issues"></a>Bekannte Probleme
* Storage-Explorer unterstützt keine ADFS-Konten.
* Tastenkombinationen für „Explorer anzeigen“ und „Kontenverwaltung anzeigen“ sollten entsprechend STRG/BEFEHL+UMSCHALT+E oder STRG/BEFEHL+UMSCHALT+A sein.
* Beim Hochladen bestimmter Dateien als Anfügeblobs für Azure Stack tritt möglicherweise ein Fehler auf.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Der Grund hierfür ist, dass wir die hier beschriebene Problemumgehung für „Filter abbrechen“ verwenden.
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Im Bereich mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Die von Storage-Explorer verwendete Electron-Shell hat Probleme mit einigen GPU-Hardwarebeschleunigern (Grafikprozessor). Wenn Storage-Explorer ein leeres Hauptfenster anzeigt, können Sie versuchen, Storage-Explorer über die Befehlszeile zu starten und die GPU-Beschleunigung durch Hinzufügen des Switches `--disable-gpu` zu deaktivieren:
```
./StorageExplorer --disable-gpu
```
* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>Version 0.9.1 und 0.9.0
10/20/2017
### <a name="new"></a>Neu
* Unterstützung der Vorschau für Azure Cosmos DB:
    * [Onlinedokumentation](./cosmos-db/storage-explorer.md)
    * Erstellen von Datenbanken und Sammlungen
    * Bearbeiten von Daten
    * Abfragen, Erstellen oder Löschen von Dokumenten
    * Aktualisieren von gespeicherten Prozeduren, benutzerdefinierten Funktionen oder Triggern
    * Verwenden von Verbindungszeichenfolgen zum Verbinden und Verwalten Ihrer Datenbanken
* Die Leistung beim Hochladen/Herunterladen zahlreicher kleiner Blobs wurde verbessert.
* Es wurde die Aktion „Alle wiederholen“ hinzugefügt, falls in einer Upload- oder Downloadgruppe für Blobs Fehler auftreten.
* Storage-Explorer hält jetzt die Iteration beim Hochladen/Herunterladen von Blobs an, wenn erkennt wird, dass Ihre Netzwerkverbindung unterbrochen ist. Sie können die Iteration dann fortsetzen, sobald die Verbindung wiederhergestellt wurde.
* Die Optionen „Alle schließen“, „Andere schließen“ und „Schließen“ wurden für Registerkarten zum Kontextmenü hinzugefügt.
* Storage-Explorer verwendet jetzt native Dialogfelder und native Kontextmenüs.
* Storage-Explorer ist jetzt leichter zugänglich. Die Verbesserungen umfassen:
    * Verbesserte Unterstützung für die Sprachausgabe für NVDA unter Windows und für VoiceOver unter Mac
    * Verbesserte Designs mit hohem Kontrast
    * Fehlerbehebungen für TAB-TASTE und Tastaturfokus

### <a name="fixes"></a>Fehlerbehebungen
* Wenn Sie versucht haben, einen Blob mit einem ungültigen Windows-Dateinamen zu öffnen oder herunterzuladen, ist bei dem Vorgang ein Fehler aufgetreten. Storage-Explorer erkennt jetzt, ob ein Blobname ungültig ist und fragt Sie, ob Sie den Blob codieren oder überspringen möchten. Storage-Explorer erkennt zudem, ob ein Dateiname codiert ist und fragt Sie, ob Sie den Dateinamen vor dem Hochladen decodieren möchten.
* Der Editor für den Blobzielcontainer würde beim Hochladen des Blobs in einigen Fällen nicht ordnungsgemäß aktualisiert werden. Dies wurde korrigiert.
* Die Unterstützung für verschiedene Typen von Verbindungszeichenfolgen und SAS-URIs ist rückläufig. Wir haben alle bekannten Probleme behandelt. Senden Sie jedoch weiterhin Feedback, wenn weitere Probleme auftreten.
* Die Updatebenachrichtigung hat für einige Benutzer in Version 0.9.0 nicht funktioniert. Dieses Problem wurde behoben. Die von diesem Fehler betroffenen Benutzer können die neueste Version von Storage-Explorer [hier](https://azure.microsoft.com/features/storage-explorer/) herunterladen.

### <a name="known-issues"></a>Bekannte Probleme
* Storage-Explorer unterstützt keine ADFS-Konten.
* Tastenkombinationen für „Explorer anzeigen“ und „Kontenverwaltung anzeigen“ sollten entsprechend STRG/BEFEHL+UMSCHALT+E oder STRG/BEFEHL+UMSCHALT+A sein.
* Beim Hochladen bestimmter Dateien als Anfügeblobs für Azure Stack tritt möglicherweise ein Fehler auf.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Der Grund hierfür ist, dass wir die hier beschriebene Problemumgehung für „Filter abbrechen“ verwenden.
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Im Bereich mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Die von Storage-Explorer verwendete Electron-Shell hat Probleme mit einigen GPU-Hardwarebeschleunigern (Grafikprozessor). Wenn Storage-Explorer ein leeres Hauptfenster anzeigt, können Sie versuchen, Storage-Explorer über die Befehlszeile zu starten und die GPU-Beschleunigung durch Hinzufügen des Switches `--disable-gpu` zu deaktivieren:
```
./StorageExplorer --disable-gpu
```
* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Version 0.8.16
21.08.2017

### <a name="new"></a>Neu
* Beim Öffnen eines Blobs fordert Storage-Explorer Sie zum Hochladen der heruntergeladenen Datei auf, wenn eine Änderung erkannt wird
* Verbesserter Azure Stack-Anmeldevorgang
* Die Leistung beim gleichzeitigen Hochladen/Herunterladen zahlreicher kleiner Dateien wurde verbessert.


### <a name="fixes"></a>Fehlerbehebungen
* Bei einigen Blobtypen führt die Auswahl von „Ersetzen“ während eines Uploadkonflikts manchmal zu einem Neustart des Uploads.
* In Version 0.8.15 wurden Uploads manchmal bei 99 Prozent angehalten.
* Beim Hochladen von Dateien in eine Dateifreigabe trat beim Upload ein Fehler auf, wenn Sie ein noch nicht vorhandenes Verzeichnis ausgewählt haben.
* Der Storage-Explorer hat falsche Zeitstempel für Shared Access Signatures und Tabellenabfragen generiert.


### <a name="known-issues"></a>Bekannte Probleme
* Derzeit kann keine Verbindungszeichenfolge mit Name und Schlüssel verwendet werden. Dies wird im nächsten Release behoben. Bis dahin können Sie die Anfügefunktion mit Name und Schlüssel verwenden.
* Wenn Sie versuchen, eine Datei mit einem ungültigen Windows-Dateinamen zu öffnen, wird für den Download der Fehler „Datei nicht gefunden“ angezeigt.
* Nach dem Klicken auf „Abbrechen“ für eine Aufgabe kann es eine Weile dauern, bis die betreffende Aufgabe abgebrochen wird. Dies ist eine Einschränkung der Azure Storage-Knotenbibliothek.
* Nach dem Abschluss des Blob-Uploads wird die Registerkarte aktualisiert, von der der Upload initiiert wurde. Dies ist eine Änderung gegenüber dem früheren Verhalten; sie bewirkt zudem, dass Sie wieder in das Stammverzeichnis des Containers gelangen, in dem Sie sich befinden.
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird.
* Im Bereich mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Für Benutzer unter Ubuntu 14.04 müssen Sie sicherstellen, dass GCC auf dem neuesten Stand ist. Hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Für Benutzer unter Ubuntu 17.04 müssen Sie GConf installieren; hierzu können Sie die folgenden Befehle ausführen und anschließend Ihren Computer neu starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Version 0.8.14
22.06.2017

### <a name="new"></a>Neu

* Die Electron-Version wurde auf 1.7.2 aktualisiert, sodass verschiedene wichtige Sicherheitsupdates genutzt werden können
* Sie können nun rasch über das Hilfemenü auf den Onlineleitfaden zur Problembehandlung zugreifen
* Storage-Explorer – [Leitfaden zur Problembehandlung][2]
* [Anweisungen][3] zum Herstellen einer Verbindung mit einem Azure Stack-Abonnement

### <a name="known-issues"></a>Bekannte Probleme

* Schaltflächen im Dialogfeld zum Bestätigen des Löschens von Ordnern reagieren nicht auf Mausklicks unter Linux. Die Problemumgehung besteht darin, die Eingabetaste zu verwenden
* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird
* Wenn mehr als drei Gruppen von Blobs oder Dateien gleichzeitig hochgeladen werden, können Fehler auftreten
* Im Panel mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Für die Ubuntu 14.04-Installation muss ein Update oder Upgrade der gcc-Version ausgeführt werden – Schritte zum Ausführen eines Upgrades finden Sie unten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Version 0.8.13
05/12/2017

#### <a name="new"></a>Neu

* Storage-Explorer – [Leitfaden zur Problembehandlung][2]
* [Anweisungen][3] zum Herstellen einer Verbindung mit einem Azure Stack-Abonnement

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Beim Dateiupload trat mit hoher Wahrscheinlichkeit ein Fehler durch ungenügenden Arbeitsspeicher auf
* Behoben: Sie können jetzt sich jetzt per PIN/Smartcard anmelden
* Behoben: „Im Portal öffnen “ funktioniert nun für Azure China, Azure Deutschland, Azure US Government und Azure Stack
* Behoben: Beim Hochladen eines Ordners in einen Blobcontainer trat gelegentlich ein Fehler „Illegal operation“ (Der Vorgang ist nicht gestattet) auf
* Behoben: „Alle auswählen“ war beim Verwalten von Momentaufnahmen deaktiviert
* Behoben: Die Metadaten des Basisblobs wurden u.U. nach Anzeigen der Eigenschaften seiner Momentaufnahmen überschrieben

#### <a name="known-issues"></a>Bekannte Probleme

* Wenn Sie die falsche PIN/das falsche Smartcard-Zertifikat auswählen, müssen Sie einen Neustart ausführen, damit diese Entscheidung im Storage-Explorer unwirksam gemacht wird
* Beim Vergrößern oder Verkleinern der Anzeige wurde die Zoomstufe u.U. vorübergehend auf den Standardwert zurückgesetzt
* Wenn mehr als drei Gruppen von Blobs oder Dateien gleichzeitig hochgeladen werden, können Fehler auftreten
* Im Panel mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Für die Ubuntu 14.04-Installation muss ein Update oder Upgrade der gcc-Version ausgeführt werden – Schritte zum Ausführen eines Upgrades finden Sie unten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Version 0.8.12, 0.8.11 und 0.8.10
07.04.2017

#### <a name="new"></a>Neu

* Storage-Explorer wird nun automatisch geschlossen, wenn Sie ein Update aus der Updatebenachrichtigung installieren
* Der direkte schnelle Zugriff verbessert Ihre Arbeit mit häufig aufgerufenen Ressourcen
* Im Blobcontainer-Editor ist nun ersichtlich, zu welchem virtuellen Computer ein geleaster Blob gehört
* Das linke Panel kann nun reduziert werden
* Die Ermittlung wird nun gleichzeitig mit dem Download ausgeführt
* Bestimmen Sie anhand von Statistiken im Blobcontainer, Dateifreigabe- und Tabellen-Editor die Größe Ihrer Ressource oder Auswahl
* Sie können sich nun bei auf Azure Active Directory (AAD) basierenden Azure Stack-Konten anmelden.
* Sie können nun Archivdateien mit einer Größe von mehr als 32 MB in Storage Premium-Konten hochladen
* Verbesserte Unterstützung für Barrierefreiheit
* Sie können nun vertrauenswürdige Base-64-codierte X.509-SSL-Zertifikate hinzufügen, indem Sie „Bearbeiten“ -&gt; „SSL-Zertifikate“ -&gt; „Zertifikate importieren“ auswählen

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Nach dem Aktualisieren der Anmeldeinformationen eines Kontos wurde die Strukturansicht in einigen Fällen nicht automatisch aktualisiert
* Behoben: Beim Generieren einer SAS für Emulator-Warteschlangen und -Tabellen wurde früher eine ungültige URL erhalten
* Behoben: Storage Premium-Konten können nun erweitert werden, während ein Proxy aktiviert ist
* Behoben: Die Schaltfläche „Anwenden“ auf der Seite zum Verwalten von Konten funktionierte nicht, wenn ein oder kein Konto ausgewählt war
* Behoben: Das Hochladen von Blobs, die Konfliktlösungen benötigen, schlug u.U. fehl – behoben in 0.8.11
* Behoben: Senden von Feedback war fehlerhaft in 0.8.11 – behoben in 0.8.12

#### <a name="known-issues"></a>Bekannte Probleme

* Nach dem Upgrade auf 0.8.10 müssen Sie alle Ihre Anmeldeinformationen aktualisieren.
* Beim Vergrößern oder Verkleinern der Anzeige wurde die Zoomstufe u.U. vorübergehend auf den Standardwert zurückgesetzt.
* Wenn mehr als drei Gruppen von Blobs oder Dateien gleichzeitig hochgeladen werden, können Fehler auftreten.
* Im Panel mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern.
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten.
* Obwohl Azure Stack derzeit keine Dateifreigaben unterstützt, wird dennoch ein Knoten „Dateifreigaben“ unter einem angefügten Azure Stack-Speicherkonto angezeigt.
* Für die Ubuntu 14.04-Installation muss ein Update oder Upgrade der gcc-Version ausgeführt werden – Schritte zum Ausführen eines Upgrades finden Sie unten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089-and-088"></a>Version 0.8.9 und 0.8.8
23.02.2017

>[!VIDEO https://www.youtube.com/embed/R6gonK3cYAc?ecver=1]

>[!VIDEO https://www.youtube.com/embed/SrRPCm94mfE?ecver=1]


#### <a name="new"></a>Neu

* Storage-Explorer 0.8.9 lädt automatisch die neueste Version für Updates herunter.
* Hotfix: Beim Anfügen eines Speicherkontos mit einem vom Portal generierten SAS-URI trat ein Fehler auf.
* Sie können nun Blob-Momentaufnahmen erstellen, verwalten und höher stufen.
* Sie können sich nun bei Azure China-, Azure Deutschland- und Azure US Government-Konten anmelden.
* Sie können jetzt die Zoomstufe ändern. Verwenden Sie die Optionen im Menü „Ansicht“ zum Vergrößern, zum Verkleinern und zum Zurücksetzen der Zoomstufe.
* Unicode-Zeichen werden jetzt in Benutzermetadaten für Blobs und Dateien unterstützt.
* Verbesserungen der Barrierefreiheit.
* Versionshinweise für die nächste Version können nun über die Updatebenachrichtigung eingesehen werden. Die aktuellen Versionshinweise können auch über das Hilfemenü angezeigt werden.

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Die Versionsnummer wird nun in der Systemsteuerung von Windows korrekt angezeigt
* Behoben: Die Suche ist nicht mehr auf 50.000 Knoten beschränkt
* Behoben: Der Upload in eine Dateifreigabe dauerte unendlich lange, wenn das Zielverzeichnis nicht bereits vorhanden war
* Behoben: Verbesserte Stabilität bei langen Upload- und Downloadvorgängen

#### <a name="known-issues"></a>Bekannte Probleme

* Beim Vergrößern oder Verkleinern der Anzeige wurde die Zoomstufe u.U. vorübergehend auf den Standardwert zurückgesetzt.
* Der Schnellzugriff funktioniert nur mit abonnementbasierten Elementen. Lokale Ressourcen oder Ressourcen, die per Schlüssel oder SAS-Token angefügt werden, werden in dieser Version nicht unterstützt.
* Beim Schnellzugriff kann das Navigieren zur Zielressource einige Sekunden dauern. Dies hängt davon ab, wie viele Ressourcen Sie verwenden.
* Wenn mehr als drei Gruppen von Blobs oder Dateien gleichzeitig hochgeladen werden, können Fehler auftreten.

16.12.2016
### <a name="version-087"></a>Version 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Neu

* Sie können im Fenster „Aktivitäten“ wählen, wie Sie Konflikte zu Beginn einer Update-, Download- oder Kopiersitzung lösen
* Bewegen Sie den Mauszeiger auf eine Registerkarte, um den vollständigen Pfad der Speicherressource anzuzeigen
* Wenn Sie auf eine Registerkarte klicken, wird sie mit dem Speicherort im Navigationsbereich auf der linken Seite synchronisiert

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Storage-Explorer ist unter Mac jetzt eine vertrauenswürdige App
* Behoben: Ubuntu 14.04 wird wieder unterstützt
* Behoben: Die Benutzeroberflächenoption „Konto hinzufügen“ blinkte manchmal beim Laden von Abonnements
* Behoben: Es kam vor, dass im Navigationsbereich auf der linken Seite nicht alle Speicherressourcen aufgeführt wurden
* Behoben: Im Aktionsbereich wurden manchmal leere Aktionen angezeigt
* Behoben: Die Fenstergröße von der letzten geschlossenen Sitzung wird jetzt beibehalten
* Behoben: Sie können mit dem Kontextmenü mehrere Registerkarten für dieselbe Ressource öffnen

#### <a name="known-issues"></a>Bekannte Probleme

* Der Schnellzugriff funktioniert nur mit abonnementbasierten Elementen. Lokale Ressourcen oder Ressourcen, die per Schlüssel oder SAS-Token angefügt werden, werden in dieser Version nicht unterstützt
* Beim Schnellzugriff kann das Navigieren zur Zielressource einige Sekunden dauern. Dies hängt davon ab, wie viele Ressourcen Sie verwenden
* Wenn mehr als drei Gruppen von Blobs oder Dateien gleichzeitig hochgeladen werden, können Fehler auftreten
* Bei der Suche wird auf ca. 50.000 Knoten gesucht. Anschließend kann es zu einer Leistungsbeeinträchtigung oder zu Ausnahmefehlern kommen
* Wenn Sie den Storage-Explorer unter macOS zum ersten Mal verwenden, werden unter Umständen mehrere Eingabeaufforderungen angezeigt, in denen die Berechtigung des Benutzers zum Zugreifen auf den Schlüsselbund abgefragt wird. Es wird empfohlen, die Option „Immer zulassen“ zu wählen, damit die Eingabeaufforderung nicht mehr angezeigt wird

18.11.2016
### <a name="version-086"></a>Version 0.8.6

#### <a name="new"></a>Neu

* Sie können die am häufigsten verwendeten Dienste jetzt für den Schnellzugriff anheften, um die Navigation zu vereinfachen.
* Sie können jetzt mehrere Editoren auf unterschiedlichen Registerkarten öffnen. Einmal klicken, um eine temporäre Registerkarte zu öffnen, und doppelklicken, um eine dauerhafte Registerkarte zu öffnen. Sie können auch auf die temporäre Registerkarte klicken, um sie zu einer dauerhaften Registerkarte zu machen
* Es wurden merkliche Verbesserungen der Leistung und Stabilität für Uploads und Downloads erzielt, vor allem für große Dateien auf schnellen Computern
* Erstellen von leeren „virtuellen“ Ordnern in Blobcontainern ist jetzt möglich
* Die bereichsbezogene Suche mit der neuen, verbesserten Teilzeichenfolgen-Suche wurde hinzugefügt, sodass Sie nun über zwei Suchoptionen verfügen:
    * Globale Suche: Hier können Sie einfach einen Suchbegriff in das Suchtextfeld eingeben
    * Bereichsbezogene Suche: Klicken Sie auf das Lupensymbol neben einem Knoten, und fügen Sie dann einen Suchbegriff am Ende des Pfads hinzu, bzw. klicken Sie mit der rechten Maustaste, und wählen Sie die Option „Ab hier suchen“ aus
* Wir haben verschiedene Designs hinzugefügt: Hell (Standard), Dunkel, Hoher Kontrast (Schwarz) und Hoher Kontrast (Weiß). Navigieren Sie zu „Bearbeiten“ -&gt; „Designs“, um die Designeinstellung zu ändern
* Sie können Blob- und Dateieigenschaften ändern
* Ab jetzt werden codierte (base64) und nicht codierte Warteschlangennachrichten unterstützt
* Unter Linux ist nun ein 64-Bit-Betriebssystem erforderlich. Für diese Version wird nur 64-Bit-Ubuntu 16.04.1 LTS unterstützt
* Wir haben unser Logo aktualisiert!

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Probleme mit dem Einfrieren des Bildschirms
* Behoben: Erweiterte Sicherheit
* Behoben: Gelegentlich treten doppelte angefügte Konten auf
* Behoben: Durch einen Blob mit einem nicht definierten Inhaltstyp konnte eine Ausnahme generiert werden
* Behoben: Öffnen des Abfragepanels für eine leere Tabelle war nicht möglich
* Behoben: Diverse Fehler bei der Suche
* Behoben: Beim Klicken auf „Mehr laden“ wurde die Anzahl der geladenen Ressourcen wurde von 50 auf 100 erhöht
* Behoben: Wenn bei der ersten Ausführung die Anmeldung bei einem Konto erfolgt, werden nun standardmäßig sämtliche Abonnements für das betreffende Konto ausgewählt

#### <a name="known-issues"></a>Bekannte Probleme

* Diese Version des Storage-Explorers wird nicht unter Ubuntu 14.04 ausgeführt
* Wenn mehrere Registerkarten für dieselbe Ressource geöffnet werden sollen, klicken Sie nicht ständig auf die gleiche Ressource. Klicken Sie auf eine andere Ressource, wechseln Sie zurück, und klicken Sie dann auf die ursprüngliche Ressource, um sie in einer weiteren Registerkarte zu öffnen
* Der Schnellzugriff funktioniert nur mit abonnementbasierten Elementen. Lokale Ressourcen oder Ressourcen, die per Schlüssel oder SAS-Token angefügt werden, werden in dieser Version nicht unterstützt
* Beim Schnellzugriff kann das Navigieren zur Zielressource einige Sekunden dauern. Dies hängt davon ab, wie viele Ressourcen Sie verwenden
* Wenn mehr als drei Gruppen von Blobs oder Dateien gleichzeitig hochgeladen werden, können Fehler auftreten
* Bei der Suche wird auf ca. 50.000 Knoten gesucht. Anschließend kann es zu einer Leistungsbeeinträchtigung oder zu Ausnahmefehlern kommen

03.10.2016
### <a name="version-085"></a>Version 0.8.5

#### <a name="new"></a>Neu

* Sie können nun im Portal generierte SAS-Schlüssel zum Anfügen an Speicherkonten und Ressourcen verwenden

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Eine Racebedingung während der Suche bewirkte gelegentlich, dass Knoten nicht erweitert werden konnten
* Behoben: „HTTP verwenden“ funktioniert nicht, wenn eine Verbindung mit Speicherkonten mithilfe von Kontoname und -schlüssel hergestellt wird
* Behoben: SAS-Schlüssel (insbesondere vom Portal generierte) geben einen Fehler „mit nachgestelltem Schrägstrich“ zurück
* Behoben: Probleme beim Tabellenimport
    * Gelegentlich wurden Partitionsschlüssel und Zeilenschlüssel umgekehrt
    * „Null“-Partitionsschlüssel können nicht gelesen werden

#### <a name="known-issues"></a>Bekannte Probleme

* Bei der Suche wird auf ca. 50.000 Knoten gesucht; anschließend kann es zu einer Leistungsbeeinträchtigung kommen
* Dateien werden von Azure Stack derzeit nicht unterstützt; beim Versuch, Dateien zu erweitern, wird also ein Fehler ausgegeben

12.09.2016
### <a name="version-084"></a>Version 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Neu

* Generieren von direkten Verknüpfungen mit Speicherkonten, Containern, Warteschlangen, Tabellen oder Dateifreigaben zum gemeinsamen Nutzen und einfachen Aufrufen Ihrer Ressourcen – Windows- und Mac OS-Unterstützung
* Suchen nach Blobcontainern, Tabellen, Warteschlangen, Dateifreigaben oder Speicherkonten über das Suchfeld
* Sie können nun Klauseln im Abfrage-Generator für Tabellen generieren
* Umbenennen und Kopieren/Einfügen von Blobcontainern, Dateifreigaben, Tabellen, Blobs, Blobordnern, Dateien und Verzeichnissen aus mit SAS angefügten Konten und Containern
* Beim Umbenennen und Kopieren von Blobcontainern und Dateifreigaben bleiben jetzt Eigenschaften und Metadaten erhalten

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Tabellenentitäten können nicht bearbeitet werden, wenn sie boolesche oder binäre Eigenschaften enthalten

#### <a name="known-issues"></a>Bekannte Probleme

* Bei der Suche wird auf ca. 50.000 Knoten gesucht; anschließend kann es zu einer Leistungsbeeinträchtigung kommen

03.08.2016
### <a name="version-083"></a>Version 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Neu

* Umbenennen von Containern, Tabellen, Dateifreigaben
* Verbesserte Oberfläche des Abfrage-Generators
* Möglichkeit zum Speichern und Laden von Abfragen
* Direkte Verknüpfungen mit Speicherkonten oder Containern, Warteschlangen, Tabellen oder Dateifreigaben zum gemeinsamen Nutzen und einfachen Aufrufen Ihrer Ressourcen (nur Windows; macOS-Unterstützung wird in Kürze verfügbar sein!)
* Möglichkeit zum Verwalten und Konfigurieren von CORS-Regeln

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Bei Microsoft-Konten ist alle 8 bis 12 Stunden eine erneute Authentifizierung erforderlich

#### <a name="known-issues"></a>Bekannte Probleme

* Gelegentlich scheint die Benutzeroberfläche nicht zu reagieren – dieses Problem kann durch Maximieren des Fensters behoben werden
* Für die Installation unter macOS sind u.U. erhöhte Berechtigungen erforderlich
* Im Panel mit den Kontoeinstellungen wird unter Umständen angezeigt, dass Sie Anmeldeinformationen erneut eingeben müssen, um Abonnements zu filtern
* Beim Umbenennen von Dateifreigaben, Blobcontainern und Tabellen werden Metadaten oder sonstige Eigenschaften für den Container nicht beibehalten, z.B. Dateifreigabe-Kontingent, öffentliche Zugriffsebene und Zugriffsrichtlinien
* Beim Umbenennen von Blobs (einzeln oder in einem umbenannten Blobcontainer) werden Momentaufnahmen nicht beibehalten. Alle anderen Eigenschaften und Metadaten für Blobs, Dateien und Entitäten werden beim Umbenennen beibehalten
* Das Kopieren und Umbenennen von Ressourcen funktioniert nicht in über SAS angefügten Konten

07.07.2016
### <a name="version-082"></a>Version 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Neu

* Speicherkonten werden nach Abonnements gruppiert; Entwicklungsspeicher und -ressourcen , die über Schlüssel oder SAS angefügt wurden, werden unter dem Knoten „(Local and Attached)“ (Lokal und angefügt) angezeigt
* Abmelden von Konten im Panel „Azure-Kontoeinstellungen“
* Konfigurieren von Proxyeinstellungen zum Aktivieren und Verwalten der Anmeldung
* Erstellen und Unterbrechen von Blob-Leases
* Öffnen von Blobcontainern, Warteschlangen, Tabellen und Dateien per einfachen Klick

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Mit .NET- oder Java-Bibliotheken eingefügte Warteschlangennachrichten werden nicht ordnungsgemäß aus base64 decodiert
* Behoben: $metrics-Tabellen werden für Blob Storage-Konten nicht angezeigt
* Behoben: Tabellenknoten funktioniert nicht für lokalen Speicher (Entwicklung)

#### <a name="known-issues"></a>Bekannte Probleme

* Für die Installation unter macOS sind u.U. erhöhte Berechtigungen erforderlich

15.06.2016
### <a name="version-080"></a>Version 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Neu

* Unterstützung von Dateifreigaben: Anzeigen, Hochladen, Herunterladen, Kopieren von Dateien und Verzeichnissen, SAS-URIs (Erstellen und Herstellen von Verbindungen)
* Verbesserte Benutzererfahrung beim Herstellen von Verbindungen mit dem Speicher mit SAS-URIs oder Kontoschlüsseln
* Exportieren von Ergebnissen der Tabellenabfrage
* Neuanordnen und Anpassen von Tabellenspalten
* Anzeigen von $logs-Blobcontainern und $metrics-Tabellen für Speicherkonten mit aktivierten Metriken
* Verbessertes Export- und Importverhalten, nun ist der Typ von Eigenschaftswerten eingeschlossen

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Eventuelle unvollständige Uploads oder Downloads von großen Blobs
* Behoben: Beim Bearbeiten, Hinzufügen oder Importieren einer Entität mit einem numerischen Zeichenfolgenwert („1“) wird dieser in einen double-Wert konvertiert
* Behoben: Der Tabellenknoten kann in der lokalen Entwicklungsumgebung nicht erweitert werden

#### <a name="known-issues"></a>Bekannte Probleme

* $metrics-Tabellen sind für Blob Storage-Konten nicht sichtbar
* Programmgesteuert hinzugefügte Warteschlangennachrichten werden u.U. nicht ordnungsgemäß angezeigt, wenn sie mit Base64-Codierung codiert sind

17.05.2016
### <a name="version-07201605090"></a>Version 0.7.20160509.0

#### <a name="new"></a>Neu

* Bessere Fehlerbehandlung bei Abstürzen von Apps

#### <a name="fixes"></a>Fehlerbehebungen

* Korrigierter Fehler: Gelegentlich wurden InfoBar-Meldungen nicht angezeigt, wenn Anmeldeinformationen angefordert wurden

#### <a name="known-issues"></a>Bekannte Probleme

* Tabellen: Wenn der Benutzer beim Hinzufügen, Bearbeiten oder Importieren einer Entität mit einer Eigenschaft mit einem mehrdeutigen Wert wie „1“ oder „1,0“ versucht, diese als `Edm.String` zu senden, wird der Wert über die Client-API als Edm.Double zurückgegeben

31.03.2016

### <a name="version-07201603250"></a>Version 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Neu

* Unterstützung von Tabellen: Anzeigen, Abfragen, Exportieren, Importieren und CRUD-Vorgänge für Entitäten
* Unterstützung von Warteschlangen: Anzeigen, Hinzufügen und Entfernen von Nachrichten aus der Warteschlange
* Generieren von SAS-URIs für Speicherkonten
* Herstellen einer Verbindung mit Speicherkonten mit SAS-URIs
* Updatebenachrichtigungen für künftige Aktualisierungen von Storage-Explorer
* Aktualisiertes Erscheinungsbild

#### <a name="fixes"></a>Fehlerbehebungen

* Verbesserungen von Leistung und Zuverlässigkeit

### <a name="known-issues-amp-mitigations"></a>Bekannte Probleme &amp; Lösungen

* Herunterladen von großen Blobdateien funktioniert nicht ordnungsgemäß – es wird empfohlen, AzCopy zu verwenden, während wir dieses Problem beheben
* Kontoanmeldeinformationen werden weder abgerufen noch zwischengespeichert, wenn der Basisordner nicht gefunden wurde bzw. in den Basisordner nicht geschrieben werden kann
* Wenn beim Hinzufügen, Bearbeiten oder Importieren einer Entität mit einer Eigenschaft mit einem mehrdeutigen Wert wie „1“ oder „1,0“ versucht wird, diese als `Edm.String` zu senden, wird der Wert über die Client-API als Edm.Double zurückgegeben
* Beim Importieren von CSV-Dateien mit mehrzeiligen Datensätzen werden die Daten möglicherweise aufgeteilt oder verschlüsselt

03.02.2016

### <a name="version-07201601291"></a>Version 0.7.20160129.1

#### <a name="fixes"></a>Fehlerbehebungen

* Verbesserte Gesamtleistung beim Hochladen, Herunterladen und Kopieren von Blobs

14.01.2016

### <a name="version-07201601050"></a>Version 0.7.20160105.0

#### <a name="new"></a>Neu

* Linux-Unterstützung (Paritätsfunktionen für OSX)
* Hinzufügen von Blobcontainern mit SAS-Schlüssel (Shared Access Signatures)
* Hinzufügen von Speicherkonten für Azure China
* Hinzufügen von Speicherkonten mit benutzerdefinierten Endpunkten
* Öffnen und Anzeigen der Inhaltstexte und Bildblobs
* Anzeigen von Blobeigenschaften und Metadaten

#### <a name="fixes"></a>Fehlerbehebungen

* Behoben: Das Hochladen oder Herunterladen einer großen Anzahl von Blobs (500+) bewirkt gelegentlich, dass die App einen weißen Bildschirm aufweist
* Behoben: Beim Festlegen der öffentlichen Zugriffsebene für Blobcontainer wird der neue Wert erst aktualisiert, wenn der Fokus auf den Container neu festgelegt wird. Zudem wird im Dialogfeld standardmäßig „No public access“ (Kein öffentlicher Zugriff) und nicht der tatsächliche aktuelle Wert angezeigt.
* Bessere Gesamtunterstützung von Tastatur/Barrierefreiheit und Benutzeroberfläche
* Langer Breadcrumb-Verlaufstext wird mit Leerraum umbrochen
* SAS-Dialogfeld unterstützt die Eingabeüberprüfung
* Lokaler Speicher ist weiterhin verfügbar, selbst wenn die Benutzeranmeldeinformationen abgelaufen sind
* Beim Löschen eines geöffneten Blobcontainers wird der Blob-Explorer auf der rechten Seite geschlossen

#### <a name="known-issues"></a>Bekannte Probleme

* Für die Linux-Installation muss ein Update oder Upgrade der gcc-Version ausgeführt werden – Schritte zum Ausführen eines Upgrades finden Sie unten:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

18.11.2015
### <a name="version-07201511160"></a>Version 0.7.20151116.0

#### <a name="new"></a>Neu

* macOS- und Windows-Versionen
* Anmelden zum Anzeigen Ihrer Speicherkonten – Verwenden Sie Ihr Organisationskonto, Microsoft-Konto, 2FA usw.
* Lokaler Entwicklungsspeicher (Verwenden des Speicheremulators, nur Windows)
* Unterstützung von Azure Resource Manager sowie von klassischen Ressourcen
* Erstellen und Löschen von Blobs, Warteschlangen oder Tabellen
* Suchen nach bestimmten Blobs, Warteschlangen oder Tabellen
* Untersuchen des Inhalts von Blobcontainern
* Anzeigen von Verzeichnissen und Navigieren durch Verzeichnisse
* Hochladen, Herunterladen und Löschen von Blobs und Ordnern
* Anzeigen von Blobeigenschaften und Metadaten
* Generieren von SAS-Schlüsseln
* Verwalten und Erstellen von SAPs (Stored Access Policies, gespeicherte Zugriffsrichtlinien)
* Suchen nach Blobs anhand des Präfix
* Hochladen oder Herunterladen von Dateien per Drag & Drop

#### <a name="known-issues"></a>Bekannte Probleme

* Beim Festlegen der öffentlichen Zugriffsebene für Blobcontainer wird der neue Wert erst aktualisiert, wenn der Fokus auf den Container neu festgelegt wird
* Wenn Sie das Dialogfeld öffnen, um die öffentliche Zugriffsebene festzulegen, wird als Standardwert „No public access“ (Kein öffentlicher Zugriff) und nicht der tatsächliche aktuelle Wert angezeigt
* Heruntergeladene Blobs können nicht umbenannt werden
* Einträge von Aktivitätsprotokollen bleiben bei Auftreten eines Fehlers gelegentlich in einem Statuszustand „hängen“, wobei der Fehler jedoch nicht angezeigt wird
* Die App stürzt ab oder der Bildschirm wird komplett weiß, wenn versucht wird, eine große Anzahl von Blobs hochzuladen oder herunterzuladen
* In einigen Fällen kann ein Kopiervorgang nicht abgebrochen werden
* Wenn Sie beim Erstellen eines Containers (Blob/Warteschlange/Tabelle) einen ungültigen Namen eingeben und mit dem Erstellen eines Containers eines anderen Typs fortfahren, kann der Fokus nicht auf den neuen Typ festgelegt werden
* Neuer Ordner kann nicht erstellt werden, oder Ordner kann nicht umbenannt werden




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
