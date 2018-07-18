---
title: Überwachung und Problembehandlung in Azure Site Recovery | Microsoft-Dokumentation
description: Überwachung und Problembehandlung bei Replikationsproblemen und Vorgängen in Azure Site Recovery mithilfe des Portals
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: bsiva
ms.openlocfilehash: b357a3231dac6dfa54cb02fe921baf771c0880f4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
ms.locfileid: "30186290"
---
# <a name="monitoring-and-troubleshooting-azure-site-recovery"></a>Überwachung und Problembehandlung in Azure Site Recovery

In diesem Artikel erfahren Sie, wie Sie die integrierten Überwachungsfunktionen von Azure Site Recovery für Überwachung und Problembehandlung nutzen. In diesem Artikel werden folgende Themen erläutert:
> [!div class="checklist"]
> - Verwenden des Azure Site Recovery-Dashboards (Tresorübersichtsseite)
> - Überwachung und Behandlung von Replikationsproblemen
> - Überwachen von Aufträgen/Vorgängen in Azure Site Recovery
> - Abonnieren von E-Mail-Benachrichtigungen

## <a name="using-the-azure-site-recovery-dashboard"></a>Verwenden des Azure Site Recovery-Dashboards

Im Azure Site Recovery-Dashboard auf der Tresorübersichtsseite werden alle Überwachungsinformationen für den Tresor an einem zentralen Ort konsolidiert. Gehen Sie vom Tresordashboard ausgehend immer weiter ins Detail, indem Sie durch die einzelnen Bereiche des Dashboards navigieren. Die Hauptteile des Azure Site Recovery-Dashboards sind:

### <a name="1-switch-between-azure-backup-and-azure-site-recovery-dashboards"></a>1. Wechseln zwischen den Dashboards von Azure Backup und Azure Site Recovery

Mit dem Umschalter am oberen Rand der Übersichtsseite können Sie zwischen den Dashboardseiten für Site Recovery und Backup wechseln. Wenn Sie diese Auswahl einmal vorgenommen haben, wird sie gespeichert und als Standard verwendet, wenn Sie die Übersichtsseite für den Tresor das nächste Mal öffnen. Wählen Sie die Option „Site Recovery“ aus, um das Site Recovery-Dashboard anzuzeigen. 

Die verschiedenen Teile der Azure Site Recovery-Dashboardseite werden alle 10 Minuten automatisch aktualisiert, sodass das Dashboard die neuesten verfügbaren Informationen widerspiegelt.

![Überwachungsfunktionen in der Übersichtsseite von Azure Site Recovery](media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)

### <a name="2-replicated-items"></a>2. Replizierte Elemente

Der Abschnitt des Dashboards für replizierte Elemente bietet eine Übersicht über die Replikationsintegrität der geschützten Server im Tresor. 

<table>
<tr>
    <td>Healthy</td>
    <td>Die Replikation verläuft für diese Server normal, und weder Fehler noch Warnungssymptome wurden erkannt.</td>
</tr>
<tr>
    <td>Warnung </td>
    <td>Es wurde mindestens ein Warnungssymptom für diese Server erkannt, das möglicherweise Auswirkungen auf die Replikation hat oder darauf hinweist, dass diese Replikation nicht normal verläuft.</td>
</tr>
<tr>
    <td>Kritisch</td>
    <td>Mindestens ein Symptom eines schwerwiegenden Replikationsfehlers wurde für diese Server erkannt. Diese Fehlersymptome weisen in der Regel darauf hin, dass die Replikation entweder hängen geblieben ist oder nicht so schnell verläuft, wie die Datenänderung für diesen Server fortschreitet.</td>
</tr>
<tr>
    <td>Nicht zutreffend</td>
    <td>Server, für die derzeit keine Replikation anliegt, z.B. Server, für die ein Failover durchgeführt wurde.</td>
</tr>
</table>

Um eine nach Replikationsintegrität gefilterte Liste der geschützten Server anzuzeigen, klicken Sie auf die Beschreibung der Replikationsintegrität neben dem Ringdiagramm. Der „Alle anzeigen“-Link in der Nähe des Abschnittstitels ist eine Verknüpfung zu der Seite für replizierte Elemente für den Tresor. Verwenden Sie den „Alle anzeigen“-Link, um eine Liste aller Server im Tresor anzuzeigen.

