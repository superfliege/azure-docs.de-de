---
title: 'Azure Time Series (Vorschauversion): Verwaltung – Bereitstellen und verwalten der Vorschauversion von Azure Time Series. | Microsoft-Dokumentation'
description: Grundlagen zum Bereitstellen und Verwalten von Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 1834a59ff675f109267a406e3c2b03411e3bb50b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273499"
---
# <a name="how-to-provision-and-manage-azure-time-series-insights-preview"></a>Bereitstellen und Verwalten von Azure Time Series Insights Preview

In diesem Dokument wird beschrieben, wie Sie eine Azure Time Series Insights Preview-Umgebung mit dem [Azure-Portal](https://portal.azure.com/) erstellen.

## <a name="overview"></a>Übersicht

Im Folgenden erhalten Sie eine kurze Übersicht darüber, wie Sie eine Azure Time Series Insights Preview-Umgebung bereitstellen:

* Azure Time Series Insights Preview-Umgebungen sind **PAYG**-Umgebungen.
  * Im Rahmen des Erstellungsprozesses müssen Sie eine Time Series-ID angeben. Time Series-IDs können aus bis zu drei Schlüsseln bestehen. Weitere Informationen zur Time Series-ID finden Sie unter [Auswählen einer Time Series-ID](./time-series-insights-update-how-to-id.md).
  * Wenn Sie eine Azure Time Series Insights Preview-Umgebung bereitstellen, erstellen Sie zwei Azure-Ressourcen:

    * Eine Azure Time Series Insights Preview-Umgebung  
    * Ein Azure-Speicherkonto vom Typ „Allgemein v1“
  
    Erfahren Sie, wie Sie [Ihre Umgebung planen](./time-series-insights-update-plan.md).

    >[!IMPORTANT]
    > Kunden, die V2-Konten verwenden, aktivieren keine Cold/Archiv-Eigenschaften für das Speicherkonto, das Sie verwenden möchten.

* Jede Azure Time Series Insights Preview-Umgebung kann (optional) mit einer Ereignisquelle verknüpft werden. Weitere Informationen finden Sie unter [Hinzufügen einer Event Hub-Quelle](./time-series-insights-how-to-add-an-event-source-eventhub.md) und [Hinzufügen einer IoT Hub-Quelle](./time-series-insights-how-to-add-an-event-source-iothub.md).
  * Sie geben eine „Timestamp ID“-Eigenschaft sowie eine eindeutige Consumergruppe während dieses Schritts an. Auf diese Weise wird sichergestellt, dass die Umgebung Zugriff auf die entsprechenden Ereignisse hat.

* Nachdem die Bereitstellung abgeschlossen ist, können Sie Ihre Zugriffsrichtlinien und anderen Umgebungsattribute gemäß Ihren geschäftlichen Anforderungen ändern.

## <a name="new-environment-creation"></a>Erstellung einer neuen Umgebung

Die folgenden Schritte beschreiben, wie Sie eine Azure Time Series Insights Preview-Umgebung erstellen:

1. Wählen Sie die Schaltfläche **PAYG** im Menü **SKU** aus. Geben Sie einen Umgebungsnamen an, bestimmen Sie, welche Abonnementgruppe und welche Ressourcengruppe verwendet werden soll. Wählen Sie dann einen unterstützten Ort für die Umgebung aus, wo sie gehostet werden soll.

1. Eingabe einer Time Series-ID

    >[!NOTE]
    > * Die Time Series-ID beachtet Groß-/Kleinschreibung und ist unveränderlich (sie kann nach ihrer Festlegung nicht mehr geändert werden).
    > * Time Series-IDs können aus bis zu 3 Schlüsseln bestehen.
    > * Weitere Informationen zum Auswählen einer Time Series-ID finden Sie unter [Auswählen einer Time Series-ID](./time-series-insights-update-how-to-id.md).

1. Erstellen Sie ein Azure-Speicherkonto, indem Sie einen Azure-Speicherkontonamen auswählen und eine Replikationsauswahl treffe. Hierdurch wird automatisch ein Azure-Speicherkonto vom Typ „Allgemein v1“ erstellt. Es wird in derselben Region wie die Azure Time Series Insights Preview-Umgebung erstellt, die Sie zuvor ausgewählt haben.

    ![Erstellen Sie eine Azure Time Series Insights-Instanz.][1]

1. Optional können Sie eine Ereignisquelle hinzufügen.

   * Time Series Insights unterstützt [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) und [Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) als Optionen. Während Sie bei der Erstellung der Umgebung nur eine einzelne Ereignisquelle hinzufügen können, können Sie später eine zusätzliche Ereignisquelle hinzufügen. Es wird empfohlen, eine eindeutige Consumergruppe zu erstellen, um sicherzustellen, dass alle Ereignisse für Ihre Azure Time Series Insights Preview-Instanz sichtbar sind. Sie können eine vorhandene Consumergruppe auswählen oder eine neue Consumergruppe erstellen, wenn Sie die Ereignisquelle hinzufügen.

   * Sie sollten auch die entsprechende „Timestamp“-Eigenschaft festlegen. Standardmäßig verwendet Azure Time Series Insights die in der Warteschlange verbrachte Zeit einer Nachricht für jede Ereignisquelle.

     > [!TIP]
     > Die in der Warteschlange verbrachte Zeit einer Nachricht ist möglicherweise nicht die beste konfigurierte Einstellung, um sie in Uploadszenarios mit Batch-Ereignissen oder Verlaufsdaten zu verwenden. Stellen Sie sicher, dass Sie Ihre Entscheidung überprüfen, ob in solchen Fällen eine „Timestamp“-Eigenschaft verwendet werden soll oder nicht.

    ![Fügen Sie Ihrer Instanz eine Ereignisquelle hinzu.][2]

1. Überprüfen und Erstellen

    ![Fügen Sie Ihrer Instanz eine Ereignisquelle hinzu.][3]

Vergewissern Sie sich, dass Ihre Umgebung mit den gewünschten Einstellungen bereitgestellt wurde.

## <a name="management"></a>Verwaltung

Sie haben die Möglichkeit, Ihre Azure Time Series Insights Preview-Umgebung über das Azure-Portal zu verwalten. Im Folgenden finden Sie die wichtigsten Unterschiede bei der Verwaltung einer **PAYG** Azure Time Series Insights Preview-Umgebung im Gegensatz zu einer S1- oder S2-Umgebung mithilfe des Azure-Portals:

* Das Blatt *Übersicht* im Azure-Portal bleibt unverändert in Azure Time Series Insights, mit Ausnahme der folgenden Unterschiede:
  * „Kapazität“ wird entfernt, weil dieses Konzept für **PAYG**-Umgebungen nicht relevant ist.
  * Die Eigenschaft „Time Series-ID“ wurde hinzugefügt. Sie bestimmt, wie Ihre Daten partitioniert werden.
  * Verweisdatasets wurden entfernt.
  * Die angezeigte URL leitet Sie zum [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) weiter.
  * Der Name Ihres Azure-Speicherkontos wird aufgeführt.

* Das Blatt *Konfigurieren* im Azure-Portal wurde in Azure Time Series Insights Preview entfernt, weil **PAYG**-Umgebungen nicht konfigurierbar sind.

* Das Datenblatt *Verweis* im Azure-Portal wurde in Azure Time Series Insights Preview entfernt, weil Verweisdaten keine Komponente von **PAYG**-Umgebungen sind.

![Überprüfen Sie Ihre Instanz.][4]

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Planen Ihrer Umgebung](./time-series-insights-update-plan.md).

Lesen Sie [Hinzufügen einer Event Hub-Quelle](./time-series-insights-how-to-add-an-event-source-eventhub.md).

Lesen Sie [Hinzufügen einer IoT Hub-Quelle](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.png
[2]: media/v2-update-manage/manage_two.png
[3]: media/v2-update-manage/manage_three.png
[4]: media/v2-update-manage/manage_four.png
