---
title: Kopieren von Daten auf die Microsoft Azure Data Box | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten auf Ihre Azure Data Box kopieren.
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
ms.date: 09/25/2018
ms.author: alkohli
ms.openlocfilehash: 9bc84a9b08c4cfbdf7f24416c923e0dbd7076556
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161929"
---
# <a name="tutorial-copy-data-to-azure-data-box"></a>Tutorial: Kopieren von Daten auf die Azure Data Box 

In diesem Tutorial wird beschrieben, wie Sie über die lokale Webbenutzeroberfläche eine Verbindung mit Ihrem Hostcomputer herstellen, Daten kopieren und anschließend den Versand der Data Box vorbereiten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit der Data Box
> * Kopieren von Daten auf die Data Box
> * Vorbereiten der Data Box für den Versand

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben das [Tutorial zum Einrichten von Azure Data Box](data-box-deploy-set-up.md) durchgearbeitet.
2. Sie haben Ihre Data Box erhalten, und die Bestellung wird im Portal mit dem Status **Übermittelt** angezeigt.
3. Sie haben einen Hostcomputer mit den Daten, die Sie auf die Data Box kopieren möchten. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
    - Es muss ein [unterstütztes Betriebssystem](data-box-system-requirements.md) ausgeführt werden.
    - Er muss mit einem Hochgeschwindigkeitsnetzwerk verbunden sein. Es wird dringend empfohlen, dass mindestens eine 10-GbE-Verbindung verfügbar ist. Falls keine 10-GbE-Verbindung verfügbar ist, kann eine 1-GbE-Datenverbindung verwendet werden, die Geschwindigkeit der Kopiervorgänge wird dadurch jedoch beeinträchtigt. 

## <a name="connect-to-data-box"></a>Herstellen einer Verbindung mit der Data Box

Je nach ausgewähltem Speicherkonto erstellt Data Box bis zu:
- Drei Freigaben für jedes verknüpfte Speicherkonto für GPv1 und GPv2
- Eine Freigabe für ein Premium- oder BLOB-Speicherkonto 

Unter Blockblob- und Seitenblobfreigaben sind Entitäten der ersten Ebene Container, und Entitäten der zweiten Ebene sind Blobs. Unter Freigaben für Azure Files sind Entitäten erster Ebene Freigaben. Entitäten zweiter Ebene sind Dateien.

Betrachten Sie das folgende Beispiel. 

- Speicherkonto: *Mystoracct*
- Freigabe für Blockblob: *Mystoracct_BlockBlob/my-container/blob*
- Freigabe für Seitenblob: *Mystoracct_PageBlob/my-container/blob*
- Freigabe für Dateien: *Mystoracct_AzFile/my-share*

Je nachdem, ob Ihre Data Box mit einem Windows Server-Hostcomputer oder einem Linux-Host verbunden ist, können sich die Schritte zum Herstellen der Verbindung und Kopieren von Daten unterscheiden.

### <a name="connect-via-smb"></a>Herstellen einer Verbindung über SMB 

Wenn Sie einen Windows Server-Hostcomputer verwenden, führen Sie die folgenden Schritte aus, um eine Verbindung mit der Data Box herzustellen.

1. Zunächst müssen Sie sich authentifizieren und eine Sitzung starten. Navigieren Sie zu **Verbindung herstellen und Daten kopieren**. Klicken Sie auf **Get credentials** (Anmeldeinformationen abrufen), um die Anmeldeinformationen für den Zugriff auf die mit Ihrem Speicherkonto verknüpften Freigaben abzurufen. 

    ![Abrufen der Anmeldeinformationen für Freigaben 1](media/data-box-deploy-copy-data/get-share-credentials1.png)

2. Kopieren Sie im Dialogfeld „Auf Freigabe zugreifen und Daten kopieren“ den **Benutzernamen** und das **Kennwort** für die Freigabe. Klicken Sie auf **OK**.
    
    ![Abrufen der Anmeldeinformationen für Freigaben 1](media/data-box-deploy-copy-data/get-share-credentials2.png)

