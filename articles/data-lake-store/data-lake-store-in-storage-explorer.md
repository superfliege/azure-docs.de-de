---
title: Verwalten von Azure Data Lake Store-Ressourcen in Azure Storage-Explorer
description: "Ermöglicht es Benutzern, auf Ihre ADLS-Daten und -Ressourcen in Azure Storage-Explorer zuzugreifen und sie zu verwalten."
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: 
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 4c69acebceac6719cf3f12ff0a93879f1b5a8943
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="manage-azure-data-lake-store-resources-with-storage-explorer-preview"></a>Verwalten von Azure Data Lake Store-Ressourcen per Storage-Explorer (Vorschauversion)

[Azure Data Lake Store (ADLS)](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview) ist ein Dienst zum Speichern großer Mengen von unstrukturierten Daten, z.B. als Text- oder Binärdaten. Benutzer können per HTTP oder HTTPS von jedem Ort aus auf die Daten zugreifen. Mit ADLS in Azure Storage-Explorer können Benutzer auf ADLS-Daten und -Ressourcen sowie andere Azure-Entitäten, z.B. Blobs und Warteschlangen, zugreifen und diese verwalten. Benutzer können ihre verschiedenen Azure-Entitäten nun mit demselben Tool an einem Ort verwalten.

Ein weiterer Vorteil ist, dass Benutzer zum Verwalten von ADLS-Daten keine Abonnementberechtigungen benötigen. In Storage-Explorer können Benutzer den ADLS-Pfad an den Knoten „Local and Attached“ (Lokal und angefügt) anfügen, sofern die Berechtigung von anderen Personen gewährt wird.

## <a name="prerequisites"></a>Voraussetzungen
Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

