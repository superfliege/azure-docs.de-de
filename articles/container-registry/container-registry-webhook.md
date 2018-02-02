---
title: Azure Container Registry-Webhooks
description: "Erfahren Sie, wie Sie Webhooks verwenden, um Ereignisse auszulösen, wenn in einem Ihrer Registrierungsrepositorys bestimmte Aktionen stattfinden."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: nepeters
ms.openlocfilehash: 915f90fd5d969d5544d56e5bec754b799f349015
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="using-azure-container-registry-webhooks"></a>Verwenden von Azure Container Registry-Webhooks

Eine Azure Container Registry speichert und verwaltet private Docker-Containerimages. Dies ähnelt der Art und Weise, in der Docker Hub öffentliche Docker-Images speichert. Sie können Webhooks verwenden, um Ereignisse auszulösen, wenn in einem Ihrer Registrierungsrepositorys bestimmte Aktionen stattfinden. Webhooks können auf Ereignisse auf Ebene der Registrierung reagieren oder auf ein bestimmtes Repositorytag begrenzt werden.

Ausführliche Informationen zu Webhookanforderungen finden Sie unter [Referenz zu Azure Container Registry-Webhookschemas](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Containerregistrierung: Erstellen Sie in Ihrem Azure-Abonnement eine Containerregistrierung. Verwenden Sie beispielsweise das [Azure-Portal](container-registry-get-started-portal.md) oder die [Azure CLI](container-registry-get-started-azure-cli.md).
* Docker CLI: Installieren Sie die [Docker-Engine](https://docs.docker.com/engine/installation/), um Ihren lokalen Computer als Docker-Host einzurichten und auf die Befehle der Docker CLI zuzugreifen.

## <a name="create-webhook-azure-portal"></a>Erstellen von Webhooks im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Navigieren Sie zu der Containerregistrierung, für die Sie einen Webhook erstellen möchten.
1. Wählen Sie unter **DIENSTE** die Option **Webhooks**.
1. Wählen Sie in der Webhook-Symbolleiste die Option **Hinzufügen**.
1. Geben Sie folgende Informationen in das Formular *Webhook erstellen* ein:

| Wert | BESCHREIBUNG |
|---|---|
| NAME | Der Name, den Sie dem Webhook geben möchten. Er darf nur Kleinbuchstaben und Zahlen enthalten und muss 5 bis 50 Zeichen lang sein. |
| Dienst-URI | Der URI, an den der Webhook POST-Benachrichtigungen senden soll. |
| Benutzerdefinierte Header | Header, die Sie zusammen mit der POST-Anforderung übergeben möchten. Diese müssen im Format „Schlüssel: Wert“ vorliegen. |
| Triggeraktionen | Aktionen, die den Webhook auslösen. Webhooks können zurzeit mit Push- und/oder Löschaktionen für ein Image ausgelöst werden. |
| Status | Der Status des Webhooks nach der Erstellung. Der Webhook ist standardmäßig aktiviert. |
| Bereich | Der Bereich, in dem der Webhook funktioniert. Standardmäßig ist der Bereich „alle Ereignisse in der Registrierung“. Der Bereich kann mit dem Format „Repository:Tag“ für ein Repository oder ein Tag angegeben werden. |

Beispielformular für Webhook:

![ACR-Webhookerstellungsoberfläche im Azure-Portal](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Erstellen von Webhooks über die Azure-Befehlszeilenschnittstelle

Um einen Webhook über die Azure-Befehlszeilenschnittstelle zu erstellen, verwenden Sie den Befehl [az acr webhook create](/cli/azure/acr/webhook#az_acr_webhook_create).

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Testen von Webhooks

### <a name="azure-portal"></a>Azure-Portal

Bevor Sie einen Webhook für Push- und Löschaktionen in einem Containerimage verwenden, können Sie ihn über die Schaltfläche **Ping** testen. Der Pingbefehl sendet eine allgemeine POST-Anforderung an den angegebenen Endpunkt und protokolliert die Antwort. Mit dieser Pingfunktion können Sie überprüfen, ob Sie den Webhook richtig konfiguriert haben.

1. Wählen Sie den Webhook aus, den Sie testen möchten.
2. Wählen Sie in der oberen Symbolleiste **Ping**.
3. Überprüfen Sie die Antwort des Endpunkts in der Spalte **HTTP-STATUS**.

![ACR-Webhookerstellungsoberfläche im Azure-Portal](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Um einen ACR-Webhook über die Azure-Befehlszeilenschnittstelle zu testen, verwenden Sie den Befehl [az acr webhook ping](/cli/azure/acr/webhook#az_acr_webhook_ping).

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Zum Anzeigen der Ergebnisse verwenden Sie den Befehl [az acr webhook list-events](/cli/azure/acr/webhook#list-events).

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Löschen von Webhooks

### <a name="azure-portal"></a>Azure-Portal

Jeder Webhook kann gelöscht werden, indem Sie den Webhook und dann die Schaltfläche **Löschen** im Azure-Portal auswählen.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Nächste Schritte

[Referenz zu Azure Container Registry-Webhookschemas](container-registry-webhook-reference.md)