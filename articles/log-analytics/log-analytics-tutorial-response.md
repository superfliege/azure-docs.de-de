---
title: Reagieren auf Ereignisse mit Azure Log Analytics-Warnungen | Microsoft-Dokumentation
description: "In diesem Tutorial lernen Sie, Warnungen in Log Analytics zu verstehen, die wichtige Informationen in Ihrem OMS-Repository identifizieren und Sie proaktiv über Probleme informieren oder Aktionen aufrufen, um zu versuchen, die Probleme zu beheben."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ccf0bd57b7f54b1d8d57b460df9bb381b29ee381
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Reagieren auf Ereignisse mit Log Analytics-Warnungen
Mit Warnungen werden in Log Analytics wichtige Informationen in Ihrem Log Analytics-Repository identifiziert.  Sie werden durch Warnungsregeln erstellt, die automatisch in regelmäßigen Abständen Protokollsuchen ausführen, und wenn Ergebnisse der Protokollsuche mit bestimmten Kriterien übereinstimmen, wird ein Warnungsdatensatz erstellt, und er kann zur Ausführung einer automatischen Antwort konfiguriert werden.  Dieses Tutorial ist eine Fortsetzung des Tutorials [Erstellen und Freigeben von Dashboards von Log Analytics-Daten](log-analytics-tutorial-dashboards.md).   

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Warnungsregel
> * Konfigurieren einer Warnungsregel zum Senden einer E-Mail-Benachrichtigung

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein, der [mit dem Log Analytics-Arbeitsbereich verbunden](log-analytics-quick-collect-azurevm.md) ist.  

## <a name="log-in-to-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) im Azure-Portal an. 

## <a name="create-alerts"></a>Erstellen von Warnungen

Warnungen werden mithilfe von Warnungsregeln erstellt, für die in regelmäßigen Abständen automatisch Protokollsuchen durchgeführt werden.  Sie können Warnungen auf der Grundlage bestimmter Leistungsmetriken oder des Entstehens bestimmter Ereignisse, der Abwesenheit eines Ereignisses oder des Entstehens einer Anzahl von Ereignissen innerhalb eines bestimmten Zeitfensters erstellen.  Warnungen können Sie beispielsweise benachrichtigen, wenn die durchschnittliche CPU-Auslastung einen bestimmten Schwellenwert überschreitet, oder ein Ereignis generiert wird, wenn ein bestimmter Windows-Dienst oder Linux-Daemon nicht ausgeführt wird.   Wenn die Ergebnisse der Protokollsuche bestimmte Kriterien erfüllen, wird ein Warnungsdatensatz erstellt. Die Regel kann dann automatisch eine oder mehrere Aktionen ausführen, um Sie proaktiv über die Warnung zu informieren oder einen anderen Prozess aufzurufen. 

Im folgenden Beispiel erstellen Sie eine „Metrische Maßeinheit“-Warnungsregel, die eine Warnung für jedes Computerobjekt in der Abfrage erstellt, deren Wert einen Schwellenwert von 90% überschreitet.

1. Klicken Sie im Azure-Portal unten links auf **Weitere Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
2. Starten Sie das OMS-Portal durch Auswahl von „OMS-Portal“, und wählen Sie auf der Seite **Übersicht** die Option **Protokollsuche**.  
3. Wählen Sie **Favoriten** ganz oben im Portal und im Bereich **Gespeicherte Suchvorgänge** auf der rechten Seite die Abfrage *Azure-VMs – Prozessorauslastung*.  
4. Klicken Sie oben auf der Seite auf **Warnung**, um den Bildschirm **Warnungsregel hinzufügen** zu öffnen.  
5. Konfigurieren Sie die Warnungsregel mit den folgenden Informationen:  
   a. Geben Sie einen **Namen** für die Warnung an, wie z.B. *VM-Prozessorauslastung überschritten >90*.  
   b. Geben Sie für **Zeitfenster** einen Zeitraum für die Abfrage an, z.B. *30*.  Die Abfrage gibt nur Datensätze zurück, die innerhalb dieses aktuellen Zeitbereichs erstellt wurden.  
   c. **Warnungshäufigkeit** gibt an, wie oft die Abfrage ausgeführt werden soll.  Geben Sie in diesem Beispiel *5* Minuten an, was in unserem angegebenen Zeitfenster angezeigt werden wird.  
   d. Wählen Sie **Metrische Maßeinheit**, und geben Sie *90* für **Aggregatwert**  und *3* für **Warnung auslösen basierend auf**  an.  
   e. Deaktivieren Sie unter **Aktionen** die E-Mail-Benachrichtigung.
6. Klicken Sie auf **Speichern** , um die Warnungsregel abzuschließen. Die Ausführung beginnt sofort.<br><br> ![Warnungsregelbeispiel](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Für Warnungsdatensätze, die in Log Analytics mit Warnungsregeln erstellt wurden, ist **Warnung** als Typ und **OMS** als SourceSystem festgelegt.<br><br> ![Beispiel für generierte Warnungsereignisse](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Warnungsaktionen
Sie können erweiterte Aktionen mit Warnungen ausführen – z.B. Erstellen einer E-Mail-Benachrichtigung, Starten eines [Automation-Runbooks](../automation/automation-runbook-types.md), Verwenden eines Webhooks zum Erstellen eines Incident-Datensatzes in Ihrem ITSM-Incident-Verwaltungssystem oder mit der [IT Service Management Connector-Lösung](log-analytics-itsmc-overview.md) als Reaktion, wenn die Warnungskriterien erfüllt sind.   

Bei E-Mail-Aktionen wird eine E-Mail mit Details der Warnung an einen oder mehrere Empfänger gesendet. Sie können den Betreff der E-Mail angeben, aber der Inhalt ist ein von Log Analytics erstelltes Standardformat.  Aktualisieren Sie die Warnungsregel, die Sie zuvor erstellt haben, und konfigurieren Sie sie so, dass Sie eine E-Mail-Nachricht erhalten, anstatt aktiv mit einer Protokollsuche nach Warnungsdatensätzen suchen zu müssen.     

1. Wählen Sie im OMS-Portal auf der oberen Menüleiste **Einstellungen** und dann **Warnungen**.
2. Klicken Sie in der Liste der Warnungsregeln neben der Warnung, die Sie zuvor erstellt haben, auf das Bleistiftsymbol.
3. Aktivieren Sie im Abschnitt **Aktionen** E-Mail-Benachrichtigungen.
4. Geben Sie einen **Betreff** für die E-Mail an, z.B. *Schwellenwert der Prozessorauslastung überschritten > 90*.
5. Fügen Sie die Adressen eines oder mehrerer E-Mail-Empfänger in das Feld **Empfänger** ein.  Verwenden Sie als Trennzeichen ein Semikolon (;), wenn Sie mehrere Adressen angeben.
6. Klicken Sie auf **Speichern** , um die Warnungsregel abzuschließen. Die Ausführung beginnt sofort.<br><br> ![Warnungsregel mit E-Mail-Benachrichtigung](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Warnungsregeln ein Problem proaktiv erkennen und darauf reagieren können, wenn sie in geplanten Intervallen Protokollsuchen ausführen und bestimmten Kriterien entsprechen.  

Unter diesem Link finden Sie vordefinierte Log Analytics-Skriptbeispiele.  

> [!div class="nextstepaction"]
> [Log Analytics-Skriptbeispiele](powershell-samples.md)