### <a name="3-failover-test-success"></a>3. Failovertest erfolgreich

Im Abschnitt „Failovertest erfolgreich“ des Dashboards wird eine Auflistung der virtuellen Computern im Tresor auf Basis des Testfailoverstatus angezeigt. 

<table>
<tr>
    <td>Test empfohlen</td>
    <td>Virtuelle Computer, bei denen kein erfolgreiches Testfailover ausgeführt wurde, seit sie einen geschützten Zustand erreicht haben.</td>
</tr>
<tr>
    <td>Erfolgreich ausgeführt</td>
    <td>Virtuelle Computer, bei denen mindestens ein erfolgreiches Testfailover ausgeführt wurde.</td>
</tr>
<tr>
    <td>Nicht zutreffend</td>
    <td>Virtuelle Computer, die derzeit nicht zu einem Testfailover berechtigt sind. Beispiele sind: Server, für die ein Failover ausgeführt wurde; Server, für die derzeit die erste Replikation ausgeführt wird; Server, für die derzeit ein Failover ausgeführt wird; Server, für die derzeit bereits ein Testfailover ausgeführt wird.</td>
</tr>
</table>

Klicken Sie auf den Testfailoverstatus neben dem Ringdiagramm, um die Liste geschützter Server auf der Basis ihres Testfailoverstatus anzuzeigen.
 
> [!IMPORTANT]
> Es hat sich als Methode bewährt, mindestens einmal alle sechs Monate ein Testfailover auf den geschützten Servern auszuführen. Durch Ausführen eines Testfailovers können Sie unterbrechungsfrei das Failover Ihrer Server und Anwendungen auf eine isolierte Umgebung testen und so Ihre Geschäftskontinuitätsvorbereitung besser bewerten.

 Ein Testfailovervorgang auf einem Server oder ein Wiederherstellungsplan wird nur dann als erfolgreich betrachtet, nachdem der Testfailover- und der Testfailoverbereinigungsvorgang erfolgreich abgeschlossen wurden.

### <a name="4-configuration-issues"></a>4. Konfigurationsprobleme

Im Abschnitt „Konfigurationsprobleme“ wird eine Liste der Probleme angezeigt, die Ihre Fähigkeit, ein erfolgreiches Failover virtueller Computer auszuführen, beeinträchtigen können. Folgende Klassen von Problemen werden in diesem Abschnitt aufgeführt:
 - **Fehlende Konfigurationen:** Geschützte Server, denen die erforderlichen Konfigurationen wie z.B. ein Wiederherstellungsnetzwerk oder eine Wiederherstellungsressourcengruppe fehlen.
 - **Fehlende Ressourcen:** Konfigurierte Ziel-/Wiederherstellungsressourcen wurden nicht gefunden oder sind nicht im Abonnement verfügbar. Beispielsweise wurde die Ressource gelöscht oder zu einem anderen Abonnement oder einer anderen Ressourcengruppe migriert. Die folgenden Ziel-/Wiederherstellungskonfigurationen werden für die Verfügbarkeit überwacht: Zielressourcengruppe, virtuelles Zielnetzwerk und Subnetz, Protokoll-/Zielspeicherkonto, Zielverfügbarkeitsgruppe, Ziel-IP-Adresse.
 - **Abonnementkontingent:** Die verfügbare Abonnementressourcenkontingent-Bilanz wird mit der Bilanz verglichen, die erforderlich ist, um ein Failover aller virtuellen Computer im Tresor ausführen zu können. Wenn die verfügbare Bilanz unzureichend ist, wird nicht genügend Kontingentbilanz gemeldet. Kontingente für die folgenden Azure-Ressourcen werden überwacht: Anzahl der VM-Kerne, Anzahl der VM-Familienkerne, Anzahl der Netzwerkadapter (NIC).
 - **Softwareupdates:** Die Verfügbarkeit neuer Softwareupdates, ablaufende Softwareversionen.

