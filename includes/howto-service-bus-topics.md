---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: 7b05f3d8bcca5f26161f4c362078fa134518cafd
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2019
ms.locfileid: "56588652"
---
## <a name="what-are-service-bus-topics-and-subscriptions"></a>Was sind Service Bus-Themen und -Abonnements?
Service Bus-Themen und -Abonnements unterstützen ein Modell der Messagingkommunikation über das *Veröffentlichen/Abonnieren* . Bei der Verwendung von Themen und Abonnements kommunizieren die Komponenten einer verteilten Anwendung nicht direkt miteinander, sondern tauschen Nachrichten über ein Thema aus, das als Zwischenstufe fungiert.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Anders als bei Service Bus-Warteschlangen, wo jede Nachricht von einem einzelnen Consumer verarbeitet wird, bieten Themen und Abonnements eine „1:n“-Kommunikationsform mit einem Veröffentlichungs- und Abonnementsmuster. Es ist möglich, mehrere Abonnements zu einem Thema anzumelden. Wenn eine Nachricht an ein Thema gesendet wird, steht sie in jedem Abonnement zur Verfügung, wo sie unabhängig von den anderen Abonnements verarbeitet wird.

Ein Themenabonnement ähnelt einer virtuellen Warteschlange, die Kopien der Nachrichten enthält, die an das Thema gesendet wurden. Optional können Sie Filterregeln für ein Thema auf Abonnementbasis registrieren. Filterregeln ermöglichen das Filtern oder Einschränken der empfangenen Meldungen zu einem Thema durch Themenabonnements.

Mit Service Bus-Themen und -Abonnements können Sie sehr viele Nachrichten an viele Benutzer und Anwendungen verarbeiten.

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


