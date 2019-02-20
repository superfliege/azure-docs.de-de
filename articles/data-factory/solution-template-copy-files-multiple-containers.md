---
title: Kopieren von Dateien aus mehreren Containern mit Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Data Factory eine Lösungsvorlage verwenden, um Dateien aus mehreren Containern zu kopieren.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/1/2018
ms.openlocfilehash: aa5f32594c295ab6a8e60af8359370f64f75a72d
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55966557"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Kopieren von Dateien aus mehreren Containern mit Azure Data Factory

Mit der in diesem Artikel beschriebenen Lösungsvorlage können Sie Dateien aus mehreren Ordnern, Containern oder Buckets zwischen Dateispeichern kopieren. Beispielsweise könnten Sie Ihr Data Lake-Repository von AWS S3 zu Azure Data Lake Store migrieren. Oder Sie könnten alle Daten aus einem Azure Blob Storage-Konto in ein anderes Azure Blob Storage-Konto replizieren. Diese Vorlage wurde für diese Anwendungsfälle entwickelt.

Wenn Sie Dateien aus einem einzelnen Container oder Bucket kopieren möchten, ist es effizienter, mit dem **Tool „Daten kopieren“** eine Pipeline mit einer einzelnen Kopieraktivität zu erstellen. Für diesen einfachen Anwendungsfall ist diese Vorlage zu umfassend.

## <a name="about-this-solution-template"></a>Informationen zu dieser Lösungsvorlage

Diese Vorlage listet die Container aus Ihrem Quellspeicher auf und kopiert dann jeden Container aus dem Quellspeicher in den Zielspeicher. 

Die Vorlage enthält drei Aktivitäten:
-   Eine **GetMetadata**-Aktivität zum Überprüfen Ihres Quellspeichers und zum Abrufen der Containerliste.
-   Eine **ForEach**-Aktivität zum Abrufen der Containerliste von der **GetMetadata**-Aktivität sowie zum Durchlaufen der Liste und zum Übergeben der einzelnen Container an die Kopieraktivität.
-   Eine Kopieraktivität (**Copy**) zum Kopieren der einzelnen Container aus dem Quellspeicher in den Zielspeicher.

Die Vorlage definiert zwei Parameter:
-   Der Parameter *SourceFilePath* ist der Pfad Ihres Datenquellspeichers, von dem Sie eine Liste der Container oder Buckets abrufen können. In den meisten Fällen handelt es sich bei dem Pfad um das Stammverzeichnis, das mehrere Containerordner enthält. Der Standardwert dieses Parameters lautet `/`.
-   Der Parameter *DestinationFilePath* ist der Pfad, unter dem die Dateien in Ihren Zielspeicher kopiert werden. Der Standardwert dieses Parameters lautet `/`.

## <a name="how-to-use-this-solution-template"></a>So verwenden Sie diese Lösungsvorlage

1. Wechseln Sie zur Vorlage zum **Kopieren von Dateien aus mehreren Containern zwischen Dateispeichern**, und erstellen Sie eine **neue Verbindung** mit Ihrem Quellspeicher. Der Quellspeicher ist der Ort, an dem Dateien aus mehreren Containern oder Buckets kopiert werden sollen.

    ![Erstellen einer neuen Verbindung mit der Quelle](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Erstellen Sie eine **neue Verbindung** mit Ihrem Zielspeicher.

    ![Erstellen einer neuen Verbindung mit dem Ziel](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Klicken Sie auf **Diese Vorlage verwenden**.

    ![Diese Vorlage verwenden](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Die im Bereich verfügbare Pipeline wird angezeigt, wie im folgenden Beispiel dargestellt:

    ![Pipeline anzeigen](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Klicken Sie auf **Debuggen**, geben Sie **Parameter** ein, und klicken Sie dann auf **Fertig stellen**.

    ![Führen Sie die Pipeline aus.](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Überprüfen Sie das Ergebnis.

    ![Ergebnis überprüfen](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in den Azure Data Factory-Dienst](introduction.md)
