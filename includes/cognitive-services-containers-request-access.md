---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: d5a6692aceedb6dc7fad7a434a433882cdbea015
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984890"
---
Sie müssen zuerst das [Formular zum Anfordern von Cognitive Services-Containern für maschinelles Sehen](https://aka.ms/VisionContainersPreview) ausfüllen und senden, um Zugriff auf den Container anzufordern. Im Formular müssen Sie Informationen über Sie selbst, Ihr Unternehmen und das Benutzerszenario eintragen, für das Sie den Container verwenden möchten. Nach der Übermittlung überprüft das Azure Cognitive Services-Team das Formular, um sicherzustellen, dass Sie die Kriterien für den Zugriff auf die private Containerregistrierung erfüllen.

> [!IMPORTANT]
> Sie müssen im Formular eine E-Mail-Adresse verwenden, die entweder einem Microsoft-Konto (MSA) oder einem Azure Active Directory-Konto (Azure AD) zugeordnet ist.

Wenn Ihre Anforderung genehmigt wurde, erhalten Sie eine E-Mail mit Anweisungen zum Abrufen Ihrer Anmeldeinformationen und zum Zugreifen auf die private Containerregistrierung.

## <a name="log-in-to-the-private-container-registry"></a>Anmelden bei der privaten Containerregistrierung

Es gibt mehrere Möglichkeiten, sich bei der privaten Containerregistrierung für Cognitive Services-Container zu authentifizieren, aber die empfohlene Methode über die Befehlszeile ist die Verwendung der [Docker-CLI](https://docs.docker.com/engine/reference/commandline/cli/).

Verwenden Sie den Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/) wie im folgenden Beispiel gezeigt, um sich bei der privaten Containerregistrierung für Cognitive Services-Container (`containerpreview.azurecr.io`) anzumelden. Ersetzen Sie *\<username\>* durch den Benutzernamen und *\<password\>* durch das Kennwort. Diese Anmeldeinformationen wurden Ihnen vom Azure Cognitive Services-Team bereitgestellt.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Wenn Sie Ihre Anmeldeinformationen in einer Textdatei gesichert haben, können Sie den Inhalt dieser Textdatei über den Befehl `cat` mit dem Befehl `docker login` verketten, wie im folgenden Beispiel gezeigt. Ersetzen Sie *\<passwordFile\>* durch den Pfad und den Namen der Textdatei mit dem Kennwort und *\<username\>* durch den Benutzernamen, der in Ihren Anmeldeinformationen angegeben wurde.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

