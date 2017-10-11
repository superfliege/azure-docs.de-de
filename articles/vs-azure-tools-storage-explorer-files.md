---
title: Azure-Dateispeicher mit Speicher-Explorer (Vorschau) | Microsoft Docs
description: Erfahren Sie, wie erfahren Sie, wie Speicher-Explorer (Preview) zum Arbeiten mit freigegebenen Ordner und Dateien zu verwenden.
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: cawa
ms.openlocfilehash: 964691758254531cb92a5b1cbe055ef61d25dba8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="using-storage-explorer-preview-with-azure-file-storage"></a>Verwenden von Speicher-Explorer (Preview) mit Azure-Dateispeicher

Azure-Dateispeicher ist ein Dienst, der Dateifreigaben in der Cloud mit dem standard Server Message Block (SMB)-Protokoll bietet. SMB 2.1 und SMB 3.0 werden unterstützt. Mit Azure-Dateispeicher können Sie ältere Anwendungen migrieren, die Dateifreigaben in Azure schnell und ohne kostspielige schreibt benötigen. Sie können Dateispeicher verwenden, um Daten öffentlich auf der ganzen Welt verfügbar zu machen oder um Anwendungsdaten privat zu speichern. In diesem Artikel erfahren Sie, wie Speicher-Explorer (Preview) zur Bearbeitung von Dateifreigaben und Dateien verwendet werden.

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

