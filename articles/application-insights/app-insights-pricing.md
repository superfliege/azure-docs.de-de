---
title: Verwalten von Preisen und Datenvolumen für Azure Application Insights | Microsoft Docs
description: Verwalten Sie Telemetriedatenvolumen, und überwachen Sie Kosten in Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 2c06c2220d3a3ed0a27b4f0febb4de95b2137ddc
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Verwalten von Preisen und Datenvolumen in Application Insights

Die Preise für [Azure Application Insights][start] basieren auf dem Datenvolumen pro Anwendung. Jede Application Insights-Ressource wird als separater Dienst abgerechnet und auf der Rechnung für Ihr Azure-Abonnement aufgeführt.

Application Insights bietet zwei Tarife: Basic und Enterprise. Der Basic-Tarif ist der Standardtarif. Er umfasst alle Features des Enterprise-Tarifs, die ohne Aufpreis genutzt werden können. Im Basic-Tarif wird in erster Linie das Volumen der erfassten Daten abgerechnet. 

Für den Enterprise-Tarif gilt eine knotenbasierte Gebühr, und jedem Knoten wird ein tägliches Datenkontingent zugewiesen. Im Enterprise-Tarif werden die Daten, die über das enthaltene Datenkontingent hinausgehen, berechnet. Wenn Sie die Operations Management Suite verwenden, sollten Sie den Enterprise-Tarif wählen. 

Wenn Sie Fragen zu den Preisen für Application Insights haben, können Sie gerne eine Frage in unserem [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights) posten.

## <a name="pricing-plans"></a>Tarife

Aktuelle Preise in Ihrer Währung und Region finden Sie auf der Seite [Application Insights – Preise][pricing].

> [!NOTE]
> Im April 2018 haben wir ein [neues Preismodell für die Azure-Überwachung eingeführt](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/). Bei diesem Modell gilt für das komplette Portfolio der Überwachungsdienste das einfache Prinzip der nutzungsbasierten Bezahlung. Erfahren Sie mehr über das [neue Preismodell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), die [Bewertung der Auswirkungen einer Migration zu diesem Modell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) basierend auf Ihren Verwendungsmustern und [die Auswahl des neuen Modells](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model).

### <a name="basic-plan"></a>Basic-Tarif

Der Basic-Tarif ist der Standardtarif, wenn eine neue Application Insights-Ressource erstellt wird. Der Basic-Tarif ist für alle Kunden die ideale Wahl, ausgenommen Kunden, die über ein Operations Management Suite-Abonnement verfügen.

* Im Basic-Tarif erfolgt die Abrechnung nach Datenvolumen. Das Datenvolumen bezieht sich auf die Anzahl von Bytes an Telemetriedaten, die von Application Insights empfangen werden. 
    
    Das Datenvolumen wird anhand der Größe des nicht komprimierten JSON-Datenpakets gemessen, das Application Insights von Ihrer Anwendung empfängt.

    Für [in Analytics importierte Tabellendaten](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import) wird das Datenvolumen als die nicht komprimierte Größe von Dateien gemessen, die an Application Insights gesendet werden.
* [Live Metrics Stream](app-insights-live-stream.md)-Daten werden bei der Preisgestaltung nicht berechnet.
* [Fortlaufender Export](app-insights-export-telemetry.md) und der [Azure Log Analytics-Connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) sind seit April 2018 im Basic-Tarif ohne zusätzliche Kosten verfügbar.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Enterprise-Tarif und Berechtigungen für Operations Management Suite-Abonnements

Wie [bereits angekündigt](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/) können Kunden, die Operations Management Suite E1 und E2 erwerben, Application Insights Enterprise ohne weitere Kosten als zusätzliche Komponente erhalten. Jede Einheit der Operations Management Suite-Pläne E1 und E2 umfasst eine Berechtigung für einen Knoten des Application Insights Enterprise-Tarifs. Jeder Application Insights-Knoten ermöglicht das Erfassen von bis zu 200 MB Daten pro Tag (getrennt von der Log Analytics-Datenerfassung) bei einer Datenaufbewahrungsdauer von 90 Tagen ohne zusätzliche Kosten. Der Tarif wird weiter unten in diesem Artikel ausführlicher erläutert. 

Da dieser Tarif nur für Kunden mit einem Operations Management Suite-Abonnement gilt, sehen Kunden ohne Operations Management Suite-Abonnement keine Option zum Auswählen dieses Tarifs.

