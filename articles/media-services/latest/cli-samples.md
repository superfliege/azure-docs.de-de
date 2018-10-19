---
title: 'Azure CLI-Beispiele: Azure Media Services | Microsoft-Dokumentation'
description: Azure CLI-Beispiele für den Azure Media Services-Dienst
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 04/15/2018
ms.author: juliako
ms.openlocfilehash: 3328403f5366f168f979a14951da938f26e1aee9
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093858"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure CLI-Beispiele für Azure Media Services

Die folgende Tabelle enthält Links zu Azure CLI-Beispielen für Azure Media Services.

|  |  |
|---|---|
|**Konto**||
| [Erstellen eines Media Services-Kontos](./scripts/cli-create-account.md) | Erstellt ein Azure Media Services-Konto. Erstellt auch einen Dienstprinzipal, der zum Zugriff auf APIs verwendet werden kann, um das Konto programmgesteuert zu verwalten. |
| [Zurücksetzen der Kontoanmeldeinformationen](./scripts/cli-reset-account-credentials.md)|Setzt Ihre Kontoanmeldeinformationen zurück und stellt die Einstellungen von „app.config“ wieder her.|
|**Medienobjekte**||
| [Erstellen von Medienobjekten](./scripts/cli-create-asset.md)|Erstellt ein Media Services-Medienobjekt, in das Inhalt hochgeladen werden kann.|
| [Hochladen einer Datei](./scripts/cli-upload-file-asset.md)|Lädt eine lokale Datei in einen Speichercontainer hoch.|
| [Veröffentlichen eines Medienobjekts](./scripts/cli-publish-asset.md)| Erstellt einen Streaminglocator und erhält Streaming-URLs zurück. |
| **Transformationen** und **Aufträge**||
| [Erstellen von Transformationen](./scripts/cli-create-transform.md)|Zeigt, wie Transformationen erstellt werden. Transformationen beschreiben einen einfachen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien (der häufig als „Rezept“ bezeichnet wird).<br/> Sie müssen immer überprüfen, ob eine Transformation mit dem gewünschten Namen und „Rezept“ bereits vorhanden ist. Wenn dies der Fall ist, verwenden Sie diese erneut. |
| [Erstellen von Aufträgen](./scripts/cli-create-jobs.md)|Übermittelt einen Auftrag mithilfe einer HTTPS-URL an eine einfache Codierungstransformation.|
| [Erstellen von EventGrid](./scripts/cli-create-event-grid.md)|Erstellt ein Event Grid-Abonnement für Änderungen des Auftragsstatus auf Kontoebene.|

## <a name="see-also"></a>Weitere Informationen

[Azure-CLI](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
