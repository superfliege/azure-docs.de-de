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
ms.date: 04/29/2019
ms.author: juliako
ms.openlocfilehash: 3c3687ceff10baec028435d1e6c513e72ca5da86
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149086"
---
# <a name="transforms-and-jobs"></a>Transformationen und Aufträge

In diesem Thema werden Details zu [Transformationen](https://docs.microsoft.com/rest/api/media/transforms) und [Aufträgen](https://docs.microsoft.com/rest/api/media/jobs) erläutert und die Beziehung zwischen diesen Entitäten erläutert. 

## <a name="overview"></a>Übersicht 

### <a name="transformsjobs-workflow"></a>Workflow für Transformationen/Aufträge

Das folgende Diagramm zeigt den Workflow für Transformationen/Aufträge:

![Transformationen](./media/encoding/transforms-jobs.png)

#### <a name="typical-workflow"></a>Typischer Workflow

1. Erstellen einer Transformation 
2. Übermitteln von Aufträgen unter dieser Transformation 
3. Auflisten von Transformationen 
4. Löschen Sie eine Transformation, wenn Sie nicht vorhaben, diese Konfiguration künftig zu verwenden. 

#### <a name="example"></a>Beispiel

Angenommen, Sie möchten den ersten Frame aller Ihrer Videos als Miniaturbild extrahieren. Dazu müssen Sie die folgenden Schritte ausführen: 

1. Legen Sie die Konfiguration oder Regel für die Verarbeitung Ihrer Videos fest, z.B. „das erste Bild des Videos als Miniaturbild verwenden“. 
2. Sie würden den Dienst für jedes Video anweisen: 
    1. Wo das Video zu finden ist  
    2. Wohin die Ausgabe (das Miniaturbild) geschrieben werden soll 

Eine **Transformation** hilft Ihnen, die Konfiguration zu erstellen (Schritt 1) und dann Aufträge mithilfe dieser Konfiguration zu übermitteln (Schritt 2).

> [!NOTE]
> Eigenschaften von **Transform** und **Job** vom Datetime-Typ sind immer im UTC-Format angegeben.

## <a name="transforms"></a>Transformationen

In der folgenden Abbildung ist das Objekt **Transform** mit den referenzierten Objekten sowie den abgeleiteten Beziehungen dargestellt. Die grauen Pfeile stehen für einen Typ, die vom Auftrag referenziert werden, während die grünen Pfeile Klassenableitungsbeziehungen angeben.<br/>Klicken Sie auf Bild, um es in voller Größe anzeigen.  

<a href="./media/api-diagrams/transform-large.png" target="_blank"><img src="./media/api-diagrams/transform-small.png"></a> 

Verwenden Sie **Transformationen**, um allgemeine Aufgaben zur Codierung oder Analyse von Videos zu konfigurieren. Jede **Transformation** beschreibt eine Vorgehensweise oder einen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien. Eine einzelne Transformation kann mehrere Regeln anwenden. Eine Transformation kann z.B. angeben, dass jedes Video in eine MP4-Datei mit einer festgelegten Bitrate codiert und eine Miniaturansicht aus dem ersten Bild im Video generiert werden soll. Sie würden für jede Regel, die Sie in die Transformation einfügen möchten, einen TransformOutput-Eintrag hinzufügen. Sie können Transformationen in Ihrem Media Services-Konto direkt über die Media Services v3-API mithilfe eines der veröffentlichten SDKs verwenden. Die Media Services-v3-API wird von Azure Resource Manager gesteuert. Deshalb können Sie auch Resource Manager-Vorlagen verwenden, um Transformationen in Ihrem Media Services-Konto zu erstellen und bereitzustellen. Mithilfe der rollenbasierten Zugriffssteuerung können Sie den Zugriff auf Transformationen sperren.

Der Aktualisierungsvorgang für die Entität [Transformieren](https://docs.microsoft.com/rest/api/media/transforms) ist für das Vornehmen von Änderungen an der Beschreibung oder den Prioritäten der zugrundeliegenden TransformOutputs vorgesehen. Es empfiehlt sich, solche Updates auszuführen, wenn alle in Bearbeitung befindlichen Aufträge abgeschlossen wurden. Wenn Sie die Konfiguration umschreiben möchten, müssen Sie eine neue Transformation erstellen.

## <a name="jobs"></a>Aufträge

In der folgenden Abbildung ist das Objekt **Job** mit den referenzierten Objekten sowie den abgeleiteten Beziehungen dargestellt.<br/>Klicken Sie auf Bild, um es in voller Größe anzeigen.  

<a href="./media/api-diagrams/job-large.png" target="_blank"><img src="./media/api-diagrams/job-small.png"></a> 

Ein **Auftrag** ist die eigentliche Anforderung an Azure Media Services, die erstellte **Transformation** auf ein bestimmtes Eingabevideo oder auf einen Audioinhalt anzuwenden. Nachdem die Transformation erstellt wurde, können Sie mithilfe von Media Services-APIs oder der veröffentlichten SDKs Aufträge übermitteln. Der **Auftrag** gibt Informationen wie den Speicherort des Eingabevideos und den Speicherort für die Ausgabe an. Sie können den Speicherort Ihres Eingabevideos über HTTPS-URLs, SAS-URLs oder [Medienobjekte](https://docs.microsoft.com/rest/api/media/assets).  

Verwenden Sie [Auftragseingabe über HTTPS](job-input-from-http-how-to.md), wenn Ihr Inhalt bereits über eine URL zugänglich ist und Sie die Quelldatei nicht in Azure speichern müssen (z. B. Import von S3). Diese Methode ist auch geeignet, wenn sich der Inhalt im Azure Blob Storage befindet, die Datei aber nicht in einem Medienobjekt gespeichert werden muss. Derzeit unterstützt diese Methode nur eine einzelne Datei für die Eingabe.
 
Verwenden Sie [Medienobjekt als Auftragseingabe](job-input-from-local-file-how-to.md), wenn sich der eingegebene Inhalt bereits in einem Medienobjekt befindet oder der Inhalt in einer lokalen Datei gespeichert ist. Es ist auch eine gute Option, wenn Sie planen, das Eingabemedienobjekt für Streaming oder Download zu veröffentlichen (z. B. wenn Sie die MP4-Datei zum Herunterladen veröffentlichen möchten, aber auch eine Sprach- oder Gesichtserkennung erfolgen soll). Diese Methode unterstützt Medienobjekte mit mehreren Dateien (z. B. MBR-Streamingsätze, die lokal verschlüsselt wurden).
 
Fortschritt und Status von Aufträgen können abgerufen werden, indem die Ereignisse mit Event Grid überwacht werden. Weitere Informationen finden Sie unter [Überwachen von Ereignissen mit EventGrid](job-state-events-cli-how-to.md).

Der Update-Vorgang für den [Auftrag](https://docs.microsoft.com/rest/api/media/jobs) kann verwendet werden, um die Eigenschaften *description*, und *priority* nach dem Übermitteln des Auftrags zu ändern. Eine Änderung an der *priority*-Eigenschaft wird nur wirksam, wenn sich der Auftrag noch in der Warteschlange befindet. Wenn die Verarbeitung des Auftrags gestartet oder bereits beendet wurde, hat das Ändern der Priorität keine Auswirkungen.

## <a name="configure-media-reserved-units"></a>Konfigurieren von reservierten Einheiten für Medien

Für die Audioanalyse- und Videoanalyseaufträge, die von Media Services v3 oder Video Indexer ausgelöst werden, wird dringend empfohlen, Ihr Konto mit zehn S3-MRUs (Media Reserved Units, reservierte Einheiten für Medien) bereitzustellen. Erstellen Sie im [Azure-Portal](https://portal.azure.com/) ein Supportticket, falls Sie mehr als zehn S3-MRUs benötigen.

Details dazu finden Sie unter [Skalieren der Medienverarbeitung an der Befehlszeilenschnittstelle](media-reserved-units-cli-how-to.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="see-also"></a>Weitere Informationen

* [Fehlercodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
* [Filterung, Sortierung und Paginierung von Media Services-Entitäten](entities-overview.md)

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Hochladen, Codieren und Streamen von Videos mithilfe von .NET](stream-files-tutorial-with-api.md)
- [Tutorial: Analysieren von Videos mit Media Services v3 mithilfe von .NET](analyze-videos-tutorial-with-api.md)
