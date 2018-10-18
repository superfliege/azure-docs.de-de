---
title: Bereitstellen von Vorlagen über das Portal in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Azure Stack-Portal Vorlagen bereitstellen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 4013b7136fd365b1d37348cfc1f0b46cce787ce2
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363122"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Bereitstellen von Vorlagen mit dem Azure Stack-Portal

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können das Portal zum Bereitstellen von Azure Resource Manager-Vorlagen (ARM) für Azure Stack verwenden.

## <a name="to-deploy-a-template"></a>So stellen Sie eine Vorlage bereit

1. Melden Sie sich am Portal an, und wählen Sie **+Ressource erstellen** und dann **Benutzerdefiniert** aus.
2. Wählen Sie **Vorlagenbereitstellung** aus.
3. Wählen Sie **Vorlage bearbeiten**, und fügen Sie Ihren JSON-Vorlagencode in das Codefenster ein. Wählen Sie **Speichern**aus.
4. Wählen Sie **Parameter bearbeiten**, geben Sie Werte für die angezeigten Parameter an, und wählen Sie dann **OK**.
5. Wählen Sie **Abonnement** aus. Wählen Sie das Abonnement, das Sie verwenden möchten, und wählen Sie dann **OK**.
6. Wählen Sie **Ressourcengruppe** aus. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. Wählen Sie dann **OK**.
7. Klicken Sie auf **Erstellen**. Eine neue Kachel auf dem Dashboard verfolgt den Fortschritt Ihrer Vorlagenbereitstellung.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bereitstellen von Vorlagen finden Sie unter:

[Bereitstellen von Vorlagen mit PowerShell](azure-stack-deploy-template-powershell.md)
