---
title: Erste Schritte mit HTTP-Anforderungen von Azure Relay Hybrid Connections in .NET | Microsoft-Dokumentation
description: Schreiben einer C#-Konsolenanwendung für HTTP-Anforderungen von Azure Relay Hybrid Connections in .NET
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: 743e5c5a44f2ed9e6f6d2df9388ef3f01c501bff
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611096"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Erste Schritte mit HTTP-Anforderungen von Relay Hybrid Connections in .NET
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bei diesem Tutorial handelt es sich um eine Einführung in [Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections). Hier erfahren Sie, wie Sie mit Microsoft .NET eine Clientanwendung erstellen, die Anforderungen an eine entsprechende Listeneranwendung sendet. 

## <a name="what-will-be-accomplished"></a>Ziele
Für Hybrid Connections ist sowohl eine Client- als auch eine Serverkomponente erforderlich. In diesem Tutorial führen Sie die folgenden Schritte aus, um zwei Konsolenanwendungen zu erstellen:

1. Erstellen eines Relay-Namespace über das Azure-Portal
2. Erstellen einer Hybridverbindung in diesem Namespace über das Azure-Portal
3. Schreiben einer Serverkonsolenanwendung (Listener) zum Empfangen von Anforderungen
4. Schreiben einer Clientkonsolenanwendung (Absender) zum Senden von Anforderungen

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* [Visual Studio 2015 oder eine höhere Version.](http://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2017 verwendet.
* Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-by-using-the-azure-portal"></a>1. Erstellen eines Namespace über das Azure-Portal
Falls Sie bereits einen Relay-Namespace erstellt haben, fahren Sie mit [Erstellen einer Hybridverbindung über das Azure-Portal](#2-create-a-hybrid-connection-using-the-azure-portal) fort.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-by-using-the-azure-portal"></a>2. Erstellen einer Hybridverbindung über das Azure-Portal
Falls Sie bereits eine Hybridverbindung erstellt haben, fahren Sie mit [Erstellen einer Serveranwendung](#3-create-a-server-application-listener) fort.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Erstellen einer Serveranwendung (Listener)
Schreiben Sie in Visual Studio eine C#-Konsolenanwendung, um auf Nachrichten des Relays zu lauschen und sie zu empfangen.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Erstellen einer Clientanwendung (Absender)
Schreiben Sie in Visual Studio eine C#-Konsolenanwendung, um Nachrichten an das Relay zu senden.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Ausführen der Anwendungen
1. Führen Sie die Serveranwendung aus.
2. Führen Sie die Clientanwendung aus, und geben Sie Text ein.
3. Vergewissern Sie sich, dass in der Konsole der Serveranwendung der Text angezeigt wird, der in die Clientanwendung eingegeben wurde.

Glückwunsch! Sie haben eine Anwendung für End-to-End-Hybridverbindungen erstellt.

## <a name="next-steps"></a>Nächste Schritte

* [Relay – häufig gestellte Fragen](relay-faq.md)
* [Erstellen eines Namespace](relay-create-namespace-portal.md)
* [Erste Schritte mit Node](relay-hybrid-connections-node-get-started.md)

