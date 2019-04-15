---
title: Übersicht über das Azure-Aktivitätsprotokoll
description: Es wird beschrieben, was das Azure-Aktivitätsprotokoll ist und wie Sie es nutzen können, um Informationen zu den Ereignissen in Ihrem Azure-Abonnement zu erhalten.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 94465e95dbf5f2eb381c124349bf8fda6622a6c2
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650290"
---
# <a name="monitor-subscription-activity-with-the-azure-activity-log"></a>Überwachen der Abonnementaktivität per Azure-Aktivitätsprotokoll

Das **Azure-Aktivitätsprotokoll** ist ein Abonnementprotokoll, das Einblicke in Ereignisse auf Abonnementebene ermöglicht, die in Azure aufgetreten sind. Dies schließt einen Datenbereich von Azure Resource Manager-Betriebsdaten bis hin zu Aktualisierungen für Dienstintegritätsereignisse ein. Das Aktivitätsprotokoll wurde bisher als „Überwachungsprotokolle“ oder „Vorgangsprotokolle“ bezeichnet, da die Verwaltungskategorie Ereignisse der Steuerungsebene für Ihre Abonnements enthält. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Modell/RDFE-Modell verwenden.

![Aktivitätsprotokoll im Vergleich zu anderen Protokolltypen](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)

Abbildung 1: Aktivitätsprotokoll im Vergleich zu anderen Protokolltypen

Das Aktivitätsprotokoll unterscheidet sich von [Diagnoseprotokollen](diagnostic-logs-overview.md). Aktivitätsprotokolle enthalten Daten zu den Vorgängen an einer Ressource von außen („Steuerungsebene“). Diagnoseprotokolle werden von einer Ressource ausgegeben und enthalten Informationen zu den Vorgängen dieser Ressource („Datenebene“).

> [!WARNING]
> Das Azure-Aktivitätsprotokoll ist in erster Linie für Aktivitäten bestimmt, die in Azure Resource Manager stattfinden. Es verfolgt keine Ressourcen nach, die das klassische Modell/RDFE-Modell verwenden. Einige klassische Ressourcentypen weisen einen Proxyressourcenanbieter in Azure Resource Manager auf (z.B. Microsoft.ClassicCompute). Wenn Sie mithilfe dieser Proxyressourcenanbieter über Azure Resource Manager mit einem klassischen Ressourcentyp interagieren, werden die Vorgänge im Aktivitätsprotokoll aufgeführt. Wenn Sie mit einem klassischen Ressourcentyp außerhalb der Azure Resource Manager-Proxys interagieren, werden Ihre Aktionen nur in das Vorgangsprotokoll aufgenommen. Das Vorgangsprotokoll kann in einem separaten Abschnitt des Portals durchsucht werden.
>
>

Sie können Ereignisse per Azure-Portal, Befehlszeilenschnittstelle, PowerShell-Cmdlets und Azure Monitor-REST-API aus dem Aktivitätsprotokoll abrufen.

> [!NOTE]
> Die [neueren Warnungen](../../azure-monitor/platform/alerts-overview.md) bieten eine erweiterte Benutzeroberfläche für die Erstellung und Verwaltung von Warnungsregeln für Aktivitätsprotokolle.  [Weitere Informationen](../../azure-monitor/platform/alerts-activity-log.md)

