---
title: Leitfaden für personenbezogene Daten, die in Azure Application Insights gespeichert sind | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie personenbezogene Daten verwalten, die in Azure Application Insights gespeichert sind, und wie Sie diese Daten identifizieren und entfernen.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.reviewer: Evgeny.Ternovsky
ms.author: mbullwin
ms.openlocfilehash: a59b57c546f18a7d91160f2ae7282af82fc42160
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044712"
---
# <a name="guidance-for-personal-data-stored-in-application-insights"></a>Leitfaden für personenbezogene Daten, die in Application Insights gespeichert sind

Application Insights ist ein Datenspeicher, der wahrscheinlich auch personenbezogene Daten enthält. In diesem Artikel erfahren Sie, wo sich diese Daten üblicherweise in Application Insights befinden und welche Möglichkeiten Sie im Zusammenhang mit diesen Daten haben.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="strategy-for-personal-data-handling"></a>Strategie für den Umgang mit personenbezogenen Daten

Die Gestaltung der Strategie für den Umgang mit personenbezogenen Daten liegt letztendlich bei Ihnen und Ihrem Unternehmen. Im Anschluss finden Sie einige mögliche Herangehensweisen. Diese sind absteigend nach technischer Präferenz sortiert:
* Sorgen Sie nach Möglichkeit dafür, dass Daten, die als _personenbezogen_ betrachtet werden können, gar nicht erst gesammelt oder aber verschleiert, anonymisiert oder anderweitig verändert werden. Diese Methode ist die bevorzugte Vorgehensweise und erspart die Erstellung einer wirksamen und kostspieligen Datenverarbeitungsstrategie.
* Sollte diese Vorgehensweise nicht möglich sein, versuchen Sie, die Daten zu normalisieren, um die Auswirkungen auf die Datenplattform und auf die Leistung zu verringern. Statt beispielsweise eine explizite Benutzer-ID zu protokollieren, erstellen Sie eine Suche nach Daten, die den Benutzernamen und dessen Details mit einer internen ID korreliert, die dann an anderer Stelle protokolliert werden kann. Wenn Sie in diesem Fall von einem Benutzer gebeten werden, seine personenbezogenen Informationen zu löschen, muss möglicherweise nur die Zeile in der Nachschlagetabelle gelöscht werden, die mit dem Benutzer im Zusammenhang steht. 
* Falls personenbezogene Daten gesammelt werden müssen, erstellen Sie einen Prozess basierend auf dem Bereinigungs-API-Pfad und dem vorhandenen Abfrage-API-Pfad, um allen Vorschriften zu genügen, die möglicherweise im Zusammenhang mit dem Exportieren und Löschen personenbezogener Daten eines Benutzers stehen.

## <a name="where-to-look-for-private-data-in-application-insights"></a>Wo muss in Application Insights nach personenbezogenen Daten gesucht werden?

Application Insights ist ein vollständig flexibler Datenspeicher, der zwar ein Schema für die Daten vorgibt, jedoch für jedes Feld das Überschreiben mit benutzerdefinierten Werten zulässt. Daher ist es unmöglich zu sagen, wo genau personenbezogene Daten in Ihrer jeweiligen Anwendung vorhanden sind. Bei den folgenden Speicherorten handelt es sich jedoch um gute Ausgangspunkte für die Suche in Ihrem Datenbestand:

* *IP-Adressen*: Obwohl Application Insights standardmäßig alle IP-Adressfelder mit „0.0.0.0“ verschleiert, ist es durchaus üblich, diesen Wert mit der tatsächlichen Benutzer-IP zu überschreiben, um Sitzungsinformationen zu speichern. Mithilfe der folgenden Analytics-Abfrage kann nach allen Tabellen gesucht werden, die in den letzten 24 Stunden in der Spalte für die IP-Adresse andere Werte als „0.0.0.0“ enthalten haben:
    ```
    search client_IP != "0.0.0.0"
    | where timestamp > ago(1d)
    | summarize numNonObfuscatedIPs_24h = count() by $table
    ```
* *Benutzer-IDs*: Standardmäßig verwendet Application Insights zufällig generierte IDs für Benutzer und die Sitzungsnachverfolgung. Allerdings werden diese Felder häufig überschrieben, um eine ID zu speichern, die für die Anwendung größere Relevanz hat, z.B. Benutzernamen, AAD-GUIDs usw. Diese IDs werden häufig als personenbezogene Daten angesehen und sollten deshalb entsprechend gehandhabt werden. Wir empfehlen, diese IDs immer zu verschleiern oder zu anonymisieren. Felder, in denen diese Werte häufig vorkommen, sind beispielsweise „session_Id“, „user_Id“, „user_AuthenticatedId“, „user_AccountId“ sowie „customDimensions“.
* *Benutzerdefinierte Daten*: Application Insights ermöglicht es Ihnen, eine Gruppe benutzerdefinierter Dimensionen an einen beliebigen Datentyp anzufügen. Bei diesen Dimensionen kann es sich um *beliebige* Daten handeln. Ermitteln Sie mithilfe der folgenden Abfrage alle benutzerdefinierten Dimensionen, die in den letzten 24 Stunden gesammelt wurden:
    ```
    search * 
    | where isnotempty(customDimensions)
    | where timestamp > ago(1d)
    | project $table, timestamp, name, customDimensions 
    ```
