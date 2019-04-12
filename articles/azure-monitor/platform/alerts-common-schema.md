---
title: Allgemeines Warnungsschema für Azure Monitor-Warnungen
description: Enthält grundlegende Informationen zum allgemeinen Warnungsschema, zu den Vorteilen der Nutzung und zur Vorgehensweise bei der Aktivierung.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 8f8dcff0b72ea92e835c0702113a9cb6a7678e86
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851596"
---
# <a name="common-alert-schema"></a>Allgemeines Warnungsschema

In diesem Artikel wird beschrieben, was das allgemeine Warnungsschema ist, welche Vorteile sich durch die Nutzung ergeben und wie Sie die Aktivierung durchführen.

## <a name="what-is-the-common-alert-schema"></a>Was ist das allgemeine Warnungsschema?

Mit dem allgemeinen Warnungsschema wird die Benutzeroberfläche für Warnungsbenachrichtigungen in Azure standardisiert. Bisher wurden für die Warnungstypen von Azure (Metrik, Protokoll und Aktivitätsprotokoll) eigene E-Mail-Vorlagen, Webhookschemas usw. verwendet. Mit dem allgemeinen Warnungsschema können Sie Warnungsbenachrichtigungen nun basierend auf einem einheitlichen Schema empfangen.

Mit jeder Warnungsinstanz werden **die betroffene Ressource** und **die Ursache der Warnung** beschrieben. Diese Instanzen werden im allgemeinen Schema in den folgenden Abschnitten beschrieben:
* **Zusammenfassung**: Eine Gruppe **standardisierter Felder** (für alle Warnungstypen gleich), die beschreiben, auf **welcher Ressource** sich die Warnung befindet, sowie zusätzliche allgemeine Warnungsmetadaten (z. B. Schweregrad oder Beschreibung). 
* **Warnungskontext**: Eine Gruppe von Feldern, mit denen die **Ursache der Warnung** beschrieben wird. Die Felder variieren **basierend auf dem Warnungstyp**. Eine Metrikwarnung enthält im Warnungskontext beispielsweise Felder wie den Metriknamen und -wert, während eine Aktivitätsprotokollwarnung Informationen zu dem Ereignis enthält, von dem die Warnung generiert wurde. 

Die typischen Integrationsszenarien, über die uns von Kunden berichtet wird, umfassen das Routing der Warnungsinstanz an das entsprechende Team anhand eines Verbindungselements (z. B. eine Ressourcengruppe) und die anschließende Bearbeitung durch das zuständige Team. Mit dem allgemeinen Warnungsschema können Sie über Warnungstypen hinweg eine standardisierte Routinglogik verwenden, indem Sie die wichtigen Felder nutzen und die Kontextfelder unverändert lassen, damit diese später von den jeweiligen Teams untersucht werden können.

Dies bedeutet, dass Sie unter Umständen über eine geringere Zahl von Integrationen verfügen, sodass die Verwaltung und Wartung _deutlich_ vereinfacht wird. Darüber hinaus werden zukünftige Warnungsnutzlast-Anreicherungen (z. B. Anpassung, Diagnoseanreicherung usw.) nur im allgemeinen Schema zu finden sein.

## <a name="what-enhancements-does-the-common-alert-schema-bring"></a>Welche Verbesserungen bewirkt das allgemeine Warnungsschema?

Das allgemeine Warnungsschema manifestiert sich hauptsächlich in Ihren Warnungsbenachrichtigungen. Hier sind die vorhandenen Verbesserungen aufgeführt:

| Aktion | Verbesserungen|
|:---|:---|
| sms | Eine einheitliche SMS-Vorlage für alle Warnungstypen. |
| E-Mail | Eine einheitliche und detaillierte E-Mail-Vorlage, mit der Sie Probleme leicht auf einen Blick diagnostizieren können. Mit eingebetteten Deep-Links für die Warnungsinstanz im Portal und die betroffene Ressource wird sichergestellt, dass Sie schnell mit dem Lösungsprozess beginnen können. |
| Webhook/Logik-App/Azure-Funktion | Eine einheitliche JSON-Struktur für alle Warnungstypen, um Ihnen das einfache Entwickeln von Integrationen für die unterschiedlichen Warnungstypen zu ermöglichen. |

Mit dem neuen Schema wird in naher Zukunft auch eine umfassendere Oberfläche für die Nutzung von Warnungen bereitgestellt – sowohl im Azure-Portal als auch in der mobilen Azure-App. 

[Erfahren Sie mehr zu den Schemadefinitionen für Webhooks/Logik-Apps/Azure Functions.](https://aka.ms/commonAlertSchemaDefinitions)

> [!NOTE]
> Für die folgenden Aktionen wird das allgemeine Warnungsschema nicht unterstützt: ITSM-Connector, Automation-Runbook.

## <a name="how-do-i-enable-the-common-alert-schema"></a>Wie kann ich das allgemeine Warnungsschema aktivieren?

Sie können sowohl im Portal als auch in der REST-API über Aktionsgruppen eine Entscheidung für oder gegen das allgemeine Warnungsschema treffen. Der Umschalter für den Wechsel zum neuen Schema befindet sich auf der Aktionsebene. Sie müssen die Entscheidung für eine E-Mail-Aktion und eine Webhookaktion also beispielsweise separat treffen.

> [!NOTE]
> 1. Für die folgenden Warnungstypen wird das allgemeine Schema standardmäßig unterstützt (keine Aktivierung erforderlich):
>     * Fehleranomaliewarnungen
> 1. Von den folgenden Warnungstypen wird das allgemeine Schema derzeit nicht unterstützt:
>     * Service Health-Warnungen
>     * Aktivitätsprotokoll – Sicherheitswarnungen
>     * Von [Azure Monitor für VMs](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) generierte Warnungen

### <a name="through-the-azure-portal"></a>Über das Azure-Portal

![Allgemeines Warnungsschema: Aktivierung](media/alerts-common-schema/portal-opt-in.png)

1. Öffnen Sie in einer Aktionsgruppe eine vorhandene oder eine neue Aktion. 
1. Wählen Sie für den Umschalter die Option „Ja“, um das allgemeine Warnungsschema wie gezeigt zu aktivieren.

### <a name="through-the-action-groups-rest-api"></a>Über die Aktionsgruppen-REST-API

Sie können auch die [Aktionsgruppen-API](https://docs.microsoft.com/rest/api/monitor/actiongroups) verwenden, um das allgemeine Warnungsschema zu aktivieren. Beim Durchführen des REST-API-Aufrufs zum [Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) können Sie das Flag „useCommonAlertSchema“ für die folgenden Aktionen auf „true“ (Aktivierung) oder „false“ (keine Aktivierung) festlegen: E-Mail, Webhook, Logik-App oder Azure-Funktion.

Mit dem folgenden Anforderungstext für den REST-API-Vorgang zum [Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) wird beispielsweise Folgendes durchgeführt:

* Aktivieren des allgemeinen Warnungsschemas für die E-Mail-Aktion „John Doe's email“
* Deaktivieren des allgemeinen Warnungsschemas für die E-Mail-Aktion „Jane Smith's email“
* Deaktivieren des allgemeinen Warnungsschemas für die Webhookaktion „Sample webhook“

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```





## <a name="next-steps"></a>Nächste Schritte

- [Definitionen des allgemeinen Warnungsschemas](https://aka.ms/commonAlertSchemaDefinitions)



