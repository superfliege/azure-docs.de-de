---
title: Kopieren von Daten aus SQL Server in Blob Storage mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der selbstgehosteten Integration Runtime in Azure Data Factory Daten aus einem lokalen Datenspeicher in die Azure-Cloud kopieren.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: afd7735712d03110a67509a7e94d336219a65b34
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="tutorial-copy-data-from-on-premises-sql-server-to-azure-blob-storage"></a>Tutorial: Kopieren von Daten aus einer lokalen SQL Server-Instanz in Azure Blob Storage
In diesem Tutorial verwenden Sie Azure PowerShell, um eine Data Factory-Pipeline zu erstellen, die Daten aus einer lokalen SQL Server-Datenbank in einen Azure Blob Storage verschiebt. Sie erstellen und verwenden eine selbstgehostete Integration Runtime (IR) von Azure Data Factory, die eine Integration von lokalen Datenspeichern und Clouddatenspeichern ermöglicht.  Informationen zur Verwendung von anderen Tools/SDKs zum Erstellen einer Data Factory finden Sie unter [Schnellstarts](quickstart-create-data-factory-dot-net.md).

Dieser Artikel enthält keine ausführliche Einführung in den Data Factory-Dienst. Eine Einführung in den Azure Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](introduction.md). 

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Tutorial: Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) weiter.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine selbstgehostete Integration Runtime.
> * Erstellen von verknüpften SQL Server- und Azure Storage-Diensten 
> * Erstellen von SQL Server- und Azure Blob-Datasets
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipelineausführung.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="sql-server-2014-or-2016"></a>SQL Server 2014 oder 2016. 
In diesem Tutorial verwenden Sie eine lokale SQL Server-Datenbank als **Quelldatenspeicher**. Erstellen Sie eine Tabelle mit dem Namen **emp** in Ihrer SQL Server-Datenbank, und fügen Sie einige Einträge in die Tabelle ein.