* *Daten im Speicher und während der Übertragung*: Application Insights erfasst Ausnahmen, Anforderungen, Abhängigkeitsaufrufe und Ablaufverfolgungen. Personenbezogene Daten können häufig auf Code- und HTTP-Aufrufebene gesammelt werden. Überprüfen Sie die Tabellen für Ausnahmen, Anforderungen, Abhängigkeiten und Ablaufverfolgungen, um solche Daten zu ermitteln. Verwenden Sie nach Möglichkeit [Telemetrieinitialisierer](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling), um diese Daten zu verschleiern.
* *Aufzeichnungen des Momentaufnahmedebuggers*: Mithilfe des Features [Momentaufnahmedebugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) in Application Insights können Sie Debugmomentaufnahmen erfassen, sobald eine Ausnahme in der Produktionsinstanz Ihrer Anwendung abgefangen wird. Momentaufnahmen zeigen die gesamte Stapelüberwachung, die zu den Ausnahmen führt, sowie die Werte für lokale Variablen in jedem Schritt im Stapel. Leider ermöglicht dieses Feature weder das selektive Löschen von Fangpunkten noch den programmgesteuerten Zugriff auf Daten innerhalb der Momentaufnahme. Falls die Standardaufbewahrungsrate für Momentaufnahmen Ihren Compliancevorgaben nicht entspricht, wird empfohlen, das Feature zu deaktivieren.

## <a name="how-to-export-and-delete-private-data"></a>Exportieren und Löschen personenbezogener Daten

Es wird __dringend__ empfohlen, Ihre Richtlinien zur Datensammlung nach Möglichkeit so zu strukturieren, dass das Sammeln personenbezogener Daten deaktiviert ist oder diese Daten verschleiert, anonymisiert oder auf andere Weise verändert werden, sodass sie nicht mehr als „personenbezogen“ anzusehen sind. Der Umgang mit den Daten nach der Sammlung führt zu Kosten für Sie und Ihr Team für das Definieren und Automatisieren einer Strategie, das Erstellen einer Schnittstelle für Ihre Kunden, über die sie mit den eigenen Daten interagieren können, sowie laufende Wartungskosten. Darüber hinaus bedeutet dies einen hohen Rechenaufwand für Application Insights, und eine große Anzahl gleichzeitiger Abfrage- oder Bereinigungs-API-Aufrufe kann sich negativ auf alle anderen Interaktionen mit Application Insights-Funktionalität auswirken. Allerdings gibt es tatsächlich einige gültige Szenarien, in denen personenbezogene Daten erfasst werden müssen. In diesen Fällen sollten Sie die Daten wie in diesem Abschnitt beschrieben handhaben.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="view-and-export"></a>Anzeigen und Exportieren

Sowohl bei Anforderungen zum Anzeigen von Daten als auch bei Anforderungen zum Exportieren von Daten sollte jeweils die [Abfrage-API](https://dev.applicationinsights.io/quickstart) verwendet werden. Das Implementieren einer Logik zum Konvertieren der Daten in eine geeignete Form für die Bereitstellung für Ihre Benutzer liegt in Ihrem Ermessen. [Azure Functions](https://azure.microsoft.com/services/functions/) eignet sich hervorragend zum Hosten einer solchen Logik.

### <a name="delete"></a>Löschen

> [!WARNING]
> Löschvorgänge in Application Insights sind destruktiv und nicht umkehrbarer! Seien Sie bei der Ausführung äußerst vorsichtig.

Im Rahmen der Vorgehensweisen für den Datenschutz haben wir einen „Bereinigungs“-API-Pfad bereitgestellt. Dieser Pfad sollte aufgrund der Risiken im Zusammenhang mit seiner Verwendung, der möglichen Leistungseinbußen sowie einer möglichen Verzerrung von Aggregationen, Messungen und anderen Aspekte Ihrer Application Insights-Daten nur selten verwendet werden. Alternative Methoden für den Umgang mit personenbezogenen Daten finden Sie oben im Abschnitt [Strategie für den Umgang mit personenbezogenen Daten](#strategy-for-personal-data-handling).

Das Bereinigen ist ein Vorgang, der hohe Berechtigungen erfordert, und weder eine App noch ein Benutzer in Azure (einschließlich des Ressourcenbesitzers) ist zur Ausführung berechtigt, sofern nicht in Azure Resource Manager explizit eine entsprechende Rolle zugewiesen wurde. Dies ist die Rolle _Datenpurger_, die aufgrund des möglichen Datenverlusts nur mit großer Sorgfalt vergeben werden sollte.

Nachdem die Azure Resource Manager-Rolle zugewiesen wurde, sind zwei neue API-Pfade mit vollständiger Entwicklerdokumentation und verknüpfter Form für den Aufruf verfügbar:

* [POST purge](https://docs.microsoft.com/rest/api/application-insights/components/purge): Verwendet ein Objekt, das Parameter der zu löschenden Daten angibt, und gibt eine Verweis-GUID zurück.
* GET purge status: Der Aufruf von „POST purge“ gibt einen Header vom Typ „x-ms-status-location“ zurück, der eine URL enthält, die Sie zum Ermitteln des Status Ihrer Bereinigungs-API aufrufen können. Beispiel: 
   ```
   x-ms-status-location: https://management.azure.com/subscriptions/[SubscriptionId]/resourceGroups/[ResourceGroupName]/providers/microsoft.insights/components/[ComponentName]/operations/purge-[PurgeOperationId]?api-version=2015-05-01
   ```

> [!IMPORTANT]
>  Obwohl die Mehrzahl der Bereinigungsvorgänge wesentlich schneller als die SLA ausgeführt werden kann, ist aufgrund der starken Auswirkung auf die von Application Insights verwendete Datenplattform **die formelle SLA für die Ausführung von Bereinigungsvorgängen auf 30 Tage festgelegt**.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Sammeln, Verarbeiten und Sichern von Daten finden Sie unter [Application Insights – Datensicherheit](app-insights-data-retention-privacy.md).