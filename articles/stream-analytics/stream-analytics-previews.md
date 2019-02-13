---
title: Azure Stream Analytics – Vorschaufeatures
description: Dieser Artikel beschreibt die Azure Stream Analytics-Features, die sich derzeit in der Vorschau befinden.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/05/2019
ms.openlocfilehash: 09f1bdfa4c9a7a179bddf9473b553924bfb58fb7
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768561"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure Stream Analytics – Vorschaufeatures

Dieser Artikel fasst alle Features zusammen, die sich derzeit in der Vorschau für Azure Stream Analytics befinden. Die Verwendung von Vorschaufeatures in einer Produktionsumgebung wird nicht empfohlen.

## <a name="public-previews"></a>Öffentliche Vorschauversionen

Die folgenden Features befinden sich in der öffentlichen Vorschau. Sie können diese Features heute nutzen, sollten sie aber nicht in Ihrer Produktionsumgebung einsetzen.

### <a name="sql-database-reference-data"></a>SQL-Datenbank-Referenzdaten

Azure Stream Analytics unterstützt Azure SQL-Datenbank als Eingangsquelle für Verweisdaten. Sie können SQL-Datenbank mit den Stream Analytics-Tools als Verweisdaten für Ihren Stream Analytics-Auftrag im Azure-Portal und in Visual Studio verwenden. Weitere Informationen finden Sie unter [Verwenden von Verweisdaten aus einer SQL-Datenbank für einen Azure Stream Analytics-Auftrag](sql-reference-data.md).

### <a name="integration-with-azure-machine-learning"></a>Integration mit Azure Machine Learning

Sie können Stream Analytics-Aufträge mit Machine Learning (ML)-Funktionen skalieren. Um weitere Informationen dazu, wie Sie ML-Funktionen in Ihrem Stream Analytics-Auftrag verwenden können, finden Sie unter [skalieren ein Stream Analytics-Auftrags mit Azure Machine Learning-Funktionen](stream-analytics-scale-with-machine-learning-functions.md). Schauen Sie sich ein praktisches Szenario an in [Durchführen von Standpunktanalysen mit Azure Stream Analytics und Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="javascript-user-defined-aggregate"></a>Benutzerdefinierte Aggregate in JavaScript

Azure Stream Analytics unterstützt in JavaScript geschriebene, benutzerdefinierte Aggregate (UDA), mit deren Hilfe Sie komplexe zustandsbehaftete Geschäftslogik implementieren können. Weitere Informationen zur Verwendung von UDAs finden Sie in der Dokumentation [Azure Stream Analytics – benutzerdefinierte JavaScript-Aggregate](stream-analytics-javascript-user-defined-aggregates.md) Dokumentation. 

### <a name="live-data-testing-in-visual-studio"></a>Livedatentests in Visual Studio

Visual Studio-Tools für Azure Stream Analytics erweitern die lokale Testfunktion, mit der Sie Ihre Abfragen anhand von Liveereignisstreams aus Cloudquellen wie Event Hub oder IoT Hub testen können. Erfahren Sie mehr über [Lokales Testen von Livedaten mithilfe von Azure Stream Analytics-Tools für Visual Studio](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>Benutzerdefinierte NET-Funktionen auf IoT Edge

Mit benutzerdefinierten .NET-Standardfunktionen können Sie .NET-Standardcode als Teil Ihrer Streamingpipeline ausführen. Sie können einfache C#-Klassen erstellen oder komplette Projekte und Bibliotheken importieren. In Visual Studio wird das vollständige Erstellen von Dokumenten und Debuggen unterstützt. Weitere Informationen finden Sie unter [Entwickeln von benutzerdefinierten .NET Standard-Funktionen für Azure Stream Analytics Edge-Aufträge](stream-analytics-edge-csharp-udf-methods.md).

## <a name="private-previews"></a>Private Vorschauversionen

Die folgenden Features befinden sich in der privaten Vorschau.

### <a name="anomaly-detection"></a>Erkennung von Anomalien

Azure Stream Analytics stellt neue Machine Learning-Modelle vor, die neben der bidirektionalen, langsam positiven und langsam negativen Trenderkennung auch die Erkennung von *Spikes* und *Dips* unterstützen.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge"></a>Benutzerdefinierter C#-Deserialisierer für Azure Stream Analytics auf IoT Edge

Entwickler können nun benutzerdefinierte Deserialisierer in C# implementieren, um Ereignisse zu deserialisieren, die von Azure Stream Analytics empfangen werden. Beispiele für Formate, die deserialisiert werden können, sind Parquet, Protobuf, XML oder ein beliebiges Binärformat.

### <a name="managed-identities-for-azure-resource-authentication-to-azure-data-lake-storage"></a>Verwaltete Identitäten für die Azure-Ressourcenauthentifizierung bei Azure Data Lake Storage

Sie können jetzt Ihre Echtzeitpipelines mit verwalteten Identitäten für die ressourcenbasierte Authentifizierung von Azure beim Schreiben in Azure Data Lake Storage Gen1 operationalisieren, sodass Sie Aufträge programmgesteuert erstellen können. Weitere Informationen finden Sie unter [Verwenden von verwalteten Identitäten für Azure-Ressourcen zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Azure Data Lake Storage Gen1-Ausgabe](stream-analytics-managed-identities-adls.md).

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code für Azure Stream Analytics

Azure Stream Analytics-Aufträge können in Visual Studio Code erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Acht neue Features in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/)

* [Vier neue Features, die jetzt in Azure Stream Analytics verfügbar sind](https://azure.microsoft.com/blog/4-new-features-now-available-in-azure-stream-analytics/)