1. Starten Sie **SQL Server Management Studio**. Bei Verwendung von SQL Server 2016 müssen Sie SQL Server Management Studio unter Umständen separat aus dem [Download Center](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms) installieren. 
2. Stellen Sie eine Verbindung mit Ihrer SQL Server-Instanz her, indem Sie Ihre Anmeldeinformationen verwenden. 
3. Erstellen Sie eine Beispieldatenbank. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf **Datenbanken**, und klicken Sie anschließend auf **Neue Datenbank**. Geben Sie im Dialogfeld **Neue Datenbank** einen **Namen** für die Datenbank ein, und klicken Sie auf **OK**. 
4. Führen Sie das folgende Abfrageskript für die Datenbank aus, um die Tabelle **emp** zu erstellen. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf die von Ihnen erstellte **Datenbank**, und klicken Sie auf **Neue Abfrage**. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Führen Sie die folgenden Befehle für die Datenbank aus, um einige Beispieldaten in die Tabelle einzufügen:

    ```sql
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="azure-storage-account"></a>Azure Storage-Konto
Sie verwenden in diesem Tutorial ein Azure Storage-Allzweckkonto (Blob Storage) als **Ziel/Senke**-Datenspeicher. Falls Sie noch nicht über ein Azure Storage-Allzweckkonto verfügen, helfen Ihnen die Informationen unter [Erstellen Sie ein Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) weiter.

#### <a name="get-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Schnellstart verwenden Sie Name und Schlüssel Ihres Azure Storage-Kontos. Das folgende Verfahren enthält die Schritte zum Abrufen des Namens und Schlüssels für Ihr Speicherkonto. 

1. Starten Sie einen Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com). Melden Sie sich mit Ihrem Azure-Benutzernamen und -Kennwort an. 
2. Klicken Sie im Menü auf der linken Seite auf **Weitere Dienste >**, filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie **Speicherkonten**.

    ![Suchen nach einem Speicherkonto](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. Filtern Sie in der Liste mit den Speicherkonten nach Ihrem Speicherkonto (falls erforderlich), und wählen Sie **Ihr Speicherkonto** aus. 
4. Wählen Sie auf der Seite **Speicherkonto** im Menü die Option **Zugriffsschlüssel**.

    ![Abrufen des Speicherkontonamens und -schlüssels](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. Kopieren Sie die Werte für die Felder **Speicherkontoname** und **key1** in die Zwischenablage. Fügen Sie sie in einen Editor ein, und speichern Sie sie. In diesem Tutorial verwenden Sie den Speicherkontonamen und -schlüssel. 

#### <a name="create-the-adftutorial-container"></a>Erstellen des Containers „adftutorial“ 
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen „adftutorial“ in Ihrem Azure-Blobspeicher. 

1. Installieren Sie den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), falls er auf Ihrem Computer noch nicht vorhanden ist. 
2. Starten Sie den **Microsoft Azure Storage-Explorer** auf Ihrem Computer.   
3. Wählen Sie im Fenster **Connect to Azure Storage** (Verbindung mit Azure Storage herstellen) die Option **Use a storage account name and key** (Einen Speicherkontonamen und -schlüssel verwenden), und klicken Sie auf **Weiter**. Falls das Fenster **Connect to Azure Storage** (Verbindung mit Azure Storage herstellen) nicht angezeigt wird, können Sie in der Strukturansicht mit der rechten Maustaste auf **Speicherkonten** und anschließend auf **Connect to Azure storage** (Verbindung mit Azure Storage herstellen) klicken. 

    ![Herstellen einer Verbindung mit Azure Storage](media/tutorial-hybrid-copy-powershell/storage-explorer-connect-azure-storage.png)
4. Fügen Sie im Fenster **Attach using Name and Key** (Mit Name und Schlüssel anfügen) den **Kontonamen** und **Kontoschlüssel** aus dem vorherigen Schritt ein. Klicken Sie auf **Weiter**. 
5. Klicken Sie im Fenster **Verbindungszusammenfassung** auf **Verbinden**.
6. Vergewissern Sie sich, dass Ihr Speicherkonto in der Strukturansicht unter **(Local and Attached) ((Lokal und angefügt))** -> **Speicherkonten** angezeigt wird. 
7. Erweitern Sie **BLOB-Container**, und stellen Sie sicher, dass kein Blobcontainer **adftutorial** vorhanden ist. Sie können die nächsten Schritte zum Erstellen des Containers überspringen, falls er bereits vorhanden sein sollte. 
8. Klicken Sie mit der rechten Maustaste auf **BLOB-Container**, und wählen Sie **BLOB-Container erstellen**.

    ![Erstellen eines Blobcontainers](media/tutorial-hybrid-copy-powershell/stroage-explorer-create-blob-container-menu.png)
9. Geben Sie als Name **adftutorial** ein, und drücken Sie die **EINGABETASTE**. 
10. Vergewissern Sie sich, dass der Container **adftutorial** in der Strukturansicht ausgewählt ist. Data Factory erstellt den Ausgabeordner automatisch in diesem Container, damit Sie ihn nicht selbst erstellen müssen. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Installieren von Azure Powershell
Installieren Sie die neueste Version von Azure PowerShell, falls sie auf Ihrem Computer noch nicht vorhanden ist. 

1. Navigieren Sie in Ihrem Webbrowser zur Seite mit den [Azure SDK-Downloads und SDKs](https://azure.microsoft.com/downloads/). 
2. Klicken Sie unter **Befehlszeilentools** -> **PowerShell** auf **Windows-Installation**. 
3. Führen Sie die **MSI**-Datei aus, um Azure PowerShell zu installieren. 

Ausführliche Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Anmelden an Azure PowerShell
Starten Sie **PowerShell** auf Ihrem Computer. Lassen Sie Azure PowerShell bis zum Ende dieser Schnellstartanleitung geöffnet. Wenn Sie PowerShell schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.

1. Führen Sie den folgenden Befehl aus, und geben Sie den Azure-Benutzernamen und das Kennwort ein, den bzw. das Sie bei der Anmeldung beim Azure-Portal verwendet haben:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Führen Sie den folgenden Befehl aus, um alle Abonnements für dieses Konto anzuzeigen, wenn Sie über mehrere Azure-Abonnements verfügen:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Führen Sie den folgenden Befehl aus, um das gewünschte Abonnement auszuwählen: Ersetzen Sie **SubscriptionId** durch die ID Ihres Azure-Abonnements:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Definieren Sie eine Variable für den Ressourcengruppennamen zur späteren Verwendung in PowerShell-Befehlen. Kopieren Sie den folgenden Befehlstext nach PowerShell, geben Sie einen Namen für die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in doppelten Anführungszeichen an, und führen Sie dann den Befehl aus. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Definieren Sie eine Variable für den Data Factory-Namen zur späteren Verwendung in PowerShell-Befehlen. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Definieren Sie eine Variable für den Speicherort der Data Factory: 

    ```powershell
    $location = "East US"
    ```
4. Führen Sie den folgenden Befehl aus, um die Azure-Ressourcengruppe zu erstellen: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$resourceGroupName` einen anderen Wert zu, und versuchen Sie es noch einmal. Fahren Sie mit dem nächsten Schritt fort, wenn Sie die Ressourcengruppe für andere Benutzer freigeben möchten.  
5. Führen Sie zum Erstellen der Data Factory das folgende **Set-AzureRmDataFactoryV2**-Cmdlet aus: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Beachten Sie folgende Punkte:

