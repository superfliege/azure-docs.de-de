---
title: Verbinden mit einer Datenquelle in Power BI-Arbeitsbereichssammlungen | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie in Power BI-Arbeitsbereichssammlungen eine Verbindung zu Datenquellen herstellen können."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: 24600c4343e3bfebe14f25020c5a7ba02d15af64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-a-data-source"></a>Herstellen einer Verbindung mit einer Datenquelle

Mit **Power BI-Arbeitsbereichssammlungen**können Sie Berichte in Ihre eigene Anwendung einbetten. Wenn Sie einen Power BI-Bericht in Ihre App einbetten, stellt der Bericht eine Verbindung mit den zugrunde liegenden Daten her, indem er eine Kopie der Daten **importiert** oder über **DirectQuery** eine **direkte Verbindung** mit der Datenquelle herstellt.

> [!IMPORTANT]
> Power BI-Arbeitsbereichssammlungen sind veraltet und nur noch bis Juni 2018 (oder bis zum Termin in Ihrem Vertrag) verfügbar. Es wird empfohlen, die Migration zu Power BI Embedded zu planen, um Unterbrechungen in Ihrer Anwendung zu vermeiden. Informationen zur Migration Ihrer Daten zu Power BI Embedded finden Sie unter [Migrieren von Inhalten aus Power BI-Arbeitsbereichssammlungen zu Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Im Folgenden werden die Unterschiede zwischen der Verwendung des **Importvorgangs** und der Verwendung von **DirectQuery** aufgeführt.

| Importieren | DirectQuery |
| --- | --- |
| Tabellen, Spalten *und Daten* werden in das Dataset des Berichts importiert oder kopiert. Um Änderungen anzuzeigen, die an den zugrunde liegenden Daten vorgenommen wurden, müssen Sie ein Dataset aktualisieren bzw. ein vollständiges aktuelles Dataset importieren. |Nur *Tabellen und Spalten* werden in das Dataset des Berichts importiert oder kopiert. Sie sehen immer die aktuellen Daten. |

Mit Power BI-Arbeitsbereichssammlungen können Sie DirectQuery mit Clouddatenquellen verwenden, allerdings derzeit nicht mit lokalen Datenquellen.

> [!NOTE]
> Das lokale Datengateway wird derzeit nicht mit Power BI-Arbeitsbereichssammlungen unterstützt. Das bedeutet, dass Sie DirectQuery nicht mit lokalen Datenquellen verwenden können.

## <a name="supported-data-sources"></a>Unterstützte Datenquellen

**DirectQuery**
* Azure SQL-Datenbank
* Azure SQL Data Warehouse

**Import**

Sie können den Import unter Verwendung aller in Power BI Desktop verfügbaren Datenquellen durchführen. Es ist jedoch **nicht** möglich, diese Daten in Power BI-Arbeitsbereichssammlungen zu aktualisieren. Stattdessen müssen Sie Änderungen an der PBIX-Datei in die Power BI-Arbeitsbereichssammlungen hochladen. Dies liegt daran, dass kein Gateway verfügbar ist. 

## <a name="benefits-of-using-directquery"></a>Vorteile der Verwendung von DirectQuery

Die Verwendung von **DirectQuery**bietet zwei wesentliche Vorteile:

* Mithilfe von **DirectQuery** können Sie Visualisierungen über umfangreiche Datasets in Fällen erstellen, in denen es ansonsten unmöglich wäre, sämtliche Daten zuerst zu importieren.
* Bei Änderungen an den zugrundeliegenden Daten kann eine Datenaktualisierung erforderlich sein, und für einige Berichte, die stets aktuelle Daten darstellen müssen, sind eventuell umfangreiche Datenübertragungen erforderlich, sodass das erneute Importieren von Daten nicht durchführbar ist. Im Gegensatz dazu werden in **DirectQuery** -Berichten immer aktuelle Daten verwendet.

## <a name="limitations-of-directquery"></a>Einschränkungen von DirectQuery

Es gibt ein paar Einschränkungen bei der Verwendung von **DirectQuery**:

* Alle Tabellen müssen aus einer einzigen Datenbank stammen.
* Wenn die Abfrage sehr komplex ist, tritt ein Fehler auf. Um den Fehler zu beheben, müssen Sie die Abfrage so umgestalten, dass sie weniger komplex ist. Wenn die Abfrage komplex sein muss, müssen Sie die Daten importieren, anstatt **DirectQuery** einzusetzen.
* Das Filtern von Beziehungen ist auf eine Richtung beschränkt, es können nicht beide Richtungen verwendet werden.
* Sie können den Datentyp einer Spalte nicht ändern.
* Standardmäßig gelten Einschränkungen für DAX-Ausdrücke, die in Measures zulässig sind. Informationen hierzu finden Sie unter [DirectQuery und Measures](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery und Measures
Um sicherzustellen, dass die an die zugrunde liegende Datenquelle gesendeten Abfragen eine akzeptable Leistung zeigen, werden den Measures Einschränkungen auferlegt. Bei Verwendung von **Power BI Desktop** können fortgeschrittene Benutzer diese Einschränkung umgehen, indem sie **Datei > Optionen und Einstellungen > Optionen** auswählen. Wählen Sie im Dialogfeld **Optionen** die Option **DirectQuery**, und wählen Sie dann die Option **Unbeschränkte Measures im DirectQuery-Modus zulassen**. Wenn diese Option ausgewählt ist, kann jeder DAX-Ausdruck verwendet werden, der für ein Measure gültig ist. Benutzern muss jedoch bewusst sein, dass einige Ausdrücke, die beim Importieren der Daten eine gute Leistung zeigen, im **DirectQuery**-Modus zu langsamen Abfragen an die Back-End-Datenquelle durchführen können. 

## <a name="see-also"></a>Weitere Informationen

* [Erste Schritte mit Microsoft Power BI-Arbeitsbereichssammlungen](get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

Weitere Fragen? [Power BI-Community ausprobieren](http://community.powerbi.com/)

