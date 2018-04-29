---
title: Problembehandlung für Azure Container Instances
description: Es wird beschrieben, wie Sie Probleme mit Azure Container Instances beheben.
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/14/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a4067db9955b804f126e889fa73641f69fef56ab
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshoot-container-and-deployment-issues-in-azure-container-instances"></a>Beheben von Container- und Bereitstellungsproblemen in Azure Container Instances

In diesem Artikel wird veranschaulicht, wie Sie die Problembehandlung beim Bereitstellen von Containern in Azure Container Instances durchführen. Außerdem werden einige allgemeine Probleme beschrieben, die unter Umständen auftreten.

## <a name="view-logs-and-stream-output"></a>Anzeigen von Protokollen und der Datenstromausgabe

Wenn Sie über einen fehlerhaften Container verfügen, können Sie zunächst mit [az container logs][az-container-logs] die Protokolle anzeigen und die Standardausgabe und Standardfehler mit [az container attach][az-container-attach] streamen.

### <a name="view-logs"></a>Anzeigen von Protokollen

Sie können den Befehl [az container logs][az-container-logs] verwenden, um Protokolle aus Ihrem Anwendungscode in einem Container anzuzeigen.

Hier ist die Protokollausgabe des aufgabenbasierten Beispielcontainers aus [Ausführen von Aufgaben in Containern in Azure Container Instances](container-instances-restart-policy.md) angegeben, die sich ergibt, nachdem eine ungültige URL zur Verarbeitung bereitgestellt wurde:

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
Traceback (most recent call last):
  File "wordcount.py", line 11, in <module>
    urllib.request.urlretrieve (sys.argv[1], "foo.txt")
  File "/usr/local/lib/python3.6/urllib/request.py", line 248, in urlretrieve
    with contextlib.closing(urlopen(url, data)) as fp:
  File "/usr/local/lib/python3.6/urllib/request.py", line 223, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/local/lib/python3.6/urllib/request.py", line 532, in open
    response = meth(req, response)
  File "/usr/local/lib/python3.6/urllib/request.py", line 642, in http_response
    'http', request, response, code, msg, hdrs)
  File "/usr/local/lib/python3.6/urllib/request.py", line 570, in error
    return self._call_chain(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 504, in _call_chain
    result = func(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 650, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found
```

### <a name="attach-output-streams"></a>Anfügen von Ausgabestreams

Mit dem Befehl [az container attach][az-container-attach] können Sie während des Containerstartvorgangs Diagnoseinformationen bereitstellen. Nachdem der Container gestartet wurde, werden STDOUT und STDERR auf Ihre lokale Konsole gestreamt.

Hier ist beispielsweise die Ausgabe aus dem aufgabenbasierten Container unter [Ausführen von Aufgaben in Containern in Azure Container Instances](container-instances-restart-policy.md) angegeben, die angezeigt wird, nachdem eine gültige URL einer großen zu verarbeitenden Textdatei bereitgestellt wurde:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
Container 'mycontainer' is in state 'Unknown'...
Container 'mycontainer' is in state 'Waiting'...
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-09 23:21:33+00:00) pulling image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Successfully pulled image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Created container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Started container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618

Start streaming logs:
[('the', 22979),
 ('I', 20003),
 ('and', 18373),
 ('to', 15651),
 ('of', 15558),
 ('a', 12500),
 ('you', 11818),
 ('my', 10651),
 ('in', 9707),
 ('is', 8195)]
```

## <a name="get-diagnostic-events"></a>Abrufen von Diagnoseereignissen

Wenn die Bereitstellung Ihres Containers nicht erfolgreich ist, sollten Sie die Diagnoseinformationen des Azure Container Instances-Ressourcenanbieters prüfen. Führen Sie zum Anzeigen der Ereignisse für Ihren Container den Befehl [az container show][az-container-show] aus:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

Die Ausgabe enthält die wichtigsten Eigenschaften Ihres Containers und die Bereitstellungsereignisse (hier verkürzt dargestellt):

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:49+00:00",
            "lastTimestamp": "2017-12-21T22:50:49+00:00",
            "message": "pulling image \"microsoft/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Successfully pulled image \"microsoft/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Created container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Started container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": null
        }
      ],
      ...
    }
  ],
  ...
}
```

## <a name="common-deployment-issues"></a>Häufige Bereitstellungsprobleme

In den folgenden Abschnitten werden häufige Probleme beschrieben, die für die meisten Fehler bei der Containerbereitstellung verantwortlich sind:

* [Imageversion wird nicht unterstützt](#image-version-not-supported)
* [Pullvorgang für Image nicht möglich](#unable-to-pull-image)
* [Container wird fortlaufend beendet und neu gestartet](#container-continually-exits-and-restarts)
* [Start des Containers dauert sehr lange](#container-takes-a-long-time-to-start)
* [Ressource nicht verfügbar](#resource-not-available-error)

## <a name="image-version-not-supported"></a>Imageversion wird nicht unterstützt

Wenn Sie ein Image angeben, das von Azure Container Instances nicht unterstützt werden kann, wird der Fehler `ImageVersionNotSupported` zurückgegeben. Der Wert des Fehlers lautet `The version of image '{0}' is not supported.` und gilt derzeit für Windows 1709-Images. Verwenden Sie ein LTS Windows-Image, um dieses Problem zu beheben. An der Unterstützung für Windows 1709-Images wird gearbeitet.

## <a name="unable-to-pull-image"></a>Pullvorgang für Image nicht möglich

Wenn Azure Container Instances den anfänglichen Pullvorgang für Ihr Image nicht durchführen kann, werden eine Zeit lang erneute Versuche gestartet, bevor der Fehler auftritt. Wenn das Image nicht abgerufen werden kann, werden in der Ausgabe von [az container show][az-container-show] beispielsweise folgende Ereignisse angezeigt:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"microsoft/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"microsoft/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

Löschen Sie den Container, und führen Sie die Bereitstellung erneut durch, um zu versuchen, das Problem zu beheben. Achten Sie hierbei darauf, dass Sie den Imagenamen richtig eingegeben haben.

## <a name="container-continually-exits-and-restarts"></a>Container wird fortlaufend beendet und neu gestartet

Wenn Ihr Container bis zum Ende ausgeführt und anschließend neu gestartet wird, müssen Sie unter Umständen für [Neustartrichtlinie](container-instances-restart-policy.md) entweder **OnFailure** oder **Never** festlegen. Wenn Sie **OnFailure** festlegen und der Container weiterhin neu gestartet wird, liegt unter Umständen ein Problem mit der Anwendung oder dem Skript vor, die bzw. das im Container ausgeführt wird.

Die Container Instances-API enthält eine `restartCount`-Eigenschaft. Die Anzahl von Neustarts für einen Container können Sie mit dem Befehl [az container show][az-container-show] in der Azure CLI 2.0 überprüfen. In der folgenden (gekürzten) Beispielausgabe sehen Sie die `restartCount`-Eigenschaft am Ende der Ausgabe.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Von den meisten Containerimages für Linux-Distributionen wird eine Shell, z.B. Bash, als Standardbefehl festgelegt. Da eine Shell allein kein Dienst mit langer Ausführungsdauer ist, werden diese Container sofort beendet und landen in einer Neustartschleife, wenn sie mit der Standardneustartrichtlinie **Always** konfiguriert wurden.

## <a name="container-takes-a-long-time-to-start"></a>Start des Containers dauert sehr lange

Die folgenden beiden Hauptfaktoren tragen zur Dauer des Containerstartvorgangs in Azure Container Instances bei:

* [Imagegröße](#image-size)
* [Imagespeicherort](#image-location)

Für Windows-Images gelten noch [weitere Aspekte](#use-recent-windows-images).

### <a name="image-size"></a>Imagegröße

Wenn das Starten Ihres Containers sehr lange dauert, aber nach einiger Zeit erfolgreich ist, sollten Sie sich zuerst die Größe des Containerimage ansehen. Da Azure Container Instances den Pullvorgang für Ihr Containerimage nach Bedarf durchführt, ist die Startdauer direkt von der Größe abhängig.

Sie können die Größe Ihres Containerimages mit dem Befehl `docker images` in der Docker CLI anzeigen:

```console
$ docker images
REPOSITORY                  TAG       IMAGE ID        CREATED        SIZE
microsoft/aci-helloworld    latest    7f78509b568e    13 days ago    68.1MB
```

Sie können die Imagegrößen klein halten, indem Sie sicherstellen, dass Ihr endgültiges Image keine Elemente enthält, die zur Laufzeit nicht erforderlich sind. Eine Möglichkeit ist die Verwendung von [mehrstufigen Builds][docker-multi-stage-builds]. Mit mehrstufigen Builds können Sie leicht sicherstellen, dass das endgültige Image nur die Artefakte enthält, die Sie für Ihre Anwendung benötigen – und keinen zusätzlichen Inhalt, der zur Erstellungszeit benötigt wurde.

### <a name="image-location"></a>Imagespeicherort

Eine weitere Möglichkeit, die Auswirkungen des Pullvorgangs für das Image auf die Startdauer Ihres Containers zu reduzieren, ist das Hosten des Containerimages in [Azure Container Registry](/azure/container-registry/) in derselben Region, in der Sie Containerinstanzen bereitstellen möchten. Auf diese Weise wird der Netzwerkpfad verkürzt, den das Containerimage zurücklegen muss, sodass sich die Downloadzeit erheblich reduziert.

### <a name="use-recent-windows-images"></a>Verwenden von kürzlich erstellten Windows-Images

Für Azure Container Instances wird ein Mechanismus für die Zwischenspeicherung verwendet, um die Dauer des Containerstartvorgangs für Images basierend auf bestimmten Windows-Images zu verkürzen.

Verwenden Sie zum Sicherstellen eines möglichst kurzen Windows-Containerstartvorgangs eine der **drei zuletzt erstellten** Versionen der folgenden **beiden Images** als Basisimage:

* [Windows Server 2016][docker-hub-windows-core] (nur LTS)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

## <a name="resource-not-available-error"></a>Ressource nicht verfügbar

Aufgrund der unterschiedlichen Auslastung regionaler Ressourcen in Azure wird beim Versuch, eine Containerinstanz bereitzustellen, möglicherweise die folgende Fehlermeldung angezeigt:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Dieser Fehler gibt an, dass aufgrund starker Auslastung in der Region, in der die Bereitstellung durchgeführt werden soll, die für den Container angegebenen Ressourcen zum aktuellen Zeitpunkt nicht zugeordnet werden können. Führen Sie einen oder mehrere der folgenden Schritte aus, um das Problem zu beheben.

* Überprüfen Sie, ob Ihre Einstellungen für die Containerbereitstellung innerhalb der unter [Kontingente und regionale Verfügbarkeit für Azure Container Instances](container-instances-quotas.md#region-availability) definierten Parameter liegen.
* Geben Sie niedrigere CPU- und Arbeitsspeichereinstellungen für den Container an.
* Führen Sie die Bereitstellung in einer anderen Azure-Region durch.
* Führen Sie die Bereitstellung zu einem späteren Zeitpunkt durch.

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show