* Der Name der Azure Data Factory muss global eindeutig sein. Wenn die folgende Fehlermeldung angezeigt wird, ändern Sie den Namen, und wiederholen Sie den Vorgang.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Um Data Factory-Instanzen zu erstellen, müssen Sie **Mitwirkender** oder **Administrator** des Azure-Abonnements sein.
* Derzeit können Sie mit Data Factory Version 2 nur in den Regionen „USA, Osten“, „USA, Osten 2“ und „Europa, Westen“ Data Factorys erstellen. Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.

## <a name="create-a-self-hosted-ir"></a>Erstellen einer selbstgehosteten IR

In diesem Abschnitt erstellen Sie eine selbstgehostete Integration Runtime und ordnen sie einem lokalen Knoten (Computer) zu.

1. Erstellen Sie eine Variable für den Namen der Integration Runtime. 

    ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
    ```
1. Erstellen Sie eine selbstgehostete Integration Runtime. Verwenden Sie einen eindeutigen Namen. Es darf keine andere Integration Runtime mit dem gleichen Namen vorhanden sein.

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Hier ist die Beispielausgabe:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Führen Sie den folgenden Befehl aus, um den Status der erstellten Integration Runtime abzurufen.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Hier ist die Beispielausgabe:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Führen Sie den folgenden Befehl aus, um Authentifizierungsschlüssel abzurufen, mit denen Sie die selbstgehostete Integration Runtime beim Data Factory-Dienst in der Cloud registrieren können. Kopieren Sie einen der Schlüssel zum Registrieren der selbstgehosteten Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Hier ist die Beispielausgabe:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

## <a name="install-integration-runtime"></a>Installieren der Integration Runtime
1. Führen Sie den [Download](https://www.microsoft.com/download/details.aspx?id=39717) der selbstgehosteten Integration Runtime auf einen lokalen Computer durch, und führen Sie die Installation aus. 
2. Klicken Sie auf der **Willkommensseite des Einrichtungsassistenten für die Microsoft Integration Runtime** auf **Weiter**.  
3. Stimmen Sie auf der Seite mit dem **Endbenutzer-Lizenzvertrag** den Bedingungen bzw. dem Lizenzvertrag zu, und klicken Sie auf **Weiter**. 
4. Klicken Sie auf der Seite **Zielordner** auf **Weiter**. 
5. Klicken Sie unter **Ready to install Microsoft Integration Runtime** (Bereit für Installation der Microsoft Integration Runtime) auf **Installieren**. 
6. Klicken Sie auf **OK**, wenn eine Warnmeldung mit dem Hinweis angezeigt wird, dass der zu konfigurierende Computer in den Energiesparmodus oder den Ruhezustand versetzt wird, falls er nicht verwendet wird. 
7. Klicken Sie auf der Seite **Completed the Microsoft Integration Runtime Setup Wizard** (Einrichtungs-Assistent für Microsoft Integration Runtime abgeschlossen) auf **Fertig stellen**.
8. Fügen Sie auf der Seite **Integrationslaufzeit (selbstgehostet) registrieren** den Schlüssel ein, den Sie im vorherigen Abschnitt gespeichert haben, und klicken Sie auf **Registrieren**. 

   ![Registrieren der Integration Runtime](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)
2. Folgende Meldung wird angezeigt, wenn die selbstgehostete Integration Runtime erfolgreich registriert wurde:

   ![Erfolgreich registriert](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

3. Klicken Sie auf der Seite **Neuer Knoten der Integrationslaufzeit (selbstgehostet)** auf **Weiter**. 

    ![Seite „Neuer Knoten der Integrationslaufzeit“](media/tutorial-hybrid-copy-powershell/new-integration-runtime-node-page.png)
4. Klicken Sie unter **Intranetkommunikationskanal** auf **Überspringen**. Sie können eine TLS/SSL-Zertifizierung wählen, um in einer Integration Runtime-Umgebung mit mehreren Knoten die Kommunikation zwischen Knoten zu schützen. 

    ![Seite „Intranetkommunikationskanal“](media/tutorial-hybrid-copy-powershell/intranet-communication-channel-page.png)
5. Klicken Sie auf der Seite **Integrationslaufzeit (selbstgehostet) registrieren** auf **Konfigurations-Manager starten**. 
6. Folgende Seite wird angezeigt, wenn der Knoten mit dem Clouddienst verbunden ist:

   ![Knoten ist verbunden](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Erstellen eines verknüpften Azure Storage-Diensts (Ziel/Senke)

1. Erstellen Sie eine JSON-Datei mit dem Namen **AzureStorageLinkedService.json** im Ordner **C:\ADFv2Tutorial** und dem folgenden Inhalt: Erstellen Sie den Ordner „ADFv2Tutorial“, wenn dieser noch nicht vorhanden ist.  

    > [!IMPORTANT]
    > Ersetzen Sie &lt;accountname&gt; und &lt;accountkey&gt; durch den Namen bzw. Schlüssel Ihres Azure Storage-Kontos, bevor Sie die Datei speichern.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```
