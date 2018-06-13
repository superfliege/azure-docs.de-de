---
title: 'Azure CLI-Skriptbeispiel: Erstellen und Übermitteln eines Auftrags | Microsoft-Dokumentation'
description: Das Azure CLI-Skript in diesem Thema zeigt, wie Sie einen Auftrag mithilfe einer HTTPS-URL an eine einfache Codierungstransformation übermitteln.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/11/2018
ms.author: juliako
ms.openlocfilehash: 4c11a959f23369aba68175ac3968cdd0deb804e8
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161328"
---
# <a name="cli-example-create-and-submit-a-job"></a>CLI-Beispiel: Erstellen und Übermitteln eines Auftrags

Das Azure CLI-Skript in diesem Artikel zeigt, wie Sie einen Auftrag erstellen und mithilfe einer HTTPS-URL an eine einfache Codierungstransformation übermitteln.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-jobs/Create-Jobs.sh "Create and submit jobs")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
