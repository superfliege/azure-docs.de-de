---
title: Einbinden eines gitRepo-Volumes in Azure Container Instances
description: Hier erfahren Sie, wie Sie ein gitRepo-Volume einbinden, um ein Git-Repository in Ihren Containerinstanzen zu klonen.
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 9acde9259fcb392458e7b2fa7d3369776978285e
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Einbinden eines gitRepo-Volumes in Azure Container Instances

Hier erfahren Sie, wie Sie ein *gitRepo*-Volume einbinden, um ein Git-Repository in Ihren Containerinstanzen zu klonen.

> [!NOTE]
> Zurzeit ist das Einbinden eines *gitRepo*-Volumes auf Linux-Container beschränkt. Bis alle Features auch für Windows-Container verfügbar sind, finden Sie die aktuellen Plattformunterschiede unter [Kontingente und Regionsverfügbarkeit für Azure Container Instances](container-instances-quotas.md).

## <a name="gitrepo-volume"></a>GitRepo-Volume

Das *gitRepo*-Volume bindet ein Verzeichnis ein und klont darin beim Containerstart das angegebene Git-Repository. Bei Verwendung eines *gitRepo*-Volumes in Ihren Containerinstanzen müssen Sie dazu keinen Code in Ihren Anwendungen hinzufügen.

Wenn Sie ein *gitRepo*-Volume einbinden, können Sie zum Konfigurieren des Volumes drei Eigenschaften festlegen:

| Eigenschaft | Erforderlich | BESCHREIBUNG |
| -------- | -------- | ----------- |
| `repository` | Ja | Die vollständige URL (einschließlich `http://` bzw. `https://`) des zu klonenden Git-Repositorys|
| `directory` | Nein | Das Verzeichnis, in dem das Repository geklont werden soll. Der Pfad darf `..` weder enthalten noch darf er damit beginnen.  Bei Angabe von `.` wird das Repository im Verzeichnis des Volumes geklont. Andernfalls wird das Git-Repository in einem Unterverzeichnis mit dem angegebenen Namen im Volumeverzeichnis geklont. |
| `revision` | Nein  | Der Commithash der zu klonenden Version. Wird er nicht angegeben, wird die `HEAD`-Revision geklont. |

## <a name="mount-a-gitrepo-volume"></a>Einbinden eines gitRepo-Volumes

Sie können ein *gitRepo*-Volume in eine Containerinstanz einbinden, indem Sie zum Bereitstellen eine [Azure Resource Manager-Vorlage](/azure/templates/microsoft.containerinstance/containergroups) verwenden.

Füllen Sie in der Vorlage zunächst das `volumes`-Array im Abschnitt `properties` für die Containergruppe auf. Füllen Sie als Nächstes für jeden Container in der Containergruppe, in der Sie das *gitRepo*-Volume einbinden möchten, das Array `volumeMounts` im Abschnitt `properties` der Containerdefinition auf.

Mit der folgenden Resource Manager-Vorlage wird beispielsweise eine Containergruppe mit einem einzelnen Container erstellt. Der Container klont zwei GitHub-Repositorys, die durch die *gitRepo*-Volumeblöcke angegeben werden. Das zweite Volume enthält zusätzliche Eigenschaften, die ein Verzeichnis zum Klonen angeben, sowie den Commithash einer spezifischen Revision, die geklont werden soll.

[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

Die Verzeichnisstruktur der zwei geklonten Repositorys, die in der vorhergehenden Vorlage definiert werden, sieht wie folgt aus:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Ein Beispiel für die Bereitstellung der Containerinstanzen mit einer Azure Resource Manager-Vorlage finden Sie unter [Bereitstellen von Gruppen mit mehreren Containern in Azure Container Instances](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie andere Volumetypen in Azure Container Instances bereitgestellt werden:

* [Einbinden einer Azure-Dateifreigabe in Azure Container Instances](container-instances-volume-azure-files.md)
* [Einbinden eines emptyDir-Volumes in Azure Container Instances](container-instances-volume-emptydir.md)
* [Einbinden eines geheimen Volumes in Azure Container Instances](container-instances-volume-secret.md)
