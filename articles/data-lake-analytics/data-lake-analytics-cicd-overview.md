---
title: Einrichten einer CI/CD-Pipeline für Azure Data Lake Analytics | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Continuous Integration und Continuous Deployment für Azure Data Lake Analytics einrichten.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a55c8fd95409de4fb1ea725d234de907f79d3c7b
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37890843"
---
# <a name="how-to-set-up-cicd-pipeline-for-azure-data-lake-analytics"></a>Einrichten einer CI/CD-Pipeline für Azure Data Lake Analytics

In diesem Dokument erfahren Sie, wie Sie eine CI/CD-Pipeline für U-SQL-Aufträge und U-SQL-Datenbanken einrichten.

## <a name="cicd-for-u-sql-job"></a>CI/CD für U-SQL-Aufträge

Azure Data Lake Tools für Visual Studio umfasst den U-SQL-Projekttyp, mit dem Sie U-SQL-Skripts organisieren können. Wenn Sie den U-SQL-Code mithilfe des U-SQL-Projekts verwalten, gestalten sich die weiteren CI/CD-Szenarien einfach.

## <a name="build-u-sql-project"></a>Erstellen des U-SQL-Projekts

Das U-SQL-Projekt kann mit MSBuild erstellt werden, indem die entsprechenden Parameter übergeben werden. Führen Sie die folgenden Schritte aus, um den Buildprozess für U-SQL-Projekte einzurichten.

### <a name="project-migration"></a>Projektmigration

Vergewissern Sie sich vor dem Einrichten der Buildaufgabe für das U-SQL-Projekt, dass Sie die neueste Version des U-SQL-Projekts verwenden. Öffnen Sie die U-SQL-Projektdatei im Editor, und überprüfen Sie, ob die folgenden Importelemente vorhanden sind:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Wenn dies nicht der Fall ist, stehen zwei Optionen zur Verfügung, um das Projekt zu migrieren:

- Option 1: Ändern des alten in das oben aufgeführte Importelement.
- Option 2: Öffnen des alten Projekts in Azure Data Lake Tools für Visual Studio nach Version 2.3.3000.0. Die alte Projektvorlage wird automatisch auf die neueste Version aktualisiert. Im neu erstellten Projekt nach Version 2.3.3000.0 wird die neue Vorlage direkt verwendet.

### <a name="get-nuget-package"></a>Abrufen des NuGet-Pakets

MSBuild enthält keine integrierte Unterstützung für den U-SQL-Projekttyp. Um diese Funktion hinzuzufügen, benötigen Sie einen Verweis für Ihre Projektmappe im [Microsoft.Azure.DataLake.USQL.SDK-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), mit dem der erforderliche Sprachdienst hinzugefügt wird.

Um den NuGet-Paketverweis hinzuzufügen, können Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe klicken und dann **NuGet-Pakete verwalten** auswählen. Sie können auch die Datei `packages.config` im Projektmappenordner auswählen und in dieser Datei die folgenden Inhalte hinzufügen.

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>Verwalten von U-SQL-Datenbankverweisen

Wenn die U-SQL-Skripts im U-SQL-Projekt Abfrageanweisungen für U-SQL-Datenbankobjekte enthalten, z.B. Abfrage einer U-SQL-Tabelle oder Verweis auf eine Assembly, müssen Sie vor dem Erstellen des U-SQL-Projekts auf das entsprechende U-SQL-Datenbankprojekt verweisen, das die Definition dieser Objekte enthält.

[Weitere Informationen zum U-SQL-Datenbankprojekt](data-lake-analytics-data-lake-tools-develop-usql-database.md)

### <a name="build-u-sql-project-with-msbuild-command-line"></a>Erstellen des U-SQL-Projekts über die MSBuild-Befehlszeile

