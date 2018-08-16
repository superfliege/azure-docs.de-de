---
title: Includedatei
description: Includedatei
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 875dd9d768b5f40f2d4ed3fad5b14a6cd6f6f6ba
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129306"
---
### <a name="sign-in-to-azure-cli"></a>Anmelden bei der Azure-Befehlszeilenschnittstelle
Melden Sie sich bei Azure an. Geben Sie in einem Terminalfenster den folgenden Befehl ein:

```cmd
az login
```

> [!Note]
> Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Falls Sie über mehrere Azure-Abonnements verfügen...
Führen Sie zum Anzeigen Ihrer Abonnements Folgendes aus: 

```cmd
az account list
```
Suchen Sie nach dem Abonnement, dessen JSON-Ausgabe `isDefault: true` enthält.
Handelt es sich dabei nicht um das Abonnement, das Sie verwenden möchten, können Sie das Standardabonnement wie folgt ändern:

```cmd
az account set --subscription <subscription ID>
```
