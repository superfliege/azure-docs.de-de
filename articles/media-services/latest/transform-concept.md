---
title: Transformationen und Aufträge in Azure Media Services | Microsoft-Dokumentation
description: Wenn Sie Media Services verwenden, müssen Sie eine Transformation erstellen, um die Regeln oder Vorgaben für die Verarbeitung Ihrer Videos zu beschreiben. Dieser Artikel gibt einen Überblick darüber, was Transformation ist und wie sie verwendet wird.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: d256d87548d54951cb77beffb88bba26a1a3de49
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="transforms-and-jobs"></a>Transformationen und Aufträge

## <a name="overview"></a>Übersicht 

Die neueste Version der Azure Media Services REST API (v3) führt eine neue, auf einer Vorlage basierende Workflowressource zum Codieren und/oder Analysieren von Videos ein, die als **Transformation** bezeichnet wird. **Transformationen** können verwendet werden, um allgemeine Aufgaben zur Codierung oder Analyse von Videos zu konfigurieren. Jede **Transformation** beschreibt einen einfachen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien. 

Das **Transformations**objekt ist das „Rezept“ und ein **Auftrag** die tatsächliche Anforderung an Azure Media Services, diese **Transformation** auf ein bestimmtes Eingabevideo oder einen Audioinhalt anzuwenden. Der **Auftrag** gibt Informationen wie den Speicherort des Eingabevideos und den Speicherort für die Ausgabe an. Sie können den Speicherort Ihres Videos mit HTTP(S)-URLs, SAS-URLs oder einem Pfad zu Dateien angeben, die sich lokal oder in Azure Blob Storage befinden. Sie können bis zu 100 Transformationen in Ihrem Azure Media Services-Konto anlegen und Aufträge unter diesen Transformationen übermitteln. Sie können dann Ereignisse wie z.B. Änderungen des Auftragsstatus über Benachrichtigungen abonnieren, die direkt in das Benachrichtigungssystem Azure Event Grid integriert sind. 

Da diese API von Azure Resource Manager gesteuert wird, können Sie Resource Manager-Vorlagen verwenden, um Transformationen in Ihrem Media Services-Konto zu erstellen und bereitzustellen. Rollenbasierte Zugriffssteuerung kann in dieser API auch auf Ressourcenebene festgelegt werden, sodass Sie den Zugriff auf bestimmte Ressourcen wie Transformationen sperren können.

## <a name="typical-workflow-and-example"></a>Typischer Workflow und Beispiel

1. Erstellen einer Transformation 
2. Übermitteln von Aufträgen unter dieser Transformation 
3. Auflisten von Transformationen 
4. Löschen Sie eine Transformation, wenn Sie nicht vorhaben, dieses „Rezept“ künftig zu verwenden. 

Angenommen, Sie möchten den ersten Frame aller Ihrer Videos als Miniaturbild extrahieren. Dazu müssen Sie die folgenden Schritte ausführen: 

1. Legen Sie die Regel für die Verarbeitung fest, z.B. das erste Bild des Videos als Miniaturbild verwenden. 
2. Anschließend geben Sie dem Dienst für jedes Video, das als Eingabe dienen kann, Folgendes an: 
    1. Wo das Video zu finden ist und 
    2. Wohin die Ausgabe, z.B. das Miniaturbild, geschrieben werden soll 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Streamen von Videodateien](stream-files-dotnet-quickstart.md)
