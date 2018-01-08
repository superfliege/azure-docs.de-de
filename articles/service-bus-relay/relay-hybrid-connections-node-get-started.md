---
title: Erste Schritte mit Azure Relay-Hybridverbindungen in Node | Microsoft-Dokumentation
description: "Schreiben einer Node.js-Konsolenanwendung für Azure Relay-Hybridverbindungen"
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 12/20/2017
ms.author: sethm
ms.openlocfilehash: d79c05253da568d3a9b2f2912d22eab2df586821
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a>Erste Schritte mit Relay-Hybridverbindungen

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Dieses Tutorial bietet eine Einführung in [Azure Relay-Hybridverbindungen](relay-what-is-it.md#hybrid-connections) und zeigt, wie Sie eine Clientanwendung, die Nachrichten an eine entsprechende Listener-Anwendung sendet, mithilfe von Node.js erstellen können. 

## <a name="what-will-be-accomplished"></a>Ziele

Da für Hybrid Connections sowohl eine Client- als auch eine Serverkomponente erforderlich ist, werden in diesem Tutorial zwei Konsolenanwendungen erstellt. Gehen Sie wie folgt vor:

1. Erstellen eines Relay-Namespace mit dem Azure-Portal
2. Erstellen einer Hybridverbindung mit dem Azure-Portal
3. Erstellen einer Serverkonsolenanwendung zum Empfangen von Nachrichten
4. Schreiben einer Clientkonsolenanwendung zum Senden von Nachrichten

## <a name="prerequisites"></a>Voraussetzungen

1. [Node.js](https://nodejs.org/en/).
2. Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Erstellen eines Namespace mithilfe des Azure-Portals

Falls Sie bereits einen Relay-Namespace erstellt haben, fahren Sie mit dem Abschnitt [Erstellen einer Hybridverbindung mit dem Azure-Portal](#2-create-a-hybrid-connection-using-the-azure-portal) fort.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Erstellen einer Hybridverbindung mit dem Azure-Portal

Wenn Sie bereits eine Hybridverbindung erstellt haben, fahren Sie mit dem Abschnitt [Erstellen einer Serveranwendung](#3-create-a-server-application-listener) fort.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Erstellen einer Serveranwendung (Listener)

Schreiben Sie eine Node.js-Konsolenanwendung, um auf Nachrichten des Relays zu lauschen und sie zu empfangen.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Erstellen einer Clientanwendung (Absender)

Schreiben Sie eine Node.js-Konsolenanwendung, um Nachrichten an das Relay zu senden.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Ausführen der Anwendungen

1. Führen Sie die Serveranwendung aus: Geben Sie in einer Node.js-Eingabeaufforderung `node listener.js` ein.
2. Führen Sie die Clientanwendung aus: Geben Sie in einer Node.js-Eingabeaufforderung `node sender.js` und einen beliebigen Text ein.
3. Stellen Sie sicher, dass von der Konsole der Serveranwendung der Text ausgegeben wird, der in die Clientanwendung eingegeben wurde.

![Ausführen von Anwendungen](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Glückwunsch! Sie haben mithilfe von Node.js eine Anwendung für End-to-End-Hybridverbindungen erstellt.

## <a name="next-steps"></a>Nächste Schritte

* [Relay – häufig gestellte Fragen](relay-faq.md)
* [Erstellen eines Namespaces](relay-create-namespace-portal.md)
* [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Erste Schritte mit Node](relay-hybrid-connections-node-get-started.md)

