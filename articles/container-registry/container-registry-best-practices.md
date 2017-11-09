---
title: "Bewährte Methoden in Azure Container Registry"
description: "Erfahren Sie, wie Sie Azure Container Registry anhand dieser bewährten Methoden effektiv verwenden."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 9ec5573082dbf9de1288e1511f5041f8c20b416e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Bewährte Methoden für Azure Container Registry

Durch Befolgen dieser bewährten Methoden sorgen Sie für eine optimale Leistung und kostengünstige Verwendung Ihrer privaten Docker-Registrierung in Azure.

## <a name="network-close-deployment"></a>Netzwerknahe Bereitstellung

Erstellen Sie Ihre Containerregistrierung in derselben Azure-Region, in der Sie Container bereitstellen. Indem Ihre Registrierung in eine Region platziert wird, die sich in Netzwerknähe Ihrer Containerhosts befindet, können Sie sowohl Wartezeiten als auch Kosten senken.

Die netzwerknahe Bereitstellung ist einer der Hauptgründe für die Verwendung einer privaten Containerregistrierung. Docker-Images weisen ein hervorragendes [Ebenenkonstrukt](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) auf, das inkrementelle Bereitstellungen ermöglicht. Allerdings müssen neue Knoten alle Ebenen mithilfe von Pull übertragen, die für ein bestimmtes Image erforderlich sind. Bei diesem anfänglichen `docker pull` können schnell mehrere Gigabyte zusammenkommen. Durch eine private Registrierung in der Nähe Ihrer Bereitstellung wird die Netzwerklatenz auf ein Mindestmaß verringert.
Darüber hinaus werden für alle öffentlichen Clouds, einschließlich Azure, Gebühren für ausgehenden Netzwerkdatenverkehr erhoben. Wenn Images aus einem Rechenzentrum in ein anderes übertragen werden, entstehen neben Wartezeiten auch Kosten für ausgehenden Netzwerkdatenverkehr.

## <a name="geo-replicate-multi-region-deployments"></a>Georeplikation von Bereitstellungen in mehreren Regionen

Verwenden Sie das Feature [Georeplikation](container-registry-geo-replication.md) von Azure Container Registry, wenn Sie Container in mehreren Regionen bereitstellen. Ganz gleich, ob Sie globale Kunden von lokalen Rechenzentren aus bedienen oder Ihr Entwicklungsteam sich an unterschiedlichen Standorten befindet – durch die Georeplikation Ihrer Registrierung können Sie die Registrierungsverwaltung vereinfachen und Wartezeiten auf ein Minimum reduzieren. Dieses Feature befindet sich zurzeit in der Vorschau und steht für [Premium](container-registry-skus.md#premium)-Registrierungen zur Verfügung.

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

## <a name="next-steps"></a>Nächste Schritte

Azure Container Registry ist in mehrere Tarifen (SKUs) verfügbar, die jeweils verschiedene Funktionen bereitstellen. Ausführliche Informationen zu verfügbaren SKUs finden Sie unter [Azure Container Registry-SKUs](container-registry-skus.md).