---
title: Transformationen und Aufträge in Azure Media Services | Microsoft-Dokumentation
description: Wenn Sie Media Services verwenden, müssen Sie eine Transformation erstellen, um die Regeln oder Vorgaben für die Verarbeitung Ihrer Videos zu beschreiben. Dieser Artikel gibt einen Überblick darüber, was Transformation ist und wie sie verwendet wird.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: e13afe26d06f5b5b2dcf7eddf00f9ee481312b2c
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024257"
---
# <a name="transforms-and-jobs"></a>Transformationen und Aufträge
 
Azure Media Services v3 stellt eine neue auf Vorlagen basierende Workflowressource für eine Konfiguration bereit, die Sie zum Codieren und/oder Analysieren von Videos verwenden können und die [Transformationen](https://docs.microsoft.com/rest/api/media/transforms) heißt. **Transformationen** können verwendet werden, um allgemeine Aufgaben zur Codierung oder Analyse von Videos zu konfigurieren. Jede **Transformation** beschreibt eine Vorgehensweise oder einen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien. 

Ein **Auftrag** ist die eigentliche Anforderung an Azure Media Services, die erstellte **Transformation** auf ein bestimmtes Eingabevideo oder auf einen Audioinhalt anzuwenden. Der **Auftrag** gibt Informationen wie den Speicherort des Eingabevideos und den Speicherort für die Ausgabe an. Sie können den Speicherort des Eingabevideos mit HTTPS-URLs, SAS-URLs oder [Media Services-Ressourcen](https://docs.microsoft.com/rest/api/media/assets) angeben.  

## <a name="typical-workflow"></a>Typischer Workflow

1. Erstellen einer Transformation 
2. Übermitteln von Aufträgen unter dieser Transformation 
3. Auflisten von Transformationen 
4. Löschen Sie eine Transformation, wenn Sie nicht vorhaben, diese Konfiguration künftig zu verwenden. 

Angenommen, Sie möchten den ersten Frame aller Ihrer Videos als Miniaturbild extrahieren. Dazu müssen Sie die folgenden Schritte ausführen: 

1. Legen Sie die Konfiguration oder Regel für die Verarbeitung Ihrer Videos fest, z.B. „das erste Bild des Videos als Miniaturbild verwenden“. 
2. Sie würden den Dienst für jedes Video anweisen: 
    1. Wo das Video zu finden ist  
    2. Wohin die Ausgabe (das Miniaturbild) geschrieben werden soll 

Eine **Transformation** hilft Ihnen, die Konfiguration zu erstellen (Schritt 1) und dann Aufträge mithilfe dieser Konfiguration zu übermitteln (Schritt 2).

## <a name="transforms"></a>Transformationen

Sie können Transformationen in Ihrem Media Services-Konto direkt über die v3-API mithilfe eines der veröffentlichten SDKs verwenden. Die Media Services-v3-API wird von Azure Resource Manager gesteuert. Deshalb können Sie auch Resource Manager-Vorlagen verwenden, um Transformationen in Ihrem Media Services-Konto zu erstellen und bereitzustellen. Mithilfe der rollenbasierten Zugriffssteuerung können Sie den Zugriff auf Transformationen sperren.

### <a name="transform-definition"></a>Transformationsdefinition

Die folgende Tabelle enthält die Eigenschaften einer Transformation und die jeweiligen Definitionen.

|NAME|BESCHREIBUNG|
|---|---|
|id|Vollqualifizierte Ressourcen-ID für die Ressource.|
|name|Der Name der Ressource.|
|properties.created |Das Datum und die Uhrzeit (in UTC), an dem die Transformation erstellt wurde, im Format „JJJJ-MM-TTThh:mm:ssZ“.|
|properties.description |Eine ausführliche Beschreibung der Transformation.|
|properties.lastModified |Das Datum und die Uhrzeit (in UTC), an dem die Transformation zuletzt aktualisiert wurde, im Format „JJJJ-MM-TTTss:mm:ssZ“.|
|properties.outputs |Ein Array von mindestens einem TransformOutput-Objekt, das die Transformation generieren soll.|
|type|Der Typ der Ressource.|

Die vollständige Definition finden Sie unter [Transformationen](https://docs.microsoft.com/rest/api/media/transforms).

Wie oben erläutert, kann eine Transformation dabei helfen, eine Konfiguration oder Regel für die Verarbeitung Ihrer Videos zu erstellen. Eine einzelne Transformation kann mehrere Regeln anwenden. Eine Transformation kann z.B. angeben, dass jedes Video in eine MP4-Datei mit einer festgelegten Bitrate codiert und eine Miniaturansicht aus dem ersten Bild im Video generiert werden soll. Sie würden für jede Regel, die Sie in die Transformation einfügen möchten, einen TransformOutput-Eintrag hinzufügen.

> [!NOTE]
> Auch wenn bei der Definition von Transformationen ein Updatevorgang unterstützt wird, sollten Sie sicherstellen, dass alle in Bearbeitung befindlichen Aufträge abgeschlossen werden, bevor Sie eine Änderung vornehmen. Normalerweise würden Sie eine vorhandene Transformation aktualisieren, um die Beschreibung zu oder die Prioritäten der zugrunde liegenden TransformOutputs zu ändern. Wenn Sie die Konfiguration umschreiben möchten, sollten Sie besser eine neue Transformation erstellen.

## <a name="jobs"></a>Aufträge

Nachdem eine Transformation erstellt wurde, können Sie mithilfe von Media Services-APIs oder der veröffentlichten SDKs Aufträge übermitteln. Fortschritt und Status von Aufträgen können abgerufen werden, indem die Ereignisse mit Event Grid überwacht werden. Weitere Informationen finden Sie unter [Überwachen von Ereignissen mit EventGrid](job-state-events-cli-how-to.md ).

### <a name="jobs-definition"></a>Auftragsdefinition

Die folgende Tabelle enthält die Eigenschaften des Auftrags und die jeweiligen Definitionen.

|NAME|BESCHREIBUNG|
|---|---|
|id|Vollqualifizierte Ressourcen-ID für die Ressource.|
|name   |Der Name der Ressource.|
|properties.correlationData |Vom Kunden angegebene Korrelationsdaten (unveränderlich), die als Teil der Job- und JobOutput-Benachrichtigungen zu Statusänderungen zurückgegeben werden. Weitere Informationen finden Sie unter [Ereignisschemas](media-services-event-schemas.md).<br/><br/>Die Eigenschaft kann auch für die Verwendung von Media Services mit mehreren Mandanten verwendet werden. Sie können die Mandanten-IDs in die Aufträge einfügen. |
|properties.created |Das Datum und die Uhrzeit (in UTC), an dem der Auftrag erstellt wurde, im Format „JJJJ-MM-TTThh:mm:ssZ“.|
|properties.description |Optionale vom Kunden bereitgestellte Beschreibung des Auftrags.|
|properties.input|Die Eingaben für den Auftrag.|
|properties.lastModified    |Das Datum und die Uhrzeit (in UTC), an dem der Auftrag zuletzt aktualisiert wurde, im Format „JJJJ-MM-TTTss:mm:ssZ“.|
|properties.outputs|Die Ausgaben für den Auftrag.|
|properties.priority    |Die Priorität, mit dem der Auftrag verarbeitet werden soll. Aufträge mit höherer Priorität werden vor Aufträgen mit niedrigerer Priorität verarbeitet. Wenn diese Eigenschaft nicht festgelegt ist, ist die Priorität standardmäßig normal.|
|properties.state   |Der aktuelle Status des Auftrags.|
|type   |Der Typ der Ressource.|

Die vollständige Definition finden Sie unter [Aufträge](https://docs.microsoft.com/rest/api/media/jobs).

> [!NOTE]
> Auch wenn Auftragsdefinitionen einen Updatevorgang unterstützen, können nach dem Übermitteln des Auftrags lediglich die Eigenschaften description und priority geändert werden. Darüber hinaus gilt eine Änderung der priority-Eigenschaft nur, wenn sich der Auftrag noch in der Warteschlange befindet. Wenn die Verarbeitung des Auftrags gestartet oder beendet wurde, hat das Ändern der Priorität keine Auswirkungen.

### <a name="pagination"></a>Paginierung

Die Paginierung von Aufträgen wird in Media Services v3 unterstützt.

> [!TIP]
> Sie sollten immer den Link „Weiter“ verwenden, um die Sammlung zu durchlaufen, und keine bestimmte Seitengröße als Referenz verwenden.

Wenn eine Abfrageantwort viele Elemente enthält, gibt der Dienst eine \@odata.nextLink-Eigenschaft zurück, um die nächste Seite der Ergebnisse abzurufen. Auf diese Weise kann das gesamte Resultset paginiert werden. Sie können die Seitengröße nicht konfigurieren. 

Wenn während der Paginierung der Sammlung Aufträge erstellt oder gelöscht werden, werden die Änderungen in den zurückgegebenen Ergebnissen übernommen (sofern sich diese Änderungen in dem Teil der Sammlung befinden, der nicht heruntergeladen wurde.) 

Im folgenden C#-Beispiel wird gezeigt, wie alle Aufträge im Konto durchlaufen werden.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Beispiele zu REST finden Sie unter [Aufträge – Liste](https://docs.microsoft.com/rest/api/media/jobs/list).


## <a name="next-steps"></a>Nächste Schritte

[Streamen von Videodateien](stream-files-dotnet-quickstart.md)
