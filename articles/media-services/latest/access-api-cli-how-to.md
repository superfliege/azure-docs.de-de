---
title: Zugriff auf die Azure Media Services-API – Azure CLI | Microsoft-Dokumentation
description: Führen Sie die Schritte in dieser exemplarischen Vorgehensweise aus, um auf die Azure Media Services-API zuzugreifen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 54a92e56df21b59430ed12f191a9cf7a918e14c9
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612898"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Zugriff auf Azure Media Services API mit Azure CLI
 
Verwenden Sie die Authentifizierung per Azure AD-Dienstprinzipal, um eine Verbindung mit der Azure Media Services-API herzustellen. Ihre Anwendung muss ein Azure AD-Token anfordern, das folgende Parameter aufweist:

* Azure AD-Mandantenendpunkt
* Media Services-Ressourcen-URI
* Ressourcen-URI für REST Media Services
* Werte der Azure AD-Anwendung: Client-ID und Clientgeheimnis

In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure CLI eine Azure AD-Anwendung und einen Dienstprinzipal erstellen und die Werte abrufen, die für den Zugriff auf Azure Media Services-Ressourcen erforderlich sind.

## <a name="prerequisites"></a>Voraussetzungen 

- Installieren und verwenden Sie die CLI lokal. Dieser Artikel erfordert mindestens Version 2.0 der Azure CLI. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

    Derzeit funktionieren nicht alle Befehle der [CLI von Media Services v3](https://aka.ms/ams-v3-cli-ref) in Azure Cloud Shell. Es wird empfohlen, die CLI lokal zu verwenden.

- [Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

    Merken Sie sich die Werte, die Sie für den Namen der Ressourcengruppe und des Media Services-Kontos verwendet haben.

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Nächste Schritte

[Streamen einer Datei](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Weitere Informationen

[Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
