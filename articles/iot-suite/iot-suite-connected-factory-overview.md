---
title: Übersicht über die Lösung für verbundene Factorys – Azure | Microsoft-Dokumentation
description: Eine Beschreibung des Anpassen des Azure IoT-Solution Accelerators für verbundene Factorys.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: c5fe6cf6dff6fd8951a949761739cb12f98834bf
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="get-started-with-the-connected-factory-solution-accelerator"></a>Erste Schritte mit dem Solution Accelerator für verbundene Factorys

Azure IoT-[Solution Accelerators][lnk-preconfigured-solutions] kombinieren mehrere Azure IoT-Dienste, um durchgängige Lösungen bereitzustellen, die allgemeine IoT-Unternehmensszenarien implementieren. Der Solution Accelerator für *verbundene Factorys* stellt eine Verbindung mit Ihren Industrieanlagen her und überwacht sie. Sie können die Lösung zum Analysieren des Datenstroms von Ihren Geräten und zum Steigern der Produktivität und Rentabilität während des Betriebs verwenden.

In diesem Tutorial erfahren Sie, wie Sie den Solution Accelerator für verbundene Factorys bereitstellen. Außerdem lernen Sie die grundlegenden Features des Solution Accelerators kennen. Sie können auf einen Großteil dieser Features über das *Lösungsdashboard* zugreifen, das zusammen mit dem Solution Accelerator bereitgestellt wird:

![Dashboard für den Solution Accelerator für verbundene Factorys][img-cf-home]

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

> [!NOTE]
> Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].

## <a name="provision-the-solution"></a>Bereitstellen der Lösung

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto bei „azureiotsuite.com“ an, und klicken Sie auf „**+**“, um eine Lösung zu erstellen.
2. Klicken Sie auf der Kachel **Verbundene Factory** auf **Auswählen**.
3. Geben Sie einen **Lösungsnamen** für Ihren Solution Accelerator für verbundene Factorys ein.
4. Wählen Sie die gewünschten Angaben für **Abonnement** und **Region**, um die Lösung bereitzustellen.
5. Klicken Sie auf **Lösung erstellen** , um den Bereitstellungsprozess zu beginnen. Dies dauert in der Regel einige Minuten.

### <a name="while-you-wait-for-the-provisioning-process-to-complete"></a>Während des Wartens auf den Abschluss des Bereitstellungsvorgangs

1. Klicken Sie auf die Kachel für Ihre Lösung mit dem Status **Bereitstellung** .
2. Beachten Sie die **Bereitstellungsstatus** während der Bereitstellung von Azure-Diensten im Azure-Abonnement.
3. Nach Abschluss der Bereitstellung ändert sich der Status in **Bereit**.
4. Klicken Sie auf die Kachel, um im rechten Bereich Details zur Lösung anzuzeigen.

> [!NOTE]
> Falls beim Bereitstellen des Solution Accelerators Probleme auftreten, finden Sie unter [Berechtigungen für die Website azureiotsuite.com][lnk-permissions] und die [häufig gestellten Fragen zu verbundenen Factorys](iot-suite-faq-cf.md) weitere Informationen. Sollten die Probleme weiterhin auftreten, können Sie im [Portal][lnk-portal] ein Dienstticket erstellen.

