---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 02/11/2019
ms.openlocfilehash: 480cec39e42e075ad83ad3e32b88c638094db64f
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741599"
---
> [!TIP]
> Mithilfe des Befehls [docker images](https://docs.docker.com/engine/reference/commandline/images/) können Sie Ihre heruntergeladenen Containerimages auflisten. Mit dem folgenden Befehl werden beispielsweise die ID, das Repository und das Tag jedes heruntergeladenen Containerimages in Form einer Tabelle aufgelistet:
>
>  ```
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY              TAG
>  ebbee78a6baa       <container-name>         latest
>  ``` 