## <a name="categories-in-the-activity-log"></a>Kategorien im Aktivitätsprotokoll
Das Aktivitätsprotokoll enthält verschiedene Kategorien von Daten. Umfassende Informationen zu den Schemas dieser Kategorien finden Sie in [diesem Artikel](../../azure-monitor/platform/activity-log-schema.md). Das umfasst:
* **Administration**: Diese Kategorie enthält die Datensätze aller Erstellungs-, Aktualisierungs-, Lösch- und Aktionsvorgänge, die über Resource Manager ausgeführt wurden. Zu den Ereignissen in dieser Kategorie gehören das Erstellen eines virtuellen Computers und das Löschen einer Netzwerksicherheitsgruppe. Jede Aktion, die von einem Benutzer oder einer Anwendung mithilfe von Resource Manager ausgeführt wird, wird als Vorgang für einen bestimmten Ressourcentyp modelliert. Wenn der Vorgangstyp „Schreiben“, „Löschen“ oder „Aktion“ ist, werden die Datensätze zum Start und zum Erfolg oder Fehler dieses Vorgangs in der Kategorie „Administration“ aufgezeichnet. Die Kategorie „Administration“ enthält außerdem alle Änderungen an der rollenbasierten Zugriffssteuerung in einem Abonnement.
* **Dienstintegrität**: Diese Kategorie enthält Datensätze zu allen Incidents im Zusammenhang mit der Dienstintegrität, die in Azure aufgetreten sind. Ein Beispiel für ein Ereignis in dieser Kategorie ist „Ausfallzeiten bei SQL Azure in der Region ‚USA, Osten‘“. Es gibt fünf Typen von Ereignissen zur Dienstintegrität: Aktion erforderlich, unterstützte Wiederherstellung, Incident, Wartung, Information oder Sicherheit. Sie werden nur angezeigt, wenn eine Ressource in Ihrem Abonnement von dem Ereignis betroffen wäre.
* **Ressourcenintegrität**: Diese Kategorie enthält Datensätze zu allen Ereignissen im Zusammenhang mit der Ressourcenintegrität, die für Ihre Azure-Ressourcen aufgetreten sind. Ein Beispiel für die Art der Ereignisse, die in dieser Kategorie angezeigt werden, ist „Integritätsstatus des virtuellen Computers ist zu Nicht verfügbar gewechselt“. Ereignisse zur Ressourcenintegrität können über einen von vier Integritätsstatus verfügen: „Available“, „Unavailable“, „Degraded“ und „Unknown“. Darüber hinaus können Ereignisse zur Ressourcenintegrität kategorisiert werden. Dabei sind die Kategorien „Von der Plattform initiiert“ oder „Vom Benutzer initiiert“ verfügbar.
* **Warnung**: Diese Kategorie enthält die Datensätze zu allen Aktivierungen von Azure-Warnungen. Ein Beispiel für ein Ereignis in dieser Kategorie ist „CPU-Auslastung auf ‚myVM‘ liegt in den letzten 5 Minuten über 80“. Eine Vielzahl von Azure-Systemen weist ein Konzept für Warnungen auf: Sie können eine Regel definieren und erhalten eine Benachrichtigung, wenn die Bedingungen mit der Regel übereinstimmen. Jedes Mal, wenn ein unterstützter Azure-Warnungstyp „aktiviert“ wird oder die Bedingungen erfüllt sind, sodass eine Benachrichtigung generiert wird, wird ein Datensatz der Aktivierung auch in dieser Kategorie des Aktivitätsprotokolls abgelegt.
* **Autoskalierung**: Diese Kategorie enthält Datensätze zu Ereignissen im Zusammenhang mit der Engine für die automatische Skalierung – basierend auf den Einstellungen für die automatische Skalierung, die Sie in Ihrem Abonnement definiert haben. Ein Beispiel für Ereignisse in dieser Kategorie ist „Fehler beim automatischen zentralen Hochskalieren“. Mit der automatischen Skalierung können Sie die Anzahl der Instanzen eines unterstützten Ressourcentyps basierend auf der Tageszeit und/oder Lastdaten (Metrik) mithilfe einer Einstellung für die automatische Skalierung automatisch horizontal hoch- oder herunterskalieren. Wenn die Bedingungen zum zentralen Hoch- oder Herunterskalieren erfüllt sind, werden Ereignisse zum Start und zum Erfolg bzw. Fehler in dieser Kategorie aufgezeichnet.
* **Empfehlung**: Diese Kategorie enthält Empfehlungsereignisse aus Azure Advisor.
* **Sicherheit**: Diese Kategorie enthält Datensätze von Warnungen, die vom Azure Security Center generiert wurden. Ein Beispiel für den Typ der Ereignisse, die in dieser Kategorie angezeigt werden, ist „Verdächtige Datei mit doppelter Erweiterung ausgeführt“.
* **Richtlinie**: Diese Kategorie enthält Datensätze aller Aktionsvorgänge für Auswirkungen, die von Azure Policy ausgeführt werden. Beispiele für Ereignistypen, die in dieser Kategorie angezeigt werden, sind „Audit“ und „Deny“. Jede Aktion, die von Policy ausgeführt wird, ist als ein Vorgang für eine Ressource modelliert.

