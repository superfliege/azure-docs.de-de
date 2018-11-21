---
title: Inhaltsformate für die Azure Container Registry
description: Erfahren Sie mehr über die unterstützten Inhaltsformate in Azure Container Registry.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 10/31/2018
ms.author: danlep
ms.openlocfilehash: e7155604339bc634078fd022e05ede5f902bc0d8
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634825"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>In Azure Container Registry unterstützte Inhaltsformate

Verwenden Sie ein privates Repository in Azure Container Registry, um eines der folgenden Inhaltsformate zu verwalten. 

## <a name="docker-compatible-container-images"></a>Docker-kompatible Containerimages

* [Docker Image Manifest V2, Schema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker Image Manifest V2, Schema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) – enthält Manifestlisten, die es Registrys ermöglichen, Images von mehreren Plattformen unter einem einzigen Verweis „image:tag“ zu speichern

* [Spezifikation für das Imageformat Open Container Initiative (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md) 


## <a name="helm-charts"></a>Helm-Diagramme

Azure Container Registry kann auch Repositorys für [Helm-Diagramme](https://helm.sh/) hosten, ein Paketformat, das zur schnellen Verwaltung und Bereitstellung von Anwendungen für Kubernetes verwendet wird. [Helm Client](https://docs.helm.sh/using_helm/#installing-helm) Version 2.11.0 oder höher wird unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* Schauen Sie sich an wie Sie Images mit Azure Container Registry [per Push übertragen und abrufen](container-registry-get-started-docker-cli.md).

* Verwenden Sie [ACR-Aufgaben](container-registry-tasks-overview.md), um Containerimages zu erstellen und zu testen. 

* Verwenden Sie das [Moby BuildKit](https://github.com/moby/buildkit) zum Erstellen und Verpacken von Containern im OCI-Format.

* Richten Sie ein [Helm-Repository](container-registry-helm-repos.md) ein, dass in Azure Container Registry gehostet wird. 


