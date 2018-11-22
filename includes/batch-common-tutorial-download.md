---
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: a8ea55a40f1ee4681b6aec147e02b7bce6f1d7cf
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279627"
---
### <a name="retrieve-output-files"></a>Abrufen von Ausgabedateien

Sie können das Azure-Portal verwenden, um die ausgegebenen MP3-Dateien herunterzuladen, die durch die ffmpeg-Aufgaben generiert werden. 

1. Klicken Sie auf **Alle Dienste** > **Speicherkonten** und anschließend auf den Namen Ihres Speicherkontos.
2. Klicken Sie auf **Blobs** > *Ausgabe*.
3. Klicken Sie mit der rechten Maustaste auf eine der ausgegebenen MP3-Dateien, und klicken Sie anschließend auf **Herunterladen**. Folgen Sie den Anweisungen in Ihrem Browser, um die Datei zu öffnen oder zu speichern.

![Herunterladen der Ausgabedatei](./media/batch-common-tutorial-download/download.png)

Die Dateien können auch programmgesteuert aus den Computeknoten oder aus dem Speichercontainer heruntergeladen werden. Dies wird in diesem Beispiel allerdings nicht gezeigt.
