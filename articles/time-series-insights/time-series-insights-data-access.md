---
title: "Konfigurieren der Sicherheit für den Zugriff und die Verwaltung von Azure Time Series Insights | Microsoft-Dokumentation"
description: "In diesem Artikel wird erläutert, wie Sie die Sicherheit und Berechtigungen als Verwaltungszugriffsrichtlinien und Datenzugriffsrichtlinien konfigurieren, um Azure Time Series Insights zu schützen."
services: time-series-insights
ms.service: time-series-insights
author: op-ravi
ms.author: omravi
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 22c8e4481f2ba4163a55cc1bbb6b33c10379a605
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Gewähren von Datenzugriff für eine Time Series Insights-Umgebung über das Azure-Portal

Time Series Insights-Umgebungen verfügen über zwei unabhängige Typen von Zugriffsrichtlinien:

* Verwaltungszugriffsrichtlinien
* Datenzugriffsrichtlinien

Beide Richtlinien gewähren Azure Active Directory-Prinzipalen (Benutzern und Apps) diverse Berechtigungen für eine bestimmte Umgebung. Die Prinzipale (Benutzer und Apps) müssen dem Active Directory (bezeichnet als „Azure-Mandant“) angehören, das dem Abonnement mit der Umgebung zugeordnet ist.

Verwaltungszugriffsrichtlinien gewähren Berechtigungen im Zusammenhang mit der Konfiguration der Umgebung. Hierzu zählt beispielsweise Folgendes:
*   Erstellung und Löschung der Umgebung, Ereignisquellen und Verweisdatasets sowie
*   Verwaltung der Datenzugriffsrichtlinien.

Datenzugriffsrichtlinien gewähren Berechtigungen für die Erstellung von Datenabfragen, für die Bearbeitung von Verweisdaten in der Umgebung und für die Freigabe gespeicherter Abfragen und Perspektiven, die der Umgebung zugeordnet sind.

Die beiden Arten von Richtlinien ermöglichen eine klare Trennung zwischen dem Zugriff auf die Umgebungsverwaltung und dem Zugriff auf die Daten in der Umgebung. Dadurch können Sie eine Umgebung beispielsweise so einrichten, dass der Besitzer/Ersteller der Umgebung keinen Datenzugriff hat. Zudem können Sie Benutzern und Diensten, die über die Berechtigung zum Lesen von Daten aus der Umgebung verfügen, den Zugriff auf die Konfiguration der Umgebung verweigern.

## <a name="grant-data-access"></a>Gewähren des Datenzugriffs
Führen Sie die folgenden Schritte aus, um einem Benutzerprinzipal Datenzugriff zu gewähren:

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

2. Suchen Sie Ihre Time Series Insights-Umgebung. Geben Sie **Time Series** im **Suchfeld** ein. Wählen Sie **Time Series Environment** (Time Series-Umgebung) in den Suchergebnissen aus. 

3. Wählen Sie Ihre Time Series Insights-Umgebung in der Liste aus.
   
4. Wählen Sie **Datenzugriffsrichtlinien** und dann **+ Hinzufügen** aus.
  ![Verwalten der Time Series Insights-Quelle – Umgebung](media/data-access/getstarted-grant-data-access1.png)

5. Wählen Sie **Benutzer auswählen** aus.  Suchen Sie den Benutzernamen oder die E-Mail-Adresse des Benutzers, den Sie hinzufügen möchten. Klicken Sie auf **Auswählen**, um die Auswahl zu bestätigen. 

   ![Verwalten der Time Series Insights-Quelle – Hinzufügen](media/data-access/getstarted-grant-data-access2.png)

6. Wählen Sie **Rolle auswählen** aus. Wählen Sie die geeignete Zugriffsrolle für den Benutzer aus:
   - Wählen Sie **Mitwirkender** aus, wenn Sie dem Benutzer das Ändern von Verweisdaten und das Freigeben gespeicherter Abfragen und Perspektiven für andere Benutzer der Umgebung ermöglichen möchten. 
   - Wählen Sie andernfalls **Leser** aus, um dem Benutzer das Abfragen von Daten und das Speichern persönlicher (nicht freigegebener) Abfragen in der Umgebung zu ermöglichen.

   Wählen Sie **OK** aus, um die Rollenauswahl zu bestätigen.

   ![Verwalten der Time Series Insights-Quelle – Benutzer auswählen](media/data-access/getstarted-grant-data-access3.png)

8. Wählen Sie auf der Seite **Benutzerrolle auswählen** die Option **OK** aus.

   ![Verwalten der Time Series Insights-Quelle – Rolle auswählen](media/data-access/getstarted-grant-data-access4.png)

9. Auf der Seite **Datenzugriffsrichtlinien** sind die Benutzer und die Rollen der einzelnen Benutzer aufgeführt.

   ![Verwalten der Time Series Insights-Quelle – Ergebnisse](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen einer Event Hub-Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung](time-series-insights-how-to-add-an-event-source-eventhub.md)
* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
* Anzeigen Ihrer Umgebung im [Time Series Insights-Explorer](https://insights.timeseries.azure.com)