Erwarten Sie Informationen zu Ihrer Lösung, die nicht aufgeführt werden? Senden Sie uns Vorschläge für neue Funktionen über den [Feedbackbereich](https://feedback.azure.com/forums/321918-azure-iot) zu.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Wenn Sie den Solution Accelerator für verbundene Factorys bereitstellen, verfügt dieser bereits über Ressourcen, mit denen Sie ein allgemeines Industrieszenario durchlaufen können. Bei diesem Szenario melden mehrere Werke (Factories), die mit der Lösung verbunden sind, die Datenwerte, die zum Berechnen der Gesamtanlageneffektivität (Overall Equipment Effectiveness, OEE) und der Key Performance Indicators (KPIs) erforderlich sind. Im weiteren Verlauf wird Folgendes vermittelt:

* Überwachen von Werk, Fertigungsstrecken, OEE von Stationen und KPI-Werten
* Analysieren der Telemetriedaten, die über diese Geräte mit Azure Time Series Insights generiert werden
* Reagieren auf Alarme zum Beheben von Problemen

Ein wichtiges Merkmal dieses Szenarios: Die Aktionen können alle per Remotezugriff über das Lösungsdashboard ausgeführt werden. Sie benötigen also keinen physischen Zugriff auf die Geräte.

## <a name="view-the-solution-dashboard"></a>Anzeigen des Lösungsdashboards

Im Lösungsdashboard können Sie die bereitgestellte Lösung verwalten. Dies ist eine hierarchische Darstellung einer globalen Werkskonfiguration. Beispielsweise können Sie OEE und KPIs anzeigen und neue Knoten für Telemetriedaten und Aktionsalarme veröffentlichen.

1. Wenn die Bereitstellung abgeschlossen ist und auf der Kachel für den Solution Accelerator **Bereit** angezeigt wird, klicken Sie auf **Starten**, um eine neue Registerkarte mit dem Portal für die Lösung mit verbundener Factory zu öffnen.

    ![Starten des Solution Accelerator][img-launch-solution]

1. Standardmäßig zeigt das Lösungsportal das *Dashboard* an. Über das Menü auf der linken Seite gelangen Sie zu anderen Bereichen des Portals.

    ![Dashboard für den Solution Accelerator für verbundene Factorys][cf-img-menu]

Im Dashboard werden die folgenden Informationen angezeigt:

* Ein Bereich namens **Factorystandorte**, in dem der Status, der Speicherort und die aktuelle Produktionskonfiguration für die Lösung angezeigt werden. Beim ersten Ausführen der Lösung sind mehrere simulierte Geräte vorhanden. Die Simulation der Fertigungsstrecke besteht aus drei echten OPC UA-Servern (OPC Unified Architecture) pro Fertigungsstrecke zum Durchführen von simulierten Aufgaben und Freigeben von Daten. Weitere Informationen zu OPC UA finden Sie in den [häufig gestellten Fragen zu verbundenen Factorys](iot-suite-faq-cf.md).
* Eine **Karte** mit den Standorten der einzelnen Geräte, die mit der Lösung verbunden sind. Für die Lösung kann die Bing Maps-API zum Ausgeben von Informationen auf der Karte verwendet werden. Wenn Ihr Abonnement für die Bing Maps Enterprise-API aktiviert ist, wird dieses Feature automatisch verwendet. Wenn nicht, helfen Ihnen die Informationen zur Verwendung einer dynamischen Karte in den [häufig gestellten Fragen][lnk-faq] weiter.
* Ein Bereich namens **Alarme** mit Alarmen, die generiert werden, wenn ein Telemetrie- oder OEE-/KPI-Wert einen bestimmten Schwellenwert überschreitet.
* Ein Bereich **Overall Equipment Effectiveness**, in dem die OEE-Werte für das gesamte Unternehmen oder das jeweils angezeigte Werk, die Fertigungsstrecke oder die Station angegeben werden. Dieser Wert wird über die Stationsansicht auf Unternehmensebene aggregiert. Der OEE-Wert und die Elemente, aus denen dieser Wert besteht, können weiter analysiert werden.
* Ein Bereich **Key Performance Indicators**, in dem die Anzahl von produzierten Einheiten und der Energieverbrauch des gesamten Unternehmens bzw. des jeweiligen Werks, der Fertigungsstrecke oder der Station angezeigt werden. Diese Werte werden über eine Stationsansicht auf Unternehmensebene aggregiert.

## <a name="view-factories"></a>Anzeigen von Werken

Im Bereich *Factorystandorte* werden der geografische Standort aller Werke in der Lösung sowie jeweils ihr Status und die aktuelle Produktionskonfiguration angezeigt. Über die Liste mit den Standorten können Sie zu den anderen Ebenen der Lösungshierarchie navigieren. Die Zeilen in der Liste sind Hyperlinks, mit denen Details der Fertigungsstrecken am jeweiligen Standort verlinkt werden. Sie können dann einen Drilldown in die Details der Fertigungsstrecke und auf die Ebene der Stationsanzeige durchführen. Außerdem können Sie einen Filter auf die Liste anwenden.

![Factorys des Solution Accelerators für verbundene Factorys][cf-img-factories]

1. Im Bereich **Factory** (Werk) wird die Liste mit den Werken für diese Lösung angezeigt.

2. In der Liste mit den Werken werden anfänglich sechs Werke angezeigt, die über den Bereitstellungsprozess erstellt werden. Sie können der Lösung zusätzliche simulierte und physische Geräte hinzufügen.

3. Klicken Sie zum Anzeigen der Details eines Werks in der Liste mit den Werken auf die entsprechende Zeile.

4. Klicken Sie in der Liste auf die entsprechende Zeile, um die Details einer Fertigungsstrecke anzuzeigen.

5. Klicken Sie in der Liste auf die entsprechende Zeile, um die veröffentlichten OPC UA-Knoten einer Station der Fertigungsstrecke anzuzeigen.

6. Klicken Sie in der Liste auf die entsprechende Zeile, um die Details eines bestimmten Knotens der Station anzuzeigen. Mit dieser Aktion wird der Kontextbereich mit Time Series Insights-Visualisierungen gestartet. Klicken Sie auf diese Graphen, um in der Umgebung des Time Series Insights-Explorers weitere Analysen durchzuführen.

## <a name="view-map"></a>Anzeigen der Karte

Wenn für Ihr Abonnement Zugriff auf die Bing Maps-API besteht, werden Ihnen auf der Karte mit den *Werken* der geografische Standort und der Status aller Werke einer Lösung angezeigt. Klicken Sie auf die Standorte, die auf der Karte angezeigt werden, um Details zum jeweiligen Standort anzuzeigen.

![Übersicht über den Solution Accelerator für verbundene Factorys][cf-img-map]

## <a name="view-alarms"></a>Anzeigen von Alarmen

Der Bereich **Alarme** enthält Alarme, die generiert werden, wenn ein gemeldeter Wert oder ein berechneter OEE-/KPI-Wert den konfigurierten Schwellenwert übersteigt. In diesem Bereich werden Alarme auf jeder Ebene der Hierarchie angezeigt – von der Stationsebenenansicht bis zur globalen Ansicht. Die Alarme enthalten jeweils eine Beschreibung des Alarms, das Datum, die Uhrzeit, den Standort und die Anzahl von Vorkommen. Die Time Series Insights-Daten geben einen Einblick in die Daten, die zu dem Alarm geführt haben. Die Time Series Insights-Daten werden in den Alarmen visualisiert, soweit dies möglich ist. Als Administrator können Sie für die Alarme beispielsweise folgende Standardaktionen durchführen:

* Schließen des Alarms
* Bestätigen des Alarms

Optional können Sie auch komplexere Aktionen durchführen. Für den Knoten „Pressure OPC UA“ (OPC UA für Druck) der Baugruppe können Sie beispielsweise auch Folgendes durchführen:

* Anzeigen hilfreicher Informationen auf einer Webseite in einem neuen Browserfenster
* Beseitigen der Alarmursache durch Aufrufen einer OPC UA-Methode auf dem Gerät
* Unterdrücken der Verfügbarkeit der Standardaktionen

    ![Alarme des Solution Accelerators für verbundene Factorys][cf-img-alerts]

> [!NOTE]
> Diese Alarme werden durch Regeln generiert, die in einer Konfigurationsdatei im Solution Accelerator angegeben werden. Mit diesen Regeln können Alarme generiert werden, wenn die OEE- bzw. KPI-Werte oder die Werte des OPC UA-Knotens den konfigurierten Schwellenwert überschreiten.

1. Im Bereich **Alarme** werden die Alarme angezeigt, die für diese Lösung generiert wurden.

2. Klicken Sie im Bereich mit den Alarmen auf einen Alarm, um die dazugehörigen Details anzuzeigen.

3. Klicken Sie zur weiteren Analyse der Alarmdaten auf den Graphen, um die Time Series Insights-Explorerumgebung zu öffnen.

4. Im Bereich mit den Alarmen sind mehrere Aktionen zum Behandeln des Alarms verfügbar. Wählen Sie die jeweils passende Option aus, und klicken Sie auf die Schaltfläche zum Ausführen von Aktionen.

## <a name="view-overall-equipment-efficiency"></a>Anzeigen der Gesamtanlageneffektivität (Overall Equipment Effectiveness, OEE)

Mit dem OEE-Wert wird die Effizienz des Fertigungsprozesses bewertet, indem produktionsbezogene Betriebsparameter verwendet werden. OEE ist in der Industrie eine standardmäßige Kennzahl. Sie wird berechnet, indem der Verfügbarkeitsfaktor, der Leistungsfaktor und der Qualitätsfaktor miteinander multipliziert werden: OEE = Verfügbarkeit x Leistung x Qualität.

![OEE zum Solution Accelerator für verbundene Factorys][cf-img-oee]

1. Navigieren Sie zur jeweils gewünschten Ansicht, um den OEE-Wert für eine Ebene der Hierarchie anzuzeigen. Der OEE-Wert für diese Ansicht wird im Bereich zusammen mit den einzelnen Elementen angezeigt, die den OEE-Prozentsatz bilden.

2. Zur eingehenderen Analyse des OEE-Werts für eine beliebige Ebene der Hierarchiedaten können Sie auf den OEE-Prozentsatz, den Verfügbarkeitsprozentsatz, den Leistungsprozentsatz oder den Qualitätsprozentsatz klicken. Es wird ein Kontextbereich mit auf Time Series Insights basierenden Visualisierungen angezeigt, in dem Daten der letzten Stunde, letzten 24 Stunden und letzten sieben Tage angegeben sind.

    ![TSI-Visualisierung des Solution Accelerators für verbundene Factorys][cf-img-tsi-visualization]

3. Klicken Sie im Bereich mit den Alarmen auf den Graphen, um die Alarmdaten eingehender zu analysieren. Mit dieser Aktion wird die Umgebung für den Time Series Insights-Explorer geöffnet.

    ![TSI-Explorer für den Solution Accelerator für verbundene Factorys][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a>Anzeigen von Key Performance Indicators

Die Lösung enthält zwei Key Performance Indicators: *Einheiten pro Stunde* und *Energieverbrauch in kWh*.

![KPI des Solution Accelerators für verbundene Factorys][cf-img-kpi]

1. Navigieren Sie zur jeweiligen Ansicht, um für eine beliebige Ebene der Hierarchie die Einheiten pro Stunde oder die verbrauchte Energie anzuzeigen. Die Einheiten pro Stunde und die verbrauchte Energie werden im Bereich angezeigt.

2. Zur Analyse der Einheiten pro Stunde oder des Energieverbrauchs für eine beliebige Ebene der Hierarchiedaten können Sie im Bereich **Key Performance Indicators** auf die entsprechende Anzeige klicken. Es wird ein Kontextbereich mit auf Time Series Insights basierenden Visualisierungen angezeigt, in dem Sie die Daten der letzten Stunde, letzten 24 Stunden und letzten sieben Tage aufrufen können.

## <a name="scenario-review"></a>Szenariorückblick

In diesem Szenario haben Sie die OEE- und KPI-Werte im Dashboard für Ihr Werk überwacht. Anschließend haben Sie Time Series Insights verwendet, um weitere Informationen bereitzustellen und einen Drilldown in die Telemetriedaten für die OEE- und KPI-Werte durchzuführen, um Anomalien erkennen zu können. Außerdem haben Sie den Bereich für Alarme verwendet, um für Ihre Werke bestehende Probleme anzuzeigen, und Sie haben die verfügbaren Aktionen genutzt, um den Alarm zu behandeln.

## <a name="other-features"></a>Andere Funktionen

In den folgenden Abschnitten werden einige Zusatzfeatures der Lösung für verbundene Factorys beschrieben, die im vorherigen Szenario nicht behandelt wurden.

## <a name="apply-filters"></a>Anwenden von Filtern

1. Klicken Sie auf den **Trichter**, um eine Liste mit verfügbaren Filtern im Bereich mit den Factorystandorten oder im Bereich mit den Alarmen anzuzeigen.

2. Der Filterbereich wird angezeigt.

    ![Filter des Solution Accelerators für verbundene Factorys][cf-img-alert-filter]

3. Wählen Sie den benötigten Filter aus. Bei Bedarf können Sie in die Filterfelder auch einen anderen Text eingeben.

4. Anschließend wird der Filter für Sie angewendet. Im Dashboard wird außerdem der Filterzustand über einen Trichter angegeben, der in den Tabellen mit den Werken und Alarmen angezeigt wird.

    ![Filter des Solution Accelerators für verbundene Factorys][cf-img-alert-filter-funnel]

    > [!NOTE]
    > Ein aktiver Filter hat keine Auswirkung auf die angezeigten OEE- und KPI-Werte, sondern dient nur zum Filtern des Listeninhalts.

5. Zum Löschen eines Filters klicken Sie auf den Trichter und dann im Bereich mit dem Filterkontext auf die Option zum Filtern. In den Tabellen mit den Werken und Alarmen wird dann der Text **Alle** angezeigt.

## <a name="browse-an-opc-ua-server"></a>Durchsuchen eines OPC UA-Servers

Beim Bereitstellen des Solution Accelerators stellen Sie automatisch simulierte OPC UA-Server bereit, die Sie mit dem Browser der Lösung durchsuchen können. Bei diesen Servern handelt es sich um *simulierte OPC UA-Server*. Simulierte Server erleichtern Ihnen das Experimentieren mit dem Solution Accelerator, ohne dass Sie echte physische Server bereitstellen müssen. Wenn Sie einen echten OPC UA-Server mit der Lösung verbinden möchten, finden Sie weitere Informationen im Tutorial [Bereitstellen eines Gateways unter Windows oder Linux für den Solution Accelerator für verbundene Factorys][lnk-connect-cf].

1. Klicken Sie auf der Navigationsleiste des Dashboards auf das **Browsersymbol**.

    ![Serverbrowser für den Solution Accelerator für verbundene Factorys][cf-img-server-browser]

2. Wählen Sie in der vorkonfigurierten Liste einen der aufgeführten Server aus. In dieser Liste werden die Server aufgeführt, die für Sie im Solution Accelerator bereitgestellt werden.

    ![Serverauswahl beim Solution Accelerator für verbundene Factorys][cf-img-server-choice]

3. Klicken Sie auf **Verbinden**. Ein Sicherheitsdialogfeld wird angezeigt. Es ist sicher, für die Simulation auf **Fortsetzen** zu klicken.

4. Klicken Sie zum Erweitern eines Knotens in der Serverstruktur auf den gewünschten Knoten. Knoten, die Telemetriedaten veröffentlichen, sind mit einem Häkchen gekennzeichnet.

    ![Serverstruktur des Solution Accelerators für verbundene Factorys][cf-img-server-tree]

5. Klicken Sie mit der rechten Maustaste auf ein Element, um Daten für den Knoten zu lesen, zu schreiben und zu veröffentlichen oder den Knoten aufzurufen. Es hängt von den Berechtigungen und Attributen des Knotens ab, welche Aktionen verfügbar sind. Bei der Option zum Lesen wird ein Kontextbereich mit dem Wert des jeweiligen Knotens angezeigt. Bei der Option zum Schreiben wird ein Kontextbereich angezeigt, in dem Sie einen neuen Wert eingeben können. Bei der Option zum Aufrufen wird ein Knoten angezeigt, über den Sie die Parameter für den Aufruf eingeben können.

## <a name="publish-a-node"></a>Veröffentlichen eines Knotens

Beim Durchsuchen eines *simulierten OPC UA-Servers* können Sie auch die Veröffentlichung neuer Knoten wählen. Sie können die Telemetrie dieser Knoten in der Lösung analysieren. Diese *simulierten OPC UA-Server* erleichtern das Experimentieren mit dem Solution Accelerator, ohne dass dafür echte physische Geräte bereitgestellt werden müssen.

1. Navigieren Sie zu einem Knoten in der Browserstruktur des OPC UA-Servers, den Sie veröffentlichen möchten.

2. Klicken Sie mit der rechten Maustaste auf den Knoten.

3. Wählen Sie **Veröffentlichen**.

    ![Veröffentlichung des Knotens durch die verbundene Factory][cf-img-publish-node]

4. Es wird ein Kontextbereich mit dem Hinweis angezeigt, dass die Veröffentlichung erfolgreich war. In der Stationsebenenansicht wird neben dem Knoten ein Häkchen angezeigt.

    ![Vorkonfigurierte Lösung für verbundene Factorys: Veröffentlichung erfolgreich][cf-img-publish-success]

## <a name="command-and-control"></a>Befehl und Steuerung

Mit verbundenen Factorys können Sie Ihre Industrieanlagen direkt über die Cloud steuern. Sie können dieses Feature nutzen, um auf Alarme zu reagieren, die von der Anlage generiert werden. Beispielsweise können Sie aus der Cloud einen Befehl an ein Gerät oder eine Anlage senden. Sie finden die verfügbaren Befehle in der Browserstruktur des OPC UA-Servers unter dem Knoten **StationCommands**. In diesem Szenario wird an einer Montagestation einer Fertigungsstraße in München ein Druckreduzierventil geöffnet. Zum Verwenden der Funktionen für Befehle und Steuerung müssen Sie für die Bereitstellung des Solution Accelerators über die Rolle **Administrator** verfügen.

1. Navigieren Sie in der Browserstruktur des OPC UA-Servers zum Knoten **StationCommands**.

2. Wählen Sie den Befehl, den Sie verwenden möchten.

3. Klicken Sie mit der rechten Maustaste auf den Knoten.

4. Wählen Sie die Option **Aufrufen**.

    ![Aufrufbefehl für den Solution Accelerator für verbundene Factorys][cf-img-call-command]

5. Ein Kontextbereich wird angezeigt. Er enthält Informationen dazu, welche Methode aufgerufen werden soll und welche Parameterdetails verfügbar sind.

6. Wählen Sie die Option **Aufrufen**.

    ![Aufrufkontext des Solution Accelerators für verbundene Factorys][cf-img-call-context]

7. Der Kontextbereich wird aktualisiert, um Sie darüber zu informieren, dass der Methodenaufruf erfolgreich war. Sie können überprüfen, ob der Aufruf erfolgreich war, indem Sie den Wert des Knotens für die Druckeinstellung ablesen, der als Folge des Aufrufs aktualisiert wurde.

    ![Aufruferfolg beim Solution Accelerator für verbundene Factorys][cf-img-call-success]

## <a name="behind-the-scenes"></a>Abläufe im Hintergrund

Wenn Sie einen Solution Accelerator bereitstellen, werden im Bereitstellungsprozess mehrere Ressourcen im gewählten Azure-Abonnement erstellt. Sie können diese Ressourcen im Azure-[Portal][lnk-portal] anzeigen. Der Bereitstellungsprozess erstellt eine **Ressourcengruppe** mit einem Namen basierend auf dem Namen, den Sie für Ihren Solution Accelerator wählen:

![Solution Accelerator im Azure-Portal][img-cf-portal]

Sie können die Einstellungen der einzelnen Ressourcen anzeigen, indem Sie sie in der Ressourcengruppe in der Liste mit den Ressourcen auswählen.

Sie können auch den Quellcode für den Solution Accelerator anzeigen. Den Quellcode des Solution Accelerators für verbundene Factorys finden Sie im GitHub-Repository [azure-iot-connected-factory][lnk-cfgithub]:

Wenn Sie fertig sind, können Sie den Solution Accelerator auf der Website [azureiotsuite.com][lnk-azureiotsuite] aus Ihrem Azure-Abonnement löschen. Über diese Website können Sie problemlos alle Ressourcen löschen, die beim Erstellen des Solution Accelerators bereitgestellt wurden.

> [!NOTE]
> Um sicherzustellen, dass der Solution Accelerator vollständig gelöscht wird, sollten Sie den Löschvorgang auf der Website [azureiotsuite.com][lnk-azureiotsuite] durchführen. Löschen Sie nicht die Ressourcengruppe im Portal.

## <a name="next-steps"></a>Nächste Schritte

Sie haben einen funktionierenden Solution Accelerator bereitgestellt und können nun mit den ersten Schritten mit IoT-Solution Accelerators fortfahren. Lesen Sie dazu die folgenden Artikel:

* [Exemplarische Vorgehensweise für den Solution Accelerator für verbundene Factorys][lnk-rm-walkthrough]
* [Verbinden Ihres Geräts mit dem Solution Accelerator für verbundene Factorys][lnk-connect-cf]
* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-solution-accelerators.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-faq]: iot-suite-v1-faq.md