---
title: "Häufig gestellte Fragen zur Plattformmigration von Security Center | Microsoft-Dokumentation"
description: In diesem FAQ-Dokument werden Fragen zur Plattformmigration von Azure Security Center beantwortet.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2017
ms.author: terrylan
ms.openlocfilehash: 69d0c368eb11953d1a6e954990a3be10df7044f0
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="security-center-platform-migration-faq"></a>Häufig gestellte Fragen zur Plattformmigration von Security Center
Seit Anfang Juni 2017 verwendet Azure Security Center den Microsoft Monitoring Agent zum Sammeln und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). In diesem FAQ-Dokument werden Fragen zur Plattformmigration beantwortet.

## <a name="data-collection-agents-and-workspaces"></a>Datensammlung, Agents und Arbeitsbereiche

### <a name="how-is-data-collected"></a>Wie werden Daten gesammelt?
Security Center verwendet den Microsoft Monitoring Agent, um Daten von Ihren virtuellen Computern zu sammeln. Die Sicherheitsdaten enthalten Informationen zu:

- Sicherheitskonfigurationen – zum Identifizieren von Sicherheitsrisiken
- Sicherheitsereignisse – zum Erkennen von Bedrohungen

Vom Agent gesammelte Daten werden entweder in einem vorhandenen, mit dem virtuellen Computer verbundenen Log Analytics-Arbeitsbereich oder in einem neuen, von Security Center erstellten Arbeitsbereich gespeichert. Bei der Erstellung eines neuen Arbeitsbereichs wird der Standort des virtuellen Computers berücksichtigt.

> [!NOTE]
> Der Microsoft Monitoring Agent ist der gleiche Agent, der auch von der Operations Management Suite (OMS), vom Log Analytics-Dienst und von System Center Operations Manager (SCOM) verwendet wird.
>
>

Wenn die (früher als Protokollsammlung bezeichnete) automatische Bereitstellung aktiviert wird oder Ihre Abonnements migriert werden, überprüft Security Center, ob der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf den einzelnen VMs installiert ist. Ist der Microsoft Monitoring Agent nicht installiert, führt Security Center standardmäßig Folgendes aus:

- Die Microsoft Monitoring Agent-Erweiterung wird auf der VM installiert.

   - Wenn am Standort der VM bereits ein von Security Center erstellter Arbeitsbereich vorhanden ist, wird der Agent mit diesem Arbeitsbereich verbunden.
   - Ist kein Arbeitsbereich vorhanden, erstellt Security Center eine neue Ressourcengruppe und einen Standardarbeitsbereich an diesem Standort und verbindet den Agent mit diesem Arbeitsbereich. Für den Arbeitsbereich und die Ressourcengruppe gilt folgende Namenskonvention:

       Arbeitsbereich: DefaultWorkspace-[Abonnement-ID]-[Region]

       Ressourcengruppe: DefaultResouceGroup-[Region]

- Aktivieren Sie im Arbeitsbereich eine Security Center-Lösung gemäß dem Tarif, der der VM in Security Center zugeordnet ist. Weitere Informationen zu Preisen finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/).
- Nur für migrierte Abonnements entfernt Security Center auch den vorherigen Azure Monitoring Agent.

