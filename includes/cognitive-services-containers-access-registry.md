---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: 27c6b59a458fb79b86d7064d710a01593a1745dc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66125130"
---
## <a name="use-docker-cli-to-authenticate-private-container-registry"></a>Authentifizieren einer privaten Containerregistrierung mit der Docker-Befehlszeilenschnittstelle

Es gibt mehrere Möglichkeiten, sich bei der privaten Containerregistrierung für Cognitive Services-Container zu authentifizieren, aber die empfohlene Methode über die Befehlszeile ist die Verwendung der [Docker-CLI](https://docs.docker.com/engine/reference/commandline/cli/).

Verwenden Sie den Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/) wie im folgenden Beispiel gezeigt, um sich bei der privaten Containerregistrierung für Cognitive Services-Container (`containerpreview.azurecr.io`) anzumelden. Ersetzen Sie *\<username\>* durch den Benutzernamen und *\<password\>* durch das Kennwort. Diese Anmeldeinformationen wurden Ihnen vom Azure Cognitive Services-Team bereitgestellt.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Wenn Sie Ihre Anmeldeinformationen in einer Textdatei gesichert haben, können Sie den Inhalt dieser Textdatei über den Befehl `cat` mit dem Befehl `docker login` verketten, wie im folgenden Beispiel gezeigt. Ersetzen Sie *\<passwordFile\>* durch den Pfad und den Namen der Textdatei mit dem Kennwort und *\<username\>* durch den Benutzernamen, der in Ihren Anmeldeinformationen angegeben wurde.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```
