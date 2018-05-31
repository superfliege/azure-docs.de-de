---
title: Verwenden einer Linux-VM-MSI für den Zugriff auf Azure Cosmos DB
description: In diesem Tutorial wird erläutert, wie Sie eine vom System zugewiesene verwaltete Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Linux-Computer verwenden, um auf Azure Cosmos DB zuzugreifen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: skwan
ms.openlocfilehash: bcbafcb5b72fd156f0d8b4a4ddd52aab1d699996
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076254"
---
# <a name="use-a-linux-vm-msi-to-access-azure-cosmos-db"></a>Verwenden einer Linux-VM-MSI für den Zugriff auf Azure Cosmos DB 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


In diesem Tutorial erfahren Sie, wie Sie eine Linux-VM-MSI erstellen und verwenden. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Linux-Computers mit MSI-Aktivierung
> * Erstellen eines Cosmos DB-Kontos
> * Erstellen einer Sammlung im Cosmos DB-Konto
> * Gewähren des Zugriffs für die MSI auf eine Azure Cosmos DB-Instanz
> * Abrufen der `principalID` der MSI des virtuellen Linux-Computers
> * Abrufen eines Zugriffstokens und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager
> * Abrufen von Zugriffsschlüsseln aus Azure Resource Manager für Cosmos DB-Aufrufe

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com), bevor Sie fortfahren.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Für die Ausführung der CLI-Beispielskripts in diesem Tutorial stehen Ihnen zwei Möglichkeiten zur Verfügung:

- Verwenden Sie [Azure Cloud Shell](~/articles/cloud-shell/overview.md) entweder vom Azure-Portal aus oder über die Schaltfläche **Ausprobieren**, die sich in der rechten oberen Ecke eines jeden Codeblocks befindet.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Linux-Computers in einer neuen Ressourcengruppe

In diesem Tutorial erstellen Sie einen neuen MSI-fähigen virtuellen Linux-Computer.

So erstellen Sie einen MSI-fähigen virtuellen Computer:

