---
title: Verwalten von Azure-Dateifreigaben mit Azure Storage-Explorer
description: Es wird beschrieben, wie Sie Azure Storage-Explorer zum Verwalten von Azure Files verwenden.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2018
ms.author: wgries
ms.openlocfilehash: ea401099ce90248be0ecb99109b845ff9a91f70d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308496"
---
# <a name="manage-azure-file-shares-with-azure-storage-explorer"></a>Verwalten von Azure-Dateifreigaben mit Azure Storage-Explorer 
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. In diesem Artikel werden die Grundlagen der Arbeit mit Azure-Dateifreigaben per [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) Schritt für Schritt beschrieben. Storage-Explorer ist ein beliebtes Clienttool, das für Windows, macOS und Linux verfügbar ist. Sie können Storage-Explorer zum Verwalten von Azure-Dateifreigaben und anderen Speicherressourcen verwenden.

Für diese Schnellstartanleitung muss Storage-Explorer installiert sein. Sie können die Anwendung unter [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) herunterladen und dann installieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe und eines Speicherkontos
> * Erstellen einer Azure-Dateifreigabe 
> * Erstellen eines Verzeichnisses
> * Hochladen einer Datei
> * Herunterladen einer Datei
> * Erstellen und Verwenden einer Freigabemomentaufnahme

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Sie können Storage-Explorer nicht verwenden, um neue Ressourcen zu erstellen. Erstellen Sie das Speicherkonto für diese Demo über das [Azure-Portal](https://portal.azure.com/). 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connect-storage-explorer-to-azure-resources"></a>Herstellen einer Verbindung von Storage-Explorer mit Azure-Ressourcen
Nach dem erstmaligen Starten von Storage-Explorer wird das Fenster **Microsoft Azure Storage-Explorer – Verbinden** angezeigt. Es gibt verschiedene Möglichkeiten, wie Sie den Storage-Explorer mit Speicherkonten verbinden können: 

- **Anmelden mit Ihrem Azure-Konto**: Sie können sich anmelden, indem Sie die Benutzeranmeldeinformationen für Ihre Organisation oder Ihr Microsoft-Konto verwenden. 
- **Herstellen einer Verbindung mit einem bestimmten Speicherkonto per Verbindungszeichenfolge oder SAS-Token**: Eine Verbindungszeichenfolge ist eine spezielle Zeichenfolge, die einen Speicherkontonamen und -schlüssel bzw. ein SAS-Token enthält. Mit dem Token greift Storage-Explorer direkt auf das Speicherkonto zu (anstelle der Anzeige aller Speicherkonten eines Azure-Kontos). Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
- **Herstellen einer Verbindung mit einem bestimmten Speicherkonto über einen Speicherkontonamen und -schlüssel**: Verwenden Sie den Speicherkontonamen und -schlüssel, um für Ihr Speicherkonto eine Verbindung mit Azure-Speicher herzustellen.

Melden Sie sich für diese Schnellstartanleitung mit Ihrem Azure-Konto an. Wählen Sie **Azure-Konto hinzufügen** und dann **Anmelden**. Folgen Sie den Aufforderungen auf dem Bildschirm, um sich an Ihrem Azure-Konto anzumelden.

![Screenshot: Fenster „Microsoft Azure Storage-Explorer – Verbinden“](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe
Gehen Sie wie folgt vor, um Ihre erste Azure-Dateifreigabe unter dem Speicherkonto *storageacct<random number>* zu erstellen:

1. Erweitern Sie das Speicherkonto, das Sie erstellt haben.
2. Klicken Sie mit der rechten Maustaste auf **Dateifreigaben**, und wählen Sie anschließend **Dateifreigabe erstellen**.  
    ![Screenshot: Ordner mit Dateifreigaben und Kontextmenü im Kontext](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Geben Sie für die Dateifreigabe *myshare* ein, und drücken Sie die EINGABETASTE.

> [!IMPORTANT]  
> Freigabenamen dürfen nur Kleinbuchstaben, Zahlen und einzelne Bindestriche enthalten (ein Bindestrich am Anfang ist nicht zulässig). Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

Nachdem die Dateifreigabe erstellt wurde, wird im rechten Bereich eine Registerkarte für Ihre Dateifreigabe geöffnet. 

## <a name="work-with-the-contents-of-an-azure-file-share"></a>Verwenden des Inhalts einer Azure-Dateifreigabe
Nachdem Sie nun eine Azure-Dateifreigabe erstellt haben, können Sie die Dateifreigabe mit SMB unter [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) oder [macOS](storage-how-to-use-files-mac.md) bereitstellen. Alternativ hierzu können Sie mit Ihrer Azure-Dateifreigabe arbeiten, indem Sie die Azure CLI verwenden. Die Nutzung der Azure CLI hat gegenüber einer Bereitstellung der Dateifreigabe per SMB den Vorteil, dass für alle Anforderungen, die per Azure CLI gesendet werden, die Datei-REST-API genutzt wird. Sie können die Datei-REST-API zum Erstellen, Ändern und Löschen von Dateien und Verzeichnissen auf Clients verwenden, die nicht über SMB-Zugriff verfügen.

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Mit dem Hinzufügen eines Verzeichnisses wird eine hierarchische Struktur zum Verwalten Ihrer Dateifreigabe bereitgestellt. Sie können in Ihrem Verzeichnis mehrere Ebenen erstellen. Allerdings müssen Sie sicherstellen, dass übergeordnete Verzeichnisse vorhanden sind, bevor Sie Unterverzeichnisse erstellen können. Für den Pfad „myDirectory/mySubDirectory“ müssen Sie beispielsweise zuerst das Verzeichnis *myDirectory* erstellen. Erstellen Sie anschließend *mySubDirectory*. 

1. Wählen Sie auf der Registerkarte für die Dateifreigabe im obersten Menü die Schaltfläche **Neuer Ordner**. Der Bereich **Neues Verzeichnis erstellen** wird geöffnet.
    ![Screenshot: Schaltfläche „Neuer Ordner“ im Kontext](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Geben Sie als Name für das Verzeichnis *myDirectory* ein, und wählen Sie anschließend **OK**. 

Das Verzeichnis *myDirectory* wird auf der Registerkarte für die Dateifreigabe *myshare* aufgeführt.

### <a name="upload-a-file"></a>Hochladen einer Datei 
Sie können eine Datei von Ihrem lokalen Computer in das neue Verzeichnis Ihrer Dateifreigabe hochladen. Sie können einen gesamten Ordner oder eine einzelne Datei hochladen.

1. Wählen Sie im obersten Menü die Option **Hochladen**. Sie haben dann die Möglichkeit, einen Ordner oder eine Datei hochzuladen.
2. Wählen Sie **Datei hochladen** und dann eine Datei zum Hochladen von Ihrem lokalen Computer aus.
3. Geben Sie unter **Upload to a directory** (In ein Verzeichnis hochladen) den Namen *myDirectory* ein, und wählen Sie anschließend die Option **Hochladen**. 

Wenn Sie dies durchgeführt haben, wird die Datei im Bereich *myDirectory* in der Liste angezeigt.

### <a name="download-a-file"></a>Herunterladen einer Datei
Klicken Sie zum Herunterladen der Kopie einer Datei Ihrer Dateifreigabe mit der rechten Maustaste auf die Datei, und wählen Sie anschließend die Option **Herunterladen**. Geben Sie an, wo Sie die Datei auf Ihrem lokalen Computer ablegen möchten, und wählen Sie anschließend **Speichern**.

Der Status des Downloads wird unten im Fenster im Bereich **Aktivitäten** angezeigt.

## <a name="create-and-modify-share-snapshots"></a>Erstellen und Ändern von Freigabemomentaufnahmen
Mit einer Momentaufnahme wird eine Kopie einer Azure-Dateifreigabe für einen bestimmten Zeitpunkt erstellt. Momentaufnahmen von Dateifreigaben ähneln anderen Technologien, mit denen Sie unter Umständen bereits vertraut sind:
- [Volumeschattenkopie-Dienst (VSS)](https://docs.microsoft.com/en-us/windows/desktop/VSS/volume-shadow-copy-service-portal) für Windows-Dateisysteme wie NTFS und ReFS
- Momentaufnahmen vom Typ [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) für Linux-Systeme
- Momentaufnahmen vom Typ [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) für macOS

Gehen Sie wie folgt vor, um eine Freigabemomentaufnahme zu erstellen:

1. Wählen Sie die Registerkarte für die Dateifreigabe *myshare*.
2. Wählen Sie im oberen Menü die Option **Momentaufnahme erstellen**. (Je nach Größe des Storage-Explorer-Fensters müssen Sie unter Umständen zuerst **Mehr** wählen, um diese Option anzuzeigen.)  
    ![Screenshot: Schaltfläche „Momentaufnahme erstellen“ im Kontext](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Auflisten und Durchsuchen von Freigabemomentaufnahmen
Nachdem die Momentaufnahme erstellt wurde, können Sie **View Snapshots for File Share** (Momentaufnahmen für Dateifreigabe anzeigen) wählen, um die Momentaufnahmen für die Freigabe aufzulisten. (Je nach Größe des Storage-Explorer-Fensters müssen Sie unter Umständen zuerst **Mehr** wählen, um diese Option anzuzeigen.) Doppelklicken Sie auf eine Freigabemomentaufnahme, um sie zu durchsuchen.

![Screenshot: Fenster zum Durchsuchen von Momentaufnahmen](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Wiederherstellen von einer Freigabemomentaufnahme
Sie müssen zuerst eine Datei aus der aktiven Azure-Dateifreigabe löschen, um demonstrieren zu können, wie eine Datei aus einer Freigabemomentaufnahme wiederhergestellt wird. Navigieren Sie zum Ordner *myDirectory*, klicken Sie mit der rechten Maustaste auf die hochgeladene Datei, und wählen Sie anschließend die Option **Löschen**. Stellen Sie diese Datei wie folgt aus der Freigabemomentaufnahme wieder her:

1. Wählen Sie **View Snapshots for File Share** (Momentaufnahmen für Dateifreigabe anzeigen). (Je nach Größe des Storage-Explorer-Fensters müssen Sie unter Umständen zuerst **Mehr** wählen, um diese Option anzuzeigen.)
2. Doppelklicken Sie in der Liste mit den Freigabemomentaufnahmen auf die Freigabemomentaufnahme.
3. Durchsuchen Sie die Momentaufnahme, bis Sie die gelöschte Datei gefunden haben. Wählen Sie die Dateifreigabe und dann die Option **Momentaufnahme wiederherstellen**. (Je nach Größe des Storage-Explorer-Fensters müssen Sie unter Umständen zuerst **Mehr** wählen, um diese Option anzuzeigen.) Es wird ein Fenster mit einer Warnung geöffnet, dass der Inhalt der Dateifreigabe beim Wiederherstellen der Datei überschrieben wird und der Vorgang nicht rückgängig gemacht werden kann. Klicken Sie auf **OK**.
4. Die Datei sollte sich jetzt an ihrem ursprünglichen Ort unter der aktiven Azure-Dateifreigabe befinden.

### <a name="delete-a-share-snapshot"></a>Löschen einer Freigabemomentaufnahme
Navigieren Sie zur [Liste mit den Freigabemomentaufnahmen](#list-and-browse-share-snapshots), um eine Freigabemomentaufnahme zu löschen. Klicken Sie mit der rechten Maustaste auf die Freigabemomentaufnahme, die Sie löschen möchten, und wählen Sie dann die Option **Löschen**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie können Storage-Explorer nicht nutzen, um Ressourcen zu entfernen. Sie können das [Azure-Portal](https://portal.azure.com/) verwenden, um die Bereinigung für diese Schnellstartanleitung durchzuführen. 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Dateifreigaben mit dem Azure-Portal](storage-how-to-use-files-portal.md)
- [Verwalten von Dateifreigaben mit Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Verwalten von Dateifreigaben mit der Azure CLI](storage-how-to-use-files-cli.md)
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)