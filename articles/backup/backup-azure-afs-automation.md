---
title: Bereitstellen und Verwalten von Sicherungen für Azure-Dateifreigaben mit PowerShell
description: Verwenden von PowerShell zum Bereitstellen und Verwalten von Sicherungen in Azure für Azure-Dateifreigaben
services: backup
author: pvrk
manager: shivamg
keywords: PowerShell; Sichern von Azure-Dateien; Wiederherstellen von Azure-Dateien;
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: 912336d697e8f7b5d9c71080ec9a052ca562da4b
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55101129"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>Verwenden von PowerShell zum Sichern und Wiederherstellen von Azure-Dateifreigaben

In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure PowerShell-Cmdlets eine Azure-Dateifreigabe in einem Recovery Services-Tresor sichern und daraus wiederherstellen. Ein Recovery Services-Tresor ist eine Ressource von Azure Resource Manager, die für den Schutz von Daten und Objekten in Azure Backup und Azure Site Recovery verwendet wird.

## <a name="concepts"></a>Konzepte

Wenn Sie mit Azure Backup noch nicht vertraut sind, können Sie sich im Artikel [Was ist Azure Backup?](backup-introduction-to-azure-backup.md) eine Übersicht über den Dienst verschaffen. Bevor Sie beginnen, informieren Sie sich in [Sichern von Azure-Dateifreigaben](backup-azure-files.md) über die Vorschaufunktionen, die zum Sichern von Azure-Dateifreigaben verwendet werden.

Damit Sie PowerShell effektiv nutzen können, ist es erforderlich, dass Sie die Objekthierarchie verstehen und wissen, womit Sie beginnen sollten.

