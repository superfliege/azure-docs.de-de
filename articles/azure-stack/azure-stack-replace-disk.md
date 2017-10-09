---
title: "Ersetzen eines physischen Datenträgers in Azure Stack | Microsoft-Dokumentation"
description: "Skizziert das Ersetzen eines physischen Datenträgers in Azure Stack."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 26e5e9f8882cc764922a2cbf0f39e7a3d1b6995b
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="replace-a-physical-disk-in-azure-stack"></a>Ersetzen eines physischen Datenträgers in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Dieser Artikel beschreibt das allgemeine Verfahren, um einen physischen Datenträger in Azure Stack zu ersetzen. Wenn ein physischer Datenträger ausfällt, sollten Sie ihn so bald wie möglich ersetzen.

Sie können dieses Verfahren für integrierte Systeme verwenden, und für Development Kit-Bereitstellungen, die über im laufenden Betrieb austauschbare Datenträger verfügen.

Die tatsächlichen Schritte zum Austausch von Datenträgern variieren und hängen von Ihrem Originalgerätehersteller-Hardwareanbieter (Original Equipment Manufacturer, OEM) ab. Die für Ihr System spezifischen ausführlichen Schritte finden Sie in der FRU-Dokumentation (Field Replaceable Unit) des Herstellers. 

## <a name="review-disk-alert-information"></a>Überprüfen von Datenträger-Warnungsinformationen
Wenn ein Datenträger ausfällt, teilt Ihnen eine Warnung mit, dass die Konnektivität mit einem physischen Datenträger verloren gegangen ist. 

 ![Warnung, die den Verlust der Konnektivität mit einem physischen Datenträger anzeigt](media/azure-stack-replace-disk/DiskAlert.png)

Wenn Sie die Warnung öffnen, informiert Sie die Warnungsbeschreibung über den Skalierungseinheitsknoten und das exakte physische Einschubfach für den Datenträger, den Sie ersetzen müssen. Azure Stack hilft Ihnen darüber hinaus, den fehlerhaften Datenträger mithilfe von LED-Anzeigefunktionen zu identifizieren.

 ## <a name="replace-the-disk"></a>Austausch des Datenträgers

Befolgen Sie die FRU-Anweisungen des OEM-Hardwareanbieters beim tatsächlichen Austausch des Datenträgers.

Um die Verwendung eines nicht unterstützten Datenträgers in einem integrierten System zu verhindern, blockiert das System Datenträger, die von Ihrem Anbieter nicht unterstützt werden. Wenn Sie versuchen, einen nicht unterstützten Datenträger zu verwenden, teilt Ihnen eine neue Warnung mit, dass ein Datenträger aufgrund eines nicht unterstützten Modells oder nicht unterstützter Firmware isoliert wurde.

Nachdem Sie den Datenträger ersetzt haben, ermittelt Azure Stack automatisch den neuen Datenträger und startet die Reparatur des virtuellen Datenträgers.  
 
 ## <a name="check-the-status-of-virtual-disk-repair"></a>Überprüfen des Status der Reparatur des virtuellen Datenträgers
 
 Nachdem Sie den Datenträger ersetzt haben, können Sie den Integritätsstatus des virtuellen Datenträgers und den Verlauf des Reparaturauftrags mithilfe des privilegierten Endpunkts überwachen. Führen Sie diese Schritte auf einem beliebigen Computer aus, der über Netzwerkkonnektivität mit dem privilegierten Endpunkt verfügt.

1. Öffnen Sie eine Windows PowerShell-Sitzung, und stellen Sie eine Verbindung mit dem privilegierten Endpunkt her.
    ````PowerShell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```` 
  
2. Führen Sie den folgenden Befehl zum Anzeigen der Integrität des virtuellen Datenträgers aus:
    ````PowerShell
        Get-VirtualDisk -CimSession s-cluster
    ````
   ![PowerShell-Ausgabe des Get-VirtualDisk-Befehls](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Führen Sie den folgenden Befehl zum Anzeigen des aktuellen Speicherauftragsstatus aus:
    ```PowerShell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ````
      ![PowerShell-Ausgabe des Get-StorageJob-Befehls](media/azure-stack-replace-disk/GetStorageJobOutput.png)

## <a name="troubleshoot-virtual-disk-repair"></a>Problembehandlung bei der Reparatur des virtuellen Datenträgers

Wenn der Auftrag zur Reparatur des virtuellen Datenträgers hängen bleibt, führen Sie den folgenden Befehl aus, um den Auftrag neu zu starten:
  ````PowerShell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ```` 