## <a name="event-schema-per-category"></a>Ereignisschema nach Kategorie

[Lesen Sie diesen Artikel, um sich über die Grundlagen des Aktivitätsprotokoll-Ereignisschemas nach Kategorie zu informieren.](../../azure-monitor/platform/activity-log-schema.md)

## <a name="what-you-can-do-with-the-activity-log"></a>Möglichkeiten mit dem Aktivitätsprotokoll

Hier sind einige Verwendungsmöglichkeiten für das Aktivitätsprotokoll aufgeführt:

![Azure-Aktivitätsprotokoll](./media/activity-logs-overview/Activity_Log_Overview_v3.png)


* Abfragen und Anzeigen des Protokolls im **Azure-Portal**
* [Erstellen einer Warnung zu einem Aktivitätsprotokollereignis](../../azure-monitor/platform/activity-log-alerts.md)
* [Streamen an einen **Event Hub**](../../azure-monitor/platform/activity-logs-stream-event-hubs.md) zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI
* Analysieren in Power BI mit dem [**Power BI-Inhaltspaket**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)
* [Speichern unter einem **Speicherkonto** zur Archivierung oder manuellen Untersuchung](../../azure-monitor/platform/archive-activity-log.md) Sie können die Aufbewahrungsdauer (in Tagen) mithilfe des **Protokollprofils** angeben.
* Fragen Sie es per PowerShell-Cmdlet, CLI oder REST-API ab.

## <a name="query-the-activity-log-in-the-azure-portal"></a>Abfragen des Aktivitätsprotokolls im Azure-Portal

Im Azure-Portal können Sie Ihr Aktivitätsprotokoll an mehreren Stellen anzeigen:
* Das **Aktivitätsprotokoll**, auf das Sie zugreifen können, indem Sie im Navigationsbereich auf der linken Seite unter **Alle Dienste** nach dem Aktivitätsprotokoll suchen.
* **Monitor** wird standardmäßig im linken Navigationsbereich angezeigt. Das Aktivitätsprotokoll ist ein Abschnitt von Azure Monitor.
* Bei den meisten **Ressourcen**, z.B. auf dem Konfigurationsblatt für einen virtuellen Computer. Das Aktivitätsprotokoll ist ein Abschnitt auf den meisten Ressourcenblättern. Wenn Sie darauf klicken, werden die Ereignisse automatisch so gefiltert, dass nur die Ereignisse für die jeweilige Ressource angezeigt werden.

Im Azure-Portal können Sie Ihr Aktivitätsprotokoll nach diesen Feldern filtern:
* Zeitraum: Die Start- und Endzeit für die Ereignisse.
* Kategorie: Die oben beschriebene Ereigniskategorie.
* Abonnement: Name von mindestens einem Azure-Abonnement.
* Ressourcengruppe: Mindestens eine Ressourcengruppe in diesen Abonnements.
* Ressource (Name): Der Name einer bestimmten Ressource.
* Ressourcentyp: Der Typ der Ressource, z.B. „Microsoft.Compute/virtualmachines“.
* Vorgangsname: Der Name eines Azure Resource Manager-Vorgangs, z.B. „Microsoft.SQL/servers/Write“.
* Schweregrad: Der Schweregrad des Ereignisses (Information, Warnung, Fehler, Kritisch).
* Ereignis initiiert von: Der „Aufrufer“ oder Benutzer, der den Vorgang durchgeführt hat.
* Open search (Freie Suche): Dies ist ein Feld für eine offene Textsuche, bei der in allen Feldern aller Ereignisse nach der Zeichenfolge gesucht wird.