Konfigurationsprobleme (mit Ausnahme der Verfügbarkeit von Softwareupdates) werden durch einen periodischen Validierungssteuerelementvorgang erkannt, der standardmäßig alle 12 Stunden ausgeführt wird. Sie können erzwingen, dass der Validierungssteuerelementvorgang sofort ausgeführt wird, indem Sie auf das Aktualisierungssymbol neben der Abschnittsüberschrift *Konfigurationsprobleme* klicken.

Klicken Sie auf die Links, um weitere Informationen zu den aufgeführten Probleme und virtuellen Computern, die von ihnen betroffen sind, zu erhalten. Um Informationen zu Problemen zu erhalten, die Auswirkungen auf bestimmte virtuelle Computer haben, können Sie weitere Informationen abrufen, indem Sie auf den **Eingreifen erforderlich**-Link unter der Zielkonfigurationenspalte für den virtuellen Computer klicken. Die Details enthalten Vorschläge, wie Sie die erkannten Probleme beheben können.

### <a name="5-error-summary"></a>5. Fehlerzusammenfassung

Im Abschnitt „Fehlerzusammenfassung“ werden die derzeit aktiven Replikationsfehlersymptome, die die Replikation von Servern im Tresor beeinträchtigen könnten, sowie die Anzahl der von jedem Fehler betroffenen Entitäten angezeigt.

Die Replikationsfehlersymptome für Server in einem kritischen oder Warnungsreplikationsintegritäts-Status werden in der Fehlerzusammenfassungstabelle angezeigt. 

- Fehler, die Auswirkungen auf lokale Infrastrukturkomponenten haben, wie das Nichtempfangen eines Taktsignals von dem Azure Site Recovery-Anbieter, der auf dem lokalen Konfigurationsserver, VMM-Server oder Hyper-V-Host ausgeführt wird, werden am Anfang des Fehlerzusammenfassungsabschnitt aufgeführt.
- Replikationsfehlersymptome, die Auswirkungen auf geschützte Server haben, werden als nächstes aufgeführt. Die Einträge der Fehlerzusammenfassungstabelle werden in absteigender Reihenfolge nach dem Schweregrad des Fehlers und dann in absteigender Reihenfolge nach der Anzahl der betroffenen Server sortiert.
 

> [!NOTE]
> 
>  Auf einem einzelnen Server können mehrere Replikationsfehlersymptome beobachtet werden. Treten mehrere Fehlersymptome auf einem einzelnen Server auf, wird dieser Server für jedes Fehlersymptom in der Liste der von ihm betroffenen Servern gezählt. Sobald das einem Fehlersymptom zugrunde liegende Problem behoben ist, werden die Replikationsparameter verbessert, und der Fehler wird vom virtuellen Computer gelöscht.
>
> > [!TIP]
> Die Anzahl der betroffenen Server ist eine gute Möglichkeit, zu verstehen, ob ein einzelnes zugrunde liegendes Problem mehrere Server beeinträchtigen kann. Beispielsweise kann eine Netzwerkstörung potenziell alle Server beeinträchtigen, die von einem lokalen Standort in Azure replizieren. In dieser Ansicht ist schnell zu erkennen, dass die Behebung eines zugrunde liegenden Problems die Replikation für mehrere Server korrigiert.
>

### <a name="6-infrastructure-view"></a>6. Infrastrukturansicht

Die Infrastrukturansicht enthält eine visuelle Szenariodarstellung der Infrastrukturkomponenten, die an der Replikation beteiligt sind. Sie zeigt auch visuell die Integrität der Verbindungen zwischen den verschiedenen Servern sowie zwischen den Servern und den Azure-Diensten, die an der Replikation beteiligt sind. 

Eine grüne Linie gibt an, dass die Verbindung fehlerfrei ist, während eine rote Linie mit dem überlagerten Fehlersymbol auf mindestens ein Fehlersymptom hinweist, dass Auswirkungen auf die Konnektivität zwischen den beteiligten Komponenten hat. Wenn Sie den Mauszeiger auf das Fehlersymbol auf der Linie bewegen, werden der Fehler und die Anzahl der betroffenen Entitäten angezeigt. 

