---
title: Reagieren auf Ereignisse mit Azure Log Analytics-Warnungen | Microsoft-Dokumentation
description: In diesem Tutorial lernen Sie, Warnungen in Log Analytics zu verstehen, die wichtige Informationen in Ihrem Arbeitsbereich identifizieren und Sie proaktiv über Probleme informieren oder Aktionen aufrufen, um die Probleme zu beheben.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: d81a41a0012d4e0be4e812d48074e7af1e92213a
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391145"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Reagieren auf Ereignisse mit Azure Monitor-Warnungen
Regeln für die Protokollsuche werden von Azure-Warnungen erstellt, um in regelmäßigen Abständen automatisch angegebene Protokollabfragen auszuführen.  Wenn die Ergebnisse der Protokollabfrage bestimmte Kriterien erfüllen, wird ein Warnungsdatensatz erstellt. Die Regel kann mithilfe von [Aktionsgruppen](../monitoring-and-diagnostics/monitoring-action-groups.md) dann automatisch eine oder mehrere Aktionen durchführen.  Dieses Tutorial ist eine Fortsetzung des Tutorials [Erstellen und Freigeben von Dashboards von Log Analytics-Daten](log-analytics-tutorial-dashboards.md).   

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Warnungsregel
> * Konfigurieren einer Aktionsgruppe zum Senden einer E-Mail-Benachrichtigung

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein, der [mit dem Log Analytics-Arbeitsbereich verbunden](log-analytics-quick-collect-azurevm.md) ist.

## <a name="sign-in-to-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-alerts"></a>Erstellen von Warnungen
Warnungen werden von Warnungsregeln in Azure Monitor erstellt und können in regelmäßigen Abständen automatisch gespeicherte Abfragen oder benutzerdefinierte Protokollsuchen ausführen.  Sie können Warnungen auf der Grundlage bestimmter Leistungsmetriken oder des Entstehens bestimmter Ereignisse, der Abwesenheit eines Ereignisses oder des Entstehens einer Anzahl von Ereignissen innerhalb eines bestimmten Zeitfensters erstellen.  Warnungen können Sie beispielsweise benachrichtigen, wenn die durchschnittliche CPU-Auslastung einen bestimmten Schwellenwert überschreitet, ein fehlendes Update ermittelt wird oder ein Ereignis generiert wird, wenn ein bestimmter Windows-Dienst oder Linux-Daemon nicht ausgeführt wird.  Wenn die Ergebnisse der Protokollsuche bestimmte Kriterien erfüllen, wird eine Warnung erstellt. Die Regel kann dann automatisch eine oder mehrere Aktionen ausführen, um Sie über die Warnung zu informieren oder einen anderen Prozess aufzurufen.

Im folgenden Beispiel erstellen Sie eine Warnungsregel vom Typ „Metrische Maßeinheit“, die auf der Abfrage *Azure-VMs – Prozessorauslastung* basiert, die unter [Erstellen und Freigeben von Dashboards von Log Analytics-Daten](log-analytics-tutorial-dashboards.md) gespeichert wurde. Eine Warnung wird für jeden virtuellen Computer erstellt, der einen Schwellenwert von 90 Prozent überschreitet.

1. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Monitor** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Monitor** aus.
2. Klicken Sie im linken Bereich auf **Warnungen** und dann oben auf der Seite auf **Neue Warnungsregel**, um eine neue Warnung zu erstellen.

    ![Erstellen einer neuen Warnungsregel](./media/log-analytics-tutorial-response/alert-rule-02.png)

3. Im ersten Schritt wählen Sie im Abschnitt **Warnung erstellen** Ihren Log Analytics-Arbeitsbereich als Ressource aus, da es sich dabei um ein protokollbasiertes Warnungssignal handelt.  Wenn Sie mehrere Abonnements besitzen, filtern Sie die Ergebnisse, indem Sie in der Dropdownliste das gewünschte **Abonnement** auswählen, das den zuvor erstellten virtuellen Computer und Log Analytics-Arbeitsbereich enthält.  Filtern Sie den **Ressourcentyp**, indem Sie in der Dropdownliste **Log Analytics** auswählen.  Wählen Sie abschließend die **Ressource** mit dem Namen **DefaultLAWorkspace**, und klicken Sie dann auf **Fertig**.

    ![Schritt 1: Erstellen einer Warnung](./media/log-analytics-tutorial-response/alert-rule-03.png)

4. Klicken Sie im Abschnitt **Warnungskriterien** auf **Kriterien hinzufügen**, um die Abfrage zu definieren, und geben Sie dann eine Logik für die Warnungsregel ein. Wählen Sie im Bereich **Signallogik konfigurieren** die Option **Benutzerdefinierte Protokollsuche** als Signalname aus, und geben Sie die Abfrage im Feld **Suchabfrage** ein.

    Beispiel: 
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    Der Bereich wird aktualisiert und zeigt die Konfigurationseinstellungen für die Warnung an.  Am oberen Rand werden die Ergebnisse für die letzten 30 Minuten des ausgewählten Signals angezeigt.