> [!NOTE]
> Sie können die automatische Installation des Microsoft Monitoring Agent außer Kraft setzen und Ihren eigenen Arbeitsbereich verwenden.  Lesen Sie [Wie verhindere ich die automatische Agent-Installation und die Arbeitsbereicherstellung?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation) und [Wie kann ich meinen vorhandenen Log Analytics-Arbeitsbereich verwenden?](#how-can-i-use-my-existing-log-analytics-workspace).
>
>

Der Standort des Arbeitsbereichs basiert auf dem Standort des virtuellen Computers. Weitere Informationen finden Sie unter [Azure Security Center-Datensicherheit](security-center-data-security.md). Wenn ein Abonnement VMs aus mehreren Geolocations enthält, erstellt Security Center mehrere Arbeitsbereiche. Mehrere Arbeitsbereiche werden erstellt, um Datenschutzregeln zu verwalten.

> [!NOTE]
> Vor der Plattformmigration hat Security Center Sicherheitsdaten Ihrer virtuellen Computer mithilfe des Azure Monitoring Agents gesammelt und in Ihrem Speicherkonto gespeichert. Nach der Plattformmigration sammelt und speichert Security Center die gleichen Daten mithilfe des Microsoft Monitoring Agents und des Arbeitsbereichs. Das Speicherkonto kann nach der Migration entfernt werden.  Security Center entfernt auch zuvor installierte Azure Monitoring Agents nach der Plattformmigration.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-the-workspaces-created-by-security-center"></a>Werden mir die von Security Center erstellten Arbeitsbereiche im Rahmen von Log Analytics oder OMS in Rechnung gestellt?
Nein. Von Security Center erstellte Arbeitsbereiche sind zwar für die knotenbasierte Abrechnung von OMS konfiguriert, es fallen jedoch keine OMS-Kosten an. Die Abrechnung von Security Center basiert immer auf Ihrer Security Center-Sicherheitsrichtlinie und den installierten Lösungen in einem Arbeitsbereich:

- **Free-Tarif**: Security Center aktiviert die Lösung „SecurityCenterFree“ im Standardarbeitsbereich. Für den Free-Tarif entstehen keine Kosten.
- **Standard-Tarif**: Security Center aktiviert die Lösung „Security“ im Standardarbeitsbereich.

Weitere Informationen zu Preisen finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/). Auf der Seite mit den Preisinformationen werden Änderungen an der Speicherung von Sicherheitsdaten und die anteilsmäßige Abrechnung ab Juni 2017 berücksichtigt.

> [!NOTE]
> Der OMS-Tarif von Arbeitsbereichen, die von Security Center erstellt wurden, hat keine Auswirkungen auf die Security Center-Abrechnung.
>
>

### <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>Wann kommt eine VM für die automatische Bereitstellung der Microsoft Monitoring Agent-Installation infrage?
Windows- oder Linux-IaaS-VMs kommen unter folgenden Voraussetzungen infrage:

- Die Microsoft Monitoring Agent-Erweiterung ist derzeit nicht auf der VM installiert.
- Die VM wird ausgeführt.
- Der Windows- oder Linux-VM-Agent ist installiert.
- Die VM wird nicht als Appliance wie z.B. Webanwendungsfirewall oder Firewall der nächsten Generation verwendet.

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Kann ich die von Security Center erstellten Standardarbeitsbereiche löschen?
**Das Löschen des Standardarbeitsbereichs wird nicht empfohlen.** Security Center verwendet die Standardarbeitsbereiche, um Sicherheitsdaten Ihrer virtuellen Computer zu speichern.  Wenn Sie einen Arbeitsbereich löschen, kann Security Center diese Daten nicht erfassen, und einige Sicherheitsempfehlungen und Warnungen sind nicht verfügbar.

Entfernen Sie zur Wiederherstellung den Microsoft Monitoring Agent auf den virtuellen Computern, die mit dem gelöschten Arbeitsbereich verbunden sind. Security Center installiert den Agent erneut und erstellt neue Standardarbeitsbereiche.

### <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Wie kann ich meinen vorhandenen Log Analytics-Arbeitsbereich verwenden?

Sie können einen vorhandenen Log Analytics-Arbeitsbereich zum Speichern der von Security Center gesammelten Daten auswählen. Wenn Sie Ihren vorhandenen Log Analytics-Arbeitsbereich verwenden möchten, gilt Folgendes:

- Der Arbeitsbereich muss Ihrem ausgewählten Azure-Abonnement zugeordnet sein.
- Sie müssen mindestens über Leseberechtigungen verfügen, um auf den Arbeitsbereich zugreifen zu können.

So wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus:

1. Wählen Sie unter **Sicherheitsrichtlinie – Datensammlung** die Option **Use another workspace** (Anderen Arbeitsbereich verwenden) aus.

   ![Verwenden eines anderen Arbeitsbereichs][5]

