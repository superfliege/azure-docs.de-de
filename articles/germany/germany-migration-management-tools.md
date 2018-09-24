---
title: Migration von Verwaltungstoolressourcen von Azure Deutschland zu Azure weltweit
description: Dieser Artikel bietet Unterstützung bei der Migration von Verwaltungstoolressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 8/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: a8d6898745abff6a90827758e3b3fed55c84c8b0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963484"
---
# <a name="migration-of-management-tools-resources-from-azure-germany-to-global-azure"></a>Migration von Verwaltungstoolressourcen von Azure Deutschland zu Azure weltweit

Dieser Artikel unterstützt Sie bei der Migration von Verwaltungstoolressourcen von Azure Deutschland zu Azure weltweit.

## <a name="traffic-manager"></a>Traffic Manager

In Azure Deutschland erstellte Traffic Manager-Profile können nicht zu Azure weltweit migriert werden. Da Sie auch alle Traffic Manager-Endpunkte zur Zielumgebung migrieren, müssen Sie das Traffic Manager-Profil ohnehin aktualisieren.

Traffic Manager kann Sie bei einer reibungslosen Migration unterstützen. Wenn Traffic Manager noch in der alten Umgebung ausgeführt wird, können Sie bereits zusätzliche Endpunkte in der Zielumgebung definieren. Sobald Traffic Manager in der neuen Umgebung ausgeführt wird, können Sie weiterhin Endpunkte in der alten Umgebung definieren, die Sie bisher noch nicht migriert haben. Dies wird als [das blau-grüne Szenario](https://azure.microsoft.com/blog/blue-green-deployments-using-azure-traffic-manager/) bezeichnet. Kurz gesagt:

- Erstellen Sie einen neuen Traffic Manager in Azure weltweit.
- Definieren Sie die Endpunkte (immer noch in Azure Deutschland).
- Ändern Sie Ihren DNS CNAME in den neuen Traffic Manager.
- Deaktivieren Sie den alten Traffic Manager
- für jeden Endpunkt in Azure Deutschland:
  - Migrieren Sie den Endpunkt zu Azure weltweit.
  - Ändern Sie das Traffic Manager-Profil, um den neuen Endpunkt zu verwenden.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Traffic Manager anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/traffic-manager/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Traffic Manager – Übersicht](../traffic-manager/traffic-manager-overview.md)
- [Erstellen eines Traffic Manager-Profils](../traffic-manager/traffic-manager-create-profile.md)
- [Blau-grünes Szenario](https://azure.microsoft.com/blog/blue-green-deployments-using-azure-traffic-manager/)








## <a name="backup"></a>Backup

Leider können Azure Backup-Aufträge und Momentaufnahmen nicht von Azure Deutschland zu Azure weltweit migriert werden.

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Azure Backup anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/backup/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Azure Backup: Übersicht](../backup/backup-introduction-to-azure-backup.md)










## <a name="scheduler"></a>Scheduler

Azure Scheduler ist veraltet. Verwenden Sie stattdessen Azure Logic Apps zum Erstellen von Planungsaufträgen.

### <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit den Features vertraut, die Azure Logic Apps bietet, indem Sie die [schrittweisen Tutorials](https://docs.microsoft.com/azure/logic-apps/#step-by-step-tutorials) nutzen.

### <a name="reference"></a>Verweis

- [Azure Logic Apps: Übersicht](../logic-apps/logic-apps-overview.md)











## <a name="network-watcher"></a>Network Watcher

Die Migration von Network Watcher zwischen Azure Deutschland und Azure weltweit wird zurzeit nicht unterstützt. Es wird empfohlen, einen neuen Network Watcher zu erstellen und zu konfigurieren. Vergleichen Sie anschließend die Ergebnisse zwischen der alten und der neuen Umgebung:

- [NSG-Datenflussprotokolle](../network-watcher/network-watcher-nsg-flow-logging-portal.md)
- [Verbindungsmonitor](../network-watcher/connection-monitor.md)

### <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Network Watcher anhand dieser [schrittweisen Tutorials](https://docs.microsoft.com/azure/network-watcher/#step-by-step-tutorials) auf.

### <a name="references"></a>Referenzen

- [Network Watcher – Übersicht](../network-watcher/network-watcher-monitoring-overview.md)













