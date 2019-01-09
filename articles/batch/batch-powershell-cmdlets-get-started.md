---
title: 'Erste Schritte mit PowerShell: Azure Batch | Microsoft-Dokumentation'
description: Schnelle Einführung in die Azure PowerShell-Cmdlets zum Verwalten von Batch-Ressourcen.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 10/05/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: bd8e5425a09f0faeaa573cec58def88f352b9a1d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548067"
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a>Verwalten von Batch-Ressourcen mit PowerShell-Cmdlets

Mit den Azure Batch-PowerShell-Cmdlets können Sie viele der Aufgaben ausführen, für die Sie die Batch-APIs, das Azure-Portal und die Azure-Befehlszeilenschnittstelle (CLI) verwenden, und Skripts dafür erstellen. Dieser Artikel enthält eine kurze Einführung in die Cmdlets, mit denen Sie Ihre Batch-Konten verwalten und Batch-Ressourcen wie Pools, Aufträge und Aufgaben verwenden.

Eine vollständige Liste mit Batch-Cmdlets und deren ausführliche Cmdlet-Syntax finden Sie in der Referenz zu [Azure Batch Cmdlets](/powershell/module/azurerm.batch/#batch)(Azure Batch-Cmdlets).

Dieser Artikel basiert auf Cmdlets im Azure Batch-Modul 4.1.5. Es empfiehlt sich, die Azure PowerShell-Module regelmäßig zu aktualisieren, um in den Genuss von Dienstupdates und Verbesserungen zu kommen.

## <a name="prerequisites"></a>Voraussetzungen

* [Installieren und konfigurieren Sie das Azure PowerShell-Modul.](/powershell/azure/overview) Informationen zum Installieren eines bestimmten Azure Batch-Moduls (etwa eines vorab veröffentlichten Moduls) finden Sie im [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM.Batch/5.0.0-preview). 

* Führen Sie das **Connect-AzureRmAccount**-Cmdlet aus, um eine Verbindung mit Ihrem Abonnement herzustellen (die Azure Batch-Cmdlets sind Teil des Azure Resource Manager-Moduls):

  ```PowerShell
  Connect-AzureRmAccount
  ```

* **Führen Sie die Registrierung beim Batch-Anbieternamespace durch**. Dieser Vorgang muss lediglich **einmal pro Abonnement** ausgeführt werden.
  
  ```PowerShell
  Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`
  ```

## <a name="manage-batch-accounts-and-keys"></a>Verwalten von Batch-Konten und Schlüsseln

### <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos

**New-AzureRmBatchAccount** erstellt ein Batch-Konto in einer angegebenen Ressourcengruppe. Falls Sie noch nicht über eine Ressourcengruppe verfügen, können Sie eine erstellen, indem Sie das [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)-Cmdlet ausführen. Geben Sie eine Azure-Region im Parameter **Standort** an, z.B. „USA, Mitte“. Beispiel: 

```PowerShell
New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"
```

Erstellen Sie anschließend ein Batch-Konto in der Ressourcengruppe, und geben Sie einen Namen für das Konto in <*account_name*> und den Speicherort und Namen der Ressourcengruppe an. Die Erstellung des Batch-Kontos kann einige Zeit dauern. Beispiel: 

```PowerShell
New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>
```

> [!NOTE]
> Der Batch-Kontoname muss in der Azure-Region für die Ressourcengruppe eindeutig sein, zwischen 3 und 24 Zeichen umfassen und darf nur Kleinbuchstaben und Zahlen enthalten.
> 

### <a name="get-account-access-keys"></a>Abrufen von Kontozugriffsschlüsseln

**Get-AzureRmBatchAccountKeys** zeigt die Zugriffsschlüssel an, die einem Azure Batch-Konto zugeordnet sind. Führen Sie beispielsweise Folgendes aus, um den primären und sekundären Schlüssel des Kontos abzurufen, das Sie erstellt haben.

 ```PowerShell
$Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

$Account.PrimaryAccountKey

$Account.SecondaryAccountKey
```

### <a name="generate-a-new-access-key"></a>Generieren eines neuen Zugriffsschlüssels

**New-AzureRmBatchAccountKey** generiert einen neuen primären oder sekundären Kontoschlüssel für ein Azure Batch-Konto. Geben Sie zum Generieren eines neuen primären Schlüssels für Ihr Batch-Konto z. B. Folgendes ein:

```PowerShell
New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary
```

> [!NOTE]
> Um einen neuen sekundären Schlüssel zu generieren, geben Sie "Secondary" für den **KeyType** -Parameter ein. Sie müssen den primären und sekundären Schlüssel separat neu generieren.
> 

### <a name="delete-a-batch-account"></a>Löschen eines Batch-Kontos

**Remove-AzureRmBatchAccount** löscht ein Batch-Konto. Beispiel: 

```PowerShell
Remove-AzureRmBatchAccount -AccountName <account_name>
```

Bestätigen Sie bei der entsprechenden Aufforderung, dass Sie das Konto entfernen möchten. Das Entfernen des Kontos kann einige Zeit in Anspruch nehmen.

## <a name="create-a-batchaccountcontext-object"></a>Erstellen eines BatchAccountContext-Objekts

Die Batch-APIs können entweder unter Verwendung der Authentifizierung mit gemeinsam verwendetem Schlüssel oder unter Verwendung der Azure Active Directory-Authentifizierung verwaltet werden. Wenn Sie Batch-PowerShell-Cmdlets für die Authentifizierung verwenden möchten, müssen Sie zunächst ein BatchAccountContext-Objekt erstellen, in dem Sie Ihre Kontoanmeldeinformationen oder Ihre Identität speichern können. Übergeben Sie das BatchAccountContext-Objekt in Cmdlets, die den Parameter **BatchContext** verwenden. 

### <a name="shared-key-authentication"></a>Authentifizierung mit gemeinsam verwendetem Schlüssel

```PowerShell
$context = Get-AzureRmBatchAccountKeys -AccountName <account_name>
```

> [!NOTE]
> Standardmäßig wird der primäre Schlüssel des Kontos für die Authentifizierung verwendet. Sie können aber ausdrücklich den zu verwendenden Schlüssel auswählen, indem Sie die **KeyInUse**-Eigenschaft des BatchAccountContext-Objekts ändern: `$context.KeyInUse = "Secondary"`.
> 

### <a name="azure-active-directory-authentication"></a>Authentifizierung über Azure Active Directory

```PowerShell
$context = Get-AzureRmBatchAccount -AccountName <account_name>
```

## <a name="create-and-modify-batch-resources"></a>Erstellen und Ändern von Batch-Ressourcen
Verwenden Sie Cmdlets wie **New-AzureBatchPool**, **New-AzureBatchJob** und **New-AzureBatchTask**, um unter einem Batch-Konto Ressourcen zu erstellen. Die entsprechenden Cmdlets **Get-** und **Set-** zum Aktualisieren der Eigenschaften vorhandener Ressourcen sind vorhanden, sowie auch Cmdlets vom Typ **Remove-**, um unter einem Batch-Konto Ressourcen zu entfernen.

Bei der Verwendung von vielen dieser Cmdlets müssen Sie zusätzlich zum Übergeben eines BatchContext-Objekts auch Objekte erstellen oder übergeben, die ausführliche Ressourceneinstellungen enthalten. Dies wird im folgenden Beispiel veranschaulicht. In der detaillierten Hilfe für die einzelnen Cmdlets finden Sie weitere Beispiele.

### <a name="create-a-batch-pool"></a>Batch-Pool erstellen

Beim Erstellen oder Aktualisieren eines Batch-Pools wählen Sie entweder die Konfiguration der Clouddienste oder die VM-Konfiguration für das Betriebssystem auf den Computeknoten aus (siehe [Übersicht über Azure Batch-Features](batch-api-basics.md#pool)). Wenn Sie die Konfiguration der Clouddienste angeben, werden Ihre Computeknoten mit einem Image der [Azure-Gastbetriebssystemversionen](../cloud-services/cloud-services-guestos-update-matrix.md#releases) versehen. Wenn Sie die VM-Konfiguration angeben, können Sie entweder eines der unterstützten Linux- oder Windows-VM-Images aus dem [Marketplace für virtuelle Azure-Computer][vm_marketplace] angeben oder ein eigenes benutzerdefiniertes Image bereitstellen.

Übergeben Sie beim Ausführen von **New-AzureBatchPool**die Betriebssystemeinstellungen in einem PSCloudServiceConfiguration- oder PSVirtualMachineConfiguration-Objekt. Der folgende Codeausschnitt erstellt beispielsweise einen neuen Batch-Pool mit Computeknoten der Größe „Standard_A1“ in der VM-Konfiguration mit Ubuntu Server 16.04-LTS-Image. Hier gibt der Parameter **VirtualMachineConfiguration** die Variable *$configuration* als PSVirtualMachineConfiguration-Objekt an. Mit dem Parameter **BatchContext** wird eine zuvor definierte Variable *$context* als BatchAccountContext-Objekt angegeben.

```PowerShell
$imageRef = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSImageReference" -ArgumentList @("UbuntuServer","Canonical","16.04.0-LTS")

$configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSVirtualMachineConfiguration" -ArgumentList @($imageRef, "batch.node.ubuntu 16.04")

New-AzureBatchPool -Id "mypool" -VirtualMachineSize "Standard_a1" -VirtualMachineConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context
```

Die Zielanzahl von Computeknoten im neuen Pool wird mit einer Formel für die automatische Skalierung bestimmt. In diesem Fall lautet die Formel einfach **$TargetDedicated=4**und gibt an, dass die Anzahl von Computeknoten im Pool maximal „4“ beträgt.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Abfragen von Pools, Aufträgen, Aufgaben und weiteren Details

Verwenden Sie Cmdlets wie **Get-AzureBatchPool**, **Get-AzureBatchJob** und **Get-AzureBatchTask** zum Abfragen von Entitäten, die in einem Batch-Konto erstellt wurden.

### <a name="query-for-data"></a>Abfragen von Daten

Verwenden Sie beispielsweise **Get-AzureBatchPools** zum Suchen Ihrer Pools. Damit werden standardmäßig alle Pools unter Ihrem Konto abgefragt, sofern Sie das BatchAccountContext-Objekt bereits in *$context*gespeichert haben:

```PowerShell
Get-AzureBatchPool -BatchContext $context
```

### <a name="use-an-odata-filter"></a>Verwenden eines OData-Filters

Mit dem **Filter** -Parameter können Sie einen OData-Filter angeben, um nur bestimmte gewünschte Objekte zu suchen. Sie können beispielsweise alle Pools suchen, deren IDs mit "myPool" beginnen:

```PowerShell
$filter = "startswith(id,'myPool')"

Get-AzureBatchPool -Filter $filter -BatchContext $context
```

Diese Methode ist nicht so flexibel wie die Verwendung von "Where-Object" in einer lokalen Pipeline. Die Abfrage wird jedoch direkt an den Batch-Dienst gesendet, sodass die gesamte Filterung auf dem Server erfolgt, was Internetbandbreite einspart.

### <a name="use-the-id-parameter"></a>Verwenden des Id-Parameters

Eine Alternative zu einem OData-Filter stellt die Verwendung des **Id** -Parameters dar. So führen Sie eine Abfrage für einen bestimmten Pool mit der ID "myPool" aus:

```PowerShell
Get-AzureBatchPool -Id "myPool" -BatchContext $context
```

Der **Id** -Parameter unterstützt ausschließlich die Suche nach der vollständigen ID, jedoch keine Platzhalter oder Filter im OData-Format.

### <a name="use-the-maxcount-parameter"></a>Verwenden des MaxCount-Parameters

Jedes Cmdlet gibt standardmäßig bis zu 1.000 Objekte zurück. Wenn dieser Grenzwert erreicht ist, können Sie entweder den Filter weiter eingrenzen, sodass weniger Objekte zurückgegeben werden, oder mit dem **MaxCount** -Parameter explizit einen maximalen Wert festlegen. Beispiel: 

```PowerShell
Get-AzureBatchTask -MaxCount 2500 -BatchContext $context
```

Setzen Sie den **MaxCount** -Parameter auf 0 oder eine negative Zahl, um die Obergrenze zu entfernen.

### <a name="use-the-powershell-pipeline"></a>Verwenden der PowerShell-Pipeline

Batch-Cmdlets können die PowerShell-Pipeline zum Senden von Daten zwischen Cmdlets nutzen. Dies hat die gleiche Auswirkung wie die Angabe eines Parameters, vereinfacht aber die Verwendung mehrerer Entitäten.

Beispielsweise können Sie nach allen Aufgaben Ihres Kontos suchen und diese anzeigen:

```PowerShell
Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context
```

Starten Sie jeden Computeknoten eines Pools neu:

```PowerShell
Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context
```

## <a name="application-package-management"></a>Anwendungspaketverwaltung

Anwendungspakete ermöglichen eine vereinfachte Anwendungsbereitstellung auf den Computeknoten in Ihren Pools. Mit den Batch PowerShell-Cmdlets können Sie Anwendungspakete in Ihrem Batch-Konto hochladen und verwalten und Paketversionen für Computeknoten bereitstellen.

**Erstellen** einer Anwendung:

```PowerShell
New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"
```

**Hinzufügen** eines Anwendungspakets:

```PowerShell
New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip
```

Festlegen der **Standardversion** für die Anwendung:

```PowerShell
Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"
```

**Auflisten** der Pakete einer Anwendung

```PowerShell
$application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

$application.ApplicationPackages
```

**Löschen** eines Anwendungspakets

```PowerShell
Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"
```

**Löschen** einer Anwendung

```PowerShell
Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"
```

> [!NOTE]
> Sie müssen alle Anwendungspaketversionen einer Anwendung löschen, bevor Sie die Anwendung löschen. Sie erhalten einen Konfliktfehler, wenn Sie versuchen, eine Anwendung zu löschen, die derzeit über Anwendungspakete verfügt.
> 

### <a name="deploy-an-application-package"></a>Bereitstellen eines Anwendungspakets

Beim Erstellen eines Pools können Sie Anwendungspakete angeben, die Sie bereitstellen möchten. Wenn Sie ein Paket bei der Poolerstellung angeben, wird es auf den einzelnen Knoten bereitgestellt, sobald diese dem Pool hinzugefügt werden. Pakete werden auch bereitgestellt, wenn ein Knoten neu gestartet oder ein Reimaging für den Knoten durchgeführt wird.

Geben Sie bei der Poolerstellung die `-ApplicationPackageReference`-Option an, um ein Anwendungspaket auf den Knoten des Pools bereitzustellen, wenn diese dem Pool hinzugefügt werden. Erstellen Sie zunächst ein **PSApplicationPackageReference**-Objekt, und konfigurieren Sie es mit der Anwendungs-ID und Paketversion, die Sie auf den Computeknoten des Pools bereitstellen möchten:

```PowerShell
$appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

$appPackageReference.ApplicationId = "MyBatchApplication"

$appPackageReference.Version = "1.0"
```

Erstellen Sie nun den Pool, und geben Sie das Paketverweisobjekt als Argument für die Option `ApplicationPackageReferences` an:

```PowerShell
New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference
```

Weitere Informationen zu Anwendungspaketen finden Sie unter [Bereitstellen von Anwendungen auf Computeknoten mit Batch-Anwendungspaketen](batch-application-packages.md).

> [!IMPORTANT]
> Damit Sie Anwendungspakete verwenden können, müssen Sie zunächst ein [Azure Storage-Konto mit Ihrem Batch-Konto verknüpfen](#linked-storage-account-autostorage) .
> 
> 

### <a name="update-a-pools-application-packages"></a>Aktualisieren der Anwendungspakete eines Pools

Erstellen Sie zum Aktualisieren der Anwendungen, die einem vorhandenen Pool zugewiesen sind, zuerst ein PSApplicationPackageReference-Objekt mit den gewünschten Eigenschaften (Anwendungs-ID und Paketversion):

```PowerShell
$appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

$appPackageReference.ApplicationId = "MyBatchApplication"

$appPackageReference.Version = "2.0"

```

Rufen Sie als Nächstes den Pool aus Batch ab, löschen Sie alle vorhandenen Pakete, fügen Sie Ihren neuen Paketverweis hinzu, und aktualisieren Sie den Batch-Dienst mit den neuen Pooleinstellungen:

```PowerShell
$pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

$pool.ApplicationPackageReferences.Clear()

$pool.ApplicationPackageReferences.Add($appPackageReference)

Set-AzureBatchPool -BatchContext $context -Pool $pool
```

Sie haben die Eigenschaften des Pools jetzt im Batch-Dienst aktualisiert. Um das neue Anwendungspaket tatsächlich auf den Computeknoten des Pools bereitzustellen, müssen Sie diese Knoten aber neu starten oder ein Reimaging dafür durchführen. Sie können jeden Knoten eines Pools mit diesem Befehl neu starten:

```PowerShell
Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context
```

> [!TIP]
> Sie können für die Computeknoten eines Pools mehrere Anwendungspakete bereitstellen. Wenn Sie ein Anwendungspaket *hinzufügen* möchten, anstatt die derzeit bereitgestellten Pakete zu ersetzen, lassen Sie im obigen Code einfach die Zeile `$pool.ApplicationPackageReferences.Clear()` weg.
> 
> 

## <a name="next-steps"></a>Nächste Schritte

* Die ausführliche Cmdlet-Syntax sowie Beispiele finden Sie in der [Referenz zu Azure Batch-Cmdlets](/powershell/module/azurerm.batch/#batch).
* Weitere Informationen zu Anwendungen und Anwendungspaketen unter Batch finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