![Recovery Services-Objekthierarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Sehen Sie sich die [Azure Backup – Recovery Services-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in der Azure-Bibliothek an, um die Cmdlet-Referenz **AzureRm.RecoveryServices.Backup PowerShell** anzuzeigen.

## <a name="setup-and-registration"></a>Einrichtung und Registrierung

> [!NOTE]
> Wie in [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.13.0) erwähnt, endet der Support für neue Features im AzureRM-Modul im November 2018. Support wird für die Sicherung von Azure-Dateifreigaben mit dem neuen Azure PowerShell-Modul geboten, das jetzt allgemein zur Verfügung steht.

Führen Sie zunächst die folgenden Schritte aus.

1. [Laden Sie die neueste Version von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0) herunter. Version 1.0.0 ist die erforderliche Mindestversion.

2. Suchen Sie die verfügbaren **Azure Backup PowerShell**-Cmdlets durch Eingabe des nachstehenden Befehls.

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    Die Aliase und Cmdlets für Azure Backup, Azure Site Recovery und den Recovery Services-Tresor werden angezeigt. Die folgende Abbildung ist ein Beispiel für die angezeigten Elemente. Dies ist nicht die vollständige Liste der Cmdlets.

    ![Liste der Recovery Services-Cmdlets](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Melden Sie sich mithilfe von **Connect-AzAccount** bei Ihrem Azure-Konto an. Dieses Cmdlet ruft eine Webseite auf, die Sie zur Eingabe Ihrer Kontoanmeldeinformationen auffordert:

    * Alternativ können Sie Ihre Kontoanmeldeinformationen als Parameter im Cmdlet **Connect-AzAccount** mit dem Parameter **-Credential** angeben.
    * Wenn Sie als CSP-Partner für einen Mandanten tätig sind, geben Sie den Kunden mit dessen Mandanten-ID oder primärem Mandantendomänennamen als Mandanten an. Beispiel: **Connect-AzAccount -Tenant** fabrikam.com.

4. Weil ein Konto mehrere Abonnements enthalten kann, müssen Sie das Abonnement, das Sie verwenden möchten, dem Konto zuordnen.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Falls Sie Azure Backup zum ersten Mal verwenden, registrieren Sie den Azure Recovery Services-Anbieter für Ihr Abonnement mithilfe des Cmdlets **Register-AzResourceProvider**.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Überprüfen Sie, ob die Anbieter erfolgreich registriert wurden, mit dem nachstehenden Befehl.
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    In der Befehlsausgabe ändert sich der Wert **RegistrationState** in **Registriert**. Wenn diese Änderung nicht angezeigt wird, führen Sie das Cmdlet **Register-AzResourceProvider** erneut aus.

Die folgenden Aufgaben können mit PowerShell automatisiert werden:

* Erstellen Sie einen Recovery Services-Tresor.
* Konfigurieren Sie die Sicherung für Azure-Dateifreigaben.
* Lösen Sie einen Sicherungsauftrag aus.
* Überwachen Sie einen Sicherungsauftrag.
* Stellen Sie eine Azure-Dateifreigabe wieder her.
* Stellen Sie eine einzelne Azure-Datei aus einer Azure-Dateifreigabe wieder her.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Führen Sie die nachstehenden Schritte aus, um einen Recovery Services-Tresors zu erstellen.

1. Der Recovery Services-Tresor ist eine Resource Manager-Ressource. Deshalb müssen Sie ihn in eine Ressourcengruppe einfügen. Sie können eine vorhandene Ressourcengruppe verwenden oder eine Ressourcengruppe mit dem Cmdlet **New-AzResourceGroup** erstellen. Wenn Sie eine Ressourcengruppe erstellen, geben Sie den Namen und den Speicherort dafür an.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Erstellen Sie mithilfe des Cmdlets **New-AzRecoveryServicesVault** den Recovery Services-Tresor. Geben Sie denselben Speicherort für den Tresor an, der für die Ressourcengruppe verwendet wurde.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Geben Sie den Typ der zu verwendenden Speicherredundanz an. Sie können [lokal redundanten Speicher](../storage/common/storage-redundancy-lrs.md) oder [georedundanten Speicher](../storage/common/storage-redundancy-grs.md) verwenden. Das folgende Beispiel zeigt, dass für die Option **-BackupStorageRedundancy** für **testVault** der Wert auf **GeoRedundant** festgelegt wurde.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Anzeigen von Tresoren in einem Abonnement

Wenn Sie alle Tresore des Abonnements anzeigen möchten, verwenden Sie **Get-AzRecoveryServicesVault**.

```powershell
Get-AzRecoveryServicesVault
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus: Beachten Sie, dass die zugeordnete **ResourceGroupName** und **Speicherort** angegeben werden.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

Viele Azure Backup-Cmdlets benötigen das Recovery Services-Tresorobjekt als Eingabe.

Verwenden Sie **Set-AzRecoveryServicesVaultContext**, um den Tresorkontext festzulegen. Nachdem der Tresorkontext festgelegt wurde, gilt er für alle nachfolgenden Cmdlets. Im folgenden Beispiel wird der Tresorkontext für **testvault** festgelegt.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> Es ist geplant, die Tresorkontexteinstellung entsprechend den Azure PowerShell-Richtlinien als veraltet zu kennzeichnen. Stattdessen sollten diese Benutzer die Tresor-ID übergeben, wie in den nachstehenden Anleitungen erwähnt wird.

Alternativ können Sie die ID des Tresors speichern oder abrufen, für den Sie einen PowerShell-Vorgang ausführen möchten, und die ID an den entsprechenden Befehl übergeben.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Konfigurieren der Sicherung für eine Azure-Dateifreigabe

### <a name="create-a-protection-policy"></a>Erstellen einer Schutzrichtlinie

Eine Sicherungsschutzrichtlinie ist mindestens einer Aufbewahrungsrichtlinie zugeordnet. Eine Aufbewahrungsrichtlinie definiert, wie lange ein Wiederherstellungspunkt gespeichert werden soll, bevor er gelöscht wird. Verwenden Sie **Get-AzRecoveryServicesBackupRetentionPolicyObject**, um die standardmäßige Aufbewahrungsrichtlinie anzuzeigen. 

Auf ähnliche Weise können Sie **Get-AzRecoveryServicesBackupSchedulePolicyObject** verwenden, um die standardmäßige Zeitplanrichtlinie abzurufen. Das Cmdlet **New-AzRecoveryServicesBackupProtectionPolicy** erstellt ein PowerShell-Objekt, das Informationen zu Sicherungsrichtlinien enthält. Die Zeitplan- und Aufbewahrungsrichtlinienobjekte werden als Eingaben für das Cmdlet **New-AzRecoveryServicesBackupProtectionPolicy** verwendet. 

Im folgenden Beispiel werden die Zeitplanrichtlinie und die Aufbewahrungsrichtlinie in Variablen gespeichert. Im Beispiel werden diese Variablen verwendet, um die Parameter beim Erstellen der Schutzrichtlinie **NewPolicy** zu definieren.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

Die Richtlinie **NewAFSPolicy**führt eine tägliche Sicherung aus und bewahrt sie 30 Tage auf.

### <a name="enable-protection"></a>Schutz aktivieren

Nachdem Sie die Schutzrichtlinie definiert haben, können Sie den Schutz für die Azure-Dateifreigabe mit dieser Richtlinie aktivieren.

Rufen Sie zuerst das entsprechende Richtlinienobjekt mit dem Cmdlet **Get-AzRecoveryServicesBackupProtectionPolicy** ab. Verwenden Sie dieses Cmdlet zum Abrufen einer bestimmten Richtlinie oder zum Anzeigen der Richtlinien, die einem bestimmten Workloadtyp zugeordnet sind.

Im folgenden Beispiel werden Richtlinien für den Workloadtyp **AzureFiles** abgerufen.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> Die Zeitzone für das Feld **BackupTime** in PowerShell ist die koordinierte Weltzeit (UTC). Wenn der Zeitpunkt der Sicherung im Azure-Portal angezeigt wird, wird die Uhrzeit auf Ihre lokale Zeitzone eingestellt.
>
>

Die folgende Richtlinie ruft die Sicherungsrichtlinie mit dem Namen **dailyafs** ab.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Verwenden Sie **Enable-AzRecoveryServicesBackupProtection**, um den Schutz des Elements mit der angegebenen Richtlinie zu aktivieren. Nachdem die Richtlinie mit dem Tresor verknüpft wurde, wird der Sicherungsworkflow zu dem im Richtlinienzeitplan definierten Zeitpunkt gestartet.

Im folgenden Beispiel wird der Schutz für die Azure-Dateifreigabe **testAzureFileShare** unter dem Speicherkonto **testStorageAcct** mit der Richtlinie **dailyafs** aktiviert.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Der Befehl wartet, bis der Auftrag zum Konfigurieren des Schutzes beendet ist, und gibt eine ähnliche Ausgabe wie gezeigt zurück.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>Auslösen einer bedarfsgesteuerten Sicherung

Verwenden Sie **Backup-AzRecoveryServicesBackupItem**, um einen Sicherungsauftrag für eine geschützte Azure-Dateifreigabe auszulösen. Rufen Sie das Speicherkonto und die Dateifreigabe darin mithilfe der folgenden Befehle ab, und lösen Sie eine bedarfsgesteuerte Sicherung aus.

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Der Befehl gibt einen Auftrag mit einer ID zur Nachverfolgung wie im folgenden Beispiel gezeigt zurück.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Momentaufnahmen von Azure-Dateifreigaben werden beim Erstellen von Sicherungen verwendet. Daher ist der Auftrag in der Regel abgeschlossen, bis der Befehl diese Ausgabe zurückgibt.

### <a name="modify-the-protection-policy"></a>Ändern der Schutzrichtlinie

Zum Ändern der Richtlinie, mit der die Azure-Dateifreigabe geschützt wird, verwenden Sie **Enable-AzRecoveryServicesBackupProtection** mit dem entsprechenden Sicherungselement und der neuen Schutzrichtlinie.

Im folgenden Beispiel wird die Schutzrichtlinie von **testAzureFS** von **dailyafs** in **monthlyafs** geändert.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-azure-files"></a>Wiederherstellen von Azure-Dateifreigaben und Azure-Dateien

Sie können eine gesamte Dateifreigabe am ursprünglichen Speicherort oder an einem alternativen Speicherort wiederherstellen. Auf ähnliche Weise können auch einzelne Dateien aus der Dateifreigabe wiederhergestellt werden.

### <a name="fetch-recovery-points"></a>Abrufen von Wiederherstellungspunkten

Verwenden Sie das Cmdlet **Get-AzRecoveryServicesBackupRecoveryPoint**, um alle Wiederherstellungspunkte für das Sicherungselement aufzulisten. Im folgenden Skript ist die Variable **$rp** ein Array von Wiederherstellungspunkten für das ausgewählte Sicherungselement der letzten sieben Tage. Das Array wird in umgekehrter Reihenfolge sortiert. Der letzte Wiederherstellungspunkt liegt bei Index **0**. Verwenden Sie die standardmäßige PowerShell-Arrayindizierung zum Auswählen des Wiederherstellungspunkts. Im Beispiel wählt **$rp[0]** den letzten Wiederherstellungspunkt aus.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```

Nachdem der entsprechende Wiederherstellungspunkt ausgewählt wurde, stellen Sie die Dateifreigabe oder Datei an einem alternativen Speicherort oder am ursprünglichen Speicherort wieder her, wie hier erläutert wird.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Wiederherstellen von Azure-Dateifreigaben an einem alternativen Speicherort

#### <a name="restore-an-azure-file-share"></a>Wiederherstellen einer Azure-Dateifreigabe

Identifizieren Sie den alternativen Speicherort, indem Sie die folgenden Informationen (Parameter) angeben:

* **TargetStorageAccountName**: Das Speicherkonto, in dem der gesicherte Inhalt wiederhergestellt wird. Das Zielspeicherkonto muss sich am gleichen Speicherort wie der Tresor befinden.
* **TargetFileShareName**: Die Dateifreigaben in dem Zielspeicherkonto, in dem der gesicherte Inhalt wiederhergestellt wird.
* **TargetFolder**: Der Ordner unter der Dateifreigabe, in dem die Daten wiederhergestellt werden. Wenn der gesicherte Inhalt in einem Stammordner wiederhergestellt werden soll, geben Sie die Werte für den Zielordner als eine leere Zeichenfolge ein.
* **ResolveConflict**: Die Anweisung bei einem Konflikt mit den wiederhergestellten Daten. Für diesen Parameter kann **Overwrite** oder **Skip** angegeben werden.

Geben Sie diese Parameter für den Restore-Befehl an, um eine gesicherte Dateifreigabe an einem alternativen Speicherort wiederherzustellen.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

Der Befehl gibt einen Auftrag mit einer ID zur Nachverfolgung zurück, wie im folgenden Beispiel gezeigt wird.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

#### <a name="restore-an-azure-file"></a>Wiederherstellen einer Azure-Datei

Falls Sie statt einer gesamten Dateifreigabe eine einzelne Datei wiederherstellen möchten, identifizieren Sie diese Datei eindeutig durch Angeben der folgenden Parameter:

* **TargetStorageAccountName**: Das Speicherkonto, in dem der gesicherte Inhalt wiederhergestellt wird. Das Zielspeicherkonto muss sich am gleichen Speicherort wie der Tresor befinden.
* **TargetFileShareName**: Die Dateifreigaben in dem Zielspeicherkonto, in dem der gesicherte Inhalt wiederhergestellt wird.
* **TargetFolder**: Der Ordner unter der Dateifreigabe, in dem die Daten wiederhergestellt werden. Wenn der gesicherte Inhalt in einem Stammordner wiederhergestellt werden soll, geben Sie die Werte für den Zielordner als eine leere Zeichenfolge ein.
* **SourceFilePath**: Der absolute Pfad der (in der Dateifreigabe wiederherzustellenden) Datei als Zeichenfolge. Dieser Pfad ist derselbe Pfad, der im PowerShell-Cmdlet **Get-AzStorageFile** verwendet wird.
* **SourceFileType**: Gibt an, ob ein Verzeichnis oder eine Datei ausgewählt wurde. Für diesen Parameter kann **Directory** oder **File** angegeben werden.
* **ResolveConflict**: Die Anweisung bei einem Konflikt mit den wiederhergestellten Daten. Für diesen Parameter kann **Overwrite** oder **Skip** angegeben werden.

Die zusätzlichen Parameter beziehen sich nur auf die einzelne wiederherzustellende Datei.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Wie weiter oben gezeigt wurde, gibt dieser Befehl auch einen Auftrag mit einer ID zur Nachverfolgung zurück.

### <a name="restore-azure-file-shares-to-the-original-location"></a>Wiederherstellen von Azure-Dateifreigaben am ursprünglichen Speicherort

Wenn Sie an einem ursprünglichen Speicherort wiederherstellen, müssen nicht alle Zielparameter und zielbezogenen Parameter angegeben werden. Nur **ResolveConflict** muss angegeben werden.

#### <a name="overwrite-an-azure-file-share"></a>Überschreiben einer Azure-Dateifreigabe

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Überschreiben einer Azure-Datei

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Nachverfolgen von Sicherungs- und Wiederherstellungsaufträgen

Bei bedarfsgesteuerten Sicherungs- und Wiederherstellungsvorgängen wird ein Auftrag zusammen mit einer ID zurückgegeben, wie weiter oben im Abschnitt [„Auslösen einer bedarfsgesteuerten Sicherung“](#trigger-an-on-demand-backup) gezeigt wurde. Verwenden Sie das Cmdlet **Get-AzRecoveryServicesBackupJobDetails**, um den Fortschritt des Auftrags nachzuverfolgen und weitere Details abzurufen.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```
