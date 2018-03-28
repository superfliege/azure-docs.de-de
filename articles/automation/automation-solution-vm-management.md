---
title: Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten (Vorschauversion)
description: Mit dieser Lösung für die VM-Verwaltung werden Ihre virtuellen Azure Resource Manager-Computer nach einem Zeitplan gestartet und beendet und mit Log Analytics proaktiv überwacht.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: ec15859a92527c4e084075b40d3439d7a19fea1a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="startstop-vms-during-off-hours-solution-preview-in-azure-automation"></a>Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten in Azure Automation (Vorschauversion)

Mit der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten können Sie Ihre virtuellen Azure-Computer nach benutzerdefinierten Zeitplänen starten und beenden. Zudem erhalten Sie über Azure Log Analytics Einblick in Ihre Daten und können durch die Nutzung von [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview) optional E-Mails senden. Die Lösung unterstützt in den meisten Szenarien sowohl Azure Resource Manager-VMs als auch klassische VMs. 

Diese Lösung umfasst eine dezentrale Automatisierungsoption für Benutzer, die durch die Verwendung serverloser und kostengünstiger Ressourcen ihre Kosten reduzieren möchten. Mit dieser Lösung haben Sie folgende Möglichkeiten:

* Starten und Beenden von VMs nach Zeitplan
* Starten und Beenden von VMs nach Zeitplan in aufsteigender Reihenfolge mithilfe von Azure-Tags (wird für klassische VMs nicht unterstützt)
* Automatisches Beenden von VMs bei geringer CPU-Auslastung

## <a name="prerequisites"></a>Voraussetzungen

