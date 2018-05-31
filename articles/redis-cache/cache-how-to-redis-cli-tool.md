---
title: Verwenden von „redis-cli.exe“ mit Azure Redis Cache | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie „redis-cli.exe“ mit Azure Redis Cache verwenden.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: wesmc
ms.openlocfilehash: facc3e2079c8f5e19266f5379762d71754a6ed84
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182280"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-redis-cache"></a>Verwenden des Redis-Befehlszeilentools mit Azure Redis Cache

*redis-cli.exe* ist ein gängiges Befehlszeilentool für die Interaktion mit einer Redis Cache-Instanz als Client. Dieses Tool ist auch für die Verwendung mit Azure Redis Cache verfügbar.

Um das Tool auf Windows-Plattformen zu nutzen, laden Sie die [Redis-Befehlszeilentools für Windows](https://github.com/MSOpenTech/redis/releases/) herunter. 

Wenn Sie das Befehlszeilentool auf einer anderen Plattform ausführen möchten, laden Sie Redis Cache von [http://redis.io/download](https://redis.io/download) herunter.

## <a name="gather-cache-access-information"></a>Sammeln von Cachezugriffsinformationen

Sie können die benötigten Informationen zum Zugriff auf den Cache mithilfe von drei Methoden sammeln:

1. Über Azure CLI mit [az redis list-keys](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Über Azure PowerShell mit [Get-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-4.4.1)
3. Über das Azure-Portal.

In diesem Abschnitt rufen Sie die Schlüssel aus dem Azure-Portal ab.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Aktivieren des Zugriffs für „redis-cli.exe“

Mit Azure Redis Cache ist nur der SSL-Port (6380) standardmäßig aktiviert. Das `redis-cli.exe`-Befehlszeilentool unterstützt SSL nicht. Sie können zu seiner Verwendung zwischen zwei Konfigurationsoptionen wählen:

1. [Nicht-SSL-Port aktivieren (6379)](cache-configure.md#access-ports) - **Diese Konfiguration wird nicht empfohlen**, da in dieser Konfiguration die Zugriffsschlüssel über TCP in Klartext gesendet werden. Diese Änderung kann den Zugriff auf Ihren Cache beeinträchtigen. Das einzige Szenario, in dem Sie diese Konfiguration berücksichtigen könnten, ist der Zugriff auf einen Testcache.

2. Laden Sie [stunnel](https://www.stunnel.org/downloads.html) herunter, und installieren Sie die Anwendung.

    Führen Sie **stunnel GUI Start** zum Starten des Servers aus.

    Klicken Sie mit der rechten Maustaste auf das Taskleistensymbol für den stunnel-Server, und klicken Sie auf **Show Log Window (Protokollfenster anzeigen)**.

    Klicken Sie im stunnel-Menü „Log Window“ (Protokollfenster) auf **Configuration (Konfiguration)** > **Edit Configuration (Konfiguration bearbeiten)**, um die aktuelle Konfigurationsdatei zu öffnen.

    Fügen Sie den folgenden Eintrag für *redis-cli.exe* im Abschnitt **Service definitions (Dienstdefinitionen)** hinzu. Fügen Sie Ihren tatsächlichen Cachename anstelle von `yourcachename` ein. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Speichern und schließen Sie die Konfigurationsdatei. 
  
    Klicken Sie im stunnel-Menü „Log Window“ (Protokollfenster) auf **Configuration (Konfiguration)** > **Reload Configuration (Konfiguration erneut laden)**.


## <a name="connect-using-the-redis-command-line-tool"></a>Stellen Sie mit dem Redis-Befehlszeilentool eine Verbindung her.

Führen Sie bei Verwendung von stunnel *redis-cli.exe* aus, und übergeben Sie nur Ihren *Port* und *Zugriffsschlüssel* (primär oder sekundär) für die Herstellung der Verbindung mit dem Cache.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![stunnel mit „redis-cli.exe“](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Bei Verwendung eines Testcaches mit dem **unsicheren** Nicht-SSL-Port führen Sie `redis-cli.exe` aus, und übergeben Sie Ihren *Hostnamen*, *Port* und *Zugriffsschlüssel* (primär oder sekundär) für die Herstellung der Verbindung mit dem Testcache.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![stunnel mit „redis-cli.exe“](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung der [Redis-Konsole](cache-configure.md#redis-console) zur Befehlsausgabe.

