---
title: Machine Learning Services (mit R) in Azure SQL-Datenbank (Vorschauversion) – Übersicht
description: In diesem Thema werden die Machine Learning Services (mit R) in Azure SQL-Datenbank und ihre Funktionsweise erläutert.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 01/31/2019
ms.openlocfilehash: e2159e7cc59830c3d0d10f1c5b9697ab5b45b666
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824929"
---
# <a name="machine-learning-services-with-r-in-azure-sql-database-preview"></a>Machine Learning Services (mit R) in Azure SQL-Datenbank (Vorschauversion)

Machine Learning Services ist eine Funktion von Azure SQL-Datenbank zum Ausführen von R-Skripts in der Datenbank. Die Funktion umfasst Microsoft-R-Pakete für leistungsstarke Predictive Analytics und maschinelles Lernen. Die relationalen Daten können über gespeicherte Prozeduren, R-Anweisungen in T-SQL-Skripts oder R-Code mit T-SQL in R-Skripts verwendet werden.

> [!NOTE]
> Machine Learning Services (mit R) in Azure SQL-Datenbank befindet sich derzeit in der öffentlichen Vorschau. Sie können sich unten [für die Vorschau registrieren](#signup).

## <a name="what-you-can-do-with-r"></a>Einsatzmöglichkeiten für R

Nutzen Sie die Leistung der Sprache R für erweiterte Analysen und maschinelles Lernen in der Datenbank. Diese Fähigkeit ermöglicht Berechnungen und die Verarbeitung an dem Ort, an dem sich die Daten befinden, sodass diese nicht mehr über das Netzwerk gepullt werden müssen. Nutzen Sie die Leistungsfähigkeit der R-Pakete für Unternehmen außerdem, um erweiterte Analysen in einem großen Maßstab durchzuführen.

Machine Learning Services umfasst eine Basisdistribution von R zusammen mit R-Paketen für Unternehmen von Microsoft. Die R-Funktionen und -Algorithmen von Microsoft sind auf Skalierbarkeit und Anwendbarkeit ausgelegt. Sie ermöglichen Predictive Analytics, statistische Modellierung, Datenvisualisierungen und leistungsstarke Algorithmen für maschinelles Lernen.

### <a name="r-packages"></a>R-Pakete

Die am häufigsten verwendeten Open-Source-R-Pakete sind bei Machine Learning Services bereits vorinstalliert. Die folgenden R-Pakete von Microsoft sind ebenfalls enthalten:

| R-Paket | BESCHREIBUNG|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open ist die erweiterte R-Distribution von Microsoft. Es handelt sich um eine vollständige Open-Source-Plattform für statistische Analysen und Data Science. Sie basiert auf R und ist damit zu 100 % kompatibel. Darüber hinaus bietet sie zusätzliche Funktionen für mehr Leistung und eine bessere Reproduzierbarkeit. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | RevoScaleR ist die primäre Bibliothek für skalierbares R. Die Funktionen in dieser Bibliothek gehören zu den am häufigsten verwendeten. Datentransformationen und -manipulationen, statistische Zusammenfassung, Visualisierung und viele Formen der Modellierung und Analyse sind in diesen Bibliotheken enthalten. Darüber hinaus verteilen die Funktionen in diesen Bibliotheken die Workloads automatisch auf die verfügbaren Kerne, um die Verarbeitung zu parallelisieren, und sie bieten die Möglichkeit, die Daten in Blöcken zu verarbeiten, die von der Berechnungs-Engine koordiniert und verwaltet werden. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | MicrosoftML fügt Algorithmen für maschinelles Lernen hinzu, mit denen Sie benutzerdefinierte Modelle für die Text-, Bild- und Standpunktanalyse erstellen können. |

Zusätzlich zu den vorinstallierten Paketen können Sie außerdem [weitere Pakete installieren](sql-database-connect-query-r.md#add-package).

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Anmelden für die Vorschau

Führen Sie die folgenden Schritte aus, um sich für die öffentliche Vorschauversion zu registrieren:

1. Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/), bevor Sie beginnen.

2. Senden Sie unter [sqldbml@microsoft.com](mailto:sqldbml@microsoft.com) eine E-Mail an Microsoft, um sich für die öffentliche Vorschauversion zu registrieren. Die öffentliche Vorschauversion von Machine Learning Services (mit R) in SQL-Datenbank ist standardmäßig nicht aktiviert.

Nachdem Sie für das Programm registriert wurden, führt Microsoft für Sie das Onboarding für die öffentliche Vorschauversion durch und aktiviert R für Ihre vorhandenen oder neuen Datenbanken.

Machine Learning Services (mit R) in SQL-Datenbank ist derzeit nur im V-Kern-basierten Kaufmodell für die Diensttarife **Universell** und **Unternehmenskritisch** für Bereitstellungen als Singleton oder als Pool für elastische Datenbanken verfügbar. In dieser ersten öffentlichen Vorschauversion werden weder der Diensttarif **Hyperscale** noch die Bereitstellungsoption **Verwaltete Instanz** unterstützt.

R ist derzeit die einzige unterstützte Sprache. Momentan ist keine Unterstützung für Python vorhanden. Die Vorschau ist anfänglich in den folgenden Regionen verfügbar: „Europa, Westen“, „Europa, Norden“, „USA, Westen 2“, „USA, Osten“, „USA, Süden-Mitte“, „USA, Norden-Mitte“, „Kanada, Mitte“, „Asien, Südosten“, „Indien, Süden“ und „Australien, Südosten“. Weitere Regionen werden später hinzugefügt.

Verwenden Sie Machine Learning Services mit R während der öffentlichen Vorschauphase nicht für Produktionsworkloads.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Key differences between Machine Learning Services in Azure SQL Database and SQL Server](sql-database-machine-learning-services-differences.md) (Wichtige Unterschiede zwischen Machine Learning Services in Azure SQL-Datenbank und SQL Server).
- Informationen zum Verwenden von Machine Learning Services (mit R) in Azure SQL-Datenbank finden Sie unter [Schnellstart](sql-database-connect-query-r.md).
- Erfahren Sie mehr mit [Tutorials für die R-Sprache von SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials).