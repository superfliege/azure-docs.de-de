---
title: Kopieren oder Verschieben von Daten in Azure Storage mit AzCopy v10 (Vorschau) | Microsoft-Dokumentation
description: Verwenden Sie das Hilfsprogramm AzCopy v10 (Vorschau) zum Verschieben oder Kopieren von Daten in oder aus Blob-, Data Lake- und Dateiinhalte(n). Kopieren Sie Daten aus lokalen Dateien nach Azure Storage oder innerhalb von bzw. zwischen Speicherkonten. Migrieren Sie Ihre Daten ganz einfach nach Azure Storage.
services: storage
author: artemuwka
ms.service: storage
ms.topic: article
ms.date: 10/09/2018
ms.author: artemuwka
ms.subservice: common
ms.openlocfilehash: c9009e898b00212dba4dec9bf38af2bfa057b8ea
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56244605"
---
# <a name="transfer-data-with-the-azcopy-v10-preview"></a>Übertragen von Daten mit AzCopy v10 (Vorschau)

AzCopy v10 (Vorschau) ist das Befehlszeilenprogramm der nächsten Generation zum Kopieren von Daten in/aus Microsoft Azure Blob und File Storage, das eine neu gestaltete Befehlszeilenoberfläche und eine neue Architektur für leistungsstarke, zuverlässige Datenübertragungen bietet. Mit AzCopy können Sie Daten zwischen einem Dateisystem und einem Speicherkonto oder zwischen Speicherkonten kopieren.

## <a name="whats-new-in-azcopy-v10"></a>Neuerungen in AzCopy v10

- Synchronisieren eines Dateisystems mit Azure Blob oder umgekehrt. Verwenden Sie `azcopy sync <source> <destination>`. Ideal geeignet für Szenarien mit inkrementellen Kopien.
- Unterstützt Azure Data Lake Storage Gen2-APIs. Verwenden von `myaccount.dfs.core.windows.net` als URI zum Aufrufen der ADLS Gen2-APIs.
- Unterstützt das Kopieren eines gesamten Kontos (nur Blob-Dienst) in ein anderes Konto.
- Der Kopiervorgang zwischen Konten verwendet nun die neuen [Put From URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url)-APIs. Es ist keine Datenübertragung an den Client erforderlich, was den Vorgang beschleunigt.
- Auflisten/Entfernen von Dateien und Blobs in einem angegebenen Pfad.
- Unterstützt Platzhaltermuster in einem Pfad sowie die Flags --include und --exclude.
- Verbesserte Resilienz: Jede AzCopy-Instanz erstellt eine Auftragsreihenfolge und eine zugehörige Protokolldatei. Sie können vorherige Aufträge anzeigen und neu starten und fehlgeschlagene Aufträge fortsetzen. AzCopy wiederholt außerdem eine Übertragung automatisch nach einem Fehler.
- Allgemeine Leistungsverbesserungen.

## <a name="download-and-install-azcopy"></a>Herunterladen und Installieren von AzCopy

### <a name="latest-preview-version-v10"></a>Neueste Vorschauversion (v10)

