---
title: Funktionsweise des Tools für die freiwillige Migration von Warnungen in Azure Monitor
description: Enthält eine Beschreibung der Funktionsweise des Tools für die Migration von Warnungen und Informationen zur Behebung von Problemen.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: a45a0cff606bc854924d5da0841b26e1cb9031bb
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631656"
---
# <a name="understand-how-the-migration-tool-works"></a>Funktionsweise des Migrationstools

Wie [bereits angekündigt](monitoring-classic-retirement.md) werden klassische Warnungen in Azure Monitor im Juli 2019 eingestellt. Das Migrationstool zum freiwilligen Auslösen der Migration steht im Azure-Portal bereit, und der Rollout erfolgt für Kunden, die klassische Warnungsregeln verwenden.

In diesem Artikel wird Schritt für Schritt beschrieben, wie das Tool für die freiwillige Migration funktioniert. Außerdem wird die Behebung einiger häufiger Probleme beschrieben.

## <a name="which-classic-alert-rules-can-be-migrated"></a>Welche klassischen Warnungsregeln können migriert werden?

Mit dem Tool können fast alle klassischen Warnungsregeln migriert werden, aber es gibt auch einige Ausnahmen. Die folgenden Warnungsregeln können mit dem Tool nicht migriert werden (und werden auch während der automatischen Migration im Juli 2019 nicht migriert).

