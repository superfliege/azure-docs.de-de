---
title: Aktionsregeln für Azure Monitor-Warnungen
description: In diesem Artikel wird erläutert, was Aktionsregeln sind und wie sie konfiguriert und verwaltet werden können.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: e5d04fd136848684e866fae9768b252e3b6ca77f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65137935"
---
# <a name="action-rules-preview"></a>Aktionsregeln (Vorschau)

In diesem Artikel wird erläutert, was Aktionsregeln sind und wie sie konfiguriert und verwaltet werden können.

## <a name="what-are-action-rules"></a>Was sind Aktionsregeln?

Mit Aktionsregeln können Sie Aktionen (oder die Unterdrückung von Aktionen) in jedem Resource Manager-Bereich (Abonnement, Ressourcengruppe und Ressource) definieren. Sie verfügen über eine Reihe von Filtern zum Eingrenzen auf eine bestimmte Teilmenge von Warnungsinstanzen, auf die Sie reagieren möchten. 

Mit Aktionsregeln können Sie folgende Vorgänge ausführen:

* Unterdrücken von Aktionen und Benachrichtigungen für geplante Wartungszeiträume oder für Wochenenden/Feiertage, sodass Sie nicht jede einzelne Warnungsregel einzeln deaktivieren müssen.
* Definieren von Aktionen und Benachrichtigungen im benötigten Umfang: Anstatt eine Aktionsgruppe einzeln für jede Warnungsregel zu definieren, können Sie nun eine Aktionsgruppe definieren, die für Warnungen ausgelöst wird, die in einem beliebigen Bereich generiert wurden. Sie können beispielsweise angeben, dass die Aktionsgruppe „ContosoActionGroup“ für jede Warnung ausgelöst wird, die in Ihrem Abonnement generiert wurde.

## <a name="configuring-an-action-rule"></a>Konfigurieren einer Aktionsregel

Sie können die Funktion aufrufen, indem Sie in Azure Monitor auf der Startseite „Warnungen“die Option **Aktionen verwalten** auswählen. Wählen Sie anschließend **Action Rules (Preview)** (Aktionsregeln (Vorschau)) aus. Sie können durch Auswählen von **Action Rules (preview)** (Aktionsregeln (Vorschau)) im Dashboard auf der Startseite für Warnungen darauf zugreifen.

![Aktionsregeln auf der Azure Monitor-Startseite](media/alerts-action-rules/action-rules-landing-page.png)

Wählen Sie **+ New Action Rule** (+ Neue Aktionsregel) aus. 

![Hinzufügen einer neuen Aktionsregel](media/alerts-action-rules/action-rules-new-rule.png)

Sie können eine Aktionsregel auch erstellen, während Sie eine Warnungsregel konfigurieren.

![Hinzufügen einer neuen Aktionsregel](media/alerts-action-rules/action-rules-alert-rule.png)

Der Erstellungsflow für Aktionsregeln wird geöffnet. Konfigurieren Sie die folgenden Elemente: 

