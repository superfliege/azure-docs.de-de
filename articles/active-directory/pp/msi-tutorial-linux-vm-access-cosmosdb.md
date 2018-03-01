---
title: "Zugreifen auf Azure Cosmos DB mithilfe einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) auf einer Linux-VM"
description: "In diesem Tutorial wird erläutert, wie Sie eine vom Benutzer zugewiesene verwaltete Dienstidentität (Managed Service Identity, MSI) auf einer Linux-VM verwenden, um auf Azure Cosmos DB zuzugreifen."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2018
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2c0c3597999e80af86f079385653d94ddfcab245
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-cosmos-db"></a>Zugreifen auf Azure Cosmos DB mithilfe einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) auf einer Linux-VM 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In diesem Tutorial erfahren Sie, wie Sie eine von einem Benutzer zugewiesene verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Linux-Computer erstellen, verwenden und anschließend damit auf Azure Cosmos DB zuzugreifen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI)
> * Zuweisen der vom Benutzer zugewiesenen MSI zu einem virtuellen Linux-Computer
> * Gewähren des Zugriffs für die MSI auf eine Azure Cosmos DB-Instanz
> * Abrufen eines Zugriffstokens mithilfe der vom Benutzer zugewiesenen MSI und Verwenden dieses Zugriffstokens für den Zugriff auf Azure Cosmos DB

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Für die Ausführung der CLI-Beispielskripts in diesem Tutorial stehen Ihnen zwei Möglichkeiten zur Verfügung:

