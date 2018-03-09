---
title: "Übersicht über Microsoft Azure Data Lake Analytics | Microsoft Docs"
description: "Data Lake Analytics ist ein Azure Big Data-Dienst zur Nutzung von Daten für die Betriebsführung auf Basis der aus Ihren Clouddaten ermittelten Erkenntnisse – unabhängig von Größe und Speicherort der Daten."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 316c35fa4b04bdb251c2b9ae14f6b32f4e4bf939
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Übersicht über Microsoft Azure Data Lake Analytics

## <a name="what-is-azure-data-lake-analytics"></a>Was ist Azure Data Lake Analytics?
Azure Data Lake Analytics ist ein bedarfsgesteuerter Dienst für Analyseaufträge zum Vereinfachen von Big Data-Vorgängen. Anstatt sich der Bereitstellung, Konfiguration und Optimierung von Hardware zu widmen, schreiben Sie Abfragen, mit denen Sie Ihre Daten transformieren und nützliche Einblicke erhalten. Der Analysedienst ist in der Lage, umgehend Aufträge jeglicher Größenordnung zu verarbeiten. Wählen Sie dazu die jeweils erforderliche Ressourcenkapazität aus. Da Sie nur für die Leistung bezahlen, die während der Ausführung Ihres Auftrags tatsächlich in Anspruch genommen wurde, ist dies eine überaus kosteneffektive Lösung. Der Analysedienst unterstützt „U-SQL“. Bei dieser Sprache wurden die Vorteile von SQL mit der Leistungsstärke von imperativem Code kombiniert. Mit U-SQL können Sie Daten übergreifend für Data Lake Store, SQL Server in Azure, Azure SQL-Datenbank und Azure SQL Data Warehouse analysieren.

## <a name="dynamic-scaling"></a>Dynamische Skalierung
  
Data Lake Analytics wurde für eine hohe Leistung in der Cloud entwickelt und optimiert.  Profitieren Sie von einer dynamischen Ressourcenbereitstellung und der Möglichkeit, Daten im Terabyte- oder sogar Exabytebereich zu analysieren. Sobald ein Auftrag abgeschlossen ist, werden die Ressourcen automatisch herunterskaliert, und Sie bezahlen nur für die tatsächlich in Anspruch genommene Verarbeitungsleistung. Außerdem sind keine Codeänderungen notwendig, wenn Sie die Menge an gespeicherten Daten oder die genutzten Computeressourcen erhöhen oder verringern. So können Sie sich ausschließlich auf Ihre Geschäftslogik konzentrieren und müssen sich keinerlei Gedanken darüber machen, wie die großen Datenmengen verarbeitet oder gespeichert werden.

## <a name="develop-faster-debug-and-optimize-smarter-using-familiar-tools"></a>Schnelleres Entwickeln, Debuggen und Optimieren von Code mithilfe vertrauter Tools
  
Data Lake Analytics ist nahtlos in Visual Studio integriert – dadurch können Sie für die Ausführung, das Debugging und die Optimierung Ihres Codes vertraute Tools nutzen. Mithilfe von Visualisierungen Ihrer U-SQL-Aufträge können Sie ermitteln, wie Ihr Code bei der Skalierung ausgeführt wird. Anhand dieser Einblicke lassen sich Leistungsengpässe leicht ermitteln und Kosten optimieren.


## <a name="u-sql-simple-and-familiar-powerful-and-extensible"></a>U-SQL: einfach und vertraut, leistungsstark und erweiterbar
  
Data Lake Analytics enthält U-SQL, eine Abfragesprache, bei der die bekannte, einfache deklarative SQL-Sprache mit den Ausdrücken von C# kombiniert wird. U-SQL basiert auf derselben verteilten Laufzeit, die auch den Big Data-Systemen bei Microsoft zugrunde liegt. Millionen von SQL- und .NET-Entwickler können ihre Daten nun anhand bereits vorhandener Fähigkeiten verarbeiten und analysieren.

## <a name="integrates-seamlessly-with-your-it-investments"></a>Nahtlose Integration in Ihre vorhandenen IT-Systeme
  
Data Lake Analytics kann vorhandene IT-Systeme für Identitäten, Verwaltung, Sicherheit und Data Warehousing verwenden. Dieser Ansatz trägt zur Vereinfachung von Datengovernance und zur problemlosen Erweiterung aktueller Datenanwendungen dar. Data Lake Analytics ist für die Benutzerverwaltung und Berechtigungsvergabe in Active Directory integriert und enthält Überwachungs- und Auditierungsfunktionen.

## <a name="affordable-and-cost-effective"></a>Erschwinglich und kosteneffektiv

Data Lake Analytics ist eine kosteneffektive Lösung für die Ausführung von Big Data-Workloads. Die Bezahlung erfolgt bei Datenverarbeitung pro Auftrag. Sie benötigen weder Hardware noch Lizenzen oder dienstspezifische Supportvereinbarungen. Wenn Aufträge gestartet oder beendet werden, wird das System automatisch zentral hoch- oder herunterskaliert. So bezahlen Sie stets nur für die tatsächlich in Anspruch genommene Ressourcenmenge. [Weitere Informationen zu Kostenkontrolle und -einsparung](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c).
    
## <a name="works-with-all-your-azure-data"></a>Funktioniert mit allen Ihren Daten in Azure
  
Data Lake Analytics kann mit Azure Data Lake Store verwendet werden, erzielt die höchstmögliche Leistung, den höchsten Durchsatz und optimale Parallelisierung und ist mit Azure Storage-Blobs, Azure SQL-Datenbank und Azure Warehouse kompatibel.

## <a name="next-steps"></a>Nächste Schritte
 
  * Erste Schritte mit Data Lake Analytics mithilfe [des Azure-Portals](data-lake-analytics-get-started-portal.md) | [von Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [der CLI](data-lake-analytics-get-started-cli2.md)
  * Verwalten von Azure Data Lake Analytics mithilfe [des Azure-Portals](data-lake-analytics-manage-use-portal.md) | [von Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [von CLI](data-lake-analytics-manage-use-cli.md) | [des Azure .NET-SDKs](data-lake-analytics-manage-use-dotnet-sdk.md) | [von Node.js](data-lake-analytics-manage-use-nodejs.md)
  * [Kostenkontrolle und -einsparung mit Data Lake Analytics](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c)
