---
title: Kopieren Ihres Microsoft Azure Data Box-Datenträgers | Microsoft-Dokumentation
description: Verwenden Sie dieses Tutorial, um zu erfahren, wie Sie Daten auf Ihren Azure Data Box-Datenträger kopieren.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: b0769ba70f495728df5c38b43bae4059b27de88b
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010819"
---
# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Tutorial: Kopieren von Daten auf den Azure Data Box-Datenträger und Durchführen der Überprüfung

In diesem Tutorial wird beschrieben, wie Sie Daten von Ihrem Hostcomputer kopieren und anschließend Prüfsummen generieren, um die Datenintegrität zu überprüfen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Kopieren von Daten auf den Data Box-Datenträger
> * Überprüfen der Integrität von Daten

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:
- Sie haben das [Tutorial: Installieren und Konfigurieren Ihres Azure Data Box-Datenträgers](data-box-disk-deploy-set-up.md) durchgearbeitet.
- Ihre Datenträger wurden ausgepackt und eingeschaltet.
- Sie verfügen über einen Hostcomputer zum Kopieren der Daten auf die Datenträger. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
    - Es muss ein [unterstütztes Betriebssystem](data-box-disk-system-requirements.md) ausgeführt werden.
    - [Windows PowerShell 4 muss installiert sein](https://www.microsoft.com/download/details.aspx?id=40855).
    - [.NET Framework 4.5 muss installiert sein](https://www.microsoft.com/download/details.aspx?id=30653).


## <a name="copy-data-to-disks"></a>Kopieren von Daten auf Datenträger

Führen Sie die folgenden Schritte aus, um eine Verbindung herzustellen und Daten von Ihrem Computer auf den Data Box-Datenträger zu kopieren.

1. Zeigen Sie den Inhalt des entsperrten Laufwerks an. 

    ![Anzeigen des Laufwerkinhalts](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Kopieren Sie die zu importierenden Daten als Blockblobs in den Ordner „BlockBlob“. Kopieren Sie entsprechend Daten, z.B. von VHD/VHDX, in den Ordner „PageBlob“. 

    Im Azure-Speicherkonto wird für jeden Unterordner der Ordner „BlockBlob“ und „PageBlob“ ein Container erstellt. Alle Dateien in den Ordnern „BlockBlob“ und „PageBlob“ werden in den Standardcontainer `$root` unter dem Azure Storage-Konto kopiert. Alle Dateien im Container `$root` werden immer als Blockblobs hochgeladen.

    Falls im Stammverzeichnis Dateien und Ordner vorhanden sind, müssen Sie diese in einen anderen Ordner verschieben, bevor Sie mit dem Kopieren von Daten beginnen.

    Befolgen Sie die Anforderungen an die Azure-Benennungsanforderungen für Namen von Containern und Blobs.

    |Entität   |Konventionen  |
    |---------|---------|
    |Containernamen für Blockblob und Seitenblob     |Müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Kleinbuchstaben, Zahlen und Bindestriche (-) enthalten. Vor und nach jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Ziffer stehen. Aufeinanderfolgende Bindestriche sind in Namen nicht zulässig. <br>Es muss sich um einen gültigen DNS-Namen mit einer Länge von 3 bis 63 Zeichen handeln.          |
    |Blobnamen für Blockblobs und Seitenblobs    |Für Blobnamen wird die Groß-/Kleinschreibung beachtet, und sie können eine beliebige Kombination von Zeichen enthalten. <br>Ein Blobname muss zwischen 1 und 1.024 Zeichen lang sein.<br>Reservierte URL-Zeichen müssen korrekt mit Escapezeichen versehen sein.<br>Die Anzahl von Pfadsegmenten, aus denen der Blobname besteht, darf 254 nicht überschreiten. Ein Pfadsegment ist die Zeichenfolge zwischen aufeinanderfolgenden Trennzeichen (z.B. der Schrägstrich (/)), die für den Namen eines virtuellen Verzeichnisses steht.         |

    > [!IMPORTANT] 
    > Für alle Container und Blobs sollten die [Azure-Namenskonventionen](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions) eingehalten werden. Wenn diese Regeln nicht befolgt werden, tritt beim Datenupload in Azure ein Fehler auf.

3. Stellen Sie beim Kopieren von Dateien sicher, dass Dateien für Blockblobs eine Größe von ca. 4,7 TiB und für Seitenblobs eine Größe von ca. 8 TiB nicht überschreiten. 
4. Sie können zum Kopieren der Daten für den Datei-Explorer Drag & Drop verwenden. Außerdem können Sie jedes SMB-kompatible Dateikopiertool, z.B. Robocopy, verwenden, um die Daten zu kopieren. Mehrere Kopieraufträge können mit dem folgenden Robocopy-Befehl initiiert werden:

    `Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt` 
    
    Die Parameter und Optionen für den Befehl werden wie folgt in Tabellenform angezeigt:
    
    |Parameter/Optionen  |Beschreibung |
    |--------------------|------------|
    |<Source>            | Gibt den Pfad zum Quellverzeichnis an.        |
    |<Destination>       | Gibt den Pfad zum Zielverzeichnis an.        |
    |/E                  | Kopiert Unterverzeichnisse, einschließlich der leeren Verzeichnisse. |
    |/MT[:N]             | Erstellt Multithread-Kopien mit N Threads, wobei N hier für eine ganze Zahl zwischen 1 und 128 steht. <br>Der Standardwert für N ist „8“.        |
    |/R: <N>             | Gibt die Anzahl von Wiederholungsversuchen für fehlerhafte Kopiervorgänge an. Der Standardwert von N ist „1.000.000“ (eine Million Wiederholungen).        |
    |/W: <N>             | Gibt die Wartezeit zwischen Wiederholungen in Sekunden an. Der Standardwert von N ist „30“ (Wartezeit von 30 Sekunden).        |
    |/NFL                | Gibt an, dass Dateinamen nicht protokolliert werden sollen.        |
    |/NDL                | Gibt an, dass Verzeichnisnamen nicht protokolliert werden sollen.        |
    |/FFT                | Setzt FAT-Dateizeitangaben voraus (Zwei-Sekunden-Genauigkeit).        |
    |/Log:<Log File>     | Schreibt die Statusausgabe in die Protokolldatei (vorhandene Protokolldatei wird überschrieben).         |

    Mehrere Datenträger können parallel genutzt werden, wobei auf jedem Datenträger mehrere Aufträge ausgeführt werden. 

6. Überprüfen Sie den Kopierstatus, während sich der Auftrag in Bearbeitung befindet. Das folgende Beispiel enthält die Ausgabe des Robocopy-Befehls zum Kopieren von Dateien auf den Data Box-Datenträger.

    ```
    C:\Users>robocopy
        -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:53 PM
           Simple Usage :: ROBOCOPY source destination /MIR
    
                 source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                   /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:64 /E /R:1 /W:1 /FFT 
    -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:58 PM
       Source : C:\Git\azure-docs-pr\contributor-guide\
         Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
      Options : *.* /DCOPY:DA /COPY:DAT /MT:8 /R:1000000 /W:30
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                   Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
       Files :        17        17         0         0         0         0
       Bytes :     3.9 k     3.9 k         0         0         0         0
       Times :   0:00:05   0:00:00                       0:00:00   0:00:00
        
       Speed :                5620 Bytes/sec.
       Speed :               0.321 MegaBytes/min.
       Ended : Thursday, March 8, 2018 2:34:59 PM
        
    C:\Users>
    ```
 
    
7. Öffnen Sie den Zielordner, um die kopierten Dateien anzuzeigen und zu überprüfen. Laden Sie die Protokolldateien herunter, falls während des Kopierprozesses Fehler auftreten, um die Problembehandlung durchzuführen. Die Protokolldateien befinden sich an den Speicherorten, die für den Robocopy-Befehl angegeben wurden.
 


> [!IMPORTANT]
> - Sie sind dafür verantwortlich, die Daten in Ordner zu kopieren, die das richtige Datenformat aufweisen. Kopieren Sie beispielsweise die Blockblobdaten in den Ordner für Blockblobs. Falls das Datenformat nicht mit dem entsprechenden Ordner (Speichertyp) übereinstimmt, tritt für den Datenupload in Azure während eines späteren Schritts ein Fehler auf.
> -  Stellen Sie beim Kopieren der Daten sicher, dass für die Datengröße die Größenbeschränkungen eingehalten werden, die im Artikel zu den [Grenzwerten für Azure Storage und Data Box-Datenträger](data-box-disk-limits.md) beschrieben sind. 
> - Falls vom Data Box-Datenträger hochgeladene Daten gleichzeitig von anderen Anwendungen außerhalb des Data Box-Datenträgers hochgeladen werden, kann dies zu Fehlern bei Uploadaufträgen und zu Datenbeschädigungen führen.

## <a name="verify-data-integrity"></a>Überprüfen der Integrität von Daten

Führen Sie die folgenden Schritte aus, um die Datenintegrität zu überprüfen.

1. Führen Sie die Datei `AzureExpressDiskService.ps1` zur Überprüfung der Prüfsumme aus. Navigieren Sie im Datei-Explorer zum Ordner *AzureImportExport* des Laufwerks. Klicken Sie mit der rechten Maustaste, und wählen Sie **Mit PowerShell ausführen**. 

    ![Durchführen der Prüfsummenüberprüfung](media/data-box-disk-deploy-copy-data/data-box-disk-checksum.png)

2. Je nach Größe Ihrer Daten kann dieser Schritt eine Weile dauern. Eine Zusammenfassung der Überprüfung der Datenintegrität und die Dauer dieses Vorgangs werden angezeigt, nachdem das Skript abgeschlossen wurde. Sie können die **EINGABETASTE** drücken, um das Befehlsfenster zu beenden.

    ![Ausgabe der Prüfsumme](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

3. Führen Sie den Befehl für jeden Datenträger einzeln aus, wenn Sie mehrere Datenträger verwenden möchten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Datenträgern erhalten, z.B.:

> [!div class="checklist"]
> * Kopieren von Daten auf den Data Box-Datenträger
> * Überprüfen der Integrität von Daten

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Data Box-Datenträger zurücksenden und den Datenupload in Azure überprüfen.

> [!div class="nextstepaction"]
> [Zurücksenden Ihres Azure Data Box-Datenträgers an Microsoft](./data-box-disk-deploy-picked-up.md)

