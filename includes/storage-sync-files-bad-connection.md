---
title: Includedatei
description: Includedatei
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 754562487f0fe9f825107445a3059cc15a1faa26
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146500"
---
Dieser Fehler kann auftreten, wenn der Zugriff auf den Azure File Sync-Dienst vom Server aus nicht verfügbar ist. Sie können diesen Fehler in folgenden Schritte beheben:

1. Stellen Sie sicher, dass der Windows-Dienst `FileSyncSvc.exe` nicht von der Firewall blockiert wird.
2. Stellen Sie sicher, dass Port 443 für ausgehende Verbindungen mit dem Azure File Sync-Dienst geöffnet ist. Hierfür können Sie das `Test-NetConnection`-Cmdlet verwenden. Die URL für den `<azure-file-sync-endpoint>`-Platzhalter unten finden Sie im Dokument [Proxy- und Firewall-Einstellungen von Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall). 

    ```PowerShell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Stellen Sie sicher, dass die Proxykonfiguration wie erwartet festgelegt ist. Verwenden Sie dazu das `Get-StorageSyncProxyConfiguration`-Cmdlet. Weitere Informationen zur Proxykonfiguration für Azure File Sync finden Sie unter [Proxy- und Firewall-Einstellungen von Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```PowerShell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
    
4. Wenden Sie sich an Ihren Netzwerkadministrator, um weitere Unterstützung zur Problembehandlung bei Netzwerkverbindungen zu erhalten.