*   Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial).
*   Ein Azure Data Lake Store-Konto. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit Azure Data Lake Store über das Azure-Portal](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="installation"></a>Installation

Installieren Sie die neuesten Komponenten für Azure Storage-Explorer: [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/). Wir unterstützen nun eine Windows-, Linux- und Macintosh-Version.

## <a name="connect-to-an-azure-subscription"></a>Herstellen einer Verbindung mit einem Azure-Abonnement

1. Klicken Sie nach dem Installieren von **Azure Storage-Explorer** auf das Symbol **Plug-In** auf der linken Seite, wie in der folgenden Abbildung dargestellt.
       
   ![Plug-In-Symbol](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. Wählen Sie **Azure-Konto hinzufügen** aus, und klicken Sie dann auf **Anmelden**.

   ![Herstellen einer Verbindung mit einem Azure-Abonnement](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. Wählen Sie im Dialogfeld **Azure-Anmeldung** die Option **Anmelden** aus, und geben Sie Ihre Azure-Anmeldeinformationen ein.

    ![Anmelden](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Wählen Sie Ihr Abonnement in der Liste aus, und klicken Sie dann auf **Anwenden**.

    ![Anwenden](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    Der Explorer-Bereich wird aktualisiert und zeigt die Konten im ausgewählten Abonnement an.

    ![Kontoliste](./media/data-lake-store-in-storage-explorer/account-list.png)

    Sie haben Ihre **Azure Data Lake Store**-Instanz nun mit Ihrem Azure-Abonnement verbunden.

## <a name="connect-to-data-lake-store"></a>Herstellen einer Verbindung mit dem Data Lake-Speicher
Wenn Sie auf Ressourcen zugreifen möchten, die in Ihrem Abonnement nicht vorhanden sind, müssen andere Personen Ihnen die Berechtigung zum Abrufen des URI für die Ressourcen gewähren. In diesem Fall können Sie eine Verbindung mit der Data Lake Store-Instanz herstellen, indem Sie nach dem Anmelden den URI verwenden. Führen Sie die folgenden Schritte aus.
1. Öffnen Sie den Storage-Explorer (Vorschau).
2. Erweitern Sie im linken Bereich die Option **Local and Attached** (Lokal und angefügt).
3. Klicken Sie mit der rechten Maustaste auf **Data Lake Store**, und wählen Sie im Kontextmenü die Option **Connect to Data Lake Store...** (Verbindung mit Data Lake Store herstellen...).

      ![Kontextmenü „Connect to Data Lake Store...“ (Verbindung mit Data Lake Store herstellen...)](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Geben Sie den URI ein. Das Tool navigiert zum gerade eingegebenen URL-Speicherort.

      ![Dialogfeld „Connect to Data Lake Store...“ (Verbindung mit Data Lake Store herstellen...)](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![Verbindung mit Data Lake Store herstellen – Ergebnis](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>Anzeigen des Inhalts eines Azure Data Lake Store-Kontos
Zu den Ressourcen eines Azure Data Lake Store-Kontos gehören Ordner und Dateien.

Die folgenden Schritte veranschaulichen, wie Sie den Inhalt eines ADLS-Kontos in Storage-Explorer (Vorschauversion) anzeigen:

1. Öffnen Sie den Storage-Explorer (Vorschau).
2. Erweitern Sie im linken Bereich das Abonnement mit dem Azure Data Lake Store-Konto, das Sie anzeigen möchten.
3. Erweitern Sie **Data Lake Store**.
4. Klicken Sie mit der rechten Maustaste auf den Knoten mit dem gewünschten Azure Data Lake Store-Konto, und wählen Sie im Kontextmenü die Option **Öffnen**. Sie können auch auf das ADLS-Konto doppelklicken, um es zu öffnen. 
5. Im Hauptbereich wird der Inhalt des ADLS-Kontos angezeigt.

     ![Hauptbereich](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="azure-data-lake-store-resources-management"></a>Azure Data Lake Store-Ressourcenverwaltung

Sie können Azure Data Lake Store-Ressourcen verwalten, indem Sie die folgenden Vorgänge durchführen:
*   Navigieren Sie zu ADLS-Ressourcen unter mehreren ADL-Konten.  
*   Verwenden Sie eine Verbindungszeichenfolge, um eine Verbindung mit ADLS herzustellen und ADLS direkt zu verwalten. 
*   Zeigen Sie von anderen Benutzern freigegebene ADLS-Ressourcen per ACL unter „Local and Attached“ (Lokal und angefügt) an.
*   Führen Sie CRUD-Vorgänge für Dateien bzw. Ordner durch: Unterstützung von rekursiven Ordnern und Mehrfachauswahl von Dateien. 
*   Fügen Sie Ordner per Drag & Drop dem Schnellzugriff und den zuletzt verwendeten Speicherorten hinzu. Dies entspricht der Verwendung des Datei-Explorers auf dem Desktop. 
*   Führen Sie das Kopieren und Öffnen eines ADL-Hyperlinks mit Storage-Explorer mit nur einem Klick durch. 
*   Zeigen Sie das Aktivitätsprotokoll im unteren rechten Bereich an, um den Aktivitätsstatus einzublenden.
*   Zeigen Sie die Ordnerstatistiken und Dateieigenschaften an.

## <a name="manage-resources-in-azure-storage-explorer"></a>Verwalten von Ressourcen in Azure Storage-Explorer
Nachdem Sie ein Azure Data Lake Store-Konto erstellt haben, können Sie Ordner und Dateien hochladen und Ressourcen auf Ihren lokalen Computer herunterladen und öffnen. Außerdem können Sie die Optionen „An Schnellzugriff anheften“, „Neuer Ordner“, „URL kopieren“ und „Alle auswählen“ verwenden. Weitere Optionen sind „Kopieren“, „Einfügen“, „Umbenennen“, „Löschen“, „Ordnerstatistiken“ und „Aktualisieren“.

Im Folgenden wird veranschaulicht, wie Sie Ressourcen in einem Azure Data Lake Store-Konto verwalten. Führen Sie die folgenden Schritte für die gewünschte Aufgabe aus:
   * **Hochladen von Dateien**

     1. Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Hochladen** und dann im Dropdownmenü die Option **Dateien hochladen...**.

        ![Menü „Dateien hochladen“](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

     2. Wählen Sie im Dialogfeld **Dateien zum Hochladen auswählen** die Dateien aus, die Sie hochladen möchten.

        ![Dialogfeld „Upload folder“ (Ordner hochladen)](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

     3. Wählen Sie **Öffnen**, um den Upload zu starten.
   * **Hochladen eines Ordners**

     1. Wählen Sie auf der Symbolleiste des Hauptbereichs die Option **Hochladen** und dann im Dropdownmenü die Option **Upload Folder...** (Ordner hochladen...).

        ![Menü „Upload folder“ (Ordner hochladen)](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     2. Wählen Sie im Dialogfeld **Select folder to upload** (Ordner für Upload auswählen) einen Ordner aus, den Sie hochladen möchten.

        ![Dialogfeld „Upload folder“ (Ordner hochladen)](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

     3. Wählen Sie die Option **Ordner auswählen**, um den Upload von Ordnern zu starten.

        ![Dialogfeld „Upload folder“ (Ordner hochladen)](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

      > [!NOTE] 
          > Sie können die Ordner und Dateien direkt auf Ihren lokalen Computer ziehen, um den Uploadvorgang zu implementieren. 
       
   * **Herunterladen von Ordnern oder Dateien auf Ihren lokalen Computer**

     1. Wählen Sie die Ordner oder Dateien aus, die Sie herunterladen möchten.
     2. Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Herunterladen**.
     3. Geben Sie im Dialogfeld **Select a folder to save the downloaded files into** (Ordner zum Speichern heruntergeladener Dateien auswählen) den Speicherort an, an den die Ordner oder Dateien heruntergeladen werden sollen. Geben Sie außerdem den gewünschten Namen an.
     4. Wählen Sie **Speichern**aus.
   * **Öffnen eines Ordners oder einer Datei von Ihrem lokalen Computer**

     1. Wählen Sie den zu öffnenden Ordner oder die Datei aus.
     2. Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Öffnen**, oder klicken Sie mit der rechten Maustaste auf den ausgewählten Ordner bzw. die Datei und dann im Kontextmenü auf **Öffnen**.
     3. Die Datei wird heruntergeladen und mit der Anwendung geöffnet, die dem zugrunde liegenden Dateityp der Datei zugeordnet ist. Ein Ordner wird im Hauptbereich geöffnet.

        ![Öffnen der Datei](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

   * **Kopieren von Ordnern oder Dateien in die Zwischenablage**

     1. Wählen Sie die Ordner oder Dateien aus, die Sie kopieren möchten.
     2. Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Kopieren**, oder klicken Sie mit der rechten Maustaste auf die ausgewählten Ordner bzw. Dateien und dann im Kontextmenü auf **Kopieren**.
     3. Navigieren Sie im linken Bereich zu einem anderen ADLS-Konto, und doppelklicken Sie darauf, um es im Hauptbereich anzuzeigen.
     4. Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Einfügen**, um eine Kopie zu erstellen. Oder klicken Sie für das Ziel im Kontextmenü auf **Einfügen**.

        ![Ordner oder Datei kopieren/einfügen](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

      > [!NOTE] 
          > + Das übergreifende Kopieren und Einfügen für Speichertypen wird **nicht** unterstützt. Sie können ADLS-Ordner oder -Dateien kopieren und in ein anderes ADLS-Konto einfügen. Es ist aber **nicht** möglich, ADLS-Ordner oder -Dateien zu kopieren und in BLOB-Speicher einzufügen (oder umgekehrt). 
          > + Das Kopieren und Einfügen funktioniert, indem die Ordner oder Dateien auf den lokalen Computer herunter- und dann auf das Ziel hochgeladen werden. Mit dem Tool wird **nicht** die Aktion auf dem Back-End durchgeführt. Das Kopieren und Einfügen für große Dateien dauert lange. Das Kopieren und Verschieben von Dateien mit hoher Leistung wird derzeit optimiert.
   * **Löschen von Ordnern und Dateien**

     1. Wählen Sie die Ordner oder Dateien aus, die Sie löschen möchten.
     2. Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Löschen**, oder klicken Sie mit der rechten Maustaste auf die ausgewählten Ordner bzw. Dateien und dann im Kontextmenü auf **Löschen**.
     3. Wählen Sie im Bestätigungsdialogfeld die Option **Ja**.

        ![Ordner oder Datei kopieren/einfügen](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

   * **An Schnellzugriff anheften**

     1. Wählen Sie den Ordner aus, den Sie anheften möchten.
     2. Wählen Sie in der Symbolleiste im Hauptbereich die Option **An Schnellzugriff anheften**.
     3. Im linken Bereich sehen Sie, dass der ausgewählte Ordner dem Knoten **Schnellzugriff** hinzugefügt wurde.

        ![An Schnellzugriff anheften](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)
     4. Nach der Erstellung des Schnellzugriffs können Sie leicht auf die Ressourcen zugreifen.
   * **Deep-Links**
     1. Wenn Sie über eine URL verfügen, können Sie sie im **Datei-Explorer** oder Browser einfach als Adresspfad eingeben.
     2. Anschließend wird **Storage Explorer.exe** automatisch gestartet, um zu dem URL-Speicherort zu navigieren, den Sie gerade eingegeben haben.

        ![Deep-Link im Datei-Explorer](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich die [neuesten Versionsanmerkungen und Videos zum Storage-Explorer (Vorschau)](http://www.storageexplorer.com) an.
* Erfahren Sie, wie Sie [Azure Cosmos DB in Azure Storage-Explorer verwalten](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer).
* Informieren Sie sich unter [Erste Schritte mit dem Speicher-Explorer (Vorschau)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer) über Storage-Explorer.
* Informieren Sie sich unter [Übersicht über Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview) über die ersten Schritte mit Azure Data Lake Store (ADLS).
* Sehen Sie sich das folgende Video zur [Verwendung von Azure Cosmos DB in Azure Storage-Explorer](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be) an.