3. Greifen Sie auf die mit Ihrem Speicherkonto („Mystoracct“ im folgenden Beispiel) verknüpften Freigaben zu. Verwenden Sie den Pfad `\\<IP of the device>\ShareName`, um auf die Freigaben zuzugreifen. Stellen Sie je nach Datenformat unter einer der folgenden Adressen eine Verbindung mit den Freigaben her (verwenden Sie dazu den Freigabenamen): 
    - *\\<IP address of the device>\Mystoracct_Blob*
    - *\\<IP address of the device>\Mystoracct_Page*
    - *\\<IP address of the device>\Mystoracct_AzFile*
    
    Um auf Ihrem Hostcomputer eine Verbindung mit den Freigaben herzustellen, öffnen Sie ein Befehlsfenster. Geben Sie an der Eingabeaufforderung Folgendes ein:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Geben Sie das Kennwort für die Freigabe ein, wenn Sie dazu aufgefordert werden. Das folgende Beispiel zeigt das Herstellen einer Verbindung mit einer Freigabe über den obigen Befehl.

    ```
    C:\Users\Databoxuser>net use \\10.126.76.172\devicemanagertest1_BlockBlob /u:devicemanagertest1
    Enter the password for 'devicemanagertest1' to connect to '10.126.76.172':
    The command completed successfully.
    ```

4. Drücken Sie WINDOWS-TASTE+R. Geben Sie im Fenster **Ausführen** die `\\<device IP address>` an. Klicken Sie auf **OK**. Der Datei-Explorer wird geöffnet.
    
    ![Herstellen einer Verbindung mit der Freigabe über den Datei-Explorer 2](media/data-box-deploy-copy-data/connect-shares-file-explorer1.png)

5. Die Freigaben sollten jetzt als Ordner angezeigt werden. Erstellen Sie einen Ordner für die Dateien, die Sie kopieren möchten (in diesem Fall Vorlagen). Mitunter werden die Ordner mit einem grauen Kreuz angezeigt. Das Kreuz bedeutet nicht, dass ein Fehler vorliegt. Die Ordner werden zum Nachverfolgen des Status von der Anwendung gekennzeichnet.
    
    ![Herstellen einer Verbindung mit der Freigabe über den Datei-Explorer 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) ![Herstellen einer Verbindung mit der Freigabe über den Datei-Explorer 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) 

### <a name="connect-via-nfs"></a>Herstellen einer Verbindung über NFS 

Wenn Sie einen Linux-Hostcomputer verwenden, führen Sie die folgenden Schritte aus, um Data Box zum Zulassen des Zugriffs auf NFS-Clients zu konfigurieren.

