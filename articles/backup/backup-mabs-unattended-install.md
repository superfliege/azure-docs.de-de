---
title: Automatische Installation von Azure Backup Server V2
description: Verwenden Sie ein PowerShell-Skript, um Azure Backup Server v2 automatisch zu installieren. Diese Art der Installation wird auch als unbeaufsichtigte Installation bezeichnet.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/30/2017
ms.author: markgal
ms.openlocfilehash: 126c1971d83a8874c096caf407231fb6dee2ff59
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606408"
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Ausführen einer unbeaufsichtigten Installation von Azure Backup Server v2

Erfahren Sie, wie Sie eine unbeaufsichtigte Installation von Azure Backup Server v2 ausführen 

Diese Schritte gelten nicht, wenn Sie Azure Backup Server v1 installieren.

## <a name="install-backup-server-v2"></a>Installieren von Backup Server v2

1. Erstellen Sie auf dem Server, der Azure Backup Server v2 hostet, eine Textdatei. (Sie können die Datei in Editor oder einem anderen Texteditor erstellen.) Speichern Sie die Datei als „MABSSetup.ini“. 

2. Fügen Sie den folgenden Code in die Datei „MABSSetup.ini“ ein. Ersetzen Sie den Text in Klammern (\< \>) durch Werte aus Ihrer Umgebung. Der folgende Text ist ein Beispiel:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Speichern Sie die Datei . Geben Sie dann an einer Eingabeaufforderung mit erhöhten Rechten auf dem Installationsserver diesen Befehl ein:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Sie können diese Flags für die Installation verwenden:</br>
**/f:** Pfad der INI-Datei</br>
**/l:** Protokollpfad</br>
**/i:** Installationspfad</br>
**/x:** Deinstallationspfad</br>

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie nach der Installation von Backup Server Ihren Server vorbereiten oder mit dem Schutz einer Workload beginnen.

- [Vorbereiten von Backup Server-Workloads](backup-azure-microsoft-azure-backup.md)
- [Sichern eines VMware-Servers mit Backup Server](backup-azure-backup-server-vmware.md)
- [Sichern von SQL Server mit Backup Server](backup-azure-sql-mabs.md)
- [Hinzufügen von Modern Backup Storage zu Backup Server](backup-mabs-add-storage.md)
