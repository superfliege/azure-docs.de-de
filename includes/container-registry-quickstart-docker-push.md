---
title: Includedatei
description: Includedatei
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 01/23/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 7ed8b96a528d56b28262936c4b200762b3e93b8e
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302243"
---
## <a name="push-image-to-registry"></a>Pushen eines Image in die Registrierung

Um ein Image mithilfe von Push an Ihre Azure Container Registry-Instanz übertragen zu können, benötigen Sie zunächst ein Image. Wenn Sie noch nicht über lokale Containerimages verfügen, führen Sie den folgenden [docker pull][docker-pull]-Befehl aus, um ein vorhandenes Image aus dem Docker-Hub abzurufen. Pullen Sie für dieses Beispiel das Image `hello-world`.

```Docker
docker pull hello-world
```

Bevor Sie ein Image mithilfe von Push in Ihre Registrierung übertragen können, müssen Sie es mit dem vollqualifizierten Namen Ihres ACR-Anmeldeservers markieren. Der Name des Anmeldeservers wird im Format *\<Registrierungsname\>.azurecr.io* (nur Kleinbuchstaben) angegeben, z. B. *mycontainerregistry007.azurecr.io*.

Markieren Sie das Image mithilfe des Befehls [docker tag][docker-tag]. Ersetzen Sie `<acrLoginServer>` durch den Anmeldeservernamen Ihrer ACR-Instanz.

```Docker
docker tag hello-world <acrLoginServer>/hello-world:v1
```

Nun können Sie das Image mit [docker push][docker-push] mithilfe von Push an die ACR-Instanz übertragen. Ersetzen Sie `<acrLoginServer>` durch den Anmeldeservernamen Ihrer ACR-Instanz. In diesem Beispiel wird das Repository **hello-world** mit dem Image `hello-world:v1` erstellt.

```Docker
docker push <acrLoginServer>/hello-world:v1
```

Nachdem das Image in Ihre Containerregistrierung gepusht wurde, entfernen Sie das Image `hello-world:v1` aus Ihrer lokalen Docker-Umgebung. (Beachten Sie, dass der Befehl [docker rmi][docker-rmi] nicht das Image aus dem Repository **hello-world** in Ihrer Azure Container Registry-Instanz entfernt.)

```Docker
docker rmi <acrLoginServer>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

