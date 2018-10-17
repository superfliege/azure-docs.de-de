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
ms.date: 09/18/2018
ms.author: tomfitz
ms.openlocfilehash: c462334597b41b914b6a0a0e3c8a67ad97e2bd7a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295579"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Azure Resource Manager-Vorlagen für Event Grid

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Event Grid:

| | |
|-|-|
|**Event Grid-Abonnements**||
| [Benutzerdefiniertes Thema und Abonnement mit WebHook-Endpunkt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Stellt ein benutzerdefiniertes Event Grid-Thema bereit. Erstellt ein Abonnement für dieses benutzerdefinierte Thema, das einen WebHook-Endpunkt verwendet. |
| [Benutzerdefiniertes Themenabonnement mit EventHub-Endpunkt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Erstellt ein Event Grid-Abonnement für ein benutzerdefiniertes Thema. Das Abonnement verwendet eine Event Hub-Instanz für den Endpunkt. |
| [Azure- oder Ressourcengruppenabonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| Abonniert Ereignisse für eine Ressourcengruppe oder ein Azure-Abonnement. Die Ressourcengruppe, die Sie im Rahmen der Bereitstellung als Ziel angeben, ist die Quelle der Ereignisse. Das Abonnement verwendet einen Webhook für den Endpunkt. |
| [Blob Storage-Konto und Abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| Stellt ein Azure Blob Storage-Konto bereit und abonniert Ereignisse für dieses Speicherkonto. |
| | |
