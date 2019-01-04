---
title: Azure Security Center – Häufig gestellte Fragen (FAQ) | Microsoft Docs
description: Dieses FAQ-Dokument beantwortet Fragen zu Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2018
ms.author: rkarlin
ms.openlocfilehash: 2be11f97882a7c6353b214886ce6c5a3a66add89
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339109"
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Security Center – Häufig gestellte Fragen 
Hier werden häufig gestellte Fragen zu Azure Security Center beantwortet. Azure Security Center ist ein Dienst, der Sie aufgrund von größerer Transparenz und besserer Kontrolle der Sicherheit Ihrer Microsoft Azure-Ressourcen dabei unterstützt, Bedrohungen zu verhindern, zu erkennen und darauf zu reagieren.

> [!NOTE]
> Ab Anfang Juni 2017 verwendet Security Center den Microsoft Monitoring Agent zum Sammeln und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). Die Informationen in diesem Artikel stellen Security Center-Funktionen nach dem Umstieg auf den Microsoft Monitoring Agent vor.
>
>

## <a name="general-questions"></a>Allgemeine Fragen
### <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Sektrum an Sicherheitslösungen verwendet werden.

### <a name="how-do-i-get-azure-security-center"></a>Wie erhalte ich Azure Security Center?
Azure Security Center wird mit Ihrem Microsoft Azure-Abonnement aktiviert und über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)aufgerufen. ([Melden Sie sich beim Portal an](https://portal.azure.com), wählen Sie **Durchsuchen** aus, und führen Sie einen Bildlauf zu **Security Center** aus.)  

## <a name="billing"></a>Abrechnung
### <a name="how-does-billing-work-for-azure-security-center"></a>Wie funktioniert die Abrechnung für Azure Security Center?
Security Center wird in zwei Tarifen angeboten:

Der **Free-Tarif** bietet einen Einblick in den Sicherheitszustand Ihrer Azure-Ressourcen, grundlegende Sicherheitsrichtlinien, Empfehlungen zur Sicherheit und die Integration in Sicherheitsprodukte und -Dienste von Partnern.

Mit dem **Standard-Tarif** werden erweiterte Funktionen zur Bedrohungserkennung hinzugefügt, darunter Informationen zu Bedrohungen, Verhaltensanalysen, Erkennung von Anomalien, Sicherheitsincidents und Berichte zur Zuordnung von Bedrohungen. Sie können eine Standard-Testversion ohne Tarif starten. Wenn Sie ein Upgrade durchführen möchten, wählen Sie in der Sicherheitsrichtlinie [Tarif](https://docs.microsoft.com/azure/security-center/security-center-pricing) aus. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="permissions"></a>Berechtigungen
Azure Security Center verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/role-assignments-portal.md). Diese stellt [integrierte Rollen](../role-based-access-control/built-in-roles.md) bereit, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Security Center bewertet die Konfiguration Ihrer Ressourcen, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist.

Informationen zu Rollen und zulässigen Aktionen in Security Center finden Sie unter [Permissions in Azure Security Center](security-center-permissions.md) (Berechtigungen in Azure Security Center).

## <a name="data-collection-agents-and-workspaces"></a>Datensammlung, Agents und Arbeitsbereiche
Security Center sammelt Daten von Ihren virtuellen Azure-Computern (VMs) und Azure-fremden Computern, um sie hinsichtlich Sicherheitslücken und Bedrohungen zu überwachen. Die Daten werden mithilfe von Microsoft Monitoring Agent gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle auf dem Computer und kopiert die Daten zur Analyse in Ihren Arbeitsbereich.

### <a name="am-i-billed-for-log-analytics-on-the-workspaces-created-by-security-center"></a>Werden mir die vom Security Center erstellten Arbeitsbereiche im Rahmen von Log Analytics in Rechnung gestellt?
Nein. Vom Security Center erstellte Arbeitsbereiche sind zwar für die knotenbasierte Abrechnung von Log Analytics konfiguriert, es fallen jedoch keine Log Analytics-Kosten an. Die Abrechnung von Security Center basiert immer auf Ihrer Security Center-Sicherheitsrichtlinie und den installierten Lösungen in einem Arbeitsbereich:

- **Free-Tarif**: Security Center aktiviert die Lösung „SecurityCenterFree“ im Standardarbeitsbereich. Für den Free-Tarif entstehen keine Kosten.
- **Standard-Tarif**: Security Center aktiviert die Lösung „Security“ im Standardarbeitsbereich.

Weitere Informationen zu Preisen finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/). Auf der Seite mit den Preisinformationen werden Änderungen an der Speicherung von Sicherheitsdaten und die anteilsmäßige Abrechnung ab Juni 2017 berücksichtigt.

> [!NOTE]
> Der Log Analytics-Tarif von Arbeitsbereichen, die vom Security Center erstellt wurden, hat keine Auswirkungen auf die Security Center-Abrechnung.
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

3. Wählen Sie **Speichern**aus.
4. Nach dem Klicken auf **Speichern** werden Sie gefragt, ob Sie überwachte virtuelle Computer neu konfigurieren möchten.

   - Klicken Sie auf **Nein**, wenn die neuen Arbeitsbereichseinstellungen **nur auf neue virtuelle Computer angewendet werden sollen**. Die neuen Arbeitsbereichseinstellungen gelten nur für neue Agent-Installationen (neu ermittelte virtuelle Computer, auf denen Microsoft Monitoring Agent nicht installiert ist).
   - Klicken Sie auf **Ja**, wenn die neuen Arbeitsbereichseinstellungen **auf alle virtuellen Computer angewendet werden sollen**. Darüber hinaus wird jeder virtuelle Computer, der mit einem von Security Center erstellten Arbeitsbereich verbunden ist, nun mit dem neuen Zielarbeitsbereich verbunden.

   > [!NOTE]
   > Wenn Sie auf „Ja“ klicken, dürfen die von Security Center erstellten Arbeitsbereiche erst gelöscht werden, wenn alle virtuellen Computer mit dem neuen Zielarbeitsbereich verbunden sind. Dieser Vorgang ist nicht erfolgreich, wenn ein Arbeitsbereich zu früh gelöscht wird.
   >
   >

   - Klicken Sie auf **Abbrechen**, um den Vorgang abzubrechen.

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Was passiert, wenn der Microsoft Monitoring Agent bereits als Erweiterung auf dem virtuellen Computer installiert wurde?
Bereits vorhandene Verbindungen mit Benutzerarbeitsbereichen werden von Security Center nicht überschrieben. Security Center speichert Sicherheitsdaten des virtuellen Computers im bereits verbundenen Arbeitsbereich. Security Center aktualisiert die Erweiterungsversion für die Einbeziehung der Azure-Ressourcen-ID der VM zur Unterstützung der Security Center-Verwendung.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Was passiert, wenn auf dem Computer ein Microsoft Monitoring Agent installiert ist, es sich dabei aber nicht um eine Erweiterung handelt?
Wenn Microsoft Monitoring Agent direkt auf dem virtuellen Computer (also nicht als Azure-Erweiterung) installiert ist, wird Microsoft Monitoring Agent nicht installiert, und die Sicherheitsüberwachung ist eingeschränkt.

Weitere Informationen finden Sie im nächsten Abschnitt: [Was geschieht, wenn ein SCOM- oder OMS-Direkt-Agent bereits auf meiner VM installiert ist?](#scomomsinstalled)

### Was geschieht, wenn auf meinem virtuellen Computer bereits ein SCOM- oder OMS-Direkt-Agent installiert ist?<a name="scomomsinstalled"></a>
Security Center kann nicht im Voraus bestimmen, ob ein Agent installiert ist.  Security Center versucht, die Microsoft Monitoring Agent-Erweiterung zu installieren, und dies ist aufgrund des vorhandenen installierten Agent nicht möglich.  Dieser Fehler verhindert das Überschreiben der Agent-Verbindungseinstellungen für den Arbeitsbereich und vermeidet das Erstellen von Multihosting.

> [!NOTE]
> Der Agent wird auf die neueste OMS-Agent-Version aktualisiert.  Dies gilt auch für SCOM-Benutzer.
>
>

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Was passiert, wenn ich diese Erweiterungen entferne?
Wenn Sie die Microsoft Monitoring-Erweiterung entfernen, kann Security Center keine Sicherheitsdaten des virtuellen Computers erfassen, und einige Sicherheitsempfehlungen und Warnungen sind nicht verfügbar. Innerhalb von 24 Stunden erkennt Security Center, dass die Erweiterung auf dem virtuellen Computer fehlt, und installiert sie erneut.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Wie verhindere ich die automatische Agent-Installation und die Arbeitsbereicherstellung?
Sie können die automatische Bereitstellung für Ihre Abonnements in der Sicherheitsrichtlinie deaktivieren, dies wird jedoch nicht empfohlen. Die Deaktivierung der automatischen Bereitstellung schränkt die Empfehlungen und Warnungen von Security Center ein. So deaktivieren Sie die automatische Bereitstellung:

1. Wenn Ihr Abonnement für den Standard-Tarif konfiguriert ist, öffnen Sie die Sicherheitsrichtlinie für dieses Abonnement, und wählen Sie den Tarif **Free** aus.

   ![Tarif][1]

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

Damit Security Center Sicherheitsdaten auf Ihren VMs erfassen und Empfehlungen und Warnungen bereitstellen kann, müssen Sie die Erweiterung Microsoft Monitoring Agent manuell installieren. Eine Anleitung zur Installation finden Sie unter [Agent-Installation für Windows-VM](../virtual-machines/extensions/oms-windows.md) oder [Agent-Installation für Linux-VM](../virtual-machines/extensions/oms-linux.md).

Sie können eine Verbindung des Agent zu jedem vorhandenen benutzerdefinierten Arbeitsbereich oder in Security Center erstellten Arbeitsbereich herstellen. Wenn in einem benutzerdefinierten Arbeitsbereich die Lösungen „Security“ oder „SecurityCenterFree“ nicht aktiviert sind, müssen Sie eine Lösung anwenden. Wählen Sie zum Anwenden den benutzerdefinierten Arbeitsbereich oder das Abonnement aus, und wenden Sie einen Tarif über das Blatt **Sicherheitsrichtlinie – Tarif** an.

   ![Tarif][1]

Security Center aktiviert basierend auf dem ausgewählten Tarif die richtige Lösung im Arbeitsbereich.

### Wie entferne ich durch Security Center installierte OMS-Erweiterungen?<a name="remove-oms"></a>
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
### <a name="how-do-i-disable-data-collection"></a>Wie deaktiviere ich Datensammlung?
Die automatische Bereitstellung ist standardmäßig deaktiviert. Sie können die automatische Bereitstellung in Ressourcen jederzeit deaktivieren, indem Sie diese Einstellung in der Sicherheitsrichtlinie deaktivieren. Die automatische Bereitstellung wird dringend empfohlen, um Sicherheitswarnungen und -empfehlungen zu Systemupdates, zu Sicherheitsrisiken für das Betriebssystem und zu Endpoint Protection zu erhalten.

Um die Datensammlung zu deaktivieren, [melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie nacheinander **Durchsuchen**, **Security Center** und **Richtlinie auswählen** aus. Wählen Sie das Abonnement aus, für das Sie die automatische Bereitstellung deaktivieren möchten. Wenn Sie ein Abonnement auswählen, wird **Sicherheitsrichtlinie – Datensammlung** geöffnet. Wählen Sie unter **Automatische Bereitstellung** die Option **Aus** aus.

### <a name="how-do-i-enable-data-collection"></a>Wie aktiviere ich die Datensammlung?
Sie können die Datensammlung für Ihr Azure-Abonnement in der Sicherheitsrichtlinie aktivieren. Zum Aktivieren der Datensammlung [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie nacheinander **Durchsuchen**, **Security Center** und **Sicherheitsrichtlinie** aus. Wählen Sie das Abonnement aus, für das Sie die automatische Bereitstellung aktivieren möchten. Wenn Sie ein Abonnement auswählen, wird **Sicherheitsrichtlinie – Datensammlung** geöffnet. Wählen Sie unter **Automatische Bereitstellung** die Option **Ein** aus.

### <a name="what-happens-when-data-collection-is-enabled"></a>Was passiert, wenn die Datensammlung aktiviert wird?
Bei aktivierter automatischer Bereitstellung wird Microsoft Monitoring Agent von Security Center auf allen unterstützten virtuellen Azure-Computern sowie auf allen neu erstellten virtuellen Computern bereitgestellt. Die automatische Bereitstellung wird dringend empfohlen, die manuelle Agent-Installation ist jedoch ebenfalls verfügbar. [Erfahren Sie, wie Sie die Microsoft Monitoring Agent-Erweiterung installieren.](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension) 

Der Agent aktiviert das Prozesserstellungsereignis 4688 und das Feld *CommandLine* im Ereignis 4688. Auf der VM erstellte neue Prozesse werden von EventLog aufgezeichnet und von den Erkennungsdiensten in Security Center überwacht. Weitere Informationen zu den für jeden neuen Prozess aufgezeichneten Details finden Sie unter [Description Fields in 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields) (Beschreibungsfelder in 4688). Außerdem sammelt der Agent die auf dem virtuellen Computer erstellten 4688-Ereignisse und speichert sie in der Suche.

Der Agent aktiviert auch die Datensammlung für [Adaptive Anwendungssteuerungen](security-center-adaptive-application.md), und Security Center konfiguriert eine lokale AppLocker-Richtlinie im Überwachungsmodus, um alle Anwendungen zuzulassen. Damit generiert AppLocker Ereignisse, die dann von Security Center gesammelt und genutzt werden können. Es ist wichtig zu beachten, dass diese Richtlinie nicht auf Computern konfiguriert wird, auf denen bereits eine AppLocker-Richtlinie konfiguriert ist. 

Wenn Security Center verdächtige Aktivitäten auf dem virtuellen Computer erkennt, wird der Kunde per E-Mail benachrichtigt, sofern [Sicherheitskontaktinformationen](security-center-provide-security-contact-details.md) bereitgestellt wurden. Außerdem wird eine Warnung auf dem Sicherheitswarnungs-Dashboard von Security Center angezeigt.



### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>Wirkt sich der Monitoring Agent auf die Leistung der Server aus?
Der Agent beansprucht eine äußerst geringe Menge von Systemressourcen und sollten nur eine geringe Auswirkung auf die Leistung haben. Weitere Informationen zu Auswirkungen auf die Leistung, zum Agent und zur Erweiterung finden Sie unter [Planungs- und Betriebshandbuch](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Wo werden meine Daten gespeichert?
Die von diesem Agent gesammelten Daten werden entweder in einem vorhandenen Log Analytics-Arbeitsbereich, der mit Ihrem Abonnement verknüpft ist, oder in einem neuen Arbeitsbereich gespeichert. Weitere Informationen finden Sie unter [Datensicherheit](security-center-data-security.md).

## Log Analytics-Bestandskunden<a name="existingloganalyticscust"></a>

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Überschreibt Security Center bereits vorhandene Verbindungen zwischen virtuellen Computern und Arbeitsbereichen?
Wenn der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf einem virtuellen Computer installiert ist, wird die vorhandene Arbeitsbereichverbindung nicht überschrieben. Stattdessen verwendet Security Center den vorhandenen Arbeitsbereich.

Eine Security Center-Lösung wird im Arbeitsbereich installiert, falls noch keine vorhanden ist, und die Lösung wird nur auf die relevanten virtuellen Computer angewendet. Wenn Sie eine Lösung hinzufügen, wird sie automatisch standardmäßig für alle mit Ihrem Log Analytics-Arbeitsbereich verbundenen Windows- und Linux-Agents bereitgestellt. Mit der [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md) können Sie einen Geltungsbereich auf Ihre Lösungen anwenden.

Wenn Microsoft Monitoring Agent direkt auf dem virtuellen Computer (also nicht als Azure-Erweiterung) installiert ist, wird Microsoft Monitoring Agent nicht installiert, und die Sicherheitsüberwachung ist eingeschränkt.

### <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Installiert das Security Center Lösungen in meinen vorhandenen Log Analytics-Arbeitsbereichen? Was bedeutet das für die Abrechnung?
Wenn Security Center erkennt, dass ein virtueller Computer bereits mit einem Arbeitsbereich verbunden ist, den Sie erstellt haben, aktiviert Security Center Lösungen in diesem Arbeitsbereich gemäß Ihres Tarifs. Da die Lösungen mittels [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md) nur auf die relevanten virtuellen Azure-Computer angewendet werden, bleibt die Abrechnung unverändert.

- **Free-Tarif:** Security Center installiert die Lösung „SecurityCenterFree“ im Arbeitsbereich. Für den Free-Tarif entstehen keine Kosten.
- **Standard-Tarif**: Security Center installiert die Lösung „Security“ im Arbeitsbereich.

   ![Lösungen im Standardarbeitsbereich][4]

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>In meiner Umgebung sind bereits Arbeitsbereiche vorhanden. Kann ich diese zum Sammeln von Sicherheitsdaten verwenden?
Wenn der Microsoft Monitoring Agent bereits als Azure-Erweiterung auf einem virtuellen Computer installiert ist, verwendet Security Center den vorhandenen verbundenen Arbeitsbereich. Eine Security Center-Lösung wird im Arbeitsbereich installiert, falls noch keine vorhanden ist, und die Lösung wird mittels [Zielgruppenadressierung für Lösungen](../operations-management-suite/operations-management-suite-solution-targeting.md) nur auf die relevanten virtuellen Computer angewendet.

Wenn Security Center den Microsoft Monitoring Agent auf virtuellen Computern installiert, werden dabei die von Security Center erstellten Standardarbeitsbereiche verwendet.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Die Lösung „Security“ ist bereits in meinen Arbeitsbereichen enthalten. Was bedeutet das für die Abrechnung?
Die Lösung „Sicherheit und Überwachung“ dient zum Aktivieren von Security Center-Features des Standard-Tarifs für virtuelle Azure-Computer. Wenn die Lösung „Sicherheit und Überwachung“ bereits in einem Arbeitsbereich installiert ist, verwendet Security Center die vorhandene Lösung. Die Abrechnung wird dadurch nicht beeinflusst.

## <a name="using-azure-security-center"></a>Verwenden von Azure Security Center
### <a name="what-is-a-security-policy"></a>Was ist eine Sicherheitsrichtlinie?
In einer Sicherheitsrichtlinie werden die Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement zu empfehlen sind. In Azure Security Center definieren Sie Richtlinien für Ihre Azure-Abonnements entsprechend den Sicherheitserfordernissen Ihres Unternehmens sowie dem Typ der Anwendungen oder der Vertraulichkeit der Daten in jedem Abonnement.

Die Sicherheitsempfehlungen und -überwachung werden entsprechend den in Azure Security Center aktivierten Sicherheitsrichtlinien umgesetzt. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Wie kann ich eine Sicherheitsrichtlinie ändern?
Damit Sie eine Sicherheitsrichtlinie ändern können, müssen Sie Systemadministrator oder Besitzer bzw. Mitwirkender dieses Abonnements sein.

Weitere Informationen dazu, wie eine Sicherheitsrichtlinie konfiguriert wird, finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md).

### <a name="what-is-a-security-recommendation"></a>Was ist eine Sicherheitsempfehlung?
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden mögliche Sicherheitsrisiken festgestellt, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess des Konfigurierens des erforderlichen Sicherheitsmechanismus geführt. Beispiele:

* Bereitstellung von Antischadsoftware, um bösartige Software zu erkennen und zu entfernen
* [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) und Regeln, um Datenverkehr zu virtuellen Computern zu steuern
* Bereitstellung einer Web Application Firewall als Schutz vor Angriffen, die auf Ihre Webanwendungen abzielen
* Bereitstellen von fehlenden Systemupdates
* Behandeln von Betriebssystemkonfigurationen, die nicht den empfohlenen Basisregeln entsprechen

Hier sind nur Empfehlungen aufgeführt, die in den Sicherheitsrichtlinien aktiviert sind.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Wie kann ich den aktuellen Sicherheitsstatus meiner Azure-Ressourcen anzeigen?
Auf dem Blatt **Security Center Overview** (Security Center – Übersicht) wird der Gesamtsicherheitsstatus Ihrer Umgebung aufgegliedert nach Compute, Netzwerk, Speicher und Daten und Anwendungen angezeigt. Jeder Ressourcentyp hat einen Indikator, der kennzeichnet, ob irgendwelche möglichen Sicherheitsrisiken festgestellt wurden. Wenn Sie auf die einzelnen Kacheln klicken, wird eine Liste mit von Security Center erkannten Sicherheitsproblemen sowie ein Bestand an Ressourcen in Ihrem Abonnement angezeigt.

### <a name="what-triggers-a-security-alert"></a>Wodurch wird eine Sicherheitswarnung ausgelöst?
Azure Security Center erfasst, analysiert und kombiniert automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von Partnerlösungen wie Antischadsoftware und Firewalls. Wurden Bedrohungen erkannt, wird eine Sicherheitswarnung erstellt. Beispiele für eine Erkennung sind:

* Gefährdete virtuelle Computer, die mit bekannten bösartigen IP-Adressen kommunizieren
* Erweiterte Schadsoftware, die mithilfe der Windows-Fehlerberichterstattung erkannt wurde
* Brute-Force-Angriffe gegen virtuelle Computer
* Sicherheitswarnungen von integrierten Partnersicherheitslösungen wie Antischadsoftware oder Web Application Firewalls

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Wo liegt bei der Bedrohungserkennung und Ausgabe von Warnungen der Unterschied zwischen Microsoft Security Response Center und Azure Security Center?
Das Microsoft Security Response Center (MSRC) führt eine selektive Sicherheitsüberwachung im Azure-Netzwerk und in der Infrastruktur durch und empfängt Threat Intelligence-Daten und Missbrauchsmeldungen von Drittanbietern. Wenn MSRC erkennt, dass eine Partei unrechtmäßig oder unbefugt auf Kundendaten zugreift oder dass der Kunde Azure nicht entsprechend den Bestimmungen für den Umgang mit vertraulichen Informationen (Acceptable Use) nutzt, wird der Kunde durch einen Manager für Sicherheitsincidents benachrichtigt. Die Benachrichtigung erfolgt in der Regel durch das Senden einer E-Mail an die im Azure Security Center angegebenen Sicherheitskontakte oder an den Besitzer des Azure-Abonnements, falls kein Sicherheitskontakt angegeben wurde.

Das Security Center ist ein Azure-Dienst, der die Azure-Umgebung des Kunden ständig überwacht und Analysefunktionen anwendet, um eine Vielzahl von potenziell schädlichen Aktivitäten automatisch zu erkennen. Diese Erkennungen werden als Sicherheitshinweise im Security Center-Dashboard angegeben.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welche Azure-Ressourcen werden von Azure Security Center überwacht?
Azure Security Center überwacht die folgenden Azure-Ressourcen:

* Virtuelle Computer (Virtual Machines, VMs) (auch [Clouddienste](../cloud-services/cloud-services-choose-me.md))
* Virtuelle Azure-Netzwerke
* Azure SQL-Dienst
* Azure-Speicherkonto
* Azure-Web-Apps (in [App Service-Umgebung](../app-service/environment/intro.md))
* Partnerlösungen, die in Ihr Azure-Abonnement integriert sind, z.B. die Firewall einer Webanwendung auf virtuellen Computern und in einer App Service-Umgebung

## <a name="virtual-machines"></a>Virtual Machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Welche Typen von virtuellen Maschinen werden unterstützt?
Die Überwachung und entsprechende Empfehlungen sind für virtuelle Computer (VMs) verfügbar, die mit dem [klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md)erstellt wurden.

Eine Liste mit unterstützten Plattformen finden Sie unter [Supported platforms in Azure Security Center (Unterstützte Plattformen in Azure Security Center)](security-center-os-coverage.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Warum erkennt Azure Security Center die Antischadsoftware nicht, die auf meiner Azure-VM ausgeführt wird?
Azure Security Center erkennt Antischadsoftware, die über Azure-Erweiterungen installiert wurde. Security Center kann z.B. Antischadsoftware nicht erkennen, die auf einem Image vorinstalliert war, das Sie bereitgestellt haben, oder Antischadsoftware, die Sie auf Ihren virtuellen Computern über Ihre eigenen Prozesse installiert haben (z.B. Systeme für die Konfigurationsverwaltung).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Warum erhalte ich die Meldung „Fehlende Überprüfungsdaten“ für meinen virtuellen Computer?
Diese Meldung wird angezeigt, wenn keine Überprüfungsdaten für einen virtuellen Computer vorhanden sind. Es kann einige Zeit dauern (weniger als eine Stunde), um die Überprüfungsdaten aufzufüllen, nachdem die Datensammlung in Azure Security Center aktiviert wurde. Nach dem ersten Auffüllen der Überprüfungsdaten erhalten Sie diese Meldung möglicherweise, weil überhaupt keine oder keine aktuellen Überprüfungsdaten vorliegen. Überprüfungen werden für beendete virtuelle Computer nicht aufgefüllt. Diese Meldung kann auch angezeigt werden, wenn die Überprüfungsdaten (gemäß der Aufbewahrungsrichtlinie für den Windows-Agenten, die einen Standardwert von 30 Tagen vorgibt) nicht vor kurzem aufgefüllt wurden.

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Wie häufig sucht Security Center nach Sicherheitsrisiken des Betriebssystems, Systemupdates und Endpoint Protection-Problemen?
Wartezeiten bei Security Center-Scans auf Sicherheitsrisiken, Updates und Problemen:

- Betriebssystem-Sicherheitskonfigurationen – Daten werden innerhalb von 48 Stunden aktualisiert.
- Systemupdates – Daten werden innerhalb von 24 Stunden aktualisiert.
- Endpoint Protection-Probleme – Daten werden innerhalb von 8 Stunden aktualisiert.

Das Security Center scannt in der Regel jede Stunde nach neuen Daten und aktualisiert die Empfehlungen entsprechend. 

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Warum erhalte ich die Meldung „VM-Agent fehlt“?
Der VM-Agent muss auf virtuellen Computern installiert sein, um die Datensammlung zu ermöglichen. Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Informationen zum Installieren des VM-Agents auf anderen virtuellen Computern finden Sie im Blogbeitrag [VM Agent and Extensions](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)(VM-Agent und Erweiterungen).


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png