1. Melden Sie sich bei Verwendung der Azure CLI in einer lokalen Konsole zunächst mit [az login](/cli/azure/reference-index#az_login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. Erstellen Sie eine [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen des virtuellen Computers und der zugehörigen Ressourcen. Verwenden Sie hierfür [az group create](/cli/azure/group/#az_group_create). Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Erstellen Sie einen virtuellen Computer mit [az vm create](/cli/azure/vm/#az_vm_create). Im folgenden Beispiel wird ein virtueller Computer mit dem Namen *myVM* mit einer MSI erstellt, wie vom `--assign-identity`-Parameter angefordert. Der `--admin-username`-Parameter und der `--admin-password`-Parameter geben den Namen und das Kennwort des Administratorbenutzers für die Anmeldung am virtuellen Computer an. Aktualisieren Sie diese Werte ggf. mit den entsprechenden Werten für Ihre Umgebung: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

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

## <a name="retrieve-the-principalid-of-the-linux-vms-msi"></a>Abrufen der `principalID` der MSI des virtuellen Linux-Computers

Um im folgenden Abschnitt über den Ressourcen-Manager auf die Zugriffsschlüssel des Cosmos DB-Kontos zugreifen zu können, müssen Sie die `principalID` der MSI des virtuellen Linux-Computers abrufen.  Vergessen Sie nicht, die Parameterwerte für `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (Ressourcengruppe, in der sich Ihr virtueller Computer befindet) und `<VM NAME>` durch Ihre eigenen Werte zu ersetzen.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
Die Antwort enthält die Details der vom System zugewiesenen MSI (notieren Sie die im nächsten Abschnitt verwendete principalID):

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Gewähren von Zugriff auf die Zugriffsschlüssel des Cosmos DB-Kontos für die Linux-VM-MSI

Cosmos DB unterstützt die Azure AD-Authentifizierung nicht nativ. Sie können eine MSI jedoch zum Abrufen eines Cosmos DB-Zugriffsschlüssels aus Resource Manager verwenden und mithilfe des Schlüssels auf Cosmos DB zugreifen. In diesem Schritt gewähren Sie der MSI Zugriff auf die Schlüssel des Cosmos DB-Kontos.

Um der MSI-Identität Zugriff auf das Cosmos DB-Konto in Azure Resource Manager über die Azure-Befehlszeilenschnittstelle zu gewähren, aktualisieren Sie die Werte für `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` und `<COSMOS DB ACCOUNT NAME>` für Ihre Umgebung. Ersetzen Sie `<MSI PRINCIPALID>` durch die Eigenschaft `principalId`, die durch Ausführen des Befehls `az resource show` unter [Abrufen der principalID der MSI des virtuellen Linux-Computers](#retrieve-the-principalID-of-the-linux-VM's-MSI) zurückgegeben wird.  Cosmos DB unterstützt beim Verwenden von Zugriffsschlüsseln zwei Granularitätsebenen: Lese-/Schreibzugriff auf das Konto und schreibgeschützter Zugriff auf das Konto.  Weisen Sie die Rolle `DocumentDB Account Contributor` zu, wenn Sie die Schlüssel für Lese-/Schreibzugriff für das Konto abrufen möchten, oder weisen Sie die Rolle `Cosmos DB Account Reader Role` zu, wenn Sie die Schlüssel für schreibgeschützten Zugriff auf das Konto abrufen möchten:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

Die Antwort enthält Details zu der erstellten Rollenzuweisung:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>/providers/Microsoft.Authorization/roleAssignments/5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "name": "5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "properties": {
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-linux-vms-msi-and-use-it-to-call-azure-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der MSI des virtuellen Linux-Computers und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager

Für den Rest des Tutorials arbeiten wir von dem zuvor erstellten virtuellen Computer aus.

Zum Ausführen dieser Schritte benötigen Sie einen SSH-Client. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/install_guide) verwenden. Wenn Sie Hilfe beim Konfigurieren der SSH-Clientschlüssel benötigen, lesen Sie die Informationen unter [Vorgehensweise: Verwenden von SSH-Schlüsseln mit Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md) oder [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, wechseln Sie zu Ihrem virtuellen Linux-Computer, und klicken Sie dann oben auf der Seite **Übersicht** auf **Verbinden**. Kopieren Sie die Zeichenfolge, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. 
2. Stellen Sie mit Ihrem SSH-Client eine Verbindung mit Ihrem virtuellen Computer her.  
3. Als Nächstes werden Sie aufgefordert, das **Kennwort** einzugeben, das Sie hinzugefügt haben, als Sie den **virtuellen Linux-Computer** erstellt haben. Anschließend sollten Sie erfolgreich angemeldet werden.  
4. Verwenden Sie cURL, um ein Zugriffstoken für Azure Resource Manager abzurufen: 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > In der Anforderung oben muss der Wert des resource-Parameters genau mit dem von Azure AD erwarteten Wert übereinstimmen. Wenn Sie die Azure Resource Manager-Ressourcen-ID verwenden, müssen Sie den nachgestellten Schrägstrich im URI verwenden.
    > In der folgenden Antwort wurde das access_token-Element aus Gründen der Übersichtlichkeit gekürzt.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Abrufen von Zugriffsschlüsseln aus Azure Resource Manager für Cosmos DB-Aufrufe  

Nun rufen Sie mit cURL und dem Zugriffstoken, das im vorherigen Abschnitt abgerufen wurde, Resource Manager auf, um den Zugriffsschlüssel für Cosmos DB abzurufen. Nachdem wir den Zugriffsschlüssel haben, können wir Cosmos DB abfragen. Ersetzen Sie die Parameter `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, und `<COSMOS DB ACCOUNT NAME>` durch Ihre eigenen Werte. Ersetzen Sie den Wert `<ACCESS TOKEN>` durch das Zugriffstoken, das Sie zuvor abgerufen haben.  Wenn Sie Schlüssel für den Lese-/Schreibzugriff abrufen möchten, verwenden Sie den Vorgangstyp `listKeys`.  Wenn Sie Schlüssel für den schreibgeschützten Zugriff abrufen möchten, verwenden Sie den Vorgangstyp `readonlykeys`:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> Für den Text in der URL oben wird zwischen Groß-/Kleinschreibung unterschieden. Wenn Sie also für Ihre Ressourcengruppen Groß-/Kleinschreibung verwenden, achten Sie darauf, die richtige Schreibweise zu verwenden. Wichtig ist darüber hinaus die Tatsache, dass es sich um eine POST-Anforderung und nicht um eine GET-Anforderung handelt. Stellen Sie sicher, dass Sie mit -d einen Wert zum Erfassen einer Längenbeschränkung übergeben, der NULL sein kann.  

Die cURL-Antwort stellt die Liste der Schlüssel bereit.  Angenommen, Sie erhalten Schlüssel für den schreibgeschützten Zugriff:  

```bash 
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

- Eine Übersicht über MSI finden Sie unter [Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen](overview.md).

