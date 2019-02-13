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
ms.date: 02/03/2019
ms.author: juliako
ms.openlocfilehash: e84f74fe4678a65a33c9cc728f290e7c905b2261
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55743734"
---
# <a name="transforms-and-jobs"></a>Transformationen und Aufträge
 
Verwenden Sie [Transformationen](https://docs.microsoft.com/rest/api/media/transforms), um allgemeine Aufgaben zur Codierung oder Analyse von Videos zu konfigurieren. Jede **Transformation** beschreibt eine Vorgehensweise oder einen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien. Eine einzelne Transformation kann mehrere Regeln anwenden. Eine Transformation kann z.B. angeben, dass jedes Video in eine MP4-Datei mit einer festgelegten Bitrate codiert und eine Miniaturansicht aus dem ersten Bild im Video generiert werden soll. Sie würden für jede Regel, die Sie in die Transformation einfügen möchten, einen TransformOutput-Eintrag hinzufügen. Sie können Transformationen in Ihrem Media Services-Konto direkt über die Media Services v3-API mithilfe eines der veröffentlichten SDKs verwenden. Die Media Services-v3-API wird von Azure Resource Manager gesteuert. Deshalb können Sie auch Resource Manager-Vorlagen verwenden, um Transformationen in Ihrem Media Services-Konto zu erstellen und bereitzustellen. Mithilfe der rollenbasierten Zugriffssteuerung können Sie den Zugriff auf Transformationen sperren.

Der Aktualisierungsvorgang für die Entität [Transformieren](https://docs.microsoft.com/rest/api/media/transforms) ist für das Vornehmen von Änderungen an der Beschreibung oder den Prioritäten der zugrundeliegenden TransformOutputs vorgesehen. Es empfiehlt sich, solche Updates auszuführen, wenn alle in Bearbeitung befindlichen Aufträge abgeschlossen wurden. Wenn Sie die Konfiguration umschreiben möchten, müssen Sie eine neue Transformation erstellen.

Ein [Auftrag](https://docs.microsoft.com/rest/api/media/jobs) ist die eigentliche Anforderung an Azure Media Services, die erstellte **Transformation** auf ein bestimmtes Eingabevideo oder auf einen Audioinhalt anzuwenden. Nachdem die Transformation erstellt wurde, können Sie mithilfe von Media Services-APIs oder der veröffentlichten SDKs Aufträge übermitteln. Der **Auftrag** gibt Informationen wie den Speicherort des Eingabevideos und den Speicherort für die Ausgabe an. Sie können den Speicherort Ihres Eingabevideos über HTTPS-URLs, SAS-URLs oder [Medienobjekte](https://docs.microsoft.com/rest/api/media/assets). Fortschritt und Status von Aufträgen können abgerufen werden, indem die Ereignisse mit Event Grid überwacht werden. Weitere Informationen finden Sie unter [Überwachen von Ereignissen mit EventGrid](job-state-events-cli-how-to.md).

Der Update-Vorgang für den [Auftrag](https://docs.microsoft.com/rest/api/media/jobs) kann verwendet werden, um die Eigenschaften *description*, und *priority* nach dem Übermitteln des Auftrags zu ändern. Eine Änderung an der *priority*-Eigenschaft wird nur wirksam, wenn sich der Auftrag noch in der Warteschlange befindet. Wenn die Verarbeitung des Auftrags gestartet oder bereits beendet wurde, hat das Ändern der Priorität keine Auswirkungen.

> [!NOTE]
> Eigenschaften von **Transform** und **Job** vom Datetime-Typ sind immer im UTC-Format angegeben.

## <a name="typical-workflow"></a>Typischer Workflow

1. Erstellen einer Transformation 
2. Übermitteln von Aufträgen unter dieser Transformation 
3. Auflisten von Transformationen 
4. Löschen Sie eine Transformation, wenn Sie nicht vorhaben, diese Konfiguration künftig zu verwenden. 

### <a name="example"></a>Beispiel

Angenommen, Sie möchten den ersten Frame aller Ihrer Videos als Miniaturbild extrahieren. Dazu müssen Sie die folgenden Schritte ausführen: 

1. Legen Sie die Konfiguration oder Regel für die Verarbeitung Ihrer Videos fest, z.B. „das erste Bild des Videos als Miniaturbild verwenden“. 
2. Sie würden den Dienst für jedes Video anweisen: 
    1. Wo das Video zu finden ist  
    2. Wohin die Ausgabe (das Miniaturbild) geschrieben werden soll 

Eine **Transformation** hilft Ihnen, die Konfiguration zu erstellen (Schritt 1) und dann Aufträge mithilfe dieser Konfiguration zu übermitteln (Schritt 2).

## <a name="paging"></a>Paging

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Media Services-Entitäten](entities-overview.md).

## <a name="next-steps"></a>Nächste Schritte

[Hochladen, Codieren und Streamen von Videodateien](stream-files-tutorial-with-api.md)
