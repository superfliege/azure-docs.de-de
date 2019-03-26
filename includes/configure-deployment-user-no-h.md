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
ms.openlocfilehash: e8888a0505a3a38d2844f82c0f7fff255d05353d
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261528"
---
Konfigurieren Sie in Azure Cloud Shell mit dem Befehl [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) Anmeldeinformationen für die Bereitstellung. Bei der FTP- und der lokalen Git-Bereitstellung für eine Web-App ist dieser Bereitstellungsbenutzer erforderlich. Der Benutzername und das Kennwort gelten auf Kontoebene. _Sie unterscheiden sich von den Anmeldeinformationen Ihres Azure-Abonnements._

Ersetzen Sie im folgenden Beispiel *\<username>* und *\<password>* (einschließlich der spitzen Klammern) durch einen neuen Benutzernamen und ein neues Kennwort. Der Benutzername muss in Azure eindeutig sein. Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen und Symbole. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Sie erhalten eine JSON-Ausgabe, in der das Kennwort als `null` angezeigt wird. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler ` 'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden.

Sie konfigurieren diesen Bereitstellungsbenutzer nur einmal. Sie können ihn für alle Azure-Bereitstellungen verwenden.

> [!NOTE]
> Notieren Sie sich den Benutzernamen und das Kennwort. Sie benötigen sie später für die Bereitstellung der Web-App.
>
>
