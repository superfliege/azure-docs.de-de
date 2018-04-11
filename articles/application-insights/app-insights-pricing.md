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
ms.openlocfilehash: 42c54509cedf48e9ebabb5b50865ed56e54bee05
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Verwalten von Preisen und Datenvolumen in Application Insights

Die Preise für [Azure Application Insights][start] basieren auf dem Datenvolumen pro Anwendung. Jede Application Insights-Ressource wird als separater Dienst abgerechnet und auf der Rechnung für Ihr Azure-Abonnement aufgeführt.

Es gibt zwei Tarife. Der Standardtarif heißt Basic und umfasst ohne Zusatzkosten die gleichen Features wie der Enterprise-Tarif. Die Abrechnung erfolgt in erster Linie anhand des erfassten Datenvolumens. Bei Verwendung der Operations Management Suite sollten Sie ggf. den Enterprise-Tarif wählen, für den eine Gebühr pro Knoten zusammen mit täglichen Zuteilungen gilt. Anschließend werden die über die Zuteilung hinausgehenden Daten berechnet.

Wenn Sie Fragen zu den Preisen für Application Insights haben, können Sie gerne eine Frage in unserem [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights) posten.

## <a name="the-price-plans"></a>Die Tarife

Aktuelle Preise in Ihrer Währung und Region finden Sie auf der [Application Insights-Seite mit Preisen][pricing].

### <a name="basic-plan"></a>Basic-Tarif

Der Basic-Tarif ist der Standard, wenn eine neue Application Insights-Ressource erstellt wird. Er ist optimal für alle Kunden, die nicht über ein Operations Management Suite-Abonnement verfügen.

* Im Basic-Tarif erfolgt die Abrechnung nach Datenvolumen: Anzahl von Bytes an Telemetriedaten, die von Application Insights empfangen werden. Das Datenvolumen wird anhand der Größe des nicht komprimierten JSON-Datenpakets gemessen, das Application Insights aus Ihrer Anwendung empfängt.
Für [in Analytics importierte Tabellendaten](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import) wird das Datenvolumen als die unkomprimierte Größe von Dateien gemessen, die an Application Insights gesendet werden.
* [Live Metrics Stream](app-insights-live-stream.md)-Daten werden bei der Preisgestaltung nicht berechnet.
* [Fortlaufender Export](app-insights-export-telemetry.md) und [Log Analytics-Connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) sind seit April 2018 im Basic-Tarif ohne zusätzliche Kosten verfügbar.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Enterprise-Tarif und Berechtigungen für Operations Management Suite-Abonnements

Wie [bereits angekündigt](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/) können Kunden, die Microsoft Operations Management Suite E1 und E2 erwerben, Application Insights Enterprise ohne weitere Kosten als zusätzliche Komponente erhalten. Jede Einheit der Operations Management Suite-Pläne E1 und E2 umfasst eine Berechtigung für einen Knoten des Enterprise-Tarifs von Application Insights. Wie weiter unten ausführlich beschrieben, ermöglicht jeder Application Insights-Knoten das Erfassen von bis zu 200 MB Daten pro Tag (getrennt von der Log Analytics-Datenerfassung) bei einer Datenaufbewahrungsdauer von 90 Tagen ohne zusätzliche Kosten. Da dies nur für Kunden mit einem Operations Management Suite-Abonnement gilt, sehen Kunden ohne Abonnement keine Option zum Auswählen dieses Tarifs.

> [!NOTE]
> Um sicherzustellen, dass Sie diese Berechtigung erhalten, muss für Ihre Application Insights-Ressourcen der Enterprise-Tarif gelten. Diese Berechtigung gilt nur für Knoten, sodass es für Application Insights-Ressourcen im Tarif Basic keine Vorteile gibt. Beachten Sie, dass diese Berechtigung nicht in den geschätzten Kosten ausgewiesen ist, die auf der Seite *Nutzung und geschätzte Kosten* gezeigt werden. Auch wenn Sie ein Abonnement in das neue Preismodell der Azure-Überwachung von April 2018 verschieben, ist der Basic-Tarif der einzige verfügbare Tarif. Sie ist also nicht empfehlenswert, wenn Sie über ein Operations Management Suite-Abonnement verfügen.

