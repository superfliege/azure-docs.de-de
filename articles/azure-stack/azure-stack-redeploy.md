---
title: Erneutes Bereitstellen von Azure Stack | Microsoft-Dokumentation
description: Erneutes Bereitstellen von Azure Stack
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: erikje
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 891cde9b16bbbb51729129b6ad7a0f3794307baa
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="redeploy-azure-stack"></a>Erneutes Bereitstellen von Azure Stack
Um Azure Stack erneut bereitzustellen, müssen Sie wie unten beschrieben noch einmal von Vorne anfangen.

## <a name="steps-to-redeploy-azure-stack"></a>Schritte zum erneuten Bereitstellen von Azure Stack
1. Öffnen Sie im Host des Development Kits eine PowerShell-Konsole mit erhöhten Rechten. Navigieren Sie dann zum Skript „asdk-installer.ps1“, führen Sie dieses aus, und klicken Sie anschließend auf **Neustart**.
2. Wählen Sie das Basisbetriebssystem aus (nicht **Azure Stack**), und klicken Sie dann auf **Weiter**.
3. Nachdem der Host des Development Kits neu gestartet wurde, löschen Sie die Datei „CloudBuilder.vhdx“, die in der vorherigen Bereitstellung verwendet wurde.
4. [Stellen Sie das Development Kit bereit](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Nächste Schritte
[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)


