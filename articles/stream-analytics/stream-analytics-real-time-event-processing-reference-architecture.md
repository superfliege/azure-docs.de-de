---
title: Echtzeitereignisverarbeitung mithilfe der Azure Stream Analytics-Ereignisverarbeitung
description: Dieser Artikel beschreibt die Referenzarchitektur für die Echtzeitereignisverarbeitung und -analyse mithilfe von Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 8a5d426d67916e010c7fff048eebdc77b93c5c38
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Referenzarchitektur: Echtzeitereignisverarbeitung mit Microsoft Azure Stream Analytics
Die Referenzarchitektur für die Echtzeitereignisverarbeitung mit Azure Stream Analytics soll als allgemeiner Plan dienen, mit dem eine Plattform-as-a-Service (PaaS) für die Datenstromverarbeitung in Echtzeit auf der Basis von Microsoft Azure bereitgestellt werden kann.

## <a name="summary"></a>Zusammenfassung
In der Vergangenheit stützten sich Analyselösungen auf Funktionen, wie z. B. ETL (Extract, Transform, Load) und Datawarehousing, wobei die Daten vor der Analyse gespeichert werden. Veränderte Anforderungen, darunter immer schneller eintreffende Daten, bringen dieses vorhandene Modell an seine Grenzen. Die Möglichkeit, Daten in sich bewegenden Streams vor der Speicherung zu analysieren, ist eine Lösung, und obwohl diese Fähigkeit nicht neu ist, wird der Ansatz in den verschiedenen Zweigen der Branche selten verwendet. 

Microsoft Azure stellt einen umfassenden Katalog von Analysetechnologien bereit, die ein ganzes Spektrum an verschiedenen Lösungsszenarien und Anforderungen unterstützen können. Angesichts der vielfältigen Angebote kann die Auswahl der für eine End-to-End-Lösung bereitzustellenden Azure-Dienste eine Herausforderung darstellen. In diesem Artikel werden die Funktionen und die Interaktion zwischen verschiedenen Azure-Diensten beschrieben, die eine Ereignis-Streaming-Lösung unterstützen. Darüber hinaus werden einige Szenarien erläutert, in denen Kunden von diesem Ansatz profitieren können.

## <a name="contents"></a>Inhalt:
* Kurzfassung
* Einführung in Echtzeitanalysen
* Nutzen von Echtzeitdaten in Azure
* Allgemeine Szenarien für Echtzeitanalysen
* Architektur und Komponenten
  * Datenquellen
  * Datenintegrationsebene
  * Echtzeitanalysenebene
  * Datenspeicherebene
  * Präsentations-/Nutzungsebene
* Zusammenfassung

**Autor:** Charles Feddersen, Lösungsarchitekt, Data Insights Center of Excellence, Microsoft Corporation

**Veröffentlicht:** Januar 2015

**Version:** 1.0

**Download:**[Referenzarchitektur: Echtzeitereignisverarbeitung mit Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um weitere Hilfe zu erhalten, nutzen Sie das [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

