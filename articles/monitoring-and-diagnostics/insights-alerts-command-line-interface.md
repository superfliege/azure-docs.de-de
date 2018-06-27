---
title: Verwenden der plattformübergreifenden Azure CLI zum Erstellen klassischer Warnungen für Azure-Dienste | Microsoft-Dokumentation
description: E-Mails oder Benachrichtigungen bzw. URLs (Webhooks) von Websites oder Automatisierung werden ausgelöst bzw. aufgerufen, wenn die von Ihnen angegebenen Bedingungen erfüllt sind.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/24/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 8112b868bc8d2ca2a9478d38ee702d8b3350d48e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287104"
---
# <a name="use-the-cross-platform-azure-cli-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Verwenden der plattformübergreifenden Azure CLI zum Erstellen von klassischen Metrikwarnungen in Azure Monitor für Azure-Dienste 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](insights-alerts-command-line-interface.md)
>
>
> [!NOTE]
> In diesem Artikel erfahren Sie, wie Sie ältere klassische Metrikwarnungen erstellen. Azure Monitor unterstützt jetzt [neuere, bessere Metrikwarnungen](monitoring-near-real-time-metric-alerts.md). Diese Warnungen ermöglichen die Überwachung mehrerer Metriken sowie Warnungen für dimensionale Metriken. Azure CLI-Unterstützung für neuere Metrikwarnungen wird bald verfügbar sein.
>
>

In diesem Artikel erfahren Sie, wie Sie klassische Azure-Metrikwarnungen mit der plattformübergreifenden Befehlszeilenschnittstelle (Azure CLI) einrichten können.

> [!NOTE]
> Azure Monitor ist der neue Name für den Dienst, der bis 25. September 2016 als „Azure Insights“ bezeichnet wurde. Die Namespaces und somit die folgenden hier beschriebenen Befehle enthalten jedoch weiterhin das Wort „insights“.

Möglicherweise erhalten Sie eine auf Metriken basierende Warnung für Ihre Azure-Dienste, oder auf Ereignissen basierende Warnungen, die in Azure auftreten.

* **Metrikwerte**: Die Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert passiert. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch dann, wenn diese Bedingung nicht mehr erfüllt wird.    

* **Aktivitätsprotokollereignisse**: Eine Warnung kann für *jedes* Ereignis ausgelöst werden, oder dann, wenn bestimmte Ereignisse auftreten. Weitere Informationen zu Aktivitätsprotokollen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen (klassisch)](monitoring-activity-log-alerts.md). 

Sie können konfigurieren, dass bei einer klassischen Metrikwarnung Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren 
* Senden von E-Mails an von Ihnen angegebene Adressen.
* Aufrufen eines Webhooks
* Starten der Ausführung eines Azure-Runbooks (derzeit nur über das Azure-Portal).

Sie haben folgende Möglichkeiten zum Konfigurieren von klassischen Metrikwarnregeln und Abrufen zugehöriger Informationen: 

* [Azure-Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure-CLI](insights-alerts-command-line-interface.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Sie können auch Hilfe zu Befehlen erhalten, indem Sie einen Befehl mit **-help** am Ende eingeben. Dies ist ein Beispiel: 

```console
 azure insights alerts -help
 azure insights alerts actions email create -help
 ```

## <a name="create-alert-rules-by-using-azure-cli"></a>Erstellen von Warnungsregeln mit der Azure CLI
1. Nachdem Sie die erforderlichen Komponenten installiert haben, melden Sie sich bei Azure an. Unter [CLI 2.0-Beispiele für den Schnellstart mit Azure Monitor](insights-cli-samples.md) werden die Befehle aufgeführt, die Sie benötigen, um zu beginnen. Mit diesen Befehlen können Sie sich anmelden, sie informieren Sie über das verwendete Abonnement und bereiten Sie auf die Ausführung von Azure Monitor-Befehlen vor.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. Um vorhandene Regeln für eine Ressourcengruppe aufzulisten, verwenden Sie folgendes Format: 

   **azure insights alerts rule list** *[Optionen] &lt;resourceGroup&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. Um eine Regel zu erstellen, benötigen Sie zunächst verschiedene wichtige Informationen.
    * Die **Ressourcen-ID** der Ressource, für die Sie eine Warnung festlegen möchten.
    * Die für diese Ressource verfügbaren **Metrikdefinitionen**.

     Eine Möglichkeit zum Abrufen der Ressourcen-ID ist das Azure-Portal. Falls die Ressource bereits erstellt wurde, wählen Sie sie im Portal aus. Wählen Sie dann auf dem nächsten Blatt im Abschnitt **Einstellungen** die Option **Eigenschaften** aus. **RESSOURCEN-ID** ist ein Feld auf dem nächsten Blatt. 
     
     Sie können auch mithilfe des [Azure-Ressourcen-Explorers](https://resources.azure.com/) die Ressourcen-ID abrufen.

     Es folgt ein Beispiel einer Ressourcen-ID für eine Web-App:

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Um eine Liste der verfügbaren Metriken und Einheiten für diese Metriken für das vorherige Ressourcenbeispiel zu erhalten, geben Sie den folgenden Azure CLI-Befehl ein:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* ist die Granularität der verfügbaren Messung (in 1-Minuten-Intervallen). Durch Verwenden verschiedener Granularitäten erhalten Sie verschiedene Metrikoptionen.
     
4. Geben Sie zum Erstellen einer metrikbasierten Warnregel einen Befehl im folgenden Format ein:

    **azure insights alerts rule metric set***[Optionen] &lt;ruleName&gt;&lt;location&gt;&lt;resourceGroup&gt;&lt;windowSize&gt;&lt;operator&gt;&lt;threshold&gt;&lt;targetResourceId&gt;&lt;metricName&gt;&lt;timeAggregationOperator&gt;*

    Im folgenden Beispiel wird eine Warnung für eine Websiteressource eingerichtet. Die Warnung wird ausgelöst, wenn fünf Minuten durchgängig Datenverkehr empfangen wird. Sie wird erneut ausgelöst, wenn fünf Minuten lang kein Datenverkehr empfangen wird.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. Um einen Webhook zu erstellen oder eine E-Mail zu senden, wenn eine klassische Metrikwarnung ausgelöst wird, müssen Sie zunächst die E-Mail oder den Webhook erstellen. Erstellen Sie dann die Regel unmittelbar danach. Sie können Webhooks oder E-Mail-Adressen nicht bereits erstellten Regeln zuordnen.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Sie können überprüfen, ob Warnungen ordnungsgemäß erstellt wurden, indem Sie sich eine einzelne Regel ansehen.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Verwenden Sie zum Löschen von Regeln einen Befehl im folgenden Format:

    **insights alerts rule delete** [Optionen] &lt;resourceGroup&gt;&lt;ruleName&gt;

    Mit diesen Befehlen werden die zuvor in diesem Artikel erstellten Regeln gelöscht.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über die Azure-Überwachung](monitoring-overview.md), einschließlich der Typen von Informationen, die Sie sammeln und überwachen können.
* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](insights-webhooks-alerts.md).
* Erfahren Sie mehr über das [Konfigurieren von Warnungen zu Aktivitätsprotokollereignissen](monitoring-activity-log-alerts.md).
* Erfahren Sie mehr zu [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Verschaffen Sie sich einen [Überblick über das Sammeln von Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md), um detaillierte Hochfrequenzmetriken für Ihren Dienst zu erfassen.
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