Nachdem Sie eine Gruppe von Filtern definiert haben, können Sie eine Abfrage in Ihrem Azure-Dashboard anheften, um immer den Überblick über bestimmte Ereignisse zu haben.

Sie haben noch mehr Optionen, wenn Sie auf das Symbol **Protokolle** klicken, über das Ihre Aktivitätsprotokolldaten in der [Lösung zum Erfassen und Analysieren von Aktivitätsprotokollen](../../azure-monitor/platform/collect-activity-logs.md) angezeigt werden. Das Blatt „Aktivitätsprotokoll“ enthält eine einfache Filter-/Suchoberfläche für Protokolle. Mit der Funktion für Azure Monitor-Protokolle stehen Ihnen jedoch effektivere Möglichkeiten, Ihre Daten zu pivotieren, abzufragen und zu visualisieren zur Verfügung.

## <a name="export-the-activity-log-with-a-log-profile"></a>Exportieren des Aktivitätsprotokolls mit einem Protokollprofil

Mit einem **Protokollprofil** wird gesteuert, wie das Aktivitätsprotokoll exportiert wird. Mit einem Protokollprofil können Sie Folgendes konfigurieren:

* Wohin das Aktivitätsprotokoll gesendet wird (Speicherkonto oder Event Hubs)
* Welche Ereigniskategorien gesendet werden sollen („Write“, „Delete“, „Action“) *„Kategorie“ hat in Protokollprofilen und Aktivitätsprotokollen eine unterschiedliche Bedeutung. Im Protokollprofil ist „Kategorie“ der Vorgangstyp („Write“, „Delete“, „Action“). In einem Aktivitätsprotokoll stellt die Eigenschaft „Kategorie“ die Quelle oder den Typ des Ereignisses dar (z.B. „Administration“, „ServiceHealth“, „Alert“ usw.).*
* Welche Regionen (Standorte) exportiert werden sollen. Stellen Sie sicher, dass Sie „global“ einbeziehen, da viele Ereignisse im Aktivitätsprotokoll globale Ereignisse sind.
* Wie lange das Aktivitätsprotokoll in einem Speicherkonto beibehalten werden soll.
    - Wenn für die Beibehaltungsdauer 0 Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
    - Wenn Aufbewahrungsrichtlinien festgelegt wurden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Optionen „Event Hubs“ oder „Log Analytics“ ausgewählt sind), werden die Aufbewahrungsrichtlinien ignoriert.
    - Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht. Der Löschvorgang beginnt um Mitternacht (UTC), jedoch kann es bis zu 24 Stunden dauern, bis die Protokolle aus Ihrem Speicherkonto gelöscht werden.

Sie können ein Speicherkonto oder Event Hub-Namespace verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle angibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden RBAC-Zugriff auf beide Abonnements.

> [!NOTE]
>  Sie können derzeit keine Daten in einem Speicherkonto archivieren, das sich hinter einem geschützten virtuellen Netzwerk befindet.

> [!WARNING]
> Das Format der Protokolldaten im Speicherkonto wurde am 1. November 2018 in JSON Lines geändert. [Dieser Artikel enthält eine Beschreibung der Auswirkungen und der Aktualisierung Ihrer Tools zur Verarbeitung des neuen Formats.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md)
>
>

