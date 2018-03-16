---
title: "Bewährte Methoden in Azure Container Registry"
description: "Erfahren Sie, wie Sie Azure Container Registry anhand dieser bewährten Methoden effektiv verwenden."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 684b778f57da4adb331958c5daef6b9906b6d253
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="best-practices-for-azure-container-registry"></a>Bewährte Methoden für Azure Container Registry

Durch Befolgen dieser bewährten Methoden sorgen Sie für eine optimale Leistung und kostengünstige Verwendung Ihrer privaten Docker-Registrierung in Azure.

## <a name="network-close-deployment"></a>Netzwerknahe Bereitstellung

Erstellen Sie Ihre Containerregistrierung in derselben Azure-Region, in der Sie Container bereitstellen. Indem Ihre Registrierung in eine Region platziert wird, die sich in Netzwerknähe Ihrer Containerhosts befindet, können Sie sowohl Wartezeiten als auch Kosten senken.

Die netzwerknahe Bereitstellung ist einer der Hauptgründe für die Verwendung einer privaten Containerregistrierung. Docker-Images verfügen über ein effizientes [Ebenenkonstrukt](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/), das inkrementelle Bereitstellungen ermöglicht. Allerdings müssen neue Knoten alle Ebenen mithilfe von Pull übertragen, die für ein bestimmtes Image erforderlich sind. Bei diesem anfänglichen `docker pull` können schnell mehrere Gigabyte zusammenkommen. Durch eine private Registrierung in der Nähe Ihrer Bereitstellung wird die Netzwerklatenz auf ein Mindestmaß verringert.
Darüber hinaus werden für alle öffentlichen Clouds, einschließlich Azure, Gebühren für ausgehenden Netzwerkdatenverkehr erhoben. Wenn Images aus einem Rechenzentrum in ein anderes übertragen werden, entstehen neben Wartezeiten auch Kosten für ausgehenden Netzwerkdatenverkehr.

## <a name="geo-replicate-multi-region-deployments"></a>Georeplikation von Bereitstellungen in mehreren Regionen

Verwenden Sie das Feature [Georeplikation](container-registry-geo-replication.md) von Azure Container Registry, wenn Sie Container in mehreren Regionen bereitstellen. Ganz gleich, ob Sie globale Kunden von lokalen Rechenzentren aus bedienen oder Ihr Entwicklungsteam sich an unterschiedlichen Standorten befindet – durch die Georeplikation Ihrer Registrierung können Sie die Registrierungsverwaltung vereinfachen und Wartezeiten auf ein Minimum reduzieren. Dieses Feature befindet sich zurzeit in der Vorschau und steht für [Premium](container-registry-skus.md)-Registrierungen zur Verfügung.

Sehen Sie sich die dreiteilige Tutorialreihe [Georeplikation in Azure Container Registry](container-registry-tutorial-prepare-registry.md) an, um sich über die Verwendung der Georeplikation zu informieren.

## <a name="repository-namespaces"></a>Repositorynamespaces

Mithilfe von Repositorynamespaces können Sie die gemeinsame Nutzung einer einzigen Registrierung in mehreren Gruppen innerhalb Ihrer Organisation ermöglichen. Registrierungen können in mehreren Bereitstellungen und Teams gemeinsam verwendet werden. Azure Container Registry unterstützt geschachtelte Namespaces und ermöglicht damit die Isolation von Gruppen.

Sehen Sie sich beispielsweise die folgenden Containerimagetags an. Images, die unternehmensweit verwendet werden (z.B. `aspnetcore`), werden im Stammnamespace abgelegt, während Containerimages, die zu den Produktions- und Marketinggruppen gehören, jeweils eigene Namespaces verwenden.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Dedizierte Ressourcengruppe

Da Containerregistrierungen Ressourcen sind, die containerhostübergreifend verwendet werden, sollte sich eine Registrierung in einer eigenen Ressourcengruppe befinden.

Auch wenn Sie mit einem bestimmten Hosttyp (z.B. Azure Container Instances) experimentieren, möchten Sie die Containerinstanz wahrscheinlich am Ende löschen. Möglicherweise empfiehlt es sich jedoch, die Sammlung von Images beizubehalten, die Sie mithilfe von Push in Azure Container Registry übertragen haben. Indem Sie Ihre Registrierung in einer eigenen Ressourcengruppe ablegen, minimieren Sie das Risiko, dass die Sammlung von Images in der Registrierung versehentlich gelöscht wird, wenn Sie die Ressourcengruppe der Containerinstanz löschen.

## <a name="authentication"></a>Authentifizierung

