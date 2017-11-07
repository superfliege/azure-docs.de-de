---
title: "Konfigurieren von Warnungen zu Metriken für Azure Database for MySQL im Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal die Warnungen zu Metriken für Azure Database for MySQL konfigurieren und auf diese zugreifen."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/24/2017
ms.openlocfilehash: 5b4263e76f89ae27b9c9f317db3038def01e3e6b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for MySQL 

In diesem Artikel erfahren Sie, wie Sie mit dem Azure-Portal Azure Database for MySQL-Warnungen einrichten können. Sie können auf der Grundlage von Überwachungsmetriken für Ihre Azure-Services eine Warnung empfangen.

Die Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert überschreitet. Die Warnung wird sowohl ausgelöst, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird. 

Sie können konfigurieren, dass bei einer Warnung die folgenden Aktionen ausgeführt werden, wenn sie ausgelöst wird:
* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren
* Senden von E-Mails an weitere von Ihnen angegebene Adressen
* Aufrufen eines Webhooks

Sie haben folgende Möglichkeiten zum Konfigurieren von Warnungsregeln und Abrufen zugehöriger Informationen:
* [Azure-Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Befehlszeilenschnittstelle](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Erstellen einer Warnungsregel anhand einer Metrik aus dem Azure-Portal
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) den zu überwachenden Azure Database for MySQL-Server aus.

2. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Warnungsregeln** aus, wie unten gezeigt:

   ![„Warnungsregeln“ auswählen](./media/howto-alert-on-metric/1-alert-rules.png)

3. Wählen Sie **Metrikwarnung hinzufügen** (Plussymbol) aus. 

4. Die Seite **Regel hinzufügen** wird geöffnet, wie unten gezeigt.  Füllen Sie die erforderlichen Informationen aus:

   ![Formular „Metrikwarnung hinzufügen“](./media/howto-alert-on-metric/2-add-rule-form.png)

   | Einstellung | Beschreibung  |
   |---------|---------|
   | Name | Geben Sie einen Namen für die Warnungsregel an. Dieser Wert wird in der E-Mail zur Warnungsbenachrichtigung gesendet. |
   | Beschreibung | Geben Sie eine kurze Beschreibung für die Warnungsregel an. Dieser Wert wird in der E-Mail zur Warnungsbenachrichtigung gesendet. |
   | Warnung bei | Wählen Sie für diese Art der Warnung die Option **Metriken** aus. |
   | Abonnement | Dieses Feld ist bereits mit dem Abonnement ausgefüllt, das Ihr Azure Database for MySQL hostet. |
   | Ressourcengruppe | Dieses Feld ist bereits mit der Ressourcengruppe für Ihr Azure Database for MySQL ausgefüllt. |
   | Ressource | Dieses Feld ist bereits mit dem Namen für Ihr Azure Database for MySQL ausgefüllt. |
   | Metrik | Wählen Sie die Metrik aus, für die Sie eine Warnung ausgeben möchten. Beispiel: **Speicher in Prozent**. |
   | Bedingung | Wählen Sie die Bedingung aus, mit der die Metrik verglichen wird. Beispiel **Größer als**. |
   | Schwellenwert | Der Schwellenwert für die Metrik, z. B. 85 (Prozent). |
   | Zeitraum | Der Zeitraum für die Metrikregel, der erfüllt sein muss, ehe die Warnung ausgelöst wird. Beispiel: **Innerhalb der letzten 30 Minuten**. |

   Auf Basis des Beispiels sucht die Warnung nach „Speicher in Prozent“ über 85 % über einen Zeitraum von 30 Minuten. Diese Warnung wird ausgelöst, wenn der durchschnittliche Speicher in Prozent für 30 Minuten über 85 % lag. Nachdem der erste Trigger ausgelöst wurde, erfolgt ein erneutes Auslösen, wenn der durchschnittliche Speicher in Prozent für mehr als 30 Minuten unter 85 % bleibt.

5. Wählen Sie die gewünschte Benachrichtigungsmethode für die Warnungsregel aus. 

   Aktivieren Sie die Option **E-Mail-Besitzer, Mitwirkende und Leser**, wenn Sie möchten, dass Administratoren und Co-Administratoren per E-Mail benachrichtigt werden, wenn die Warnung ausgelöst wird.

   Wenn Sie möchten, dass bei Auslösen der Warnung eine Benachrichtigung an weitere E-Mail-Adressen gesendet wird, fügen Sie diese dem Feld **Zusätzliche Administrator-E-Mail-Adresse** hinzu. Trennen Sie mehrere E-Mail-Adressen durch Semikolons: *email@contoso.com;email2@contoso.com*

   Geben Sie optional einen gültigen URI im Feld **Webhook** an, wenn dieser bei Auslösen der Warnung aufgerufen werden soll.

6. Wählen Sie **OK** , um die Warnung zu erstellen.

   Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen
Nachdem Sie eine Warnung erstellt haben, können Sie sie auswählen und folgende Aktionen ausführen:

* Ein Diagramm anzeigen, das den Schwellenwert der Metrik und die tatsächlichen Werte vom Vortag zeigt, die für diese Warnung relevant sind.
* Die Warnungsregel **bearbeiten** oder **löschen**.
* Die Warnung **deaktivieren** oder **aktivieren**, wenn Sie den Empfang von Benachrichtigungen vorübergehend beenden oder fortsetzen möchten.


## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