> [!NOTE]
> Um sicherzustellen, dass Sie diese Berechtigung erhalten, muss für Ihre Application Insights-Ressourcen der Enterprise-Tarif gelten. Diese Berechtigung gilt nur basierend auf Knoten. Für Application Insights-Ressourcen im Basic-Tarif werden keine Vorteile gewährt. Diese Berechtigung ist nicht in den geschätzten Kosten ausgewiesen, die im Bereich **Nutzung und geschätzte Kosten** gezeigt werden. Wenn Sie zudem ein Abonnement im April 2018 zum neuen Preismodell für die Azure-Überwachung migriert haben, ist der Basic-Tarif der einzige verfügbare Tarif. Wenn Sie über ein Operations Management Suite-Abonnement verfügen, wird davon abgeraten, dieses zum neuen Preismodell für die Azure-Überwachung zu migrieren.

Weitere Informationen zum Enterprise-Tarif finden Sie unter [Details des Enterprise-Tarifs](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Webtests mit mehreren Schritten

Für [mehrstufige Webtests](app-insights-monitor-web-app-availability.md#multi-step-web-tests) wird eine zusätzliche Gebühr erhoben. Mehrstufige Webtests sind Webtests, die eine Sequenz von Aktionen ausführen.

Es gibt keine gesonderte Gebühr für *Pingtests* einer einzelnen Seite. Telemetriedaten aus Pingtests und mehrstufigen Tests werden ebenso wie andere Telemetriedaten aus Ihrer App in Rechnung gestellt.

## <a name="review-pricing-plans-and-estimate-costs"></a>Überprüfen von Tarifen und Schätzen von Kosten

Application Insights erleichtert das Verständnis der verfügbaren Tarife und der Kosten, die basierend auf den aktuellen Verwendungsmustern voraussichtlich anfallen. Öffnen Sie zunächst im Azure-Portal in der Application Insights-Ressource den Bereich **Nutzung und geschätzte Kosten**:

![Auswählen von Preisen](./media/app-insights-pricing/pricing-001.png)

A. Überprüfen Sie Ihr Datenvolumen für den Monat. Dies schließt alle nach einer beliebigen [Stichprobenerstellung](app-insights-sampling.md) in Ihren Server- und Client-Apps sowie Verfügbarkeitstests empfangenen und beibehaltenen Daten ein.  
B: Für [Webtests mit mehreren Schritten](app-insights-monitor-web-app-availability.md#multi-step-web-tests) wird eine gesonderte Gebühr erhoben. (Darin nicht enthalten sind einfache Verfügbarkeitstests, die von der Gebühr für das Datenvolumen abgedeckt sind.)  
C. Zeigen Sie Trends zum Datenvolumen für den letzten Monat an.  
D: Aktivieren Sie die Datenerfassungs-[Stichprobenerstellung](app-insights-sampling.md).   
E. Legen Sie die Obergrenze für das tägliche Datenvolumen fest.  

Die Gebühren für Application Insights werden Ihrer Azure-Rechnung hinzugefügt. Sie können Details zu Ihrer Azure-Rechnung im Bereich **Abrechnung** des Azure-Portals oder im [Azure-Abrechnungsportal](https://account.windowsazure.com/Subscriptions) einsehen. 

![Auswählen der Option „Abrechnung“ im Menü auf der linken Seite](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Datenrate
Das Volumen der Daten, die Sie senden, ist auf dreierlei Weise beschränkt:

* **Stichprobenerstellung**: Mit der Stichprobenerstellung kann die Menge der von Ihrem Server und Ihren Client-Apps gesendeten Telemetriedaten bei minimaler Verzerrung von Metriken verringert werden. Die Stichprobenerstellung ist Ihr wichtigstes Tool, um die Menge der von Ihnen gesendeten Daten zu optimieren. Erfahren Sie mehr über die [Merkmale von Stichproben](app-insights-sampling.md). 
* **Tägliche Obergrenze**: Beim Erstellen einer Application Insights-Ressource im Azure-Portal ist die tägliche Obergrenze auf 100 GB pro Tag festgelegt. Die Standardeinstellung beim Erstellen einer Application Insights-Ressource über Visual Studio ist klein (nur 32,3 MB pro Tag). Zur Vereinfachung von Tests ist der Standardwert für die tägliche Obergrenze festgelegt. Es ist vorgesehen, dass der Benutzer vor dem Bereitstellen der App in der Produktion die tägliche Obergrenze erhöht. 

    Die Obergrenze liegt bei 1.000 GB pro Tag, es sei denn, Sie fordern einen höheren Maximalwert für eine Anwendung mit hohem Datenverkehr an. 

    Überlegen Sie gründlich, wenn Sie die tägliche Obergrenze festlegen. Sie sollten darauf achten, *niemals die tägliche Obergrenze zu erreichen*. Anderenfalls verlieren Sie die Daten des restlichen Tages, und Sie können nicht Ihre Anwendung überwachen. Um die tägliche Obergrenze zu ändern, verwenden Sie die Option **Obergrenze für das tägliche Volumen**. Sie können über den Bereich **Nutzung und geschätzte Kosten** auf diese Option zugreifen. (Dies wird weiter unten in diesem Artikel ausführlicher beschrieben.)
    Wir haben die Beschränkung für bestimmte Abonnementtypen entfernt, bei denen Guthaben gewährt wurden, die nicht für Application Insights verwendet werden konnten. Wenn zuvor für das Abonnement ein Ausgabenlimit galt, enthielt das Dialogfeld mit der täglichen Obergrenze Anweisungen, wie das Ausgabenlimit zu entfernen ist und wie die tägliche Obergrenze über 32,3 MB pro Tag erhöht werden kann.
* **Drosselung**: Durch eine Drosselung wird die Datenrate auf 32.000 Ereignisse pro Sekunde eingeschränkt (gemittelt über 1 Minute).

*Was geschieht, wenn meine App die Drosselungsrate überschreitet?*

* Die Datenmenge, die Ihre App sendet, wird minütlich gemessen. Wenn sie die pro Sekunde, über eine Minute gemittelt Datenmenge überschreitet, lehnt der Server einige Anforderungen ab. Das SDK puffert die Daten und versucht dann, diese erneut zu senden. Es führt zu einem mehrere Minuten andauernden Anstieg. Wenn Ihre App die Drosselungsrate beim Senden von Daten laufend überschreitet, werden einige Daten gelöscht. (Die ASP.NET-, Java- und JavaScript-SDKs versuchen, Daten auf diese Weise erneut zu senden, wohingegen andere SDKs gedrosselte Daten möglicherweise einfach löschen.) Tritt eine Drosselung auf, erhalten Sie zur Warnung eine Benachrichtigung über diesen Vorgang.

*Woher weiß ich, wie viele Daten meine App sendet?*

Sie können eine der folgenden Optionen verwenden, um festzustellen, wie viele Daten Ihre App sendet:

* Navigieren Sie zum Bereich **Nutzung und geschätzte Kosten**, um das Diagramm des täglichen Datenvolumens anzuzeigen. 
* Fügen Sie im Metrik-Explorer ein neues Diagramm hinzu. Wählen Sie für die Diagrammmetrik **Datenpunktvolumen** aus. Aktivieren Sie **Gruppierung**, und gruppieren Sie nach **Datentyp**.

## <a name="reduce-your-data-rate"></a>Verringern der Datenrate
Hier sind einige Schritte, die Sie ausführen können, um Ihr Datenvolumen zu reduzieren:

* Verwenden Sie [Stichproben](app-insights-sampling.md). Durch diese Technologie wird Ihre Datenrate ohne Datenschiefe bei Ihren Metriken reduziert. Sie können weiterhin zwischen verwandten Elementen in der Search navigieren. Bei Server-Apps erfolgt die Stichprobenerstellung automatisch.
* [Begrenzen Sie die Anzahl der gemeldeten AJAX-Aufrufe](app-insights-javascript.md#detailed-configuration) für jeden Seitenaufruf, oder deaktivieren Sie AJAX-Berichte.
* [Bearbeiten Sie „ApplicationInsights.config“](app-insights-configuration-with-applicationinsights-config.md), um nicht benötigte Sammlungsmodule zu deaktivieren. Das kann z. B. für Leistungsindikator- oder Abhängigkeitsdaten gelten.
* Teilen Sie Ihre Telemetrie auf getrennte Instrumentierungsschlüssel auf. 
* Aggregieren Sie Metriken vorab. Wenn Sie in Ihre App Aufrufe an TrackMetric eingefügt haben, können Sie Datenverkehr reduzieren, indem Sie die Überladung verwenden, die Ihre Berechnung des Durchschnitts und die Standardabweichung eines Batches von Messungen akzeptiert. Oder Sie können ein [vorab aggregierendes Paket](https://www.myget.org/gallery/applicationinsights-sdk-labs) verwenden.

## <a name="manage-the-maximum-daily-data-volume"></a>Verwalten des maximalen täglichen Datenvolumens

Zur Beschränkung der gesammelten Daten können Sie die Obergrenze für das tägliche Volumen verwenden. Wenn die Obergrenze erreicht ist, gehen sämtliche Telemetriedaten, die von Ihrer Anwendung gesendet wurden, für den restlichen Tag verloren. Von einer Erreichung der täglichen Obergrenze durch Ihre Anwendung wird *abgeraten*. Die Integrität und Leistung Ihrer Anwendung kann nicht nachverfolgt werden, nachdem sie die tägliche Obergrenze erreicht haben.

Verwenden Sie anstelle der Obergrenze für das tägliche Volumen die [Stichprobenerstellung](app-insights-sampling.md), um das Datenvolumen auf das gewünschte Niveau festzulegen. Verwenden Sie dann als letztes Mittel die tägliche Obergrenze für den Fall, dass Ihre Anwendung unerwartet beginnt, weitaus größere Mengen an Telemetriedaten zu senden.

Um die tägliche Obergrenze zu ändern, klicken Sie im Abschnitt **Konfigurieren** Ihrer Application Insights-Ressource auf der Seite **Nutzung und geschätzte Kosten** auf **Tägliche Obergrenze**.

![Anpassen der Volumenobergrenze für Telemetriedaten pro Tag](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Stichproben
Die [Stichprobenerstellung](app-insights-sampling.md) ist eine Methode, die Rate, mit der Telemetriedaten an Ihre App gesendet werden, zu verringern. Gleichzeitig soll die Möglichkeit erhalten bleiben, bei Diagnosesuchläufen relevante Ereignisse zu ermitteln. So erhalten Sie auch korrekte Ereigniszahlen.

Die Stichprobenerstellung ist eine effektive Möglichkeit, die Gebühren zu senken und innerhalb Ihres monatlichen Kontingents zu bleiben. Der Algorithmus für die Stichprobenerstellung behält Elemente in Bezug auf die Telemetrie bei, sodass Sie beispielsweise in Search die Anforderung ermitteln können, die in Beziehung zu einer bestimmten Ausnahme steht. Der Algorithmus behält außerdem korrekte Zahlenwerte bei, sodass Sie im Metrik-Explorer die richtigen Werte für Anforderungsraten, Ausnahmeraten und weitere Messwerte sehen.

Es gibt verschiedene Formen der Stichprobenerstellung.

* Die [adaptive Stichprobenerstellung](app-insights-sampling.md) ist die Standardeinstellung für das ASP.NET-SDK. Die adaptive Stichprobenerstellung wird automatisch an die Menge der an Ihre App gesendeten Telemetriedaten angepasst. Sie erfolgt im SDK Ihrer Web-App automatisch, sodass der Telemetriedatenverkehr im Netzwerk verringert wird. 
* *Erfassungs-Stichprobenerstellung* stellt eine Alternative dar, die an dem Punkt arbeitet, an dem Telemetriedaten von Ihrer App den Application Insights-Dienst erreichen. Die Erfassungs-Stichprobenerstellung wirkt sich nicht auf die Menge der Telemetriedaten aus, die von Ihrer App gesendet werden, verringert jedoch die Menge der Daten, die vom Dienst beibehalten werden. Mit der Erfassungs-Stichprobenerstellung können Sie das Kontingent verringern, das von Telemetriedaten von Browsern und anderen SDKs beansprucht wird.

Um die Erfassungs-Stichprobenerstellung festzulegen, navigieren Sie zum Bereich **Preise**:

![Klicken Sie im Bereich „Kontingent und Preise“ auf die Kachel „Stichproben“, und wählen Sie dann eine Einheit für die Stichprobenerstellung aus.](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> Über den Bereich **Datensampling** wird nur der Wert der Erfassungs-Stichprobenerstellung gesteuert. Die Rate der Stichprobenerstellung, die vom Application Insights-SDK in Ihrer App angewendet wird, wird nicht widergespiegelt. Falls für die eingehenden Telemetriedaten bereits im SDK Stichproben erstellt wurden, wird die Erfassungs-Stichprobenerstellung nicht angewendet.
>

Verwenden Sie eine [Analytics-Abfrage](app-insights-analytics.md), um den tatsächlichen Prozentsatz der Stichprobenerstellung unabhängig davon zu ermitteln, wo er angewendet wird. Die Abfrage sieht folgendermaßen aus:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

In jedem beibehaltenen Datensatz gibt `itemCount` die Anzahl ursprünglicher Datensätze an, die der Datensatz darstellt. Diese entspricht 1 + Anzahl der vorherigen verworfenen Datensätze. 

## <a name="automation"></a>Automation

Sie können mit der Azure-Ressourcenverwaltung ein Skript schreiben, um den Tarif festzulegen. [Weitere Informationen](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Zusammenfassung der Grenzwerte

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Stichproben](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
