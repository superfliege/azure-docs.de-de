---
title: Erstellen eines Azure Media Services-Kontos mit der Azure CLI | Microsoft-Dokumentation
description: Führen Sie die Schritte dieser Schnellstartanleitung aus, um ein Azure Media Services-Konto zu erstellen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 988b56569c2537f9f0c74b1bcc2bf5e2a9400a80
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378804"
---
# <a name="create-an-azure-media-services-account"></a>Erstellen eines Azure Media Services-Kontos

Um mit dem Verschlüsseln, Codieren, Analysieren, Verwalten und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Beim Erstellen eines Media Services-Kontos erstellen Sie auch ein zugehöriges Speicherkonto (oder verwenden ein vorhandenes) in der gleichen geografischen Region wie das Media Services-Konto.

In diesem Artikel werden die Schritte zum Erstellen eines neuen Azure Media Services-Kontos mithilfe der Azure CLI beschrieben.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an, und starten Sie **CloudShell**, um CLI-Befehle auszuführen, wie in den nächsten Schritten gezeigt.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie die Befehlszeilenschnittelle lokal installieren und verwenden möchten, erfordert dieses Thema mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Einrichten des Azure-Abonnements

Geben Sie im folgenden Befehle die ID des Azure-Abonnements an, das Sie für das Media Services-Konto verwenden möchten. Sie können eine Liste der Abonnements anzeigen, auf die Sie Zugriff haben, indem Sie zu [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) navigieren.

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Streamen einer Datei](stream-files-dotnet-quickstart.md)
