---
title: Verwalten der Kosten für Daten in Azure Log Analytics | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie in Azure den Tarif ändern und das Datenvolumen sowie die Aufbewahrungsrichtlinie für Ihren Log Analytics-Arbeitsbereich verwalten.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: magoedte
ms.openlocfilehash: 9a360b41b24f4aca3c3aba29387ecd55faf881b7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="manage-cost-by-controlling-data-volume-and-retention-in-log-analytics"></a>Verwalten der Kosten durch die Steuerung der Datenmenge und -aufbewahrung in Log Analytics
Log Analytics ist für die Skalierung und Unterstützung der täglichen Sammlung, Indizierung und Speicherung enormer Datenmengen aus beliebigen Quellen in Ihrem Unternehmen oder aus in Azure bereitgestellten Quellen konzipiert.  Dies ist zwar ggf. die primäre Motivation für die Verwendung in Ihrem Unternehmen, letztendlich geht es jedoch um Kosteneffizienz. In diesem Zusammenhang ist es wichtig zu verstehen, dass die Kosten eines Log Analytics-Arbeitsbereichs nicht nur auf dem Umfang der gesammelten Daten basieren, sondern auch davon abhängen, welcher Tarif gewählt wurde und wie lange die von den verbundenen Quellen generierten Daten gespeichert werden sollen.  

In diesem Artikel erfahren Sie, wie Sie Datenvolumen und Speicherwachstum proaktiv überwachen und Grenzwerte festlegen, um die damit verbundenen Kosten zu steuern. 

Für Daten können abhängig von den folgenden Faktoren erhebliche Kosten anfallen: 

- Anzahl der Systeme, Infrastrukturkomponenten, Cloudressourcen usw., aus denen Sie Daten sammeln 
- Typ der von der Quelle erstellten Daten, z.B. Nachrichtenwarteschlangen, Protokolle, Ereignisse, sicherheitsrelevante Daten oder Leistungsmetriken 
- Umfang der Daten, die durch diese Quellen generiert und im Arbeitsbereich erfasst werden 
- Aufbewahrungsdauer der Daten im Arbeitsbereich  
- Anzahl aktivierter Managementlösungen, Datenquelle und Sammlungshäufigkeit 

Lesen Sie die Dokumentationen für die einzelnen Lösungen, da diese eine Schätzung des Umfang der gesammelten Daten bietet.   

Im Tarif „Free“ ist die Aufbewahrung der Daten auf sieben Tage beschränkt. Bei den Tarifen "Pro GB (eigenständig)“ und „Pro Knoten (OMS)“ sind gesammelte Daten der letzten 31 Tage verfügbar, und die Aufbewahrungsdauer kann auf bis zu zwei Jahre erhöht werden. Gebühren fallen an, wenn Sie eine längere Aufbewahrungsdauer auswählen. Im Tarif „Free“ liegt das Erfassungslimit bei 500 MB pro Tag. Sollten Sie dieses zulässige Volumen immer wieder überschreiten, können Sie Ihren Arbeitsbereich auf den GB- oder knotenbasierten Tarif umstellen, um Daten über diesen Grenzwert hinaus zu sammeln. Sie können Ihren Tariftyp jederzeit ändern. Weitere Informationen zu den Preisen finden Sie unter [Preisdetails](https://azure.microsoft.com/pricing/details/log-analytics/). 

> [!NOTE]
> Im April 2018 haben wir ein [neues Preismodell für die Azure-Überwachung eingeführt](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/). Bei diesem Modell gilt für das gesamte Portfolio der Überwachungsdienste das einfache Prinzip der nutzungsbasierten Bezahlung. Erfahren Sie mehr über das [neue Preismodell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), die [Bewertung der Auswirkungen einer Migration zu diesem Modell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) basierend auf Ihren Verwendungsmustern und [die Auswahl des neuen Modells](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model). 

