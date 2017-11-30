---
title: "Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten | Microsoft-Dokumentation"
description: "Mit den Lösungen für die VM-Verwaltung werden Ihre virtuellen Azure Resource Manager-Computer nach einem Zeitplan gestartet und beendet und mit Log Analytics proaktiv überwacht."
services: automation
documentationCenter: 
authors: eslesar
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: magoedte
ms.openlocfilehash: 2ff2208f62c24c460c9d17533e28fd007549828b
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten in Azure Automation

Mit der Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten können Sie Ihre virtuellen Azure-Computer nach benutzerdefinierten Zeitplänen starten und beenden. Zudem erhalten Sie über Log Analytics Einblick in Ihre Daten und können durch Verwenden von [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview) optional E-Mails senden. Die Lösung unterstützt in den meisten Szenarien sowohl Azure Resource Manager-VMs als auch klassische VMs. 

Diese Lösung umfasst eine dezentrale Automatisierungsfunktion für Kunden, die durch Nutzung serverloser und kostengünstiger Ressourcen ihre Kosten reduzieren möchten. Folgende Features sind enthalten:

* Starten/Beenden von VMs nach Zeitplan
* Starten/Beenden von VMs nach Zeitplan in aufsteigender Reihenfolge mithilfe von Azure-Tags (wird für klassische VMs nicht unterstützt)
* Automatisches Beenden von VMs bei geringer CPU-Auslastung

## <a name="prerequisites"></a>Voraussetzungen

