---
title: Bereitstellen in Azure Container Instances aus Azure Container Registry | Azure-Dokumentation
description: Bereitstellen in Azure Container Instances aus Azure Container Registry
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4205b47dc67920021812c1e573a98de64ad198ec
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Bereitstellen in Azure Container Instances aus Azure Container Registry | Azure-Dokumentation

Azure Container Registry (ACR) ist eine Azure-basierte, private Registrierung für Docker-Containerimages. In diesem Artikel wird beschrieben, wie Containerimages, die in Azure Container Registry gespeichert sind, auf Azure Container Instances bereitgestellt werden können.

## <a name="deploy-with-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

Die Azure-CLI enthält Befehle zum Erstellen und Verwalten von Containern in Azure Container Instances. Wenn Sie ein privates Image im Befehl [az container create][az-container-create] angeben, können Sie auch das Kennwort für die Imageregistrierung festlegen, das erforderlich ist, um sich bei der Containerregistrierung zu authentifizieren.

```azurecli-interactive
az container create --resource-group myResourceGroup --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword
```

Der Befehl [az container create][az-container-create] unterstützt auch die Angabe von `--registry-login-server` und `--registry-username`. Allerdings ist standardmäßig der Anmeldeserver für Azure Container Registry immer *registryname*.azurecr.io, und der Standardbenutzername ist *registryname*, sodass diese Werte aus dem Imagenamen abgeleitet werden, wenn sie nicht explizit bereitgestellt werden.

## <a name="deploy-with-azure-resource-manager-template"></a>Bereitstellen mit einer Azure Resource Manager-Vorlage

Sie können die Eigenschaften von Azure Container Registry in einer Azure Resource Manager-Vorlage angeben, indem Sie die `imageRegistryCredentials`-Eigenschaft in die Definition der Containergruppe einbeziehen:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Um das Kennwort für Ihre Containerregistrierung nicht direkt in der Vorlage zu speichern, wird empfohlen, dass Sie es als geheimen Schlüssel in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) aufbewahren und in der Vorlage mithilfe der [nativen Integration zwischen Azure Resource Manager und Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md) auf darauf verweisen.

## <a name="deploy-with-azure-portal"></a>Bereitstellen über das Azure-Portal

Wenn Sie Containerimages im Azure Container Registry verwalten, können Sie leicht mit dem Azure-Portal einen Container in Azure Container Instances erstellen.

1. Navigieren Sie im Azure-Portal wieder zu Ihrer Containerregistrierung.

2. Klicken Sie auf **Repositorys**, und wählen Sie dann das Repository aus, über das die Bereitstellung erfolgen soll. Klicken Sie mit der rechten Maustaste auf das Tag für das Containerimage, das Sie bereitstellen möchten, und klicken Sie auf **Instanz ausführen**.

    ![„Instanz ausführen“ in Azure Container Registry im Azure-Portal][acr-runinstance-contextmenu]

3. Geben Sie einen Namen für den Container und einen Namen für die Ressourcengruppe ein. Sie können den Standardwert auch bei Bedarf ändern.

    ![Erstellmenü für Azure Container Instances][acr-create-deeplink]

4. Sobald die Bereitstellung abgeschlossen wurde, können Sie vom Benachrichtigungsbereich aus zur Containergruppe navigieren, um die IP-Adresse und andere Eigenschaften zu sehen.

    ![Detailansicht für eine Containergruppe in Azure Container Instances][aci-detailsview]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Container erstellen, sie an eine private Containerregistrierung und an Azure Container Instances weiterleiten, indem Sie das [Tutorial abschließen](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create