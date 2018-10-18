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
ms.openlocfilehash: c9bddf8e20524433b31793e277efd954a5d1320e
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423373"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Gewähren von Datenzugriff für eine Time Series Insights-Umgebung über das Azure-Portal

In diesem Artikel werden die beiden Arten von Zugriffsrichtlinien für Azure Time Series Insights thematisiert.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-creating-and-managing-access-policies-within-time-series-insights-br"></a>In diesem Video erfahren Sie, wie Sie Zugriffsrichtlinien für Time Series Insights erstellen und verwalten. </br>

> [!VIDEO https://www.youtube.com/embed/5zTIdyHMJW8]

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

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

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

## <a name="provide-guest-access-to-a-user-from-another-aad-tenant"></a>Bereitstellen eines Gastzugriffs über einen anderen AAD-Mandanten für einen Benutzer

Der Begriff „Gast“ bezeichnet keine Verwaltungsrolle, sondern ein Konto, das von einem Mandanten zu einem anderem Mandanten eingeladen wurde. Wenn das Gastkonto in das Verzeichnis des Mandanten eingeladen wird, kann diesem die gleiche Zugriffssteuerung zugewiesen werden wie jedem anderen Konto, um entweder einer TSI-Umgebung über das Blatt „Access Control (IAM)“ Verwaltungszugriff zu gewähren, oder um in der Umgebung über das Blatt „Datenzugriffsrichtlinien“ Zugriff auf die Daten zu gewähren. Weitere Informationen zum Gastzugriff für den AAD-Mandanten finden Sie in diesem [Dokument](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Führen Sie die folgenden Schritte aus, um einem AAD-Benutzer über einen anderen Mandanten Zugriff auf die Time Series Insights-Umgebung zu gewähren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie Ihre Time Series Insights-Umgebung. Geben Sie **Time Series** im **Suchfeld** ein. Wählen Sie **Time Series Environment** (Time Series-Umgebung) in den Suchergebnissen aus.

3. Wählen Sie Ihre Time Series Insights-Umgebung in der Liste aus.

4. Klicken Sie erst auf **Datenzugriffsrichtlinien** und dann auf +**Einladen**.

    ![Verwalten der Time Series Insights-Quelle: Benutzer einladen](media/data-access/getstarted-grant-data-access6.png)

5. Geben Sie die E-Mail-Adresse des Benutzers an, den Sie einladen möchten. Diese E-Mail sollte mit Azure AD verknüpft sein. Sie können der Einladung eine persönliche Nachricht hinzufügen.

    ![Verwalten der Time Series Insights-Quelle – Benutzer auswählen](media/data-access/getstarted-grant-data-access7.png)

6. Dann sollte auf dem Bildschirm eine Bestätigungsmeldung angezeigt werden.

    ![Verwalten der Time Series Insights-Quelle: Benutzer bestätigen](media/data-access/getstarted-grant-data-access8.png)

7. Wählen Sie **Benutzer auswählen** aus. Suchen Sie nach der E-Mail-Adresse des Gastbenutzers, den Sie zuvor eingeladen haben, um diesen hinzuzufügen. Klicken Sie auf **Auswählen**, um die Auswahl zu bestätigen.
  
    ![Verwalten der Time Series Insights-Quelle: Benutzer bestätigen](media/data-access/getstarted-grant-data-access9.png)

8. Wählen Sie **Rolle auswählen** aus. Wählen Sie eine geeignete Zugriffsrolle für den Gastbenutzer aus:

    * Wählen Sie **Mitwirkender** aus, wenn Sie dem Benutzer das Ändern von Verweisdaten und das Freigeben gespeicherter Abfragen und Perspektiven für andere Benutzer der Umgebung erlauben möchten.

    * Wählen Sie andernfalls **Leser** aus, um dem Benutzer das Abfragen von Daten und das Speichern persönlicher (nicht freigegebener) Abfragen in der Umgebung zu ermöglichen.

    Wählen Sie **OK** aus, um die Rollenauswahl zu bestätigen.

    ![Verwalten der Time Series Insights-Quelle – Rolle auswählen](media/data-access/getstarted-grant-data-access10.png)

9. Wählen Sie auf der Seite **Benutzerrolle auswählen** die Option **OK** aus.

10. Auf der Seite **Datenzugriffsrichtlinien** werden dann die Gastbenutzer mit denen ihnen zugewiesenen Rollen aufgeführt.

    ![Verwalten der Time Series Insights-Quelle: Rolle bestätigen](media/data-access/getstarted-grant-data-access11.png)

11. Anschließend muss der Gastbenutzer einige Schritte ausführen, um auf die Umgebung zuzugreifen, die sich in dem Azure-Mandanten befindet, zu dem Sie ihn zuvor eingeladen haben. Zunächst müssen sie die Einladung annehmen. Diese wird an die E-Mail-Adresse gesendet, die Sie in Schritt 5 angegeben haben. Der Benutzer muss auf „Get Started“ (Erste Schritte) klicken, um die Einladung anzunehmen.

    ![Verwalten der Time Series Insights-Quelle: Benutzer einladen](media/data-access/getstarted-grant-data-access12.png)

12. Anschließend muss er die Berechtigungen annehmen, die der Organisation des Administrators zugeordnet sind.

    ![Verwalten der Time Series Insights-Quelle: Berechtigungen annehmen](media/data-access/getstarted-grant-data-access13.png)

13. Wenn sich der Gastbenutzer bei dem E-Mail-Postfach anmeldet, an das Sie die Einladung gesendet haben und dieser die Einladung annimmt, wird er auf insights.azure.com weitergeleitet. Auf dieser Seite muss er auf den Avatar klicken, der neben der E-Mail-Adresse auf der rechten Seite angezeigt wird. 

    ![Verwalten der Time Series Insights-Quelle: Berechtigungen annehmen](media/data-access/getstarted-grant-data-access14.png)

14. Anschließend muss der Gastbenutzer Ihren Azure-Mandanten aus dem Dropdownmenü für das Verzeichnis auswählen. Dabei handelt es sich dann um den Mandanten, zu dem Sie ihn eingeladen haben. 

    ![Verwalten der Time Series Insights-Quelle: Berechtigungen annehmen](media/data-access/getstarted-grant-data-access15.png)

15. Wenn der Gastbenutzer schließlich Ihren Mandanten auswählt, wird diesem die Time Series Insights-Umgebung angezeigt, zu der Sie diesem zuvor Zugriff gewährt haben. Dann sollte er Zugriff auf alle Funktionen haben, die der Rolle zugeordnet sind, die Sie in Schritt 8 ausgewählt haben.

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen einer Event Hub-Ereignisquelle zu Ihrer Azure Time Series Insights-Umgebung](time-series-insights-how-to-add-an-event-source-eventhub.md)
* [Senden von Ereignissen](time-series-insights-send-events.md) an die Ereignisquelle
* Anzeigen Ihrer Umgebung im [Time Series Insights-Explorer](https://insights.timeseries.azure.com)
