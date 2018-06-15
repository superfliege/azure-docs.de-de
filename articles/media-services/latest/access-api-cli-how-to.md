---
title: Zugriff auf die Azure Media Services-API – CLI 2.0 | Microsoft-Dokumentation
description: Führen Sie die Schritte in dieser exemplarischen Vorgehensweise aus, um auf die Azure Media Services-API zuzugreifen.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 9295c3f9dfabc8ef7749758e926df443843720a1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639774"
---
# <a name="access-azure-media-services-api-with-cli-20"></a>Zugriff auf Azure Media Services API mit CLI 2.0
 
Verwenden Sie die Authentifizierung per Azure AD-Dienstprinzipal, um eine Verbindung mit der Azure Media Services-API herzustellen. Ihre Anwendung muss ein Azure AD-Token anfordern, das folgende Parameter aufweist:

* Azure AD-Mandantenendpunkt
* Media Services-Ressourcen-URI
* Ressourcen-URI für REST Media Services
* Werte der Azure AD-Anwendung: Client-ID und Clientgeheimnis

In diesem Artikel erfahren Sie, wie Sie mithilfe von CLI 2.0 eine Azure AD-Anwendung und einen Dienstprinzipal erstellen und die Werte abrufen, die für den Zugriff auf Azure Media Services-Ressourcen erforderlich sind.

## <a name="prerequisites"></a>Voraussetzungen 

Erstellen Sie ein Azure Media Services-Konto, wie in [dieser Schnellstartanleitung](create-account-cli-quickstart.md) beschrieben.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an, und starten Sie **CloudShell**, um CLI-Befehle auszuführen, wie in den nächsten Schritten gezeigt.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, erfordert dieses Thema die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Streamen einer Datei](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Weitere Informationen

[CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