1. Geben Sie die IP-Adressen der zulässigen Clients an, die auf die Freigabe zugreifen können. Wechseln Sie in der lokalen Webbenutzeroberfläche zur Seite **Verbindung herstellen und Daten kopieren**. Klicken Sie unter **NFS-Einstellungen** auf **NFS-Clientzugriff**. 

    ![Konfigurieren des NFS-Clientzugriffs 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. Geben Sie die IP-Adresse des NFS-Clients an, und klicken Sie auf **Hinzufügen**. Sie können den Zugriff für mehrere NFS-Clients konfigurieren, indem Sie diesen Schritt wiederholen. Klicken Sie auf **OK**.

    ![Konfigurieren des NFS-Clientzugriffs 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Stellen Sie sicher, dass auf dem Linux-Hostcomputer eine [unterstützte Version](data-box-system-requirements.md) des NFS-Clients installiert ist. Verwenden Sie die jeweilige Version für Ihre Linux-Distribution. 

3. Führen Sie nach der Installation des NFS-Clients den folgenden Befehl aus, um die NFS-Freigabe auf Ihrem Data Box-Gerät einzubinden:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    Das folgende Beispiel zeigt, wie Sie über NFS eine Verbindung mit einer Data Box-Freigabe herstellen. Die IP-Adresse des Data Box-Geräts lautet `10.161.23.130`, die Freigabe `Mystoracct_Blob` wird auf der Ubuntu-VM eingebunden, und der Bereitstellungspunkt ist `/home/databoxubuntuhost/databox`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`


## <a name="copy-data-to-data-box"></a>Kopieren von Daten auf die Data Box

Nachdem Sie eine Verbindung mit den Data Box-Freigaben hergestellt haben, kopieren Sie im nächsten Schritt Ihre Daten. Beachten Sie vor dem Kopieren von Daten Folgendes:

- Stellen Sie sicher, dass Sie die Daten in Freigaben kopieren, die das richtige Datenformat aufweisen. Kopieren Sie beispielsweise die Blockblobdaten in die Freigabe für Blockblobs. Wenn das Datenformat nicht mit dem entsprechenden Freigabetyp übereinstimmt, tritt später beim Hochladen der Daten in Azure ein Fehler auf.
-  Stellen Sie beim Kopieren der Daten sicher, dass für die Datengröße die Größenbeschränkungen eingehalten werden, die im Artikel zu den [Grenzwerten für Azure Storage und Data Box](data-box-limits.md) beschrieben sind. 
- Falls von Data Box hochgeladene Daten gleichzeitig von anderen Anwendungen außerhalb von Data Box hochgeladen werden, kann dies zu Fehlern bei Uploadaufträgen und zu Datenbeschädigungen führen.
- Es wird empfohlen, SMB und NFS nicht gleichzeitig zu verwenden und Daten nicht an dasselbe Endziel in Azure zu kopieren. In solchen Fällen kann das endgültige Ergebnis nicht bestimmt werden.

### <a name="copy-data-via-smb"></a>Kopieren von Daten über SMB

Nachdem Sie eine Verbindung mit der SMB-Freigabe hergestellt haben, starten Sie einen Datenkopiervorgang. 

Sie können jedes SMB-kompatible Dateikopiertool (beispielsweise Robocopy) verwenden, um Ihre Daten zu kopieren. Mit Robocopy können mehrere Kopieraufträge initiiert werden. Verwenden Sie den folgenden Befehl:
    
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile> 
  
 Die Attribute werden in der folgenden Tabelle beschrieben.
    
|Attribut  |BESCHREIBUNG  |
|---------|---------|
|/e     |Kopiert Unterverzeichnisse, einschließlich der leeren Verzeichnisse.         |
|/r:     |Gibt die Anzahl von Wiederholungsversuchen für fehlerhafte Kopiervorgänge an.         |
|/w:     |Gibt die Wartezeit zwischen Wiederholungen in Sekunden an.         |
|/is     |Schließt die gleichen Dateien ein.         |
|/nfl     |Gibt an, dass Dateinamen nicht protokolliert werden sollen.         |
|/ndl    |Gibt an, dass Verzeichnisnamen nicht protokolliert werden sollen.        |
|/np     |Gibt an, dass der Status des Kopiervorgangs (die Anzahl bisher kopierter Dateien oder Verzeichnisse) nicht angezeigt wird. Die Anzeige des Status beeinträchtigt die Leistung erheblich.         |
|/MT     | Verwendet Multithreading (empfohlen werden 32 oder 64 Threads). Diese Option wird nicht mit verschlüsselten Dateien verwendet. Sie müssen verschlüsselte und unverschlüsselte Dateien möglicherweise voneinander trennen. Das Kopieren mit nur einem Thread beeinträchtigt die Leistung jedoch erheblich.           |
|/fft     | Reduziert die Zeitstempelgranularität für alle Dateisysteme.        |
|/b     | Kopiert Dateien im Sicherungsmodus.        |
|/z    | Kopiert Dateien im Neustartmodus. Verwenden Sie diese Option, wenn die Umgebung instabil ist. Diese Option reduziert den Durchsatz durch eine zusätzliche Protokollierung.      |
| /zb     | Verwendet den Neustartmodus. Wenn der Zugriff verweigert wird, wird der Sicherungsmodus verwendet. Diese Option reduziert den Durchsatz durch das Setzen von Prüfpunkten.         |
|/efsraw     | Kopiert alle verschlüsselten Dateien im EFS-Raw-Modus. Verwenden Sie diese Option nur mit verschlüsselten Dateien.         |
|log+:<LogFile>| Fügt die Ausgabe an die vorhandene Protokolldatei an.|    
 
Das folgende Beispiel zeigt die Ausgabe des Robocopy-Befehls zum Kopieren von Dateien auf die Data Box.
    
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
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:32
    -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:58 PM
        Source : C:\Git\azure-docs-pr\contributor-guide\
            Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
        Options : *.* /DCOPY:DA /COPY:DAT /MT:32 /R:5 /W:60
    
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
    C:\Users>
       

Verwenden Sie zum Optimieren der Leistung die folgenden Robocopy-Parameter beim Kopieren der Daten.

|    Plattform    |    Überwiegend kleine Dateien < 512 KB                           |    Überwiegend mittelgroße Dateien 512 KB – 1 MB                      |    Überwiegend große Dateien > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 Robocopy-Sitzungen <br> 16 Threads pro Sitzung    |    3 Robocopy-Sitzungen <br> 16 Threads pro Sitzung    |    2 Robocopy-Sitzungen <br> 24 Threads pro Sitzung    |  |


Weitere Informationen zum Robocopy-Befehl finden Sie unter [Robocopy and a few examples](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx) (Robocopy und einige Beispiele).

Öffnen Sie den Zielordner, um die kopierten Dateien anzuzeigen und zu überprüfen. Falls während des Kopierprozesses Fehler auftreten, laden Sie zur Problembehandlung die Fehlerdateien herunter.

Um die Datenintegrität zu gewährleisten, wird inline eine Prüfsumme berechnet, während die Daten kopiert werden. Überprüfen Sie nach Abschluss des Kopiervorgangs den belegten Speicherplatz und den freien Speicherplatz auf Ihrem Gerät.
    
   ![Überprüfen des freien und belegten Speicherplatzes im Dashboard](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)

### <a name="copy-data-via-nfs"></a>Kopieren von Daten über NFS

Wenn Sie einen Linux-Hostcomputer verwenden, verwenden Sie ein Kopierhilfsprogramm wie Robocopy. Einige der verfügbaren Alternativen in Linux sind [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) oder [Ultracopier](https://ultracopier.first-world.info/).  

Der `cp`-Befehl ist eine der besten Optionen zum Kopieren eines Verzeichnisses. Weitere Informationen zur Verwendung finden Sie auf den [Handbuchseiten zum cp-Befehl](http://man7.org/linux/man-pages/man1/cp.1.html).

Befolgen Sie die nachstehenden Richtlinien, wenn Sie die rsync-Option für einen Multithread-Kopiervorgang verwenden:

 - Installieren Sie je nach Dateisystem, das Ihr Linux-Client verwendet, das **CIFS Utils**- oder **NFS Utils**-Paket.

    `sudo apt-get install cifs-utils``sudo apt-get install nfs-utils`

 -  Installieren Sie **Rsync** und **Parallel** (variiert abhängig von der Linux-Version).

    `sudo apt-get install rsync`
    `sudo apt-get install parallel` 

 - Erstellen Sie einen Bereitstellungspunkt.

    `sudo mkdir /mnt/databox`

 - Binden Sie das Volume ein.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

 - Spiegeln Sie die Verzeichnisstruktur des Ordners.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

 - Kopieren Sie Dateien. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     Hierbei gibt „j“ die Anzahl von Parallelisierungen und „X“ die Anzahl paralleler Kopien an.

     Wir empfehlen, mit 16 parallelen Kopien zu beginnen und die Anzahl von Threads je nach verfügbaren Ressourcen zu erhöhen.

## <a name="prepare-to-ship"></a>Vorbereiten des Versands

Der letzte Schritt ist die Vorbereitung des Geräts für den Versands. In diesem Schritt werden alle Gerätefreigaben offline geschaltet. Sobald Sie mit der Vorbereitung des Geräts für den Versand beginnen, ist der Zugriff auf die Freigaben nicht mehr möglich.
1. Wechseln Sie zu **Versandvorbereitung**, und klicken Sie auf **Vorbereitung starten**. 
   
    ![Versandvorbereitung 1](media/data-box-deploy-copy-data/prepare-to-ship1.png)

2. Prüfsummen werden standardmäßig inline während der Vorbereitung für den Versand berechnet. Die Berechnung der Prüfsumme dauert je nach Größe Ihrer Daten einige Zeit. Klicken Sie auf **Vorbereitung starten**.
    1. Die Gerätefreigaben werden offline geschaltet, und das Gerät wird gesperrt, wenn der Versand vorbereitet wird.
        
        ![Versandvorbereitung 1](media/data-box-deploy-copy-data/prepare-to-ship2.png) 
   
    2. Der Gerätestatus ändert sich in *Bereit für den Versand*, sobald die Vorbereitung des Geräts abgeschlossen ist. 
        
        ![Versandvorbereitung 1](media/data-box-deploy-copy-data/prepare-to-ship3.png)

    3. Laden Sie die Liste der Dateien (Manifest), die bei diesem Vorgang kopiert wurden, herunter. Sie können diese Liste später verwenden, um die in Azure hochgeladenen Dateien zu überprüfen.
        
        ![Versandvorbereitung 1](media/data-box-deploy-copy-data/prepare-to-ship4.png)

3. Fahren Sie das Gerät herunter. Wechseln Sie zur Seite **Herunterfahren oder neu starten**, und klicken Sie auf **Herunterfahren**. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **OK**, um fortzufahren.
4. Entfernen Sie die Kabel. Im nächsten Schritt versenden Sie das Gerät an Microsoft.

 
<!--## Appendix - robocopy parameters

This section describes the robocopy parameters used when copying the data to optimize the performance.

|    Platform    |    Mostly small files < 512 KB                           |    Mostly medium  files 512 KB-1 MB                      |    Mostly large files > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 Robocopy sessions <br> 16 threads per sessions    |    3 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 24 threads per sessions    |  |
|    Data Box Heavy     |    6 Robocopy sessions <br> 24 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |   
|    Data Box Disk         |    4 Robocopy sessions <br> 16 threads per sessions             |    2 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 16 threads per sessions    |   
-->

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit der Data Box
> * Kopieren von Daten auf die Data Box
> * Vorbereiten der Data Box für den Versand

Im nächsten Tutorial erfahren Sie, wie Sie die Einrichtung vornehmen und Daten auf Ihre Data Box kopieren.

> [!div class="nextstepaction"]
> [Zurücksenden der Azure Data Box an Microsoft](./data-box-deploy-picked-up.md)