Bei der Authentifizierung bei einer Azure-Containerregistrierung gibt es im Wesentlichen zwei Szenarien: die individuelle Authentifizierung und die Dienstauthentifizierung (oder „monitorlose Authentifizierung“). Die folgende Tabelle enthält eine kurze Übersicht über diese Szenarien sowie die jeweils empfohlene Authentifizierungsmethode.

| Typ | Beispielszenario | Empfohlene Methode |
|---|---|---|
| Einzelne Identität | Ein Entwickler, der Images mittels Pull auf seinen bzw. mittels Push von seinem Entwicklungscomputer überträgt. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Monitorlose/Dienstidentität | Build- und Bereitstellungspipelines, an denen der Benutzer nicht direkt beteiligt ist. | [Dienstprinzipal](container-registry-authentication.md#service-principal) |

Ausführliche Informationen zur Azure Container Registry-Authentifizierung finden Sie unter [Authentifizieren bei einer Azure-Containerregistrierung](container-registry-authentication.md).

## <a name="manage-registry-size"></a>Verwalten von Registrierungsgrößen

Die Speichereinschränkungen der einzelnen [Containerregistrierungs-SKUs][container-registry-skus] sind jeweils für eines der folgenden Szenarien vorgesehen: **Basic** für den Einstieg, **Standard** für den Großteil der Produktionsanwendungen und **Premium** für Hyperskalierungsleistung und [Georeplikation][container-registry-geo-replication]. Während der gesamten Dauer Ihrer Registrierung sollten Sie die Größe durch regelmäßiges Löschen von ungenutztem Inhalt verwalten.

Sie können die aktuelle Nutzung einer Registrierung in der **Übersicht** der Containerregistrierung im Azure-Portal finden:

![Informationen zur Registrierungsnutzung im Azure-Portal][registry-overview-quotas]

Sie können die Größe Ihrer Registrierung über die [Azure-Befehlszeilenschnittstelle][azure-cli] oder über das [Azure-Portal][azure-portal] verwalten. Das Löschen von Repositorys und Images wird nur für verwaltete SKUs (Basic, Standard, Premium) unterstützt. In einer klassischen Registrierung können Sie keine Repositorys, Images oder Tags löschen.

### <a name="delete-in-azure-cli"></a>Löschen über die Azure-Befehlszeilenschnittstelle

Mit dem Befehl [az acr repository delete][az-acr-repository-delete] können Sie ein Repository oder den Inhalt eines Repositorys löschen.

Wenn Sie ein Repository einschließlich aller darin enthaltenen Tags und Imageebenendaten löschen möchten, geben Sie beim Ausführen von [az acr repository delete][az-acr-repository-delete] nur den Repositorynamen an. Im folgenden Beispiel werden das Repository *myapplication* sowie alle darin enthaltenen Tags und Imageebenendaten gelöscht:

```azurecli
az acr repository delete --name myregistry --repository myapplication
```

Mit den Argumenten `--tag` und `--manifest` können Sie auch Imagedaten aus einem Repository löschen. Ausführliche Informationen zu diesen Argumenten finden Sie in der [Befehlsreferenz für „az acr repository delete“][az-acr-repository-delete].

### <a name="delete-in-azure-portal"></a>Löschen über das Azure-Portal

Wenn Sie ein Repository aus einer Registrierung im Azure-Portal löschen möchten, navigieren Sie zunächst zu Ihrer Containerregistrierung. Klicken Sie unter **DIENSTE** auf **Repositorys**, und klicken Sie dann mit der rechten Maustaste auf das Repository, das Sie löschen möchten. Klicken Sie auf **Löschen**, um das Repository und die darin enthaltenen Docker-Images zu löschen.

![Löschen eines Repositorys aus einem Verzeichnis im Azure-Portal][delete-repository-portal]

Auf ähnliche Weise können Sie auch Tags aus einem Repository löschen. Navigieren Sie zu dem Repository, klicken Sie unter **TAGS** mit der rechten Maustaste auf das Tag, das Sie löschen möchten, und klicken Sie anschließend auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Azure Container Registry ist in mehrere Tarifen (SKUs) verfügbar, die jeweils verschiedene Funktionen bereitstellen. Ausführliche Informationen zu verfügbaren SKUs finden Sie unter [Azure Container Registry-SKUs](container-registry-skus.md).

<!-- IMAGES -->
[delete-repository-portal]: ./media/container-registry-best-practices/delete-repository-portal.png
[registry-overview-quotas]: ./media/container-registry-best-practices/registry-overview-quotas.png

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az_acr_repository_delete
[azure-cli]: /cli/azure
[azure-portal]: https://portal.azure.com
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-skus]: container-registry-skus.md
