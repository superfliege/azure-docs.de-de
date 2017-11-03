---
title: Azure Container Registry-Webhooks
description: "Erfahren Sie, wie Sie Webhooks verwenden, um Ereignisse auszulösen, wenn in einem Ihrer Registrierungsrepositorys bestimmte Aktionen stattfinden."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: Docker, Container, ACR
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.openlocfilehash: 5a9dab91aafb92f944b473f05144242143e36477
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2017
---
# <a name="using-azure-container-registry-webhooks"></a>Verwenden von Azure Container Registry-Webhooks

Eine Azure Container Registry speichert und verwaltet private Docker-Containerimages. Dies ähnelt der Art und Weise, in der Docker Hub öffentliche Docker-Images speichert. Sie verwenden Webhooks, um Ereignisse auszulösen, wenn in einem Ihrer Registrierungsrepositorys bestimmte Aktionen stattfinden. Webhooks können auf Ereignisse auf Ebene der Registrierung reagieren oder auf ein bestimmtes Repositorytag begrenzt werden.

Weitere Hintergrundinformationen und Konzepte finden Sie unter [Verwenden von Azure Container Registry-Webhooks – Azure-Portal](./container-registry-intro.md).

## <a name="prerequisites"></a>Voraussetzungen

- Eine über Azure verwaltete Containerregistrierung: Erstellen Sie eine verwaltete Containerregistrierung in Ihrem Azure-Abonnement. Verwenden Sie beispielsweise das [Azure-Portal](container-registry-get-started-portal.md) oder die [Azure CLI](container-registry-get-started-azure-cli.md).
- Docker CLI: Installieren Sie das [Docker-Modul](https://docs.docker.com/engine/installation/), um Ihren lokalen Computer als Docker-Host einzurichten und auf die Befehle der Docker CLI zuzugreifen.

## <a name="create-webhook-azure-portal"></a>Erstellen von Webhooks im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu der Registrierung, in der Sie Webhooks erstellen möchten.

2. Wählen Sie auf dem Blatt „Container“ die **Webhooks** unter **DIENSTE** aus.

3. Wählen Sie aus der Symbolleiste des Webhook-Blatts die Option **Hinzufügen** aus.

4. Geben Sie folgende Informationen in das Formular *Webhook erstellen* ein:

| Wert | Beschreibung |
|---|---|
| Name | Der Name, den Sie dem Webhook geben möchten. Er darf nur Kleinbuchstaben und Zahlen enthalten und muss 5 bis 50 Zeichen lang sein. |
| Dienst-URI | Der URI, an den der Webhook POST-Benachrichtigungen senden soll. |
| Benutzerdefinierte Header | Header, die Sie zusammen mit der POST-Anforderung übergeben möchten. Diese müssen im Format „Schlüssel: Wert“ vorliegen. |
| Triggeraktionen | Aktionen, die den Webhook auslösen. Webhooks können zurzeit mit Push- und/oder Löschaktionen für ein Image ausgelöst werden. |
| Status | Der Status des Webhooks nach der Erstellung. Der Webhook ist standardmäßig aktiviert. |
| Bereich | Der Bereich, in dem der Webhook funktioniert. Standardmäßig ist der Bereich „alle Ereignisse in der Registrierung“. Der Bereich kann mit dem Format „Repository:Tag“ für ein Repository oder ein Tag angegeben werden. |

Beispielformular für Webhook:

![ACR-Webhookerstellungsoberfläche im Azure-Portal](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Erstellen von Webhooks über die Azure-Befehlszeilenschnittstelle

Um einen Webhook über die Azure-Befehlszeilenschnittstelle zu erstellen, verwenden Sie den Befehl [az acr webhook create](/cli/azure/acr/webhook#create).

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Testen von Webhooks

### <a name="azure-portal"></a>Azure-Portal

Bevor Sie einen Webhook für Push- und Löschaktionen in einem Containerimage verwenden, können Sie ihn über die Schaltfläche **Ping** testen. Der Pingbefehl sendet eine allgemeine POST-Anforderung an den angegebenen Endpunkt und protokolliert die Antwort. Dadurch können Sie überprüfen, ob Sie den Webhook ordnungsgemäß konfiguriert haben.

1. Wählen Sie den Webhook aus, den Sie testen möchten.
2. Wählen Sie in der oberen Symbolleiste **Ping**.
3. Überprüfen Sie die Antwort des Endpunkts in der Spalte **HTTP-STATUS**.

![ACR-Webhookerstellungsoberfläche im Azure-Portal](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Um einen ACR-Webhook über die Azure-Befehlszeilenschnittstelle zu testen, verwenden Sie den Befehl [az acr webhook ping](/cli/azure/acr/webhook#ping).

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
