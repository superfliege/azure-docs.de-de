---
title: Erneutes Bereitstellen von Azure Stack | Microsoft-Dokumentation
description: Erneutes Bereitstellen von Azure Stack
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 0dec5ea70376ff1c8cf488689f1a66190256f6ff
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="redeploy-azure-stack"></a>Erneutes Bereitstellen von Azure Stack
Wenn ein Fehler beim Bereitstellen von Azure Stack auftritt, können Sie das Setup über folgenden PowerShell-Befehl erneut ausführen: `.\InstallAzureStackpoc.ps1 -rerun`. Mit diesem Befehl wird das Azure Stack-Setup an dem Punkt neu gestartet, an dem zuvor der Fehler aufgetreten ist, sodass das Setup nicht von vorne begonnen werden muss. Tritt jedoch der gleichen Setupfehler wieder auf, kann eine komplett neue Bereitstellung erforderlich sein, um das Problem zu beheben. 

Um Azure Stack erneut bereitzustellen, müssen Sie wie unten beschrieben noch einmal von Vorne anfangen.

## <a name="steps-to-redeploy-azure-stack"></a>Schritte zum erneuten Bereitstellen von Azure Stack
1. Öffnen Sie im Host des Development Kits eine PowerShell-Konsole mit erhöhten Rechten. Navigieren Sie dann zum Skript „asdk-installer.ps1“, führen Sie dieses aus, und klicken Sie anschließend auf **Neustart**.
2. Wählen Sie das Basisbetriebssystem aus (nicht **Azure Stack**), und klicken Sie dann auf **Weiter**.
3. Nachdem der Host des Development Kits neu gestartet wurde, löschen Sie die Datei „CloudBuilder.vhdx“, die in der vorherigen Bereitstellung verwendet wurde.
4. [Stellen Sie das Development Kit bereit](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Nächste Schritte
[Herstellen einer Verbindung mit Azure Stack](azure-stack-connect-azure-stack.md)

