---
title: Was ist Azure Monitor für VMs (Vorschauversion)? | Microsoft-Dokumentation
description: Azure Monitor for VMs ist ein Feature von Azure Monitor, das Integritäts- und Leistungsüberwachung des Azure VM-Betriebssystems mit der automatischen Erkennung von Anwendungskomponenten und Abhängigkeiten mit anderen Ressourcen kombiniert und die Kommunikation unter ihnen als Zuordnung darstellt. Dieser Artikel bietet eine Übersicht.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2019
ms.author: magoedte
ms.openlocfilehash: 0733b4179793e88759ca1c3330880546650fbe09
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856374"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>Was ist Azure Monitor für VMs (Vorschauversion)?

Azure Monitor für VMs überwacht Ihre virtuellen Azure-Computer (VM) und VM-Skalierungsgruppen bedarfsorientiert. Der Dienst analysiert die Leistung und Integrität Ihrer Windows- und Linux-VMs und überwacht deren Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen. 

Als Lösung umfasst Azure Monitor für VMs die Unterstützung für die Überwachung von Leistungs- und Anwendungsabhängigkeiten für VMs, die lokal oder bei einem anderen Cloudanbieter gehostet sind. Drei wichtige Features bieten tiefgreifende Einblicke:

* **Logische Komponenten von virtuellen Azure-Computern mit Windows und Linux**: Werden an vorkonfigurierten Integritätskriterien gemessen und benachrichtigen Sie, wenn die ausgewertete Bedingung erfüllt ist.  

* **Vordefinierte, beliebte Leistungsdiagramme**: Zeigen Kernleistungsmetriken des Gast-VM-Betriebssystems an.

* **Abhängigkeitsdiagramm**: Zeigt die mit der VM verbundenen Komponenten aus verschiedenen Ressourcengruppen und Abonnements an.  

Die Features sind in Form von drei Perspektiven geordnet:

* Health
* Leistung
* Map

>[!NOTE]
>Derzeit wird das Integritätsfeature nur für virtuelle Azure-Computer und VM-Skalierungsgruppen angeboten. Die Features „Leistung“ und „Zuordnung“ unterstützen sowohl Azure-VMs als auch virtuelle Computer, die in Ihrer Umgebung oder von anderen Cloudanbieter gehostet werden.

Die Integration in Log Analytics ermöglicht die leistungsstarke Aggregation und Filterung sowie die Möglichkeit zum Analysieren von Datentrends im zeitlichen Verlauf. Eine derartige umfassende Überwachung Ihrer Workloads kann nicht mit Azure Monitor, der Dienstzuordnung oder Log Analytics allein erreicht werden.  

Sie können diese Daten direkt in einem einzelnen virtuellen Computer anzeigen, oder Sie können eine aggregierte Ansicht Ihrer virtuellen Computer mit Azure Monitor bereitstellen. Diese Ansicht basiert auf der Perspektive der einzelnen Features:

* **Integrität**: Die virtuellen Computer beziehen sich auf eine Ressourcengruppe.
* **Zuordnung** und **Leistung**: Die virtuellen Computer sind für das Berichten an einen bestimmten Log Analytics-Arbeitsbereich konfiguriert.

![VM-Insights-Perspektive im Azure-Portal](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

Azure Monitor für VMs kann eine vorhersehbare Leistung und Verfügbarkeit wichtiger Anwendungen bieten. Es identifiziert kritische Ereignisse des Betriebssystems, Leistungsengpässe und Netzwerkprobleme. Azure Monitor für VMs kann außerdem Aufschluss darüber geben, ob ein Problem mit weiteren Abhängigkeiten zusammenhängt.  

## <a name="data-usage"></a>Datennutzung 

Wenn Sie Azure Monitor für VMs bereitstellen, werden die von Ihren virtuellen Computern gesammelten Daten erfasst und in Azure Monitor gespeichert. Basierend auf den Preisen, die auf der [Seite mit der Azure Monitor-Preisübersicht](https://azure.microsoft.com/pricing/details/monitor/) veröffentlicht sind, wird Azure Monitor für VMs für Folgendes abgerechnet:
* Die erfassten und gespeicherten Daten.
* Die Anzahl der auf Integritätskriterien bezogenen metrischem Zeitreihen, die überwacht werden.
* Die erstellten Warnungsregeln.
* Die gesendeten Benachrichtigungen. 

Die Protokollgröße unterscheidet sich je nach der Zeichenfolgenlänge von Indikatoren und kann mit der Anzahl der logischen Datenträger und Netzwerkadapter anwachsen. Wenn Sie bereits über einen Arbeitsbereich verfügen und diese Indikatoren sammeln, erfolgt keine doppelte Belastung. Wenn Sie bereits die Dienstzuordnung verwenden, besteht die einzige für Sie sichtbare Veränderung in den zusätzlichen Verbindungsdaten, die an Azure Monitor gesendet werden.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Anforderungen und Methoden, die Ihnen beim Überwachen Ihrer virtuellen Computer helfen, finden Sie unter [Bereitstellen von Azure Monitor für VMs](vminsights-onboard.md).