2. Wechseln Sie in **Azure PowerShell** zum Ordner **ADFv2Tutorial**.

   Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2LinkedService** aus, um den verknüpften Dienst **AzureStorageLinkedService** zu erstellen. Die Cmdlets in diesem Tutorial akzeptieren Werte für die Parameter **ResourceGroupName** und **DataFactoryName**. Alternativ dazu können Sie auch das vom Cmdlet „Set-AzureRmDataFactoryV2“ zurückgegebene **DataFactory**-Objekt übergeben, ohne „ResourceGroupName“ und „DataFactoryName“ bei jeder Ausführung eines Cmdlets einzugeben.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Hier ist eine Beispielausgabe:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Erstellen und Verschlüsseln einen verknüpftes SQL Server-Diensts (Quelle)

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **SqlServerLinkedService.json** und dem folgenden Inhalt: Ersetzen Sie **&lt;servername>**, **&lt;databasename>**, **&lt;username>**, **&lt;servername>** und **&lt;password>** durch die Werte Ihrer SQL Server-Instanz, bevor Sie die Datei speichern. 

    > [!IMPORTANT]
    > Ersetzen Sie **&lt;integration** **runtime** **name>** durch den Namen Ihrer Integration Runtime.

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. Führen Sie **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** aus, und übergeben Sie die obige JSON-Nutzlast, um vertrauliche Daten aus der JSON-Nutzlast in der lokalen selbstgehosteten Integration Runtime zu verschlüsseln. Mit dieser Verschlüsselung wird sichergestellt, dass die Anmeldeinformationen per DPAPI (Data Protection Application Programming Interface) verschlüsselt werden. Die verschlüsselten Anmeldeinformationen werden lokal (lokaler Computer) auf dem selbstgehosteten Integration Runtime-Knoten gespeichert. Die Ausgabenutzlast kann an eine weitere JSON-Datei umgeleitet werden (in diesem Fall „encryptedLinkedService.json“), die verschlüsselte Anmeldeinformationen enthält.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Führen Sie den folgenden Befehl mithilfe der im vorherigen Schritt erstellten JSON-Datei aus, um den **SqlServerLinkedService** zu erstellen:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Schritt erstellen Sie die Eingabe- und Ausgabedatasets, die ein- und ausgehende Daten für den Kopiervorgang darstellen (lokale SQL Server-Datenbank = > Azure-Blobspeicher).

