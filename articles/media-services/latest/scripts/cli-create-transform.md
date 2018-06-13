---
title: 'Azure CLI-Skriptbeispiel: Erstellen einer Transformation | Microsoft-Dokumentation'
description: Verwenden Sie das Azure CLI-Skript, um eine Transformation zu erstellen.
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
ms.openlocfilehash: 8cc6b779729bf0c596a7c1b7318eed6f47dffb1d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161345"
---
# <a name="cli-example-create-a-transform"></a>CLI-Beispiel: Erstellen einer Transformation

Das Azure CLI-Skript in diesem Artikel zeigt, wie eine Transformation erstellt wird. Transformationen beschreiben einen einfachen Workflow von Aufgaben zur Verarbeitung Ihrer Video- oder Audiodateien (der häufig als „Rezept“ bezeichnet wird). Sie müssen immer überprüfen, ob eine Transformation mit dem gewünschten Namen und „Rezept“ bereits vorhanden ist. Wenn dies der Fall ist, sollten Sie diese erneut verwenden.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Azure CLI-Version 2.0.20 verwenden. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. 

## <a name="example-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-transform/Create-Transform.sh "Create a transform")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele finden Sie unter [Azure CLI-Beispiele](../cli-samples.md).