Diese Einstellungen können über die Option „Exportieren“ auf dem Blatt „Aktivitätsprotokoll“ im Portal konfiguriert werden. Sie können auch [mithilfe der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931927.aspx), über PowerShell-Cmdlets oder über die Befehlszeilenschnittstelle programmgesteuert konfiguriert werden. Ein Abonnement kann nur über ein Protokollprofil verfügen.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Konfigurieren von Protokollprofilen mit dem Azure-Portal

Sie können das Aktivitätsprotokoll an einen Event Hub streamen oder in einem Speicherkonto speichern, indem Sie im Azure-Portal die Option „In Event Hub exportieren“ verwenden.

1. Navigieren Sie zu **Aktivitätsprotokoll**, indem Sie das Menü auf der linken Seite des Portals verwenden.

    ![Navigation zum Aktivitätsprotokoll im Portal](./media/activity-logs-overview/activity-logs-portal-navigate-v2.png)
2. Klicken Sie oben auf dem Blatt auf die Schaltfläche **In Event Hub exportieren**.

    ![Schaltfläche „Exportieren“ im Portal](./media/activity-logs-overview/activity-logs-portal-export-v2.png)
3. Auf dem daraufhin angezeigten Blatt können Sie Folgendes auswählen:
   * Regionen, die für die Ereignisse exportiert werden sollen
   * das Speicherkonto, in dem Sie Ereignisse speichern möchten
   * die Anzahl der Tage, die diese Ereignisse im Speicher aufbewahrt werden sollen. Bei einer Einstellung von 0 Tagen werden die Protokolle unbegrenzt aufbewahrt.
   * den Service Bus-Namespace, in dem Sie einen Event Hub für das Streamen dieser Ereignisse erstellen möchten

     ![Blatt zum Exportieren des Aktivitätsprotokolls](./media/activity-logs-overview/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern. Die Einstellungen werden sofort auf Ihr Abonnement angewendet.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Konfigurieren von Protokollprofilen mit den Azure PowerShell-Cmdlets

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

#### <a name="get-existing-log-profile"></a>Abrufen eines vorhandenen Protokollprofils

```powershell
Get-AzLogProfile
```

#### <a name="add-a-log-profile"></a>Hinzufügen eines Protokollprofils

```powershell
Add-AzLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
```

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| --- | --- | --- |
| Name |Ja |Name des Protokollprofils. |
| StorageAccountId |Nein  |Ressourcen-ID des Speicherkontos, in dem das Aktivitätsprotokoll gespeichert werden soll. |
| serviceBusRuleId |Nein  |Service Bus-Regel-ID für den Service Bus-Namespace, unter dem Event Hubs erstellt werden sollen. Dies ist eine Zeichenfolge mit dem folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| Standort |Ja |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. |
| RetentionInDays |Ja |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| Category (Kategorie) |Nein  |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

#### <a name="remove-a-log-profile"></a>Entfernen eines Protokollprofils

```powershell
Remove-AzLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cli"></a>Konfigurieren von Protokollprofilen mit der Azure CLI

#### <a name="get-existing-log-profile"></a>Abrufen eines vorhandenen Protokollprofils

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

Die `name` -Eigenschaft sollte den Namen des Protokollprofils enthalten.

#### <a name="add-a-log-profile"></a>Hinzufügen eines Protokollprofils

```azurecli
az monitor log-profiles create --name <profile name> \
    --locations <location1 location2 ...> \
    --location <location> \
    --categories <category1 category2 ...>
```

Die vollständige Dokumentation für die Erstellung eines Überwachungsprofils mit der CLI finden Sie in der [Referenz zu Befehlen der Befehlszeilenschnittstelle](/cli/azure/monitor/log-profiles#az-monitor-log-profiles-create).

#### <a name="remove-a-log-profile"></a>Entfernen eines Protokollprofils

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Aktivitätsprotokoll (bisher „Überwachungsprotokolle“)](../../azure-resource-manager/resource-group-audit.md)
* [Stream the Azure Activity Log to Event Hubs (Streamen des Azure-Aktivitätsprotokolls auf Event Hubs)](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)
