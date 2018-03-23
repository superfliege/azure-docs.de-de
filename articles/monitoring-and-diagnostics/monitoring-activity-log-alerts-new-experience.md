---
title: Erstellen von Aktivitätsprotokollwarnungen und Verwenden der neuen Oberfläche „Warnungen“ (Vorschauversion) in Azure Monitor für die Verwaltung | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Erstellung von Aktivitätsprotokollwarnungen auf der Registerkarte „Warnungen“ (Vorschauversion) in Azure Monitor. Im Artikel wird die neue Benutzeroberfläche für dieses Feature ausführlich beschrieben.
author: JYOTHIRMAISURI
manager: vvithal
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: aabc0e57-78cd-44dd-a8d1-af5e1e567360
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: v-jysur
ms.custom: ''
ms.openlocfilehash: a7553e4155df0d4ee49b798f44ca636dc7ecdcd2
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="create-activity-log-alerts-using-the-new-alerts-preview-experience"></a>Erstellen von Aktivitätsprotokollwarnungen mit der neuen Oberfläche „Warnungen“ (Vorschauversion)

Aktivitätsprotokollwarnungen werden aktiviert, wenn ein neues Aktivitätsprotokollereignis eintritt, das die in der Warnung angegebenen Bedingungen erfüllt.

Diese Warnungen gelten für Azure-Ressourcen und können mit einer Azure Resource Manager-Vorlage erstellt werden. Sie können auch im Azure-Portal erstellt, aktualisiert oder gelöscht werden. In diesem Artikel werden die Konzepte erläutert, auf denen Aktivitätsprotokollwarnungen basieren. Anschließend wird veranschaulicht, wie Sie das Azure-Portal zum Einrichten einer Warnung für Aktivitätsprotokollereignisse einrichten, indem Sie die neue Oberfläche [Azure-Warnungen (Vorschauversion)](monitoring-overview-unified-alerts.md) verwenden.

Normalerweise erstellen Sie Aktivitätsprotokollwarnungen, um Benachrichtigungen zu erhalten, wenn es für Ressourcen in Ihrem Azure-Abonnement zu spezifischen Änderungen kommt, die häufig für bestimmte Ressourcengruppen oder Ressourcen gelten. Es kann beispielsweise sein, dass Sie benachrichtigt werden möchten, wenn ein virtueller Computer in **myProductionResourceGroup** (Beispielressourcengruppe) gelöscht wird oder wenn einem Benutzer Ihres Abonnements neue Rollen zugewiesen werden.

Sie können eine Aktivitätsprotokollwarnung basierend auf einer beliebigen Eigenschaft der obersten Ebene im JSON-Objekt für ein Aktivitätsprotokollereignis konfigurieren. Im Portal werden aber die gängigsten Optionen angezeigt, die in den folgenden Abschnitten beschrieben sind:

>[!NOTE]

> Wenn die Kategorie „Verwaltung“ lautet, müssen Sie in Ihrer Warnung mindestens eins der oben genannten Kriterien angeben. Eine Warnung, die jedes Mal aktiviert wird, wenn ein Ereignis in den Aktivitätsprotokollen erstellt wird, kann nicht erstellt werden.
>

Bei Aktivierung einer Aktivitätsprotokollwarnung wird eine Aktionsgruppe verwendet, um Aktionen oder Benachrichtigungen zu generieren. Eine Aktionsgruppe ist ein wiederverwendbarer Satz von Benachrichtigungsempfängern, z.B. E-Mail-Adressen, Webhook-URLs oder SMS-Telefonnummern. Mehrere Warnungen können auf die Empfänger verweisen, um Ihre Benachrichtigungskanäle zu zentralisieren und gruppieren. Wenn Sie Ihre Aktivitätsprotokollwarnung definieren, stehen Ihnen zwei Optionen zur Verfügung. Ihre Möglichkeiten:

* Verwenden Sie eine vorhandene Aktionsgruppe in Ihrer Aktivitätsprotokollwarnung.
* Erstellen Sie eine neue Aktionsgruppe.

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](monitoring-action-groups.md).

