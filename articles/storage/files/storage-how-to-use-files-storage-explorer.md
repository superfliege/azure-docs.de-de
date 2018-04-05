---
title: Verwalten von Azure-Dateifreigaben mit Azure Storage-Explorer
description: Es wird beschrieben, wie Sie Azure Storage-Explorer zum Verwalten von Azure Files verwenden.
services: storage
documentationcenter: ''
author: wmgries
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2018
ms.author: wgries
ms.openlocfilehash: 0c16d3b0ef0b178f99eaafecd74eabb00cca5af9
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-with-azure-storage-explorer"></a>Verwalten von Azure-Dateifreigaben mit Azure Storage-Explorer 
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. In dieser Anleitung werden die Grundlagen der Arbeit mit Azure-Dateifreigaben mit [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) Schritt für Schritt beschrieben. Azure Storage-Explorer ist ein beliebtes Clienttool, das für Windows, macOS und Linux zum Verwalten von Azure-Dateifreigaben und anderen Speicherressourcen verfügbar ist.

Für diese Schnellstartanleitung muss Azure Storage-Explorer installiert sein. Falls Sie die Installation durchführen müssen, können Sie die Anwendung auf der Seite für [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) herunterladen.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe und eines Speicherkontos
> * Erstellen einer Azure-Dateifreigabe 
> * Erstellen eines Verzeichnisses
> * Hochladen einer Datei
> * Herunterladen einer Datei
> * Erstellen und Verwenden einer Freigabemomentaufnahme

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Mit dem Azure Storage-Explorer können keine neuen Ressourcen erstellt werden. Erstellen Sie das Speicherkonto für diese Demonstration daher mit dem [Azure-Portal](https://portal.azure.com/). 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connecting-azure-storage-explorer-to-azure-resources"></a>Herstellen einer Verbindung für Azure Storage-Explorer mit Azure-Ressourcen
Nach dem ersten Start wird das Fenster **Microsoft Azure Storage-Explorer – Verbinden** angezeigt. Es gibt verschiedene Möglichkeiten, wie Sie mit Azure Storage-Explorer eine Verbindung mit Speicherkonten herstellen können: 

- **Anmeldung über Ihr Azure-Konto**: Sie können sich mit Ihren Benutzeranmeldeinformationen für Ihre Organisation oder für Ihr Microsoft-Konto anmelden. 
- **Herstellen einer Verbindung mit einem bestimmten Speicherkonto per Verbindungszeichenfolge oder SAS-Token**: Eine Verbindungszeichenfolge ist eine spezielle Zeichenfolge, die den Speicherkontonamen und den Speicherkontoschlüssel bzw. das SAS-Token enthält. Hiermit kann Azure Storage-Explorer direkt auf das Speicherkonto zugreifen (im Gegensatz zum Anzeigen aller Speicherkonten eines Azure-Kontos). Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
- **Herstellen einer Verbindung mit einem bestimmten Speicherkonto über einen Speicherkontonamen und -schlüssel**: Verwenden Sie den Speicherkontonamen und -schlüssel Ihres Speicherkontos, um die Verbindung mit Azure-Speicher herzustellen.

Melden Sie sich für diese Schnellstartanleitung mit Ihrem Azure-Konto an. Wählen Sie **Azure-Konto hinzufügen**, und klicken Sie auf **Anmelden**. Befolgen Sie die Anweisungen auf dem Bildschirm, um sich an Ihrem Azure-Konto anzumelden.

![Fenster „Microsoft Azure Storage-Explorer – Verbinden“](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe
Gehen Sie wie folgt vor, um Ihre erste Azure-Dateifreigabe im Speicherkonto *storageacct<random number>* zu erstellen:

1. Erweitern Sie das Speicherkonto, das Sie erstellt haben.
2. Klicken Sie mit der rechten Maustaste auf **Dateifreigabe**, und wählen Sie **Dateifreigabe erstellen**.  
    ![Screenshot: Ordner mit Dateifreigaben und Kontextmenü im Kontext](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Geben Sie für die Dateifreigabe *myshare* ein, und drücken Sie die **EINGABETASTE**.

> [!Important]  
> Freigabenamen dürfen nur Kleinbuchstaben, Zahlen und einzelne Bindestriche enthalten und dürfen nicht mit einem Bindestrich beginnen. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

Nachdem die Dateifreigabe erstellt wurde, wird im rechten Bereich eine Registerkarte für Ihre Dateifreigabe geöffnet. 

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Bearbeiten des Inhalts der Azure-Dateifreigabe
Nachdem Sie nun eine Azure-Dateifreigabe erstellt haben, können Sie die Dateifreigabe mit SMB unter [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) oder [macOS](storage-how-to-use-files-mac.md) bereitstellen. Alternativ hierzu können Sie Ihre Azure-Dateifreigabe mit dem Azure-Portal ändern. Alle Anforderungen über das Azure-Portal werden mit der Datei-REST-API durchgeführt, sodass Sie Dateien und Verzeichnisse ohne SMB-Zugriff erstellen, ändern und löschen können.

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Mit dem Hinzufügen eines Verzeichnisses wird eine hierarchische Struktur zum Verwalten Ihrer Dateifreigabe bereitgestellt. Sie können mehrere Ebenen erstellen. Allerdings müssen Sie sicherstellen, dass alle übergeordneten Verzeichnisse vorhanden sind, bevor Sie ein Unterverzeichnis erstellen. Für den Pfad „myDirectory/mySubDirectory“ müssen Sie beispielsweise zuerst das Verzeichnis *myDirectory* und dann das Verzeichnis *mySubDirectory* erstellen. 

1. Klicken Sie auf der Registerkarte für die Dateifreigabe im obersten Menü auf die Schaltfläche **+ Neuer Ordner**. Die Seite **Neues Verzeichnis erstellen** wird geöffnet.
    ![Screenshot: Schaltfläche „Neuer Ordner“ im Kontext](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Geben Sie als Namen *myDirectory* ein, und klicken Sie anschließend auf **OK**. 

Das Verzeichnis *myDirectory* wird auf der Registerkarte für die Dateifreigabe *myshare* aufgeführt.

### <a name="upload-a-file"></a>Hochladen einer Datei 
Laden Sie eine Datei von Ihrem lokalen Computer in das neue Verzeichnis Ihrer Dateifreigabe hoch. Sie können einen ganzen Ordner oder nur eine einzelne Datei hochladen.

1. Wählen Sie im Menü am oberen Rand die Option **Hochladen**. Sie haben dann die Möglichkeit, einen Ordner oder eine Datei hochzuladen.

2. Wählen Sie **Datei hochladen**, und wählen Sie dann eine Datei Ihres lokalen Computers aus, die hochgeladen werden soll.

3. Geben Sie unter **Upload to a directory** (In ein Verzeichnis hochladen) den Namen *myDirectory* ein, und klicken Sie anschließend auf **Hochladen**. 

Nach Abschluss des Vorgangs sollte die Datei in der Liste auf der Seite **myDirectory** angezeigt werden.

### <a name="download-a-file"></a>Herunterladen einer Datei
Sie können eine Kopie einer Datei in Ihrer Dateifreigabe herunterladen, indem Sie mit der rechten Maustaste auf die Datei klicken und **Herunterladen** wählen. Wählen Sie aus, wo Sie die Datei auf Ihrem lokalen Computer ablegen möchten, und klicken Sie anschließend auf **Speichern**.

Der Status des Downloads wird unten im Fenster im Bereich **Aktivitäten** angezeigt.

## <a name="create-and-modify-share-snapshots"></a>Erstellen und Ändern von Freigabemomentaufnahmen
Mit einer Momentaufnahme wird eine Kopie einer Azure-Dateifreigabe für einen bestimmten Zeitpunkt erstellt. Momentaufnahmen von Dateifreigaben ähneln anderen Technologien, mit denen Sie unter Umständen bereits vertraut sind:
- [Volumeschattenkopie-Dienst (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636) für Windows-Dateisysteme wie NTFS und ReFS.
- Momentaufnahmen vom Typ [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) für Linux-Systeme
- Momentaufnahmen vom Typ [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) für macOS 

Gehen Sie wie folgt vor, um eine Freigabemomentaufnahme zu erstellen:

1. Öffnen Sie die Registerkarte für die Dateifreigabe *myshare*.
2. Klicken Sie im Menü am oberen Rand der Registerkarte auf **Momentaufnahme erstellen**. (Diese Option befindet sich ggf. hinter der Option **... Mehr**, je nach den Fensterabmessungen von Azure Storage-Explorer.)  
    ![Screenshot: Schaltfläche „Momentaufnahme erstellen“ im Kontext](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Auflisten und Durchsuchen von Freigabemomentaufnahmen
Nachdem die Momentaufnahme erstellt wurde, können Sie auf **View Snapshots for File Share** (Momentaufnahmen für Dateifreigabe anzeigen) klicken, um die Momentaufnahmen für die Freigabe aufzulisten. (Diese Option befindet sich ggf. hinter der Option **... Mehr**, je nach den Fensterabmessungen von Azure Storage-Explorer.) Doppelklicken Sie auf eine Freigabemomentaufnahme, um sie zu durchsuchen.

![Screenshot: Bildschirm zum Durchsuchen von Momentaufnahmen](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Wiederherstellen von einer Freigabemomentaufnahme
Wir müssen zuerst eine Datei aus der aktiven Azure-Dateifreigabe löschen, um demonstrieren zu können, wie eine Datei aus einer Freigabemomentaufnahme wiederhergestellt wird. Navigieren Sie zum Ordner *myDirectory*, klicken Sie mit der rechten Maustaste auf die hochgeladene Datei, und klicken Sie dann auf **Löschen**. Stellen Sie diese Datei anschließend aus der Freigabemomentaufnahme wieder her:

1. Klicken Sie auf **View Snapshots for File Share** (Momentaufnahmen für Dateifreigabe anzeigen). (Diese Option befindet sich ggf. hinter der Option **... Mehr**, je nach den Fensterabmessungen von Azure Storage-Explorer.)
2. Wählen Sie in der Liste eine Freigabemomentaufnahme aus, und doppelklicken Sie darauf, um dorthin zu navigieren.
3. Navigieren Sie durch die Momentaufnahme, bis Sie die gelöschte Datei gefunden haben. Wählen Sie sie aus, und klicken Sie dann auf **Momentaufnahme wiederherstellen**. (Diese Option befindet sich ggf. hinter der Option **... Mehr**, je nach den Fensterabmessungen von Azure Storage-Explorer.) Es wird eine Warnung angezeigt, dass durch das Wiederherstellen der Datei der Inhalt der Dateifreigabe überschrieben wird und dass der Vorgang nicht rückgängig gemacht werden kann. Klicken Sie auf **OK**.
4. Die Datei sollte sich jetzt an ihrem ursprünglichen Ort unter der aktiven Azure-Dateifreigabe befinden.

### <a name="delete-a-share-snapshot"></a>Löschen einer Freigabemomentaufnahme
[Navigieren Sie zur Liste mit den Freigabemomentaufnahmen](#list-and-browse-share-snapshots), um eine Freigabemomentaufnahme zu löschen. Klicken Sie mit der rechten Maustaste auf die Freigabemomentaufnahme, die Sie löschen möchten, und wählen Sie „Löschen“.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Mit Azure Storage-Explorer können keine Ressourcen entfernt werden. Sie können die Bereinigung für diese Schnellstartanleitung über das [Azure-Portal](https://portal.azure.com/) durchführen. 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Dateifreigaben mit dem Azure-Portal](storage-how-to-use-files-portal.md)
- [Verwalten von Dateifreigaben mit Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Verwalten von Dateifreigaben mit der Azure CLI](storage-how-to-use-files-cli.md)
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)