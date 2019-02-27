---
title: Häufig gestellte Fragen zu Azure Monitor für Container | Microsoft-Dokumentation
description: Azure Monitor für Container ist eine Lösung, die die Integrität Ihrer AKS-Cluster und Containerinstanzen in Azure überwacht. Dieser Artikel beantwortet häufig gestellte Fragen.
services: azure-monitor
author: mgoedtel
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2019
ms.author: magoedte
ms.openlocfilehash: 27a191bb62ae59aa154167a22c99d3e699f3eb5a
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418429"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Monitor für Container
Dieser Microsoft-Artikel enthält eine Liste häufig gestellter Fragen zu Azure Monitor für Container. Wenn Sie weitere Fragen zur Lösung haben, besuchen Sie das [Diskussionsforum](https://feedback.azure.com/forums/34192--general-feedback), und stellen Sie Ihre Fragen. Wenn eine Frage häufiger gestellt wird, fügen wir sie diesem Artikel hinzu, damit sie schnell und einfach gefunden werden kann.

## <a name="why-am-i-not-seeing-data-if-log-analytics-workspace-is-configured-with-the-free-pricing-tier"></a>Warum sehe ich keine Daten, wenn der Log Analytics-Arbeitsbereich mit dem Tarif „Free“ konfiguriert ist? 

Möglicherweise haben Sie das standardmäßige Limit von 500 MB erreicht oder eine tägliche Obergrenze festgelegt, um die Menge der täglich zu sammelnden Daten zu kontrollieren. Um Ihre Datennutzung zu überprüfen und zu verwalten, lesen Sie [Protokollieren von Datennutzung und Kosten](../platform/manage-cost-storage.md). 

## <a name="what-are-the-states-of-containers-specified-in-the-containerinventory-table"></a>Welche Zustände von Containern sind in der Tabelle „ContainerInventory“ angegeben?
Die Tabelle „ContainerInventory“ enthält Informationen zu beendeten und ausgeführten Containern. Die Tabelle wird von einem Workflow innerhalb des Agent ausgefüllt, der den Docker nach allen Containern abfragt (ausgeführte und beendete) und diese Daten an den Log Analytics-Arbeitsbereich weiterleitet.
 
## <a name="how-do-i-resolve-errors-related-to-missing-subscription-registration-for-microsoftoperationsmanagement"></a>Wie behebe ich Fehler im Zusammenhang mit **Fehlende Abonnementregistrierung für Microsoft.OperationsManagement**?
Um den Fehler zu beheben, registrieren Sie den Ressourcenanbieter **Microsoft.OperationsManagement** in dem Abonnement, in dem der Arbeitsbereich definiert ist. Die Dokumentation für diesen Vorgang finden Sie [hier](../../azure-resource-manager/resource-manager-register-provider-errors.md).

## <a name="does-azure-monitor-for-containers-include-support-for-rbac-enabled-aks-clusters"></a>Enthält Azure Monitor für Container Support für RBAC-fähige AKS-Cluster?
RBAC-fähige AKS Cluster werden derzeit von der Lösung nicht unterstützt. Die Seite mit den Lösungsdetails zeigt möglicherweise nicht die richtigen Informationen auf den Blättern an, die Daten für diese Cluster anzeigen.

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Wie aktiviere ich die Protokollsammlung für Container im Kube-System-Namespace über Helm?
Die Protokollsammlung von Containern im Kube-System-Namespace ist standardmäßig deaktiviert. Die Protokollsammlung kann durch das Festlegen einer Umgebungsvariablen auf dem Omsagent aktiviert werden. Weitere Informationen finden Sie auf der GitHub-Seite [Azure Monitor für Container](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers). 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>Wie aktualisiere ich den Omsagent auf die neueste Version?
Informationen zum Upgrade des Agent, finden Sie unter [Agentverwaltung](container-insights-manage-agent.md).

## <a name="how-do-i-enable-multi-line-logging"></a>Wie aktiviere ich die mehrzeilige Protokollierung?
Derzeit unterstützt Azure Monitor für Container keine mehrzeilige Protokollierung, aber es gibt Workarounds. Sie können alle Dienste so konfigurieren, dass sie im JSON-Format schreiben, und dann schreibt Docker/Moby sie als eine einzelne Zeile.

Beispielsweise können Sie Ihr Protokoll als JSON-Objekt umschließen, wie im folgenden Beispiel für eine Beispiel node.js-Anwendung gezeigt:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Diese Daten sehen im Azure Monitor für Protokolle bei der Abfrage wie folgt aus:

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Eine ausführliche Betrachtung des Problems finden Sie unter folgendem [Github-Link](https://github.com/moby/moby/issues/22920).

## <a name="how-do-i-resolve-azure-active-directory-errors-when-i-enable-live-logs"></a>Wie löse ich Azure Active Directory-Fehler, wenn ich die Live-Protokollen aktivieren? 
Es wird möglicherweise der folgende Fehler angezeigt: **The reply url specified in the request does not match the reply urls configured for the application: '60b4dec7-5a69-4165-a211-12c40b5c0435'** (Die in der Anforderung angegebene Antwort-URL entspricht nicht den für die Anwendung konfigurierten Anwort-URLs: 60b4dec7-5a69-4165-a211-12c40b5c0435). Die Lösung für dieses Problem finden Sie im Artikel [Anzeigen von Containerprotokollen in Echtzeit mit Azure Monitor für Container (Vorschauversion)](container-insights-live-logs.md#configure-aks-with-azure-active-directory). 

## <a name="next-steps"></a>Nächste Schritte
Um in die Überwachung Ihrer AKS-Cluster einzusteigen, lesen Sie [Onboarding von Azure Monitor für Container](container-insights-onboard.md), um ein Verständnis für die Anforderungen und verfügbaren Methoden zum Aktivieren von Überwachung zu entwickeln. 