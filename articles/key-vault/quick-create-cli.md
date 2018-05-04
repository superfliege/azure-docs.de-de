---
title: 'Azure-Schnellstart: Erstellen einer Key Vault-Instanz – Befehlszeilenschnittstelle | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Azure Key Vault-Instanz mithilfe der Befehlszeilenschnittstelle erstellen.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/16/2018
ms.author: barclayn
ms.openlocfilehash: aaf8b93a41399b7754fb458d7d1d278a64f82139
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="quickstart-create-an-azure-key-vault-using-the-cli"></a>Schnellstart: Erstellen einer Azure Key Vault-Instanz mithilfe der Befehlszeilenschnittstelle

Azure Key Vault ist ein Clouddienst, der als sicherer Geheimnisspeicher fungiert. Dadurch können Schlüssel, Kennwörter, Zertifikate und andere Geheimnisse sicher gespeichert werden. Weitere Informationen zu Key Vault finden Sie in der [Übersicht](key-vault-overview.md). Über die Azure-Befehlszeilenschnittstelle können Azure-Ressourcen mithilfe von Befehlen oder Skripts erstellt und verwaltet werden. In diesem Artikel erstellen Sie eine Key Vault-Instanz. In dieser Schnellstartanleitung erstellen Sie einen Schlüsseltresor. Danach speichern Sie ein Geheimnis.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.4 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

Geben Sie Folgendes ein, um sich über die Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

Weitere Informationen zu den Anmeldeoptionen für die Befehlszeilenschnittstelle finden Sie unter [Anmelden mit Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *ContosoResourceGroup* am Standort *eastus*.

```azurecli
az group create --name 'ContosoResourceGroup' --location eastus
```

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Als Nächstes erstellen Sie eine Key Vault-Instanz in der Ressourcengruppe aus dem vorherigen Schritt. Dazu müssen einige Informationen angegeben werden:

- Im Rahmen dieser Schnellstartanleitung verwenden wir **Contoso-vault2**. Geben Sie bei Ihren Tests einen eindeutigen Namen an.
- Der Ressourcengruppenname lautet **ContosoResourceGroup**.
- Als Standort wird **USA, Osten** verwendet.

```azurecli
az keyvault create --name 'Contoso-Vault2' --resource-group 'ContosoResourceGroup' --location eastus
```

Die Ausgabe dieses Cmdlets zeigt Eigenschaften der neu erstellten Key Vault-Instanz. Beachten Sie die beiden folgenden Eigenschaften:

- **Tresorname:** In diesem Beispiel wird **Contoso-Vault2** verwendet. Sie verwenden diesen Namen für andere Schlüsseltresor-Befehle.
- **Tresor-URI:** https://contoso-vault2.vault.azure.net/ in diesem Beispiel. Anwendungen, die Ihren Tresor über die zugehörige REST-API nutzen, müssen diesen URI verwenden.

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-key-vault"></a>Hinzufügen eines Geheimnisses zu Key Vault

Ein Geheimnis kann dem Tresor mit einigen wenigen Zusatzschritten hinzugefügt werden. Dieses Kennwort kann dann von einer Anwendung verwendet werden. Das Kennwort soll **ExamplePassword** heißen und den Wert **Pa$$w0rd** enthalten.

Geben Sie die folgenden Befehle ein, um in Key Vault ein Geheimnis namens **ExamplePassword** mit dem Wert **Pa$$w0rd** zu erstellen:

```azurecli
az keyvault secret set --vault-name 'Contoso-Vault2' --name 'ExamplePassword' --value 'Pa$$w0rd'
```

Nun können Sie mit dem dazugehörigen URI auf das Kennwort verweisen, das Sie der Azure Key Vault-Instanz hinzugefügt haben. Verwenden Sie **https://ContosoVault.vault.azure.net/secrets/ExamplePassword**, um die aktuelle Version abzurufen. 

Geben Sie Folgendes ein, um den Wert im Geheimnis als Nur-Text anzuzeigen:

```azurecli
az keyvault secret show --name 'ExamplePassword' --vault-name 'Contoso-Vault2'
```

Sie haben nun eine Key Vault-Instanz erstellt sowie ein Geheimnis gespeichert und abgerufen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.
Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl [az group delete](/cli/azure/group#delete) aus, um die Ressourcengruppe und alle dazugehörigen Ressourcen zu löschen. Die Ressourcen können wie folgt gelöscht werden:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Key Vault-Instanz erstellt und ein Geheimnis darin gespeichert. Das Tutorial zu Webanwendungen mit Key Vault enthält weitere Informationen zu Key Vault und zur Verwendung mit Ihren Anwendungen.

> [!div class="nextstepaction"]
> [Verwenden des Azure-Schlüsseltresors aus einer Webanwendung](key-vault-use-from-web-application.md) Im Tutorial [Configure an Azure web application to read a secret from Key Vault](tutorial-web-application-keyvault.md) (Konfigurieren einer Azure-Webanwendung für das Lesen eines Geheimnisses aus Key Vault) erfahren Sie, wie Sie mithilfe einer Webanwendung mit [verwalteter Dienstidentität](/active-directory/managed-service-identity/overview.md) ein Geheimnis aus Key Vault lesen.
