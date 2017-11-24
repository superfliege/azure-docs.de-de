---
title: Verwenden von Azure Media Clipper im Portal | Microsoft-Dokumentation
description: Erstellen von Clips mit Azure Media Clipper im Azure-Portal
services: media-services
keywords: Clip;Schneiden;Subclip;Codierung;Medien
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: faaae7edbc2fb62ae219dd963f405e7246c982d4
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper-in-the-portal"></a>Erstellen von Clips im Portal mit Azure Media Clipper
Sie können Azure Media Clipper im Portal verwenden, um Clips aus Medienobjekten in Ihren Media Services-Konten zu erstellen. Navigieren Sie zunächst zu Ihrem Media Services-Konto im Portal. Wählen Sie dann die Registerkarte **Subclip** aus.

Auf der Registerkarte **Subclip** können Sie damit beginnen, Clips zusammenzustellen. Im Portal lädt Clipper MP4-Dateien mit Einzelbitrate, MP4-Dateien mit Mehrfachbitrate und Livearchive, die mit einem gültigen Streamingfaktor veröffentlicht wurden. Nicht veröffentlichte Medienobjekte werden nicht geladen.

## <a name="producing-clips"></a>Erstellen von Clips
Um einen Clip zu erstellen, ziehen Sie ein Medienobjekt per Drag & Drop auf die Clipoberfläche. Wenn die Zeitenmarkierungen bekannt sind, können Sie sie manuell auf der Oberfläche eingeben. Andernfalls geben Sie das Medienobjekt wieder oder verschieben die Wiedergabeposition, um die gewünschte Start- und Endmarkierung zu finden. Wenn keine Markierungen für Start- oder Endzeit bereitgestellt wurde, startet der Clip am Anfang bzw. wird bis zum Ende des Eingabemedienobjekts wiedergegeben.

Um mit der gewünschten Genauigkeit zu navigieren – basierend auf Frames oder einer Gruppe von Bildern (GOP, Group of Pictures) –, verwenden Sie die Schaltflächen „Einen Frame vor“/„Eine GOP vor“ oder „Einen Frame zurück“/„Eine GOP zurück“. Um mehrere Medienobjekte zu schneiden, ziehen Sie mehrere Medienobjekte per Drag & Drop aus dem Auswahlbereich für Medienobjekte auf die Clipoberfläche. Sie können Medienobjekte auf der Oberfläche auswählen und in der gewünschten Reihenfolge neu anordnen. Der Auswahlbereich für Medienobjekte bietet Metadaten zu Dauer, Typ und Auflösung für jedes Medienobjekt. Wenn Sie mehrere Medienobjekte verketten, berücksichtigen Sie die Quellauflösung jeder Eingabedatei. Wenn sich die Quellauflösungen unterscheiden, wird die Eingabe mit der niedrigeren Auslösung hochskaliert, um der Auflösung des Medienobjekts mit der höchsten Auflösung zu entsprechen. Um die Ausgabe des Schnittauftrags in der Vorschau anzuzeigen, wählen Sie die Schaltfläche für die Vorschau aus. Der Clip wird zwischen den ausgewählten Start- und Endzeiten wiedergegeben.

## <a name="producing-dynamic-manifest-filters"></a>Erstellen von dynamischen Manifestfiltern
[Dynamische Manifestfilter](https://azure.microsoft.com/blog/dynamic-manifest/) beschreiben einen Satz von Regeln, die auf Manifestattributen und Medienobjekt-Zeitachsen basieren. Diese Regeln legen fest, wie sich der Streamingendpunkt auf die Wiedergabeliste der Ausgabe auswirkt (Manifest). Mithilfe des Filters können die Segmente geändert werden, die für die Wiedergabe gestreamt werden. Die von Clipper erstellten Filter sind lokal und spezifisch für das Quellmedienobjekt. Im Gegensatz zu gerenderten Clips sind Filter keine neuen Medienobjekte und erfordern keinen Codierungsauftrag für die Erstellung. Sie können über das [.NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-dynamic-manifest) oder die [REST-API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest) schnell erstellt werden, bieten jedoch nur GOP-Genauigkeit. In der Regel weisen Medienobjekte, die für das Streaming codiert wurden, eine GOP-Größe von zwei Sekunden auf.

Um einen dynamischen Manifestfilter zu erstellen, wählen Sie im Menü „Erweiterte Einstellungen“ den dynamischen Manifestfilter als Schnittmodus aus. Sie können die gleichen Schritte ausführen, mit denen Sie einen Clip produzieren, um den Filter zu erstellen. Filter können nur für ein einziges Medienobjekt erstellt werden.

## <a name="submitting-clipping-jobs"></a>Übermitteln von Schnittaufträgen
Wenn Sie die Zusammenstellung des Clips abgeschlossen haben, wählen Sie die Schaltfläche zum Übermitteln von Aufträgen, um den zugehörigen Schnittauftrag oder den dynamischen Manifestaufruf zu initiieren.