---
title: Azure Stream Analytics – Vorschaufeatures
description: Dieser Artikel beschreibt die Azure Stream Analytics-Features, die sich derzeit in der Vorschau befinden.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.openlocfilehash: 637afab45e04c68777f8d1b42817c912cdc09941
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53133740"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics – Vorschaufeatures

Dieser Artikel fasst alle Features zusammen, die sich derzeit in der Vorschau für Azure Stream Analytics befinden. Die Verwendung von Vorschaufeatures in einer Produktionsumgebung wird nicht empfohlen.

## <a name="public-previews"></a>Öffentliche Vorschauversionen

Die folgenden Features befinden sich in der öffentlichen Vorschau. Sie können diese Features heute nutzen, sollten sie aber nicht in Ihrer Produktionsumgebung einsetzen.

### <a name="integration-with-azure-machine-learning"></a>Integration mit Azure Machine Learning

Sie können Stream Analytics-Aufträge mit Machine Learning (ML)-Funktionen skalieren. Um weitere Informationen dazu, wie Sie ML-Funktionen in Ihrem Stream Analytics-Auftrag verwenden können, finden Sie unter [skalieren ein Stream Analytics-Auftrags mit Azure Machine Learning-Funktionen](stream-analytics-scale-with-machine-learning-functions.md). Schauen Sie sich ein praktisches Szenario an in [Durchführen von Standpunktanalysen mit Azure Stream Analytics und Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="session-windows"></a>Sitzungsfenster

Stream Analytics verfügt über native Unterstützung für Windowing-Funktionen, sodass Entwickler komplexe Streaming-Verarbeitungsaufträge mit sehr geringem Aufwand erstellen können. Mit Gruppenereignissen für [Sitzungsfenster](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics) werden Ereignisse gruppiert, die zu ähnlichen Zeiten eingehen. Zeiträume, in denen keine Daten anfallen, werden herausgefiltert. Weitere Informationen zu Windowing-Funktionen finden Sie auf [Einführung in die Stream Analytics-Windowing-Funktionen](stream-analytics-window-functions.md).

### <a name="blob-output-partitioning-by-custom-time"></a>Blob-Ausgabepartitionierung nach benutzerdefinierter Zeit

Azure Stream Analytics können in Blob Storage basierend auf benutzerdefinierten Zeitattributen ausgegeben werden. Weitere Informationen finden Sie in [Benutzerdefinierte DateTime-Pfadmuster für die Blob Storage-Ausgabe von Azure Stream Analytics](stream-analytics-custom-path-patterns-blob-storage-output.md).

### <a name="javascript-user-defined-aggregate"></a>Benutzerdefinierte Aggregate in JavaScript

Azure Stream Analytics unterstützt in JavaScript geschriebene, benutzerdefinierte Aggregate (UDA), mit deren Hilfe Sie komplexe zustandsbehaftete Geschäftslogik implementieren können. Weitere Informationen zur Verwendung von UDAs finden Sie in der Dokumentation [Azure Stream Analytics – benutzerdefinierte JavaScript-Aggregate](stream-analytics-javascript-user-defined-aggregates.md) Dokumentation. 

### <a name="live-data-testing-in-visual-studio"></a>Livedatentests in Visual Studio

Visual Studio-Tools für Azure Stream Analytics erweitern die lokale Testfunktion, mit der Sie Ihre Abfragen anhand von Liveereignisstreams aus Cloudquellen wie Event Hub oder IoT Hub testen können. Erfahren Sie mehr über [Lokales Testen von Livedaten mithilfe von Azure Stream Analytics-Tools für Visual Studio](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>Benutzerdefinierte NET-Funktionen auf IoT Edge

Mit benutzerdefinierten .NET-Standardfunktionen können Sie .NET-Standardcode als Teil Ihrer Streamingpipeline ausführen. Sie können einfache C#-Klassen erstellen oder komplette Projekte und Bibliotheken importieren. In Visual Studio wird das vollständige Erstellen von Dokumenten und Debuggen unterstützt. Weitere Informationen finden Sie unter [Entwickeln von benutzerdefinierten .NET Standard-Funktionen für Azure Stream Analytics Edge-Aufträge](stream-analytics-edge-csharp-udf-methods.md).

## <a name="private-previews"></a>Private Vorschauversionen

Die folgenden Features befinden sich in der privaten Vorschau. Um auf diese Vorschauen zuzugreifen, besuchen Sie die Seite [Anmeldung](https://aka.ms/ASApreview1) für die private Vorschau von Azure Stream Analytics.

### <a name="anomaly-detection"></a>Erkennung von Anomalien

Azure Stream Analytics stellt neue Machine Learning-Modelle vor, die neben der bidirektionalen, langsam positiven und langsam negativen Trenderkennung auch die Erkennung von *Spikes* und *Dips* unterstützen.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge"></a>Benutzerdefinierter C#-Deserialisierer für Azure Stream Analytics auf IoT Edge

Entwickler können nun benutzerdefinierte Deserialisierer in C# implementieren, um Ereignisse zu deserialisieren, die von Azure Stream Analytics empfangen werden. Beispiele für Formate, die deserialisiert werden können, sind Parquet, Protobuf, XML oder ein beliebiges Binärformat.

### <a name="blob-output-partitioning-by-custom-attribute"></a>Blob-Ausgabepartitionierung nach benutzerdefiniertem Attribut

Es ist nun möglich, die Ausgabe von Azure Stream Analytics auf Blob Storage zu partitionieren, basierend auf einer beliebigen Spalte in Ihrer Abfrage.

### <a name="managed-identities-for-azure-resources-authentication-to-azure-data-lake-storage"></a>Verwaltete Identitäten für die Azure-Ressourcenauthentifizierung mit zu Azure Data Lake Storage

Sie können jetzt Ihre Echtzeitpipelines mit verwalteten Identitäten für die ressourcenbasierte Authentifizierung von Azure beim Schreiben in Azure Data Lake Storage Gen1 operationalisieren, sodass Sie Aufträge programmgesteuert erstellen können. Weitere Informationen finden Sie unter [Verwenden von verwalteten Identitäten für Azure-Ressourcen zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Azure Data Lake Storage Gen1-Ausgabe](stream-analytics-managed-identities-adls.md).

## <a name="next-steps"></a>Nächste Schritte

* [Acht neue Features in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/)

* [4 neue Features jetzt in Azure Stream Analytics verfügbar](https://azure.microsoft.com/blog/4-new-features-now-available-in-azure-stream-analytics/)
