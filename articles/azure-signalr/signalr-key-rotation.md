---
title: Zugriffsschlüsselrotation für Azure SignalR Service
description: Enthält eine Übersicht dazu, warum Kunden routinemäßig die Zugriffsschlüssel rotieren müssen und wie dies per Azure-Portal-GUI und Azure CLI möglich ist.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 04321e62ea41b58e9ee4314b600c77e6c39b7ade
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065833"
---
# <a name="access-key-rotation-for-azure-signalr-service"></a>Zugriffsschlüsselrotation für Azure SignalR Service

Jede Azure SignalR Service-Instanz verfügt über ein Zugriffsschlüsselpaar, das aus einem Primär- und einem Sekundärschlüssel besteht. Sie werden verwendet, um SignalR-Clients zu authentifizieren, wenn Anforderungen an den Dienst gesendet werden. Die Schlüssel werden der Endpunkt-URL der Instanz zugeordnet. Bewahren Sie Ihre Schlüssel sicher auf, und rotieren Sie sie regelmäßig. Es werden zwei Zugriffsschlüssel bereitgestellt, damit Sie die Verbindungen mit einem Schlüssel aufrechterhalten können, während Sie den anderen erneut generieren.

## <a name="why-rotate-access-keys"></a>Warum müssen Zugriffsschlüssel rotiert werden?

Aus Sicherheits- und Konformitätsgründen sollten Sie die Zugriffsschlüssel regelmäßig rotieren.

## <a name="regenerate-access-keys"></a>Erneutes Generieren von Zugriffsschlüsseln

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich mit Ihren Anmeldeinformationen an.

1. Suchen Sie in der Azure SignalR Service-Instanz, die die erneut zu generierenden Schlüssel enthält, nach dem Abschnitt **Schlüssel**.

1. Wählen Sie im Navigationsmenü die Option **Schlüssel**.

1. Wählen Sie die Option **Primären Schlüssel erneut generieren** bzw. **Sekundären Schlüssel erneut generieren**.

   Ein neuer Schlüssel und die entsprechende Verbindungszeichenfolge werden erstellt und angezeigt.

   ![Erneutes Generieren von Schlüsseln](media/signalr-key-rotation/regenerate-keys.png)

Sie können auch die [Azure CLI](/cli/azure/ext/signalr/signalr/key?view=azure-cli-latest#ext-signalr-az-signalr-key-renew) verwenden, um Schlüssel erneut zu generieren.

## <a name="update-configurations-with-new-connection-strings"></a>Aktualisieren von Konfigurationen mit neuen Verbindungszeichenfolgen

1. Kopieren Sie die neu generierte Verbindungszeichenfolge.

1. Aktualisieren Sie alle Konfigurationen, damit die neue Verbindungszeichenfolge verwendet wird.

1. Starten Sie die Anwendung neu, falls dies erforderlich ist.

## <a name="forced-access-key-regeneration"></a>Erzwungene erneute Generierung des Zugriffsschlüssels

In bestimmten Situationen kann es vorkommen, dass von Azure SignalR Service eine obligatorische erneute Generierung des Zugriffsschlüssels erzwungen wird. Der Dienst benachrichtigt Kunden per E-Mail und Nachricht im Portal. Wenn Sie diese Benachrichtigungen erhalten oder für den Dienst ein Fehler aufgrund des Zugriffsschlüssels auftritt, sollten Sie die Schlüssel rotieren, indem Sie sich an die Anleitung in diesem Leitfaden halten.

## <a name="next-steps"></a>Nächste Schritte

Es ist eine bewährte Sicherheitsmethode, die Zugriffsschlüssel regelmäßig zu rotieren.

In diesem Leitfaden wurde beschrieben, wie Sie das erneute Generieren der Zugriffsschlüssel durchführen. Fahren Sie mit den nächsten Tutorials zur Authentifizierung mit OAuth oder Azure Functions fort.

> [!div class="nextstepaction"]
> [Integrieren in ASP.NET Core-Identität](./signalr-authenticate-oauth.md)

> [!div class="nextstepaction"]
> [Erstellen einer serverlosen Echtzeit-App mit Authentifizierung](./signalr-authenticate-azure-functions.md)