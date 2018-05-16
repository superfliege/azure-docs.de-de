---
title: Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Cosmos DB
description: In diesem Tutorial wird erläutert, wie Sie eine vom System zugewiesene verwaltete Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Windows-Computer verwenden, um auf Azure Cosmos DB zuzugreifen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: arluca
ms.openlocfilehash: 0352a64597442d0d4fe53b2d2d5adec5c25d47e7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="use-a-windows-vm-msi-to-access-azure-cosmos-db"></a>Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Cosmos DB

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine Windows-VM-MSI für den Zugriff auf Cosmos DB erstellen und verwenden. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines MSI-fähigen virtuellen Windows-Computers 
> * Erstellen eines Cosmos DB-Kontos
> * Gewähren von Zugriff auf die Zugriffsschlüssel des Cosmos DB-Kontos für die Windows-VM-MSI
> * Abrufen eines Zugriffstokens mithilfe der MSI des virtuellen Windows-Computers zum Aufrufen von Azure Resource Manager
> * Abrufen von Zugriffsschlüsseln aus Azure Resource Manager für Cosmos DB-Aufrufe

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Windows-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Windows-Computer erstellt.  Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. 
3. Geben Sie die Informationen zum virtuellen Computer ein. Der **Benutzername** und das **Kennwort**, die hier erstellt werden, sind die Anmeldeinformationen, die Sie für die Anmeldung beim virtuellen Computer verwenden.
4. Wählen Sie in der Dropdownliste das richtige **Abonnement** für den virtuellen Computer aus.
5. Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen**. Klicken Sie zum Abschluss auf **OK**.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie auf der Seite „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

   ![Alternativer Bildtext](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer 

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne Anmeldeinformationen in Ihren Code einfügen zu müssen. Im Hintergrund werden durch das Aktivieren der MSI auf einem virtuellen Computer über das Azure-Portal zwei Vorgänge ausgelöst: Der virtuelle Computer wird bei Azure AD registriert, um eine verwaltete Identität zu erstellen, und die Identität wird auf dem virtuellen Computer konfiguriert.

1. Wählen Sie den **virtuellen Computer** aus, auf dem Sie MSI aktivieren möchten.  
2. Klicken Sie in der links angezeigten Navigationsleiste auf **Konfiguration**. 
3. Die Option **Verwaltete Dienstidenität** wird angezeigt. Wählen Sie zum Registrieren und Aktivieren von MSI die Option **Ja** oder zum Deaktivieren „Nein“. 
4. Achten Sie darauf, zum Speichern der Konfiguration auf **Speichern** zu klicken.  
   ![Alternativer Bildtext](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-cosmos-db-account"></a>Erstellen eines Cosmos DB-Kontos 

Erstellen Sie ein Cosmos DB-Konto, wenn Sie noch keines besitzen. Sie können diesen Schritt überspringen und ein vorhandenes Cosmos DB-Konto verwenden. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+/Neuen Dienst erstellen**.
2. Klicken Sie auf **Datenbanken** und dann auf **Azure Cosmos DB**. Es wird ein Bereich „Neues Konto“ angezeigt.
3. Geben Sie eine **ID** für das Cosmos DB-Konto ein, das Sie später verwenden.  
4. **API** sollte auf „SQL“ festgelegt werden. Der in diesem Tutorial beschriebene Ansatz kann auch mit den anderen verfügbaren API-Typen verwendet werden, die Schritte in diesem Tutorial gelten jedoch für die SQL-API.
5. Stellen Sie sicher, dass **Abonnement** und **Ressourcengruppe** dem entsprechen, was Sie bei der Erstellung Ihrer VM im vorherigen Schritt angegeben haben.  Wählen Sie einen **Standort** aus, an dem Cosmos DB verfügbar ist.
6. Klicken Sie auf **Create**.

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Erstellen einer Sammlung im Cosmos DB-Konto

Als Nächstes fügen Sie im Cosmos DB-Konto eine Datensammlung hinzu, die Sie in späteren Schritten abfragen können.

1. Navigieren Sie zu Ihrem neu erstellten Cosmos DB-Konto.
2. Klicken Sie auf der Registerkarte **Übersicht** auf die Schaltfläche **+ Sammlung hinzufügen**. Daraufhin wird der Bereich „Sammlung hinzufügen“ eingeblendet.
3. Weisen Sie der Sammlung eine Datenbank-ID und eine Sammlungs-ID zu, wählen Sie eine Speicherkapazität aus, geben Sie einen Partitionsschlüssel und einen Durchsatzwert ein, und klicken Sie dann auf **OK**.  Für dieses Tutorial ist es ausreichend, als Datenbank-ID und Sammlungs-ID „Test“ zu verwenden und eine feste Speicherkapazität sowie den niedrigsten Durchsatz (400 RU/s) auszuwählen.  

## <a name="grant-windows-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Gewähren von Zugriff auf die Zugriffsschlüssel des Cosmos DB-Kontos für die Windows-VM-MSI

Cosmos DB unterstützt die Azure AD-Authentifizierung nicht nativ. Sie können eine MSI jedoch zum Abrufen eines Cosmos DB-Zugriffsschlüssels aus Resource Manager verwenden und mithilfe des Schlüssels auf Cosmos DB zugreifen. In diesem Schritt gewähren Sie der MSI Zugriff auf die Schlüssel des Cosmos DB-Kontos.

Um der MSI-Identität Zugriff auf das Cosmos DB-Konto in Azure Resource Manager mithilfe von PowerShell zu gewähren, aktualisieren Sie die Werte für `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` und `<COSMOS DB ACCOUNT NAME>` für Ihre Umgebung. Ersetzen Sie `<MSI PRINCIPALID>` durch die Eigenschaft `principalId`, die durch Ausführen des Befehls `az resource show` unter [Abrufen der principalID der MSI des virtuellen Linux-Computers](#retrieve-the-principalID-of-the-linux-VM's-MSI) zurückgegeben wird.  Cosmos DB unterstützt beim Verwenden von Zugriffsschlüsseln zwei Granularitätsebenen: Lese-/Schreibzugriff auf das Konto und schreibgeschützter Zugriff auf das Konto.  Weisen Sie die Rolle `DocumentDB Account Contributor` zu, wenn Sie die Schlüssel für Lese-/Schreibzugriff für das Konto abrufen möchten, oder weisen Sie die Rolle `Cosmos DB Account Reader Role` zu, wenn Sie die Schlüssel für schreibgeschützten Zugriff auf das Konto abrufen möchten:

```azurepowershell
$spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
```

## <a name="get-an-access-token-using-the-windows-vms-msi-to-call-azure-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der MSI des virtuellen Windows-Computers zum Aufrufen von Azure Resource Manager

Für den Rest des Tutorials arbeiten wir von dem virtuellen Computer aus, den wir zuvor erstellt haben. 

In diesem Abschnitt müssen Sie PowerShell-Cmdlets von Azure Resource Manager verwenden.  Wenn Sie das Programm nicht installiert haben, [laden Sie die neuesten Version herunter](https://docs.microsoft.com/powershell/azure/overview), bevor Sie fortfahren.

Auch müssen Sie die neueste Version von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) auf Ihrem virtuellen Windows-Computer installieren.

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, wechseln Sie zu Ihrem virtuellen Windows-Computer, und klicken Sie dann oben auf der Seite **Übersicht** auf **Verbinden**. 
2. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des virtuellen Windows-Computers hinzugefügt haben. 
3. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt PowerShell in der Remotesitzung.
4. Erstellen Sie mithilfe des PowerShell-Befehls „Invoke-WebRequest“ eine Anforderung an den lokalen MSI-Endpunkt, um ein Zugriffstoken für Azure Resource Manager zu erhalten.

    ```powershell
        $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```

    > [!NOTE]
    > Der Wert des „resource“-Parameters muss exakt mit dem von Azure AD erwarteten Wert übereinstimmen. Wenn Sie die Azure Resource Manager-Ressourcen-ID verwenden, müssen Sie den nachgestellten Schrägstrich im URI verwenden.
    
    Extrahieren Sie im nächsten Schritt das Element „Content“, das als JSON-formatierte (JavaScript Object Notation) Zeichenfolge im $response-Objekt gespeichert ist. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Extrahieren Sie dann das Zugriffstoken aus der Antwort.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Abrufen von Zugriffsschlüsseln aus Azure Resource Manager für Cosmos DB-Aufrufe

Nun rufen Sie mit PowerShell und dem Zugriffstoken, das im vorherigen Abschnitt abgerufen wurde, Resource Manager auf, um den Zugriffsschlüssel für Cosmos DB abzurufen. Nachdem wir den Zugriffsschlüssel haben, können wir Cosmos DB abfragen. Ersetzen Sie die Parameter `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, und `<COSMOS DB ACCOUNT NAME>` durch Ihre eigenen Werte. Ersetzen Sie den Wert `<ACCESS TOKEN>` durch das Zugriffstoken, das Sie zuvor abgerufen haben.  Wenn Sie Schlüssel für den Lese-/Schreibzugriff abrufen möchten, verwenden Sie den Vorgangstyp `listKeys`.  Wenn Sie Schlüssel für den schreibgeschützten Zugriff abrufen möchten, verwenden Sie den Vorgangstyp `readonlykeys`:

```powershell
Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
In der Antwort wird die Liste der Schlüssel zurückgegeben.  Angenommen, Sie erhalten Schlüssel für den schreibgeschützten Zugriff:

```powershell
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```
Nachdem Sie nun über den Zugriffsschlüssel für das Cosmos DB-Konto verfügen, können Sie ihn an ein Cosmos DB SDK übergeben und Aufrufe für den Zugriff auf das Konto durchführen.  Für ein kurzes Beispiel können Sie den Zugriffsschlüssel an die Azure-Befehlszeilenschnittstelle übergeben.  Sie erhalten die <COSMOS DB CONNECTION URL> von der Registerkarte **Übersicht** auf dem Blatt des Cosmos DB-Kontos im Azure-Portal.  Ersetzen Sie <ACCESS KEY> durch den Wert, den Sie oben abgerufen haben:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Mit diesem CLI-Befehl werden Details zur Sammlung zurückgegeben:

```bash
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](overview.md).
