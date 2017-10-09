---
title: Senden von Warnungen aus Application Insights nach SQL | Microsoft-Dokumentation
description: Tutorial zum Senden von Warnungen als Reaktion auf Fehler in der Anwendung mithilfe von Azure Application Insights
keywords: 
author: bwren
ms.author: bwren
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 40b3cd74fc68158b679db462278edef4827d8603
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>Überwachen der Anwendungsintegrität und Senden entsprechender Warnungen mit Azure Application Insights

Mit Azure Application Insights können Sie Ihre Anwendung überwachen und Warnungen erhalten, wenn sie nicht verfügbar ist sowie wenn Fehler oder Leistungsprobleme auftreten.  Dieses Tutorial führt Sie durch den Prozess zum Erstellen von Tests, um die Verfügbarkeit einer Anwendung kontinuierlich zu überprüfen und verschiedene Arten von Warnungen als Reaktion auf erkannte Probleme zu senden.  Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Verfügbarkeitstests zur kontinuierlichen Überprüfung der Anwendungsreaktion
> * Senden von E-Mails an Administratoren, wenn ein Problem auftritt
> * Erstellen von Warnungen auf Grundlage von Leistungsmetriken 
> * Verwenden einer Logik-App zum Senden einer Telemetriedatenzusammenfassung nach einem Zeitplan


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Installieren Sie [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit den folgenden Workloads:
    - ASP.NET und Webentwicklung
    - Azure-Entwicklung
    - Stellen Sie eine .NET-Anwendung in Azure bereit, und [aktivieren Sie das Application Insights SDK](app-insights-asp-net.md). 


## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) im Azure-Portal an.

## <a name="create-availability-test"></a>Erstellen eines Verfügbarkeitstests
Verfügbarkeitstests in Application Insights ermöglichen Ihnen das automatische Testen Ihrer Anwendung an verschiedenen Standorten auf der ganzen Welt.   In diesem Tutorial führen Sie einen einfachen Test durch, um sicherzustellen, dass die Anwendung verfügbar ist.  Sie könnten auch eine vollständige exemplarische Vorgehensweise erstellen, um ihren Betrieb im Detail zu testen. 

1. Wählen Sie zuerst **Application Insights** und anschließend Ihr Abonnement aus.  
1. Wählen Sie im Menü **Untersuchen** die Option **Verfügbarkeit** aus, und klicken Sie dann auf **Test hinzufügen**.
 
    ![Verfügbarkeitstest hinzufügen](media/app-insights-tutorial-alert/add-test.png)

2. Geben Sie einen Namen für den Test ein, und behalten Sie die anderen Standardwerte bei.  Damit wird die Startseite der Anwendung alle 5 Minuten von 5 unterschiedlichen geografischen Standorten aus angefordert. 
3. Wählen Sie **Warnungen**, um den Bereich **Warnungen** zu öffnen, in dem Sie detailliert festlegen können, wie bei einem Fehlschlagen des Tests reagiert wird. Geben Sie eine E-Mail-Adresse ein, an die eine Nachricht gesendet wird, wenn die Warnungskriterien erfüllt sind.  Geben Sie optional die Adresse eines Webhooks ein, der aufgerufen wird, wenn die Warnungskriterien erfüllt sind.

    ![Test erstellen](media/app-insights-tutorial-alert/create-test.png)
 
4. Kehren Sie zum Testbereich zurück. Nach wenigen Minuten sollten dort Ergebnisse des Verfügbarkeitstests angezeigt werden.  Klicken Sie auf einen Testnamen, um Details zu den einzelnen Standorten anzuzeigen.  Das Punktdiagramm zeigt Erfolg und Dauer der einzelnen Tests an.

    ![Testdetails](media/app-insights-tutorial-alert/test-details.png)

5.  Sie können einen Drilldown zu den Details eines bestimmten Test ausführen, indem Sie im Punktdiagramm auf den zugehörigen Punkt klicken.  Das folgende Beispiel zeigt die Details für eine fehlgeschlagene Anforderung.

    ![Testergebnis](media/app-insights-tutorial-alert/test-result.png)
  
6. Wenn die Warnungskriterien erfüllt sind, wird eine E-Mail wie im folgenden Beispiel an die von Ihnen angegebene Adresse gesendet.

    ![E-Mail-Warnung](media/app-insights-tutorial-alert/alert-mail.png)


## <a name="create-an-alert-from-metrics"></a>Warnung auf Grundlage von Metriken erstellen
Zusätzlich zum Senden von Warnungen durch einen Verfügbarkeitstest können Sie eine Warnung auf Grundlage aller Leistungsmetriken erstellen, die für Ihre Anwendung erfasst werden.

2. Wählen Sie im Menü **Konfigurieren** die Option **Warnungen** aus.  Daraufhin wird der Bereich „Azure-Warnungen“ geöffnet.  Möglicherweise sind hier weitere Warnungsregeln für andere Dienste konfiguriert.
3. Klicken Sie auf **Metrikwarnung hinzufügen**.  Daraufhin wird der Bereich zum Erstellen einer neuen Warnungsregel geöffnet.

    ![Metrikwarnung hinzufügen](media/app-insights-tutorial-alert/add-metric-alert.png)

4. Geben Sie einen **Namen** für die Warnungsregel ein, und wählen Sie in der Dropdownliste Ihre Anwendung als **Ressource** aus.
5. Wählen Sie eine **Metrik** zum Abrufen aus.  Ein Diagramm wird angezeigt, das den Wert dieser Anforderung während der letzten 24 Stunden angibt.  Dies hilft Ihnen beim Festlegen der Bedingung für die Metrik.

    ![Hinzufügen einer Warnungsregel](media/app-insights-tutorial-alert/add-alert-01.png)

6. Geben Sie eine **Bedingung** und einen **Schwellenwert** für die Warnung ein. Dies ist die Anzahl der Fälle, in denen die Metrik überschritten werden muss, damit eine Warnung erstellt wird. 
6. Aktivieren Sie unter **Benachrichtigen über** das Kontrollkästchen **E-Mail-Besitzer, Mitwirkende und Leser**, damit an diese Benutzer eine E-Mail gesendet wird, wenn die Warnungsbedingung erfüllt ist, und fügen Sie ggf. die E-Mail-Adressen weiterer Empfänger hinzu.  Hier können Sie auch einen Webhook oder eine Logik-App angeben, die ausgeführt werden, wenn die Bedingung erfüllt ist.  Mit diesen kann versucht werden, die Auswirkungen des erkannten Problems zu minimieren oder 

    ![Hinzufügen einer Warnungsregel](media/app-insights-tutorial-alert/add-alert-02.png)


## <a name="proactively-send-information"></a>Proaktiv Informationen senden
Warnungen werden als Reaktion auf eine bestimmte Gruppe von Problemen erstellt, die in der Anwendung erkannt wurden, und in der sind Warnungen kritischen Bedingungen vorbehalten, die sofortige Aufmerksamkeit erfordern.  Mit einer Logik-App, die automatisch nach einem Zeitplan ausgeführt wird, können Sie proaktiv Informationen über Ihre Anwendung erhalten.  Beispielsweise kann täglich eine E-Mail mit zusammengefassten Informationen, die einer weiteren Auswertung bedürfen, an Administratoren gesendet werden.

Ausführliche Informationen zum Erstellen einer Logik-App mit Application Insights finden Sie unter [Automatisieren von Application Insights-Prozessen mithilfe von Logic Apps](automate-with-logic-apps.md)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit dem Erstellen von Warnungen bei Problemen vertraut gemacht haben, erfahren Sie im nächsten Tutorial, wie die Interaktion von Benutzern mit Ihrer Anwendung analysiert wird.

> [!div class="nextstepaction"]
> [Grundlegendes zu Benutzern](app-insights-tutorial-users.md)
