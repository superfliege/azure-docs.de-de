---
title: Verwalten von Warnungen aus anderen Überwachungsdiensten in Azure Monitor
description: Verwalten von Nagios-, Zabbix- und SCOM-Warnungen in Azure Monitor
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: bc597d42fe89c0e03c4af1db3a935031b9043a98
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345147"
---
# <a name="manage-alerts-from-other-monitoring-services"></a>Verwalten von Warnungen aus anderen Überwachungsdiensten

Sie können jetzt Ihre Benachrichtigungen von Nagios, Zabbix und System Center Operations Manager über die [einheitliche Oberfläche „Warnungen“](https://aka.ms/azure-alerts-overview) anzeigen. Diese Warnungen stammen von der Integration mit Nagios-/Zabbix-Servern oder System Center Operations Manager in Log Analytics. 

## <a name="prerequisites"></a>Voraussetzungen
Alle Datensätze im Log Analytics-Repository mit einem Warnungstyp werden in die einheitliche Oberfläche „Warnungen“ importiert, daher müssen Sie die Konfiguration durchführen, die zum Erfassen dieser Datensätze erforderlich ist.
1. Für Warnungen von **Nagios** und **Zabbix** müssen Sie [diese Server konfigurieren](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents), sodass sie Warnungen an Log Analytics senden.
1. Für **System Center Operations Manager**-Warnungen müssen Sie Ihre [Operations Manager-Verwaltungsgruppe mit Ihrem Log Analytics-Arbeitsbereich verbinden](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Alle in System Center Operations Manager erstellten Warnungen werden in Log Analytics importiert.

## <a name="view-your-alert-instances"></a>Anzeigen Ihrer Warnungsinstanzen
Nachdem Sie den Import in Log Analytics konfiguriert haben, können Sie mit der Anzeige von Warnungsinstanzen aus diesen Überwachungsdiensten in der [einheitlichen Oberfläche „Warnungen“](https://aka.ms/azure-alerts-overview) beginnen. Sobald sie auf der einheitlichen Oberfläche „Warnungen“ vorhanden sind, können Sie [Ihre Warnungsinstanzen verwalten](https://aka.ms/managing-alert-instances), [die für diese Warnungen erstellten intelligenten Gruppen verwalten](https://aka.ms/managing-smart-groups) und [den Zustand der Warnungen und intelligenten Gruppen ändern](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  Nagios-Warnungen sind auf der einheitlichen Oberfläche „Warnungen“ nicht zustandsbehaftet. Die [Überwachungsbedingung](https://aka.ms/azure-alerts-overview) einer Warnung wechselt z. B. nicht von „Ausgelöst“ zu „Gelöst“. Stattdessen werden sowohl „Ausgelöst“ als auch „Gelöst“ als separate Warnungsinstanzen angezeigt. 