- Für die Runbooks wird ein [Ausführendes Azure-Konto](automation-offering-get-started.md#authentication-methods) verwendet.  Das ausführende Konto ist die bevorzugte Authentifizierungsmethode, da anstelle eines Kennworts, das ablaufen oder sich häufig ändern kann, eine Zertifikatauthentifizierung verwendet wird.  

- Mit dieser Lösung werden nur VMs verwaltet, die sich unter demselben Abonnement wie Ihr Azure Automation-Konto befinden.  

- Diese Lösung wird nur in den folgenden Azure-Regionen bereitgestellt: „Australien, Südosten“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Osten“, „Japan, Osten“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“ und „Europa, Westen“.  
    
    > [!NOTE]
    > Die Runbooks zum Verwalten des VM-Zeitplans können für VMs in jeder beliebigen Region verwendet werden.  

- Um beim Onboarding über den Azure Marketplace festzulegen, dass E-Mail-Benachrichtigungen gesendet werden, wenn die Runbooks zum Starten und Beenden von VMs abgeschlossen sind, wählen Sie **Ja**. Dies führt dazu, dass SendGrid bereitgestellt wird. 

    > [!IMPORTANT]
    > SendGrid ist ein Drittanbieterdienst. Support erhalten Sie unter [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Für SendGrid gelten die folgenden Einschränkungen:

    * Maximal ein SendGrid-Konto pro Benutzer pro Abonnement
    * Maximal zwei SendGrid-Konten pro Abonnement

Wenn Sie eine frühere Version dieser Lösung bereitgestellt haben, müssen Sie sie vor der Bereitstellung dieses Release zunächst aus Ihrem Konto löschen.  

## <a name="solution-components"></a>Lösungskomponenten

Diese Lösung umfasst vorkonfigurierte Runbooks, Zeitpläne und die Integration in Log Analytics, sodass Sie das Starten und Beenden der virtuellen Computer gemäß Ihren Geschäftsanforderungen anpassen können. 

### <a name="runbooks"></a>Runbooks

In der folgenden Tabelle sind die in Ihrem Automation-Konto bereitgestellten Runbooks aufgeführt.  Es ist nicht ratsam, am Runbookcode Änderungen vorzunehmen. Schreiben Sie stattdessen Ihr eigenes Runbook, um neue Funktionen nutzen zu können.

> [!IMPORTANT]
> Führen Sie keine Runbooks aus, an deren Name der Zusatz „child“ angehängt ist.
>

Alle übergeordneten Runbooks enthalten den Parameter *WhatIf*. Bei der Festlegung auf **True** unterstützt *WhatIf* die Beschreibung des exakten Verhaltens des Runbooks, wenn es ohne den Parameter *WhatIf* ausgeführt wird, und es wird überprüft, ob die richtigen VMs als Ziel verwendet werden.  Ein Runbook führt seine zugehörigen definierten Aktionen nur aus, wenn der Parameter *WhatIf* auf **False** festgelegt ist. 

|**Runbook** | **Parameter** | **Beschreibung**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Wird nur über das übergeordnete Runbook aufgerufen. Erstellt für das AutoStop-Szenario Warnungen pro Ressource.| 
|AutoStop_CreateAlert_Parent | WhatIf: TRUE oder FALSE <br> VMList | Erstellt oder aktualisiert Azure-Warnungsregeln auf VMs im Zielabonnement oder den Zielressourcengruppen. <br> VMList: Durch Kommas getrennte Liste mit VMs.  Beispiel: *vm1,vm2,vm3*.| 
|AutoStop_Disable | none | Deaktiviert AutoStop-Warnungen und den Standardzeitplan.| 
|AutoStop_StopVM_Child | WebHookData | Wird nur über das übergeordnete Runbook aufgerufen. Warnungsregeln rufen dieses Runbook auf, um den virtuellen Computer zu beenden.|  
|Bootstrap_Main | none | Wird einmal zum Einrichten von Bootstrapkonfigurationen wie webhookURI verwendet, auf die normalerweise nicht über Azure Resource Manager zugegriffen werden kann. Dieses Runbook wird nach der erfolgreichen Bereitstellung automatisch entfernt.|  
|ScheduledStartStop_Child | VMName <br> Aktion: Beenden oder Starten <br> ResourceGroupName | Wird nur über das übergeordnete Runbook aufgerufen. Führt für den geplanten Beendigungsvorgang das Beenden bzw. Starten aus.|  
|ScheduledStartStop_Parent | Aktion: Beenden oder Starten <br> WhatIf: TRUE oder FALSE | Dies wirkt sich auf alle virtuellen Computer des Abonnements aus. Bearbeiten Sie **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupNames**, damit die Ausführung nur für diese Zielressourcengruppen erfolgt. Sie können zudem bestimmte VMs ausschließen, indem Sie die Variable **External_ExcludeVMNames** aktualisieren. *WhatIf* verhält sich wie in den anderen Runbooks.|  
|SequencedStartStop_Parent | Aktion: Beenden oder Starten <br> WhatIf: TRUE oder FALSE | Erstellen Sie auf jeder VM, für die Sie die Aktivität zum Starten/Beenden verwenden möchten, Tags mit den Namen **SequenceStart** und **SequenceStop**. Der Wert des Tags muss eine positive ganze Zahl (1, 2, 3) sein, die der Reihenfolge entspricht, in der das Starten oder Beenden durchgeführt werden soll. *WhatIf* verhält sich wie in den anderen Runbooks. <br> **Hinweis:** VMs müssen sich in Ressourcengruppen befinden, die in Azure Automation-Variablen als „External_Start_ResourceGroupNames“, „External_Stop_ResourceGroupNames“ und „External_ExcludeVMNames“ definiert sind. Diese müssen über die entsprechenden Tags verfügen, damit Aktionen wirksam werden.|

### <a name="variables"></a>Variables

In der folgenden Tabelle sind die in Ihrem Automation-Konto erstellten Variablen aufgeführt.  Sie sollten nur Variablen ändern, die über das Präfix **External** verfügen. Wenn Sie Variablen mit dem Präfix **Internal** ändern, hat dies unerwünschte Auswirkungen.  

|**Variable** | **Beschreibung**|
---------|------------|
|External_AutoStop_Condition | Der bedingte Operator, der zum Konfigurieren der Bedingung erforderlich ist, bevor eine Warnung ausgelöst wird. Zulässige Werte sind **GreaterThan**, **GreaterThanOrEqual**, **LessThan** und **LessThanOrEqual**.|  
|External_AutoStop_Description | Die Warnung zum Beenden des virtuellen Computers, wenn der CPU-Prozentsatz den Schwellenwert überschreitet.|  
|External_AutoStop_MetricName | Der Name der Leistungsmetrik, für die die Azure-Warnungsregel konfiguriert werden muss.| 
|External_AutoStop_Threshold | Der Schwellenwert für die Azure-Warnungsregel, der in der Variablen *External_AutoStop_MetricName* angegeben ist. Prozentwerte können zwischen 1 und 100 liegen.|  
|External_AutoStop_TimeAggregationOperator | Der Zeitaggregationsoperator, der auf die ausgewählte Fenstergröße angewendet wird, um die Bedingung auszuwerten. Zulässige Werte sind **Average**, **Minimum**, **Maximum**, **Total** und **Last**.|  
|External_AutoStop_TimeWindow | Die Größe des Fensters, in dem Azure ausgewählte Metriken zum Auslösen einer Warnung analysiert. Für diesen Parameter können Zeiträume eingegeben werden. Mögliche Werte reichen von 5 Minuten bis 6 Stunden.|  
|External_EmailFromAddress | Gibt den Absender der E-Mail an.|  
|External_EmailSubject | Gibt den Text für die Betreffzeile der E-Mail an.|  
|External_EmailToAddress | Gibt die Empfänger der E-Mail an. Trennen Sie die Namen durch Kommas.|  
|External_ExcludeVMNames | Geben Sie die Namen der auszuschließenden VMs ein, und trennen Sie Namen durch Kommas ohne Leerzeichen.|  
|External_IsSendEmail | Gibt die Option zum Senden einer E-Mail-Benachrichtigung nach Abschluss des Vorgangs an.  Geben Sie **Yes** oder **No** an, wenn keine E-Mails gesendet werden sollen.  Wählen Sie für diese Option **No**, wenn Sie während der ersten Bereitstellung keine E-Mail-Benachrichtigungen aktiviert haben.|  
|External_Start_ResourceGroupNames | Gibt eine oder mehrere Zielressourcengruppen, deren Werte durch Kommas getrennt sind, für Startaktionen an.|  
|External_Stop_ResourceGroupNames | Gibt eine oder mehrere Zielressourcengruppen, deren Werte durch Kommas getrennt sind, für Beendigungsaktionen an.|  
|Internal_AutomationAccountName | Gibt den Namen des Automation-Kontos an.|  
|Internal_AutoSnooze_WebhookUri | Gibt den Webhook-URI an, der für das AutoStop-Szenario aufgerufen wird.|  
|Internal_AzureSubscriptionId | Gibt die Azure-Abonnement-ID an.|  
|Internal_ResourceGroupName | Gibt den Ressourcengruppennamen des Automation-Kontos an.|  
|Internal_SendGridAccountName | Gibt den Namen des SendGrid-Kontos an.|  
|Internal_SendGridPassword | Gibt das Kennwort des SendGrid-Kontos an.|  

<br>

In allen Szenarien müssen die Variablen **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** und **External_ExcludeVMNames** für die Ziel-VMs angegeben werden, mit Ausnahme der Angabe einer durch Kommas getrennten Liste von VMs für das Runbook **AutoStop_CreateAlert_Parent**. Dies bedeutet, dass sich die VMs in Zielressourcengruppen befinden müssen, damit Aktionen zum Starten und Beenden durchgeführt werden. Die Logik ähnelt der von Azure-Richtlinien, da Sie das Abonnement oder die Ressourcengruppe als Ziel verwenden können und neu erstellte VMs Aktionen erben. Hierdurch wird vermieden, dass für jeden virtuellen Computer jeweils ein separater Zeitplan gepflegt werden muss und dass Aktionen zum Starten und Beenden für die Skalierung verwaltet werden müssen.

### <a name="schedules"></a>Zeitpläne

In der folgenden Tabelle sind die einzelnen in Ihrem Automation-Konto erstellten Standardzeitpläne aufgeführt.  Sie können sie ändern oder Ihre eigenen benutzerdefinierten Zeitpläne erstellen.  Standardmäßig sind alle deaktiviert, mit Ausnahme von **Scheduled_StartVM** und **Scheduled_StopVM**.

Es ist nicht ratsam, alle Zeitpläne zu aktivieren, da dies zu sich überlappenden Zeitplanaktionen führen kann. Am besten ermitteln Sie, welche Optimierungen Sie ausführen möchten, und nehmen dann die entsprechenden Änderungen vor.  Weitere Erläuterungen finden Sie in den Beispielszenerien im Übersichtsabschnitt.   

|**Zeitplanname** | **Frequency** | **Beschreibung**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Alle 8 Stunden | Führt das Runbook „AutoStop_CreateAlert_Parent“ alle acht Stunden aus. Hiermit werden dann die VM-basierten Werte unter „External_Start_ResourceGroupNames“, „External_Stop_ResourceGroupNames“ und „External_ExcludeVMNames“ in Azure Automation-Variablen beendet.  Alternativ hierzu können Sie mithilfe des Parameters „VMList“ eine durch Kommas getrennte Liste mit VMs angeben.|  
|Scheduled_StopVM | Benutzerdefiniert, täglich | Führt das Runbook „Scheduled_Parent“ mit dem Parameter *Stop* jeden Tag zum angegebenen Zeitpunkt aus.  Beendet automatisch alle VMs, für die die Regeln der Ressourcenvariablen erfüllt werden. Es ist ratsam, den dazugehörigen Zeitplan zu aktivieren (**Scheduled-StartVM**).|  
|Scheduled_StartVM | Benutzerdefiniert, täglich | Führt das Runbook „Scheduled_Parent“ mit dem Parameter *Start* jeden Tag zum angegebenen Zeitpunkt aus.  Startet automatisch alle VMs, die die Regeln der entsprechenden Variablen erfüllen.  Es ist ratsam, den dazugehörigen Zeitplan zu aktivieren (**Scheduled-StopVM**).|
|Sequenced-StopVM | 01:00 Uhr (UTC), jeden Freitag | Führt das Runbook „Sequenced_Parent“ mit dem Parameter *Stop* jeden Freitag zum angegebenen Zeitpunkt aus.  Beendet der Reihe nach (in aufsteigender Reihenfolge) alle VMs mit dem in den jeweiligen Variablen definierten Tag **SequenceStop**.  Weitere Informationen zu Tagwerten und Ressourcenvariablen finden Sie im Abschnitt „Runbooks“.  Es ist ratsam, den dazugehörigen Zeitplan zu aktivieren (**Sequenced-StartVM**).|
|Sequenced-StartVM | 13:00 Uhr (UTC), jeden Montag | Führt das Runbook „Sequenced_Parent“ mit dem Parameter *Start* jeden Montag zum angegebenen Zeitpunkt aus. Startet der Reihe nach (in absteigender Reihenfolge) alle VMs mit dem in den jeweiligen Variablen definierten Tag **SequenceStart**.  Weitere Informationen zu Tagwerten und Ressourcenvariablen finden Sie im Abschnitt „Runbooks“.  Es ist ratsam, den dazugehörigen Zeitplan zu aktivieren (**Sequenced-StopVM**).|

<br>

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Lösung zum Starten/Beenden von VMs außerhalb der Kernzeit dem Automation-Konto hinzuzufügen und anschließend die Variablen zum Anpassen der Lösung zu konfigurieren.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.<br> ![Azure-Portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. Geben Sie im Bereich „Marketplace“ ein Schlüsselwort ein, z.B. **Starten** oder **Starten/Beenden**. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Alternativ hierzu können Sie ein oder mehrere Schlüsselwörter des vollständigen Namens der Lösung eingeben und dann die EINGABETASTE drücken.  Wählen Sie **VMs außerhalb der Geschäftszeiten starten/beenden [Vorschau]** in den Suchergebnissen aus.  
3. Sehen Sie sich im Bereich **VMs außerhalb der Geschäftszeiten starten/beenden [Vorschau]** für die ausgewählte Lösung die Zusammenfassung an, und klicken Sie dann auf **Erstellen**.  
4. Der Bereich **Lösung hinzufügen** wird angezeigt. Sie werden aufgefordert, die Lösung vor dem Importieren in Ihr Automation-Abonnement zu konfigurieren.<br><br> ![Blatt „Lösung hinzufügen“ der VM-Verwaltung](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Wählen Sie im Bereich **Lösung hinzufügen** die Option **Arbeitsbereich**. Wählen Sie einen OMS-Arbeitsbereich aus, der mit demselben Azure-Abonnement verknüpft ist, unter dem sich das Automation-Konto befindet. Wählen Sie die Option **Neuen Arbeitsbereich erstellen**, wenn kein Arbeitsbereich vorhanden ist. Führen Sie im Bereich **OMS-Arbeitsbereich** die folgenden Schritte aus: 
   - Geben Sie einen Namen für den neuen **OMS-Arbeitsbereich** an.
   - Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
   - Unter **Ressourcengruppe** können Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen.  
   - Wählen Sie einen **Speicherort**aus.  Derzeit sind nur die Standorte **Australien, Südosten**, **Kanada, Mitte**, **Indien, Mitte**, **USA, Osten**, **Japan, Osten**, **Asien, Südosten**, **Vereinigtes Königreich, Süden** und **Europa, Westen** verfügbar.
   - Wählen Sie einen **Tarif**aus.  Die Lösung ermöglicht zwei OMS-Tarife: kostenlos (Free) und kostenpflichtig.  Beim Free-Tarif gelten Beschränkungen für die Datenmenge, die pro Tag erfasst werden kann, die Aufbewahrungsdauer und die Laufzeit von Runbookaufträgen in Minuten.  Für den kostenpflichtigen OMS-Tarif gilt keine Beschränkung für die täglich erfasste Datenmenge.  

        > [!NOTE]
        > Der kostenpflichtige Tarif pro GB (Standalone) wird zwar als Option angezeigt, ist aber nicht verfügbar.  Wenn Sie diesen Tarif auswählen und mit der Erstellung der Lösung in Ihrem Abonnement fortfahren, tritt ein Fehler auf.  Dies wird behoben sein, wenn die Lösung offiziell veröffentlicht wird.<br>Bei dieser Lösung werden nur Automation-Auftragsminuten und die Protokollerfassung genutzt.  Ihrer Umgebung werden keine weiteren OMS-Knoten hinzugefügt.  

6. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen im Bereich **OMS-Arbeitsbereich** auf **Erstellen**.  Sie können den Status unter **Benachrichtigungen** über das Menü nachverfolgen, und nach Abschluss des Vorgangs gelangen Sie zurück zum Bereich **Lösung hinzufügen**.  
7. Wählen Sie im Bereich **Lösung hinzufügen** die Option **Automation-Konto**.  Wenn Sie einen neuen OMS-Arbeitsbereich erstellen, müssen Sie auch ein dazugehöriges neues Automation-Konto erstellen.  Wählen Sie die Option **Automation-Konto erstellen**, und geben Sie im Bereich **Automation-Konto hinzufügen** Folgendes an: 
  - Geben Sie im Feld **Name** den Namen des Automation-Kontos ein.

    Alle anderen Optionen werden basierend auf dem OMS-Arbeitsbereich automatisch aufgefüllt. Diese Optionen können nicht geändert werden.  Ein ausführendes Azure-Konto ist die Standardmethode für die Authentifizierung von Runbooks in dieser Lösung.  Nach dem Klicken auf **OK** werden die Konfigurationsoptionen überprüft, und das Automation-Konto wird erstellt.  Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

    Andernfalls können Sie auch ein vorhandenes ausführendes Automation-Konto wählen.  Beachten Sie, dass das von Ihnen gewählte Konto nicht bereits mit einem anderen OMS-Arbeitsbereich verknüpft sein darf. Wenn bereits eine Verknüpfung besteht, erhalten Sie eine Meldung mit dem Hinweis, dass Sie ein anderes ausführendes Automation-Konto wählen oder ein neues Konto erstellen müssen.<br><br> ![Automation-Konto bereits mit OMS-Arbeitsbereich verknüpft](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Wählen Sie abschließend im Bereich **Lösung hinzufügen**die Option **Konfiguration**. Der Bereich **Parameter** wird angezeigt.<br><br> ![Bereich „Parameter“ für die Lösung](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  Hier erhalten Sie folgende Aufforderung:  
   - Geben Sie einen Wert für die Zielressourcengruppennamen(**Target ResourceGroup Names**) ein. Dies sind Namen von Ressourcengruppen mit VMs, die mit dieser Lösung verwaltet werden.  Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt).  Die Verwendung eines Platzhalterzeichens wird unterstützt, wenn Sie einen Vorgang für VMs in allen Ressourcengruppen des Abonnements durchführen möchten.
   - Geben Sie die **VM Exclude List (string)** (VM-Ausschlussliste (Zeichenfolge)) an. Dies ist der Name von einem oder mehreren virtuellen Computern der Zielressourcengruppe.  Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt).  Platzhalter können verwendet werden.
   - Wählen Sie einen **Zeitplan** aus. Dies ist ein wiederkehrendes Datum und eine Uhrzeit zum Starten und Beenden der VMs in den Zielressourcengruppen.  Der Zeitplan wird standardmäßig für die UTC-Zeitzone konfiguriert. Es kann keine andere Region ausgewählt werden.  Falls Sie den Zeitplan nach dem Konfigurieren der Lösung für Ihre Zeitzone konfigurieren möchten, helfen Ihnen die Informationen unter [Ändern des Zeitplans für das Starten und Herunterfahren](#modifying-the-startup-and-shutdown-schedule) weiter.
   - Um **E-Mail-Benachrichtigungen** von SendGrid zu empfangen, übernehmen Sie den Standardwert **Ja**, und geben Sie eine gültige E-Mail-Adresse an. Wenn Sie hier **Nein** wählen und sich dann später doch für den Empfang von E-Mail-Benachrichtigungen entscheiden, können Sie die Variable **External_EmailToAddress** mit gültigen E-Mail-Adressen getrennt durch Kommas aktualisieren und anschließend die Variable **External_IsSendEmail** ändern (Wert **Ja**).  

9. Wählen Sie **Erstellen**, nachdem Sie die Konfiguration der erforderlichen Anfangseinstellungen für die Lösung abgeschlossen haben.  Nachdem alle Einstellungen überprüft wurden, wird die Lösung für Ihr Abonnement bereitgestellt.  Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

## <a name="collection-frequency"></a>Sammlungshäufigkeit

Die Daten des Automation-Auftragsprotokolls und -Auftragsdatenstroms werden alle fünf Minuten im Log Analytics-Repository erfasst.  

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie die Lösung „VM-Verwaltung“ hinzufügen, wird die Kachel **StartStopVM View** (StartStopVM-Ansicht) im Azure-Portal Ihrem Dashboard für den Log Analytics-Arbeitsbereich hinzugefügt.  Auf dieser Kachel werden die Anzahl und eine grafische Darstellung der Runbookaufträge für die Lösung angezeigt, die gestartet und erfolgreich abgeschlossen wurden.<br><br> ![Kachel „StartStopVM View“ (StartStopVM-Ansicht) der VM-Verwaltung](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

In Ihrem Automation-Konto können Sie auf die Lösung zugreifen und sie verwalten, indem Sie die Option **Arbeitsbereich** wählen. Wählen Sie auf der Log Analytics-Seite im Bereich links die Option **Lösungen**.  Wählen Sie auf der Seite **Lösungen** in der Liste die Lösung **Start-Stop-VM [workspace]** aus.<br><br> ![Liste der Lösungen in Log Analytics](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Nach dem Auswählen der Lösung wird der Lösungsbereich **Start-Stop-VM[workspace]** angezeigt. Hier können Sie wichtige Details überprüfen, z.B. die Kachel **StartStopVM**. Wie im Log Analytics-Arbeitsbereich auch, werden auf dieser Kachel die Anzahl und eine grafische Darstellung der Runbookaufträge für die Lösung angezeigt, die gestartet und erfolgreich abgeschlossen wurden.<br><br> ![Seite der Automation-Lösung „Updateverwaltung“](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Hier können Sie eine weiter gehende Analyse der Auftragsdatensätze durchführen, indem Sie auf die Ringkachel klicken. Im Lösungsdashboard werden der Auftragsverlauf und vordefinierte Protokollsuchabfragen angezeigt. Wechseln Sie zum erweiterten Log Analytics-Portal, um basierend auf Ihren Suchabfragen eine Suche durchzuführen.  

Alle übergeordneten Runbooks enthalten den Parameter *WhatIf*. Bei der Festlegung auf **True** wird die Beschreibung des exakten Verhaltens des Runbooks unterstützt, wenn es ohne den Parameter *WhatIf* ausgeführt wird, und es wird überprüft, ob die richtigen VMs als Ziel verwendet werden. Runbooks führen die zugehörigen definierten Aktionen nur aus, wenn der Parameter *WhatIf* auf **False** festgelegt ist.  


### <a name="scenario-1-daily-startstop-vms-across-a-subscription-or-target-resource-groups"></a>Szenario 1: Tägliches Starten/Beenden von VMs in einem Abonnement oder in Zielressourcengruppen 

Dies ist die Standardkonfiguration bei der ersten Bereitstellung der Lösung.  Beispielsweise können Sie sie so konfigurieren, dass alle VMs in einem Abonnement zum Feierabend beendet und am Morgen wieder gestartet werden, wenn Sie wieder ins Büro kommen. Wenn Sie die Zeitpläne **Scheduled-StartVM** und **Scheduled-StopVM** während der Bereitstellung konfigurieren, werden Ziel-VMs nach diesen Zeitplänen gestartet und beendet.
>[!NOTE]
>Die Zeitzone entspricht Ihrer aktuellen Zeitzone zum Zeitpunkt der Konfiguration des Parameters für den geplanten Zeitpunkt. Sie wird in Azure Automation aber im UTC-Format gespeichert.  Sie müssen keine Zeitzonenkonvertierung durchführen, da dies während der Bereitstellung verarbeitet wird.

Sie steuern die entsprechenden VMs, indem Sie die folgenden Variablen konfigurieren: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** und **External_ExcludeVMNames**.  

>[!NOTE]
> Der Wert für **Target ResourceGroup Names** wird sowohl als Wert für **External_Start_ResourceGroupNames** als auch für **External_Stop_ResourceGroupNames** gespeichert. Für weitere Granularität können Sie jede dieser Variablen so ändern, dass sie für unterschiedliche Ressourcengruppen gelten.  Verwenden Sie **External_Start_ResourceGroupNames** für Startaktionen und **External_Stop_ResourceGroupNames** für Beendigungsaktionen. VMs werden automatisch den Zeitplänen zum Starten und Beenden hinzugefügt.

Um die Konfiguration zu testen und zu überprüfen, starten Sie das Runbook **ScheduledStartStop_Parent** manuell und legen den Parameter ACTION auf **start** oder **stop** und den Parameter WHATIF auf **True** fest.<br><br> ![Konfigurieren von Parametern für Runbooks](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)


 Zeigen Sie für die geplante Aktion die Vorschau an, und nehmen Sie alle erforderlichen Änderungen vor, bevor Sie die Implementierung für VMs in der Produktion durchführen.  Sie können das Runbook manuell ausführen, wobei Sie den Parameter auf **False** festlegen, oder Sie können die Automation-Zeitpläne **Schedule-StartVM** und **Schedule-StopVM** gemäß Ihrem vorgegebenen Zeitplan automatisch ausführen.

### <a name="scenario-2-sequence-the-startstop-vms-across-a-subscription-by-using-tags"></a>Szenario 2: Festlegen der Reihenfolge zum Starten/Beenden von VMs in einem Abonnement mithilfe von Tags
In einer Umgebung mit mehreren Komponenten auf mehreren VMs, die eine verteilte Workload unterstützen, spielt die Reihenfolge, in der Komponenten nacheinander gestartet und beendet werden, eine wichtige Rolle.  Führen Sie hierzu die folgenden Schritte aus:

1. Fügen Sie ein **SequenceStart**- und ein **SequenceStop**-Tag mit einer positiven ganzen Zahl den VMs hinzu, die in den Variablen **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupNames** als Ziel angegeben sind.  Die Aktionen zum Starten und Beenden werden in aufsteigender Reihenfolge durchgeführt.  Informationen zum Markieren von VMs finden Sie unter [Markieren eines virtuellen Windows-Computers in Azure](../virtual-machines/windows/tag.md) und [Markieren eines virtuellen Linux-Computers in Azure](../virtual-machines/linux/tag.md).
2. Ändern Sie die Zeitpläne **Sequenced-StartVM** und **Sequenced-StopVM** in das Datum und den Zeitpunkt, die Ihren Anforderungen entsprechen, und aktivieren Sie die Zeitpläne.  

Starten Sie das Runbook **SequencedStartStop_Parent** manuell, um Ihre Konfiguration zu testen und zu überprüfen. Legen Sie den Parameter ACTION auf **start** oder **stop** und den Parameter WHATIF auf **True** fest.<br><br> ![Konfigurieren von Parametern für Runbooks](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Zeigen Sie für die Aktion eine Vorschau an, und nehmen Sie alle erforderlichen Änderungen vor, bevor Sie die Implementierung für Produktions-VMs durchführen.  Nach Abschluss des Vorgangs können Sie das Runbook manuell ausführen, wobei Sie den Parameter auf **False** festlegen, oder Sie können die Automation-Zeitpläne **Sequenced-StartVM** und **Sequenced-StopVM** entsprechend Ihrem vorgegebenen Zeitplan automatisch ausführen.  

### <a name="scenario-3-automate-startstop-vms-across-a-subscription-based-on-cpu-utilization"></a>Szenario 3: Automatisches Starten/Beenden von VMs in einem Abonnement basierend auf der CPU-Auslastung
Diese Lösung kann einen Beitrag zur Verwaltung der Kosten für die Ausführung von VMs in Ihrem Abonnement leisten, indem Azure-VMs, die außerhalb der Spitzenzeiten (z.B. nach Geschäftsschluss) nicht verwendet werden, ausgewertet und automatisch beendet werden, wenn die Prozessorauslastung unter x% liegt.  

Standardmäßig ist die Lösung so vorkonfiguriert, dass die Metrik zur CPU-Auslastung in Prozent ausgewertet wird, um zu ermitteln, ob die durchschnittliche Auslastung kleiner oder gleich 5% ist.  Dies wird mit den folgenden Variablen gesteuert und kann geändert werden, wenn die Standardwerte nicht Ihren Anforderungen entsprechen:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Sie können die Aktion entweder für ein Abonnement und eine Ressourcengruppe oder für eine spezifische Liste von VMs aktivieren, aber nicht für beides.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Festlegen der Beendigungsaktion für ein Abonnement und eine Ressourcengruppe

1. Konfigurieren Sie die Variablen **External_Stop_ResourceGroupNames** und **External_ExcludeVMNames**, um die Ziel-VMs anzugeben.  
2. Aktivieren und aktualisieren Sie den Zeitplan **Schedule_AutoStop_CreateAlert_Parent**.
3. Führen Sie das Runbook **AutoStop_CreateAlert_Parent** aus. Legen Sie dabei den Parameter ACTION auf **start** und den Parameter WHATIF auf **True** fest, um für die Änderungen eine Vorschau anzuzeigen.

#### <a name="target-the-stop-action-by-vm-list"></a>Festlegen der Beendigungsaktion über eine VM-Liste

1. Führen Sie das Runbook **AutoStop_CreateAlert_Parent** aus. Legen Sie dabei den Parameter ACTION auf **start** fest, fügen Sie eine durch Trennzeichen getrennte Liste von VMs im Parameter *VMList* hinzu, und legen Sie den Parameter WHATIF auf **True** fest. Zeigen Sie eine Vorschau für die Änderungen an.  
2. Konfigurieren Sie den Parameter **External_ExcludeVMNames** mit einer durch Kommas getrennten Liste von VMs (VM1,VM2,VM3).
3. In diesem Szenario werden die Variablen **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupnames** nicht berücksichtigt.  Für dieses Szenario müssen Sie Ihren eigenen Automation-Zeitplan erstellen. Ausführliche Informationen finden Sie unter [Planen eines Runbooks in Azure Automation](../automation/automation-schedules.md).

Nachdem Sie einen Zeitplan zum Beenden von VMs basierend auf der CPU-Auslastung erstellt haben, müssen Sie nun einen der folgenden Zeitpläne aktivieren, um sie zu starten.

* Festlegen der Startaktion nach Abonnement und Ressourcengruppe.  Informationen zum Testen und Aktivieren von Zeitplänen vom Typ **Scheduled-StartVM** finden Sie in den im [Szenario 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) beschriebenen Schritten.
* Festlegen der Startaktion nach Abonnement, Ressourcengruppe und Tag.  Informationen zum Testen und Aktivieren von Zeitplänen vom Typ **Sequenced-StartVM** finden Sie in den im [Szenario 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) beschriebenen Schritten.


### <a name="configuring-email-notifications"></a>Konfigurieren von E-Mail-Benachrichtigungen

Ändern Sie die folgenden drei Variablen, um E-Mail-Benachrichtigungen nach der Bereitstellung der Lösung zu konfigurieren:

* External_EmailFromAddress: Geben Sie die E-Mail-Adresse des Absenders an.
* External_EmailToAddress: Geben Sie eine durch Kommas getrennte Liste mit E-Mail-Adressen (user@hotmail.com, user@outlook.com) zum Empfangen von E-Mail-Benachrichtigungen an.
* External_IsSendEmail: Wählen Sie **Yes**, um E-Mails zu empfangen. Legen Sie den Wert auf **No** fest, um E-Mail-Benachrichtigungen zu deaktivieren.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Ändern der Zeitpläne für das Starten und Herunterfahren

Bei der Verwaltung der Zeitpläne für das Starten und Herunterfahren werden bei dieser Lösung die gleichen Schritte ausgeführt, die unter [Planen eines Runbooks in Azure Automation](automation-schedules.md) beschrieben werden.     

## <a name="log-analytics-records"></a>Log Analytics-Datensätze

Automation erstellt zwei Arten von Datensätzen im OMS-Repository: Auftragsprotokolle und Auftragsdatenströme.

### <a name="job-logs"></a>Auftragsprotokolle

Eigenschaft | BESCHREIBUNG|
----------|----------|
Aufrufer |  Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.|
Category (Kategorie) | Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobLogs“.|
CorrelationId | Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt.|
JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“.|
Ressourcen-ID | Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
ResourceGroup | Gibt den Namen der Ressourcengruppe des Runbookauftrags an.|
ResourceProvider | Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können.  Für Automation lautet der Wert „Azure Automation“.|
ResourceType | Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
resultType | Der Status des Runbookauftrags.  Mögliche Werte:<br>- Gestartet<br>- Beendet<br>- Ausgesetzt<br>- Fehler<br>- Erfolgreich|
resultDescription | Beschreibt den resultierenden Zustand des Runbookauftrags.  Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen|
RunbookName | Gibt den Namen des Runbooks an.|
SourceSystem | Gibt das Quellsystem für die gesendeten Daten an.  Für Automation lautet der Wert „OpsManager“.|
StreamType | Gibt den Typ des Ereignisses an. Mögliche Werte:<br>- Ausführlich<br>- Ausgabe<br>- Fehler<br>- Warnung|
SubscriptionId | Gibt die Abonnement-ID des Auftrags an.
Zeit | Ausführungsdatum und -uhrzeit des Runbookauftrags.|


### <a name="job-streams"></a>Auftragsdatenströme

Eigenschaft | BESCHREIBUNG|
----------|----------|
Aufrufer |  Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.|
Category (Kategorie) | Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobStreams“.|
JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“.|
ResourceGroup | Gibt den Namen der Ressourcengruppe des Runbookauftrags an.|
Ressourcen-ID | Gibt die Ressourcen-ID in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
ResourceProvider | Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können.  Für Automation lautet der Wert „Azure Automation“.|
ResourceType | Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
resultType | Das Ergebnis des Runbookauftrags zum Zeitpunkt der Ereignisgenerierung. Ein möglicher Wert ist:<br>- InProgress|
resultDescription | Enthält den Ausgabedatenstrom des Runbooks.|
RunbookName | Der Name des Runbooks.|
SourceSystem | Gibt das Quellsystem für die gesendeten Daten an.  Für Automation lautet der Wert „OpsManager“.|
StreamType | Der Typ des Auftragsstreams. Mögliche Werte:<br>- Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich|
Zeit | Ausführungsdatum und -uhrzeit des Runbookauftrags.|

Wenn Sie Protokollsuchen durchführen, bei denen Kategoriedatensätze für **JobLogs** oder **JobStreams** zurückgegeben werden, können Sie die Ansicht **JobLogs** oder **JobStreams** wählen. Es werden dann mehrere Kacheln angezeigt, mit denen die von der Suche zurückgegebenen Updates zusammengefasst werden.

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen

Die folgende Tabelle enthält Beispiele für Protokollsuchen für Auftragsdatensätze, die mit dieser Lösung erfasst wurden. 

Abfragen | BESCHREIBUNG|
----------|----------|
Suchen nach Aufträgen für das Runbook „ScheduledStartStop_Parent“, die erfolgreich abgeschlossen wurden | search Category == "JobLogs" &#124; where ( RunbookName_s == "ScheduledStartStop_Parent" ) &#124; where ( ResultType == "Completed" )  &#124; summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) &#124; sort by TimeGenerated desc|
Suchen nach Aufträgen für das Runbook „SequencedStartStop_Parent“, die erfolgreich abgeschlossen wurden | search Category == "JobLogs" &#124; where ( RunbookName_s == "SequencedStartStop_Parent" ) &#124; where ( ResultType == "Completed" )  &#124; summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) &#124; sort by TimeGenerated desc

## <a name="removing-the-solution"></a>Entfernen der Lösung

Wenn Sie die Lösung nicht mehr benötigen, können Sie sie aus dem Automation-Konto löschen.  Beim Löschen der Lösung werden nur die Runbooks entfernt. Die Zeitpläne oder Variablen, die beim Hinzufügen der Lösung erstellt wurden, werden nicht gelöscht.  Diese Objekte müssen Sie manuell löschen, wenn Sie sie nicht mit anderen Runbooks verwenden.  

Führen Sie die folgenden Schritte aus, um die Lösung zu löschen:

1.  Wählen Sie in Ihrem Automation-Konto im linken Bereich die Option **Arbeitsbereich** aus.  
2.  Wählen Sie auf der Seite **Lösungen** die Lösung **Start-Stop-VM[Workspace]** aus.  Wählen Sie auf der Seite **VMManagementSolution[Workspace]** im Menü die Option **Löschen**.<br><br> ![Löschen der VM-Mgmt-Lösung](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  Bestätigen Sie im Fenster **Lösung löschen**, dass Sie die Lösung löschen möchten.
4.  Während die Informationen überprüft werden und die Lösung gelöscht wird, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.  Nachdem der Vorgang zum Löschen der Lösung begonnen hat, kehren Sie zur Seite **Lösungen** zurück.  

Das Automation-Konto und der Log Analytics-Arbeitsbereich werden bei diesem Vorgang nicht gelöscht.  Wenn Sie den Log Analytics-Arbeitsbereich nicht beibehalten möchten, müssen Sie ihn manuell löschen.  Dies ist über das Azure-Portal möglich:

1.    Wählen Sie auf der Startseite des Azure-Portals die Option **Log Analytics**.
2. Wählen Sie im Bereich **Log Analytics** den Arbeitsbereich aus.
3. Wählen Sie im Bereich mit den Einstellungen für den Arbeitsbereich die Menüoption **Löschen**.  
      
## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation-Auftragsprotokolle mit Log Analytics finden Sie unter [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md).
- Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md).
- Weitere Informationen zu Log Analytics und Datenerfassungsquellen finden Sie unter [Sammeln von Azure-Speicherdaten in Log Analytics – Übersicht](../log-analytics/log-analytics-azure-storage.md).






   