Weitere Informationen zum Enterprise-Tarif finden Sie auf der [Seite mit Enterprise-Preisdetails](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Webtests mit mehreren Schritten

Für [Webtests mit mehreren Schritten](app-insights-monitor-web-app-availability.md#multi-step-web-tests) wird eine zusätzliche Gebühr erhoben. Dies sind Webtests, die eine Sequenz von Aktionen ausführen.

Es gibt keine gesonderte Gebühr für „Pingtests“ einer einzelnen Seite. Telemetriedaten von Pingtests und Tests mit mehreren Schritten werden zusammen mit anderen Telemetriedaten aus Ihrer App in Rechnung gestellt.

## <a name="review-pricing-plans-and-estimate-costs"></a>Überprüfen von Tarifen und Schätzen von Kosten

Application Insights erleichtert das Verständnis der verfügbaren Tarife und der Kosten, die basierend auf den aktuellen Nutzungsmustern voraussichtlich anfallen. Öffnen Sie zuerst im Azure-Portal in der Application Insights-Ressource die Seite **Nutzung und geschätzte Kosten**:

![Wählen Sie „Preise“ aus.](./media/app-insights-pricing/pricing-001.png)

**a.** Überprüfen Sie Ihr Datenvolumen für den Monat. Dies schließt alle nach einer beliebigen [Stichprobennahme](app-insights-sampling.md) in Ihren Server- und Client-Apps sowie Verfügbarkeitstests empfangenen und beibehaltenen Daten ein.

**b.** Für [Webtests mit mehreren Schritten](app-insights-monitor-web-app-availability.md#multi-step-web-tests) wird eine gesonderte Gebühr erhoben. (Darin nicht enthalten sind einfache Verfügbarkeitstests, die von der Gebühr für das Datenvolumen abgedeckt sind.)

**c.** Zeigen Sie Trends zum Datenvolumen für den letzten Monat an.

**d.** Aktivieren Sie die [Stichprobenerstellung](app-insights-sampling.md) der Datenerfassung. 

**e.** Konfigurieren Sie die Obergrenze für das tägliche Datenvolumen.

Die Gebühren für Application Insights werden Ihrer Azure-Rechnung hinzugefügt. Sie sehen die Details zu Ihrer Azure-Rechnung im Bereich "Abrechnung" des Azure-Portals oder im [Azure-Abrechnungsportal](https://account.windowsazure.com/Subscriptions). 

![Wählen Sie im seitlichen Menü die Option „Abrechnung“.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Datenrate
Es gibt drei Möglichkeiten zum Begrenzen des gesendeten Datenvolumens:

* **Stichprobenerstellung**: Dieser Mechanismus kann die Menge der von Ihrem Server und Ihren Client-Apps gesendeten Telemetriedaten bei minimaler Verzerrung von Metriken verringern. Dies ist Ihr wichtigstes Tool zum Optimieren der Menge der Daten. Erfahren Sie mehr über die [Merkmale von Stichproben](app-insights-sampling.md). 
* **Tägliche Obergrenze**: Beim Erstellen einer Application Insights-Ressource über das Azure-Portal ist dieser Wert auf 100 GB pro Tag festgelegt. Die Standardeinstellung beim Erstellen einer Application Insights-Ressource in Visual Studio ist niedrig (nur 32,3 MB pro Tag) und nur zum Vereinfachen von Tests gedacht. In diesem Fall ist es vorgesehen, dass der Benutzer vor dem Bereitstellen der App in der Produktion die tägliche Obergrenze erhöht. Die Obergrenze liegt bei 1.000 GB pro Tag, es sei denn, Sie haben einen höheren Maximalwert für eine Anwendung mit hohem Datenverkehr angefordert. Seien Sie beim Festlegen der täglichen Obergrenze aufmerksam, da es Ihre Absicht sein muss, **die tägliche Obergrenze niemals zu erreichen**, weil dann für den Rest des Tages Daten verloren gehen und Sie nicht in der Lage sein werden, Ihre Anwendung zu überwachen. Um dies zu ändern, verwenden Sie die Option für das tägliche Volumen, verknüpft über die Seite „Nutzung und geschätzte Kosten“ (siehe unten). Wir haben die Beschränkung für bestimmte Abonnementtypen entfernt, für die Gutschriften gelten, die für Application Insights nicht verwendet werden konnten. Wenn zuvor für das Abonnement ein Ausgabenlimit galt, enthielt das Dialogfeld mit der täglichen Obergrenze Anweisungen, wie es aufzuheben ist und wie die tägliche Obergrenze über 32,3 MB pro Tag erhöht werden kann.
* **Drosselung**: Hiermit wird die Datenrate auf 32.000 Ereignisse pro Sekunde eingeschränkt (gemittelt über 1 Minute).

*Was geschieht, wenn meine App die Drosselungsrate überschreitet?*

* Die Datenmenge, die Ihre App sendet, wird minütlich gemessen. Wenn sie die pro Sekunde, über eine Minute gemittelt Datenmenge überschreitet, lehnt der Server einige Anforderungen ab. Das SDK puffert die Daten und versucht dann, erneut zu senden, wobei ein rascher Anstieg über mehrere Minuten hinweg verteilt wird. Wenn Ihre App die Drosselungsrate beim Senden von Daten laufend überschreitet, werden einige Daten gelöscht. (Die ASP.NET-, Java- und JavaScript-SDKs versuchen auf diese Weise erneut zu senden, andere SDKs löschen gedrosselte Daten möglicherweise einfach.) Tritt eine Drosselung auf, erhalten Sie zur Warnung eine Benachrichtigung über diesen Vorgang.

*Woher weiß ich, wie viele Daten meine App sendet?*

* Öffnen Sie die Seite **Nutzung und geschätzte Kosten**, um das Diagramm des täglichen Datenvolumens anzuzeigen. 
* Oder fügen Sie im Metrik-Explorer ein neues Diagramm hinzu, und wählen Sie **Datenpunktvolumen** als Metrik aus. Aktivieren Sie "Gruppierung", und gruppieren Sie nach **Datentyp**.

## <a name="to-reduce-your-data-rate"></a>So verringern Sie die Datenrate
Hier sind einige Schritte, die Sie ausführen können, um Ihr Datenvolumen zu reduzieren:

* Verwenden Sie [Stichproben](app-insights-sampling.md). Diese Technologie verringert die Datenrate, ohne die Metriken zu verzerren und ohne die Navigation zwischen verwandten Elementen bei der Suche zu stören. In Server-Apps arbeitet sie automatisch.
* [Begrenzen Sie die Anzahl der gemeldeten AJAX-Aufrufe](app-insights-javascript.md#detailed-configuration) für jeden Seitenaufruf, oder deaktivieren Sie AJAX-Berichte.
* Deaktivieren Sie nicht benötigte Erfassungsmodule durch [Bearbeiten von „ApplicationInsights.config“](app-insights-configuration-with-applicationinsights-config.md). Das kann z. B. für Leistungsindikator- oder Abhängigkeitsdaten gelten.
* Teilen Sie Ihre Telemetrie auf getrennte Instrumentierungsschlüssel auf. 
* Aggregieren Sie Metriken vorab. Wenn Sie Ihrer App Aufrufe an TrackMetric eingefügt haben, können Sie Datenverkehr reduzieren, indem Sie die Überladung verwenden, die Ihre Berechnung des Durchschnitts und die Standardabweichung eines Batches von Messungen akzeptiert. Oder Sie können ein [vorab aggregierendes Paket](https://www.myget.org/gallery/applicationinsights-sdk-labs)verwenden.

## <a name="managing-the-maximum-daily-data-volume"></a>Verwalten des maximalen täglichen Datenvolumens

Sie können die gesammelten Daten mithilfe der täglichen Volumenobergrenze begrenzen. Jedoch gehen alle von der Anwendung gesendeten Telemetriedaten für den Rest des Tags verloren, wenn die Obergrenze überschritten wurde. Es ist **nicht ratsam**, die Anwendung die tägliche Obergrenze erreichen zu lassen, da Sie nach dem Überschreiten der Obergrenze die Integrität und Leistung der Anwendung nicht mehr nachverfolgen können.

Verwenden Sie stattdessen [Stichproben](app-insights-sampling.md), um das gewünschte Datenvolumen anzupassen, und verwenden Sie die tägliche Obergrenze nur als letzten Ausweg für den Fall, dass die Anwendung unerwartet ein weitaus höheres Volumen an Telemetriedaten sendet.

Um die tägliche Obergrenze zu ändern, klicken Sie im Abschnitt „Konfigurieren“ Ihrer Application Insights-Ressource auf der Seite **Nutzung und geschätzte Kosten** auf **Tägliche Obergrenze**.

![Anpassen der Volumenobergrenze für Telemetriedaten pro Tag](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Stichproben
Die [Stichprobenerstellung](app-insights-sampling.md) ist eine Methode, die Rate, mit der Telemetriedaten an Ihre App gesendet werden, zu verringern. Gleichzeitig soll die Möglichkeit erhalten bleiben, bei Diagnosesuchläufen relevante Ereignisse zu ermitteln und korrekte Ereigniszahlen zu erhalten.

Die Stichprobenerstellung ist eine effektive Möglichkeit, die Gebühren zu senken und innerhalb Ihres monatlichen Kontingents zu bleiben. Der Stichprobenalgorithmus behält Elemente in Bezug auf die Telemetrie bei, sodass Sie beispielsweise in Search die Anforderung ermitteln können, die in Beziehung zu einer bestimmten Ausnahme steht. Der Algorithmus behält außerdem korrekte Zahlenwerte bei, sodass Sie im Metrik-Explorer die richtigen Werte für Anforderungsraten, Ausnahmeraten und weitere Messwerte sehen.

Es gibt verschiedene Formen der Stichprobenerstellung.

* [Adaptive sampling](app-insights-sampling.md) verwendet, bei der die Menge der an Ihre App gesendeten Telemetriedaten automatisch angepasst wird. Die Stichprobenerstellung erfolgt im SDK Ihrer Web-App automatisch, sodass der Telemetriedatenverkehr im Netzwerk verringert wird. 
* *Erfassungs-Stichprobenerstellung* stellt eine Alternative dar, die an dem Punkt arbeitet, an dem Telemetriedaten von Ihrer App den Application Insights-Dienst erreichen. Diese Art der Stichprobenerstellung wirkt sich nicht auf die Menge der Telemetriedaten aus, die von Ihrer App gesendet werden, verringert jedoch die Menge der Daten, die vom Dienst beibehalten werden. Damit können Sie das Kontingent verringern, das von Telemetriedaten von Browsern und anderen SDKs beansprucht wird.

Konfigurieren Sie zum Festlegen der Erfassungs-Stichprobenerstellung die Einstellung im Dialogfeld „Preise“:

![Klicken Sie im Dialogfeld „Kontingent und Preise“ auf die Kachel „Stichproben“, und wählen Sie eine Einheit für die Stichprobenerstellung.](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> Über das Dialogfeld „Datensampling“ wird nur der Wert für die Erfassung von Stichproben gesteuert. Die Samplingrate, die vom Application Insights SDK in Ihrer App angewendet wird, wird nicht widergespiegelt. Falls für die eingehenden Telemetriedaten bereits im SDK Stichproben erstellt wurden, wird die Erfassungs-Stichprobenerstellung nicht angewendet.
>

Verwenden Sie etwa folgende [Analytics-Abfrage](app-insights-analytics.md) , um den tatsächlichen Stichproben-Prozentsatz unabhängig davon zu ermitteln, wo er angewendet wird:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

In jedem beibehaltenen Datensatz gibt `itemCount` die Anzahl ursprünglicher Datensätze an, die der Datensatz darstellt (Anzahl zuvor verworfener Datensätze + 1). 

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
