---
title: "Aktivieren der Azure CLI für Azure Stack-Benutzer | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die plattformübergreifende Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden, um Ressourcen in Azure Stack zu verwalten und bereitzustellen."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: d184bb9edbe2542d7321d8b9ccc5d23f2401f8d5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Aktivieren der Azure CLI für Azure Stack-Benutzer

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Es gibt keine bedienerspezifischen Azure Stack-Aufgaben, die Sie über die Befehlszeilenschnittstelle ausführen können. Bevor Benutzer jedoch Ressourcen über die Befehlszeilenschnittstelle verwalten können, müssen Azure Stack-Bediener den Benutzern Folgendes bereitstellen:

* Das **Azure Stack-Zertifizierungsstellen-Stammzertifikat** ist erforderlich, wenn die Benutzer die Befehlszeilenschnittstelle auf einer Arbeitsstation außerhalb des Azure Stack Development Kits verwenden.  

* Der **Endpunkt der VM-Aliase** stellt einen Alias (etwa „UbuntuLTS“ oder „Win2012Datacenter“) bereit, der beim Bereitstellen virtueller Computer als einzelner Parameter auf einen Herausgeber, ein Angebot, eine SKU und die Version eines Images verweist.  

In den folgenden Abschnitten wird beschrieben, wie Sie diese Werte abrufen.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Exportieren des Azure Stack-Zertifizierungsstellen-Stammzertifikats

Das Azure Stack-Zertifizierungsstellen-Stammzertifikat ist im Development Kit und auf einem virtuellen Mandantencomputer verfügbar, der innerhalb der Development Kit-Umgebung ausgeführt wird. Melden Sie sich bei Ihrem Development Kit oder dem virtuellen Mandantencomputer an, und führen Sie das folgende Skript aus, um das Azure Stack-Stammzertifikat im PEM-Format zu exportieren:

```powershell
$label = "AzureStackSelfSignedRootCert"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Cerficate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Einrichten des Endpunkts der VM-Aliase

Azure Stack-Betreiber sollten einen öffentlich erreichbaren Endpunkt einrichten, der eine VM-Aliasdatei hostet.  Bei der VM-Aliasdatei handelt es sich um eine JSON-Datei mit einem allgemeinen Namen für ein Image, der dann beim Bereitstellen eines virtuellen Computers als Azure CLI-Parameter angegeben wird.  

Stellen Sie vor dem Hinzufügen eines Eintrags zu einer Aliasdatei sicher, dass Sie [Images aus dem Marketplace]((azure-stack-download-azure-marketplace-item.md) heruntergeladen oder [ein eigenes benutzerdefinierte Image veröffentlicht](azure-stack-add-vm-image.md) haben.  Wenn Sie ein benutzerdefiniertes Image veröffentlichen, notieren Sie sich die während der Veröffentlichung angegebenen Informationen zu Herausgeber, Angebot, SKU und Version.  Wenn es sich um ein Image aus dem Marketplace handelt, können Sie die Informationen mithilfe des ```Get-AzureVMImage```-Cmdlets anzeigen.  
   
Eine [Aliasbeispieldatei](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) mit zahlreichen allgemeinen Imagealiasen ist verfügbar, die Sie als Ausgangspunkt verwenden können.  Sie sollten diese Datei an einem Ort hosten, auf den die CLI-Clients zugreifen können.  Eine Möglichkeit besteht darin, die Datei in einem Blobspeicherkonto zu hosten und die URL an Ihre Benutzer zu senden:

1.  Laden Sie die [Beispieldatei](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) von GitHub herunter.
2.  Erstellen Sie ein neues Speicherkonto in Azure Stack.  Erstellen Sie anschließend einen neuen Blobcontainer.  Legen Sie die Zugriffsrichtlinie auf „Öffentlich“ fest.  
3.  Laden Sie die JSON-Datei in den neuen Container hoch.  Anschließend können Sie die URL des Blobs anzeigen, indem Sie auf den Blobnamen klicken und dann die URL aus den Blobeigenschaften auswählen.


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit der Azure CLI](azure-stack-deploy-template-command-line.md)

[Verbinden mit PowerShell](azure-stack-connect-powershell.md)

[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)

