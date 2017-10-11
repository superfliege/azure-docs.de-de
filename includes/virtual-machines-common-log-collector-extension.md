
Diagnostizieren von Problemen mit einem Microsoft Azure-Cloud-Dienst muss der Dienst-Protokolldateien auf virtuellen Computern sammeln, wenn die Probleme auftreten. Sie können mit der AzureLogCollector-Erweiterung bei Bedarf können Sie eine einmalige Sammlung von Protokollen ein oder mehrere Cloud-Dienst-VMs (von Web- und Workerrollen) und die gesammelten Dateien an ein Azure Storage-Konto – ohne Remoteanmeldung an VMs übertragen.

> [!NOTE]
> Eine Beschreibung für den Großteil der protokollierten Informationen finden Sie unter http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.
> 
> 

Es gibt zwei Modi für die Sammlung abhängig von den Typen der zu sammelnden Dateien.

* Azure-Gast-Agent-Protokolle nur (GA). Dieser Sammlungsmodus umfasst alle Protokolle, die im Zusammenhang mit der Azure-Gast-Agents und anderen Azure-Komponenten.
* Alle Protokolle (voll). Dieser Sammlungsmodus sammelt alle Dateien im GA-Modus und zusätzlich:
  
  * Ereignisprotokolle System und Anwendung
  * HTTP-Fehlerprotokollen
  * IIS-Protokolle
  * Setupprotokolle
  * Sonstige Systemprotokolle

In beiden sammlungsmodi können zusätzliche Daten Sammlungsordner angegeben werden, mithilfe einer Auflistung der folgenden Struktur:

* **Namen**: der Name der Sammlung, der als Name des Unterordners innerhalb der Zip-Datei, die gesammelt werden, verwendet wird.
* **Speicherort**: der Pfad zu dem Ordner auf dem virtuellen Computer, auf dem Datei gesammelt werden.
* **SearchPattern**: das Muster mit den Namen der zu sammelnden Dateien. Standardwert ist "*"
* **Rekursive**: Wenn die Dateien rekursiv im Ordner gesammelt werden.

