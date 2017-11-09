---
title: "Verwalten der Kosten für Datenaufbewahrung in Azure Log Analytics | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Tarif und die Datenaufbewahrungsrichtlinie für Ihren Log Analytics-Arbeitsbereich im Azure-Portal ändern."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 35064c792b72222d59b1d3f0913a92a4a2b34612
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2017
---
# <a name="manage-cost-of-data-retention-with-your-log-analytics-workspace"></a>Verwalten der Kosten für Datenaufbewahrung in Ihrem Log Analytics-Arbeitsbereich
Wenn Sie sich bei Log Analytics anmelden, besteht abhängig vom ausgewählten Tarif ein Grenzwert dafür, wie lange die durch Ihre verbundenen Quellen generierten Daten in Ihrem Arbeitsbereich gespeichert werden.  In diesem Artikel werden die Erwägungen hervorgehoben, die die Kosten für das Aufbewahren dieser Daten über verschiedene Zeiträume und die Konfiguration des genannten Grenzwerts beeinflussen können.   

Da Log Analytics große Datenmengen aus lokalen Quellen, der Cloud und aus hybriden Umgebungen nutzen kann, können für die Speicherung dieser Daten über einen bestimmten Zeitraum in Abhängigkeit von den folgenden Faktoren beträchtliche Kosten anfallen:

* Anzahl der Systeme, Infrastrukturkomponenten, Cloudressourcen usw., aus denen Sie Daten sammeln
* Typ der von der Quelle erstellten Daten, z.B. Nachrichtenwarteschlangen, Protokolle, Ereignisse, sicherheitsrelevante Daten oder Leistungsmetriken
* Umfang der von diesen Quellen generierten Daten 
* Anzahl der aktivierten Managementlösungen, Datenquellen und Abrufhäufigkeit

> [!NOTE]
> Lesen Sie die Dokumentationen für die einzelnen Lösungen, da diese eine Schätzung des Umfang der gesammelten Daten bietet.   

Im Tarif *Free* ist die Aufbewahrung der Daten ist auf sieben Tage beschränkt.  In den Tarifen *Standalone* und *Paid* stehen die gesammelten Daten für 31 Tage zur Verfügung.  

Wenn Sie im Tarif *Free* feststellen, dass Sie die zulässige Menge konsistent überschreiten, können Sie Ihren Arbeitsbereich in einen kostenpflichtigen Tarif ändern, um Daten über diesen Grenzwert hinaus sammeln zu können. 

> [!NOTE]
> Gebühren fallen an, wenn Sie in einem kostenpflichtigen Tarif eine längere Aufbewahrungsdauer auswählen. Sie können Ihren Tariftyp jederzeit ändern. Weitere Informationen zu den Preisen finden Sie unter [Preisdetails](https://azure.microsoft.com/pricing/details/log-analytics/). 

## <a name="change-the-data-retention-period"></a>Ändern des Datenaufbewahrungszeitraums 

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an. 
2. Klicken Sie links unten auf **Weitere Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
3. Wählen Sie im Abonnementbereich von Log Analytics den zu ändernden Arbeitsbereich in der Liste aus.
4. Klicken Sie auf der Seite des Arbeitsbereichs im linken Bereich auf **Aufbewahrung**.
5. Verwenden Sie im Aufbewahrungsbereich für den Arbeitsbereich den Schieberegler, um die Anzahl Tage zu erhöhen oder zu verringern, und klicken Sie dann auf **Speichern**.  Wenn Sie sich im Tarif *Free* befinden, können Sie den Datenaufbewahrungszeitraum nicht ändern. Sie müssen in einen kostenpflichtigen Tarif wechseln, um diese Einstellung zu steuern.<br><br> ![Ändern des Datenaufbewahrungszeitraums für den Arbeitsbereich](media/log-analytics-manage-cost/manage-cost-change-retention.png)

## <a name="next-steps"></a>Nächste Schritte  

Wenn Sie zur Verwaltung des Verbrauchs und der Kosten festlegen möchten, wie viele Daten gesammelt werden und welche Quellen diese senden sowie welche Datentypen gesendet werden, finden Sie weitere Informationen unter [Analysieren der Datennutzung in Log Analytics](log-analytics-usage.md).