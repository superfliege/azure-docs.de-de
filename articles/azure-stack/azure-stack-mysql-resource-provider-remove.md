---
title: Entfernen des MySQL-Ressourcenanbieters in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Ressourcenanbieter aus Ihrer Azure Stack-Bereitstellung entfernen können.
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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 7d3b0e179972464a1ed857c576ca8a7c8fc2e162
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686795"
---
# <a name="remove-the-mysql-resource-provider"></a>Entfernen des MySQL-Ressourcenanbieters

Bevor Sie den MySQL-Ressourcenanbieter entfernen, müssen Sie alle Anbieterabhängigkeiten entfernen. Außerdem benötigen Sie eine Kopie des Bereitstellungspakets, mit dem der Ressourcenanbieter installiert wurde.

  |Azure Stack-Mindestversion|MySQL RP Version|
  |-----|-----|
  |Version 1808 (1.1808.0.97)|[MySQL RP Version 1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
  |Version 1804 (1.0.180513.1)|[MySQL RP Version 1.1.24.0](https://aka.ms/azurestackmysqlrp11240)
  |     |     |

## <a name="dependency-cleanup"></a>Bereinigung von Abhängigkeiten

Es müssen verschiedene Bereinigungsaufgaben durchgeführt werden, bevor Sie das Skript „DeployMySqlProvider.ps1 script“ zum Entfernen des Ressourcenanbieters ausführen können.

Azure Stack-Mandantenbenutzer sind für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen aller ihrer Datenbanken aus dem Ressourcenanbieter (Die Daten werden dabei nicht gelöscht.)
* Aufheben der Registrierung des Anbieternamespace

Der Azure Stack-Bediener ist für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen der Hostserver aus dem MySQL-Adapter
* Löschen aller Pläne, die auf den MySQL-Adapter verweisen
* Löschen aller Kontingente, die dem MySQL-Adapter zugeordnet sind

## <a name="to-remove-the-mysql-resource-provider"></a>So entfernen Sie den MySQL-Ressourcenanbieter

1. Vergewissern Sie sich, dass Sie alle vorhandenen Abhängigkeiten des MySQL-Ressourcenanbieters entfernt haben.

   >[!NOTE]
   >Die Deinstallation des MySQL-Ressourcenanbieters wird fortgesetzt, auch wenn abhängige Ressourcen den Ressourcenanbieter derzeit verwenden.
  
2. Rufen Sie eine Kopie der Binärdatei des MySQL-Ressourcenanbieters ab, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren.
3. Rufen Sie eine Kopie der Binärdatei des SQL-Ressourcenanbieters ab, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren.
4. Öffnen Sie ein neues PowerShell-Konsolenfenster mit erhöhten Rechten, und wechseln Sie zu dem Verzeichnis, in dem Sie die Binärdateien des MySQL-Ressourcenanbieters extrahiert haben.
5. Führen Sie das Skript „DeployMySqlProvider.ps1“ mit den folgenden Parametern aus:
    - **Deinstallieren**. Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen.
    - **PrivilegedEndpoint**. Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts.
    - **AzureEnvironment**. Die Azure-Umgebung, die für die Bereitstellung von Azure Stack verwendet wird. Nur für Azure AD-Bereitstellungen erforderlich.
    - **CloudAdminCredential**. Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind.
    - **DirectoryTenantID**
    - **AzCredential**. Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack.

## <a name="next-steps"></a>Nächste Schritte

[Anbieten von App Services als PaaS](azure-stack-app-service-overview.md)
