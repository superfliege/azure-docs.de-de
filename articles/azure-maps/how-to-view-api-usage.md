---
title: 'Gewusst wie: Anzeigen der Nutzung der Azure Maps-API | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie die Metriken für Ihr Azure Maps-API-Aufrufe im Portal anzeigen.
author: dsk-2015
ms.author: dkshir
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: e62d2ff1fdd6bc94244511a2de95c4268a58d6f9
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2018
ms.locfileid: "42141065"
---
# <a name="how-to-view-the-azure-maps-api-usage"></a>Anzeigen der Nutzung der Azure Maps-API
In diesem Artikel erfahren Sie, wie Sie die API-Nutzungsmetriken für Ihr Azure Maps-Konto im [Portal](https://portal.azure.com) anzeigen. Die Metriken werden in einem benutzerfreundlichen Diagrammformat für einen anpassbaren Zeitraum angezeigt. 

## <a name="view-metric-snapshot"></a>Anzeigen der Metrikenmomentaufnahme 

Sie sehen einige gängige Metriken auf der **Übersicht**-Seite Ihres Maps-Kontos. Derzeit werden *Anforderungen gesamt*, *Fehler gesamt*  und *Verfügbarkeit* für einen auswählbaren Zeitraum angezeigt. 

![Übersicht zu Metriken von Azure Maps](media/how-to-view-api-usage/portal-overview.png)

Fahren Sie mit dem nächsten Abschnitt fort, wenn Sie diese Diagramme für Ihre spezielle Analyse anpassen müssen.


## <a name="view-detailed-metrics"></a>Anzeigen detaillierter Metriken

1. Melden Sie sich bei Ihrem Azure-Abonnement im [Portal](https://portal.azure.com) an. 

2. Klicken Sie auf der linken Seite auf die Menüoption **Alle Ressourcen**, und navigieren Sie zu Ihrem *Azure Maps-Konto*.

3. Sobald Ihr Maps-Konto geöffnet ist, klicken Sie auf der linken Seite auf das Menü **Metriken**.

4. Wählen Sie im Bereich **Metriken** eine der folgenden Optionen aus:

    1. **Verfügbarkeit**, womit der *Durchschnitt* der API-Verfügbarkeit über einen bestimmten Zeitraum angezeigt wird, oder 
    2. **Nutzung**, die Ihnen die *Anzahl* der Nutzungen für Ihr Konto anzeigt. 

    ![Metrikenbereich von Azure Maps](media/how-to-view-api-usage/portal-metrics.png)

5. Nachdem Sie die Metriken ausgewählt haben, können Sie den *Zeitbereich* auswählen; der Standardwert lautet **Letzte 12 Stunden (automatisch)**. Sie können auch die *Zeitgranularität* auswählen und in der gleichen Dropdownliste wählen, ob Sie die *lokale* oder *GMT*-Uhrzeit anzeigen möchten. Klicken Sie auf **Anwenden**.

    ![Metrikenzeitbereich von Azure Maps](media/how-to-view-api-usage/time-range.png)
 
6. Sobald Sie Ihre Metrik hinzufügen, können Sie aus den für die Metrik relevanten Eigenschaften einen **Filter hinzufügen** und dann den Wert der Eigenschaft auswählen, für den Sie das Diagramm anzeigen möchten. 

    ![Metrikenfilter von Azure Maps](media/how-to-view-api-usage/filter.png)

7. Sie können auch basierend auf Ihrer ausgewählten Metrikeigenschaft die **Aufteilung für Ihre Metrik anwenden**. Dadurch wird das Diagramm in mehrere Diagramme aufgeteilt, jeweils eins für jeden Wert dieser Eigenschaft. Beispielsweise entspricht in der folgenden Abbildung die Farbe jedes Diagramms dem am unteren Rand des Diagramms angezeigten Eigenschaftswert.

    ![Aufteilung der Metriken von Azure Maps](media/how-to-view-api-usage/splitting.png)
 
8. Um mehrere Metriken im gleichen Diagramm zu beobachten, klicken Sie einfach oben auf die Schaltfläche **Metrik hinzufügen**.


## <a name="next-steps"></a>Nächste Schritte

Da Sie nun gelernt haben, Ihre Nutzung von Azure Maps nachzuverfolgen, können Sie weitere Informationen zu den verwendeten APIs lesen:

* [Dokumentation zur REST API von Azure Maps](https://docs.microsoft.com/rest/api/maps)