## <a name="site-recovery"></a>Site Recovery

Es ist nicht möglich, Ihr aktuelles Site Recovery-Setup zu Azure weltweit zu migrieren. Richten Sie Ihre Lösung erneut in Azure weltweit ein.

### <a name="next-steps"></a>Nächste Schritte

Frischen Sie Ihre Kenntnisse anhand dieser schrittweisen Tutorials zum Einrichten von Notfallwiederherstellung auf:
- [Azure zu Azure](https://docs.microsoft.com/azure/site-recovery/#azure-to-azure)
- [VMware zu Azure](https://docs.microsoft.com/azure/site-recovery/#vmware)
- [Hyper-V in Azure](https://docs.microsoft.com/azure/site-recovery/#hyper-v)

### <a name="references"></a>Referenzen

Lesen Sie auch [Verwenden von Site Recovery](./germany-migration-compute.md#compute-iaas) zum Migrieren von Azure Deutschland zu Azure weltweit.













## <a name="azure-policy"></a>Azure Policy

Es gibt keine direkte Möglichkeit, Richtlinien zu migrieren. Der Bereich der zugewiesenen Richtlinien ändert sich in den meisten Fällen, insbesondere da sich die Abonnement-ID ändert. Aber die folgende Möglichkeit besteht, Richtliniendefinitionen beizubehalten und sie in Azure weltweit wiederzuverwenden.

Mithilfe der Azure CLI können Sie mit diesem Befehl alle Richtlinien in Ihrer aktuellen Umgebung auflisten.
> [!NOTE]
> Vergessen Sie nicht, zunächst in die AzureGermanCloud-Umgebung in der CLI zu wechseln.

```azurecli
az policy definition list --query '[].{Type:policyType,Name:name}' --output table
```

Exportieren Sie nur Richtlinien mit dem PolicyType *Benutzerdefiniert*. Exportieren Sie die *policyRule* in eine Datei. Das folgende Beispiel exportiert eine benutzerdefinierte Richtlinie „Allow Germany Central Only“ (nur „Deutschland, Mitte“ zulassen, kurz *allowgconly*) in eine Datei im aktuellen Ordner. 

```azurecli
az policy definition show --name allowgconly --output json --query policyRule > policy.json
```

Die Exportdatei sollte in etwa wie im folgenden Beispiel aussehen:

```json
{
  "if": {
    "not": {
      "equals": "germanycentral",
      "field": "location"
    }
  },
  "then": {
    "effect": "Deny"
  }
}
```

Wechseln Sie nun in die Azure weltweit-Umgebung, und ändern Sie die Richtlinienregel durch Bearbeiten der Datei. Ändern Sie z.B. *germanycentral* in *westeurope*:

```json
{
  "if": {
    "not": {
      "equals": "westeurope",
      "field": "location"
    }
  },
  "then": {
    "effect": "Deny"
  }
}
```

Erstellen Sie die neue Richtlinie:

```azurecli
cat policy.json |az policy definition create --name "allowweonly" --rules @-
```

Sie verfügen nun über eine neue Richtlinie namens „allowweonly“, die nur „Europa, Westen“ als Standort zulässt.

Weisen Sie die Richtlinie den Bereichen in Ihrer neuen Umgebung entsprechend zu. Sie können die alten Zuordnungen in Azure Deutschland dokumentieren, indem Sie den folgenden Befehl ausführen:

```azurecli
az policy assignment list
```

## <a name="next-steps"></a>Nächste Schritte

- Frischen Sie Ihre Kenntnisse zu Azure Policy anhand [dieses schrittweisen Tutorials](../governance/policy/tutorials/create-and-manage.md) auf.

## <a name="references"></a>Referenzen

- [Anzeigen von Richtlinien mit der CLI](../governance/policy/tutorials/create-and-manage.md#view-policy-definitions-with-azure-cli)
- [Erstellen der Richtliniendefinition mit der CLI](../governance/policy/tutorials/create-and-manage.md#create-a-policy-definition-with-azure-cli)
- [Anzeigen von Richtlinien mit PowerShell](../governance/policy/tutorials/create-and-manage.md#view-policy-definitions-with-powershell)
- [Erstellen der Richtliniendefinition mit PowerShell](../governance/policy/tutorials/create-and-manage.md#create-a-policy-definition-with-powershell)