- [Herunterladen Sie und installieren Sie die Speicher-Explorer (Preview)](http://www.storageexplorer.com/)

- [Herstellen einer Verbindung mit einer Azure-Speicherkonto oder einen Dienst](https://docs.microsoft.com//azure/vs-azure-tools-storage-manage-with-storage-explorer#connect-to-a-storage-account-or-service)

## <a name="create-a-file-share"></a>Erstellen Sie eine Dateifreigabe

Alle Dateien müssen in einer Dateifreigabe befinden, die einfach eine logische Gruppierung von Dateien ist. Ein Konto kann eine unbegrenzte Anzahl von Dateifreigaben enthalten, und jede Freigabe kann eine unbegrenzte Anzahl von Dateien gespeichert.

Die folgenden Schritte veranschaulichen das Erstellen einer Dateifreigabe im Speicher-Explorer (Preview).

1. Öffnen Sie die Speicher-Explorer (Preview).

2. Erweitern Sie im linken Bereich das Speicherkonto an, anhand derer Sie die Dateifreigabe erstellen möchten

3. Mit der rechten Maustaste **Dateifreigaben**, "und"-Wählen Sie aus dem Kontextmenü - **Dateifreigabe erstellen**.

    ![Dateifreigabe erstellen](media/vs-azure-tools-storage-explorer-files/image1.png)

4. Ein Textfeld wird angezeigt, unter dem **Dateifreigaben** Ordner. Geben Sie den Namen für die Dateifreigabe ein. Finden Sie unter der [freigeben Benennungsregeln](https://docs.microsoft.com//azure/storage/storage-dotnet-how-to-use-blobs#create-a-container) Abschnitt für eine Liste von Regeln und Einschränkungen zum Benennen von Dateifreigaben.

    ![Benennen die Freigabe](media/vs-azure-tools-storage-explorer-files/image2.png)

5. Drücken Sie **EINGABETASTE** Abschluss, um die Dateifreigabe zu erstellen oder **Esc** auf "Abbrechen". Sobald die Dateifreigabe wurde erfolgreich erstellt wurde, wird angezeigt, unter dem **Dateifreigaben** Ordner für das ausgewählte Speicherkonto an.

    ![Die neue Freigabe](media/vs-azure-tools-storage-explorer-files/image3.png)

## <a name="view-a-file-shares-contents"></a>Zeigen Sie eine Dateifreigabe-Inhalt

Dateifreigaben enthalten Dateien und Ordnern (die auch Dateien enthalten können).

Die folgenden Schritte veranschaulichen, wie Sie zum Anzeigen des Inhalts einer Dateifreigabe im Speicher-Explorer (Preview): +

1. Öffnen Sie die Speicher-Explorer (Preview).

2. Erweitern Sie im linken Bereich Speicherkontos mit der Dateifreigabe, dass Sie anzeigen möchten.

3. Erweitern Sie des Speicherkontos **Dateifreigaben**.

4. Mit der rechten Maustaste in der Dateifreigabe, die Sie anzeigen möchten, und - wählen Sie aus dem Kontextmenü - **öffnen**. Sie können auch die Dateifreigabe doppelklicken, die Sie anzeigen möchten.

    ![Öffnen der Freigabe](media/vs-azure-tools-storage-explorer-files/image4.png)

5. Im Hauptfenster werden die Dateifreigabe Inhalt angezeigt.
    
    ![Die Freigabe von Inhalt](media/vs-azure-tools-storage-explorer-files/image5.png)

## <a name="delete-a-file-share"></a>Löschen Sie eine Dateifreigabe

Dateifreigaben können einfach erstellt und nach Bedarf gelöscht werden. (Informationen zum Löschen einzelner Dateien finden Sie im Abschnitt [zum Verwalten von Dateien in einer Dateifreigabe](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie eine Dateifreigabe in Speicher-Explorer (Preview) zu löschen:

1. Öffnen Sie die Speicher-Explorer (Preview).

2. Erweitern Sie im linken Bereich Speicherkontos mit der Dateifreigabe, dass Sie anzeigen möchten.

3. Erweitern Sie des Speicherkontos **Dateifreigaben**.

4. Mit der rechten Maustaste in der Dateifreigabe, die Sie verwenden möchten, löschen und - wählen Sie aus dem Kontextmenü - **löschen**. Sie können auch die Taste **löschen** zum Entfernen der Freigabe der aktuell ausgewählten Datei.

    ![Löschen](media/vs-azure-tools-storage-explorer-files/image6.png)

5. Wählen Sie **Ja** zum Dialogfeld "Bestätigung".
    
    ![Bestätigungsdialogfeld](media/vs-azure-tools-storage-explorer-files/image7.png)

## <a name="copy-a-file-share"></a>Kopieren Sie eine Dateifreigabe

Speicher-Explorer (Preview) können Sie eine Dateifreigabe in die Zwischenablage kopieren, und fügen dann die Dateifreigabe in ein anderes Speicherkonto. (Informationen zum Kopieren der einzelner Dateien, finden Sie im Abschnitt [zum Verwalten von Dateien in einer Dateifreigabe](https://docs.microsoft.com//azure/vs-azure-tools-storage-explorer-blobs#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie eine Dateifreigabe aus einem Speicherkonto in ein anderes kopiert wird.

1. Öffnen Sie die Speicher-Explorer (Preview).

2. Erweitern Sie im linken Bereich Speicherkontos mit der Dateifreigabe, dass Sie kopieren möchten.

3. Erweitern Sie des Speicherkontos **Dateifreigaben**.

4. Mit der rechten Maustaste in der Dateifreigabe, die Sie verwenden möchten, kopieren und - wählen Sie aus dem Kontextmenü - **Kopie Dateifreigabe**.

    ![Kopieren Sie die Dateifreigabe](media/vs-azure-tools-storage-explorer-files/image8.png)

5. Mit der rechten Maustaste in des gewünschte "Target" Speicherkonto, in die Sie einfügen möchten die Dateifreigabe, und - wählen Sie aus dem Kontextmenü - **Dateifreigabe einfügen**.

    ![Fügen Sie die Dateifreigabe](media/vs-azure-tools-storage-explorer-files/image9.png)

## <a name="get-the-sas-for-a-file-share"></a>Rufen Sie die SAS für eine Dateifreigabe

Ein [freigegebene zugriffssignatur (SAS)](https://docs.microsoft.com//azure/storage/storage-dotnet-shared-access-signature-part-1) bietet delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Dies bedeutet, dass Sie, dass ein Client Berechtigungen für Objekte in Ihrem Speicherkonto für einen bestimmten Zeitraum Zeit und mit den angegebenen Berechtigungssatz beschränkt erteilen können, ohne Ihre Zugriffsschlüssel freigeben.

Die folgenden Schritte veranschaulichen, wie eine SAS für eine Dateifreigabe erstellen: +

1. Öffnen Sie die Speicher-Explorer (Preview).

2. Erweitern Sie im linken Bereich Speicherkontos mit der Dateifreigabe für die Sie eine SAS abrufen möchten.

3. Erweitern Sie des Speicherkontos **Dateifreigaben**.

4. Mit der rechten Maustaste in der gewünschte Dateifreigabe und - wählen Sie aus dem Kontextmenü - **freigegebene Zugriffssignatur**.

    ![Abrufen der Shared Access Signature](media/vs-azure-tools-storage-explorer-files/image10.png)

5. In der **Shared Access Signature** Dialogfeld, geben Sie die Richtlinie, die Start-und Ablaufdaten, die Zeitzone und Zugriffsebenen für die Ressource verwendet werden sollen.

    ![SAS-Dialogfeld](media/vs-azure-tools-storage-explorer-files/image11.png)

6. Wählen Sie abschließend die SAS-Optionen angeben, **erstellen**.

7. Ein zweites **Shared Access Signature** Dialogfeld wird dann angezeigt, die die Datei enthält freigeben, zusammen mit der URL und der Formularwerte, die auf die Speicherressource zugreifen können. Wählen Sie **Kopie** neben der URL, die Sie in die Zwischenablage kopieren möchten.
    
    ![Zweite SAS-Dialogfeld](media/vs-azure-tools-storage-explorer-files/image12.png)

8. Aus und klicken Sie **schließen**.

## <a name="manage-access-policies-for-a-file-share"></a>Verwalten von Richtlinien für den Zugriff für eine Dateifreigabe

Die folgenden Schritte veranschaulichen, wie verwalten (hinzufügen und entfernen) Zugriffsrichtlinien für eine Dateifreigabe: +. Die Zugriffsrichtlinien dient zum Erstellen von SAS-URLs, die über die Benutzer Zugriff auf die Speicherdatei Ressource während eines definierten Zeitraums Zeit verwenden können.

1. Öffnen Sie die Speicher-Explorer (Preview).

2. Erweitern Sie im linken Bereich Speicherkontos mit der Dateifreigabe, deren Zugriffsrichtlinien, die Sie verwalten möchten.

3. Erweitern Sie des Speicherkontos **Dateifreigaben**.

4. Wählen Sie die gewünschte Dateifreigabe und - aus dem Kontextmenü - **Zugriffsrichtlinien verwalten**.

    ![Kontextmenü für den Zugriff Richtlinien verwalten](media/vs-azure-tools-storage-explorer-files/image13.png)

5. Die **Zugriffsrichtlinien** Dialogfeld listet alle Zugriffsrichtlinien für die ausgewählte Datei-Freigabe bereits erstellt.
    
    ![Zugriffsrichtlinien](media/vs-azure-tools-storage-explorer-files/image14.png)

6. Führen Sie folgende Schritte aus, je nach dem Zugriff Richtlinie die Dateiverwaltungsaufgabe aus:
    
    - **Hinzufügen eine neuen Zugriffsrichtlinie** : Wählen Sie aus **hinzufügen**. Einmal generiert, die **Zugriffsrichtlinien** Dialogfeld zeigt die neu hinzugefügte Zugriffsrichtlinie (mit den Standardeinstellungen).

    - **Bearbeiten Sie eine Zugriffsrichtlinie** : Nehmen Sie alle gewünschten Änderungen vor, und wählen Sie **speichern**.

    - **Entfernen Sie eine Zugriffsrichtlinie** : Wählen Sie aus **entfernen** neben der Zugriffsrichtlinie, die Sie entfernen möchten.

7. Erstellen Sie eine neue SAS-URL, die mit der Zugriffsrichtlinie, die Sie zuvor erstellt haben:
    
    ![Abrufen von SAS](media/vs-azure-tools-storage-explorer-files/image15.png)
    
    ![SAS-Namen und Eigenschaften](media/vs-azure-tools-storage-explorer-files/image16.png)

## <a name="managing-files-in-a-file-share"></a>Verwalten von Dateien in einer Dateifreigabe

Nach der Erstellung einer Dateifreigabe können Sie Hochladen einer Datei in die Dateifreigabe, eine Datei auf Ihren lokalen Computer herunterladen, öffnen Sie eine Datei auf dem lokalen Computer und vieles mehr.

Die folgenden Schritte veranschaulichen, wie zum Verwalten von Dateien (und Ordner) in eine Dateifreigabe.

1.  Öffnen Sie die Speicher-Explorer (Preview).

2.  Erweitern Sie im linken Bereich Speicherkontos mit der Dateifreigabe, die Sie verwalten möchten.

3.  Erweitern Sie des Speicherkontos **Dateifreigaben**.

4.  Doppelklicken Sie auf die Dateifreigabe, die Sie anzeigen möchten.

5.  Im Hauptfenster werden die Dateifreigabe Inhalt angezeigt.

    ![Die Freigabe von Inhalt](media/vs-azure-tools-storage-explorer-files/image17.png)

6.  Im Hauptfenster werden die Dateifreigabe Inhalt angezeigt.

7.  Führen Sie folgende Schritte aus, abhängig von der Aufgabe, die Sie ausführen möchten:

    - **Hochladen von Dateien in einer Dateifreigabe**

        ein.  Wählen Sie auf der Symbolleiste im Hauptbereich **hochladen**, und klicken Sie dann **Dateien hochladen** aus dem Dropdown-Menü.

        ![Hochladen von Dateien](media/vs-azure-tools-storage-explorer-files/image18.png)
        
        b. In der **Hochladen von Dateien** Dialogfeld Wählen Sie die Auslassungspunkte (**...** ) auf der rechten Seite der Schaltfläche den **Dateien** Textfeld, um die Dateien wählen Sie hochladen möchten.

        ![Hinzufügen von Dateien](media/vs-azure-tools-storage-explorer-files/image19.png)

        c. Wählen Sie **hochladen**.

    - **Hochladen eines Ordners an eine Dateifreigabe**
        
        ein. Wählen Sie auf der Symbolleiste im Hauptbereich **hochladen**, und klicken Sie dann **Ordner hochladen** aus dem Dropdown-Menü.

        ![Menü Ordner hochladen](media/vs-azure-tools-storage-explorer-files/image20.png)

        b. In der **Ordner Upload** Dialogfeld Wählen Sie die Auslassungspunkte (**...** ) auf der rechten Seite der Schaltfläche den **Ordner** Textfeld, um den Ordner auszuwählen, deren Inhalt Sie hochladen möchten.

        c. Geben Sie optional einen Zielordner, in dem Inhalt des ausgewählten Ordners hochgeladen werden. Wenn der Zielordner vorhanden ist, wird sie erstellt.

        d. Wählen Sie **hochladen**.

    - **Eine Datei auf Ihren lokalen Computer herunterladen.**
        
        ein. Wählen Sie die Datei, die Sie herunterladen möchten.
        
        b. Wählen Sie auf der Symbolleiste im Hauptbereich **herunterladen**.
        
        c. In der **angeben, wo die heruntergeladene Datei gespeichert** Dialogfeld, geben Sie den Speicherort die heruntergeladenen Datei und den Namen, die sie gewähren möchten.

        d. Wählen Sie **Speichern** aus.

    - **Öffnen Sie eine Datei auf dem lokalen computer**
        
        ein.  Wählen Sie die Datei, die Sie öffnen möchten.
        
        b.  Wählen Sie auf der Symbolleiste im Hauptbereich **öffnen**.
        
        c.  Die Datei wird heruntergeladen und mit der zugrunde liegenden Datei Dateityp zugeordneten Anwendung geöffnet.

    - **Eine Datei in die Zwischenablage kopieren**

        ein. Wählen Sie die Datei, die Sie kopieren möchten.

        b. Wählen Sie auf der Symbolleiste im Hauptbereich **Kopie**.

        c. Klicken Sie im linken Bereich auf eine andere Dateifreigabe navigieren Sie, und doppelklicken Sie darauf, um es im Hauptbereich anzuzeigen.

        d. Wählen Sie auf der Symbolleiste im Hauptbereich **einfügen** eine Kopie der Datei erstellt.

    - **Löschen einer Datei**

        ein. Wählen Sie die Datei, die Sie löschen möchten.

        b. Wählen Sie auf der Symbolleiste im Hauptbereich **löschen**.

        c. Wählen Sie **Ja** zum Dialogfeld "Bestätigung".

## <a name="next-steps"></a>Nächste Schritte

- Anzeigen der [neuesten Versionshinweise Speicher-Explorer (Preview) und Videos](http://www.storageexplorer.com/).

- Erfahren Sie, wie [Erstellen von Anwendungen mit Azure-Blobs, Tabellen, Warteschlangen und Dateien](https://azure.microsoft.com/documentation/services/storage/).