![Erstellungsflow für neue Aktionsregel](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>`Scope`

Wählen Sie zuerst den Bereich aus, d. h. Zielressource, Ressourcengruppe oder Abonnement. Sie können auch mehrere der obigen Optionen auswählen (innerhalb eines einzelnen Abonnements). 

![Bereich der Aktionsregel](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filterkriterien

Sie können außerdem einen oder mehrere Filter zum weiteren Eingrenzen auf eine bestimmte Teilmenge von Warnungen im definierten Bereich festlegen. 

Die folgenden Filter sind verfügbar: 

* **Schweregrad**: Wählen Sie einen oder mehrere Schweregrade für Warnungen aus. Schweregrad = Sev1 bedeutet, dass die Aktionsregel für alle Warnungen mit dem Schweregrad Sev1 gilt.
* **Überwachungsdienst**: Filter, der auf dem ursprünglichen Überwachungsdienst basiert. Auch hier ist die Mehrfachauswahl möglich. Überwachungsdienst = „Application Insights“ bedeutet, dass die Aktionsregel für alle auf „Application Insights“ basierenden Warnungen gilt.
* **Ressourcentyp**: Filter, der auf einem bestimmten Ressourcentyp basiert. Auch hier ist die Mehrfachauswahl möglich. Ressourcentyp = „Virtual Machines“ bedeutet, dass die Aktionsregel für alle Virtual Machines gilt.
* **Warnungsregel-ID**: Ermöglicht Ihnen das Filtern nach bestimmten Warnungsregeln anhand der Resource Manager-ID der Warnungsregel.
* **Überwachungsbedingung**: Hiermit können Sie nach Warnungsinstanzen mit der Überwachungsbedingung „Ausgelöst“ oder „Aufgelöst“ filtern.
* **Beschreibung:** Abgleich von regulären Ausdrücken in der Beschreibung, die als Teil der Warnungsregel definiert wurde.
* **Warnungskontext (Nutzlast)**: Abgleich von regulären Ausdrücken in den [Warnungskontext](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields)-Feldern einer Warnungsinstanz.

Diese Filter werden in Verbindung miteinander angewendet. Wenn Sie beispielsweise „Ressourcentyp' = „Virtual Machines“ und „Schweregrad“ = „Sev0“ festlegen, wird nach allen Sev0-Warnungen ausschließlich auf Ihren VMs gefiltert. 

![Filter für Aktionsregeln](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Konfiguration von Unterdrückung oder Aktionsgruppen

Konfigurieren Sie nun die Aktionsregel für die Unterdrückung von Warnungen oder die Unterstützung für Aktionsgruppen. Es können nicht beide ausgewählt werden. Die Konfiguration gilt für alle Warnungsinstanzen, die dem zuvor definierten Bereich und den Filtern entsprechen.

#### <a name="suppression"></a>Unterdrückung

Wenn Sie **Suppression** (Unterdrückung) auswählen, konfigurieren Sie den Zeitraum, für den Aktionen und Benachrichtigungen unterdrückt werden sollen. Wählen Sie eine der folgenden Optionen aus:
* **From now (always)** (Ab jetzt (immer)): Alle Benachrichtigungen werden unbefristet unterdrückt.
* **At a scheduled time** (Zur geplanten Zeit): Benachrichtigungen werden in einem bestimmten Zeitraum unterdrückt.
* **With a recurrence** (Mit Wiederholung): Benachrichtigungen werden in einem geplanten Wiederholungsintervall (täglich, wöchentlich oder monatlich) unterdrückt.

![Unterdrückung von Aktionsregeln](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Aktionsgruppe

Fügen Sie bei Auswahl von **Aktionsgruppe** auf der Umschaltfläche eine vorhandene Aktionsgruppe hinzu, oder erstellen Sie eine neue. 

> [!NOTE]
> Sie können einer Aktionsregel nur eine Aktionsgruppe zuordnen.

![Aktionsgruppe für Aktionsregel](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Details der Aktionsregel

Konfigurieren Sie schließlich folgende Details für die Aktionsregel.
* NAME
* Ressourcengruppe, in der sie gespeichert wird
* BESCHREIBUNG 

## <a name="example-scenarios"></a>Beispielszenarien

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Szenario 1: Unterdrückung von Warnungen nach Schweregrad

Contoso möchte Benachrichtigungen für alle Warnungen des Schweregrads Sev4 auf allen VMs im Abonnement „ContosoSub“ an Wochenenden unterdrücken.

**Lösung:** Erstellen Sie eine Aktionsregel mit
* Bereich = „ContosoSub“
* Filter
    * Schweregrad = „Sev4“
    * Ressourcentyp = „Virtual Machines“
* Unterdrückung, deren Wiederholung auf wöchentlich festgelegt ist, und „Samstag“ und „Sonntag“ sind ausgewählt

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Szenario 2: Unterdrückung von Warnungen nach Warnungskontext (Nutzlast)

Contoso möchte Benachrichtigungen für alle Protokollwarnungen unterdrücken, die für „Computer-01“ in „ContosoSub“ unbefristet unterdrücken, während Wartungsarbeiten ausgeführt werden.

**Lösung:** Erstellen Sie eine Aktionsregel mit
* Bereich = „ContosoSub“
* Filter
    * Überwachungsdienst = „Log Analytics“
    * Warnungskontext (Nutzlast) enthält „Computer-01“
* Unterdrückung ist auf „From now (Always)“ (Ab jetzt (immer)) festgelegt

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Szenario 3: In einer Ressourcengruppe definierte Aktionsgruppe

Contoso hat eine [Metrikwarnung auf Abonnementebene](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor) definiert, möchte jedoch die für Warnungen ausgelösten Aktionen für die Ressourcengruppe „ContosoRG“ separat definieren.

**Lösung:** Erstellen Sie eine Aktionsregel mit
* Bereich = „ContosoRG“
* Keine Filter
* Aktionsgruppe auf „ContosoActionGroup“ festgelegt

## <a name="managing-your-action-rules"></a>Verwalten Ihrer Aktionsregeln

Sie können Ihre Aktionsregeln in der Listenansicht anzeigen und verwalten, wie unten dargestellt.

![Listenansicht für Aktionsregeln](media/alerts-action-rules/action-rules-list-view.png)

Hier können Sie Aktionsregeln je nach Bedarf aktivieren, deaktivieren und löschen, indem Sie das jeweilige Kontrollkästchen daneben aktivieren. Wenn Sie auf eine Aktionsregel klicken, wird die zugehörige Konfigurationsseite geöffnet, in der Sie ihre Definition aktualisieren und die Aktionsregel aktivieren bzw. deaktivieren können.

## <a name="best-practices"></a>Bewährte Methoden

Protokollwarnungen, die mit der Option [„Anzahl von Ergebnissen“](https://docs.microsoft.com/azure-monitor/platform/alerts-unified-log) erstellt werden, generieren eine **einzige Warnungsinstanz** anhand sämtlicher Suchergebnisse (die sich z.B. über mehrere Computer erstrecken können). Wenn in diesem Szenario eine Aktionsregel den Filter „Warnungskontext (Nutzlast)“ verwendet, reagiert sie auf die Warnungsinstanz, sofern eine Übereinstimmung vorhanden ist. In Szenario 2, wie bereits beschrieben, gilt Folgendes: Wenn die Suchergebnisse für die generierte Protokollwarnung sowohl „Computer-01“ als auch „Computer-02“ enthalten, wird die gesamte Benachrichtigung unterdrückt (d.h., es wird überhaupt keine Benachrichtigung für „Computer-02“ generiert).

![Aktionsregeln und Protokollwarnungen (Anzahl von Ergebnissen)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

Um Protokollwarnungen mit Aktionsregeln optimal zu nutzen, empfiehlt es sich, Protokollwarnungen mit der Optionen [„Metrische Maßeinheit“](https://docs.microsoft.com/azure-monitor/platform/alerts-unified-log) zu erstellen. Mit dieser Option werden entsprechend dem definierten Gruppenfeld separate Warnungsinstanzen generiert. In Szenario 2 werden schließlich separate Warnungsinstanzen für „Computer-01“ und „Computer-02“ generiert. Mi der im Szenario beschriebenen Aktionsregel wird nur die Benachrichtigung für „Computer-01“ unterdrückt, während die Benachrichtigung für „Computer-02“ weiterhin normal ausgelöst wird.

![Aktionsregeln und Protokollwarnungen (Anzahl von Ergebnissen)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>Häufig gestellte Fragen

* F: Beim Konfigurieren einer Aktionsregel möchte ich alle möglichen einander überlappenden Aktionsregeln sehen, sodass ich doppelte Benachrichtigungen vermeiden kann. Ist dies möglich?

    A. Wen Sie beim Konfigurieren einer Aktionsregel einen Bereich definieren, wird eine Liste von Aktionsregeln angezeigt, die sich im betreffenden Bereich überlappen (falls zutreffend). Diese Überlappung kann wie folgt aussehen:
    * Genaue Übereinstimmung: Beispielsweise können sich die von Ihnen definierte Aktionsregel und die überlappende Aktionsregel auf dasselbe Abonnement beziehen.
    * Teilmenge: Die von Ihnen definierte Aktionsregel bezieht sich z.B. auf ein Abonnement, und die überlappende Aktionsregel bezieht sich auf eine Ressourcengruppe in diesem Abonnement.
    * Obermenge: Die von Ihnen definierte Aktionsregel bezieht sich auf eine Ressourcengruppe, und die überlappende Aktionsregel bezieht sich auf das Abonnement, das diese Ressourcengruppe enthält.
    * Schnittmenge: Die von Ihnen definierte Aktionsregel bezieht sich z.B. auf „VM1“ und „VM2“, und die überlappende Aktionsregel bezieht sich auf „VM2“ und „VM3“.

    ![Überlappende Aktionsregeln](media/alerts-action-rules/action-rules-overlapping.png)

* F: Kann ich beim Konfigurieren einer Warnungsregel feststellen, ob bereits Aktionsregeln definiert sind, die möglicherweise Aktionen für die Warnungsregel ausführen, die ich gerade definiere?

    A. Wenn Sie die Zielressource für Ihre Warnungsregel definieren, können Sie die Liste von Aktionsregeln anzeigen, die Aktionen für denselben Bereich ausführen (sofern zutreffend). Klicken Sie dazu im Abschnitt „Aktionen“ auf „View configured actions“ (Konfigurierte Aktionen anzeigen). Diese Liste wird gemäß den folgenden Szenarien für den Bereich aufgefüllt:
    * Genaue Übereinstimmung: Beispielsweise können sich die von Ihnen definierte Warnungsregel und die Aktionsregel auf dasselbe Abonnement beziehen.
    * Teilmenge: Die von Ihnen definierte Warnungsregel bezieht sich auf ein Abonnement, und die Aktionsregel bezieht sich auf eine Ressourcengruppe in diesem Abonnement.
    * Obermenge: Die von Ihnen definierte Warnungsregel bezieht sich auf eine Ressourcengruppe, und die Aktionsregel bezieht sich auf das Abonnement, das die Ressourcengruppe enthält.
    * Schnittmenge: Die von Ihnen definierte Warnungsregel bezieht sich beispielsweise auf „VM1“ und „VM2“, und die Aktionsregel bezieht sich auf „VM2“ und „VM3“.
    
    ![Überlappende Aktionsregeln](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

* F: Kann ich die durch eine Aktionsregel unterdrückten Warnungen anzeigen?

    A. Auf der [Seite mit der Liste der Warnungen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances) kann eine weitere Spalte mit dem Namen „Suppression Status“ (Unterdrückungsstatus) ausgewählt werden. Wenn die Benachrichtigung für eine Warnungsinstanz unterdrückt wurde, wird dieser Status in der Liste angezeigt.

    ![Unterdrückte Warnungsinstanzen](media/alerts-action-rules/action-rules-suppressed-alerts.png)

* F: Was geschieht, wenn eine Aktionsregel mit einer Aktionsgruppe und eine weitere mit aktiver Unterdrückung im selben Bereich vorhanden sind?

    A. **Unterdrückung hat in einem Bereich immer Vorrang.**

* F: Was geschieht, wenn ich eine Ressource von zwei separaten Aktionsregeln überwachen lasse? Erhalte ich dann eine oder zwei Benachrichtigungen? Ein Beispiel ist „VM2“ in diesem Szenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1'
      action rule 'AR2' defined for 'VM2' and 'VM3' with action group 'AG1'

    A. Für jede Warnung zu „VM1“ und „VM3“ wird Aktionsgruppe „AG1“ einmal ausgelöst. Für jede Warnung zu „VM2“ wird Aktionsgruppe „AG1“ zweimal ausgelöst (**Aktionen werden von Aktionsregeln nicht dedupliziert**). 

* F: Was geschieht, wenn ich eine Ressource in zwei separaten Aktionsregeln überwachen lasse, von denen eine eine Aktion ausführt und die andere unterdrückt wird? Ein Beispiel ist „VM2“ in diesem Szenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1' 
      action rule 'AR2' defined for 'VM2' and 'VM3' with suppression

    A. Für jede Warnung zu „VM1“ wird Aktionsgruppe „AG1“ einmal ausgelöst. Aktionen und Benachrichtigungen für jede Warnung zu „VM2“ und „VM3“ werden unterdrückt. 

* F: Was geschieht, wenn ich eine Warnungsregel und eine Aktionsregel für dieselbe Ressource definiert habe, die unterschiedliche Aktionsgruppen aufrufen? Ein Beispiel ist „VM1“ in diesem Szenario:

      alert rule  'rule1' on          'VM1' with action group 'AG2'
      action rule 'AR1'   defined for 'VM1' with action group 'AG1' 
 
    A. Für jede Warnung zu „VM1“ wird Aktionsgruppe „AG1“ einmal ausgelöst. Wird Warnungsregel „rule1“ ausgelöst, wird zusätzlich auch „AG2“ ausgelöst. **In Aktionsregeln definierte Aktionsgruppen und Warnungsregeln arbeiten unabhängig voneinander, und es erfolgt keine Deduplizierung**. 

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Warnungen in Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)
