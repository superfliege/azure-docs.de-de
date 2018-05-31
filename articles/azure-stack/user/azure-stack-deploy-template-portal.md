---
title: Bereitstellen von Vorlagen über das Portal in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Azure Stack-Portal Vorlagen bereitstellen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 278f15271d3a5443102f5e387d3db1adb53fe7db
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34357383"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Bereitstellen von Vorlagen mit dem Azure Stack-Portal

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können das Portal zum Bereitstellen von Azure Resource Manager-Vorlagen (ARM) für Azure Stack verwenden.

So stellen Sie eine Vorlage bereit

1. Melden Sie sich beim Portal an, und wählen Sie **Neu** und dann **Benutzerdefiniert** aus.
2. Wählen Sie **Vorlagenbereitstellung** aus.
3. Wählen Sie **Vorlage bearbeiten**, und fügen Sie Ihren JSON-Vorlagencode in das Codefenster ein. Wählen Sie **Speichern**aus.
4. Wählen Sie **Parameter bearbeiten**, geben Sie Werte für die angezeigten Parameter an, und wählen Sie dann **OK**.
5. Wählen Sie **Abonnement** aus. Wählen Sie das Abonnement, das Sie verwenden möchten, und wählen Sie dann **OK**.
6. Wählen Sie **Ressourcengruppe** aus. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. Wählen Sie dann **OK**.
7. Klicken Sie auf **Erstellen**. Eine neue Kachel auf dem Dashboard verfolgt den Fortschritt Ihrer Vorlagenbereitstellung.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit PowerShell](azure-stack-deploy-template-powershell.md)
