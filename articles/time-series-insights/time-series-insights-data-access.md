---
title: Konfigurieren der Sicherheit für den Zugriff und die Verwaltung von Azure Time Series Insights | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie die Sicherheit und Berechtigungen als Verwaltungszugriffsrichtlinien und Datenzugriffsrichtlinien konfigurieren, um Azure Time Series Insights zu schützen.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 7cb5dc5b170103f98d56abc920f36dd85f855961
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364818"
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

[!INCLUDE [iot-tsi-data-access](../../includes/iot-tsi-data-access.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen einer Event Hub-Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung](time-series-insights-how-to-add-an-event-source-eventhub.md)
* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
* Anzeigen Ihrer Umgebung im [Time Series Insights-Explorer](https://insights.timeseries.azure.com)
