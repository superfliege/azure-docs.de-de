---
title: Includedatei
description: Includedatei
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/08/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: ac2cf4d688b1bdc54ed2d7341f0e195d3b2fe42d
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236468"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Was geschieht während der Bereitstellung mit meiner App?

Alle offiziell unterstützten Bereitstellungsmethoden haben eines gemeinsam: Sie nehmen Änderungen an den Dateien im Ordner `/site/home/wwwroot` Ihrer App vor. Dies sind dieselben Dateien, die in der Produktionsumgebung ausgeführt werden. Aus diesem Grund kann die Bereitstellung aufgrund gesperrter Dateien zu einem Fehler führen, oder die App zeigt während der Bereitstellung in der Produktion ein unerwartetes Verhalten, da nicht alle Dateien gleichzeitig aktualisiert werden. Es gibt mehrere Möglichkeiten, wie Sie diese Probleme umgehen können:

- Beenden Sie die App, oder aktivieren Sie den Offlinemodus für die App während der Bereitstellung. Weitere Informationen finden Sie unter [Dealing with locked files during deployment](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment) (Umgang mit gesperrten Dateien während der Bereitstellung).
- Stellen Sie einen [Stagingslot](../articles/app-service/web-sites-staged-publishing.md) bereit, bei dem [Automatisch tauschen](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) aktiviert ist. 
- Verwenden Sie stattdessen [Run-From-Zip](https://github.com/Azure/app-service-announcements/issues/84).
