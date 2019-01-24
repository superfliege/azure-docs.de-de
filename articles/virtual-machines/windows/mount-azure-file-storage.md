---
title: Bereitstellen von Azure File Storage über einen virtuellen Windows Azure-Computer | Microsoft-Dokumentation
description: Speichern Sie die Datei mit Azure File Storage in der Cloud, und stellen Sie Ihre Clouddateifreigabe über einen virtuellen Azure-Computer bereit.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.component: files
ms.openlocfilehash: a2ddaa28f7fbcc6b363ce2c636521fe54ea752f7
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478177"
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Verwenden von Azure-Dateifreigaben mit virtuellen Windows-Computern 

Sie können Azure-Dateifreigaben verwenden, um Dateien über den virtuellen Computer zu speichern und darauf zuzugreifen. Sie können beispielsweise ein Skript oder eine Anwendungskonfigurationsdatei speichern, das bzw. die für alle virtuellen Computer freigegeben werden soll. In diesem Artikel erfahren Sie, wie eine Azure-Dateifreigabe erstellt und eingebunden wird und wie Dateien hoch- und heruntergeladen werden.

## <a name="connect-to-a-file-share-from-a-vm"></a>Verbinden einer Dateifreigabe über einen virtuellen Computer

In diesem Abschnitt wird davon ausgegangen, dass Sie bereits über eine Dateifreigabe verfügen, die Sie verbinden möchten. Wenn Sie eine Dateifreigabe erstellen müssen, finden Sie weiter unten in diesem Artikel unter [Erstellen einer Dateifreigabe](#create-a-file-share) weitere Informationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Menü auf **Speicherkonten**.
3. Wählen Sie Ihr Speicherkonto aus.
4. Wählen Sie auf der Seite **Übersicht** unter **Dienste** die Option **Dateien** aus.
5. Wählen Sie eine Dateifreigabe aus, oder klicken Sie auf **+ Dateifreigabe**, um eine neue Dateifreigabe zu erstellen.
6. Klicken Sie auf **Verbinden**, um eine Seite zu öffnen, auf der die Befehlszeilensyntax für die Bereitstellung der Dateifreigabe über Windows oder Linux angezeigt wird.
7. Wählen Sie unter **Laufwerkbuchstaben** den Buchstaben aus, mit dem Sie das Laufwerk identifizieren möchten.
8. Wählen Sie aus, welche Syntax verwendet werden soll, und wählen Sie die Schaltfläche „Kopieren“ auf der rechten Seite aus, um die Syntax in die Zwischenablage zu kopieren. Fügen Sie sie an einer Stelle ein, an der Sie problemlos darauf zugreifen können. 
8. Stellen Sie eine Verbindung zum virtuellen Computer her, und öffnen Sie eine Eingabeaufforderung.
9. Fügen Sie die Syntax der bearbeiteten Verbindung ein, und drücken Sie die **EINGABETASTE**.
10. Wenn die Verbindung erstellt wurde, erhalten Sie folgende Nachricht: **Der Befehl wurde erfolgreich abgeschlossen.**
11. Überprüfen Sie die Verbindung, indem Sie den Laufwerkbuchstaben eingeben, um zu diesem Laufwerk zu wechseln, und geben Sie anschließend **dir** ein, um die Inhalte der Dateifreigabe anzuzeigen.



## <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Menü auf **Speicherkonten**.
3. Wählen Sie Ihr Speicherkonto aus.
4. Wählen Sie auf der Seite **Übersicht** unter **Dienste** die Option **Dateien** aus.
5. Klicken Sie auf der Seite „Dateidienst“ auf **+ Dateifreigabe**.
6. Geben Sie den Dateifreigabenamen ein. Dateifreigabenamen können Kleinbuchstaben, Zahlen und Bindestriche enthalten. Der Name darf nicht mit einem Bindestrich beginnen, und Sie dürfen nicht mehrere aufeinander folgende Bindestriche verwenden. 
7. Geben Sie ein Limit für die Dateigröße ein (bis zu 5.120 GB).
8. Klicken Sie auf **OK**, um die Dateifreigabe zu erstellen.
   
## <a name="upload-files"></a>Hochladen von Dateien
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Menü auf **Speicherkonten**.
3. Wählen Sie Ihr Speicherkonto aus.
4. Wählen Sie auf der Seite **Übersicht** unter **Dienste** die Option **Dateien** aus.
5. Wählen Sie eine Dateifreigabe aus.
6. Klicken Sie auf **Hochladen**, um die Seite **Hochladen von Dateien** zu öffnen.
7. Klicken Sie auf das Ordnersymbol, um Ihr lokales Dateisystem nach einer hochzuladenden Datei zu durchsuchen.   
8. Klicken Sie auf **Hochladen**, um die Datei in der Dateifreigabe hochzuladen.

## <a name="download-files"></a>Herunterladen von Dateien
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Menü auf **Speicherkonten**.
3. Wählen Sie Ihr Speicherkonto aus.
4. Wählen Sie auf der Seite **Übersicht** unter **Dienste** die Option **Dateien** aus.
5. Wählen Sie eine Dateifreigabe aus.
6. Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie **Herunterladen** aus, um die Datei auf den lokalen Computer herunterzuladen.
   

## <a name="next-steps"></a>Nächste Schritte

Sie können Dateifreigaben auch mit PowerShell erstellen und verwalten. Weitere Informationen finden Sie unter [Erste Schritte mit Azure File Storage unter Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
