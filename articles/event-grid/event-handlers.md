---
title: Azure Event Grid-Ereignishandler
description: Informationen zu unterstützten Ereignishandlern für Azure Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/25/2018
ms.author: tomfitz
ms.openlocfilehash: ccbabed3fd1df39ac2ba732064392a1a0ecede2a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="event-handlers-in-azure-event-grid"></a>Ereignishandler in Azure Event Grid

Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. Sie können aber auch einen beliebigen Webhook für die Behandlung von Ereignissen verwenden. Der Webhook muss zum Behandeln von Ereignissen nicht in Azure gehostet werden.

Dieser Artikel enthält Links zu Inhalten für die einzelnen Ereignishandler.

## <a name="azure-automation"></a>Azure Automation

Verwenden Sie Azure Automation für die Verarbeitung von Ereignissen mit automatisierten Runbooks.

|Titel  |Beschreibung  |
|---------|---------|
|[Integration von Azure Automation mit Event Grid und Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Erstellen Sie einen virtuellen Computer, der ein Ereignis gesendet. Das Ereignis löst ein Automation-Runbook, das den virtuellen Computer markiert, sowie eine Nachricht aus, die an einen Microsoft Teams-Kanal gesendet wird. |

## <a name="azure-functions"></a>Azure Functions

Verwenden Sie Azure Functions für eine serverlose Reaktion auf Ereignisse.

|Titel  |Beschreibung  |
|---------|---------|
| [Event Grid-Trigger für Azure Functions](../azure-functions/functions-bindings-event-grid.md) | Übersicht über die Verwendung des Event Grid-Triggers in Functions. |
| [Automatisieren der Größenänderung von hochgeladenen Bildern mit Event Grid](resize-images-on-storage-blob-upload-event.md) | Benutzer laden Bilder über eine Web-App in ein Speicherkonto hoch. Wenn ein Speicherblob erstellt wird, sendet Event Grid ein Ereignis an die Funktions-App, die daraufhin die Größe des hochgeladenen Bilds anpasst. |
| [Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md) | Wenn Event Hubs eine Capture-Datei erstellt, sendet Event Grid ein Ereignis an eine Funktions-App. Die App ruft die Capture-Datei ab und migriert Daten zu einem Data Warehouse. |
| [Beispiele für die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid sendet Nachrichten von einem Service Bus-Thema an eine Funktions-App und an eine Logik-App. |

## <a name="logic-apps"></a>Logic Apps

Verwenden Sie Logic Apps, um Geschäftsprozesse für die Reaktion auf Ereignisse zu automatisieren.

|Titel  |Beschreibung  |
|---------|---------|
| [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Überwachen von Änderungen an virtuellen Computer mit Azure Event Grid und Logic Apps) | Eine Logik-App überwacht die Änderungen an einem virtuellen Computer und sendet E-Mails zu diesen Änderungen. |
| [Senden von E-Mail-Benachrichtigungen zu Azure IoT Hub-Ereignissen mit Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Eine Logik-App sendet eine E-Mail-Benachrichtigung, wenn Ihrer IoT Hub-Instanz ein Gerät hinzugefügt wird. |
| [Beispiele für die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid sendet Nachrichten von einem Service Bus-Thema an eine Funktions-App und an eine Logik-App. |

## <a name="queue-storage"></a>Queue Storage

Verwenden Sie Queue Storage, um Ereignisse zu empfangen, die gepullt werden müssen.

|Titel  |Beschreibung  |
|---------|---------|
| [Weiterleiten benutzerdefinierter Ereignisse an Azure Queue Storage mit Azure CLI und Event Grid](custom-event-to-queue-storage.md) | Beschreibt, wie benutzerdefinierte Ereignisse an eine Queue Storage-Instanz gesendet werden. |

## <a name="webhooks"></a>WebHooks

Verwenden Sie Webhooks für anpassbare Endpunkte, die auf Ereignisse reagieren.

|Titel  |Beschreibung  |
|---------|---------|
| [Empfangen von Ereignissen an einem HTTP-Endpunkt](receive-events.md) | Beschreibt, wie Sie einen HTTP-Endpunkt überprüfen, um Ereignisse von einem Ereignisabonnement zu empfangen, und Ereignisse empfangen und deserialisieren. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
