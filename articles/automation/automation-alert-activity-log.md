---
title: Überwachen von Azure Automation-Runbooks mit Aktivitätsprotokollwarnungen
description: In diesem Artikel erfahren Sie, wie Sie Azure Automation-Runbooks mit dem Aktivitätsprotokoll überwachen.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: a0bd291af98477308cda898580fd52e33b1e6bbd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34356950"
---
# <a name="monitoring-runbooks-with-azure-activity-log-alerts"></a>Überwachen von Runbooks mit metrischen Alerts

In diesem Artikel erfahren Sie, wie Sie Warnungen basierend auf den Abschlussstatus von Runbooks erstellen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Anmelden bei Azure unter https://portal.azure.com

## <a name="create-alert"></a>Erstellen einer Warnung

Mit Warnungen können Sie eine zu überwachende Bedingung und eine auszuführende Aktion definieren, die ausgeführt wird, sobald diese Bedingung erfüllt ist.

Navigieren Sie im Azure-Portal zu **Alle Dienste**, und wählen Sie **Überwachen** aus. Wählen Sie auf der Seite „Überwachen“ die Option **Warnungen**, und klicken Sie auf **+ Neue Warnungsregel**.

### <a name="define-the-alert-condition"></a>Definieren der Warnungsbedingung

1. Klicken Sie unter **1. Warnungsbedingung definieren** auf **+ Ziel auswählen**. Wählen Sie Ihr Abonnement aus, und wählen Sie unter **Nach Ressourcentyp filtern** die Option **Automation-Konten** aus. Wählen Sie Ihr Automation-Konto, und klicken Sie auf **Fertig**.

   ![Auswählen einer Ressource für die Warnung](./media/automation-alert-activity-log/select-resource.png)

### <a name="configure-alert-criteria"></a>Konfigurieren der Warnungskriterien

1. Klicken Sie auf **+ Kriterien hinzufügen**. Wählen Sie **Metriken** für den **Signaltyp** aus, und wählen Sie in der Tabelle **Aufträge gesamt** aus.

1. Auf der Seite **Signallogik konfigurieren** definieren Sie die Logik, die die Warnung auslöst. Unter dem Verlaufsdiagramm werden Ihnen zwei Dimensionen angezeigt, **Runbookname** und **Status**. Dimensionen sind verschiedene Eigenschaften für eine Metrik, die zum Filtern von Ergebnissen verwendet werden kann. Wählen Sie unter **Runbookname** das Runbook aus, für das eine Warnung ausgegeben werden soll, oder lassen Sie das Feld leer, um für alle Runbooks Warnungen auszugeben. Wählen Sie unter **Status** in der Dropdownliste einen Status aus, den Sie überwachen möchten. Der Name des Runbooks und die Statuswerte, die in der Dropdownliste angezeigt werden, gelten nur für Aufträge, die in der letzten Woche ausgeführt wurden.

   Wenn Sie zu einem Status oder einem Runbook eine Warnung ausgegeben werden soll, der bzw. das nicht in der Dropdownliste angezeigt wird, klicken Sie auf das **\+** neben der Dimension. Dadurch wird ein Dialogfeld geöffnet, in dem Sie einen benutzerdefinierten Wert eingeben können, der in letzter Zeit für diese Dimension nicht ausgegeben wurde. Wenn Sie einen Wert eingeben, der für eine Eigenschaft nicht existiert, wird Ihre Warnung nicht ausgelöst.

1. Definieren Sie unter **Warnungslogik** die Bedingung und den Schwellenwert für Ihre Warnung. Unten sehen Sie eine Vorschau Ihrer definierten Bedingung.

