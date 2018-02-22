---
title: "Bereitstellen von Vorlagen über das Portal in Azure Stack | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie mit dem Azure Stack-Portal Vorlagen bereitstellen.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 30c077747685ad7617148e9130926120420b098b
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Bereitstellen von Vorlagen mit dem Azure Stack-Portal

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Verwenden Sie das Portal zum Bereitstellen von Azure Resource Manager-Vorlagen im Azure Stack Development Kit.

Resource Manager-Vorlagen stellen alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereit.

1. Melden Sie sich beim Portal an, klicken Sie nacheinander auf **Neu**, **Benutzerdefiniert** und **Vorlagenbereitstellung**.
2. Klicken Sie auf **Vorlage bearbeiten**, fügen Sie Ihren JSON-Vorlagencode in das Blatt ein, und klicken Sie dann auf **Speichern**.
3. Klicken Sie auf **Parameter bearbeiten**, geben Sie Werte für die aufgeführten Parameter ein, und klicken Sie dann auf **OK**.
4. Klicken Sie auf **Abonnement**, wählen Sie das Abonnement aus, das Sie verwenden möchten, und klicken Sie dann auf **OK**.
5. Klicken Sie auf **Ressourcengruppe**, wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue, und klicken Sie dann auf **OK**.
6. Klicken Sie auf **Create**. Eine neue Kachel auf dem Dashboard verfolgt den Fortschritt Ihrer Vorlagenbereitstellung.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von Vorlagen mit PowerShell](azure-stack-deploy-template-powershell.md)

