---
title: Leitfaden für in Azure Log Analytics gespeicherte personenbezogene Daten | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie personenbezogene Daten verwalten, die in Azure Log Analytics gespeichert sind, und wie Sie diese Daten identifizieren und entfernen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 3692c83a4991fc67ec176687bd076ab14e4c640d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129369"
---
# <a name="guidance-for-personal-data-stored-in-log-analytics"></a>Leitfaden für in Azure Log Analytics gespeicherte personenbezogene Daten

Log Analytics ist ein Datenspeicher, der wahrscheinlich auch personenbezogene Daten enthält. In diesem Artikel erfahren Sie, wo sich solche Daten üblicherweise in Log Analytics befinden und welche Möglichkeiten Sie im Zusammenhang mit diesen Daten haben.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Strategie für den Umgang mit personenbezogenen Daten

Die Gestaltung der Strategie für den Umgang mit personenbezogenen Daten liegt letztendlich bei Ihnen und Ihrem Unternehmen. Im Anschluss finden Sie einige mögliche Herangehensweisen. Diese sind absteigend nach technischer Präferenz sortiert:

* Sorgen Sie nach Möglichkeit dafür, dass Daten, die als „personenbezogen“ betrachtet werden können, gar nicht erst gesammelt oder aber verschleiert, anonymisiert oder anderweitig verändert werden. Dies ist _mit Abstand_ die bevorzugte Vorgehensweise und erspart die Erstellung einer wirksamen und äußerst kostspieligen Datenverarbeitungsstrategie.
* Sollte diese Vorgehensweise nicht möglich sein, versuchen Sie, die Daten zu normalisieren, um die Auswirkungen auf die Datenplattform und auf die Leistung zu verringern. Statt beispielsweise eine explizite Benutzer-ID zu protokollieren, erstellen Sie Nachschlagedaten, die den Benutzernamen und die dazugehörigen Details mit einer internen ID korrelieren, die dann an anderer Stelle protokolliert werden kann. Wenn Sie daraufhin von einem Benutzer gebeten werden, seine personenbezogenen Informationen zu löschen, muss ggf. nur die entsprechende Zeile in der Nachschlagetabelle gelöscht werden. 
* Falls personenbezogene Daten gesammelt werden müssen, erstellen Sie einen Prozess basierend auf dem Bereinigungs-API-Pfad und dem vorhandenen Abfrage-API-Pfad, um allen Vorschriften zu genügen, die möglicherweise im Zusammenhang mit dem Exportieren und Löschen personenbezogener Daten eines Benutzers gelten. 

## <a name="where-to-look-for-private-data-in-log-analytics"></a>Wo befinden sich personenbezogene Daten in Log Analytics?

Log Analytics ist ein flexibler Datenspeicher, der zwar ein Schema für die Daten vorgibt, jedoch für jedes Feld das Überschreiben mit benutzerdefinierten Werten zulässt. Darüber hinaus können beliebige benutzerdefinierte Schemas erfasst werden. Daher ist es unmöglich zu sagen, wo genau personenbezogene Daten in Ihrem jeweiligen Arbeitsbereich vorhanden sind. Bei den folgenden Speicherorten handelt es sich jedoch um gute Ausgangspunkte für die Suche in Ihrem Datenbestand:

* *IP-Adressen:* Log Analytics erfasst verschiedenste IP-Informationen für viele verschiedene Tabellen. Die folgende Abfrage zeigt beispielsweise alle Tabellen, in denen in den letzten 24 Stunden IPv4-Adressen erfasst wurden:
    ```
    search * 
    | where * matches regex @'\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(\.|$)){4}\b' //RegEx originally provided on https://stackoverflow.com/questions/5284147/validating-ipv4-addresses-with-regexp
    | summarize count() by $table
    ```
* *Benutzer-IDs:* Benutzer-IDs befinden sich in verschiedensten Lösungen und Tabellen. Mit dem Suchbefehl können Sie Ihr gesamtes Dataset nach einem bestimmten Benutzernamen durchsuchen:
    ```
    search "[username goes here]"
    ```
Wichtig: Suchen Sie nicht nur nach lesbaren Benutzernamen, sondern auch nach GUIDs, die direkt zu einem bestimmten Benutzer zurückverfolgt werden können.
* *Geräte-IDs:* Geräte-IDs sind mit Benutzer-IDs vergleichbar und werden gelegentlich als „privat“ betrachtet. Verwenden Sie die gleiche Vorgehensweise, die weiter oben für Benutzer-IDs angegeben ist, um Tabellen zu ermitteln, bei denen dies ggf. relevant ist. 
* *Benutzerdefinierte Daten:* In Log Analytics stehen verschiedene Erfassungsmethoden zur Verfügung: benutzerdefinierte Protokolle und benutzerdefinierte Felder, die [HTTP-Datensammler-API](log-analytics-data-collector-api.md) und benutzerdefinierte Daten, die im Rahmen von Systemereignisprotokollen erfasst werden. Alle diese Optionen können personenbezogene Daten enthalten und müssen auf entsprechende Daten untersucht werden.
* *Durch die Lösung erfasste Daten:* Da es sich bei dem Lösungsmechanismus um einen Mechanismus mit offenem Ende handelt, empfiehlt es sich, alle von Lösungen generierten Tabellen zu überprüfen, um die Compliance zu gewährleisten.

