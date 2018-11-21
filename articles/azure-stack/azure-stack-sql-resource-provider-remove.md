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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: b7af23ccdd379aac9959bb9993fc1781a44e705e
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684025"
---
# <a name="remove-the-sql-resource-provider"></a>Entfernen des SQL-Ressourcenanbieters

Bevor Sie den SQL-Ressourcenanbieter entfernen, müssen Sie alle Anbieterabhängigkeiten entfernen. Außerdem benötigen Sie eine Kopie des Bereitstellungspakets, mit dem der Ressourcenanbieter installiert wurde.

  |Azure Stack-Mindestversion|SQL RP Version|
  |-----|-----|
  |Version 1808 (1.1808.0.97)|[SQL RP Version 1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
  |Version 1804 (1.0.180513.1)|[SQL RP Version 1.1.24.0](https://aka.ms/azurestacksqlrp11240)
  |     |     |

## <a name="dependency-cleanup"></a>Bereinigung von Abhängigkeiten

Es müssen verschiedene Bereinigungsaufgaben durchgeführt werden, bevor Sie das Skript „DeploySqlProvider.ps1“ zum Entfernen des Ressourcenanbieters ausführen können.

Azure Stack-Mandantenbenutzer sind für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen aller ihrer Datenbanken aus dem Ressourcenanbieter (Die Daten werden dabei nicht gelöscht.)
* Aufheben der Registrierung des Anbieternamespace

Der Azure Stack-Operator ist für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen der Hostserver aus dem MySQL-Adapter
* Löschen aller Pläne, die auf den MySQL-Adapter verweisen
* Löschen aller Kontingente, die dem MySQL-Adapter zugeordnet sind

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
