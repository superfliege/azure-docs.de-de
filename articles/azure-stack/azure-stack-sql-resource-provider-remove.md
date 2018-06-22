---
title: Entfernen des SQL-Ressourcenanbieters in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den SQL-Ressourcenanbieter aus Ihrer Azure Stack-Bereitstellung entfernen können.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 9f90201cad0f74923460c2f25eff4de98dc6690a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294779"
---
# <a name="removing-the-mysql-resource-provider"></a>Entfernen des MySQL-Ressourcenanbieters  
Vor dem Entfernen des SQL-Ressourcenanbieters müssen zunächst unbedingt alle Abhängigkeiten entfernt werden.

## <a name="remove-the-mysql-resource-provider"></a>Entfernen des MySQL-Ressourcenanbieters 

1. Vergewissern Sie sich, dass Sie alle vorhandenen Abhängigkeiten des SQL-Ressourcenanbieters entfernt haben.

  > [!NOTE]
  > Die Deinstallation des SQL-Ressourcenanbieters wird fortgesetzt, auch wenn abhängige Ressourcen den Ressourcenanbieter derzeit verwenden. 
  
2. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des SQL-Ressourcenanbieteradapters heruntergeladen haben.
3. Führen Sie das Bereitstellungsskript mit den folgenden Parametern erneut aus:
    - Verwenden des Parameters „-Uninstall“
    - Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts.
    - Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind.
    - Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack.

## <a name="next-steps"></a>Nächste Schritte
[Anbieten von App Services als PaaS](azure-stack-app-service-overview.md)
