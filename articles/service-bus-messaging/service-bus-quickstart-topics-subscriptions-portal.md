---
title: 'Schnellstart: Erstellen von Service Bus-Themen und -Abonnements im Azure-Portal | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie im Azure-Portal ein Service Bus-Thema und Abonnements dieses Themas erstellen.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: 2af346b2c9fa5c46593aa9421c3a762bda78dc2f
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "59610152"
---
# <a name="quickstart-use-the-azure-portal-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>Schnellstart: Erstellen eines Service Bus-Themas und eines oder mehrerer Abonnements dieses Themas im Azure-Portal
In diesem Schnellstart erstellen Sie im Azure-Portal ein Service Bus-Thema und anschließend Abonnements dieses Themas. 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Was sind Service Bus-Themen und -Abonnements?
Service Bus-Themen und -Abonnements unterstützen ein Modell der Messagingkommunikation über das *Veröffentlichen/Abonnieren* . Bei der Verwendung von Themen und Abonnements kommunizieren die Komponenten einer verteilten Anwendung nicht direkt miteinander, sondern tauschen Nachrichten über ein Thema aus, das als Zwischenstufe fungiert.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Anders als bei Service Bus-Warteschlangen, bei denen jede Nachricht von einem einzelnen Consumer verarbeitet wird, bieten Themen und Abonnements eine 1:n-Kommunikationsform mit einem Veröffentlichungs- und Abonnementmuster. Es ist möglich, mehrere Abonnements zu einem Thema anzumelden. Wenn eine Nachricht an ein Thema gesendet wird, steht sie in jedem Abonnement zur Verfügung, wo sie unabhängig von den anderen Abonnements verarbeitet wird. Ein Themenabonnement ähnelt einer virtuellen Warteschlange, die Kopien der Nachrichten enthält, die an das Thema gesendet wurden. Sie können optional auch Filterregeln für einzelne Abonnements eines Themas anmelden. Auf diese Weise können Sie filtern oder einschränken, welche Nachrichten an ein Thema von welchen Themenabonnements empfangen werden.

Mit Service Bus-Themen und -Abonnements können Sie viele Nachrichten an eine große Anzahl von Benutzern und Anwendungen verarbeiten.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-topics-three-subscriptions-portal](../../includes/service-bus-create-topics-three-subscriptions-portal.md)]



## <a name="next-steps"></a>Nächste Schritte
Informationen zum Senden von Nachrichten an ein Thema und zum Empfangen dieser Nachrichten über ein Abonnement finden Sie im folgenden Artikel: Auswählen der Programmiersprache im Inhaltsverzeichnis. 

> [!div class="nextstepaction"]
> [Veröffentlichen und Abonnieren von Nachrichten](service-bus-dotnet-how-to-use-topics-subscriptions.md)