## <a name="how-to-export-and-delete-private-data"></a>Exportieren und Löschen personenbezogener Daten

Wie bereits weiter oben im Abschnitt [Strategie für den Umgang mit personenbezogenen Daten](#strategy-for-personal-data-handling) erwähnt, wird __dringend__ empfohlen, die Datensammlungsrichtlinien nach Möglichkeit so zu strukturieren, dass personenbezogene Daten gar nicht erst gesammelt oder aber verschleiert, anonymisiert oder anderweitig verändert werden, sodass sie nicht mehr als „personenbezogen“ anzusehen sind. Der Umgang mit den Daten ist in erster Linie mit Kosten für Sie und Ihr Team verbunden: Sie müssen eine Strategie ausarbeiten und automatisieren sowie eine Schnittstelle entwickeln, über die Ihre Kunden mit ihren Daten interagieren können. Außerdem fallen laufende Wartungskosten an. Darüber hinaus bedeutet dies einen hohen Rechenaufwand für Log Analytics, und eine hohe Anzahl gleichzeitiger Abfrage- oder Bereinigungs-API-Aufrufe kann sich negativ auf alle anderen Interaktionen mit Log Analytics-Funktionen auswirken. Allerdings gibt es tatsächlich einige Szenarien, in denen personenbezogene Daten erfasst werden müssen. In diesen Fällen sollten Sie die Daten wie in diesem Abschnitt beschrieben handhaben.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Anzeigen und Exportieren

Sowohl bei Anforderungen zum Anzeigen von Daten als auch bei Anforderungen zum Exportieren von Daten sollte jeweils die [Abfrage-API](https://dev.loganalytics.io/) verwendet werden. Die Logik, mit der die Daten in eine geeignete Form konvertiert werden, um sie für Ihre Benutzer bereitzustellen, muss von Ihnen selbst implementiert werden. [Azure Functions](https://azure.microsoft.com/services/functions/) eignet sich hervorragend zum Hosten einer solchen Logik.

### <a name="delete"></a>Löschen

> [!WARNING]
> Löschvorgänge in Log Analytics sind destruktiv und unumkehrbar. Seien Sie bei der Ausführung äußerst vorsichtig.

Im Rahmen der Vorgehensweisen für den Datenschutz haben wir einen API-Pfad für die *Bereinigung* verfügbar gemacht. Dieser Pfad sollte aufgrund der Risiken im Zusammenhang mit seiner Verwendung, möglichen Leistungseinbußen sowie einer möglichen Verzerrung von Aggregationen, Messungen und anderen Aspekten Ihrer Log Analytics-Daten möglichst selten verwendet werden. Alternative Methoden für den Umgang mit personenbezogenen Daten finden Sie im Abschnitt [Strategie für den Umgang mit personenbezogenen Daten](#strategy-for-personal-data-handling).

Das Bereinigen ist ein Vorgang, der hohe Berechtigungen erfordert, und weder eine App noch ein Benutzer in Azure (einschließlich des Ressourcenbesitzers) ist zur Ausführung berechtigt, sofern nicht in Azure Resource Manager explizit eine entsprechende Rolle zugewiesen wurde. Dies ist die Rolle _Datenpurger_, die aufgrund des möglichen Datenverlusts sehr zurückhaltend vergeben werden sollte. 

Nachdem die Azure Resource Manager-Rolle zugewiesen wurde, sind zwei neue API-Pfade verfügbar: 

* [POST purge] (https://docs.microsoft.com/rest/api/loganalytics/workspaces%202015-03-20/purge): Verwendet ein Objekt, das Parameter der zu löschenden Daten angibt, und gibt eine Verweis-GUID zurück. 
* GET purge status: Der Aufruf von „POST purge“ gibt einen Header vom Typ „x-ms-status-location“ zurück, der eine URL enthält, die Sie zum Ermitteln des Status Ihrer Bereinigungs-API aufrufen können. Beispiel: 

    ```
    x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/Microsoft.OperatonalInsights/workspaces/[WorkspaceName]/operations/purge-[PurgeOperationId]?api-version=2015-03-20
    ```

Die Mehrzahl der Bereinigungsvorgänge wird wahrscheinlich deutlich schneller ausgeführt als in unserer SLA angegeben. Aufgrund der starken Auswirkungen auf die von Log Analytics verwendete Datenplattform wurde die formelle SLA für die Ausführung von Bereinigungsvorgängen aber auf 30 Tage festgelegt. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Sammeln, Verarbeiten und Schützen von Daten finden Sie unter [Log Analytics – Datensicherheit](log-analytics-data-security.md).