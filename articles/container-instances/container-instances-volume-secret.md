---
title: Einbinden eines geheimen Volumes in Azure Container Instances
description: Hier erfahren Sie, wie Sie ein geheimes Volume zum Speichern vertraulicher Informationen einbinden, auf das von Ihren Containerinstanzen zugegriffen werden kann.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: a030582c885dd0a5680bd23046ea2a9c0329701a
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830070"
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Einbinden eines geheimen Volumes in Azure Container Instances

Hier erfahren Sie, wie Sie ein *geheimes* Volume in Ihren Containerinstanzen einbinden, damit die Container in Ihren Containergruppen vertrauliche Informationen speichern und abrufen können.

> [!NOTE]
> Zurzeit ist das Einbinden eines *geheimen* Volumes auf Linux-Container beschränkt. Unter [Festlegen von Umgebungsvariablen](container-instances-environment-variables.md) erfahren Sie, wie Sie sichere Umgebungsvariablen für Windows- und Linux-Container übergeben. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="secret-volume"></a>Geheimes Volume

Sie können ein *geheimes* Volume verwenden, um für die Container in einer Containergruppe vertrauliche Informationen bereitzustellen. Das *geheime* Volume speichert die angegebenen Geheimnisse in Dateien im Volume, auf die die Container in Ihrer Containergruppe zugreifen können. Mithilfe von Geheimnissen in einem *geheimen* Volume können Sie vermeiden, dass vertrauliche Daten wie SSH-Schlüssel oder Datenbankanmeldeinformationen in den Anwendungscode aufgenommen werden müssen.

Alle *geheimen* Volumes werden durch [tmpfs][tmpfs], ein auf RAM basierendes Dateisystem, gesichert. Ihre Inhalte werden nie in permanenten Speicher geschrieben.

## <a name="mount-a-secret-volume"></a>Einbinden eines geheimen Volumes

Sie können ein *geheimes* Volume in eine Containerinstanz einbinden, indem Sie zum Bereitstellen eine [Azure Resource Manager-Vorlage](/azure/templates/microsoft.containerinstance/containergroups) verwenden.

Füllen Sie in der Vorlage zunächst das `volumes`-Array im Abschnitt `properties` für die Containergruppe auf. Füllen Sie als Nächstes für jeden Container in der Containergruppe, in die Sie das *geheime* Volume einbinden möchten, das Array `volumeMounts` im Abschnitt `properties` der Containerdefinition auf.

Mit der folgenden Resource Manager-Vorlage wird beispielsweise eine Containergruppe mit einem einzelnen Container erstellt. Der Container bindet ein *geheimes* Volume mit zwei Base64-codierten Geheimnissen ein.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-secret.json -->
[!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

Ein Beispiel für die Bereitstellung der Containerinstanzen mit einer Azure Resource Manager-Vorlage finden Sie unter [Bereitstellen von Gruppen mit mehreren Containern in Azure Container Instances](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie andere Volumetypen in Azure Container Instances bereitgestellt werden:

* [Einbinden einer Azure-Dateifreigabe in Azure Container Instances](container-instances-volume-azure-files.md)
* [Einbinden eines emptyDir-Volumes in Azure Container Instances](container-instances-volume-emptydir.md)
* [Einbinden eines gitRepo-Volumes in Azure Container Instances](container-instances-volume-gitrepo.md)

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs