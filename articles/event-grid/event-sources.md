---
title: Azure Event Grid-Ereignisquellen
description: Dieser Artikel beschreibt unterstützte Ereignisquellen für Azure Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: tomfitz
ms.openlocfilehash: f9c3bcb6b92b43fe5b5bad72c99e6ce199c17448
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
---
# <a name="event-sources-in-azure-event-grid"></a>Ereignisquellen in Azure Event Grid

Eine Ereignisquelle ist die Quelle, in der das Ereignis auftritt. Mehrere Azure-Dienste werden automatisch für das Senden von Ereignissen konfiguriert. Sie können auch benutzerdefinierte Anwendungen erstellen, die Ereignisse senden. Benutzerdefinierte Anwendungen müssen nicht in Azure gehostet werden, damit Event Grid für die Ereignisverteilung verwendet werden kann.

Dieser Artikel enthält Links zu Inhalten für jede Ereignisquelle.

## <a name="azure-subscriptions"></a>Azure-Abonnements

Abonnieren Sie Azure-Abonnementereignisse, um auf Änderungen an Ressourcen in einem ganzen Azure-Abonnement zu reagieren.

|Titel |BESCHREIBUNG  |
|---------|---------|
| [Integration von Azure Automation mit Event Grid und Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Erstellen Sie einen virtuellen Computer, der ein Ereignis gesendet. Das Ereignis löst ein Automation-Runbook, das den virtuellen Computer markiert, sowie eine Nachricht aus, die an einen Microsoft Teams-Kanal gesendet wird. |
| [Ereignisschema](event-schema-subscriptions.md) | Zeigt Felder in Azure-Abonnementereignissen an. |

## <a name="custom-topics"></a>Benutzerdefinierte Themen

Abonnieren Sie benutzerdefinierte Themen, um auf Anwendungsereignisse zu reagieren.

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure CLI](custom-event-quickstart.md) | Zeigt die Verwendung von Azure CLI zum Senden von benutzerdefinierten Ereignissen. |
| [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure PowerShell](custom-event-quickstart-powershell.md) | Zeigt die Verwendung von Azure PowerShell zum Senden von benutzerdefinierten Ereignissen. |
| [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit dem Azure-Portal](custom-event-quickstart-portal.md) | Zeigt die Verwendung des Portals zum Senden von benutzerdefinierten Ereignissen. |
| [Posten eines Ereignisses in einem benutzerdefinierten Thema](post-to-custom-topic.md) | Zeigt das Posten eines Ereignisses in einem benutzerdefinierten Thema. |
| [Weiterleiten von benutzerdefinierten Ereignissen an Azure Queue Storage](custom-event-to-queue-storage.md) | Beschreibt das Senden von benutzerdefinierten Ereignisse an eine Queue Storage-Instanz. |
| [Ereignisschema](event-schema.md) | Zeigt Felder in benutzerdefinierten Ereignissen an. |

## <a name="event-hubs"></a>Event Hubs

Abonnieren Sie Event Hubs, um auf Capture-Dateiereignisse zu reagieren.

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md) | Wenn Event Hubs eine Capture-Datei erstellt, sendet Event Grid ein Ereignis an eine Funktions-App. Die App ruft die Capture-Datei ab und migriert Daten zu einem Data Warehouse. |
| [Ereignisschema](event-schema-event-hubs.md) | Zeigt Felder in Event Hubs-Ereignissen an. |

## <a name="iot-hub"></a>IoT Hub

Abonnieren Sie IoT Hub-Ereignisse, um auf Ereignisse der Geräteerstellung und -löschung zu reagieren.

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Senden von E-Mail-Benachrichtigungen zu Azure IoT Hub-Ereignissen mit Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Eine Logik-App sendet jedes Mal eine E-Mail-Benachrichtigung, wenn Ihrer IoT Hub-Instanz ein Gerät hinzugefügt wird. |
| [Reagieren auf IoT Hub-Ereignisse mithilfe von Event Grid zum Auslösen von Aktionen](../iot-hub/iot-hub-event-grid.md) | Übersicht über die Integration von IoT Hub-Instanzen in Event Grid. |
| [Ereignisschema](event-schema-iot-hub.md) | Zeigt Felder in IoT Hub-Ereignissen an. |

## <a name="media-services"></a>Media Services

Abonnieren Sie Media Services-Ereignisse, um auf Auftragsstatusereignisse zu reagieren.

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Reagieren auf Media Services-Ereignisse](../media-services/latest/reacting-to-media-services-events.md) | Übersicht über die Integration von Media Services in Event Grid. |
| [Weiterleiten von Azure Media Services-Ereignissen an einen benutzerdefinierten Webendpunkt mithilfe der CLI](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Zeigt, wie Sie Ereignisse von Media Services senden. |
| [Ereignisschema](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Zeigt Felder in Media Services-Ereignissen an. |

## <a name="resource-groups"></a>Ressourcengruppen

Abonnieren Sie Ressourcengruppenereignisse, um auf Änderungen in Ressourcen in einer ganzen Ressourcengruppe zu reagieren.

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Überwachen von Änderungen an virtuellen Computer mit Azure Event Grid und Logic Apps) | Eine Logik-App überwacht die Änderungen an einem virtuellen Computer und sendet E-Mails zu diesen Änderungen. |
| [Ereignisschema](event-schema-resource-groups.md) | Zeigt Felder in Ressourcengruppenereignissen an. |

## <a name="service-bus"></a>Service Bus

Abonnieren Sie Service Bus-Ereignisse, um ohne aktiven Listener auf Nachrichten zu reagieren.

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Beispiele für die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid sendet Nachrichten von einem Service Bus-Thema an eine Funktions-App und an eine Logik-App. |
| [Übersicht über die Integration von Azure Service Bus in Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Übersicht über die Integration von Service Bus in Event Grid. |
| [Ereignisschema](event-schema-service-bus.md) | Zeigt Felder in Service Bus-Ereignissen an. |

## <a name="storage"></a>Speicher

Abonnieren Sie Blob Storage-Ereignisse, um auf Ereignisse der Bloberstellung und -löschung zu reagieren.

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Webendpunkt mit Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Zeigt die Verwendung von Azure CLI zum Senden von Blob Storage-Ereignissen. |
| [Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Webendpunkt mit PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Zeigt die Verwendung von Azure PowerShell zum Senden von Blob Storage-Ereignissen. |
| [Reacting to Blob storage events (preview)](../storage/blobs/storage-blob-event-overview.md) (Reagieren auf Blob Storage-Ereignisse (Vorschauversion)) | Übersicht über die Integration von Blob Storage in Event Grid. |
| [Ereignisschema](event-schema-blob-storage.md) | Zeigt Felder in Blob Storage-Ereignissen an. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
