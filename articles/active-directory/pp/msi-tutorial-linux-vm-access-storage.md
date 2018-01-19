---
title: Zugreifen auf Azure Storage mithilfe einer vom Benutzer zugewiesenen MSI auf einer Linux-VM
description: "In diesem Tutorial wird erläutert, wie Sie eine vom Benutzer zugewiesene verwaltete Dienstidentität (Managed Service Identity, MSI) auf einer Linux-VM verwenden, um auf Azure Storage zuzugreifen."
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: arluca
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 91fe06825d1db586b715617241b0ca39115414c0
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-storage"></a>Zugreifen auf Azure Storage mithilfe einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI) auf einer Linux-VM

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Linux-Computer erstellen, verwenden und anschließend damit auf Azure Storage zuzugreifen. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer vom Benutzer zugewiesenen verwalteten Dienstidentität (MSI)
> * Zuweisen der vom Benutzer zugewiesenen MSI zu einem virtuellen Linux-Computer
> * Gewähren des Zugriffs auf eine Azure Storage-Instanz für die MSI
> * Abrufen eines Zugriffstokens mithilfe der vom Benutzer zugewiesenen MSI und Verwenden dieses Zugriffstokens für den Zugriff auf Azure Storage

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Für die Ausführung der CLI-Beispielskripts in diesem Tutorial stehen Ihnen zwei Möglichkeiten zur Verfügung:

- Verwenden Sie [Azure Cloud Shell](~/articles/cloud-shell/overview.md) entweder vom Azure-Portal aus oder über die Schaltfläche „Ausprobieren“, die sich in der oberen rechten Ecke eines jeden Codeblocks befindet.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Linux-Computers in einer neuen Ressourcengruppe