## <a name="prerequisites"></a>Voraussetzungen
* Sie müssen ein Speicherkonto für die Erweiterung um generierte Zip-Dateien zu speichern.
* Sie müssen sicherstellen, dass die Verwendung von Azure PowerShell-Cmdlets V0.8.0 oder höher. Weitere Informationen finden Sie unter [Azure-Downloads](https://azure.microsoft.com/downloads/).

## <a name="add-the-extension"></a>Fügen Sie die Erweiterung
Sie können [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) Cmdlets oder [REST-APIs der Dienstverwaltung](https://msdn.microsoft.com/library/ee460799.aspx) zum Hinzufügen der Erweiterung "azurelogcollector".

Für Cloud-Dienste, das vorhandene Azure Powershell-Cmdlet **Set AzureServiceExtension**, kann verwendet werden, um die Erweiterung für Cloud-Dienst-Rolleninstanzen zu aktivieren. Jedes Mal, wenn diese Erweiterung durch dieses Cmdlet aktiviert ist, wird auf den ausgewählten Rolleninstanzen der ausgewählten Rollen die Protokollsammlung ausgelöst.

Für virtuelle Maschinen, das vorhandene Azure Powershell-Cmdlet **Set-AzureVMExtension**, kann verwendet werden, um die Erweiterung auf virtuellen Computern zu aktivieren. Jedes Mal, wenn diese Erweiterung über die Cmdlets aktiviert ist, wird für jede Instanz die Protokollsammlung ausgelöst.

Diese Erweiterung verwendet intern die JSON-basierte PublicConfiguration und PrivateConfiguration. Im folgenden wird das Layout eines JSON-Beispiels für öffentliche und private Konfiguration.

### <a name="publicconfiguration"></a>PublicConfiguration
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri to your storage account with sp=wl",
        "AdditionalData":
        [
          {
                  "Name":  "StorageData",
                  "Location":  "%roleroot%storage",
                  "SearchPattern":  "*.*",
                  "Recursive":  "true"
          },
          {
                "Name":  "CustomDataFolder2",
                "Location":  "c:\customFolder",
                "SearchPattern":  "*.log",
                "Recursive":  "false"
          },
        ]
    }

### <a name="privateconfiguration"></a>PrivateConfiguration
    {

    }

> [!NOTE]
> Diese Erweiterung benötigt **PrivateConfiguration**. Sie bieten nur eine leere Struktur für die **– PrivateConfiguration** Argument.
> 
> 

Führen Sie eine der folgenden zwei Schritte eine oder mehrere Instanzen eines Cloud-Dienst oder virtuellen Computer der ausgewählten Rollen auf dem die Sammlung auf jedem virtuellen Computer ausführen und die gesammelten Dateien an angegebenen Azure-Konto senden "azurelogcollector" hinzugefügt.

## <a name="adding-as-a-service-extension"></a>Als eine Erweiterung hinzufügen
1. Führen Sie die Anleitungen zum Verbinden von Azure PowerShell mit Ihrem Abonnement.
2. Geben Sie die Service-Name, Slots, Rollen und Instanzen auf die Sie hinzufügen möchten und die Erweiterung "azurelogcollector" aktivieren.
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify the slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified the roles on which the extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"
3. Geben Sie den zusätzlichen Datenordner für die Dateien gesammelt werden sollen (dieser Schritt ist optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > Sie können Token `%roleroot%` Stammlaufwerk der Rolle an, da es kein festes Laufwerk verwendet wird nicht.
   > 
   > 
4. Geben Sie den Azure Storage-Kontoname und der Schlüssel, die auf die gesammelte Dateien hochgeladen werden.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. Rufen Sie wie folgt die setazureservicelogcollector. ps1 (enthalten am Ende des Artikels), um die Erweiterung "azurelogcollector" für einen Cloud-Dienst zu aktivieren. Wenn die Ausführung abgeschlossen ist, finden Sie die hochgeladene Datei unter`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

Im folgenden ist die Definition der Parameter an das Skript übergeben. (Dies wird unten ebenfalls kopiert.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
      [Parameter(Mandatory=$true)]
    [string]   $ServiceName,

    [Parameter(Mandatory=$false)]
    [string[]] $Roles ,

    [Parameter(Mandatory=$false)]
    [string[]] $Instances,

    [Parameter(Mandatory=$false)]
    [string]   $Slot = 'Production',

    [Parameter(Mandatory=$false)]
    [string]   $Mode = 'Full',

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountName,

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountKey,

    [Parameter(Mandatory=$false)]
    [PSObject[]] $AdditionDataLocationList = $null
    )

* *ServiceName*: den Namen des Cloud-Diensts.
* *Rollen*: eine Liste der Rollen, z. B. "WebRole1" oder "WorkerRole1".
* *Instanzen*: eine Liste mit den Namen der Rolleninstanzen, die durch Komma - getrennt verwenden Sie das Platzhalterzeichen ("*") für alle Rolleninstanzen.
* *Slot*: Slot-Name. "Produktion" oder "Staging".
* *Modus*: Sammlungsmodus. "Full" oder "GA".
* *%Storageaccountname*: Name des Azure-Speicherkonto zum Speichern von gesammelten Daten.
* *StorageAccountKey*: Name des Azure-speicherkontoschlüssel.
* *AdditionalDataLocationList*: eine Liste der folgenden Struktur:
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a>Als VM-Erweiterung hinzufügen
Führen Sie die Anleitungen zum Verbinden von Azure PowerShell mit Ihrem Abonnement.

1. Geben Sie den Dienstnamen, VM und den Sammlungsmodus an.
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify the VM name
        $VMName = "'YourVMName'"
   
        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify the additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. Geben Sie den Azure Storage-Kontoname und der Schlüssel, die auf die gesammelte Dateien hochgeladen werden.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. Rufen Sie wie folgt die setazurevmlogcollector. ps1 (enthalten am Ende des Artikels), um die Erweiterung "azurelogcollector" für einen Cloud-Dienst zu aktivieren. Wenn die Ausführung abgeschlossen ist, finden Sie die hochgeladene Datei unter https://YouareStorageAccountName.BLOB.Core.Windows.NET/vmlogs

Im folgenden ist die Definition der Parameter an das Skript übergeben. (Dies wird unten ebenfalls kopiert.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
        [Parameter(Mandatory=$true)]
      [string]   $ServiceName,

      [Parameter(Mandatory=$false)]
      [string] $VMName ,

        [Parameter(Mandatory=$false)]
      [string]   $Mode = 'Full',

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountName,

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountKey,

      [Parameter(Mandatory=$false)]
      [PSObject[]] $AdditionDataLocationList = $null
      )

* Dienstname: Ihre Cloud-Dienstname.
* Der Name des virtuellen Computers %vmname.
* Modus: Sammlungsmodus. "Full" oder "GA".
* %Storageaccountname: Name des Azure-Speicherkonto zum Speichern von gesammelten Daten.
* StorageAccountKey: Name des Azure-speicherkontoschlüssel.
* AdditionalDataLocationList: Eine Liste der folgenden Struktur:

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a>Erweiterung des PowerShell-Skriptdateien
Setazureservicelogcollector. ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Roles ,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Instances = '*',

                  [Parameter(Mandatory=$false)]
                  [string]   $Slot = 'Production',

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
    {
        $instanceText = $Instances[0]
        for ($i = 1;$i -lt $Instances.Count;$i++)
        {
              $instanceText = $instanceText+ "," + $Instances[$i]
          }
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
    }
    else  #For all instances if not specified.  The value should be a space or *
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
    }

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need to get the Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json
    "publicConfig is:  $publicConfigJSON"

    #we just provide a empty privateConfig object
    $privateconfig = "{
    }"

    if ($Roles -ne $null)
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }
    else
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }

    #
    #This is an optional step: generate a sasUri to the container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"


Setazurevmlogcollector. ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string] $VMName ,

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need to get the Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
    #
    $privateconfig = "{
    }"

    if ($VMName -ne $null )
    {
          $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
          $VM.VM.OSVirtualHardDisk.OS

          if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
          {
                Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

                #
                #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
                #the presence of SubstatusList field.
                #
                $Completed = $false
                while ($Completed -ne $true)
                {
                        $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                        $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                        if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                        {
                            Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                              $Completed = $true
                        }
                        elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                        {
                              $Completed = $false
                              Write-Output "Waiting for operation to complete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove the extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, the extension cannot be enabled"
    }

## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt überprüfen oder Ihre Protokolle von einem sehr einfachen Speicherort kopieren.

