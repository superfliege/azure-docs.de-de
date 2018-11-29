---
title: Überprüfen von Softwareupdates von Microsoft mit Validation-as-a-Service in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Softwareupdates von Microsoft mit Validation-as-a-Service überprüfen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 8e0009bf0fc34d3e0d22755d93d941b85db62ffd
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334464"
---
# <a name="validate-software-updates-from-microsoft"></a>Überprüfen der Softwareupdates von Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft veröffentlicht in regelmäßigen Abständen Updates für die Azure Stack-Software. Diese Updates werden Azure Stack-Partnern vor der öffentlichen Bereitstellung zur Verfügung gestellt, damit diese die Updates zusammen mit ihren Lösungen überprüfen und Feedback an Microsoft senden können.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Anwenden des monatlichen Updates

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Erstellen eines Workflows

Bei der Überprüfung von Updates wird der gleiche Workflow verwendet wie bei der **Paketvalidierung**. Gehen Sie wie unter [Erstellen eines Paketvalidierungsworkflows](azure-stack-vaas-validate-oem-package.md#create-a-package-validation-workflow) beschrieben vor.

## <a name="run-tests"></a>Ausführen von Tests

Bei der Überprüfung von Updates wird der gleiche Workflow verwendet wie bei der **Paketvalidierung**. Gehen Sie wie unter [Ausführen von Paketvalidierungstests](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests) beschrieben vor.

Bei der Überprüfung von Updates muss keine Paketsignierung angefordert werden.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen und Verwalten von Tests im VaaS-Portal](azure-stack-vaas-monitor-test.md)
