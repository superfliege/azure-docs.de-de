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
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 7fcc7d5a1d87fe93d32772dbbb84f1d3c91d5631
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648784"
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