5. Konfigurieren Sie die Warnung mit den folgenden Informationen:  
   a. Wählen Sie in der Dropdownliste **Basierend auf* die Option **Metrische Maßeinheit** aus.  Mit „Metrische Maßeinheit“ wird eine Warnung für jedes Objekt in der Abfrage mit einem Wert erzeugt, der den angegebenen Schwellenwert überschreitet.  
   b. Wählen Sie für **Bedingung** die Option **Größer als** aus, und geben Sie für **Schwellenwert** den Wert **90** ein.  
   c. Wählen Sie im Abschnitt „Warnung auslösen basierend auf“ die Option **Aufeinanderfolgende Sicherheitsverletzungen** und in der Dropdownliste **Größer als** aus, und geben Sie den Wert „3“ ein.  
   d. Übernehmen Sie im Abschnitt „Evaluation based on“ (Auswertung basierend auf) die Standardwerte. Die Regel wird alle fünf Minuten ausgeführt und gibt Datensätze zurück, die innerhalb dieses aktuellen Zeitbereichs erstellt wurden.  
6. Klicken Sie auf **Fertig**, um die Warnungsregel fertig zu stellen.

    ![Konfigurieren eines Warnungssignals](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

7. Geben Sie nun im zweiten Schritt im Feld **Name der Warnungsregel** einen Namen für Ihre Warnung ein, etwa **CPU in Prozent größer als 90 Prozent**.  Geben Sie eine **Beschreibung** mit Details zu Ihrer Warnung ein, und wählen Sie für **Schweregrad** aus den angegebenen Optionen **Kritisch (Schweregrad 0)** aus.

    ![Konfigurieren der Warnungsdetails](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

8. Um die Warnungsregel direkt bei der Erstellung zu aktivieren, übernehmen Sie den Standardwert für **Regel beim Erstellen aktivieren**.  
9. Im dritten und letzten Schritt geben Sie eine **Aktionsgruppe** an. Dadurch wird sichergestellt, dass bei jeder Warnungsauslösung die gleichen Aktionen ausgeführt werden und diese für jede definierte Regel verwendet werden können.  Konfigurieren Sie eine neue Aktionsgruppe mit den folgenden Informationen:  
   a. Klicken Sie auf **Neue Aktionsgruppe**, und der Bereich **Aktionsgruppe hinzufügen** wird angezeigt.  
   b. Geben Sie unter **Name der Aktionsgruppe** einen Namen wie **IT-Vorgänge – Benachrichtigen** und einen **Kurznamen** wie **itops-n** ein.  
   c. Überprüfen Sie, ob die Standardwerte für **Abonnement** und **Ressourcengruppe** richtig sind. Ist dies nicht der Fall, wählen Sie die korrekten Werte in der Dropdownliste aus.  
   d. Geben Sie im Bereich „Aktionen“ einen Namen für die Aktion ein, beispielsweise **E-Mail senden**, und wählen Sie unter **Aktionstyp** in der Dropdownliste **E-Mail/SMS/Push/Sprachanruf** aus. Der Eigenschaftenbereich **E-Mail/SMS/Push/Sprachanruf** wird mit weiteren Informationen auf der rechten Seite geöffnet.  
   e. Aktivieren Sie im Bereich **E-Mail/SMS/Push/Sprachanruf** die Option **E-Mail**, und geben Sie eine gültige SMTP-E-Mail-Adresse ein, an die die Nachricht gesendet werden soll.  
   f. Klicken Sie zum Speichern der Änderungen auf **OK** .  
       ![Erstellen einer neuen Aktionsgruppe](./media/log-analytics-tutorial-response/action-group-properties-01.png)

10. Klicken Sie auf **OK**, um die Aktionsgruppe zu erstellen.
11. Klicken Sie auf **Warnungsregel erstellen**, um die Warnungsregel fertig zu stellen. Die Ausführung beginnt sofort.

    ![Abschließen der Erstellung der neuen Warnungsregel](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>Anzeigen von Warnungen im Azure-Portal
Sie haben eine Warnung erstellt und können nun Azure-Warnungen in einem einzelnen Bereich anzeigen sowie alle Warnungsregeln in allen Ihren Azure-Abonnements verwalten. Sie listet alle (aktivierten oder deaktivierten) Warnungsregeln auf und kann basierend auf Zielressourcen, Ressourcengruppen, Regelnamen oder Status sortiert werden. Der Bereich enthält eine aggregierte Zusammenfassung von allen ausgelösten Warnungen und konfigurierten bzw. aktivierten Warnungsregeln.

![Statusseite der Azure-Warnungen](./media/log-analytics-tutorial-response/azure-alerts-02.png)

Bei Auslösung der Warnung gibt die Tabelle die Bedingung sowie die Anzahl von Vorkommnissen im auswählten Zeitbereich (Standardwert: letzte sechs Stunden) an.  Ihr Posteingang sollte eine entsprechende E-Mail mit dem betroffenen virtuellen Computer und den wichtigsten Ergebnissen enthalten, die der Suchabfrage in diesem Fall entsprachen. Die E-Mail sieht in etwa wie folgt aus:

![Beispiel für eine Warnungs-E-Mail-Aktion](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Warnungsregeln ein Problem proaktiv erkennen und darauf reagieren können, wenn sie in geplanten Intervallen Protokollsuchen ausführen und bestimmten Kriterien entsprechen.

Unter diesem Link finden Sie vordefinierte Log Analytics-Skriptbeispiele.

> [!div class="nextstepaction"]
> [Log Analytics-Skriptbeispiele](powershell-samples.md)