- Für die Runbooks wird ein [Ausführendes Azure-Konto](automation-offering-get-started.md#authentication-methods) verwendet.  Das ausführende Konto ist die bevorzugte Authentifizierungsmethode, da anstelle eines Kennworts, das ablaufen oder sich häufig ändern kann, eine Zertifikatauthentifizierung verwendet wird.  

- Mit dieser Lösung können nur VMs verwaltet werden, die unter demselben Abonnement wie das Automation-Konto enthalten sind.  

- Diese Lösung wird nur in den folgenden Azure-Regionen bereitgestellt: „Australien, Südosten“, „Kanada, Mitte“, „Indien, Mitte“, „USA, Osten“, „Japan, Osten“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“ und „Europa, Westen“.  
    
    > [!NOTE]
    > Die Runbooks zum Verwalten des VM-Zeitplans können für VMs in jeder beliebigen Region verwendet werden.  

- Um beim Onboarding über den Azure Marketplace festzulegen, dass E-Mail-Benachrichtigungen gesendet werden, wenn die Runbooks zum Starten und Beenden von VMs abgeschlossen sind, müssen Sie **Ja** auswählen, um SendGrid bereitzustellen. 

    > [!IMPORTANT]
    > SendGrid ist ein Drittanbieterdienst. Um Unterstützung zu SendGrid zu erhalten, wenden Sie sich an [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Bei der Verwendung von SendGrid bestehen folgende Einschränkungen:

    * Maximal ein SendGrid-Konto pro Benutzer pro Abonnement
    * Maximal zwei SendGrid-Konten pro Abonnement

Wenn Sie eine frühere Version dieser Lösung bereitgestellt haben, müssen Sie sie vor der Bereitstellung dieses Releases zunächst aus Ihrem Konto löschen.  

## <a name="solution-components"></a>Lösungskomponenten

Diese Lösung umfasst vorkonfigurierte Runbooks, Zeitpläne und die Integration in Log Analytics, sodass Sie das Starten und Beenden Ihrer virtuellen Computer entsprechend Ihren Geschäftsanforderungen anpassen können. 

### <a name="runbooks"></a>Runbooks

In der folgenden Tabelle sind die in Ihrem Automation-Konto bereitgestellten Runbooks aufgeführt.  Es wird davon abgeraten, am Runbookcode Änderungen vorzunehmen. Schreiben Sie stattdessen für neue Funktionen Ihr eigenes Runbook.

> [!NOTE]
> Führen Sie Runbooks mit dem Namen „Child“ am Ende des Namens nicht direkt aus.
>

Alle übergeordneten Runbooks enthalten den Parameter *WhatIf*, der bei Festlegung auf **TRUE** die Beschreibung des exakten Verhaltens des Runbooks unterstützt, wenn es ohne den Parameter *WhatIf* ausgeführt wird, und der überprüft, ob die richtigen VMs als Ziel verwendet werden.  Runbooks führen die zugehörigen definierten Aktionen nur aus, wenn der Parameter *WhatIf* auf **FALSE** festgelegt ist. 

|**Runbook** | **Parameter** | **Beschreibung**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Wird nur über das übergeordnete Runbook aufgerufen. Erstellt für das AutoStop-Szenario Warnungen pro Ressource.| 
|AutoStop_CreateAlert_Parent | WhatIf: TRUE oder FALSE <br> VMList | Erstellt oder aktualisiert Azure-Warnungsregeln auf VMs im Zielabonnement oder den Zielressourcengruppen. <br> VMList: durch Trennzeichen getrennte Liste von VMs,  z.B. *vm1,vm2,vm3*.| 
|AutoStop_Disable | (Keine) | Deaktiviert AutoStop-Warnungen und den Standardzeitplan.| 
|AutoStop_StopVM_Child | WebHookData | Wird nur über das übergeordnete Runbook aufgerufen. Warnungsregeln rufen dieses Runbook auf. Dieses beendet dann den virtuellen Computer.|  
|Bootstrap_Main | (Keine) | Wird einmal zum Einrichten von Bootstrapkonfigurationen wie webhookURI verwendet, auf die normalerweise nicht über Azure Resource Manager zugegriffen werden kann. Dieses Runbook wird automatisch entfernt, wenn die Bereitstellung erfolgreich durchgeführt wurde.|  
|ScheduledStartStop_Child | VMName <br> Aktion: Beenden oder Starten <br> ResourceGroupName | Wird nur über das übergeordnete Runbook aufgerufen. Führt die eigentliche Aktion zum Beenden oder Starten für eine geplante Beendigung aus.|  
|ScheduledStartStop_Parent | Aktion: Beenden oder Starten <br> WhatIf: TRUE oder FALSE | Wirkt sich auf alle VMs im Abonnement aus, es sei denn, Sie bearbeiten die Variablen **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupNames**, sodass es nur für diese Zielressourcengruppen ausgeführt wird. Sie können zudem bestimmte VMs ausschließen, indem Sie die Variable **External_ExcludeVMNames** aktualisieren. „WhatIf“ verhält sich wie in den anderen Runbooks.|  
|SequencedStartStop_Parent | Aktion: Beenden oder Starten <br> WhatIf: TRUE oder FALSE | Erstellen Sie ein Tag mit der Bezeichnung **SequenceStart** und ein anderes Tag namens **SequenceStop** auf allen VMs, für die Sie die Reihenfolge der Aktivitäten zum Starten\\Beenden festlegen möchten. Der Wert des Tags muss eine positive ganze Zahl (1,2,3) sein, die der Reihenfolge entspricht, in der Aktionen zum Starten\\Beenden aufsteigend durchgeführt werden sollen. „WhatIf“ verhält sich wie in den anderen Runbooks. <br> **Hinweis: VMs müssen sich in Ressourcengruppen mit definierten „External_Start_ResourceGroupNames“, „External_Stop_ResourceGroupNames“ und „External_ExcludeVMNames“ in Azure Automation-Variablen befinden und die entsprechenden Tags aufweisen, damit Aktionen wirksam werden.**|

### <a name="variables"></a>Variables

In der folgenden Tabelle sind die in Ihrem Automation-Konto erstellten Variablen aufgeführt.  Es wird empfohlen, nur Variablen mit dem Präfix **External** zu ändern. Das Ändern von Variablen mit dem Präfix **Internal** führt zu unerwünschten Auswirkungen.  

|**Variable** | **Beschreibung**|
---------|------------|
|External_AutoStop_Condition | Dies ist der bedingte Operator, der zum Konfigurieren der Bedingung erforderlich ist, die eine Warnung auslöst. Zulässige Werte sind **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, **LessThanOrEqual**.|  
|External_AutoStop_Description | Warnung zum Beenden des virtuellen Computers, wenn der CPU-Prozentsatz den Schwellenwert überschreitet.|  
|External_AutoStop_MetricName | Name der Leistungsmetrik, für die die Azure-Warnungsregel konfiguriert wird.| 
|External_AutoStop_Threshold | Schwellenwert für die Azure-Warnungsregel, der in der Variable *External_AutoStop_MetricName* angegeben ist. Prozentwerte können zwischen 1 und 100 liegen.|  
|External_AutoStop_TimeAggregationOperator | Der Zeitaggregationsoperator, der auf die ausgewählte Fenstergröße angewendet wird, um die Bedingung auszuwerten. Zulässige Werte sind **Average**, **Minimum**, **Maximum**, **Total**, **Last**.|  
|External_AutoStop_TimeWindow | Die Größe des Fensters, über die in Azure die ausgewählte Metrik zum Auslösen einer Warnung analysiert wird. Für diesen Parameter können Zeiträume eingegeben werden. Mögliche Werte reichen von fünf Minuten bis sechs Stunden.|  
|External_EmailFromAddress | Gibt den Absender der E-Mail an.|  
|External_EmailSubject | Gibt den Text für die Betreffzeile der E-Mail an.|  
|External_EmailToAddress | Gibt die Empfänger der E-Mail an. Trennen Sie Namen durch Kommas.|  
|External_ExcludeVMNames | Geben Sie die Namen der auszuschließenden VMs ein. Trennen Sie Namen durch Kommas ohne Leerzeichen.|  
|External_IsSendEmail | Gibt die Option zum Senden einer E-Mail-Benachrichtigung nach Abschluss an.  Geben Sie **Yes** oder **No** an, wenn keine E-Mails gesendet werden sollen.  Diese Option muss auf **No** festgelegt sein, wenn Sie SendGrid bei der ersten Bereitstellung nicht erstellt haben.|  
|External_Start_ResourceGroupNames | Gibt eine oder mehrere Zielressourcengruppen, deren Werte durch Kommas getrennt sind, für Startaktionen an.|  
|External_Stop_ResourceGroupNames | Gibt eine oder mehrere Zielressourcengruppen, deren Werte durch Kommas getrennt sind, für Beendigungsaktionen an.|  
|Internal_AutomationAccountName | Gibt den Namen des Automation-Kontos an.|  
|Internal_AutoSnooze_WebhookUri | Gibt den Webhook-URI an, der für das AutoStop-Szenario aufgerufen wird.|  
|Internal_AzureSubscriptionId | Gibt die Azure-Abonnement-ID an.|  
|Internal_ResourceGroupName | Gibt den Ressourcengruppennamen des Azure Automation-Kontos an.|  
|Internal_SendGridAccountName | Gibt den Namen des SendGrid-Kontos an.|  
|Internal_SendGridPassword | Gibt das Kennwort des SendGrid-Kontos an.|  

<br>

In allen Szenarien müssen die Variablen **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** und **External_ExcludeVMNames** für die Ziel-VMs angegeben werden, mit Ausnahme der Angabe einer durch Trennzeichen getrennten Liste von VMs für das Runbook **AutoStop_CreateAlert_Parent**. Das heißt, die VMs müssen sich in Zielressourcengruppen befinden, damit Aktionen zum Starten oder Beenden durchgeführt werden. Die Logik ähnelt der von Azure-Richtlinien, da Sie das Abonnement oder die Ressourcengruppe als Ziel verwenden können und neu erstellte VMs Aktionen erben. Dadurch wird vermieden, dass für jeden virtuellen Computer jeweils ein separater Zeitplan verwaltet werden muss und dass Aktionen zum Starten/Beenden als Skalierung verwaltet werden müssen.

### <a name="schedules"></a>Zeitpläne

In der folgenden Tabelle sind die einzelnen in Ihrem Automation-Konto erstellten Standardzeitpläne aufgeführt.  Sie können sie ändern oder Ihre eigenen benutzerdefinierten Zeitpläne erstellen.  Standardmäßig sind alle diese Zeitpläne deaktiviert, mit Ausnahme von **Scheduled_StartVM** und **Scheduled-StopVM**.

Es wird davon abgeraten, alle Zeitpläne zu aktivieren, da dies im Hinblick darauf, nach welchem Zeitplan eine Aktion durchgeführt wird, zu Überschneidungen führen kann.  Stattdessen empfiehlt es sich, festzulegen, welche Optimierungen Sie ausführen möchten, und dann die entsprechenden Änderungen vorzunehmen.  Weitere Erläuterungen finden Sie in den Beispielszenerien im Übersichtsabschnitt.   

|**ScheduleName** | **Frequency** | **Beschreibung**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Ausführung alle acht Stunden | Führt das Runbook „AutoStop_CreateAlert_Parent“ alle 8 Stunden aus. Dieses Runbook beendet dann VMs basierend auf den Werten in „External_Start_ResourceGroupNames“, „External_Stop_ResourceGroupNames“ und „External_ExcludeVMNames“ in Azure Automation-Variablen.  Alternativ können Sie mithilfe des Parameters „VMList“ eine durch Trennzeichen getrennte Liste von VMs angeben.|  
|Scheduled_StopVM | Benutzerdefiniert, jeden Tag | Führt das Runbook „Scheduled_Parent“ mit dem Parameter „Stop“ jeden Tag zum angegebenen Zeitpunkt aus.  Beendet automatisch alle VMs, die den über Ressourcenvariablen definierten Regeln entsprechen.  Es empfiehlt sich, den gleichgeordneten Zeitplan „Scheduled-StartVM“ zu aktivieren.|  
|Scheduled_StartVM | Benutzerdefiniert, jeden Tag | Führt das Runbook „Scheduled_Parent“ mit dem Parameter *Start* jeden Tag zum angegebenen Zeitpunkt aus.  Startet automatisch alle VMs, die den in den jeweiligen Variablen definierten Regeln entsprechen.  Es empfiehlt sich, den gleichgeordneten Zeitplan **Scheduled-StopVM** zu aktivieren.|
|Sequenced-StopVM | 01:00 Uhr (UTC), jeden Freitag | Führt das Runbook „Sequenced_Parent“ mit dem Parameter *Stop* jeden Freitag zum angegebenen Zeitpunkt aus.  Beendet der Reihe nach (in aufsteigender Reihenfolge) alle VMs mit dem in den jeweiligen Variablen definierten Tag **SequenceStop**.  Weitere Informationen zu Tagwerten und Ressourcenvariablen finden Sie im Abschnitt „Runbooks“.  Es empfiehlt sich, den gleichgeordneten Zeitplan **Sequenced-StartVM** zu aktivieren.|
|Sequenced-StartVM | 13:00 Uhr (UTC), jeden Montag | Führt das Runbook „Sequenced_Parent“ mit dem Parameter *Start* jeden Montag zum angegebenen Zeitpunkt aus.  Startet der Reihe nach (in absteigender Reihenfolge) alle VMs mit dem in den jeweiligen Variablen definierten Tag **SequenceStart**.  Weitere Informationen zu Tagwerten und Ressourcenvariablen finden Sie im Abschnitt „Runbooks“.  Es empfiehlt sich, den gleichgeordneten Zeitplan **Sequenced-StopVM** zu aktivieren.|

<br>

## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Lösung zum Starten/Beenden von VMs außerhalb der Kernzeit [Vorschau] dem Automation-Konto hinzuzufügen und anschließend die Variablen zum Anpassen der Lösung zu konfigurieren.

1. Klicken Sie im Azure-Portal auf **Neu**.<br> ![Azure-Portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. Geben Sie im Bereich „Marketplace“ **VM starten** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **VMs außerhalb der Geschäftszeiten starten/beenden [Vorschau]** in den Suchergebnissen aus.  
3. Sehen Sie sich im Bereich **VMs außerhalb der Geschäftszeiten starten/beenden [Vorschau]** für die ausgewählte Lösung die Zusammenfassung an, und klicken Sie dann auf **Erstellen**.  
4. Der Bereich **Lösung hinzufügen** wird angezeigt, und Sie werden aufgefordert, die Lösung vor dem Importieren in Ihr Automation-Abonnement zu konfigurieren.<br><br> ![Blatt „Lösung hinzufügen“ der VM-Verwaltung](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Wählen Sie auf dem Blatt **Lösung hinzufügen** die Option **Arbeitsbereich** aus. Hier wählen Sie einen OMS-Arbeitsbereich aus, der mit demselben Azure-Abonnement verknüpft ist, unter dem sich das Automation-Konto befindet. Sie können aber auch einen neuen Arbeitsbereich erstellen.  Wenn Sie noch nicht über einen Arbeitsbereich verfügen, können Sie die Option **Neuen Arbeitsbereich erstellen** auswählen und im Bereich für den **OMS-Arbeitsbereich** wie folgt vorgehen: 
   - Geben Sie einen Namen für den neuen **OMS-Arbeitsbereich** an.
   - Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
   - Unter **Ressourcengruppe** können Sie eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe auswählen.  
   - Wählen Sie einen **Speicherort**aus.  Derzeit können nur die Standorte **Australien, Südosten**, **Kanada, Mitte**, **Indien, Mitte**, **USA, Osten**, **Japan, Osten**, **Asien, Südosten**, **Vereinigtes Königreich, Süden** und **Europa, Westen** ausgewählt werden.
   - Wählen Sie einen **Tarif**aus.  Für die Lösung sind zwei OMS-Tarife verfügbar: kostenlos (Free) und kostenpflichtig.  Beim Free-Tarif gelten Beschränkungen für die Datenmenge, die pro Tag erfasst werden kann, die Aufbewahrungsdauer und die Laufzeit von Runbookaufträgen in Minuten.  Für den kostenpflichtigen OMS-Tarif gilt keine Beschränkung für die täglich erfasste Datenmenge.  

        > [!NOTE]
        > Der kostenpflichtige Tarif pro GB (Standalone) wird zwar als Option angezeigt, ist aber nicht verfügbar.  Wenn Sie diesen Tarif auswählen und mit der Erstellung der Lösung in Ihrem Abonnement fortfahren, tritt ein Fehler auf.  Dies wird behoben sein, wenn die Lösung offiziell veröffentlicht wird.<br>Bei Verwendung dieser Lösung werden nur Automation-Auftragsminuten und die Protokollerfassung genutzt.  Mit der Lösung werden Ihrer Umgebung keine weiteren OMS-Knoten hinzugefügt.  

6. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen auf dem Blatt **OMS-Arbeitsbereich** auf **Erstellen**.  Die Informationen werden überprüft, und der Arbeitsbereich wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.  Sie gelangen wieder zum Blatt **Lösung hinzufügen**.  
7. Wählen Sie auf dem Blatt **Lösung hinzufügen** die Option **Automation-Konto**.  Bei der Erstellung eines neuen OMS-Arbeitsbereichs müssen Sie auch ein neues Automation-Konto erstellen, das dem zuvor angegebenen neuen OMS-Arbeitsbereich zugeordnet wird, einschließlich Azure-Abonnement, -Ressourcengruppe und -Region.  Sie können die Option **Automation-Konto erstellen** wählen und auf dem Blatt **Automation-Konto hinzufügen** Folgendes angeben: 
  - Geben Sie im Feld **Name** den Namen des Automation-Kontos ein.

    Alle anderen Optionen werden basierend auf dem ausgewählten OMS-Arbeitsbereich automatisch aufgefüllt, und diese Optionen können nicht geändert werden.  Ein ausführendes Azure-Konto ist die Standardmethode für die Authentifizierung von Runbooks in dieser Lösung.  Nach dem Klicken auf **OK** werden die Konfigurationsoptionen überprüft, und das Automation-Konto wird erstellt.  Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

    Andernfalls können Sie auch ein vorhandenes ausführendes Automation-Konto wählen.  Beachten Sie, dass das ausgewählte Konto nicht bereits mit einem anderen OMS-Arbeitsbereich verknüpft sein darf. Andernfalls wird auf dem Blatt eine Meldung mit einem entsprechenden Hinweis angezeigt.  Wenn bereits eine Verknüpfung besteht, müssen Sie ein anderes ausführendes Automation-Konto wählen oder ein neues Konto erstellen.<br><br> ![Automation-Konto bereits mit OMS-Arbeitsbereich verknüpft](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Wählen Sie auf dem Blatt **Lösung hinzufügen** die Option **Konfiguration**. Das Blatt **Parameter** wird angezeigt.<br><br> ![Bereich „Parameter“ für die Lösung](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  Auf dem Blatt **Parameter** werden Sie zur Ausführung der folgenden Schritte aufgefordert:  
   - Geben Sie die **Zielressourcengruppennamen** an. Dies sind Ressourcengruppennamen, die mit dieser Lösung verwaltete VMs enthalten.  Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt).  Die Verwendung eines Platzhalterzeichens wird unterstützt, wenn Sie einen Vorgang für VMs in allen Ressourcengruppen des Abonnements durchführen möchten.
   - Geben Sie die **VM Exclude List (string)** (VM-Ausschlussliste [Zeichenfolge]) an, die den Namen eines oder mehrerer virtuellen Computer aus der Zielressourcengruppe enthält.  Sie können mehrere Namen eingeben und die Namen jeweils durch ein Komma trennen (Groß-/Kleinschreibung wird nicht berücksichtigt).  Platzhalter können verwendet werden.
   - Wählen Sie einen **Zeitplan** mit einem wiederkehrenden Datum und einer Uhrzeit zum Starten und Beenden der VMs in den Zielressourcengruppen.  Standardmäßig wird der Zeitplan für die UTC-Zeitzone konfiguriert, und die Auswahl einer anderen Region ist nicht möglich.  Wenn Sie den Zeitplan nach dem Konfigurieren der Lösung für Ihre Zeitzone konfigurieren möchten, finden Sie weitere Informationen unter [Ändern des Zeitplans für das Starten und Herunterfahren](#modifying-the-startup-and-shutdown-schedule).
   - Um **E-Mail-Benachrichtigungen** von SendGrid zu empfangen, übernehmen Sie den Standardwert **Ja**, und geben Sie eine gültige E-Mail-Adresse an.  Wenn Sie **Nein** auswählen und dann später entscheiden, dass E-Mail-Benachrichtigungen gesendet werden sollen, müssen Sie die Lösung erneut über den Azure Marketplace bereitstellen.  

10. Wählen Sie **Erstellen**, nachdem Sie die Konfiguration der erforderlichen Anfangseinstellungen für die Lösung abgeschlossen haben.  Alle Einstellungen werden überprüft, und anschließend wird versucht, die Lösung unter Ihrem Abonnement bereitzustellen.  Dieser Vorgang kann einige Sekunden dauern, und Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

## <a name="collection-frequency"></a>Sammlungshäufigkeit

Die Daten des Automation-Auftragsprotokolls und -Auftragsdatenstroms werden alle fünf Minuten im Log Analytics-Repository erfasst.  

## <a name="using-the-solution"></a>Verwenden der Lösung

Wenn Sie im Azure-Portal die Lösung „VM-Verwaltung“ in Ihrem Log Analytics-Arbeitsbereich hinzufügen, wird die Kachel **StartStopVM View** (StartStopVM-Ansicht) in Ihrem Dashboard hinzugefügt.  Auf dieser Kachel werden die Anzahl und eine grafische Darstellung der Runbookaufträge für die Lösung angezeigt, die gestartet und erfolgreich abgeschlossen wurden.<br><br> ![Kachel „StartStopVM View“ (StartStopVM-Ansicht) der VM-Verwaltung](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

In Ihrem Automation-Konto können Sie auf die Lösung zugreifen und sie verwalten, indem Sie die Option **Arbeitsbereich** auswählen und dann auf der Seite „Log Analytics“ im linken Bereich die Option **Lösungen** auswählen.  Wählen Sie auf der Seite **Lösungen** in der Liste die Lösung **Start-Stop-VM [Workspace]** aus.<br><br> ![Liste der Lösungen in Log Analytics](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Nach dem Auswählen der Lösung wird das Blatt für die Lösung **Start-Stop-VM[Workspace]** angezeigt, auf dem Sie wichtige Details überprüfen können. Ein Beispiel hierfür ist die Kachel **StartStopVM**, wie in Ihrem Log Analytics-Arbeitsbereich, auf der die Anzahl und eine grafische Darstellung der Runbookaufträge für die Lösung angezeigt werden, die gestartet und erfolgreich abgeschlossen wurden.<br><br> ![Seite der Automation-Lösung „Updateverwaltung“](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Hier können Sie auch eine weitere Analyse der Auftragsdatensätze durchführen, indem Sie auf die Kachel „Ring“ klicken. Im Lösungsdashboard werden dann der Auftragsverlauf und vordefinierte Protokollsuchabfragen angezeigt. Zudem können Sie zum Log Analytics-Portal wechseln, um eine Suche basierend auf Ihren Suchabfragen durchzuführen.  

Alle übergeordneten Runbooks enthalten den Parameter *WhatIf*, der bei Festlegung auf **TRUE** die Beschreibung des exakten Verhaltens des Runbooks unterstützt, wenn es ohne den Parameter *WhatIf* ausgeführt wird, und der überprüft, ob die richtigen VMs als Ziel verwendet werden.  Runbooks führen die zugehörigen definierten Aktionen nur aus, wenn der Parameter *WhatIf* auf **FALSE** festgelegt ist.  


### <a name="scenario-1-daily-stopstart-vms-across-a-subscription-or-target-resource-groups"></a>Szenario 1: Tägliches Beenden/Starten von VMs in einem Abonnement oder in Zielressourcengruppen 

Dies ist die Standardkonfiguration bei der ersten Bereitstellung der Lösung.  Beispielsweise können Sie sie so konfigurieren, dass alle VMs in einem Abonnement zum Feierabend beendet und am Morgen wieder gestartet werden, wenn Sie wieder ins Büro kommen. Wenn Sie die Zeitpläne **Scheduled-StartVM** und **Scheduled-StopVM** während der Bereitstellung konfigurieren, werden Ziel-VMs nach diesen Zeitplänen gestartet und beendet.
>[!NOTE]
>Die Zeitzone entspricht Ihrer aktuellen Zeitzone zum Zeitpunkt der Konfiguration des Parameters für den geplanten Zeitpunkt. Sie wird jedoch in Azure Automation im UTC-Format gespeichert.  Sie müssen keine Zeitzonenkonvertierung durchführen, da dies während der Bereitstellung verarbeitet wird.

Sie steuern die entsprechenden VMs, indem Sie die zwei Variablen **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** sowie **External_ExcludeVMNames** konfigurieren.  

>[!NOTE]
> Der Wert für die Variable **Target ResourceGroup Names** wird als Wert für die beiden Variablen **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupNames** gespeichert. Für weitere Granularität können Sie jede dieser Variablen so ändern, dass sie für unterschiedliche Ressourcengruppen gelten.  Verwenden Sie **External_Start_ResourceGroupNames** für Startaktionen und **External_Stop_ResourceGroupNames** für Beendigungsaktionen. Neue VMs werden automatisch den Zeitplänen zum Starten und Beenden hinzugefügt.

Um die Konfiguration zu testen und zu überprüfen, starten Sie das Runbook **ScheduledStartStop_Parent** manuell, und legen Sie den Parameter *ACTION* auf **start** oder **stop** und den Parameter *WhatIf* auf **TRUE** fest.<br><br> ![Konfigurieren von Parametern für Runbooks](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Dadurch können Sie die durchgeführte Aktion in der Vorschau anzeigen und bei Bedarf Änderungen vornehmen, bevor Sie sie auf VMs in der Produktionsumgebung implementieren.  Wenn Sie mit dem Ergebnis zufrieden sind, können Sie das Runbook manuell ausführen, wobei Sie den Parameter auf **FALSE** festlegen. Oder Sie können die Automation-Zeitpläne **Schedule-StartVM** und **Schedule-StopVM** entsprechend Ihrem vorgegebenen Zeitplan automatisch ausführen.

### <a name="scenario-2-sequence-the-stopstart-vms-across-a-subscription-using-tags"></a>Szenario 2: Festlegen der Reihenfolge zum Beenden/Starten von VMs in einem Abonnement mithilfe von Tags
In einer Umgebung mit mehreren Komponenten auf mehreren VMs, die eine verteilte Workload unterstützen, spielt die Reihenfolge, in der Komponenten nacheinander gestartet und beendet werden, eine wichtige Rolle.  Hierzu führen Sie die folgenden Schritte aus.

1. Fügen Sie den VMs in Ihrem Abonnement ein **SequenceStart**- und ein **SequenceStop**-Tag mit einer positiven ganzen Zahl hinzu, die in den Variablen **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupNames** als Ziel angegeben sind.  Die Aktionen zum Starten und Beenden werden in aufsteigender Reihenfolge durchgeführt.  Informationen zum Markieren von VMs finden Sie unter [Markieren eines virtuellen Windows-Computers in Azure](../virtual-machines/windows/tag.md) und [Markieren eines virtuellen Linux-Computers in Azure](../virtual-machines/linux/tag.md).
2. Ändern Sie die Zeitpläne **Sequenced-StartVM** und **Sequenced-StopVM** in das Datum und den Zeitpunkt, die Ihren Anforderungen entsprechen, und aktivieren Sie die Zeitpläne.  

Um die Konfiguration zu testen und zu überprüfen, starten Sie das Runbook **SequencedStartStop_Parent** manuell, und legen Sie den Parameter *ACTION* auf **start** oder **stop**  und den Parameter *WhatIf* auf **TRUE** fest.<br><br> ![Konfigurieren von Parametern für Runbooks](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Dadurch können Sie die durchgeführte Aktion in der Vorschau anzeigen und bei Bedarf Änderungen vornehmen, bevor Sie sie auf VMs in der Produktionsumgebung implementieren.  Wenn Sie mit dem Ergebnis zufrieden sind, können Sie das Runbook manuell ausführen, wobei Sie den Parameter auf **FALSE** festlegen. Oder Sie können die Automation-Zeitpläne **Sequenced-StartVM** und **Sequenced-StopVM** entsprechend Ihrem vorgegebenen Zeitplan automatisch ausführen.  

### <a name="scenario-3-auto-stopstart-vms-across-a-subscription-based-on-cpu-utilization"></a>Szenario 3: Automatisches Beenden/Starten von VMs in einem Abonnement basierend auf der CPU-Auslastung
Diese Lösung kann zur Verwaltung der Kosten für die Ausführung von VMs in Ihrem Abonnement beitragen, dadurch dass Azure-VMs ausgewertet werden, die außerhalb der Spitzenzeiten (z.B. nach Geschäftsschluss) nicht verwendet werden, sodass sie beendet werden, wenn die Prozessorauslastung unter x % liegt.  

Standardmäßig ist die Lösung so vorkonfiguriert, dass die Metrik zur CPU-Auslastung in Prozent sowie eine durchschnittliche Auslastung kleiner als oder gleich 5 % ausgewertet wird.  Dies wird durch die folgenden Variablen gesteuert und kann geändert werden, wenn die zugehörigen Standardwerte nicht Ihren Anforderungen entsprechen:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Sie können die Aktion entweder nur für ein Abonnement und eine Ressourcengruppe oder für eine spezifische Liste von VMs aktivieren, jedoch nicht für beides.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Festlegen der Beendigungsaktion für ein Abonnement und eine Ressourcengruppe

1. Konfigurieren Sie die Variablen **External_Stop_ResourceGroupNames** und **External_ExcludeVMNames**, um die Ziel-VMs anzugeben.  
2. Aktivieren und aktualisieren Sie den Zeitplan **Schedule_AutoStop_CreateAlert_Parent**.
3. Führen Sie das Runbook **AutoStop_CreateAlert_Parent** aus. Legen Sie dabei den Parameter *ACTION* auf **start** und den Parameter *WhatIf* auf **TRUE** fest, um die Änderungen in der Vorschau anzuzeigen.

#### <a name="target-stop-action-by-vm-list"></a>Festlegen der Beendigungsaktion über eine VM-Liste

1. Führen Sie das Runbook **AutoStop_CreateAlert_Parent** aus. Legen Sie dabei den Parameter *ACTION* auf **start** fest, fügen Sie eine durch Trennzeichen getrennte Liste von VMs im Parameter *VMList* hinzu, und legen Sie den Parameter *WhatIf* auf **TRUE** fest, um die Änderungen in der Vorschau anzuzeigen.  
2. Konfigurieren Sie den Parameter **External_ExcludeVMNames** mit einer durch Trennzeichen getrennten Liste von VMs (VM1,VM2,VM3).
3. In diesem Szenario werden die Variablen **External_Start_ResourceGroupNames** und **External_Stop_ResourceGroupnames** nicht berücksichtigt.  Für dieses Szenario müssen Sie Ihren eigenen Automation-Zeitplan erstellen. Ausführliche Informationen finden Sie unter [Planen eines Runbooks in Azure Automation](../automation/automation-schedules.md).

Nachdem Sie einen Zeitplan zum Beenden von VMs basierend auf der CPU-Auslastung erstellt haben, müssen Sie nun einen der folgenden Zeitpläne aktivieren, um sie zu starten.

* Festlegen der Startaktion nach Abonnement und Ressourcengruppe.  Informationen zum Testen und Aktivieren des Zeitplans **Scheduled-StartVM** finden Sie in den im [Szenario 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) beschriebenen Schritten.
* Festlegen der Startaktion nach Abonnement, Ressourcengruppe und Tag.  Informationen zum Testen und Aktivieren des Zeitplans „Sequenced-StartVM“ finden Sie in den im [Szenario 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) beschriebenen Schritten.


### <a name="configuring-e-mail-notifications"></a>Konfigurieren von E-Mail-Benachrichtigungen

Um E-Mail-Benachrichtigungen nach der Bereitstellung der Lösung zu konfigurieren, können Sie die folgenden drei Variablen ändern:

* External_EmailFromAddress: Angeben der E-Mail-Adresse des Absenders
* External_EmailToAddress: Eine durch Trennzeichen getrennte Liste mit E-Mail-Adressen (user@hotmail.com, user@outlook.com) zum Empfangen von E-Mail-Benachrichtigungen
* External_IsSendEmail: Wenn diese Variable auf **Yes** festgelegt ist, erhalten Sie E-Mails. Wenn die E-Mail-Benachrichtigungen deaktiviert werden sollen, legen Sie den Wert auf **No** fest.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Ändern der Zeitpläne für das Starten und Herunterfahren

Bei der Verwaltung der Zeitpläne für das Starten und Herunterfahren werden bei dieser Lösung die gleichen Schritte ausgeführt, die unter [Planen eines Runbooks in Azure Automation](automation-schedules.md) beschrieben werden.     

## <a name="log-analytics-records"></a>Log Analytics-Datensätze

Automation erstellt zwei Arten von Datensätzen im OMS-Repository.

### <a name="job-logs"></a>Auftragsprotokolle

Eigenschaft | Beschreibung|
----------|----------|
Aufrufer |  Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.|
Kategorie | Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobLogs“.|
CorrelationId | Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt.|
JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“.|
resourceId | Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
ResourceGroup | Gibt den Namen der Ressourcengruppe des Runbookauftrags an.|
ResourceProvider | Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können.  Für Automation lautet der Wert „Azure Automation“.|
ResourceType | Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
resultType | Der Status des Runbookauftrags.  Mögliche Werte:<br>- Gestartet<br>- Beendet<br>- Ausgesetzt<br>- Fehler<br>- Erfolgreich|
resultDescription | Beschreibt den resultierenden Zustand des Runbookauftrags.  Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen|
RunbookName | Gibt den Namen des Runbooks an.|
SourceSystem | Gibt das Quellsystem für die gesendeten Daten an.  Für Automation lautet der Wert „:OpsManager“.|
StreamType | Gibt den Typ des Ereignisses an. Mögliche Werte:<br>- Ausführlich<br>- Ausgabe<br>- Fehler<br>- Warnung|
SubscriptionId | Gibt die Abonnement-ID des Auftrags an.
Time | Ausführungsdatum und -uhrzeit des Runbookauftrags.|


### <a name="job-streams"></a>Auftragsdatenströme

Eigenschaft | Beschreibung|
----------|----------|
Aufrufer |  Der Benutzer oder das System, von dem der Vorgang initiiert wurde.  Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System.|
Kategorie | Klassifizierung des Datentyps.  Für Automation lautet der Wert „JobStreams“.|
JobId | Die GUID, bei der es sich um die ID des Runbookauftrags handelt.|
operationName | Gibt den Typ des in Azure ausgeführten Vorgangs an.  Für Automation lautet der Wert „Job“.|
ResourceGroup | Gibt den Namen der Ressourcengruppe des Runbookauftrags an.|
resourceId | Gibt die Ressourcen-ID in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
ResourceProvider | Gibt den Azure-Dienst an, über den Sie die Ressourcen beziehen, die Sie bereitstellen und verwalten können.  Für Automation lautet der Wert „Azure Automation“.|
ResourceType | Gibt den Ressourcentyp in Azure an.  Für Automation ist der Wert das dem Runbook zugeordnete Automation-Konto.|
resultType | Das Ergebnis des Runbookauftrags zum Zeitpunkt der Ereignisgenerierung.  Mögliche Werte:<br>- InProgress|
resultDescription | Enthält den Ausgabedatenstrom des Runbooks.|
RunbookName | Der Name des Runbooks.|
SourceSystem | Gibt das Quellsystem für die gesendeten Daten an.  Für Automation lautet der Wert „OpsManager“.|
StreamType | Der Typ des Auftragsstreams. Mögliche Werte:<br>- Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich|
Time | Ausführungsdatum und -uhrzeit des Runbookauftrags.|

Wenn Sie Protokollsuchen durchführen, bei denen Datensätze der Kategorie **JobLogs** oder **JobStreams** zurückgegeben werden, können Sie die Ansicht **JobLogs** oder **JobStreams** wählen. Es werden dann mehrere Kacheln angezeigt, mit denen die von der Suche zurückgegebenen Updates zusammengefasst werden.

## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen

Die folgende Tabelle enthält Beispiele für Protokollsuchen für Auftragsdatensätze, die mit dieser Lösung erfasst wurden. 

Abfrage | Beschreibung|
----------|----------|
Suchen nach Aufträgen für das Runbook „ScheduledStartStop_Parent“, die erfolgreich abgeschlossen wurden | search Category == "JobLogs" &#124; where ( RunbookName_s == "ScheduledStartStop_Parent" ) &#124; where ( ResultType == "Completed" )  &#124; summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) &#124; sort by TimeGenerated desc|
Suchen nach Aufträgen für das Runbook „SequencedStartStop_Parent“, die erfolgreich abgeschlossen wurden | search Category == "JobLogs" &#124; where ( RunbookName_s == "SequencedStartStop_Parent" ) &#124; where ( ResultType == "Completed" )  &#124; summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) &#124; sort by TimeGenerated desc

## <a name="removing-the-solution"></a>Entfernen der Lösung

Wenn Sie die Lösung nicht mehr brauchen sollten, können Sie sie aus dem Automation-Konto löschen.  Beim Löschen der Lösung werden nur die Runbooks entfernt. Die Zeitpläne oder Variablen, die beim Hinzufügen der Lösung erstellt wurden, werden nicht gelöscht.  Diese Objekte müssen Sie manuell löschen, wenn Sie sie nicht mit anderen Runbooks verwenden.  

Führen Sie die folgenden Schritte aus, um die Lösung zu löschen:

1.  Wählen Sie in Ihrem Automation-Konto im linken Bereich die Option **Arbeitsbereich** aus.  
2.  Wählen Sie auf der Seite **Lösungen** die Lösung **Start-Stop-VM[Workspace]** aus.  Klicken Sie auf der Seite **VMManagementSolution[Workspace]** im Menü auf **Löschen**.<br><br> ![Löschen der VM-Mgmt-Lösung](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  Bestätigen Sie im Fenster **Lösung löschen**, dass Sie die Lösung löschen möchten.
4.  Während die Informationen überprüft werden und die Lösung gelöscht wird, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.  Nachdem der Vorgang zum Löschen der Lösung begonnen hat, kehren Sie zur Seite **Lösungen** zurück.  

Das Automation-Konto und der Log Analytics-Arbeitsbereich werden bei diesem Vorgang nicht gelöscht.  Wenn Sie den Log Analytics-Arbeitsbereich nicht beibehalten möchten, müssen Sie ihn manuell löschen.  Dies kann auch im Azure-Portal erfolgen.   Wählen Sie im Azure-Portal auf der Startseite **Log Analytics** und dann auf dem Blatt **Log Analytics** den Arbeitsbereich aus. Klicken Sie dann auf dem Blatt mit den Arbeitsbereichseinstellungen im Menü auf **Löschen**.  
      
## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation-Auftragsprotokolle mit Log Analytics finden Sie unter [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md)
- Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md)
- Weitere Informationen zu Log Analytics und Datenerfassungsquellen finden Sie unter [Sammeln von Azure-Speicherdaten in Log Analytics – Übersicht](../log-analytics/log-analytics-azure-storage.md)






   

