---
title: Importieren von Log Analytics-Daten in Power BI | Microsoft Dokumentation
description: Power BI ist ein cloudbasierter Business Analytics-Dienst von Microsoft, der umfassende Visualisierungen und Berichte zur Analyse verschiedener Datensätze bietet.  In diesem Artikel wird beschrieben, wie Log Analytics-Daten konfiguriert und in Power BI importiert werden und wie die automatische Aktualisierung konfiguriert wird.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 7610fcfc144e4337f8556c8824dc7d104265bcc4
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044551"
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Importieren von Log Analytics-Daten in Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) ist ein cloudbasierter Business Analytics-Dienst von Microsoft, der umfassende Visualisierungen und Berichte zur Analyse verschiedener Datensätze bietet.  Sie können die Ergebnisse einer Log Analytics-Suche in ein Power BI-Dataset importieren, damit Sie Features wie das Durchsuchen von Daten aus verschiedenen Quellen und das Freigeben von Berichten im Web und auf mobilen Geräten nutzen können.

## <a name="overview"></a>Übersicht
Zum Importieren von Daten aus einem Log Analytics-Arbeitsbereich in Power BI erstellen Sie in Power BI basierend auf einer Protokollsuchabfrage in Log Analytics ein Dataset.  Die Abfrage wird bei jeder Aktualisierung des Datasets ausgeführt.  Sie können dann Power BI-Berichte erstellen, die Daten aus dem Dataset verwenden.  Zum Erstellen des Datasets in Power BI exportieren Sie Ihre Abfrage aus Log Analytics in die [Power Query-Sprache (M) ](https://msdn.microsoft.com/library/mt807488.aspx).  Diese verwenden Sie dann zum Erstellen einer Abfrage in Power BI Desktop, die Sie anschließend als Dataset in Power BI veröffentlichen.  Die Details für diesen Prozess werden nachfolgend beschrieben.

![Von Log Analytics nach Power BI](media/log-analytics-powerbi/overview.png)

## <a name="export-query"></a>Exportieren einer Abfrage
Erstellen Sie zunächst eine [Protokollsuche](log-analytics-log-search-new.md), welche die Daten von Log Analytics zurückgibt, mit denen das Power BI-Dataset aufgefüllt werden soll.  Anschließend exportieren Sie die Abfrage in der [Power Query-Sprache (M)](https://msdn.microsoft.com/library/mt807488.aspx), die von Power BI Desktop verwendet werden kann.

1. Erstellen Sie die Protokollsuche in Log Analytics zum Extrahieren der Daten für das Dataset.
2. Wenn Sie das Protokollsuchportal verwenden, klicken Sie auf **Power BI**.  Wenn Sie das Analytics-Portal verwenden, wählen Sie **exportieren** > **Power BI Query (M)**.  Mit beiden Optionen wird die Abfrage in eine Textdatei namens **PowerBIQuery.txt** exportiert. 

    ![Exportieren einer Protokollsuche](media/log-analytics-powerbi/export-logsearch.png) ![Exportieren einer Protokollsuche](media/log-analytics-powerbi/export-analytics.png)

3. Öffnen Sie die Textdatei, und kopieren Sie den Inhalt.

## <a name="import-query-into-power-bi-desktop"></a>Importieren einer Abfrage in Power BI Desktop
Power BI Desktop ist eine Desktopanwendung, die Ihnen das Erstellen von Datasets und Berichten ermöglicht, die in Power BI veröffentlicht werden können.  Sie können damit auch eine Abfrage in der aus Log Analytics exportierten Power Query-Sprache erstellen. 

1. Installieren Sie [Power BI Desktop](https://powerbi.microsoft.com/desktop/), wenn Sie es noch nicht haben, und öffnen Sie die Anwendung.
2. Wählen Sie **Daten abrufen** > **Leere Abfrage**, um eine neue Abfrage zu öffnen.  Wählen Sie dann **Erweiterter Editor**, und fügen Sie den Inhalt der exportierten Datei in die Abfrage ein. Klicken Sie auf **Fertig**.

    ![Power BI Desktop-Abfrage](media/log-analytics-powerbi/desktop-new-query.png)

5. Die Abfrage wird ausgeführt, und die Ergebnisse werden angezeigt.  Sie werden möglicherweise aufgefordert, Anmeldeinformationen zum Verbinden mit Azure einzugeben.  
6. Geben Sie einen beschreibenden Namen für die Abfrage ein.  Der Standardwert lautet **Abfrage1**. Klicken Sie auf **Schließen und übernehmen**, um das Dataset dem Bericht hinzuzufügen.

    ![Power BI Desktop-Name](media/log-analytics-powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Veröffentlichen in Power BI
Wenn Sie in Power BI veröffentlichen, werden ein Dataset und ein Bericht erstellt.  Wenn Sie einen Bericht in Power BI Desktop erstellen, wird dieser mit Ihren Daten veröffentlicht.  Wenn nicht, wird ein leerer Bericht erstellt.  Sie können den Bericht in Power BI ändern oder einen neuen auf Basis des Datasets erstellen.

8. Erstellen Sie einen Bericht auf Basis Ihrer Daten.  Wenn Sie mit Power BI Desktop nicht vertraut sind, lesen Sie in der [Dokumentation](https://docs.microsoft.com/power-bi/desktop-report-view) dazu nach.  Wenn bereit sind, ihn an Power BI zu senden, klicken Sie auf **Veröffentlichen**.  Wählen Sie ein Ziel in Ihrem Power BI-Konto aus, wenn Sie dazu aufgefordert werden.  Sofern Sie kein bestimmtes Ziel verwenden möchten, verwenden Sie **Mein Arbeitsbereich**.

    ![Veröffentlichen in Power BI Desktop](media/log-analytics-powerbi/desktop-publish.png)

3. Wenn die Veröffentlichung abgeschlossen ist, klicken Sie auf **In Power BI öffnen**, um Power BI mit Ihrem neuen Dataset zu öffnen.


### <a name="configure-scheduled-refresh"></a>Konfigurieren von geplanten Aktualisierungen
Das in Power BI erstellte Dataset enthält dieselben Daten, die zuvor in Power BI Desktop angezeigt wurden.  Sie müssen das Dataset in regelmäßigen Abständen aktualisieren, um die Abfrage erneut auszuführen und es mit den aktuellen Daten aus Log Analytics zu füllen.  

1. Klicken Sie auf den Arbeitsbereich, in dem Sie Ihren Bericht hochgeladen haben, und wählen Sie dann das Menü **Datasets**. Wählen Sie das Kontextmenü neben dem neuen Dataset und dann **Einstellungen** aus. Unter **Anmeldeinformationen für die Datenquelle** sollte eine Meldung angezeigt werden, dass die Anmeldeinformationen ungültig sind.  Dies liegt daran, dass Sie noch keine Anmeldeinformationen für das Dataset zum Aktualisieren seiner Daten bereitgestellt haben.  Klicken Sie auf **Anmeldeinformationen bearbeiten**, und geben Sie Anmeldeinformationen mit Zugriff auf Log Analytics an.

    ![Power BI-Zeitplan](media/log-analytics-powerbi/powerbi-schedule.png)

5. Legen Sie unter **Geplante Aktualisierung** die Option **Halten Sie Ihre Daten aktuell** fest.  Sie haben die Möglichkeit, die **Aktualisierungshäufigkeit** und eine oder mehrere bestimmte Uhrzeiten für die Ausführung der Aktualisierung zu ändern.

    ![Power BI-Aktualisierung](media/log-analytics-powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr zu [Protokollsuchvorgängen](log-analytics-log-searches.md) zum Erstellen von Abfragen, die nach Power BI exportiert werden können.
* Erfahren Sie mehr über [Power BI](http://powerbi.microsoft.com), um Visualisierungen anhand von Log Analytics-Exporten zu erstellen.
