---
title: Entfernen des SQL-Ressourcenanbieters in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den SQL-Ressourcenanbieter aus Ihrer Azure Stack-Bereitstellung entfernen.
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
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: f5aa67ad0588e3f42e68056c8ffca97767975e8b
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49361480"
---
# <a name="remove-the-sql-resource-provider"></a>Entfernen des SQL-Ressourcenanbieters

Bevor Sie den SQL-Ressourcenanbieter entfernen, müssen Sie alle Anbieterabhängigkeiten entfernen. Außerdem benötigen Sie eine Kopie des Bereitstellungspakets, mit dem der Ressourcenanbieter installiert wurde.

Es müssen verschiedene Bereinigungsaufgaben durchgeführt werden, bevor Sie das Skript _DeploySqlProvider.ps1_ zum Entfernen des Ressourcenanbieters ausführen können.
Die Mandanten sind für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen aller ihrer Datenbanken aus dem Ressourcenanbieter (Die Daten werden dabei nicht gelöscht.)
* Aufheben der Registrierung des Namespace des Ressourcenanbieters

Der Administrator ist für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen der Hostserver aus dem SQL-Ressourcenanbieter
* Löschen aller Pläne, die auf den SQL-Ressourcenanbieter verweisen
* Löschen aller Kontingente, die dem SQL-Ressourcenanbieter zugeordnet sind

## <a name="to-remove-the-sql-resource-provider"></a>So entfernen Sie den SQL-Ressourcenanbieter

1. Vergewissern Sie sich, dass Sie alle vorhandenen Abhängigkeiten des SQL-Ressourcenanbieters entfernt haben.

   > [!NOTE]
   > Die Deinstallation des SQL-Ressourcenanbieters wird fortgesetzt, auch wenn abhängige Ressourcen den Ressourcenanbieter derzeit verwenden.
  
2. Rufen Sie eine Kopie der Binärdatei des SQL-Ressourcenanbieters ab, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren.

3. Öffnen Sie ein neues PowerShell-Konsolenfenster mit erhöhten Rechten, und wechseln Sie zu dem Verzeichnis, in dem Sie die Binärdateien des SQL-Ressourcenanbieters extrahiert haben.

4. Führen Sie das „DeploySqlProvider.ps1“-Skript mit den folgenden Parametern aus:

    * **Deinstallieren**. Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen.
    * **PrivilegedEndpoint**. Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts.
    * **AzureEnvironment**. Die Azure-Umgebung, die für die Bereitstellung von Azure Stack verwendet wird. Nur für Azure AD-Bereitstellungen erforderlich.
    * **CloudAdminCredential**. Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind.
    * **AzCredential**. Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack.

## <a name="next-steps"></a>Nächste Schritte

[Anbieten von App Services als PaaS](azure-stack-app-service-overview.md)