2. Wählen Sie im Pulldownmenü einen Arbeitsbereich zum Speichern der gesammelten Daten aus.

   > [!NOTE]
   > Das Pulldownmenü enthält nur Arbeitsbereiche, auf die Sie Zugriff haben und die sich in Ihrem Azure-Abonnement befinden.
   >
   >

3. Wählen Sie **Speichern** aus.
4. Nach dem Klicken auf **Speichern** werden Sie gefragt, ob Sie überwachte virtuelle Computer neu konfigurieren möchten.

   - Klicken Sie auf **Nein**, wenn die neuen Arbeitsbereichseinstellungen **nur auf neue virtuelle Computer angewendet werden sollen**. Die neuen Arbeitsbereichseinstellungen gelten nur für neue Agent-Installationen (neu ermittelte virtuelle Computer, auf denen Microsoft Monitoring Agent nicht installiert ist).
   - Klicken Sie auf **Ja**, wenn die neuen Arbeitsbereichseinstellungen **auf alle virtuellen Computer angewendet werden sollen**. Darüber hinaus wird jeder virtuelle Computer, der mit einem von Security Center erstellten Arbeitsbereich verbunden ist, nun mit dem neuen Zielarbeitsbereich verbunden.

   > [!NOTE]
   > Wenn Sie auf „Ja“ klicken, dürfen die von Security Center erstellten Arbeitsbereiche erst gelöscht werden, wenn alle virtuellen Computer mit dem neuen Zielarbeitsbereich verbunden sind. Dieser Vorgang ist nicht erfolgreich, wenn ein Arbeitsbereich zu früh gelöscht wird.
   >
   >

   - Klicken Sie auf **Abbrechen**, um den Vorgang abzubrechen.

      ![Neukonfigurieren überwachter virtuelle Computer][6]

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Was passiert, wenn der Microsoft Monitoring Agent bereits als Erweiterung auf dem virtuellen Computer installiert wurde?
Bereits vorhandene Verbindungen mit Benutzerarbeitsbereichen werden von Security Center nicht überschrieben. Security Center speichert Sicherheitsdaten des virtuellen Computers im bereits verbundenen Arbeitsbereich. Security Center aktualisiert die Erweiterungsversion für die Einbeziehung der Azure-Ressourcen-ID der VM zur Unterstützung der Security Center-Verwendung.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Was passiert, wenn auf dem Computer ein Microsoft Monitoring Agent installiert ist, es sich dabei aber nicht um eine Erweiterung handelt?
Wenn Microsoft Monitoring Agent direkt auf dem virtuellen Computer (also nicht als Azure-Erweiterung) installiert ist, wird Microsoft Monitoring Agent nicht installiert, und die Sicherheitsüberwachung ist eingeschränkt.

