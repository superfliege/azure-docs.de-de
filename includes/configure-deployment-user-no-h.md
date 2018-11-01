---
title: Includedatei
description: Includedatei
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b5512ed5810ebd05596dc3ca7a29957b4c595c59
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035432"
---
Erstellen Sie in Cloud Shell mit dem Befehl [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) Anmeldeinformationen für die Bereitstellung. Bei der FTP- und der lokalen Git-Bereitstellung für eine Web-App ist dieser Bereitstellungsbenutzer erforderlich. Der Benutzername und das Kennwort gelten auf der Kontoebene. _Sie unterscheiden sich von den Anmeldeinformationen Ihres Azure-Abonnements._

Ersetzen Sie im folgenden Beispiel *\<username>* und *\<password>* (einschließlich Klammern) durch einen neuen Benutzernamen und ein neues Kennwort. Der Benutzername muss in Azure eindeutig sein. Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen, Symbole. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Sie sollten eine JSON-Ausgabe erhalten, in der das Kennwort als `null` angezeigt wird. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler ` 'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden.

Sie müssen diesen Bereitstellungsbenutzer nur einmal erstellen und können ihn dann für alle Azure-Bereitstellungen verwenden.

> [!NOTE]
> Notieren Sie sich den Benutzernamen und das Kennwort. Sie benötigen sie später für die Bereitstellung der Web-App.
>
>