Erstellen Sie zunächst eine neue Linux-VM. Sie können die MSI auf Wunsch auch auf einer vorhandenen VM aktivieren.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+/Neuen Dienst erstellen**.
2. Wählen Sie **Compute** und dann **Ubuntu Server 16.04 LTS**.
3. Geben Sie die Informationen zum virtuellen Computer ein. Wählen Sie unter **Authentifizierungstyp** die Option **Öffentlicher SSH-Schlüssel** oder **Kennwort**. Mit den erstellten Anmeldeinformationen können Sie sich auf dem virtuellen Computer anmelden.

    ![Alternativer Bildtext](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Wählen Sie in der Dropdownliste ein **Abonnement** für den virtuellen Computer aus.
5. Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen** aus. Klicken Sie zum Abschluss auf **OK**.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter für den unterstützten Datenträgertyp, um weitere Größen anzuzeigen. Behalten Sie auf dem Blatt „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

## <a name="create-a-user-assigned-msi"></a>Erstellen einer vom Benutzer zugewiesenen MSI

1. Wenn Sie (anstelle einer Azure Cloud Shell-Sitzung) die CLI-Konsole verwenden, melden Sie sich zunächst bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie die neue MSI erstellen möchten:

    ```azurecli
    az login
    ```

2. Erstellen Sie mit [az identity create](/cli/azure/identity#az_identity_create) eine vom Benutzer zugewiesene MSI. Der Parameter `-g` gibt die Ressourcengruppe an, in der die MSI erstellt wurde, während der Parameter `-n` den jeweiligen Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<MSI NAME>` durch Ihre eigenen Werte:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Die Antwort enthält Details zu der erstellten vom Benutzer zugewiesenen MSI, ähnlich wie im folgenden Beispiel dargestellt wird. Notieren Sie sich den Wert `id` Ihrer MSI, da er im nächsten Schritt verwendet wird:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "9012",
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

## <a name="create-a-storage-account"></a>Speicherkonto erstellen 

Erstellen Sie jetzt ein Speicherkonto, wenn Sie noch keines besitzen. Sie können diesen Schritt auch überspringen und falls gewünscht ein vorhandenes Speicherkonto verwenden. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+/Neuen Dienst erstellen**.
2. Klicken Sie auf **Speicher** und dann auf **Speicherkonto**. Anschließend wird ein neuer Bereich namens „Speicherkonto erstellen“ angezeigt.
3. Geben Sie einen **Namen** für das Speicherkonto ein, das Sie später verwenden werden.  
4. **Bereitstellungsmodell** und **Kontoart** sollten jeweils auf „Resource Manager“ und „Allgemein“ festgelegt werden. 
5. Stellen Sie sicher, dass **Abonnement** und **Ressourcengruppe** dem entsprechen, was Sie bei der Erstellung Ihrer VM im vorherigen Schritt angegeben haben.
6. Klicken Sie auf **Create**.

    ![Erstellen eines neuen Speicherkontos](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Erstellen eines Blob-Containers im Speicherkonto

Da Dateien Blob Storage erfordern, müssen wir einen Blobcontainer erstellen, in dem die Datei gespeichert wird. Anschließend laden Sie eine Datei in den Blobcontainer im neuen Speicherkonto hoch und davon herunter.

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.
2. Klicken Sie auf der linken Seite unter „Blob-Dienst“ auf den Link **Container**.
3. Klicken Sie oben auf der Seite auf **+ Container**, und ein Bereich namens „Neuer Container“ wird geöffnet.
4. Geben Sie dem Container einen Namen, wählen Sie eine Zugriffsebene aus, und klicken Sie dann auf **OK**. Der angegebene Name wird später auch in diesem Tutorial verwendet. 

    ![Erstellen eines Speichercontainers](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Laden Sie eine Datei in den neu erstellten Container hoch, indem Sie auf den Containernamen und auf **Hochladen** klicken. Danach wählen Sie eine Datei aus und klicken auf **Hochladen**.

    ![Hochladen einer Textdatei](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-user-assigned-msi-access-to-an-azure-storage-container"></a>Gewähren von Zugriff auf einen Azure Storage-Container für die vom Benutzer zugewiesene MSI

Mithilfe einer MSI kann der Code Zugriffstoken zur Authentifizierung von Ressourcen abrufen, die die Azure AD-Authentifizierung unterstützen. In diesem Tutorial verwenden Sie Azure Storage.

Zuerst gewähren Sie der MSI Zugriff auf einen Azure Storage-Container. In diesem Fall verwenden Sie den zuvor erstellten Container. Aktualisieren Sie abhängig von Ihrer Umgebung die Werte für `<MSI PRINCIPALID>`, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>` und `<CONTAINER NAME>`. Ersetzen Sie `<CLIENT ID>` durch die Eigenschaft `clientId`, die unter [Erstellen einer vom Benutzer zugewiesenen MSI](#create-a-user-assigned-msi) vom Befehl `az identity create` zurückgegeben wurde:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role ‘Reader’ --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/blobServices/default/containers/<CONTAINER NAME>"
```

Die Antwort enthält Details zu der erstellten Rollenzuweisung:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.storage/storageAccounts/<STORAGE ACCOUNT NAME>/blogServices/default/<CONTAINER NAME>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-user-assigned-msis-identity-and-use-it-to-call-azure-storage"></a>Abrufen eines Zugriffstokens mithilfe der vom Benutzer zugewiesenen MSI und Verwenden dieses Zugriffstokens für den Aufruf von Azure Storage

Für den Rest des Tutorials müssen Sie von der VM aus arbeiten, die wir zuvor erstellt haben.

Zum Ausführen dieser Schritte benötigen Sie einen SSH-Client. Wenn Sie Windows verwenden, können Sie den SSH-Client im [Windows-Subsystem für Linux](https://msdn.microsoft.com/commandline/wsl/about) verwenden. Wenn Sie Hilfe beim Konfigurieren der SSH-Clientschlüssel benötigen, finden Sie unter [Verwenden von SSH-Schlüsseln mit Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) oder [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md) weitere Informationen.

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, wechseln Sie zu Ihrem virtuellen Linux-Computer, und klicken Sie dann oben auf der Seite **Übersicht** auf **Verbinden**. Kopieren Sie die Zeichenfolge, um eine Verbindung mit Ihrem virtuellen Computer herzustellen.
2. **Verbinden** Sie den virtuellen Computer mit dem gewünschten SSH-Client. 
3. Übermitteln Sie im Terminalfenster mit der cURL eine Anforderung an den lokalen MSI-Endpunkt zum Abrufen eines Zugriffstokens für Azure Storage.

   Im folgenden Beispiel wird die cURL-Anforderung zum Abrufen eines Zugriffstokens angezeigt. Ersetzen Sie `<CLIENT ID>` durch die Eigenschaft `clientId`, die unter [Erstellen einer vom Benutzer zugewiesenen MSI](#create-a-user-assigned-msi) vom Befehl `az identity create` zurückgegeben wurde:

   ```bash
   curl -H Metadata:true "http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fstorage.azure.com/&client_id=<CLIENT ID>"
   ```

   > [!NOTE]
   > In der Anforderung oben muss der Wert des `resource`-Parameters genau mit dem von Azure AD erwarteten Wert übereinstimmen. Wenn Sie die Azure Storage-Ressourcen-ID verwenden, müssen Sie den nachgestellten Schrägstrich im URI verwenden.
   > In der folgenden Antwort wurde das access_token-Element aus Gründen der Übersichtlichkeit gekürzt.

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
   "refresh_token":"",
   "expires_in":"3599",
   "expires_on":"1504130527",
   "not_before":"1504126627",
   "resource":"https://storage.azure.com",
   "token_type":"Bearer"}
   ```

4. Greifen Sie nun mithilfe des Zugriffstokens auf Azure Storage zu, z.B. zum Lesen der Inhalte der Beispieldatei, die Sie zuvor in den Container hochgeladen haben. Ersetzen Sie `<STORAGE ACCOUNT>`, `<CONTAINER NAME>` und `<FILE NAME>` durch die Werte, die Sie zuvor angegeben haben, und `<ACCESS TOKEN>` durch den im vorherigen Schritt zurückgegebenen Token.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME>?api-version=2017-11-09 -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Die Antwort enthält den Inhalt der Datei:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Wenn Sie dieses gleiche Tutorial mit SAS-Speicheranmeldeinformationen ausführen möchten, lesen Sie [Verwenden einer Linux-VM-MSI für den Zugriff auf Azure Storage über SAS-Anmeldeinformationen](msi-tutorial-linux-vm-access-storage-sas.md).
- Weitere Informationen zum SAS-Feature des Azure Storage-Kontos finden Sie hier:
  - [Verwenden von Shared Access Signatures (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Constructing a Service SAS (Erstellen einer Dienstebenen-SAS)](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.





