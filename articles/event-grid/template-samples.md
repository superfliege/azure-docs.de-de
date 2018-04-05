---
title: Beispiele für Azure Resource Manager-Vorlagen – Event Grid | Microsoft-Dokumentation
description: Beispiele für Azure Resource Manager-Vorlagen – Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 03/27/2018
ms.author: tomfitz
ms.openlocfilehash: f4d6a663b4e0d2c2166028051e713668534b20bc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Azure Resource Manager-Vorlagen für Event Grid

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Event Grid:

| | |
|-|-|
|**Event Grid-Abonnements**||
| [Benutzerdefiniertes Thema und Abonnement mit WebHook-Endpunkt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Stellt ein benutzerdefiniertes Event Grid-Thema bereit. Erstellt ein Abonnement für dieses benutzerdefinierte Thema, das einen WebHook-Endpunkt verwendet. |
| [Benutzerdefiniertes Themenabonnement mit EventHub-Endpunkt](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeCustomTopicToEventHub.json)| Erstellt ein Event Grid-Abonnement für ein benutzerdefiniertes Thema, das bereits vorhanden ist. Das Abonnement verwendet eine Event Hub-Instanz für den Endpunkt. |
| [Ressourcengruppenabonnement](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeResourceGroupToWebHook.json)| Abonniert Ereignisse für eine Ressourcengruppe. Die Ressourcengruppe, die Sie im Rahmen der Bereitstellung als Ziel angeben, ist die Quelle der Ereignisse. Das Abonnement verwendet eine Event Hub-Instanz für den Endpunkt. |
| [Blob Storage-Konto und Abonnement](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/createBlobAndSubscribe.json)| Stellt ein Azure Blob Storage-Konto bereit und abonniert Ereignisse für dieses Speicherkonto. |
| | |
