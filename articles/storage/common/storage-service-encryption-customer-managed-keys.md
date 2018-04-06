---
title: Azure Storage Service Encryption mit von Kunden verwalteten Schlüsseln in Azure Key Vault | Microsoft-Dokumentation
description: Mit Azure Storage Service Encryption können Sie Azure Blob Storage auf Dienstseite beim Speichern von Daten verschlüsseln und wieder entschlüsseln, wenn Daten mit von Kunden verwalteten Schlüsseln abgerufen werden.
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/07/2018
ms.author: lakasa
ms.openlocfilehash: 04688f943ac9eba27ca193aa2054c69b6a94547d
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Storage Service Encryption mit von Kunden verwalteten Schlüsseln in Azure Key Vault

Es ist uns ein Anliegen, dass Microsoft Azure Sie beim Schutz Ihrer Daten gemäß den Sicherheits- und Konformitätsanforderungen Ihrer Organisation unterstützt. Azure Storage schützt Ihre Daten mittels Storage Service Encryption (SSE). Damit werden die Daten beim Schreiben in den Speicher automatisch verschlüsselt und beim Abrufen auch wieder entschlüsselt. Die Ver- und Entschlüsselung erfolgt automatisch und transparent. Dabei wird mit der 256-Bit-[AES-Verschlüsselung](https://wikipedia.org/wiki/Advanced_Encryption_Standard) eine der stärksten verfügbaren Blockchiffren verwendet.

Mit SSE können von Microsoft verwaltete oder eigene Verschlüsselungsschlüssel verwendet werden. In diesem Artikel erfahren Sie, wie Sie Ihren eigenen Verschlüsselungsschlüssel verwenden. Weitere Informationen zur Verwendung von von Microsoft verwalteten Schlüsseln oder SSE im Allgemeinen finden Sie unter [Azure-Speicherverschlüsselung für ruhende Daten](storage-service-encryption.md).

SSE für Blob und File Storage ist in Azure Key Vault integriert, sodass Sie Ihre Verschlüsselungsschlüssel in einem Schlüsseltresor verwalten können. Sie können Ihre eigenen Verschlüsselungsschlüssel erstellen und in einem Schlüsseltresor speichern oder mit Azure Key Vault-APIs Verschlüsselungsschlüssel generieren. Mit Azure Key Vault können Sie auch Ihre Schlüssel und deren Verwendung verwalten und überwachen.

Warum sollten Sie eigene Schlüssel erstellen? Benutzerdefinierte Schlüssel bieten Ihnen mehr Flexibilität, sodass Sie Zugriffssteuerelemente erstellen, drehen, deaktivieren und definieren können. Außerdem können Sie die Verschlüsselungsschlüssel prüfen, die Ihre Daten schützen.

## <a name="get-started-with-customer-managed-keys"></a>Erste Schritte mit von Kunden verwalteten Schlüsseln

Um vom Kunden verwaltete Schüssel mit SSE zu verwalten, können Sie einen neuen Schlüsseltresor und Schlüssel erstellen oder bereits vorhandene verwenden. Das Speicherkonto und der Schlüsseltresor müssen sich in derselben Region befinden, dürfen aber zu verschiedenen Abonnements gehören. 

### <a name="step-1-create-a-storage-account"></a>Schritt 1: Erstellen eines Speicherkontos

Erstellen Sie zuerst ein Speicherkonto, falls Sie noch kein Konto besitzen. Weitere Informationen finden Sie unter [Erstellen eines neuen Speicherkontos](storage-quickstart-create-account.md).

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>Schritt 2: Aktivieren von SSE für Blob und File Storage

Um SSE mit von Kunden verwalteten Schlüsseln zu aktivieren, müssen zwei Features für den Schlüsselschutz aktiviert werden:„Soft Delete“ (Vorläufig löschen) und „Do Not Purge“ (Nicht bereinigen). Mit diesen Einstellungen verhindern Sie versehentliches oder absichtliches Löschen der Schlüssel. Die maximale Beibehaltungsdauer der Schlüssel ist auf 90 Tage festgelegt, um Benutzer vor böswilligen Akteuren oder Ransomware Angriffe zu schützen.

Wenn Sie von Kunden verwaltete Schlüssel für SSE automatisch aktivieren möchten, können Sie die [REST-API des Azure Storage-Ressourcenanbieters](https://docs.microsoft.com/rest/api/storagerp), die [Clientbibliothek des Speicherressourcenanbieters für .NET](https://docs.microsoft.com/dotnet/api), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) oder die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/storage/storage-azure-cli) verwenden.

Um von Kunden-verwaltete Schlüssel mit SSE verwenden zu können, müssen Sie dem Speicherkonto eine Speicherkontoidentität zuweisen. Führen Sie den folgenden PowerShell-Befehl aus, um die Identität festzulegen:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

Führen Sie die folgenden PowerShell-Befehle aus, um „Soft Delete“ (Vorläufig löschen) und „Do Not Purge“ (Nicht bereinigen) zu aktivieren:

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>Schritt 3: Aktivieren der Verschlüsselung mit von Kunden verwalteten Schlüsseln

Standardmäßig verwendet SSE von Microsoft verwaltete Schlüssel. Sie können SSE mit von Kunden verwalteten Schlüsseln für das Speicherkonto über das [Azure-Portal](https://portal.azure.com/) aktivieren. Klicken Sie auf dem Blatt **Einstellungen** Blatt für das Speicherkonto auf **Verschlüsselung**. Wählen Sie wie in der folgenden Abbildung die Option **Eigenen Schlüssel verwenden**.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>Schritt 4: Auswählen Ihres Schlüssels

Sie können Ihren Schlüssel als URI festlegen oder einen Schlüssel aus einem Schlüsseltresor angeben.

#### <a name="specify-a-key-as-a-uri"></a>Festlegen eines Schlüssels als URI

Gehen Sie wie folgt vor, um Ihren Schlüssel über einen URI anzugeben:

1. Wählen Sie die Option **Schlüssel-URI eingeben**.  
2. Geben Sie den URI im Feld **Schlüssel-URI** ein.

    ![Screenshot des Portals: Verschlüsselungsoption „Schlüssel-URI eingeben“](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)


#### <a name="specify-a-key-from-a-key-vault"></a>Festlegen eines Schlüssels aus einem Schlüsseltresor 

Gehen Sie wie folgt vor, um Ihren Schlüssel aus einem Schlüsseltresor anzugeben:

1. Wählen Sie die Option **Select from Key Vault** (Aus Schlüsseltresor wählen).  
2. Wählen Sie den Schlüsseltresor mit dem Schlüssel, den Sie verwenden möchten.
3. Wählen Sie den Schlüssel aus dem Schlüsseltresor.

    ![Screenshot des Portals: Verschlüsselungsoption „Eigenen Schlüssel verwenden“](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Wenn das Speicherkonto keinen Zugriff auf den Schlüsseltresor hat, können Sie den Azure PowerShell-Befehl in der folgenden Abbildung ausführen, um Zugriff zu gewähren.

![Screenshot des Portals: Zugriff auf den Schlüsseltresor verweigert](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Sie können den Zugriff auch über das Azure-Portal gewähren. Navigieren Sie dazu im Azure-Portal zu Azure Key Vault, und gewähren Sie Zugriff auf das Speicherkonto.


Sie können den obigen Schlüssel mit den folgenden PowerShell-Befehlen einem vorhandenen Speicherkonto zuordnen:
```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount"
$keyVault = Get-AzureRmKeyVault -VaultName "mykeyvault"
$key = Get-AzureKeyVaultKey -VaultName $keyVault.VaultName -Name "keytoencrypt"
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVault.VaultName -ObjectId $storageAccount.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
Set-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -AccountName $storageAccount.StorageAccountName -EnableEncryptionService "Blob" -KeyvaultEncryption -KeyName $key.Name -KeyVersion $key.Version -KeyVaultUri $keyVault.VaultUri
```


### <a name="step-5-copy-data-to-storage-account"></a>Schritt 5: Kopieren von Daten in das Speicherkonto

Übertragen von Daten in das neue Speicherkonto zur Verschlüsselung. Weitere Informationen finden Sie unter [Azure Storage Service Encryption für ruhende Daten](storage-service-encryption.md#faq-for-storage-service-encryption).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>Schritt 6: Abfragen des Status der verschlüsselten Daten

Abfragen des Status der verschlüsselten Daten

## <a name="faq-for-sse-with-customer-managed-keys"></a>Häufig gestellte Fragen zu SSE mit von Kunden verwalteten Schlüsseln

**F: Ich verwende Storage Premium. Kann ich SSE mit von Kunden verwalteten Schlüsseln verwenden?**

A: Ja. SSE mit von Microsoft verwalteten und vom Kunden verwalteten Schlüsseln wird sowohl im Standardspeicher als auch in Storage Premium unterstützt.

**F: Kann ich neue Speicherkonten, bei denen SSE und vom Kunden verwaltete Schlüssel aktiviert sind, mit Azure PowerShell und der Azure-Befehlszeilenschnittstelle erstellen?**

A: Ja.

**F: Wie viel teurer ist Azure Storage, wenn ich SSE mit von Kunden verwalteten Schlüsseln verwende?**

A: Die Verwendung von Azure Key Vault ist nicht kostenlos. Weitere Informationen finden Sie unter [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/). Es entstehen keine zusätzlichen Kosten für SSE, das für alle Speicherkonten aktiviert ist.

**F: Kann ich den Zugriff auf die Verschlüsselungsschlüssel widerrufen?**

A: Ja, Sie können den Zugriff jederzeit widerrufen. Dazu stehen Ihnen mehrere Möglichkeiten zur Verfügung. Weitere Informationen finden Sie unter [Azure Key Vault – PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) und [Azure Key Vault – CLI](https://docs.microsoft.com/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Blobs im Speicherkonto blockiert, da Azure Storage keinen Zugriff mehr auf den Kontoverschlüsselungsschlüssel hat.

**F: Kann ich ein Speicherkonto und einen Schlüssel in unterschiedlichen Regionen erstellen?**

A: Nein, das Speicherkonto, Azure Key Vault und der Schlüssel müssen sich in der gleichen Region befinden.

**F: Kann ich von Kunden verwaltete Schlüssel für SSE aktivieren, während ich das Speicherkonto erstelle?**

A: Nein. Wenn Sie zuerst das Speicherkonto erstellen, sind nur von Microsoft verwaltete Schlüssel für SSE verfügbar. Sie müssen die Speicherkontoeigenschaften aktualisieren, um von Kunden verwaltete Schlüssel zu verwenden. Sie können REST oder eine der Speicherclientbibliotheken verwenden, um Ihr Speicherkonto programmgesteuert zu aktualisieren, oder die Eigenschaften des Speicherkontos nach dem Erstellen mithilfe des Azure-Portals aktualisieren.

**F: Kann ich die Verschlüsselung deaktivieren, während ich vom Kunden verwaltete Schlüssel mit SSE deaktiviere?**

A: Nein, Sie können die Verschlüsselung nicht deaktivieren. Die Verschlüsselung ist standardmäßig für alle Dienste aktiviert: Blob, File, Table und Queue Storage. Sie können auch zwischen von Microsoft verwalteten Schlüsseln und von Kunden verwalteten Schlüsseln wechseln und umgekehrt.

**F: Ist SSE standardmäßig aktiviert, wenn ich ein neues Speicherkonto erstelle?**

A: Die Verschlüsselung ist standardmäßig für alle Speicherkonten und für alle Dienste aktiviert: Blob, File, Table und Queue Storage.

**F: Ich kann SSE nicht mit von Kunden verwalteten Schlüsseln für mein Speicherkonto aktivieren.**

A: Handelt es sich um ein Azure Resource Manager-Speicherkonto? Klassische Speicherkonten unterstützen von Kunden verwaltete Schlüssel nicht. SSE mit von Kunden verwalteten Schlüsseln kann nur auf Resource Manager-Speicherkonten aktiviert werden.

**F: Wozu dienen die Optionen „Soft Delete“ (Vorläufig löschen) und „Do Not Purge“ (Nicht bereinigen)? Muss ich diese Einstellung aktivieren, um SSE mit von Kunden verwalteten Schlüsseln zu verwenden?**

A: „Soft Delete“ (Vorläufig löschen) und „Do Not Purge“ (Nicht bereinigen) müssen aktiviert werden, um von Kunden verwaltete Schlüssel zu verwenden. Mit diesen Einstellungen verhindern Sie versehentliches oder absichtliches Löschen der Schlüssel. Die maximale Beibehaltungsdauer der Schlüssel ist auf 90 Tage festgelegt, um Benutzer vor böswilligen Akteuren und Ransomware Angriffe zu schützen. Diese Einstellung kann nicht deaktiviert werden.

**F: Ist SSE mit von Kunden verwalteten Schlüsseln nur in bestimmten Regionen zulässig?**

A: SSE mit von Kunden verwalteten Schlüsseln ist in allen Regionen für Blob und File Storage verfügbar.

**F: Wohin wende ich mich, wenn Probleme auftreten oder ich Feedback geben möchte?**

A: Wenden Sie sich in allen Angelegenheiten, die Storage Service Encryption betreffen, an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zu allen Sicherheitsfunktionen, die Entwickler beim Erstellen sicherer Anwendungen unterstützen, finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

-   Eine Übersicht über Azure Key Vault finden Sie unter [Was ist Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-whatis).

-   Informationen zu den ersten Schritten mit Azure Key Vault finden Sie unter [Erste Schritte mit Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
