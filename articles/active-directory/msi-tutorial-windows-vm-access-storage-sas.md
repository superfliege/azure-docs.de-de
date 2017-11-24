---
title: "Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Storage mit SAS-Anmeldeinformationen"
description: "Dieses Tutorial veranschaulicht, wie Sie mit einer Windows-VM-MSI (Managed Service Identität, verwaltete Dienstidentität) auf Azure Storage zugreifen und dabei SAS-Anmeldeinformationen anstelle des Zugriffsschlüssels eines Speicherkontos verwenden."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: bryanla
ms.openlocfilehash: f43ec5bdbf32c3f7c4c1fb3a5aae8367bd050fc9
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Storage mithilfe von SAS-Anmeldeinformationen

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Windows-Computer aktivieren und diese MSI anschließend verwenden, um SAS-Anmeldeinformationen (Shared Access Signature) für Storage zu erhalten. Im Speziellen geht es um [Anmeldeinformationen für eine Dienst-SAS](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Eine Dienst-SAS erlaubt für einen begrenzten Zeitraum den eingeschränkten Zugriff auf Objekte in einem Speicherkonto für einen bestimmten Dienst (in unserem Fall der Blob-Dienst), ohne dass dabei der Zugriffsschlüssel für das Konto offengelegt wird. Sie können SAS-Anmeldeinformationen wie gewohnt bei Speichervorgängen verwenden, z.B. bei der Verwendung des Storage SDK. Für dieses Tutorial veranschaulichen wir das Hoch- und Herunterladen eines Blobs mithilfe von Azure Storage PowerShell. Sie lernen Folgendes:


> [!div class="checklist"]
> * Aktivieren von MSI auf einem virtuellen Windows-Computer 
> * Gewähren des Zugriffs auf eine Speicherkonto-SAS für Ihren virtuellen Computer in Ressourcen-Manager 
> * Abrufen eines Zugriffstokens mithilfe der Identität Ihres virtuellen Computers und Verwenden dieses Zugriffstokens zum Abrufen der SAS von Ressourcen-Manager 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Windows-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Windows-Computer erstellt. Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+/Neuen Dienst erstellen**.
2.  Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. 
3.  Geben Sie die Informationen zum virtuellen Computer ein. Der **Benutzername** und das **Kennwort**, die hier erstellt werden, sind die Anmeldeinformationen, die Sie für die Anmeldung beim virtuellen Computer verwenden.
4.  Wählen Sie in der Dropdownliste das richtige **Abonnement** für den virtuellen Computer aus.
5.  Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen** aus. Klicken Sie zum Abschluss auf **OK**.
6.  Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie auf dem Blatt „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

    ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Im Hintergrund werden durch das Aktivieren von MSI zwei Vorgänge ausgelöst: Auf dem virtuellen Computer wird die MSI-VM-Erweiterung installiert, und MSI wird für den virtuellen Computer aktiviert.  

1. Navigieren Sie zu der Ressourcengruppe des neuen virtuellen Computers, und wählen Sie den virtuellen Computer aus, den Sie im vorherigen Schritt erstellt haben.
2. Klicken Sie in den „Einstellungen“ des virtuellen Computers auf der linken Seite auf **Konfiguration**.
3. Wählen Sie zum Registrieren und Aktivieren von MSI die Option **Ja** oder zum Deaktivieren „Nein“.
4. Achten Sie darauf, zum Speichern der Konfiguration auf **Speichern** zu klicken.

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Wenn Sie überprüfen möchten, welche Erweiterungen sich auf der VM befinden, klicken Sie auf **Erweiterungen**. Wenn die MSI aktiviert ist, wird **ManagedIdentityExtensionforWindows** in der Liste angezeigt.

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto. 

Erstellen Sie ein Speicherkonto, sofern Sie über keines verfügen. Sie können diesen Schritt auch überspringen und VM-MSI Zugriff auf die SAS-Anmeldeinformationen eines vorhandenen Speicherkontos gewähren. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+/Neuen Dienst erstellen**.
2. Klicken Sie auf **Speicher**, dann auf **Speicherkonto**, und anschließend wird ein neuer Bereich namens „Speicherkonto erstellen“ angezeigt.
3. Geben Sie einen Namen für das Speicherkonto ein, das Sie später verwenden werden.  
4. **Bereitstellungsmodell** und **Kontoart** sollten jeweils auf „Resource Manager“ und „Allgemein“ festgelegt werden. 
5. Stellen Sie sicher, dass **Abonnement** und **Ressourcengruppe** dem entsprechen, was Sie bei der Erstellung Ihrer VM im vorherigen Schritt angegeben haben.
6. Klicken Sie auf **Erstellen**.

    ![Erstellen eines neuen Speicherkontos](media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Erstellen eines Blob-Containers im Speicherkonto

Später werden wir eine Datei in das neue Speicherkonto hoch- und daraus herunterladen. Da Dateien Blob-Speicher erfordern, müssen wir einen Blob-Container erstellen, in dem die Datei gespeichert wird.

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.
2. Klicken Sie im linken Bereich unter „Blob-Dienst“ auf den Link **Container**.
3. Klicken Sie oben auf der Seite auf **+ Container**, und ein Bereich namens „Neuer Container“ wird geöffnet.
4. Geben Sie dem Container einen Namen, wählen Sie eine Zugriffsebene aus, und klicken Sie dann auf **OK**. Der angegebene Name wird später in diesem Tutorial verwendet. 

    ![Erstellen eines Speichercontainers](media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Gewähren des MSI-Zugriffs für Ihren virtuellen Computer für die Verwendung einer Speicherkonto-SAS 

Azure Storage unterstützt die Azure AD-Authentifizierung nicht nativ.  Sie können eine MSI jedoch zum Abrufen einer Speicher-SAS von Ressourcen-Manager verwenden und mithilfe dieser SAS auf den Speicher zugreifen.  In diesem Schritt erteilen Sie Ihrem virtuellen Computer MSI-Zugriff auf die Speicherkonto-SAS.   

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.   
2. Klicken Sie im linken Bereich auf den Link **Zugriffssteuerung (IAM)**.  
3. Klicken Sie dann oben auf der Seite auf **+ Hinzufügen**, um dem virtuellen Computer eine neue Rollenzuweisung hinzuzufügen.
4. Legen Sie **Rolle** rechts auf der Seite auf „Speicherkontomitwirkender“ fest.  
5. Legen Sie in der nächsten Dropdownliste **Zugriff zuweisen zu** auf die Ressource „Virtueller Computer“ fest.  
6. Stellen Sie im nächsten Schritt sicher, dass das richtige Abonnement in der Dropdownliste **Abonnement** aufgeführt wird, und legen Sie dann **Ressourcengruppe** auf „Alle Ressourcengruppen“ fest.  
7. Wählen Sie schließlich unter **Auswählen** in der Dropdownliste Ihren virtuellen Windows-Computer aus, und klicken Sie dann auf **Speichern**. 

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager 

Für den Rest des Tutorials arbeiten wir von dem virtuellen Computer aus, den wir zuvor erstellt haben.

In diesem Abschnitt müssen Sie PowerShell-Cmdlets von Azure Resource Manager verwenden.  Wenn Sie das Programm nicht installiert haben, [laden Sie die neuesten Version herunter](https://docs.microsoft.com/powershell/azure/overview), bevor Sie fortfahren.

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, wechseln Sie zu Ihrem virtuellen Windows-Computer, und klicken Sie dann oben auf der Seite **Übersicht** auf **Verbinden**.
2. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des virtuellen Windows-Computers hinzugefügt haben. 
3. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt PowerShell in der Remotesitzung. 
4. Erstellen Sie mithilfe des PowerShell-Befehls „Invoke-WebRequest“ eine Anforderung an den lokalen MSI-Endpunkt, um ein Zugriffstoken für Azure Resource Manager zu erhalten.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
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

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Abrufen von SAS-Anmeldeinformationen von Azure Resource Manager für Speicheraufrufe 

Nun rufen Sie mit PowerShell und dem Zugriffstoken, das im vorherigen Abschnitt abgerufen wurde, den Ressourcen-Manager auf, um Anmeldeinformationen für eine Speicher-SAS zu erstellen. Sobald wir die SAS-Anmeldeinformationen haben, können wir Speichervorgänge aufrufen.

Für diese Anforderung verwenden wir die folgenden HTTP-Anforderungsparameter, um die SAS-Anmeldeinformationen zu erstellen:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Diese Parameter sind im POST-Text der Anforderung für die SAS-Anmeldeinformationen enthalten. Weitere Informationen zu Parametern für das Erstellen von SAS-Anmeldeinformationen finden Sie in der [REST-Referenz zu „List Service SAS“](/rest/api/storagerp/storageaccounts/listservicesas).

Konvertieren Sie zuerst die Parameter in JSON, und rufen Sie dann den `listServiceSas`-Endpunkt des Speichers auf, um die SAS-Anmeldedaten zu erstellen:

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> Bei der URL wird die Groß-/Kleinschreibung beachtet. Achten Sie daher darauf, dieselbe Groß- und Kleinschreibung zu verwenden wie zuvor beim Benennen der Ressourcengruppe, einschließlich des Großbuchstaben „G“ in „resourceGroups“. 

Jetzt können wir die SAS-Anmeldeinformationen aus der Antwort extrahieren:

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

Wenn Sie die SAS-Anmeldeinformationen überprüfen, wird Ihnen Folgendes angezeigt:

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Als Nächstes erstellen wir eine Datei namens „test.txt“. Führen Sie dann mithilfe der SAS-Anmeldeinformationen die Authentifizierung mit dem `New-AzureStorageContent`-Cmdlet aus, und laden Sie die Datei in unseren Blob-Container hoch und anschließend wieder herunter.

```bash
echo "This is a test text file." > test.txt
```

Installieren Sie unbedingt zunächst die Azure Storage-Cmdlets mit `Install-Module Azure.Storage`. Laden Sie dann das Blob, das Sie soeben erstellt haben, mit dem PowerShell-Cmdlet `Set-AzureStorageBlobContent` hoch:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Antwort:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Sie können das Blob, das Sie soeben erstellt haben, auch mit dem PowerShell-Cmdlet `Get-AzureStorageBlobContent` herunterladen:

```powershell
Get-AzureStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Antwort:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```


## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](../active-directory/msi-overview.md).
- Wenn Sie das gleiche Tutorial mit Speicherkontoschlüsseln ausführen möchten, lesen Sie [Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Storage](msi-tutorial-windows-vm-access-storage.md).
- Weitere Informationen zum SAS-Feature des Azure Storage-Kontos finden Sie hier:
  - [Verwenden von Shared Access Signatures (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Constructing a Service SAS (Erstellen einer Dienstebenen-SAS)](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.