Nach dem Migrieren des Projekts und dem Abrufen des NuGet-Pakets können Sie die MSBuild-Standardbefehlszeile mit den folgenden zusätzlichen Argumenten aufrufen, um das U-SQL-Projekt zu erstellen:

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder
``` 

Die Definition und die Werte der Argumente lauten wie folgt:

* USQLSDKPath=<U-SQL-NuGet-Paket>\build\runtime: Dieser Parameter verweist auf den Installationspfad des NuGet-Pakets für den U-SQL-Sprachdienst.
* USQLTargetType=Merge oder SyntaxCheck:
    * Merge: Im Merge-Modus werden CodeBehind-Dateien, z.B CS-, PY- und R-Dateien, kompiliert und die resultierende benutzerdefinierte Codebibliothek (z.B. eine DLL-Binärdatei, Python oder R-Code) in das U-SQL-Skript aufgenommen.
    * SyntaxCheck: Im SyntaxCheck-Modus werden zunächst CodeBehind-Dateien im U-SQL-Skript zusammengeführt und dann das U-SQL-Skript kompiliert, um den Code zu überprüfen.
* DataRoot=<DataRoot path>: DataRoot wird nur für den SyntaxCheck-Modus benötigt. Beim Erstellen des Skripts mit dem SyntaxCheck-Modus überprüft MSBuild die Verweise im Skript auf Datenbankobjekte. Stellen Sie sicher, dass Sie vor dem Erstellen eine entsprechende lokale Umgebung, die die referenzierten Objekte aus der U-SQL-Datenbank enthält, im DataRoot-Ordner des Buildcomputers einrichten. Sie können diese Datenbankabhängigkeiten auch durch [Verweisen auf ein U-SQL-Datenbankprojekt](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project) verwalten. Beachten Sie, dass MSBuild nur Verweise auf Datenbankobjekte und keine Dateien überprüft.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Continuous Integration in Visual Studio Team Service

Neben der Befehlszeile können Kunden auch den Visual Studio-Build oder die MSBuild-Aufgabe verwenden, um U-SQL-Projekte in Visual Studio Team Service zu erstellen. Stellen Sie beim Einrichten der Buildaufgabe Folgendes sicher:

1.  Fügen Sie die NuGet-Wiederherstellungsaufgabe hinzu, um das NuGet-Paket mit der referenzierten Projektmappe einschließlich `Azure.DataLake.USQL.SDK` abzurufen, sodass MSBuild die Ziele der U-SQL-Sprache finden kann. 

    ![Data Lake, Festlegen der CI/CD-MSBuild-Aufgabe für das U-SQL-Projekt](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

2.  Legen Sie MSBuild-Argumente fest. Sie können die Argumente im Visual Studio-Build oder der MSBuild-Aufgabe wie unten gezeigt festlegen, oder Sie können Variablen für diese Argumente in der VSTS-Builddefinition definieren.

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/<your project name>/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.1019-preview/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory)
    ```

    ![Data Lake, Festlegen der CI/CD-MSBuild-Variablen für das U-SQL-Projekt](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

### <a name="u-sql-project-build-output"></a>Buildausgabe für das U-SQL-Projekt

Nach dem Ausführen des Builds werden alle Skripts im U-SQL-Projekt erstellt und in einer ZIP-Datei mit dem Namen `USQLProjectName.usqlpack` ausgegeben. Die Ordnerstruktur in Ihrem Projekt wird in der ZIP-Buildausgabe beibehalten.

>[!NOTE]
>
>Die CodeBehind-Datei für jedes U-SQL-Skript wird als Inlineanweisung in der Buildausgabe des Skripts zusammengeführt.
>

## <a name="test-u-sql-script"></a>Testen des U-SQL-Skripts

Azure Data Lake enthält Testprojekte für das U-SQL-Skript und C#-UDO/UDAG/UDF:
* [Informationen zum Hinzufügen von Testfällen für das U-SQL-Skript und den erweiterten C#-Code](data-lake-analytics-cicd-test.md#test-u-sql-scripts)
* [Informationen zum Ausführen dieser Testfälle in Visual Studio Team Service](data-lake-analytics-cicd-test.md#run-test-cases-in-visual-studio-team-service)

## <a name="u-sql-job-deployment"></a>Bereitstellung von U-SQL-Aufträgen

Nach dem Überprüfen des Codes über den Build- und Testprozess können Sie U-SQL-Aufträge direkt aus Visual Studio Team Service über die **Azure PowerShell-Aufgabe** übermitteln. Sie können das Skript zudem in Azure Data Lake Store und Azure Blob Storage bereitstellen und [die geplanten Aufträge über Azure Data Factory ausführen](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-visual-studio-team-service"></a>Übermitteln von U-SQL-Aufträgen über Visual Studio Team Service

Die Buildausgabe des U-SQL-Projekts ist eine ZIP-Datei mit dem Namen **USQLProjectName.usqlpack**. Sie enthält alle U-SQL-Skripts im Projekt. Sie können die [Azure PowerShell-Aufgabe in Visual Studio Team Service](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) mit dem folgenden PowerShell-Beispielskript verwenden, um die U-SQL Aufträge direkt aus dem Visual Studio Team Service-Build oder der Releasepipeline zu übermitteln.

```powershell
param(
    [Parameter(Mandatory=$true)][string]$AnalyticsAccountName, #ADLA account name
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, #Root folder (e.g. artifacts root folder)
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
        # [System.IO.Compression.ZipFile]::ExtractToDirectory($USQLPackfile, $UnzipOutput, 0)
        # $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
        # Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue | Where-Object {$_.DirectoryName -match $subFolder}

    return $USQLFiles
}

Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."
    # submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "($usqlFiles.IndexOf())Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzureRmDataLakeAnalyticsJob -Account $AnalyticsAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        LogJobInformation $jobToSubmit
        
        Write-Output "waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzureRmDataLakeAnalyticsJob -Account $AnalyticsAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
        
        # ProcessResult $jobResult
    }
}

Function ProcessResult($jobResult)
{
    if ($jobResult.Result -eq "Failed")
    {
        Write-Error "Job Failed. Job Id: $($jobResult.JobId), Job Name: $($jobResult.Name), Log: $($jobResult.LogFolder)"
    }
    else
    {
        Write-Output "Job Succeeded. Job Id: $($jobResult.JobId), Job Name: $($jobResult.Name)"
    }
}

Function LogJobInformation($jobInfo)
{
    Write-Output "************************************************************************"
    Write-Output ([string]::Format("Job Id: {0}", $(DefaultIfNull $jobInfo.JobId)))
    Write-Output ([string]::Format("Job Name: {0}", $(DefaultIfNull $jobInfo.Name)))
    Write-Output ([string]::Format("Job State: {0}", $(DefaultIfNull $jobInfo.State)))
    Write-Output ([string]::Format("Job Started at: {0}", $(DefaultIfNull  $jobInfo.StartTime)))
    Write-Output ([string]::Format("Job Ended at: {0}", $(DefaultIfNull  $jobInfo.EndTime)))
    Write-Output ([string]::Format("Job Result: {0}", $(DefaultIfNull $jobInfo.Result)))
    Write-Output "************************************************************************"
}

Function DefaultIfNull($item)
{
    if ($item -ne $null)
    {
        return $item
    }
    return ""
}

Function Main()
{

    Write-Output "Starting USQL script deployment..."

    # Submit ADLA jobs with usql scripts in given sub-folder.
    # Order is important here. Scripts with least dependency goes first followed 
    # by scripts which more dependencies.
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>Bereitstellen von U-SQL-Aufträgen über Azure Data Factory

Neben der Übermittlung von U-SQL-Aufträgen direkt aus Visual Studio Team Service können Sie die erstellten Skripts auch in Azure Data Lake Store und Azure Blob Storage hochladen und [die geplanten Aufträge über Azure Data Factory ausführen](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

Verwenden Sie die [Azure PowerShell-Aufgabe in Visual Studio Team Service](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) mit dem folgenden PowerShell-Beispielskript, um die U-SQL-Skripts in das Azure Data Lake Store-Konto hochzuladen.

```powershell
param(
    [Parameter(Mandatory=$true)][string]$ADLSName, #ADLA account name
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot #Root folder (e.g. artifacts root folder)
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading DLL files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles
    Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $usqlScripts.FullName -Destination "/ScriptResource2/$usqlScripts" -Force -Recurse
    # $files = @(get-childitem $usqlScripts -recurse)
    # foreach($file in $files)
    # {
    #    Write-Host "Uploading file: $($file.Name)"
    #    Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/ScriptResource/$file" -Force -Recurse
    # }
}

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    return $UnzipOutput
}

UploadResources
```

## <a name="cicd-for-u-sql-database"></a>CI/CD für U-SQL-Datenbanken

Azure Data Lake Tools für Visual Studio enthält eine Vorlage für das U-SQL-Datenbankprojekt, mit der Entwickler die U-SQL-Datenbanken schnell und einfach entwickeln, verwalten und bereitstellen können. [Erfahren Sie mehr über das U-SQL-Datenbankprojekt](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="build-u-sql-database-project"></a>Erstellen des U-SQL-Datenbankprojekts

### <a name="get-nuget-package"></a>Abrufen des NuGet-Pakets

MSBuild enthält keine integrierte Unterstützung für den U-SQL-Datenbankprojekttyp. Um diese Funktion hinzuzufügen, benötigen Sie einen Verweis für Ihre Projektmappe im [Microsoft.Azure.DataLake.USQL.SDK-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), mit dem der erforderliche Sprachdienst hinzugefügt wird.

Um den NuGet-Paketverweis hinzuzufügen, können Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe klicken und dann für diese **NuGet-Pakete verwalten** auswählen. Suchen und installieren Sie anschließend das NuGet-Paket. Sie können auch die Datei „packages.config“ im Projektmappenordner auswählen und in dieser Datei die folgenden Inhalte hinzufügen.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-database-project-with-msbuild-command-line"></a>Erstellen des U-SQL-Datenbankprojekts über die MSBuild-Befehlszeile

Sie können die MSBuild-Standardbefehlszeile aufrufen und den Verweis auf das U-SQL SDK-NuGet-Paket wie unten gezeigt als zusätzliches Argument übergeben, um das U-SQL-Datenbankprojekt zu erstellen:

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

Das Argument `USQLSDKPath=<U-SQL Nuget package>\build\runtime` verweist auf den Installationspfad des NuGet-Pakets für den U-SQL-Sprachdienst.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Continuous Integration in Visual Studio Team Service

Neben der Befehlszeile können Kunden auch den **Visual Studio-Build** oder die **MSBuild-Aufgabe** verwenden, um U-SQL-Datenbankprojekte in Visual Studio Team Service zu erstellen. Stellen Sie beim Einrichten der Buildaufgabe Folgendes sicher:

1.  Fügen Sie die NuGet-Wiederherstellungsaufgabe hinzu, um das NuGet-Paket mit der referenzierten Projektmappe einschließlich `Azure.DataLake.USQL.SDK` abzurufen, sodass MSBuild die Ziele der U-SQL-Sprache finden kann. 

    ![Data Lake, Festlegen der CI/CD-MSBuild-Aufgabe für das U-SQL-Datenbankprojekt](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

2.  Legen Sie MSBuild-Argumente fest. Sie können die Argumente im Visual Studio-Build oder der MSBuild-Aufgabe wie unten gezeigt festlegen, oder Sie können Variablen für diese Argumente in der VSTS-Builddefinition definieren.

```
/p:USQLSDKPath=$(Build.SourcesDirectory)/<your project name>/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.1019-preview/build/runtime
```

![Data Lake, Festlegen der CI/CD-MSBuild-Variablen für das U-SQL-Datenbankprojekt](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

### <a name="u-sql-database-project-build-output"></a>Buildausgabe für das U-SQL-Datenbankprojekt

Die Buildausgabe für das U-SQL-Datenbankprojekt ist ein U-SQL-Datenbankbereitstellungspaket, das mit dem Suffix `.usqldbpack` benannt ist. Das `.usqldbpack`-Paket ist eine ZIP-Datei und enthält alle DDL-Anweisungen in einem einzelnen U-SQL-Skript im Ordner „DDL“ sowie alle DLLs und zusätzlichen Dateien für Assemblys im Ordner „Temp“.

## <a name="test-table-valued-function-and-stored-procedure"></a>Testen von Tabellenwertfunktionen und gespeicherten Prozeduren

Derzeit wird das direkte Hinzufügen von Testfällen für Tabellenwertfunktionen und gespeicherte Prozeduren nicht unterstützt. Um dies zu umgehen, können Sie ein U-SQL-Projekt erstellen, das U-SQL-Skripts zum Aufrufen dieser Funktionen enthält, und Testfälle für die Funktionen schreiben. Führen Sie die folgenden Schritte aus, um Testfälle für Tabellenwertfunktionen und gespeicherte Prozeduren einzurichten, die im U-SQL-Datenbankprojekt definiert sind:

1.  Erstellen Sie ein U-SQL-Projekt für Testzwecke, und schreiben Sie U-SQL-Skripts, die die Tabellenwertfunktionen und gespeicherten Prozeduren aufrufen.
2.  Fügen Sie einen Datenbankverweis auf dieses U-SQL-Projekt hinzu. Um die Definition der Tabellenwertfunktionen und gespeicherten Prozeduren abzurufen, müssen Sie auf das Datenbankprojekt verweisen, das die DDL-Anweisung enthält. [Erfahren Sie mehr über den Datenbankverweis](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).
3.  Fügen Sie Testfälle für die U-SQL-Skripts hinzu, die Tabellenwertfunktionen und gespeicherte Prozeduren aufrufen. [Informationen zum Hinzufügen von Testfällen für ein U-SQL-Skript](data-lake-analytics-cicd-test.md#test-u-sql-scripts).

## <a name="deploy-u-sql-database-through-visual-studio-team-service"></a>Bereitstellen einer U-SQL-Datenbank über Visual Studio Team Service

`PackageDeploymentTool.exe` enthält die Programmier- und Befehlszeilenschnittstellen, über die das U-SQL-Datenbankbereitstellungspaket (.usqldbpack) bereitgestellt werden kann. Das SDK ist im [U-SQL SDK-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) enthalten und befindet sich unter „build/runtime/PackageDeploymentTool.exe“. Durch Verwenden von `PackageDeploymentTool.exe` können Sie U-SQL-Datenbanken im Azure Data Lake Analytics-Konto sowie im lokalen Konto bereitstellen.

>[!NOTE]
>
>Die Unterstützung für die Bereitstellung von U-SQL-Datenbanken über die PowerShell-Befehlszeile und den Visual Studio Team Service-Freigabetask wird derzeit entwickelt.
>

Führen Sie die folgenden Schritte aus, um die Aufgabe für die Datenbankbereitstellung in Visual Studio Team Service einzurichten:

1. Fügen Sie eine PowerShell-Skript-Aufgabe im Build oder der Releasepipeline hinzu, und führen Sie das folgende PowerShell-Skript aus. Mit dieser Aufgabe können Azure SDK-Abhängigkeiten für `PackageDeploymentTool.exe` abgerufen werden. Sie können den Parameter „-outputfolder“ festlegen, um diese Abhängigkeiten in einen bestimmten Ordner zu laden. Übergeben Sie diesen Ordnerpfad an `PackageDeploymentTool.exe` in Schritt 2. 

    ```powershell
    param (
        [string]$outputfolder = "RequiredDll",
        [string]$workingfolder = ""
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.2.3-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.3.3-preview -outf Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.3.3-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force

    echo "Copy required DLLs to output folder..."

    mkdir $outputfolder -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview\lib\net452\*.dll $outputfolder
    copy Microsoft.Azure.Management.DataLake.Store.2.3.3-preview\lib\net452\*.dll $outputfolder
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $outputfolder
    ```

2. Fügen Sie eine **Befehlszeilenaufgabe** im Build oder der Releasepipeline hinzu, und füllen Sie das Skript aus, das `PackageDeploymentTool.exe` aufruft. Das Beispielskript sieht wie folgt aus: 

    * Lokales Bereitstellen der U-SQL-Datenbank:

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Verwenden der interaktiven Authentifizierung zum Bereitstellen der U-SQL-Datenbank im Azure Data Lake Analytics-Konto:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Verwenden der Authentifizierung über geheimen Schlüssel zum Bereitstellen der U-SQL-Datenbank im Azure Data Lake Analytics-Konto:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete>
        ```

    * Verwenden der Authentifizierung über Zertifikatsdatei zum Bereitstellen der U-SQL-Datenbank im Azure Data Lake Analytics-Konto:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

**Beschreibung der Parameter in „PackageDeploymentTool.exe“:**

**Allgemeine Parameter:**

|Parameter|BESCHREIBUNG|Standardwert|Erforderlich|
|---------|-----------|-------------|--------|
|Paket|Pfad des bereitzustellenden U-SQL-Datenbankbereitstellungspakets|null|true|
|Datenbank|Name der Datenbank, in der die Bereitstellung erfolgt oder die erstellt wird|master|false|
|LogFile|Pfad der Datei für die Protokollierung, standardmäßig in der Standardausgabe (Konsole)|null|false|
|LogLevel|Protokollebene: Ausführlich, Normal, Warnung, Fehler|LogLevel.Normal|false|

**Parameter für die lokale Bereitstellung:**

|Parameter|BESCHREIBUNG|Standardwert|Erforderlich|
|---------|-----------|-------------|--------|
|DataRoot|Pfad des lokalen Datenstammordners|null|true|

**Parameter für die Azure Data Lake Analytics-Bereitstellung:**

|Parameter|BESCHREIBUNG|Standardwert|Erforderlich|
|---------|-----------|-------------|--------|
|Konto|Gibt die Bereitstellung im jeweiligen Azure Data Lake Analytics-Konto nach Kontoname an|null|true|
|ResourceGroup|Azure-Ressourcengruppenname für das Azure Data Lake Analytics-Konto|null|true|
|SubscriptionId|Azure-Abonnement-ID für das Azure Data Lake Analytics-Konto|null|true|
|Tenant|Mandantenname (Domänenname des AAD-Verzeichnisses, den Sie auf der Seite für die Abonnementverwaltung im Azure-Portal finden)|null|true|
|AzureSDKPath|Pfad zum Suchen von abhängigen Assemblys in Azure SDK|null|true|
|Interactive|Verwenden oder Nichtverwenden des interaktiven Modus für die Authentifizierung|false|false|
|ClientId|AAD-Anwendungs-ID für die nicht interaktive Authentifizierung, erforderlich für die nicht interaktive Authentifizierung|null|Erforderlich für die nicht interaktive Authentifizierung|
|Geheimnis|Geheimnis/Kennwort für die nicht interaktive Authentifizierung, nur in vertrauenswürdiger/sicherer Umgebung zu verwenden|null|Erforderlich für die nicht interaktive Authentifizierung (oder Verwenden von „SecreteFile“)|
|SecreteFile|Geheimnis/Kennwort für die nicht interaktive Authentifizierung wird in der Datei gespeichert. Stellen Sie sicher, dass sie nur für den aktuellen Benutzer lesbar ist.|null|Erforderlich für die nicht interaktive Authentifizierung (oder Verwenden von „Secrete“)|
|CertFile|X.509-Zertifizierung für die nicht interaktive Authentifizierung wird in der Datei gespeichert. Standardmäßig wird die Authentifizierung über geheimen Clientschlüssel verwendet.|null|false|
|JobPrefix|Präfix für die Datenbankbereitstellung des U-SQL-DDL-Auftrags|Deploy_ und DateTime.Now|false|

## <a name="next-steps"></a>Nächste Schritte

- [Testen des Azure Data Lake Analytics-Codes](data-lake-analytics-cicd-test.md)
- [Ausführen von U-SQL-Skripts auf dem lokalen Computer](data-lake-analytics-data-lake-tools-local-run.md)
- [Entwickeln einer U-SQL-Datenbank mithilfe eines U-SQL-Datenbankprojekts](data-lake-analytics-data-lake-tools-develop-usql-database.md)