---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: ae88a6132c79d9d9f930f6445e53b0f0452c9f2a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262606"
---
## <a name="test"></a>Testen der Funktion

Verwenden Sie cURL, um die bereitgestellte Funktion auf einem Mac oder Linux-Computer oder mithilfe von Bash unter Windows zu testen. Führen Sie den folgenden cURL-Befehl aus, und ersetzen Sie dabei den Platzhalter `<app_name>` mit dem Namen der Funktionen-App. Fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an.

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Die Funktionsantwort wird in einem Browser angezeigt.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Wenn cURL in der Befehlszeile nicht zur Verfügung steht, geben Sie dieselbe URL in die Adresse des Webbrowsers ein. Ersetzen Sie erneut den Platzhalter `<app_name>` durch den Namen der Funktionen-App, fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an, und führen Sie die Anforderung aus.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Die Funktionsantwort wird in einem Browser angezeigt.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
