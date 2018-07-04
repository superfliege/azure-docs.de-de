---
title: Erstellen von ereignisbasierten Triggern in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie in Azure Data Factory ein Trigger erstellt wird, der eine Pipeline als Reaktion auf ein Ereignis ausführt.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: douglasl
ms.openlocfilehash: a9c15b239ee0bd0dde0b1f11691565b2676e3d07
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062120"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Erstellen eines Triggers, der eine Pipeline als Reaktion auf ein Ereignis ausführt

Dieser Artikel beschreibt die ereignisbasierten Trigger, die Sie in Ihren Data Factory-Pipelines erstellen können.

Die ereignisgesteuerte Architektur (EDA) ist ein allgemeines Datenintegrationsmuster, das die Produktion, Erkennung, Verbrauch und Reaktion auf Ereignisse beinhaltet. In Datenintegrationsszenarien müssen Data Factory-Kunden häufig Pipelines basierend auf Ereignissen auslösen. Data Factory und [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) sind jetzt integriert, sodass Sie bei einem Ereignis Pipelines auslösen können.

## <a name="data-factory-ui"></a>Data Factory-Benutzeroberfläche

### <a name="create-a-new-event-trigger"></a>Erstellen eines neuen Triggers

Ein typisches Ereignis ist der Eingang einer Datei oder das Löschen einer Datei in Ihrem Azure-Speicherkonto. Sie können einen Trigger erstellen, der auf dieses Ereignis in Ihrer Data Factory-Pipeline reagiert.

> [!NOTE]
> Diese Integration unterstützt nur Storage-Konten der Version 2 (Universell).

![Erstellen eines neuen Triggers](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="select-the-event-trigger-type"></a>Auswählen des Ereignistriggertyps

Sobald die Datei in Ihrem Speicherort eingeht und das entsprechende Blob-Element erstellt wird, löst dieses Ereignis Ihre Data Factory-Pipeline aus und führt sie aus. Sie können einen Trigger erstellen, der auf ein Blob-Erstellungsereignis, ein Blob-Löschereignis oder auf beide Ereignisse in den Data Factory-Pipelines reagiert.

![Auswählen des Triggertyps als Ereignis](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="configure-the-event-trigger"></a>Konfigurieren des Ereignistriggers

Mit den Eigenschaften **Blob-Pfad beginnt mit** und **Blob-Pfad endet mit** können Sie die Container, Ordner und Blob-Namen angeben, für die Ereignisse empfangen werden sollen. Für die Eigenschaften **Blob-Pfad beginnt mit** und **Blob-Pfad endet mit** können Sie eine Vielzahl von Mustern verwenden, wie in den Beispielen im weiteren Verlauf dieses Artikels dargestellt. Sie müssen mindestens eine dieser Eigenschaften angeben.

![Konfigurieren des Ereignistriggers](media/how-to-create-event-trigger/event-based-trigger-image3.png)

## <a name="json-schema"></a>JSON-Schema

Die folgende Tabelle enthält eine Übersicht über die Schemaelemente, die mit ereignisbasierten Triggern verknüpft sind:

| **JSON-Element** | **Beschreibung** | **Typ** | **Zulässige Werte** | **Erforderlich** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | Die Ressourcen-ID von Azure Resource Manager im Speicherkonto. | Zeichenfolge | Azure Resource Manager-ID | Ja |
| **events** | Der Ereignistyp, die diesen Trigger auslöst. | Array    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Ja, eine beliebige Kombination. |
| **blobPathBeginsWith** | Der Blobpfad muss mit dem Muster, beginnen, das für das Auslösen des Triggers vorgesehen ist. Beispielsweise löst „/records/blobs/december/“ nur den Trigger für Blobs im Ordner „december“ im Container „records“ aus. | Zeichenfolge   | | Es muss mindestens eine dieser Eigenschaften angegeben werden: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | Der Blobpfad muss mit dem Muster enden, das für das Auslösen des Triggers vorgesehen ist. „December/boxes.csv“ löst z.B. nur den Trigger für Blobs mit der Bezeichnung „boxes“ im Ordner „december“ aus. | Zeichenfolge   | | Es muss mindestens eine dieser Eigenschaften angegeben werden: blobPathBeginsWith, blobPathEndsWith. |

## <a name="examples-of-event-based-triggers"></a>Beispiele für ereignisbasierte Trigger

Dieser Abschnitt enthält Beispiele für die Einstellungen für ereignisbasierte Trigger.

-   **Blobpfad beginnt mit**(„/cntainername/") –empfängt Ereignisse für einen beliebigen Blob im Container.
-   **Blobpfad beginnt mit**(„/Containername/Blobs/Ordnername“): Empfängt Ereignisse für beliebige Blobs im Container „Containername“ im Ordner „Ordnername“.
-   **Blobpfad beginnt mit**(„/Containername/Blobs/Ordnername/datei.txt“): Empfängt Ereignisse für ein Blob mit der Bezeichnung „datei.txt“ im Container „Containername“ im Ordner „Ordnername“.
-   **Blobpfad endet mit**("file.txt") – empfängt Ereignisse für einen Blob mit der Bezeichnung datei.txt in einem beliebigen Pfad.
-   **Blobpfad endet mit**(„/Containername/Blobs/datei.txt“): Empfängt Ereignisse für ein Blob mit der Bezeichnung „datei.txt“ im Container „Containername“.
-   **Blobpfad endet mit**(„/foldername/file.txt“) – empfängt Ereignisse für einen Blob mit der Bezeichnung datei.txt im Ornder „foldername“ in einem beliebigen Container.

> [!NOTE]
> Sie müssen das Segment `/blobs/` des Pfads immer dann einbeziehen, wenn Sie Container und Ordner, Container und Datei oder Container, Ordner und Datei angeben.

## <a name="using-blob-events-trigger-properties"></a>Verwenden von Eigenschaften von Triggern für Blobereignisse

Wenn ein Trigger für Blobereignisse ausgelöst wird, werden Ihrer Pipeline zwei Variablen zur Verfügung gestellt: *FolderPath* und *FileName*. Verwenden Sie diese Variablen für den Zugriff auf die Ausdrücke `@triggerBody().fileName` und `@triggerBody().folderPath`.

Angenommen, ein Trigger ist so konfiguriert, dass er ausgelöst wird, wenn ein Blob mit `.csv` als Wert von `blobPathEndsWith` erstellt wird. Wenn eine CSV-Datei im Speicherkonto abgelegt wird, bestimmen die Ordner *folderPath* und *fileName* den Speicherort der CSV-Datei. Beispielsweise hat *folderPath* den Wert `/containername/foldername/nestedfoldername` und *fileName* den Wert `filename.csv`.

## <a name="next-steps"></a>Nächste Schritte
Detaillierte Informationen zu Triggern finden Sie unter [Pipelineausführung und -trigger](concepts-pipeline-execution-triggers.md#triggers).