Durch Klicken auf das Fehlersymbol werden eine gefilterte Liste der betroffenen Entitäten für den/die Fehler angezeigt.

![Site Recovery-Infrastrukturansicht (Tresor)](media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

> [!TIP]
> Stellen Sie sicher, dass auf den lokalen Infrastrukturkomponenten (Konfigurationsserver, zusätzliche Prozessserver, replizierende VMware-VMs, Hyper-V-Hosts, VMM-Server) die neueste Version der Azure Site Recovery-Software ausgeführt wird. Um alle Features der Infrastrukturansicht nutzen zu können, müssen Sie [Update Rollup 22](https://support.microsoft.com/help/4072852) oder höher für Azure Site Recovery ausführen.

Um die Infrastrukturansicht zu verwenden, wählen Sie das entsprechende Replikationsszenario (virtuelle Azure-Computer, VMware-VMs/physische Server oder Hyper-V) abhängig von Ihrer Quellumgebung aus. Die in der Tresorübersichtsseite dargestellte Infrastrukturansicht ist eine aggregierte Ansicht für den Tresor. Sie können einen weiteren Drilldown in den einzelnen Komponenten ausführen, indem Sie auf die Felder klicken.

Eine auf den Kontext eines einzelnen replizierenden Computers beschränkte Infrastrukturansicht ist auf der Übersichtsseite des replizierten Elements verfügbar. Um zur Übersichtsseite für einen replizierenden Server zu wechseln, gehen Sie vom Tresormenü aus zu replizierten Elementen, und wählen Sie den Server aus, dessen Details Sie anzeigen möchten.

### <a name="infrastructure-view---faq"></a>Infrastrukturansicht – FAQ

**F.** Warum sehe ich nicht die Infrastrukturansicht für meinen virtuellen Computer? </br>
**A.** Das Feature Infrastrukturansicht ist nur für virtuelle Computer verfügbar, die in Azure replizieren. Das Feature ist derzeit nicht für virtuelle Computer verfügbar, die zwischen lokalen Standorten replizieren.

**F.** Warum unterscheidet sich die Anzahl der virtuellen Computer in der Tresorinfrastrukturansicht von der Gesamtzahl, die im Ringdiagramm der replizierten Elemente angezeigt wird?</br>
**A.** Die Tresorinfrastrukturansicht ist nach Replikationsszenarien begrenzt. Nur virtuelle Computer, die am aktuell ausgewählten Replikationsszenario beteiligt sind, werden in die Anzahl der virtuellen Computer einbezogen, die in der Infrastrukturansicht enthalten sind. Außerdem werden für das ausgewählte Szenario nur virtuelle Computer, die zurzeit zum Replizieren in Azure konfiguriert sind, in die Anzahl von virtuellen Computern einbezogen, die in der Infrastrukturansicht enthalten sind (Beispiel: virtuelle Computer, für die ein Failover ausgeführt wurde, virtuelle Computer, die zurück zu einem lokalen Standort replizieren, sind in der Infrastrukturansicht nicht enthalten).

**F.** Warum unterscheidet sich die Anzahl der replizierten Elemente im Zusammenfassungsfach auf der Übersichtsseite von der Gesamtzahl der auf dem Dashboard im Ringdiagramm angezeigten replizierten Elemente?</br>
**A.** Nur die virtuellen Computer, für die die erste Replikation abgeschlossen wurde, sind in der im Zusammenfassungsfach angezeigten Anzahl enthalten. Das Ringdiagramm der replizierten Elemente enthält insgesamt alle virtuellen Computer im Tresor, einschließlich der Server, für die die erste Replikation gerade ausgeführt wird.

**F.** Für welche Replikationsszenarien ist die Infrastrukturansicht verfügbar? </br>
**A.**
>[!div class="mx-tdBreakAll"]
>|Replikationsszenario  | VM-Status  | Infrastrukturansicht verfügbar  |
>|---------|---------|---------|
>|Virtuelle Computer, die zwischen zwei lokalen Standorten replizieren     | -        | Nein       |
>|Alle     | Failover ausgeführt         |  Nein        |
>|Virtuelle Computer, die zwischen zwei Azure-Regionen replizieren     | Derzeitige Ausführung der ersten Replikation oder geschützt         | Ja         |
>|VMware-VMs, die in Azure replizieren     | Derzeitige Ausführung der ersten Replikation oder geschützt        | Ja        |
>|VMware-VMs, die in Azure replizieren     | Virtuelle Computer, für die ein Failover ausgeführt wurde, die zu einem lokalen VMware-Standort zurück repliziert werden         | Nein         |
>|Hyper-V-VMs, die in Azure replizieren     | Derzeitige Ausführung der ersten Replikation oder geschützt        | Ja       |
>|Hyper-V-VMs, die in Azure replizieren     | Failover ausgeführt / Failback wird derzeit ausgeführt        |  Nein        |


### <a name="7-recovery-plans"></a>7. Wiederherstellungspläne

Im Abschnitt „Wiederherstellungspläne“ wird die Anzahl der Wiederherstellungspläne im Tresor angezeigt. Klicken Sie auf die Anzahl, um die Liste der Wiederherstellungspläne anzuzeigen, neue Wiederherstellungspläne zu erstellen oder vorhandene zu bearbeiten. 

### <a name="8-jobs"></a>8. Aufträge

Azure Site Recovery-Aufträge verfolgen den Status der Azure Site Recovery-Vorgänge. Die meisten Vorgänge in Azure Site Recovery werden asynchron ausgeführt, wobei ein Verfolgungsauftrag den Fortschritt des Vorgangs verfolgt.  Informationen zum Überwachen des Status eines Vorgangs finden Sie im Abschnitt [Überwachen von Aufträgen/Vorgängen in Azure Site Recovery](#monitor-azure-site-recovery-jobsoperations).

Dieser Abschnitt „Aufträge“ des Dashboards liefert folgende Informationen:

<table>
<tr>
    <td>Fehler</td>
    <td>Fehler bei Azure Site Recovery-Aufträgen in den letzten 24 Stunden</td>
</tr>
<tr>
    <td>Vorgang wird ausgeführt</td>
    <td>Azure Site Recovery-Aufträge, die gerade ausgeführt werden.</td>
</tr>
<tr>
    <td>Warten auf Eingabe</td>
    <td>Azure Site Recovery-Aufträge, die zurzeit angehalten sind und auf Benutzereingabe warten.</td>
</tr>
</table>

Der „Alle anzeigen“-Link neben der Abschnittsüberschrift ist eine Verknüpfung, um zur Aufträgelistenseite zu wechseln.

## <a name="monitor-and-troubleshoot-replication-issues"></a>Überwachung und Behandlung von Replikationsproblemen

Zusätzlich zu den Informationen auf der Tresordashboardseite können Sie weitere Details und Informationen zur Problembehandlung auf der Listenseite für virtuelle Computer und der Detailseite des virtuellen Computers erfahren. Sie können die Liste der geschützten virtuellen Computer im Tresor anzeigen, indem Sie die Option **Replizierte Elemente** im Tresormenü auswählen. Alternativ können Sie durch Klicken auf die bereichsbezogenen Verknüpfungen auf der Tresordashboardseite eine gefilterte Liste der geschützten Elemente abrufen.

![Listenansicht der replizierten Elemente in Site Recovery](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

Mit der Filteroption auf der Listenseite für replizierte Elemente können Sie verschiedene Filter anwenden, z.B. Replikationsintegrität und Replikationsrichtlinie. 

Mit der Spaltenauswahloption können Sie zusätzliche Spalten zum Anzeigen angeben, z.B. RPO, Zielkonfigurationsprobleme und Replikationsfehler. Sie können Vorgänge auf einem virtuellen Computer einleiten oder Fehler anzeigen, die Auswirkungen auf den virtuellen Computer haben, indem Sie mit der rechten Maustaste in der Liste der Computer auf eine bestimmte Zeile klicken.

Wählen Sie für einen weiteren Drilldown einen virtuellen Computer aus, indem Sie darauf klicken. Daraufhin wird die Detailseite des virtuellen Computers geöffnet. Die Übersichtsseite unter den Details des virtuellen Computers enthält ein Dashboard, wo Sie zusätzliche Informationen zu dem Computer finden. 

Auf der Übersichtsseite für den replizierten Computer finden Sie:
- RPO (Recovery Point Objective): Aktuelle RPO für den virtuellen Computer und die Uhrzeit, an dem die RPO zuletzt berechnet wurde.
- Neueste verfügbare Wiederherstellungspunkte für den Computer
- Ggf. Konfigurationsprobleme, die die Failoverbereitschaft des Computers beeinträchtigen könnten. Klicken Sie auf den Link, um weitere Details zu erhalten.
- Fehlerdetails: Liste der Replikationsfehlersymptome, die derzeit auf dem Computer beobachtet werden, sowie deren mögliche Ursachen und empfohlenen Wiederherstellungsmaßnahmen
- Ereignisse: Eine chronologische Liste der aktuellen Ereignisse, die den Computer beeinträchtigen. Während die Fehlerdetails die derzeit auf dem Computer feststellbaren Fehlersymptome anzeigen, sind die Ereignisse eine historische Aufzeichnung verschiedener Ereignisse, die möglicherweise Auswirkungen auf den Computer gehabt haben, einschließlich Fehlersymptome, die bereits zuvor bei dem Computer festgestellt wurden.
- Infrastrukturansicht für Computer, die in Azure replizieren

![Details/Übersicht der replizierten Elemente in Site Recovery](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

Das Aktionsmenü am oberen Rand der Seite bietet Optionen, um verschiedene Vorgänge auf dem virtuellen Computer durchzuführen, z.B. ein Testfailover. Mit der Fehlerdetailsschaltfläche im Aktionsmenü können Sie alle aktiven Fehler einschließlich Replikationsfehlern, Konfigurationsproblemen und auf bewährten Konfigurationsmethoden basierender Konfigurationswarnungen für den virtuellen Computer anzeigen.

> [!TIP]
> Wie unterscheidet sich die RPO vom letzten verfügbaren Wiederherstellungspunkt?
> 
>Azure Site Recovery verwendet einen mehrstufigen asynchronen Prozess zum Replizieren virtueller Computer in Azure. Im vorletzten Schritt der Replikation werden aktuelle Änderungen auf dem virtuellen Computer sowie Metadaten in ein Protokoll-/Cachespeicherkonto kopiert. Nachdem diese Änderungen zusammen mit dem Tag zum Identifizieren eines wiederherstellbaren Punkts in das Speicherkonto in der Zielregion geschrieben wurden, besitzt Azure Site Recovery die erforderlichen Informationen zum Generieren eines wiederherstellbaren Punkts für den virtuellen Computer. An diesem Punkt ist die RPO für die Änderungen erfüllt, die bisher in das Speicherkonto hochgeladen wurden. Mit anderen Worten: Die RPO für den virtuellen Computer entspricht an diesem Punkt in Zeiteinheiten ausgedrückt der Zeitspanne, die seit dem Zeitstempel verstrichen ist, der dem wiederherstellbaren Punkt entspricht.
>
>Der im Hintergrund ausgeführte Azure Site Recovery-Dienst wählt die hochgeladenen Daten im Speicherkonto aus und wendet sie auf die Replikatdatenträger an, die für den virtuellen Computer erstellt wurden. Er generiert dann einen Wiederherstellungspunkt und macht diesen Punkt beim Failover für eine Wiederherstellung verfügbar. Der letzte verfügbare Wiederherstellungspunkt gibt den Zeitstempel an, der dem letzten Wiederherstellungspunkt entspricht, der bereits verarbeitet und auf die Replikatdatenträger angewendet wurde.
>> [!WARNING]
> Eine abweichende Uhr oder falsche Systemzeit auf dem replizierenden Quellcomputer oder den lokalen Infrastrukturservern verfälscht den berechneten RPO-Wert. Um sicherzustellen, dass die RPO-Werte genau berichtet werden, achten Sie darauf, dass die Systemuhr auf den an der Replikation beteiligten Servern korrekt eingestellt ist. 
>

## <a name="monitor-azure-site-recovery-jobsoperations"></a>Überwachen von Aufträgen/Vorgängen in Azure Site Recovery

Azure Site Recovery führt die Vorgänge, die Sie angeben, asynchron aus. Beispiele für Vorgänge, die Sie ausführen können, sind Aktivieren der Replikation, Erstellen eines Wiederherstellungsplans, Testen des Failovers, Aktualisieren der Replikationseinstellungen usw. Für alle derartigen Vorgänge wird ein entsprechender Auftrag erstellt, um den Vorgang zu verfolgen und zu überwachen. Das Auftragsobjekt verfügt über alle erforderlichen Informationen, die Sie zum Nachverfolgen von Status und Fortschritt des Vorgangs benötigen. Sie können den Status der verschiedenen Site Recovery-Vorgänge für den Tresor auf der Seite „Aufträge“ verfolgen. 

Um die Liste der Site Recovery-Aufträge für den Tresor anzuzeigen, wechseln Sie zum Abschnitt **Überwachung und Berichte** des Tresormenüs, und wählen Sie „Aufträge > Site Recovery-Aufträge“. Wählen Sie einen Auftrag aus der Liste der Aufträge auf der Seite aus, indem Sie darauf klicken, um weitere Details zum angegebenen Auftrag zu erfahren. Wenn ein Auftrag nicht erfolgreich abgeschlossen wurde oder Fehler aufweist, finden Sie weitere Informationen zu dem Fehler und möglichen Abhilfemaßnahmen durch Klicken auf die Fehlerdetailsschaltfläche am oberen Rand der Auftragsdetailsseite (Zugriff ist auch über die Listenseite „Aufträge“ durch Klicken mit der rechten Maustaste auf den nicht erfolgreichen Auftrag möglich.) Mit der Filteroption im Aktionsmenü oben auf der Auftragslistenseite können Sie die Liste nach bestimmten Kriterien filtern und mit der Exportschaltfläche Details der ausgewählten Aufträge nach Excel exportieren. Sie können auch über die Verknüpfung auf der Site Recovery-Dashboardseite auf die Aufträgelistenansicht zugreifen. 

 Für Vorgänge, die Sie vom Azure-Portal aus ausführen, können Sie den erstellten Auftrag und seinen aktuellen Status auch vom Benachrichtigungenabschnitt (Glockensymbol oben rechts) des Azure-Portals aus nachverfolgen.

## <a name="subscribe-to-email-notifications"></a>Abonnieren von E-Mail-Benachrichtigungen

Mit der integrierten E-Mail-Benachrichtigungsfunktion können Sie den Empfang von E-Mail-Benachrichtigungen zu kritischen Ereignissen abonnieren. Wenn Sie abonniert haben, werden E-Mail-Benachrichtigungen für die folgenden Ereignisse gesendet:
- Die Replikationsintegrität eines replizierenden Computer ist so beeinträchtigt, dass ein kritischer Zustand erreicht ist.
- Keine Konnektivität zwischen den lokalen Infrastrukturkomponenten und Azure Site Recovery-Dienst. Konnektivität zwischen Site Recovery-Dienst und lokalen, im Tresor registrierten Infrastrukturkomponenten wie Konfigurationsserver (VMware) oder System Center Virtual Machine Manager (Hyper-V) wird mithilfe eines Taktmechanismus erkannt.
- Ggf. Fehler beim Failovervorgang.

Um den Empfang von E-Mail-Benachrichtigungen für Azure Site Recovery zu abonnieren, wechseln Sie zum Abschnitt **Überwachung und Berichte** des Tresormenüs, und:
1. Wählen Sie „Warnungen und Ereignisse > Site Recovery-Ereignisse“ aus.
2. Wählen Sie „E-Mail-Benachrichtigungen“ aus dem Menü oben auf der Seite „Ereignisse“, die geöffnet wird.
3. Verwenden Sie den Assistenten für E-Mail-Benachrichtigungen, um E-Mail-Benachrichtigungen zu aktivieren oder deaktivieren und Empfänger für die Benachrichtigungen auszuwählen. Sie können angeben, dass Benachrichtigungen an alle Abonnementadministratoren gesendet werden, und/oder eine Liste der E-Mail-Adressen angeben, an die Benachrichtigungen gesendet werden. 
