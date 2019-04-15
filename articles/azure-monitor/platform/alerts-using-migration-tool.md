---
title: Migrieren Ihrer klassischen Warnungen in Azure Monitor mithilfe des freiwilligen Migrationstools
description: Hier erfahren Sie, wie Sie das freiwillige Migrationstool verwenden, um Ihre klassischen Warnungsregeln zu migrieren.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 58c664beee942fe7115c7fff38a039c23bed6ac3
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631645"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Verwenden des freiwilligen Migrationstools zum Migrieren Ihrer klassischen Warnungsregeln

Wie [bereits angekündigt](monitoring-classic-retirement.md) werden klassische Warnungen in Azure Monitor im Juli 2019 eingestellt. Das Migrationstool zum freiwilligen Auslösen der Migration steht im Azure-Portal bereit, und der Rollout erfolgt für Kunden, die klassische Warnungsregeln verwenden. In diesem Artikel werden Sie durch die Schritte zur Verwendung des Migrationstools für das freiwillige Migrieren Ihrer klassischen Warnungsregeln vor Beginn der automatischen Migration im Juli 2019 geführt.

## <a name="benefits-of-new-alerts"></a>Vorteile der neuen Warnungen

Klassische Warnungen werden durch neue einheitliche Warnungen in Azure Monitor ersetzt. Die neue Warnungsplattform bietet die folgenden Vorteile:

- Warnungen für eine Vielzahl mehrdimensionaler Metriken für [viele weitere Azure-Dienste](alerts-metric-near-real-time.md#metrics-and-dimensions-supported).
- Neue Metrikwarnungen unterstützen [Warnungsregeln für mehrere Ressourcen](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor), wodurch der erforderliche Aufwand zur Verwaltung vieler Regeln erheblich reduziert wird.
- Einheitlicher Benachrichtigungsmechanismus
  - [Aktionsgruppen](action-groups.md) ist ein modularer Benachrichtigungsmechanismus, der mit allen neuen Warnungstypen (Metrik, Protokoll und Aktivitätsprotokoll) verwendet werden kann.
  - Sie können auch neue Benachrichtigungsmechanismen wie SMS, Sprache und ITSM-Connector nutzen.
- Die [einheitliche Oberfläche für Warnungen](alerts-overview.md) führt alle Warnungen für unterschiedliche Signale (Metrik, Aktivitätsprotokoll und Protokoll) an einem zentralen Ort zusammen.

## <a name="before-you-migrate"></a>Vor der Migration

Im Rahmen der Migration werden klassische Warnungsregeln in entsprechende neue Warnungsregeln konvertiert und Aktionsgruppen werden erstellt.

- Das Nutzlastformat für Benachrichtigungen sowie die APIs zum Erstellen und Verwalten neuer Warnungsregeln unterscheiden sich von denen der klassischen Warnungsregeln, da mehr Funktionen unterstützt werden. Lesen Sie Informationen zum [Vorbereiten der Migration](alerts-prepare-migration.md).

- Einige klassische Warnungsregeln können mit dem Tool nicht migriert werden. [Informieren Sie sich, welche Regeln nicht migrierbar sind, und erfahren Sie, wie Sie diese migrieren können](alerts-understand-migration.md#which-classic-alert-rules-can-be-migrated).

    > [!NOTE]
    > Der Migrationsprozess hat keine Auswirkung auf die Auswertung Ihrer klassischen Warnungsregeln. Diese werden weiterhin ausgeführt und senden Warnungen, bis sie migriert wurden und neue Warnungsregeln mit der Auswertung beginnen.


## <a name="how-to-use-the-migration-tool"></a>Verwenden des Migrationstools

Das folgende Verfahren beschreibt, wie Sie die Migration Ihrer klassischen Warnungsregeln im Azure-Portal auslösen:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Überwachen**.

2. Klicken Sie auf **Warnungen** und dann auf **Warnungsregeln verwalten** oder **Klassische Warnungen anzeigen**.

3. Klicken Sie auf **Zu neuen Regeln migrieren**, um die Landing Page der Migration aufzurufen. Auf dieser Seite sind alle Ihre Abonnements und der jeweilige Migrationsstatus aufgelistet.

    ![Landing Page der Migration](media/alerts-migration/migration-landing.png "Regeln migrieren")

4. Alle Abonnements, die mit dem Tool migriert werden können, sind als **Bereit zur Migration** gekennzeichnet.

    > [!NOTE]
    > Der Rollout des Migrationstools erfolgt in mehreren Phasen für alle Abonnements, die klassische Warnungsregeln verwenden. In den frühen Phasen des Rollouts werden möglicherweise einige Abonnements als nicht bereit zur Migration angezeigt.

5. Wählen Sie ein oder mehrere Abonnements aus, und klicken Sie auf **Migrationsvorschau anzeigen**

6. Auf dieser Seite werden die Details der klassischen Warnungsregeln angezeigt, die jeweils für ein Abonnement migriert werden. Sie können auch die **Migrationsdetails für dieses Abonnement herunterladen** (in einem CSV-Format).

    ![Migrationsvorschau](media/alerts-migration/migration-preview.png "Migrationsvorschau anzeigen")

7. Geben Sie eine oder mehrere **E-Mail-Adressen** an, die über den Migrationsstatus benachrichtigt werden sollen. Wir senden eine E-Mail, wenn die Migration abgeschlossen oder eine Aktion von Ihnen erforderlich ist.

8. Klicken Sie auf **Migration starten**. Lesen Sie die Informationen im Bestätigungsdialogfeld, und bestätigen Sie, wenn Sie zum Starten des Migrationsprozesses bereit sind.

    >[!IMPORTANT]
    > Sobald Sie den Migrationsprozess für ein Abonnement initiiert haben, können Sie keine klassischen Warnungsregeln mehr für das Abonnement bearbeiten/erstellen. Ihre klassischen Warnungsregeln werden jedoch weiter ausgeführt und stellen Warnungen bereit, bis sie vollständig migriert wurden. Dadurch wird die Übereinstimmung zwischen klassischen Warnungsregeln und den neuen Regeln, die während der Migration erstellt werden, gewährleistet. Sobald die Migration für Ihr Abonnement abgeschlossen ist, können Sie keine klassischen Warnungsregeln mehr verwenden.

    ![Migrationsbestätigung](media/alerts-migration/migration-confirm.png "Migrationsstart bestätigen")

9. Wenn die Migration abgeschlossen oder eine Aktion von Ihnen erforderlich ist, wird eine E-Mail an die in Schritt 8 angegebenen E-Mail-Adressen gesendet. Sie können den Status auch regelmäßig auf der Landing Page der Migration im Portal überprüfen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="why-is-my-subscriptions-listed-as-not-ready-for-migration"></a>**Warum sind meine Abonnements als nicht bereit zur Migration aufgeführt?**

Der Rollout des Migrationstools an alle Kunden erfolgt in mehreren Phasen. In den frühen Phasen sind möglicherweise die meisten oder alle Ihre Abonnements als **Nicht bereit zur Migration** gekennzeichnet. Bis Mitte April sollten jedoch alle Abonnements zur Migration bereit sein.

Sobald ein Abonnement zur Migration bereit ist, werden Abonnementbesitzer per E-Mail über die Verfügbarkeit des Tools benachrichtigt. Achten Sie auf diese Benachrichtigung.

### <a name="who-can-trigger-the-migration"></a>**Wer kann die Migration auslösen?**

Benutzer, denen die Rolle „Überwachungsmitwirkender“ auf Abonnementebene zugewiesen ist, können die Migration auslösen. Erfahren Sie mehr über die [rollenbasierte Zugriffssteuerung für den Migrationsprozess](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-is-the-migration-going-to-take"></a>**Wie lange dauert die Migration?**

Bei den meisten Abonnements ist die Migration in der Regel nach weniger als einer Stunde abgeschlossen. Sie können den Migrationsfortschritt auf der Landing Page der Migration verfolgen.  Sie können sicher sein, dass während dieser Zeit Ihre Warnungen weiterhin entweder im klassischen Warnungssystem oder im neuen System ausgeführt werden.

### <a name="what-can-i-do-if-i-run-into-an-issue-during-migration"></a>**Was kann ich tun, wenn während der Migration ein Problem auftritt?**

Suchen Sie im [Leitfaden zur Problembehandlung nach Abhilfemaßnahmen für Probleme, die während der Migration auftreten können](alerts-understand-migration.md#common-issues-and-remediations). Wenn zum Abschließen der Migration eine Aktion von Ihnen erforderlich ist, werden Sie während der Migration über die angegebenen E-Mail-Adressen benachrichtigt.

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Migration](alerts-prepare-migration.md)
- [Grundlegendes zur Funktionsweise des Migrationstools](alerts-understand-migration.md)