- Klassische Warnungsregeln für VM-Gastmetriken (sowohl Windows als auch Linux). [Hier finden Sie eine Anleitung zur erneuten Erstellung dieser Warnungsregeln in neuen Metrikwarnungen](#guest-metrics-on-virtual-machines).
- Klassische Warnungsregeln für Metriken von klassischem Speicher. [Hier finden Sie eine Anleitung zur Überwachung Ihrer klassischen Speicherkonten](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Klassische Warnungsregeln in einigen Speicherkontometriken. [Details hierzu finden Sie weiter unten](#storage-account-metrics).

Wenn Ihr Abonnement über klassische Regeln dieser Art verfügt, werden die anderen Regeln migriert, aber für diese Regeln ist eine manuelle Migration erforderlich. Da wir keine automatische Migration bereitstellen können, funktionieren alle vorhandenen klassischen Metrikwarnungen noch bis Juni 2020, damit Sie genügend Zeit für die Umstellung auf neue Warnungen haben. Nach Juni 2019 können aber keine neuen klassischen Warnungen mehr erstellt werden.

### <a name="guest-metrics-on-virtual-machines"></a>Gastmetriken auf virtuellen Computern

Um neue Metrikwarnungen für Gastmetriken erstellen zu können, müssen die Gastmetriken an den benutzerdefinierten Metrikspeicher von Azure Monitor gesendet werden. Befolgen Sie unten die Anleitung, um die Azure Monitor-Senke in den Diagnoseeinstellungen zu aktivieren.

- [Aktivieren von Gastmetriken für Windows-VMs](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Aktivieren von Gastmetriken für Linux-VMs](https://docs.microsoft.com/azure/azure-monitor/platform/collect-custom-metrics-linux-telegraf)

Nachdem die oben genannten Schritte abgeschlossen wurden, können neue Warnungen für Gastmetriken erstellt werden. Die klassischen Warnungen können gelöscht werden, nachdem die neuen Metrikwarnungen erstellt wurden.

### <a name="storage-account-metrics"></a>Speicherkontometriken

Alle klassischen Warnungen in Speicherkonten können migriert werden, mit Ausnahme dieser Warnungen in den folgenden Metriken:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError

Klassische Warnungsregeln für Metriken vom Typ „Percent“ müssen basierend auf der [Zuordnung zwischen alten und neuen Speichermetriken](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics) migriert werden. Schwellenwerte müssen entsprechend geändert werden, da die neue verfügbare Metrik eine absolute Metrik ist.

Die klassischen Warnungsregeln AnonymousThrottlingError und SASThrottlingError müssen in zwei neue Warnungen aufgeteilt werden, da es keine kombinierte Metrik gibt, die über die gleiche Funktionalität verfügt. Schwellenwerte müssen entsprechend angepasst werden.

## <a name="roll-out-phases"></a>Rolloutphasen

Der Rollout des Migrationstools erfolgt für Kunden, die klassische Warnungsregeln verwenden, in mehreren Phasen. **Abonnementbesitzer** erhalten eine E-Mail, wenn das Abonnement für die Migration mit dem Tool bereit ist.

> [!NOTE]
> Während des Rollouts des Tools in Phasen ist es in den frühen Phasen ggf. der Fall, dass die meisten Ihrer Abonnements noch nicht für die Migration bereit sind.

Derzeit ist nur eine **Teilmenge** der Abonnements, die **ausschließlich** über klassische Warnungsregeln mit den folgenden Ressourcentypen verfügen, als migrationsbereit gekennzeichnet. Unterstützung für weitere Ressourcentypen wird in den nächsten Phasen hinzugefügt.

- Microsoft.apimanagement/service
- Microsoft.batch/batchaccounts
- Microsoft.cache/redis
- Microsoft.datafactory/datafactories
- Microsoft.dbformysql/servers
- Microsoft.dbforpostgresql/servers
- Microsoft.eventhub/namespaces
- Microsoft.logic/workflows
- Microsoft.network/applicationgateways
- Microsoft.network/dnszones
- Microsoft.network/expressroutecircuits
- Microsoft.network/loadbalancers
- Microsoft.network/networkwatchers/connectionmonitors
- Microsoft.network/publicipaddresses
- Microsoft.network/trafficmanagerprofiles
- Microsoft.network/virtualnetworkgateways
- Microsoft.search/searchservices
- Microsoft.servicebus/namespaces
- Microsoft.streamanalytics/streamingjobs
- Microsoft.timeseriesinsights/environments
- Microsoft.web/hostingenvironments/workerpools
- Microsoft.web/serverfarms
- Microsoft.web/sites

## <a name="who-can-trigger-the-migration"></a>Wer kann die Migration auslösen?

Alle Benutzer, die auf Abonnementebene über die integrierte Rolle **Mitwirkender an der Überwachung**  verfügen, können die Migration auslösen. Benutzer mit einer benutzerdefinierten Rolle mit den folgenden Berechtigungen können die Migration ebenfalls auslösen:

- */Lesen
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-issues-and-remediations"></a>Häufig Probleme und ihre Lösungen

Nachdem Sie [die Migration ausgelöst haben](alerts-using-migration-tool.md), verwenden wir die angegebenen E-Mail-Adresse(n), um Sie über den Abschluss der Migration oder die erforderliche Ausführung einer Aktion zu benachrichtigen. Im folgenden Abschnitt sind einige häufige Probleme und die zugehörigen Lösungen beschrieben.

### <a name="validation-failed"></a>Fehler bei der Überprüfung

Aufgrund von einigen kürzlich durchgeführten Änderungen an den klassischen Warnungsregeln in Ihrem Abonnement kann das Abonnement nicht migriert werden. Dies ist ein vorübergehendes Problem. Sie können die Migration neu starten, nachdem der Migrationsstatus nach einigen Tagen wieder **Bereit für die Migration** lautet.

### <a name="policyscope-lock-preventing-us-from-migrating-your-rules"></a>Richtlinien-/Bereichssperre verhindert die Migration Ihrer Regeln

Im Rahmen der Migration werden neue Metrikwarnungen und neue Aktionsgruppen erstellt und klassische Warnungsregeln gelöscht (nachdem neue Regeln erstellt wurden). Es besteht aber entweder eine Richtlinien- oder Bereichssperre, die verhindert, dass wir Ressourcen erstellen können. Je nach Richtlinien- oder Bereichssperre können einige oder alle Regeln nicht migriert werden. Sie können dieses Problem beheben, indem Sie die Richtlinien- bzw. Bereichssperre vorübergehend aufheben und die Migration erneut auslösen.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden des Migrationstools](alerts-using-migration-tool.md)
- [Vorbereiten der Migration](alerts-prepare-migration.md)