Die Verwaltung der Datenmenge ist unabhängig vom Preismodell oder Tarif ein fundamentaler Aspekt der Kostenkontrolle. Abgesehen von der Wahl und Konfiguration der spezifischen Lösung kann das Datenvolumen in Log Analytics auf zwei Arten beschränkt werden: tägliche Obergrenze und Datenaufbewahrung.  

## <a name="review-estimated-cost"></a>Überprüfen der geschätzten Kosten
Mit Log Analytics können Sie auf der Grundlage aktueller Nutzungsmuster problemlos die zu erwartenden Kosten ermitteln.  Führen Sie dazu die folgenden Schritte aus.  

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an. 
2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.<br><br> ![Azure-Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. Wählen Sie im Bereich mit den Log Analytics-Abonnements Ihren Arbeitsbereich aus, und klicken Sie anschließend im linken Bereich auf **Nutzung und geschätzte Kosten**.<br><br> ![Seite „Nutzung und geschätzte Kosten“](media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>

Hier können Sie Ihr Datenvolumen für den Monat überprüfen. Dieses beinhaltet alle Daten, die in Ihrem Log Analytics-Arbeitsbereich empfangen und aufbewahrt wurden.  Klicken Sie im oberen Bereich der Seite auf **Nutzungsdetails**, um das Dashboard „Nutzung“ anzuzeigen. Dort finden Sie Informationen zu Datenvolumentrends nach Quelle, Computern und Angebot. Klicken Sie auf **Datenmengenverwaltung**, um die tägliche Obergrenze anzuzeigen oder festzulegen oder um die Aufbewahrungsdauer zu ändern.
 
Die Gebühren für Log Analytics fließen in Ihre Azure-Rechnung ein. Die Details Ihrer Azure-Rechnung finden Sie im Bereich „Abrechnung“ des Azure-Portals oder im [Azure-Abrechnungsportal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Tägliche Obergrenze
Wenn Sie über das Azure-Portal einen Log Analytics-Arbeitsbereich mit dem Tarif *Free* erstellen, liegt die tägliche Obergrenze bei 500 MB pro Tag. Bei den anderen Tarifen gibt es kein Limit. Sie können eine tägliche Obergrenze konfigurieren und die tägliche Erfassung für Ihren Arbeitsbereich einschränken. Dabei ist jedoch Vorsicht geboten, da dieses Limit möglichst nicht erreicht werden sollte.  Andernfalls gehen die restlichen Daten für den Tag verloren, und die Integrität von Ressourcen, die IT-Diensten zugrunde liegen, kann nicht mehr zuverlässig überwacht werden.  Die tägliche Obergrenze ist dazu gedacht, einen unerwarteten Anstieg des Datenvolumens aus Ihren verwalteten Ressourcen zu verhindern und den Grenzwert einzuhalten – oder einfach ungeplante Gebühren für Ihren Arbeitsbereich zu vermeiden.  

Bei Erreichen des Tageslimits werden für den Rest des Tages keine kostenpflichtigen Datentypen mehr gesammelt.  Im oberen Seitenbereich erscheint ein Warnbanner für den ausgewählten Log Analytics-Arbeitsbereich, und an die Tabelle *Operation* wird unter der Kategorie **LogManagement** ein Vorgangsereignis gesendet. Die Datensammlung wird nach der unter *Daily limit will be set at* (Tageslimit wird festgelegt um) definierten Zurücksetzungszeit fortgesetzt. Es empfiehlt sich, eine Warnungsregel auf der Grundlage dieses Vorgangsereignisses zu definieren und so zu konfigurieren, dass bei Erreichen des Tageslimits für Daten eine Benachrichtigung erfolgt. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identifizieren des zu definierenden Tageslimits für Daten 
Informieren Sie sich unter [Analysieren der Datennutzung in Log Analytics](log-analytics-usage.md) über den Datenerfassungstrend sowie über die zu definierende tägliche Volumenobergrenze. Wählen Sie die Obergrenze mit Bedacht, da Sie Ihre Ressourcen nach Erreichen des Limits nicht mehr überwachen können. 

### <a name="manage-the-maximum-daily-data-volume"></a>Verwalten des maximalen täglichen Datenvolumens 
In den folgenden Schritten erfahren Sie, wie Sie ein Tageslimit für die von Log Analytics erfasste Datenmenge konfigurieren.  

1. Klicken Sie links in Ihrem Arbeitsbereich auf **Nutzung und geschätzte Kosten**.
2. Klicken Sie im oberen Bereich der Seite **Nutzung und geschätzte Kosten** für den ausgewählten Arbeitsbereich auf **Datenmengenverwaltung**. 
5. Die tägliche Obergrenze ist standardmäßig **AUS**. Klicken Sie auf **EIN**, um sie zu aktivieren, und legen Sie das Limit für das Datenvolumen in GB/Tag fest.<br><br> ![Konfigurieren des Log Analytics-Datenlimits](media/log-analytics-manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-limit-reached"></a>Warnen bei Erreichen des Limits
Im Azure-Portal wird bei Erreichen des Schwellenwerts für das Datenlimit zwar ein visueller Hinweis angezeigt, dieses Verhalten steht jedoch möglicherweise nicht im Einklang mit der gewünschten Behandlung von Betriebsproblemen, die eine umgehende Reaktion erfordern.  Wenn Sie eine Warnbenachrichtigung erhalten möchten, können Sie in Azure Monitor eine neue Warnregel erstellen.  Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor – Warnungen (Vorschauversion)](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).      

Im Anschluss finden Sie die empfohlenen Einstellungen für die Warnung:

* Ziel: Wählen Sie Ihre Log Analytics-Ressource aus.
* Kriterien: 
   * Signalname: Custom log search
   * Suchabfrage: Operation | where Detail has 'OverQuota'
   * Basierend auf: Anzahl von Ergebnissen
   * Bedingung: Größer als
   * Schwellenwert: 0
   * Zeitraum: 5 (Minuten)
   * Häufigkeit: 5 (Minuten)
* Name der Warnungsregel: Daily data limit reached
* Schweregrad: Warnung (Schweregrad 1)

Nachdem die Warnung definiert wurde, wird bei Erreichen des Limits eine Warnung ausgelöst und die in der Aktionsgruppe definierte Reaktion ausgeführt. Dadurch kann Ihr Team per E-Mail und SMS benachrichtigt werden, und es können Aktionen mithilfe von Webhooks oder Automation-Runbooks oder mittels [Integration in eine externe ITSM-Lösung](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts) automatisiert werden. 

## <a name="change-the-data-retention-period"></a>Ändern des Datenaufbewahrungszeitraums 
Die folgenden Schritte zeigen, wie Sie die Aufbewahrungsdauer von Protokolldaten in Ihrem Arbeitsbereich konfigurieren.
 
1. Klicken Sie links in Ihrem Arbeitsbereich auf **Nutzung und geschätzte Kosten**.
2. Klicken Sie im oberen Bereich der Seite **Nutzung und geschätzte Kosten** auf **Datenmengenverwaltung**.
5. Passen Sie mithilfe des Schiebereglers die Anzahl von Tagen an, und klicken Sie anschließend auf **OK**.  Wenn Sie sich im Tarif *Free* befinden, können Sie den Datenaufbewahrungszeitraum nicht ändern. Sie müssen in einen kostenpflichtigen Tarif wechseln, um diese Einstellung zu steuern.<br><br> ![Ändern des Datenaufbewahrungszeitraums für den Arbeitsbereich](media/log-analytics-manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="troubleshooting"></a>Problembehandlung
**Frage:** Was kann ich tun, wenn Log Analytics keine Daten mehr erfasst? 
**Antwort:** Wenn Sie den Tarif „Free“ verwenden und an einem Tag mehr als 500 MB Daten gesendet haben, wird die Datensammlung für den Rest des Tages beendet. Das Erreichen des Tageslimits ist häufig die Ursache dafür, dass Log Analytics die Datensammlung beendet oder Daten scheinbar fehlen.  
Log Analytics erstellt ein Ereignis vom Typ „Operation“, wenn die Datensammlung beginnt und endet.  
Führen Sie über die Suche die folgende Abfrage aus, um zu überprüfen, ob Sie das Tageslimit erreichen und Daten fehlen: Operation | where OperationCategory == 'Data Collection Status'   
Wenn die Datensammlung beendet wird, hat „OperationStatus“ den Wert „Warning“ (Warnung). Wenn die Datensammlung beginnt, hat „OperationStatus“ den Wert „Succeeded“ (Erfolgreich).  
Die folgende Tabelle beschreibt die Gründe, warum die Datensammlung endet, und eine empfohlene Aktion zum Fortsetzen der Datensammlung:  

|Grund für die Beendigung der Datensammlung| Lösung| 
|-----------------------|---------|
|Tageslimit für kostenlose Daten erreicht<sup>1</sup>|Warten Sie, bis die Datensammlung am Folgetag automatisch neu gestartet wird, oder wechseln Sie zu einem kostenpflichtigen Tarif.|
|Das in der Datenmengenverwaltung definierte Tageslimit wurde erreicht.|Warten Sie, bis die Datensammlung am Folgetag automatisch neu gestartet wird, oder erhöhen Sie das Tageslimit für das Datenvolumen, wie unter [Verwalten des maximalen täglichen Datenvolumens](#manage-the-maximum-daily-volume) beschrieben.|
|Das Azure-Abonnement befindet sich aus folgendem Grund in einem angehaltenen Zustand:<br> Kostenlose Testversion endete<br> Azure Pass ist abgelaufen<br> Monatliches Ausgabenlimit ist erreicht (z.B. in einem MSDN- oder Visual Studio-Abonnement)|Konvertieren in ein kostenpflichtiges Abonnement<br> Limit entfernen oder warten, bis das Limit zurückgesetzt wird|

<sup>1</sup> Wenn Ihr Arbeitsbereich im kostenlosen Tarif liegt, können Sie täglich nur 500 MB an Daten an den Dienst senden. Wenn Sie das Tageslimit erreichen, wird die Datensammlung bis zum nächsten Tag angehalten. Daten, die gesendet werden, während die Datensammlung angehalten ist, werden nicht indiziert und stehen nicht für die Suche zur Verfügung. Wenn die Datensammlung fortgesetzt wird, werden nur neu gesendete Daten verarbeitet. 

In Log Analytics gilt die UTC-Zeit. Die Zurücksetzungszeit variiert zwischen Arbeitsbereichen, damit nicht alle mit einer Obergrenze versehenen Arbeitsbereiche gleichzeitig mit der Erfassung von Daten beginnen. Erreicht der Arbeitsbereich das Tageslimit, wird die Verarbeitung nach der Zurücksetzungszeit fortgesetzt, die in **Daily limit will be set at** (Tageslimit wird festgelegt um) definiert ist.<br><br> ![UTC-Zeitzone für das Log Analytics-Limit](media/log-analytics-manage-cost-storage/data-volume-mgmt-limit-utc.png)

**Frage:** Wie kann ich benachrichtigt werden, wenn die Datensammlung beendet wird? 
**Antwort**: Verwenden Sie die *Schritte zum Erstellen einer täglichen Datenobergrenze*, um eine Benachrichtigung zu erhalten, wenn die Datensammlung beendet wird, und verwenden Sie die Schritte zum Hinzufügen von Aktionen zu Warnungsregeln, um eine E-Mail-, Webhook- oder Runbook-Aktion für die Warnungsregel zu konfigurieren. 

## <a name="next-steps"></a>Nächste Schritte  

Unter [Analysieren der Datennutzung in Log Analytics](log-analytics-usage.md) erfahren Sie, wie Sie zur Verwaltung des Verbrauchs und der Kosten ermitteln, wie viele Daten gesammelt werden, von welchen Quellen sie stammen und welche Arten von Daten gesendet werden.
