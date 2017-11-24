---
title: "Problembehandlung für Azure Container Instances"
description: Es wird beschrieben, wie Sie Probleme mit Azure Container Instances beheben.
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 78bd45f7f71fd25e351d4e9b922a6a3f171437fd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Beheben von Bereitstellungsproblemen für Azure Container Instances

In diesem Artikel wird veranschaulicht, wie Sie die Problembehandlung beim Bereitstellen von Containern in Azure Container Instances durchführen. Außerdem werden einige allgemeine Probleme beschrieben, die unter Umständen auftreten.

## <a name="get-diagnostic-events"></a>Abrufen von Diagnoseereignissen

Sie können den Befehl [az container logs](/cli/azure/container#logs) verwenden, um Protokolle aus Ihrem Anwendungscode in einem Container anzuzeigen. Falls die Bereitstellung Ihres Containers nicht erfolgreich ist, sollten Sie die Diagnoseinformationen des Azure Container Instances-Ressourcenanbieters prüfen. Führen Sie den folgenden Befehl aus, um die Ereignisse für Ihren Container anzuzeigen:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

Die Ausgabe enthält die wichtigsten Eigenschaften Ihres Containers und die Bereitstellungsereignisse:

```bash
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
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Häufige Bereitstellungsprobleme

Es gibt einige häufige Probleme, die die Ursache der meisten Bereitstellungsfehler sind.

## <a name="unable-to-pull-image"></a>Pullvorgang für Image nicht möglich

Wenn Azure Container Instances den anfänglichen Pullvorgang für Ihr Image nicht durchführen kann, werden eine Zeit lang erneute Versuche gestartet, bevor der Fehler auftritt. Wenn der Pullvorgang für das Image nicht möglich ist, werden beispielsweise folgende Ereignisse angezeigt:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

Löschen Sie den Container, und führen Sie die Bereitstellung erneut durch, um zu versuchen, das Problem zu beheben. Achten Sie hierbei darauf, dass Sie den Imagenamen richtig eingegeben haben.

## <a name="container-continually-exits-and-restarts"></a>Container wird fortlaufend beendet und neu gestartet

Wenn Ihr Container bis zum Ende ausgeführt und anschließend neu gestartet wird, müssen Sie unter Umständen für [Neustartrichtlinie](container-instances-restart-policy.md) entweder **OnFailure** oder **Never** festlegen. Wenn Sie **OnFailure** festlegen und der Container weiterhin neu gestartet wird, liegt unter Umständen ein Problem mit der Anwendung oder dem Skript vor, die bzw. das im Container ausgeführt wird.

Die Container Instances-API enthält eine `restartCount`-Eigenschaft. Die Anzahl von Neustarts für einen Container können Sie mit dem Befehl [az container show](/cli/azure/container#az_container_show) in der Azure CLI 2.0 überprüfen. In der folgenden (gekürzten) Beispielausgabe sehen Sie die `restartCount`-Eigenschaft am Ende der Ausgabe.

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

Wenn das Starten Ihres Containers sehr lange dauert, aber nach einiger Zeit erfolgreich ist, sollten Sie sich zuerst die Größe des Containerimage ansehen. Da Azure Container Instances den Pullvorgang für Ihr Containerimage nach Bedarf durchführt, ist die Startdauer direkt von der Größe abhängig.

Sie können die Größe Ihres Containerimage mit der Docker CLI anzeigen:

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

Sie können die Imagegrößen klein halten, indem Sie sicherstellen, dass Ihr endgültiges Image keine Elemente enthält, die zur Laufzeit nicht erforderlich sind. Eine Möglichkeit ist die Verwendung von [mehrstufigen Builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Mit mehrstufigen Builds können Sie leicht sicherstellen, dass das endgültige Image nur die Artefakte enthält, die Sie für Ihre Anwendung benötigen – und keinen zusätzlichen Inhalt, der zur Erstellungszeit benötigt wurde.

Die andere Möglichkeit, die Auswirkungen des Pullvorgangs für das Image auf die Startdauer Ihres Containers zu reduzieren, ist das Hosten des Containerimage per Azure Container Registry in derselben Region, in der Sie Azure Container Instances verwenden möchten. Auf diese Weise wird der Netzwerkpfad verkürzt, den das Containerimage zurücklegen muss, sodass sich die Downloadzeit erheblich reduziert.

## <a name="resource-not-available-error"></a>Ressource nicht verfügbar

Aufgrund der unterschiedlichen Auslastung regionaler Ressourcen in Azure wird beim Versuch, eine Containerinstanz bereitzustellen, möglicherweise die folgende Fehlermeldung angezeigt:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Dieser Fehler gibt an, dass aufgrund starker Auslastung in der Region, in der die Bereitstellung durchgeführt werden soll, die für den Container angegebenen Ressourcen zum aktuellen Zeitpunkt nicht zugeordnet werden können. Führen Sie einen oder mehrere der folgenden Schritte aus, um das Problem zu beheben.

* Überprüfen Sie, ob Ihre Einstellungen für die Containerbereitstellung innerhalb der unter [Regionale Verfügbarkeit für Azure Container Instances](container-instances-region-availability.md) definierten Parameter liegen.
* Geben Sie niedrigere CPU- und Arbeitsspeichereinstellungen für den Container an.
* Führen Sie die Bereitstellung in einer anderen Azure-Region durch.
* Führen Sie die Bereitstellung zu einem späteren Zeitpunkt durch.
