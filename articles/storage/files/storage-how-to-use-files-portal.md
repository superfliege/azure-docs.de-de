---
title: Verwalten von Azure-Dateifreigaben mit dem Azure-Portal
description: Es wird beschrieben, wie Sie Azure Files über das Azure-Portal verwalten.
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
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 24996e80d2e75978a814bbf471176d4edcf22549
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031650"
---
# <a name="managing-azure-file-shares-with-the-azure-portal"></a>Verwalten von Azure-Dateifreigaben mit dem Azure-Portal 
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können unter Windows, Linux und macOS bereitgestellt werden. In dieser Anleitung werden die Grundlagen der Arbeit mit Azure-Dateifreigaben mit dem [Azure-Portal](https://portal.azure.com/) Schritt für Schritt beschrieben. In diesem Artikel werden folgende Themen erläutert:

> [!div class="checklist"]
> * Erstellen einer Ressourcengruppe und eines Speicherkontos
> * Erstellen einer Azure-Dateifreigabe 
> * Erstellen eines Verzeichnisses
> * Hochladen einer Datei 
> * Herunterladen einer Datei
> * Erstellen und Verwenden einer Freigabemomentaufnahme

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe
Gehen Sie wie folgt vor, um eine Dateifreigabe zu erstellen:

1. Wählen Sie im Dashboard das Speicherkonto aus.
2. Wählen Sie auf der Speicherkontoseite im Abschnitt **Dienste** die Option **Dateien**.
    ![Screenshot: Abschnitt „Dienste“ des Speicherkontos, Auswahl des Diensts „Dateien“](media/storage-how-to-use-files-portal/create-file-share-1.png)

3. Klicken Sie im Menü oben auf der Seite **Dateidienst** auf **+ Dateifreigabe**. Die Seite **Neue Dateifreigabe** wird angezeigt.
4. Geben Sie unter **Name** den Namen *myshare* ein.
5. Klicken Sie auf **OK**, um die Azure-Dateifreigabe zu erstellen.

## <a name="work-with-the-contents-of-the-azure-file-share"></a>Bearbeiten des Inhalts der Azure-Dateifreigabe
Nachdem Sie nun eine Azure-Dateifreigabe erstellt haben, können Sie die Dateifreigabe mit SMB unter [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) oder [macOS](storage-how-to-use-files-mac.md) bereitstellen. Alternativ hierzu können Sie mit Ihrer Azure-Dateifreigabe im Azure-Portal arbeiten. Alle Anforderungen über das Azure-Portal werden mit der Datei-REST-API durchgeführt, sodass Sie Dateien und Verzeichnisse ohne SMB-Zugriff erstellen, ändern und löschen können.

### <a name="create-a-directory"></a>Erstellen eines Verzeichnisses
Gehen Sie wie folgt vor, um im Stammverzeichnis Ihrer Azure-Dateifreigabe ein neues Verzeichnis mit dem Namen *myDirectory* zu erstellen:

1. Wählen Sie auf der Seite **Dateidienst** die Dateifreigabe **myshare** aus. Die Seite für Ihre Dateifreigabe wird geöffnet.
2. Wählen Sie im Menü oben auf der Seite die Option **+ Verzeichnis hinzufügen**. Die Seite **Neues Verzeichnis** wird angezeigt.
3. Geben Sie *myDirectory* ein, und klicken Sie anschließend auf **OK**.

### <a name="upload-a-file"></a>Hochladen einer Datei 
Um das Hochladen einer Datei demonstrieren zu können, müssen Sie zuerst eine Datei erstellen oder auswählen, die hochgeladen werden kann. Hierfür können Sie die von Ihnen bevorzugte Vorgehensweise verwenden. Gehen Sie wie folgt vor, nachdem Sie die gewünschte Datei zum Hochladen ausgewählt haben:

1. Klicken Sie auf das Verzeichnis **myDirectory**. Das Panel **myDirectory** wird geöffnet.
2. Klicken Sie oben im Menü auf **Hochladen**. Das Panel **Dateien hochladen** wird geöffnet.  
    ![Screenshot: Panel zum Hochladen von Dateien](media/storage-how-to-use-files-portal/upload-file-1.png)

3. Klicken Sie auf das Ordnersymbol, um ein Fenster zu öffnen, in dem Sie Ihre lokalen Dateien durchsuchen können. 
4. Wählen Sie eine Datei aus, und klicken Sie anschließend auf **Öffnen**. 
5. Überprüfen Sie auf der Seite **Dateien hochladen** den Dateinamen, und klicken Sie anschließend auf **Hochladen**.
6. Nach Abschluss des Vorgangs sollte die Datei in der Liste auf der Seite **myDirectory** angezeigt werden.

### <a name="download-a-file"></a>Herunterladen einer Datei
Sie können eine Kopie der hochgeladenen Datei herunterladen, indem Sie mit der rechten Maustaste auf die Datei klicken. Nachdem Sie auf die Schaltfläche zum Herunterladen geklickt haben, richtet sich die genaue Vorgehensweise nach dem verwendeten Betriebssystem und Browser.

## <a name="create-and-modify-share-snapshots"></a>Erstellen und Ändern von Freigabemomentaufnahmen
Eine weitere nützliche Aufgabe, die Sie mit einer Azure-Dateifreigabe durchführen können, ist die Erstellung von Freigabemomentaufnahmen. Mit einer Momentaufnahme wird für eine Azure-Dateifreigabe ein bestimmter Zeitpunkt beibehalten. Freigabemomentaufnahmen ähneln Betriebssystemtechnologien, mit denen Sie unter Umständen bereits vertraut sind:
- [Volumeschattenkopie-Dienst (VSS)](https://docs.microsoft.com/en-us/windows/desktop/VSS/volume-shadow-copy-service-portal) für Windows-Dateisysteme wie NTFS und ReFS
- Momentaufnahmen vom Typ [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) für Linux-Systeme
- Momentaufnahmen vom Typ [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) für macOS 

Gehen Sie wie folgt vor, um eine Freigabemomentaufnahme zu erstellen:

1. Öffnen Sie die Seite für die Dateifreigabe, indem Sie das Speicherkonto in Ihrem Dashboard unter **Dateien** > **myshare** öffnen. 
2. Klicken Sie auf der Seite für die Dateifreigabe im obersten Menü auf die Schaltfläche **Momentaufnahme**, und wählen Sie anschließend die Option **Momentaufnahme erstellen**.  
    ![Screenshot: Schaltfläche „Momentaufnahme erstellen“](media/storage-how-to-use-files-portal/create-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Auflisten und Durchsuchen von Freigabemomentaufnahmen
Nachdem die Momentaufnahme erstellt wurde, können Sie erneut auf **Momentaufnahme** klicken und dann **Momentaufnahmen anzeigen** wählen, um die Momentaufnahmen für die Freigabe aufzulisten. Im sich ergebenden Bereich werden die Momentaufnahmen für diese Freigabe angezeigt. Klicken Sie auf eine Freigabemomentaufnahme, um sie zu durchsuchen.

### <a name="restore-from-a-share-snapshot"></a>Wiederherstellen von einer Freigabemomentaufnahme
Wir müssen zuerst eine Datei aus der aktiven Azure-Dateifreigabe löschen, um demonstrieren zu können, wie eine Datei aus einer Freigabemomentaufnahme wiederhergestellt wird. Navigieren Sie zum Ordner *myDirectory*, klicken Sie mit der rechten Maustaste auf die hochgeladene Datei, und klicken Sie dann auf **Löschen**. Stellen Sie diese Datei anschließend aus der Freigabemomentaufnahme wieder her:

1. Klicken Sie im obersten Menü auf **Momentaufnahmen**, und wählen Sie die Option **Momentaufnahmen anzeigen**. 
2. Wenn Sie auf die zuvor erstellte Momentaufnahme klicken, wird der Inhalt auf einer neuen Seite geöffnet. 
3. Wenn Sie in der Momentaufnahme auf **myDirectory** klicken, sollte die gelöschte Datei angezeigt werden. 
4. Klicken Sie mit der rechten Maustaste auf die gelöschte Datei, und wählen Sie **Wiederherstellen**.
5. Es wird ein Popupelement angezeigt, in dem Sie zwischen dem Wiederherstellen der Datei als Kopie und dem Überschreiben der Originaldatei wählen können. Da wir die Originaldatei gelöscht haben, können wir **Originaldatei überschreiben** wählen, um für die Datei den Zustand vor dem Löschen wiederherzustellen. Klicken Sie auf **OK**, um die Datei auf der Azure-Dateifreigabe wiederherzustellen.  
    ![Screenshot: Dialogfeld „Datei wiederherstellen“](media/storage-how-to-use-files-portal/restore-snapshot-1.png)

6. Nachdem die Wiederherstellung der Datei abgeschlossen ist, können Sie die Seite für die Momentaufnahme schließen und zurück zu **myshare** > **myDirectory** navigieren. Die Datei sollte sich wieder am ursprünglichen Platz befinden.

### <a name="delete-a-share-snapshot"></a>Löschen einer Freigabemomentaufnahme
[Navigieren Sie zur Liste mit den Freigabemomentaufnahmen](#list-and-browse-a-share-snapshot), um eine Freigabemomentaufnahme zu löschen. Klicken Sie auf das Kontrollkästchen neben dem Namen der Freigabemomentaufnahme, und wählen Sie die Schaltfläche **Löschen**.

![Screenshot: Löschen einer Freigabemomentaufnahme](media/storage-how-to-use-files-portal/delete-snapshot-1.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Dateifreigaben mit Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Verwalten von Dateifreigaben mit der Azure CLI](storage-how-to-use-files-cli.md)
- [Verwalten von Dateifreigaben mit Azure Storage-Explorer](storage-how-to-use-files-storage-explorer.md)
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)