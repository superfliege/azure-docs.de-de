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
ms.date: 05/09/2019
ms.author: juliako
ms.openlocfilehash: a27f7597ddc934b925d63507a816ac5816b682d6
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65151042"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Zugriff auf Azure Media Services API mit Azure CLI
 
Zum Herstellen einer Verbindung mit der Azure Media Services-API mithilfe der Azure AD-Dienstprinzipalauthentifizierung muss Ihre Anwendung ein Azure AD-Token anfordern, das die folgenden Parameter aufweist:

* Azure AD-Mandantenendpunkt
* Media Services-Ressourcen-URI
* Ressourcen-URI für REST Media Services
* Werte der Azure AD-Anwendung: Client-ID und Clientgeheimnis

Ausführliche Erläuterungen finden Sie unter [Zugreifen auf Media Services v3-APIs](media-services-apis-overview.md#accessing-the-azure-media-services-api).

In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure CLI eine Azure AD-Anwendung und einen Dienstprinzipal erstellen und die Werte abrufen, die für den Zugriff auf Azure Media Services-Ressourcen erforderlich sind.

## <a name="prerequisites"></a>Voraussetzungen 

[Erstellen Sie ein Media Services-Konto.](create-account-cli-how-to.md)

Merken Sie sich die Werte, die Sie für den Namen der Ressourcengruppe und des Media Services-Kontos verwendet haben.
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>Weitere Informationen

- [Skalieren der Medienverarbeitung](media-reserved-units-cli-how-to.md)
- [CLI-Beispiel: Erstellen eines Azure Media Services-Kontos](./scripts/cli-create-account.md) 
- [CLI-Beispiel: Zurücksetzen der Kontoanmeldeinformationen](./scripts/cli-reset-account-credentials.md)
- [CLI-Beispiel: Erstellen eines Medienobjekts](./scripts/cli-create-asset.md)
- [CLI-Beispiel: Hochladen einer lokalen Datei in einen Container](./scripts/cli-upload-file-asset.md)
- [CLI-Beispiel: Erstellen einer Transformation](./scripts/cli-create-transform.md)
- [CLI-Beispiel: Erstellen und Übermitteln eines Auftrags](./scripts/cli-create-jobs.md)
- [CLI-Beispiel: Erstellen eines Azure Event Grid-Abonnements](./scripts/cli-create-event-grid.md)
- [CLI-Beispiel: Veröffentlichen eines Medienobjekts](./scripts/cli-publish-asset.md)
- [Erstellen von Filtern mit der CLI](filters-dynamic-manifest-cli-howto.md)
- [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>Nächste Schritte

Der Streamingendpunkt, von dem aus Sie die Inhalte streamen möchten, muss sich im Status „Wird ausgeführt“ befinden. Der folgende CLI-Befehl startet den standardmäßigen Streamingendpunkt:

`az ams streaming-endpoint start -n default -a <amsaccount> -g <amsResourceGroup>`