Weitere Informationen finden Sie im nächsten Abschnitt: [Was geschieht, wenn ein SCOM- oder OMS-Direkt-Agent bereits auf meiner VM installiert ist?](security-center-platform-migration-faq.md#what-happens-if-a-scom-or-oms-direct-agent-is-already-installed-on-my-vm)

### <a name="what-happens-if-a-scom-or-oms-direct-agent-is-already-installed-on-my-vm"></a>Was geschieht, wenn ein SCOM- oder OMS-Direkt-Agent bereits auf meiner VM installiert ist?
Security Center kann nicht im Voraus bestimmen, ob ein Agent installiert ist.  Security Center versucht, die Microsoft Monitoring Agent-Erweiterung zu installieren, und dies ist aufgrund des vorhandenen installierten Agent nicht möglich.  Dieser Fehler verhindert das Überschreiben der Agent-Verbindungseinstellungen für den Arbeitsbereich und vermeidet das Erstellen von Multihosting.

> [!NOTE]
> Der Agent wird auf die neueste OMS-Agent-Version aktualisiert.  Dies gilt auch für SCOM-Benutzer.
>
>

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Was passiert, wenn ich diese Erweiterungen entferne?
Wenn Sie die Microsoft Monitoring-Erweiterung entfernen, kann Security Center keine Sicherheitsdaten des virtuellen Computers erfassen, und einige Sicherheitsempfehlungen und Warnungen sind nicht verfügbar. Innerhalb von 24 Stunden erkennt Security Center, dass die Erweiterung auf dem virtuellen Computer fehlt, und installiert sie erneut.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Wie verhindere ich die automatische Agent-Installation und die Arbeitsbereicherstellung?
Sie können die automatische Bereitstellung für Ihre Abonnements in der Sicherheitsrichtlinie deaktivieren, dies wird jedoch nicht empfohlen. Die Deaktivierung der automatischen Bereitstellung schränkt die Empfehlungen und Warnungen von Security Center ein. Die automatische Bereitstellung ist für Abonnements mit dem Standard-Tarif erforderlich. So deaktivieren Sie die automatische Bereitstellung:

1. Wenn Ihr Abonnement für den Standard-Tarif konfiguriert ist, öffnen Sie die Sicherheitsrichtlinie für dieses Abonnement, und wählen Sie den Tarif **Free** aus.

   ![Tarif ][1]

2. Deaktivieren Sie als Nächstes die automatische Bereitstellung, indem Sie auf dem Blatt **Sicherheitsrichtlinie – Datensammlung** auf **Aus** klicken.
   ![Datensammlung][2]

### <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Sollte ich die automatische Agent-Installation und Arbeitsbereicherstellung deaktivieren?

> [!NOTE]
> Lesen Sie unbedingt die Abschnitte [Welche Auswirkungen hat die Deaktivierung der automatischen Bereitstellung?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) und [Welche Schritte werden bei Deaktivierung der automatischen Bereitstellung empfohlen?](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning), falls Sie entscheiden, die automatische Bereitstellung zu deaktivieren.
>
>

Möglicherweise möchten Sie die automatische Bereitstellung deaktivieren, wenn Folgendes auf Sie zutrifft:

- Die automatische Agent-Installation durch Security Center gilt für das gesamte Abonnement.  Sie können die automatische Installation nicht auf eine Teilmenge der VMs anwenden. Wenn wichtige VMs nicht mit Microsoft Monitoring Agent installiert werden können, sollten Sie die automatische Bereitstellung deaktivieren.
- Installation der Microsoft Monitoring Agent-Erweiterung aktualisiert die Agent-Version. Dies gilt für einen Direkt-Agent und einen SCOM-Agent. Wenn ein installierter SCOM-Agent der Version 2012 aktualisiert wird, können Verwaltungsfunktionen verloren gehen, wenn die Version des SCOM-Servers ebenfalls 2012 ist. Sie sollten die Deaktivierung der automatischen Bereitstellung erwägen, wenn Sie Version 2012 des SCOM-Agent installiert haben.
- Wenn Sie über einen benutzerdefinierten Arbeitsbereich außerhalb des Abonnements (einen zentralen Arbeitsbereich) verfügen, sollten Sie die automatische Bereitstellung deaktivieren. Sie können die Microsoft Monitoring Agent-Erweiterung manuell installieren und ihre Verbindung mit Ihrem Arbeitsbereich herstellen, ohne dass Security Center die Verbindung überschreibt.
- Wenn Sie das Erstellen mehrerer Arbeitsbereiche pro Abonnement vermeiden möchten und innerhalb des Abonnements über Ihren eigenen benutzerdefinierten Arbeitsbereich verfügen, haben Sie zwei Optionen:

   1. Sie können die automatische Bereitstellung deaktivieren. Legen Sie nach der Migration die standardmäßigen Arbeitsbereichseinstellungen wie in [Wie kann ich meinen vorhandenen Log Analytics-Arbeitsbereich verwenden?](#how-can-i-use-my-existing-log-analytics-workspace) beschrieben fest.
   2. Alternativ können Sie zulassen, dass die Migration abgeschlossen, der Microsoft Monitoring Agent auf den VMs installiert und die Verbindung der VMs mit dem erstellten Arbeitsbereich hergestellt wird. Wählen Sie dann Ihren eigenen benutzerdefinierten Arbeitsbereich durch Einstellung der standardmäßigen Arbeitsbereichseinstellung mit Aktivierung zur Neukonfiguration der bereits installierten Agents. Weitere Informationen finden Sie unter [Wie kann ich meinen vorhandenen Log Analytics-Arbeitsbereich verwenden?](#how-can-i-use-my-existing-log-analytics-workspace).

### <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Welche Auswirkungen hat die Deaktivierung der automatischen Bereitstellung?
Sobald die Migration abgeschlossen ist, kann Security Center keine Sicherheitsdaten der VM erfassen, und einige Sicherheitsempfehlungen und Warnungen sind nicht verfügbar. Wenn Sie die Deaktivierung durchführen, sollten Sie den Microsoft Monitoring Agent manuell installieren. Siehe [Welche Schritte werden bei Deaktivierung der automatischen Bereitstellung empfohlen?](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).

### <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Welche Schritte werden bei Deaktivierung der automatischen Bereitstellung empfohlen?
Damit Security Center Sicherheitsdaten auf Ihren VMs erfassen und Empfehlungen und Warnungen bereitstellen kann, müssen Sie Microsoft Monitoring Agent manuell installieren. Eine Anleitung zur Installation finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](../log-analytics/log-analytics-windows-agents.md).

Sie können eine Verbindung des Agent zu jedem vorhandenen benutzerdefinierten Arbeitsbereich oder in Security Center erstellten Arbeitsbereich herstellen. Wenn in einem benutzerdefinierten Arbeitsbereich die Lösungen „Security“ oder „SecurityCenterFree“ nicht aktiviert sind, müssen Sie eine Lösung anwenden. Wählen Sie zum Anwenden den benutzerdefinierten Arbeitsbereich oder das Abonnement aus, und wenden Sie einen Tarif über das Blatt **Sicherheitsrichtlinie – Tarif** an.

   ![Tarif ][1]

Security Center aktiviert basierend auf dem ausgewählten Tarif die richtige Lösung im Arbeitsbereich.

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Wie entferne ich durch Security Center installierte OMS-Erweiterungen?
Der Microsoft Monitoring Agent kann manuell entfernt werden. Dies wird jedoch nicht empfohlen, da es die Empfehlungen und Warnungen von Security Center einschränkt.

> [!NOTE]
> Bei aktivierter Datensammlung wird der Agent nach dem Entfernen erneut installiert.  Daher müssen Sie vor dem manuellen Entfernen des Agents die Datensammlung deaktivieren. Die Anleitung zum Deaktivieren der Datensammlung finden Sie unter [Wie verhindere ich die automatische Agent-Installation und die Arbeitsbereicherstellung?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?).
>
>

So entfernen Sie den Agent manuell:

1.  Öffnen Sie im Portal **Log Analytics**.
2.  Wählen Sie auf dem Blatt „Log Analytics“ einen Arbeitsbereich aus:
3.  Wählen Sie die virtuellen Computer aus, die Sie nicht überwachen möchten, und wählen Sie anschließend **Trennen** aus.

   ![Entfernen des Agents][3]

> [!NOTE]
> Falls ein virtueller Linux-Computer bereits über einen OMS-Agent verfügt, bei dem es sich nicht um eine Erweiterung handelt, wird der Agent beim Entfernen der Erweiterung ebenfalls entfernt und muss vom Kunden erneut installiert werden.
>
>

## <a name="existing-oms-customers"></a>OMS-Bestandskunden

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Überschreibt Security Center bereits vorhandene Verbindungen zwischen virtuellen Computern und Arbeitsbereichen?
Wenn der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf einem virtuellen Computer installiert ist, wird die vorhandene Arbeitsbereichverbindung nicht überschrieben. Stattdessen verwendet Security Center den vorhandenen Arbeitsbereich.

Eine Security Center-Lösung wird im Arbeitsbereich installiert, falls noch keine vorhanden ist, und die Lösung wird nur auf die relevanten virtuellen Computer angewendet. Wenn Sie eine Lösung hinzufügen, wird sie automatisch standardmäßig für alle mit Ihrem Log Analytics-Arbeitsbereich verbundenen Windows- und Linux-Agents bereitgestellt. Mit der [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md) (ein OMS-Feature) können Sie einen Geltungsbereich auf Ihre Lösungen anwenden.

Wenn Microsoft Monitoring Agent direkt auf dem virtuellen Computer (also nicht als Azure-Erweiterung) installiert ist, wird Microsoft Monitoring Agent nicht installiert, und die Sicherheitsüberwachung ist eingeschränkt.

### <a name="what-should-i-do-if-i-suspect-that-the-data-platform-migration-broke-the-connection-between-one-of-my-vms-and-my-workspace"></a>Was kann ich tun, wenn ich den Verdacht habe, dass die Datenplattformmigration die Verbindung zwischen einem virtuellen Computer und meinem Arbeitsbereich unterbrochen hat?
Dieser Fall sollte nicht eintreten. Falls doch, [erstellen Sie eine Azure-Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md), und geben Sie folgende Details an:

- Die Azure-Ressourcen-ID des betroffenen virtuellen Computers
- Die Azure-Ressourcen-ID des Arbeitsbereichs, der vor der Verbindungsunterbrechung für die Erweiterung konfiguriert war
- Den Agent und die zuvor installierte Version

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-the-billing-implications"></a>Installiert Security Center Lösungen in meinen vorhandenen OMS-Arbeitsbereichen? Was bedeutet das für die Abrechnung?
Wenn Security Center erkennt, dass ein virtueller Computer bereits mit einem Arbeitsbereich verbunden ist, den Sie erstellt haben, aktiviert Security Center Lösungen in diesem Arbeitsbereich gemäß Ihres Tarifs. Da die Lösungen mittels [Zielgruppenadressierung für Lösungen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting) nur auf die relevanten virtuellen Azure-Computer angewendet werden, bleibt die Abrechnung unverändert.

- **Free-Tarif:** Security Center installiert die Lösung „SecurityCenterFree“ im Arbeitsbereich. Für den Free-Tarif entstehen keine Kosten.
- **Standard-Tarif**: Security Center installiert die Lösung „Security“ im Arbeitsbereich.

   ![Lösungen im Standardarbeitsbereich][4]

> [!NOTE]
> Die Lösung „Security“ in Log Analytics ist die Lösung „Sicherheit und Überwachung“ in OMS.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>In meiner Umgebung sind bereits Arbeitsbereiche vorhanden. Kann ich diese zum Sammeln von Sicherheitsdaten verwenden?
Wenn der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf einem virtuellen Computer installiert ist, verwendet Security Center den vorhandenen verbundenen Arbeitsbereich. Eine Security Center-Lösung wird im Arbeitsbereich installiert, falls noch keine vorhanden ist, und die Lösung wird mittels [Zielgruppenadressierung für Lösungen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting) nur auf die relevanten virtuellen Computer angewendet.

Wenn Security Center den Microsoft Monitoring Agent auf virtuellen Computern installiert, werden dabei die von Security Center erstellten Standardarbeitsbereiche verwendet. In Kürze können Kunden die zu verwendenden Arbeitsbereiche konfigurieren.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Die Lösung „Security“ ist bereits in meinen Arbeitsbereichen enthalten. Was bedeutet das für die Abrechnung?
Die Lösung „Sicherheit und Überwachung“ dient zum Aktivieren von Security Center-Features des Standard-Tarifs für virtuelle Azure-Computer. Wenn die Lösung „Sicherheit und Überwachung“ bereits in einem Arbeitsbereich installiert ist, verwendet Security Center die vorhandene Lösung. Die Abrechnung wird dadurch nicht beeinflusst.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Security Center-Plattformmigration finden Sie in den folgenden Artikeln:

- [Azure Security Center – Plattformmigration](security-center-platform-migration.md)
- [Azure Security Center – Handbuch zur Problembehandlung](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png
[6]: ./media/security-center-platform-migration-faq/reconfigure-monitored-vm.png