1. Wählen Sie unter **Auswertung basierend auf** die Zeitspanne für die Abfrage und wie oft die Abfrage ausgeführt werden soll. Wenn Sie beispielsweise **In den letzten 5 Minuten** für **Zeitraum** und **Jede Minute** für **Häufigkeit** wählen, sucht die Warnung nach der Anzahl der Runbooks, die Ihre Kriterien in den letzten 5 Minuten erfüllt haben. Diese Abfrage wird jede Minute ausgeführt, und sobald die von Ihnen definierten Warnungskriterien nicht mehr in einem 5-Minuten-Fenster gefunden werden, löst sich die Warnung von selbst auf. Klicken Sie abschließend auf **Fertig**.

   ![Auswählen einer Ressource für die Warnung](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Definieren von Warnungsdetails

1. Wählen Sie unter **2. Warnungsdetails definieren** einen Anzeigenamen und eine Beschreibung für die Warnung aus. Stellen Sie den **Schweregrad** entsprechend Ihrer Warnungsbedingung ein. Es gibt fünf Schweregrade von 0 bis 5. Die Warnungen werden unabhängig vom Schweregrad gleich behandelt. Sie können den Schweregrad an Ihre Geschäftslogik anpassen.

1. Am Ende des Abschnitts befindet sich eine Schaltfläche, mit der Sie die Regel nach Abschluss aktivieren können. Standardmäßig sind die Regeln bei der Erstellung aktiviert. Wenn Sie „Nein“ auswählen, können Sie die Warnung im Zustand **Deaktiviert** erstellen. Sie können diese in Azure Monitor auf der Seite **Regeln** auswählen und auf **Aktivieren** klicken, sobald Sie diese aktivieren möchten.

### <a name="define-the-action-to-take"></a>Definieren der auszuführenden Aktion

1. Klicken Sie unter **3. Aktionsgruppe definieren** auf **+ Neue Aktionsgruppe**. Eine Aktionsgruppe ist eine Gruppe mit Aktionen, die Sie übergreifend für mehrere Warnungen verwenden können. Dies können beispielsweise E-Mail-Benachrichtigungen, Runbooks, Webhooks und vieles mehr sein. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../monitoring-and-diagnostics/monitoring-action-groups.md).

1. Geben Sie im Feld **Name der Aktionsgruppe** einen Anzeigenamen und einen Kurznamen an. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

1. Wählen Sie im Abschnitt **Aktionen** unter **AKTIONSTYP** die Option **E-Mail/SMS/Push/Sprachanruf** aus.

1. Vergeben Sie auf der Seite **E-Mail/SMS/Push/Sprachanruf** einen Namen. Aktivieren Sie das Kontrollkästchen **E-Mail**, und geben Sie eine gültige E-Mail-Adresse ein.

   ![Konfigurieren der E-Mail-Aktionsgruppe](./media/automation-alert-activity-log/add-action-group.png)

1. Klicken Sie auf der Seite **E-Mail/SMS/Push/Sprachanruf** auf **OK**, um sie zu schließen, und anschließend erneut auf **OK**, um die Seite **Aktionsgruppe hinzuzufügen** zu schließen. Der auf dieser Seite angegebene Name wird als **AKTIONSNAME** gespeichert.

1. Klicken Sie auf **Speichern**, wenn Sie fertig sind. Dadurch wird die Regel erstellt, die Sie benachrichtigt, wenn ein Runbook mit einem bestimmten Status abgeschlossen wird.

> [!NOTE]
> Beim Hinzufügen einer E-Mail-Adresse zu einer Aktionsgruppe wird eine Benachrichtigungs-E-Mail gesendet, die angibt, dass die Adresse zu einer Aktionsgruppe hinzugefügt wurde.

## <a name="notification"></a>Benachrichtigung

Wenn die Warnungskriterien erfüllt sind, führt die Aktionsgruppe die definierte Aktion aus. Im Beispiel dieses Artikels wird eine E-Mail verschickt. In der folgenden Abbildung sehen Sie ein Beispiel für eine E-Mail, die Sie nach dem Auslösen der Warnung empfangen:

![E-Mail-Warnung](./media/automation-alert-activity-log/alert-email.png)

Wenn die Metrik nicht mehr außerhalb des definierten Schwellenwerts liegt, wird die Warnung deaktiviert und die Aktionsgruppe führt die definierte Aktion aus. Wenn ein E-Mail-Aktionstyp ausgewählt ist, wird eine Auflösungs-E-Mail gesendet, die angibt, dass sie aufgelöst wurde.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem folgenden Artikel fort, um mehr über andere Möglichkeiten zu erfahren, wie Sie Benachrichtigungen in Ihr Automation-Konto integrieren können.

> [!div class="nextstepaction"]
> [Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks](automation-create-alert-triggered-runbook.md)