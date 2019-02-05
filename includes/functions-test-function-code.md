---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 914c006daf49e22ebec870a549bfdbc63f882647
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55148042"
---
## <a name="test"></a>Testen der Funktion

Verwenden Sie cURL, um die bereitgestellte Funktion auf einem Mac oder Linux-Computer oder mithilfe von PowerShell unter Windows zu testen. Führen Sie den folgenden cURL-Befehl aus, und ersetzen Sie dabei den Platzhalter `<app_name>` mit dem Namen der Funktionen-App. Fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an.

```powershell
Invoke-WebRequest -Uri "https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>"
```

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Die Funktionsantwort wird in einem Browser angezeigt.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Wenn `cURL` oder `Invoke-WebRequest` in der Befehlszeile nicht zur Verfügung steht, geben Sie dieselbe URL in die Adressleiste des Webbrowsers ein. Ersetzen Sie erneut den Platzhalter `<app_name>` durch den Namen der Funktionen-App, fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an, und führen Sie die Anforderung aus.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Die Funktionsantwort wird in einem Browser angezeigt.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