Laden Sie die neueste Vorschauversion von AzCopy herunter:
- [Windows](https://aka.ms/downloadazcopy-v10-windows)
- [Linux](https://aka.ms/downloadazcopy-v10-linux)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac)

### <a name="latest-production-version-v81"></a>Neueste Produktionsversion (v8.1)

Laden Sie die [neueste Produktversion von AzCopy für Windows](https://aka.ms/downloadazcopy) herunter.

### <a name="azcopy-supporting-table-storage-service-v73"></a>AzCopy unterstützt den Table Storage-Dienst (v7.3)

Laden Sie [AzCopy v7.3 mit Unterstützung für das Kopieren von Daten in den bzw. aus dem Microsoft Azure Table Storage-Dienst](https://aka.ms/downloadazcopynet) herunter.

## <a name="post-installation-steps"></a>Schritte nach der Installation

Eine Installation von AzCopy v10 ist nicht erforderlich. Öffnen Sie eine bevorzugte Befehlszeilenanwendung, und navigieren Sie zu dem Ordner, in dem die ausführbare Datei `azcopy.exe` gespeichert ist. Bei Bedarf können Sie den Speicherort für den AzCopy-Ordner zum Systempfad hinzufügen.

## <a name="authentication-options"></a>Authentifizierungsoptionen

Mit AzCopy v10 können Sie die folgenden Optionen bei der Authentifizierung mit Azure Storage verwenden:
- **Azure Active Directory [unterstützt für Blob und ADLS Gen2-Dienste]**. Verwenden Sie ```.\azcopy login```, um sich mit Azure Active Directory anzumelden.  Dem Benutzer muss die [Rolle „Mitwirkender an Storage-Blobdaten“ zugewiesen](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) sein, damit er in den Blobspeicher mithilfe von Azure Active Directory-Authentifizierung schreiben kann.
- **SAS-Token [unterstützt für Blob und Dateidienste]**. Fügen Sie das SAS-Token dem Blobpfad in der Befehlszeile hinzu, um es zu verwenden. Sie können das SAS-Token mit dem Azure-Portal, mit [Storage-Explorer](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/), mit [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageblobsastoken) oder mit anderen Tools Ihrer Wahl generieren. Weitere Informationen finden Sie unter [Beispiele](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).

> [!IMPORTANT]
> Wenn Sie eine Supportanfrage an Microsoft-Support senden (oder eine Problembehandlung unter Einbeziehung eines Drittanbieters durchführen), geben Sie bitte die editierte Version des Befehls frei, den Sie auszuführen versuchen. Auf diese Weise stellen Sie sicher, dass die SAS nicht versehentlich für irgendeine Person freigegeben wird. Die editierte Version steht am Anfang der Protokolldatei. Weitere Details finden Sie weiter unten in diesem Artikel im Abschnitt „Problembehandlung“.

## <a name="getting-started"></a>Erste Schritte

AzCopy v10 verfügt über eine einfache selbstdokumentierte Syntax. Die allgemeine Syntax sieht wie folgt aus, wenn Sie sich bei Azure Active Directory angemeldet haben:

```azcopy
.\azcopy <command> <arguments> --<flag-name>=<flag-value>
# Examples if you have logged into the Azure Active Directory:
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile"
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/container"

# Examples if you are using SAS tokens to authenticate:
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container?sastoken" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile?sastoken"
```

Eine Liste der verfügbaren Befehle können Sie folgendermaßen abrufen:

```azcopy
.\azcopy -help
# Using the alias instead
.\azcopy -h
```

Um die Hilfeseite und Beispiele für einen bestimmten Befehl anzuzeigen, führen Sie den folgenden Befehl aus:

```azcopy
.\azcopy <cmd> -help
# Example:
.\azcopy cp -h
```

## <a name="create-a-blob-container-or-file-share"></a>Erstellen eines Blobcontainers oder einer Dateifreigabe 

**Erstellen eines Blobcontainers**

```azcopy
.\azcopy make "https://account.blob.core.windows.net/container-name"
```

**Erstellen einer Dateifreigabe**

```azcopy
.\azcopy make "https://account.file.core.windows.net/share-name"
```

**Erstellen eines Blobcontainers mit ADLS Gen2**

Wenn Sie hierarchische Namespaces in Ihrem Blobspeicherkonto aktiviert haben, können Sie mit dem folgenden Befehl ein neues Dateisystem (Blobcontainer) erstellen, damit Sie Dateien hochladen können.

```azcopy
.\azcopy make "https://account.dfs.core.windows.net/top-level-resource-name"
```

## <a name="copy-data-to-azure-storage"></a>Kopieren von Daten in Azure Storage

Verwenden Sie den Kopierbefehl, um Daten aus der Quelle in das Ziel zu übertragen. Die Quelle bzw. das Ziel kann Folgendes sein:
- Ein lokales Dateisystem
- Azure-Blob/virtuelles Verzeichnis/Container-URI
- Azure-Datei/Verzeichnis/Dateifreigabe-URI
- Azure Data Lake Storage Gen2-Dateisystem/Verzeichnis/Datei-URI

```azcopy
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
# Using alias instead
.\azcopy cp <source path> <destination path> --<flag-name>=<flag-value>
```

Der folgende Befehl lädt alle Dateien unter dem Ordner `C:\local\path` rekursiv in den Container `mycontainer1` hoch und erstellt im Container das Verzeichnis `path`:

```azcopy
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

Der folgende Befehl lädt alle Dateien unter dem Ordner `C:\local\path` (nicht rekursiv, also ohne Unterverzeichnisse) in den Container `mycontainer1` hoch:

```azcopy
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/mycontainer1<sastoken>"
```

Um weitere Beispiele abzurufen, verwenden Sie den folgenden Befehl:

```azcopy
.\azcopy cp -h
```

## <a name="copy-data-between-two-storage-accounts"></a>Kopieren von Daten zwischen zwei Speicherkonten

Das Kopieren von Daten zwischen zwei Speicherkonten verwendet die [Put Block From URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url)-API und nutzt nicht die Netzwerkbandbreite des Clientcomputers. Daten werden zwischen zwei Azure Storage-Servern direkt kopiert, während AzCopy den Kopiervorgang einfach orchestriert. Diese Option ist derzeit nur für Blob Storage verfügbar.

Um die Daten zwischen zwei Speicherkonten zu kopieren, verwenden Sie den folgenden Befehl:
```azcopy
.\azcopy cp "https://myaccount.blob.core.windows.net/<sastoken>" "https://myotheraccount.blob.core.windows.net/<sastoken>" --recursive=true
```

> [!NOTE]
> Der Befehl zählt alle Blobcontainer auf und kopiert sie in das Zielkonto. Zu diesem Zeitpunkt unterstützt AzCopy v10 nur das Kopieren von Blockblobs zwischen zwei Speicherkonten. Alle anderen Speicherkontoobjekte (Anfügeblobs, Seitenblobs, Dateien, Tabellen und Warteschlangen) werden übersprungen.

## <a name="copy-a-vhd-image-to-a-storage-account"></a>Kopieren eines VHD-Image in ein Speicherkonto

AzCopy v10 lädt Daten standardmäßig in Blockblobs hoch. Wenn eine Quelldatei jedoch die Erweiterung VHD aufweist, lädt AzCopy v10 sie standardmäßig in ein Seitenblob hoch. Dieses Verhalten ist derzeit nicht konfigurierbar.

## <a name="sync-incremental-copy-and-delete-blob-storage-only"></a>Sync: inkrementelles Kopieren und Löschen (nur Blob Storage)

> [!NOTE]
> Der Sync-Befehl synchronisiert Inhalte aus der Quelle mit dem Ziel und beinhaltet das LÖSCHEN von Zieldateien, wenn diese in der Quelle nicht vorhanden sind. Stellen Sie sicher, dass Sie das Ziel verwenden, das Sie synchronisieren möchten.

Verwenden Sie den folgenden Befehl, um Ihr lokales Dateisystem mit einem Speicherkonto zu synchronisieren:

```azcopy
.\azcopy sync "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

Auf die gleiche Weise können Sie einen Blobcontainer mit einem lokalen Dateisystem synchronisieren:

```azcopy
# If you're using Azure Active Directory authentication the sastoken is not required
.\azcopy sync "https://account.blob.core.windows.net/mycontainer1" "C:\local\path" --recursive=true
```

Mit diesem Befehl können Sie die Quelle inkrementell basierend auf den Zeitstempeln der letzten Änderung mit dem Ziel synchronisieren. Wenn Sie eine Datei in der Quelle hinzufügen oder löschen, verfährt AzCopy v10 im Ziel auf die gleiche Weise. Vor dem Löschen fragt AzCopy eine Bestätigung des Löschens der Dateien ab.

## <a name="advanced-configuration"></a>Erweiterte Konfiguration

### <a name="configure-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

Um die Proxyeinstellungen für AzCopy v10 zu konfigurieren, legen Sie die Umgebungsvariable „https_proxy“ mit dem folgenden Befehl fest:

```cmd
# For Windows:
set https_proxy=<proxy IP>:<proxy port>
# For Linux:
export https_proxy=<proxy IP>:<proxy port>
# For MacOS
export https_proxy=<proxy IP>:<proxy port>
```

### <a name="optimize-throughput"></a>Optimieren des Durchsatzes

Legen Sie die Umgebungsvariable AZCOPY_CONCURRENCY_VALUE fest, um die Anzahl der gleichzeitigen Anforderungen zu konfigurieren und die Durchsatzleistung und die Ressourcennutzung zu steuern. Standardmäßig ist der Wert auf 300 festgelegt. Die Verringerung des Werts schränkt die von AzCopy v10 verwendete Bandbreite und CPU ein.

```cmd
# For Windows:
set AZCOPY_CONCURRENCY_VALUE=<value>
# For Linux:
export AZCOPY_CONCURRENCY_VALUE=<value>
# For MacOS
export AZCOPY_CONCURRENCY_VALUE=<value>
# To check the current value of the variable on all the platforms
.\azcopy env
# If the value is blank then the default value is currently in use
```

## <a name="troubleshooting"></a>Problembehandlung

AzCopy v10 erstellt Protokolldateien und Plandateien für alle Aufträge. Sie können die Protokolle verwenden, um potenzielle Probleme zu untersuchen und zu beheben. Die Protokolle enthalten den Status des Fehlers (UPLOADFAILED, COPYFAILED und DOWNLOADFAILED), den vollständigen Pfad und den Grund des Fehlers. Die Auftragsprotokolle und Plandateien sind im Ordner „%USERPROFILE\\.azcopy“ (unter Windows) bzw. im Ordner „$HOME\\.azcopy“ (unter Mac und Linux) gespeichert.

> [!IMPORTANT]
> Wenn Sie eine Supportanfrage an Microsoft-Support senden (oder eine Problembehandlung unter Einbeziehung eines Drittanbieters durchführen), geben Sie bitte die editierte Version des Befehls frei, den Sie auszuführen versuchen. Auf diese Weise stellen Sie sicher, dass die SAS nicht versehentlich für irgendeine Person freigegeben wird. Die editierte Version steht am Anfang der Protokolldatei.

### <a name="change-the-location-of-the-log-files"></a>Ändern des Speicherorts der Protokolldateien

Sie können den Speicherort der Protokolldateien bei Bedarf ändern oder, um eine Überfüllung des Betriebssystemdatenträgers zu vermeiden.

```cmd
# For Windows:
set AZCOPY_LOG_LOCATION=<value>
# For Linux:
export AZCOPY_LOG_LOCATION=<value>
# For MacOS
export AZCOPY_LOG_LOCATION=<value>
# To check the current value of the variable on all the platforms
.\azcopy env
# If the value is blank then the default value is currently in use
```

### <a name="review-the-logs-for-errors"></a>Überprüfen der Protokolle auf Fehler

Der folgende Befehl ruft alle Fehler mit dem Status UPLOADFAILED aus dem Protokoll 04dc9ca9-158f-7945-5933-564021086c79 ab:

```azcopy
cat 04dc9ca9-158f-7945-5933-564021086c79.log | grep -i UPLOADFAILED
```

### <a name="view-and-resume-jobs"></a>Anzeigen und Fortsetzen von Aufträgen

Jeder Übertragungsvorgang erstellt einen AzCopy-Auftrag. Sie können den Verlauf der Aufträge mit dem folgenden Befehl anzeigen:

```azcopy
.\azcopy jobs list
```

Verwenden Sie den folgenden Befehl, um die Auftragsstatistiken anzuzeigen:

```azcopy
.\azcopy jobs show <job-id>
```

Um die Übertragungen nach ihrem Status zu filtern, verwenden Sie den folgenden Befehl:

```azcopy
.\azcopy jobs show <job-id> --with-status=Failed
```

Sie können einen fehlerhaften/abgebrochenen Auftrag mithilfe seines Bezeichners und dem SAS-Token (es wird aus Sicherheitsgründen nicht persistent gespeichert) fortsetzen:

```azcopy
.\azcopy jobs resume <jobid> --sourcesastokenhere --destinationsastokenhere
```

### <a name="change-the-default-log-level"></a>Ändern der Standardprotokollebene

Standardmäßig ist die AzCopy-Protokollebene auf INFO festgelegt. Wenn Sie die Ausführlichkeit des Protokolls verringern möchten, um Speicherplatz zu sparen, überschreiben Sie die Einstellung mit der ``--log-level``-Option. Verfügbare Protokollebenen sind: DEBUG, INFO, WARNING, ERROR, PANIC und FATAL.

## <a name="next-steps"></a>Nächste Schritte

Wir freuen uns immer auf Ihr Feedback! Wenn Sie Fragen haben, Probleme melden oder allgemeines Feedback bereitstellen möchten, wenden Sie sich an https://github.com/Azure/azure-storage-azcopy. Vielen Dank!
