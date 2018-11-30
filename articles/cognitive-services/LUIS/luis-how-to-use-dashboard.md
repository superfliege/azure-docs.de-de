---
title: Anwendungsdashboard für Language Understanding-Apps
titleSuffix: Azure Cognitive Services
description: Erfahren Sie etwas über das Anwendungsdashboard, ein visualisiertes Berichterstellungstool, mit dem Sie Ihre Apps auf einen Blick überwachen können.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 0daf25b3c7edc69ada57fb3ce2b8dbb7e50e5aed
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335009"
---
# <a name="application-dashboard"></a>Anwendungsdashboard
Das App-Dashboard ermöglicht Ihnen, Ihre App auf einen Blick zu überwachen. Das **Dashboard** wird angezeigt, wenn Sie eine App öffnen, indem Sie auf der Seite **Meine Apps** auf den Namen der Anwendung klicken, und dann im oberen Bereich **Dashboard** auswählen. 

> [!CAUTION]
> Wenn Sie die aktuellen Metriken für LUIS abrufen möchten, müssen Sie Folgendes verwenden:
> * Einen LUIS-[Endpunktschlüssel](luis-how-to-azure-subscription.md), der in Azure erstellt wurde
> * Einen LUIS-Endpunktschlüssel für alle Endpunktanforderungen, einschließlich LUIS-[API](https://aka.ms/luis-endpoint-apis) und -Bot
> * Einen separaten Endpunktschlüssel für jede LUIS-App. Verwenden Sie nicht denselben Endpunktschlüssel für alle Apps. Der Endpunktschlüssel wird auf Schlüsselebene nachverfolgt, nicht auf App-Ebene.  

Auf der Seite **Dashboard** erhalten Sie eine Übersicht über die LUIS-App, einschließlich des aktuellen Modellzustands sowie der [Endpunktnutzung](luis-glossary.md#endpoint) über die Zeit. 
  
## <a name="app-status"></a>App-Status
Das Dashboard zeigt den Trainings- und Veröffentlichungsstatus der Anwendung an, einschließlich Datum und Uhrzeit des letzten Trainings und der letzten Veröffentlichung der Anwendung.  

![Dashboard – App-Status](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Modelldatenstatistik
Das Dashboard zeigt die Gesamtanzahl der Absichten, Entitäten und bezeichneten Äußerungen in der App an. 

![App-Datenstatistik](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Endpunkttreffer
Das Dashboard zeigt die gesamten Endpunkttreffer, die die LUIS-App empfängt, an, und ermöglicht Ihnen, die Treffer innerhalb eines angegebenen Zeitraums anzuzeigen. Die angezeigte Gesamtanzahl der Treffer ist die Summe der Endpunkttreffer, bei denen ein [Endpunktschlüssel](./luis-concept-keys.md#endpoint-key) verwendet wird, und der Endpunkttreffer, bei denen ein [Erstellungsschlüssel](./luis-concept-keys.md#authoring-key) verwendet wird.

![Endpunkttreffer](./media/luis-how-to-use-dashboard/dashboard-endpointhits.png)

> [!NOTE] 
> Die aktuelle Endpunkt-Trefferanzahl ist im Azure-Portal in der Übersicht über den LUIS-Dienst zu finden. 
 
### <a name="total-endpoint-hits"></a>Gesamte Endpunkttreffer
Die Gesamtanzahl der Endpunkttreffer, die Ihre App seit der Erstellung bis zum aktuellen Datum erhalten hat.

### <a name="endpoint-hits-per-period"></a>Endpunkttreffer pro Zeitraum
Die Anzahl der innerhalb eines früheren Zeitraums empfangenen Treffer, angezeigt pro Tag. Die Punkte zwischen dem Start- und dem Enddatum stellen die Tage dar, die in diesen Zeitraum fallen. Zeigen Sie auf die einzelnen Punkte, um Anzahl der Treffer an jedem Tag innerhalb des Zeitraums anzuzeigen. 

So wählen Sie einen Punkt zum Anzeigen im Diagramm aus
 
1. Klicken Sie auf **Zusätzliche Einstellungen** ![Schaltfläche „Zusätzliche Einstellungen“](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png), um auf die Liste der Zeiträume zuzugreifen. Sie können Zeiträume zwischen einer Woche und einem Jahr auswählen. 

    ![Endpunkttreffer pro Zeitraum](./media/luis-how-to-use-dashboard/timerange.png)

2. Wählen Sie einen Zeitraum in der Liste aus, und klicken Sie dann auf den Pfeil „Zurück“, ![Pfeil „Zurück“](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) um das Diagramm anzuzeigen.

### <a name="key-usage"></a>Schlüsselverwendung
Die Anzahl der vom Endpunktschlüssel der Anwendung erzielten Treffer. Weitere Informationen zu Endpunktschlüsseln finden Sie unter [Schlüssel in LUIS](luis-concept-keys.md). 
  
## <a name="intent-breakdown"></a>Absichtsaufschlüsselung
Unter **Intent Breakdown** (Absichtsaufschlüsselung) wird eine Aufschlüsselung der Absichten anhand der bezeichneten Äußerungen oder der Endpunkttreffer angezeigt. Dieses zusammenfassende Diagramm zeigt die relative Bedeutung der einzelnen Absichten in der App. Wenn Sie auf ein Segment zeigen, werden der Name der Absicht und deren prozentualer Anteil an der Gesamtanzahl der bezeichneten Äußerungen oder der Endpunkttreffer angezeigt. 

![Absichtsaufschlüsselung](./media/luis-how-to-use-dashboard/intent-breakdown.png)

So steuern Sie, ob die Aufschlüsselung auf bezeichneten Äußerungen oder Endpunkttreffern basiert

1. Klicken Sie auf **Zusätzliche Einstellungen** ![Schaltfläche „Zusätzliche Einstellungen“](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png), um auf die Liste zuzugreifen, wie in der folgenden Abbildung gezeigt:

    ![Liste mit Absichtsaufschlüsselung](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Wählen Sie einen Wert in der Liste aus, und klicken Sie dann auf den Pfeil „Zurück“, ![Pfeil „Zurück“](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) um das Diagramm anzuzeigen.

## <a name="entity-breakdown"></a>Entitätsaufschlüsselung
Im Dashboard wird eine Aufschlüsselung der Entitäten anhand der bezeichneten Äußerungen oder der Endpunkttreffer angezeigt. Dieses zusammenfassende Diagramm zeigt die relative Bedeutung der einzelnen Entitäten in der App. Wenn Sie auf ein Segment zeigen, werden der Name der Entität und der prozentuale Anteil an den bezeichneten Äußerungen oder Endpunkttreffern angezeigt. 

![Entitätsaufschlüsselung](./media/luis-how-to-use-dashboard/entity-breakdown.png)

So steuern Sie, ob die Aufschlüsselung auf bezeichneten Äußerungen oder Endpunkttreffern basiert

1. Klicken Sie auf **Zusätzliche Einstellungen** ![Schaltfläche „Zusätzliche Einstellungen“](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png), um auf die Liste zuzugreifen, wie in der folgenden Abbildung gezeigt:

    ![Liste der Entitätsaufschlüsselung](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Wählen Sie einen Wert in der Liste aus, und klicken Sie dann auf den Pfeil „Zurück“, ![Pfeil „Zurück“](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) um das Diagramm entsprechend anzuzeigen.
