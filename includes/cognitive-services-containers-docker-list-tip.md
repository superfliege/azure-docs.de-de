---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: e7960bd288e87c4e3c6b395322a5ab92f3040d88
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302280"
---
> [!TIP]
> Mithilfe des Befehls [docker images](https://docs.docker.com/engine/reference/commandline/images/) können Sie Ihre heruntergeladenen Containerimages auflisten. Mit dem folgenden Befehl werden beispielsweise die ID, das Repository und das Tag jedes heruntergeladenen Containerimages in Form einer Tabelle aufgelistet:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY                                                                TAG
>  ebbee78a6baa        mcr.microsoft.com/azure-cognitive-services/luis                           latest
>  ``` 