- Verwenden Sie [Azure Cloud Shell](~/articles/cloud-shell/overview.md) entweder vom Azure-Portal aus oder über die Schaltfläche **Ausprobieren**, die sich in der rechten oberen Ecke eines jeden Codeblocks befindet.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Linux-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Linux-Computer erstellt. Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf **+ Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Ubuntu Server 16.04 LTS-VM** aus.
3. Geben Sie die Informationen zum virtuellen Computer ein. Wählen Sie unter **Authentifizierungstyp** die Option **Öffentlicher SSH-Schlüssel** oder **Kennwort**. Mit den erstellten Anmeldeinformationen können Sie sich auf dem virtuellen Computer anmelden.

    ![MSI für Linux-VM](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Wählen Sie in der Dropdownliste ein **Abonnement** für den virtuellen Computer aus.
5. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus, und geben Sie den Namen der Ressourcengruppe für diese VM ein. Klicken Sie zum Abschluss auf **OK**.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter für den unterstützten Datenträgertyp, um weitere Größen anzuzeigen. Behalten Sie auf dem Blatt **Einstellungen** die Standardwerte bei, und klicken Sie auf **OK**.

## <a name="create-a-user-assigned-msi"></a>Erstellen einer vom Benutzer zugewiesenen MSI

1. Wenn Sie (anstelle einer Azure Cloud Shell-Sitzung) die CLI-Konsole verwenden, melden Sie sich zunächst bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die neue MSI erstellen möchten:

    ```azurecli
    az login
    ```

2. Erstellen Sie mit [az identity create](/cli/azure/identity#az_identity_create) eine vom Benutzer zugewiesene MSI. Der Parameter `-g` gibt die Ressourcengruppe an, in der die MSI erstellt wurde, während der Parameter `-n` den jeweiligen Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<MSI NAME>` durch Ihre eigenen Werte:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Die Antwort enthält Details für die erstellte, vom Benutzer zugewiesene MSI, wie im folgenden Beispiel gezeigt (notieren Sie die Werte `clientId` und `id` für Ihre MSI, da diese in späteren Schritten verwendet werden):

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Zuweisen der vom Benutzer zugewiesenen MSI zu einer Linux-VM

Im Gegensatz zu einer vom System zugewiesenen MSI kann eine vom Benutzer zugewiesene MSI von Clients für mehrere Azure-Ressourcen verwendet werden. Im Rahmen dieses Tutorials weisen Sie diese einer einzelnen VM zu. Sie können sie auch mehreren VMs zuweisen.

Weisen Sie mit [az vm assign-identity](/cli/azure/vm#az_vm_assign_identity) die vom Benutzer zugewiesene MSI Ihrer Linux-VM zu. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<VM NAME>` durch Ihre eigenen Werte. Verwenden Sie die Eigenschaft `id`, die im vorherigen Schritt für den Parameterwert `--identities` zurückgegeben wird:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-cosmos-db-account"></a>Erstellen eines Cosmos DB-Kontos 

Erstellen Sie jetzt ein Cosmos DB-Konto, wenn Sie noch keines besitzen. Sie können diesen Schritt auch überspringen und falls gewünscht ein vorhandenes Cosmos DB-Konto verwenden. 

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

## <a name="grant-your-user-assigned-msi-access-to-the-cosmos-db-account-access-keys"></a>Gewähren von Zugriff auf die Zugriffsschlüssel des Cosmos DB-Kontos für die vom Benutzer zugewiesenen MSI

Cosmos DB unterstützt die Azure AD-Authentifizierung nicht nativ.  Sie können eine MSI jedoch zum Abrufen eines Cosmos DB-Zugriffsschlüssels aus Resource Manager verwenden und mithilfe des Schlüssels auf Cosmos DB zugreifen.  In diesem Schritt gewähren Sie der vom Benutzer zugewiesenen MSI Zugriff auf die Schlüssel des Cosmos DB-Kontos.

Gewähren Sie zunächst mithilfe der Azure-Befehlszeilenschnittstelle der MSI-Identität Zugriff auf das Cosmos DB-Konto in Azure Resource Manager. Aktualisieren Sie abhängig von Ihrer Umgebung die Werte für `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` und `<COSMOS DB ACCOUNT NAME>`. Ersetzen Sie `<MSI PRINCIPALID>` durch die `principalId`-Eigenschaft, die unter [Erstellen einer vom Benutzer zugewiesenen MSI](#create-a-user-assigned-msi) vom Befehl `az identity create` zurückgegeben wurde.  Cosmos DB unterstützt beim Verwenden von Zugriffsschlüsseln zwei Granularitätsebenen: Lese-/Schreibzugriff auf das Konto und schreibgeschützter Zugriff auf das Konto.  Weisen Sie die Rolle `DocumentDB Account Contributor` zu, wenn Sie die Schlüssel für Lese-/Schreibzugriff für das Konto abrufen möchten, oder weisen Sie die Rolle `Cosmos DB Account Reader Role` zu, wenn Sie die Schlüssel für schreibgeschützten Zugriff auf das Konto abrufen möchten:

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

## <a name="get-an-access-token-using-the-user-assigned-msi-and-use-it-to-call-azure-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der vom Benutzer zugewiesenen MSI und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager

Für den Rest des Tutorials arbeiten wir von dem zuvor erstellten virtuellen Computer aus.

Zum Ausführen dieser Schritte benötigen Sie einen SSH-Client. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/install_guide) verwenden. Wenn Sie Hilfe beim Konfigurieren der SSH-Clientschlüssel benötigen, finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md) oder [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md) weitere Informationen.

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, wechseln Sie zu Ihrem virtuellen Linux-Computer, und klicken Sie dann oben auf der Seite **Übersicht** auf **Verbinden**. Kopieren Sie die Zeichenfolge, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. 
2. Stellen Sie mit Ihrem SSH-Client eine Verbindung mit Ihrem virtuellen Computer her.  
3. Als Nächstes werden Sie aufgefordert, das **Kennwort** einzugeben, das Sie hinzugefügt haben, als Sie den **virtuellen Linux-Computer** erstellt haben. Anschließend sollten Sie erfolgreich angemeldet werden.  
4. Verwenden Sie CURL, um ein Zugriffstoken für Azure Resource Manager abzurufen.  

    Die cURL-Anforderung und -Antwort für das Zugriffstoken finden Sie unten.  Ersetzen Sie <CLIENT ID> durch den Wert von „clientId“ Ihrer vom Benutzer zugewiesenen MSI:
    
    ```bash
    curl 'http://localhost:50342/oauth2/token?resource=https://management.azure.com/&client_id=<CLIENT ID>' -H "Metadata:true"
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

- Eine Übersicht über MSI finden Sie unter [Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen](msi-overview.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.