Weitere Informationen zu Dienstintegritätsbenachrichtigungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](monitoring-activity-log-alerts-on-service-notifications.md).


## <a name="whats-new-in-alerts-preview-for-activity-logs"></a>Welche Neuigkeiten gibt es für „Warnungen“ (Vorschauversion) für Aktivitätsprotokolle?

[Azure-Warnungen (Vorschauversion)](monitoring-overview-unified-alerts.md) verfügt jetzt über eine erweiterte Benutzeroberfläche für Aktivitätsprotokollwarnungen. Mit der [erweiterten Benutzeroberfläche für Warnungen](monitoring-overview-unified-alerts.md) haben Sie nun folgende Möglichkeiten:

- [Erstellen](#create-an-alert-rule-for-an-activity-log) und [Verwalten](#view-and-manage-activity-log-alert-rules) der Regeln für Aktivitätsprotokollwarnungen über das Blatt **Überwachen** > **Warnungen (Vorschauversion)**. Erfahren Sie mehr zu [Aktivitätsprotokollen](monitoring-overview-activity-logs.md).

- **Neue Optionen für das Ziel von Warnungen**: Beim Erstellen einer neuen Regel für Aktivitätsprotokollwarnungen können Sie jetzt eine Zielressource bzw. -ressourcengruppe oder ein Abonnement auswählen.


## <a name="create-an-alert-rule-for-an-activity-log"></a>Erstellen einer Warnungsregel für ein Aktivitätsprotokoll

> [!NOTE]

>  Stellen Sie beim Erstellen der Warnungsregeln Folgendes sicher:

> - Das Abonnement im Bereich unterscheidet sich nicht von dem Abonnement, unter dem die Warnung erstellt wird.
- Folgende Kriterien müssen verwendet werden: Ebene/Status/Aufrufer/Ressourcengruppe/Ressourcen-ID/Ressourcentyp/Ereigniskategorie, unter der die Warnung konfiguriert wird.
- Der JSON-Code der Warnungskonfiguration enthält keine „anyOf“-Bedingung oder geschachtelten Bedingungen (nur ein „allOf“-Element ohne weitere „allOf“/„anyOf“-Elemente ist zulässig).


Gehen Sie dazu wie folgt vor:

1. Wählen Sie im Azure-Portal die Option **Überwachen** > **Warnungen (Vorschauversion)**.
2. Klicken Sie oben im Fenster **Warnungen (Vorschauversion)** auf **Neue Warnungsregel**.

     ![Neue Warnungsregel](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule.png)

     Das Fenster **Regel erstellen** wird angezeigt.

      ![Optionen für neue Warnungsregel](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule-options.png)

3. Geben Sie unter **Warnungsbedingung definieren** die folgenden Informationen an, und klicken Sie auf **Fertig**.

    - **Warnungsziel**: Verwenden Sie zum Anzeigen und Auswählen des Ziels für die neue Warnung die Option **Nach Abonnement filtern** / **Nach Ressourcentyp filtern**, und wählen Sie die Ressource oder Ressourcengruppe aus der angegebenen Liste aus.

    > [!NOTE]

    > Sie können eine Ressource, eine Ressourcengruppe oder ein gesamtes Abonnement auswählen.

    **Beispielansicht für Warnungsziel**

     ![Auswählen des Ziels](./media/monitoring-activity-log-alerts-new-experience/select-target.png)

    - Klicken Sie unter **Zielkriterien** auf **Kriterien hinzufügen**, und wählen Sie als Signaltyp **Aktivitätsprotokoll** aus.

    - Wählen Sie das Signal in der angegebenen Liste aus.

    Sie können die Zeitachse für den Protokollverlauf und die entsprechende Warnungslogik für dieses Zielsignal auswählen:

    **Anzeige „Kriterien hinzufügen“**

    ![Kriterien hinzufügen](./media/monitoring-activity-log-alerts-new-experience/add-criteria.png)

    **History time** (Verlaufszeitraum): Letzte 6/12/24 Stunden, Letzte Woche.

    **Warnungslogik**:

        - **Event Level**- The severity level of the event.**Verbose,Informational, Warning, Error**, or **Critical**.
        - **Status**: The status of the event.**Started, Failed**, or **Succeeded**.
        - **Event initiated by**: Also known as the caller; The email address or Azure Active Directory identifier of the user who performed the operation.

        **Sample signal graph with alert logic applied** :

        ![ criteria selected](./media/monitoring-activity-log-alerts-new-experience/criteria-selected.png)

4. Geben Sie unter **define alert rules details** (Informationen zu Warnungsregeln definieren) die folgenden Details an:

    - **Name der Warnungsregel**: Der Name für die neue Warnungsregel.
    - **Beschreibung**: Die Beschreibung für die neue Warnungsregel.
    - **Warnung in Ressourcengruppe speichern**: Wählen Sie die Ressourcengruppe aus, in der Sie die neue Regel speichern möchten.

5. Geben Sie unter **Aktionsgruppe** im Dropdownmenü die Aktionsgruppe an, der Sie die neue Warnungsregel zuweisen möchten. Alternativ hierzu können Sie auch [eine neue Aktionsgruppe erstellen](monitoring-action-groups.md) und der neuen Regel zuweisen. Klicken Sie zum Erstellen einer neuen Gruppe auf **+ Neue Gruppe**.

6. Klicken Sie zum Aktivieren der Regeln nach der Erstellung für **Regel beim Erstellen aktivieren** auf **Ja**.
7. Klicken Sie auf **Warnungsregel erstellen**.

    Die neue Warnungsregel für das Aktivitätsprotokoll wird erstellt, und oben rechts im Fenster wird eine Bestätigungsmeldung angezeigt.

    ![ Warnungsregel erstellt](./media/monitoring-activity-log-alerts-new-experience/alert-created.png)

    Sie können eine Regel aktivieren, deaktivieren, bearbeiten oder löschen. [Erfahren Sie mehr](#view-and-manage-activity-log-alert-rules) zur Verwaltung von Aktivitätsprotokollregeln.

## <a name="view-and-manage-activity-log-alert-rules"></a>Anzeigen und Verwalten von Regeln für Aktivitätsprotokollwarnungen

1. Klicken Sie im Azure-Portal auf **Überwachen** > **Warnungen (Vorschauversion)** und dann oben links im Fenster auf **Regeln verwalten**.

    ![ Verwalten von Warnungsregeln](./media/monitoring-activity-log-alerts-new-experience/manage-alert-rules.png)

    Die Liste mit den verfügbaren Regeln wird angezeigt.

2. Suchen Sie nach der zu ändernden Aktivitätsprotokollregel.

    ![ Suche nach Regel für Aktivitätsprotokollwarnungen](./media/monitoring-activity-log-alerts-new-experience/searth-activity-log-rule-to-edit.png)

    Sie können die verfügbaren Filter **Abonnement**, **Ressourcengruppe** und **Ressource** verwenden, um die Aktivitätsregel zu ermitteln, die Sie bearbeiten möchten.

    > [!NOTE]

    > Sie können nur **Beschreibung**, **Zielkriterien** und **Aktionsgruppen** bearbeiten.

3.  Wählen Sie die Regel aus, und doppelklicken Sie darauf, um die Regeloptionen zu bearbeiten. Nehmen Sie die erforderlichen Änderungen vor, und klicken Sie dann auf **Speichern**.

    ![ Verwalten von Warnungsregeln](./media/monitoring-activity-log-alerts-new-experience/activity-log-rule-edit-page.png)

4.  Sie können eine Regel deaktivieren, aktivieren oder löschen. Wählen Sie oben im Fenster die entsprechende Option, nachdem Sie die Regel wie in Schritt 2 beschrieben ausgewählt haben.


## <a name="next-steps"></a>Nächste Schritte

- [Archivieren des Azure-Aktivitätsprotokolls](monitoring-archive-activity-log.md)
- [Streamen des Azure-Aktivitätsprotokolls an Event Hubs](monitoring-stream-activity-logs-event-hubs.md)
- [Migrieren von Azure-Warnungen für Verwaltungsereignisse zu Aktivitätsprotokollwarnungen](monitoring-migrate-management-alerts.md)