### <a name="create-a-dataset-for-source-sql-database"></a>Erstellen eines Datasets für die SQL-Quelldatenbank

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **SqlServerDataset.json** und dem folgenden Inhalt:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Dataset** aus, um das Dataset **SqlServerDataset** zu erstellen.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Erstellen eines Datasets für eine Azure Blob Storage-Senke

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **AzureBlobDataset.json** und dem folgenden Inhalt:

    > [!IMPORTANT]
    > Bei diesem Beispielcode wird davon ausgegangen, dass Sie im Azure Blob Storage über einen Container mit dem Namen **adftutorial** verfügen.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. Um das Dataset **AzureBlobDataset** zu erstellen, führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Dataset** aus.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>Erstellen von Pipelines

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>Erstellen der Pipeline „SqlServerToBlobPipeline“

1. Erstellen Sie im Ordner **C:\ADFv2Tutorial** eine JSON-Datei mit dem Namen **SqlServerToBlobPipeline.json** und dem folgenden Inhalt:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Pipeline** aus, um die Pipeline **SQLServerToBlobPipeline** zu erstellen.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>Starten und Überwachen einer Pipelineausführung

1. Starten Sie eine Pipelineausführung für die Pipeline „SQLServerToBlobPipeline“, und erfassen Sie die ID der Pipelineausführung für die zukünftige Überwachung.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Führen Sie das folgende Skript aus, um den Ausführungsstatus der Pipeline **SQLServerToBlobPipeline** kontinuierlich zu überprüfen und das Endergebnis auszugeben.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Hier ist die Ausgabe der Beispielausführung:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. Sie können die Ausführungs-ID der Pipeline **SQLServerToBlobPipeline** abrufen und das detaillierte Ergebnis der Aktivitätsausführung wie folgt überprüfen.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Hier ist die Ausgabe der Beispielausführung:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 3,
      "throughput": 0.01171875,
      "errors": [],
      "effectiveIntegrationRuntime": "MyIntegrationRuntime",
      "billedDuration": 3
    }
    ```
## <a name="verify-the-output"></a>Überprüfen der Ausgabe
Die Pipeline erstellt den Ausgabeordner `fromonprem` automatisch im Blobcontainer `adftutorial`. Vergewissern Sie sich, dass die Datei **dbo.emp.txt** im Ausgabeordner enthalten ist. Verwenden Sie den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), um sicherzustellen, dass die Ausgabe erstellt wurde. 

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in einem Azure Blob Storage von einem Speicherort in einen anderen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine selbstgehostete Integration Runtime.
> * Erstellen von verknüpften SQL Server- und Azure Storage-Diensten 
> * Erstellen von SQL Server- und Azure Blob-Datasets
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipelineausführung.

Eine Liste mit den von Azure Data Factory unterstützten Datenspeichern finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Fahren Sie mit dem folgenden Tutorial fort, um mehr über das Kopieren von Daten per Massenvorgang aus einer Quelle in ein Ziel zu erfahren:

> [!div class="nextstepaction"]
>[Massenkopieren von Daten](tutorial-bulk-copy.md)
