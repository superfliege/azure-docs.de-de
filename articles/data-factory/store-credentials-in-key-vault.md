---
title: Speichern von Anmeldeinformationen in Azure Key Vault | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Anmeldeinformationen für verwendete Datenspeicher in einem Azure-Schlüsseltresor speichern können, die von Azure Data Factory zur Laufzeit automatisch abgerufen werden können."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jingwang
ms.openlocfilehash: cb4545dcd41fe9a2c73bc86096843b717d61a785
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Speichern von Anmeldeinformationen in Azure Key Vault

Sie können Anmeldeinformationen für Datenspeicher in einem [Azure Key Vault](../key-vault/key-vault-whatis.md)-Tresor speichern. Azure Data Factory ruft die Anmeldeinformationen ab, wenn eine Aktivität erfolgt, die den Datenspeicher verwendet.

> [!NOTE]
> Derzeit unterstützt nur der [Dynamics-Connector](connector-dynamics-crm-office-365.md) diese Funktion.

## <a name="steps"></a>Schritte

Wenn Sie eine Data Factory erstellen, kann beim Erstellen der Factory auch eine Dienstidentität erstellt werden. Die Dienstidentität ist eine verwaltete Anwendung, die im Azure Active Directory registriert ist und diese spezifische Data Factory darstellt.

- Beim Erstellen einer Data Factory über das **Azure-Portal oder PowerShell** wird seit der öffentlichen Vorschauversion die Dienstidentität immer automatisch erstellt.
- Wenn Sie eine Data Factory über das **SDK** erstellen, wird die Dienstidentität nur erstellt, wenn Sie im Factory-Objekt für die Erstellung „Identity = new FactoryIdentity()“ angeben. Sehen Sie sich das Beispiel unter [.NET-Schnellstart – Erstellen einer Data Factory](quickstart-create-data-factory-dot-net.md#create-a-data-factory) an.
- Beim Erstellen einer Data Factory mithilfe der **REST-API** wird die Dienstidentität nur erstellt, wenn Sie im Anforderungstext den Abschnitt „identity“ angeben. Sehen Sie sich das Beispiel unter [REST-Schnellstart – Erstellen einer Data Factory](quickstart-create-data-factory-rest-api.md#create-a-data-factory) an.

Führen Sie die folgenden Schritte aus, um auf in Azure Key Vault gespeicherte Anmeldeinformationen zu verweisen:

1. Kopieren Sie die mit Ihrer Data Factory generierte „DIENSTIDENTITÄTSANWENDUNGS-ID“. Informationen finden Sie unter [Abrufen der Dienstidentität](#retrieve-service-identity).
2. Gewähren Sie der Dienstidentität Zugriff auf Ihren Azure Key Vault-Tresor. Suchen Sie in Ihrem Schlüsseltresor unter „Zugriffssteuerung“ > „Hinzufügen“ diese Dienstidentitätsanwendungs-ID, und fügen Sie die Berechtigung „Leser“ hinzu. Dies ermöglicht der angegebenen Data Factory den Zugriff auf das Geheimnis im Schlüsseltresor.
3. Erstellen Sie einen verknüpften Dienst, der auf Ihren Azure Key Vault-Tresor verweist. Siehe [Mit Azure Key Vault verknüpfter Dienst](#azure-key-vault-linked-service).
4. Erstellen Sie einen mit einem Datenspeicher verknüpften Dienst, in dem Sie auf das entsprechende Geheimnis verweisen, das im Schlüsseltresor gespeichert ist. Informationen finden Sie unter [Verweisen auf im Schlüsseltresor gespeicherte Anmeldeinformationen](#reference-credential-stored-in-key-vault).

## <a name="retrieve-service-identity"></a>Abrufen der Dienstidentität

Sie können die Dienstidentität über das Azure-Portal oder programmgesteuert abrufen. Die folgenden Abschnitte enthalten einige Beispiele.

>[!TIP]
> Wird die Dienstidentität nicht angezeigt, können Sie durch Aktualisieren der Factory die [Dienstidentität erstellen](#generate-service-identity).

### <a name="using-azure-portal"></a>Verwenden des Azure-Portals

Sie finden die Informationen zur Dienstidentität im Azure-Portal unter „Ihre Data Factory“ > „Einstellungen“ > „Eigenschaften“:

- DIENSTIDENTITÄTS-ID
- DIENSTIDENTITÄTSMANDANT
- **DIENSTIDENTITÄTSANWENDUNGS-ID**: Kopieren Sie diesen Wert, um Zugriff in Key Vault zu gewähren.

![Abrufen der Dienstidentität](media/store-credentials-in-key-vault/retrieve-service-identity-portal.png)

### <a name="using-powershell"></a>Verwenden von PowerShell

Die Dienstidentitätsprinzipal-ID und die Mandanten-ID werden zurückgegeben, wenn Sie wie folgt eine bestimmte Data Factory abrufen:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Kopieren Sie die Prinzipal-ID, und führen Sie den unten angegebenen Azure Active Directory-Befehl mit der Prinzipal-ID als Parameter aus, um **ApplicationId** abzurufen. Diesen Wert verwenden Sie dann zum Gewähren des Zugriffs in Key Vault:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="azure-key-vault-linked-service"></a>Mit Azure Key Vault verknüpfter Dienst

Folgende Eigenschaften werden für den mit Azure Key Vault verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die "type"-Eigenschaft muss auf **AzureKeyVault** festgelegt werden. | Ja |
| baseUrl | Geben Sie die Azure Key Vault-URL an. | Ja |

**Beispiel:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
        "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-credential-stored-in-key-vault"></a>Verweisen auf im Schlüsseltresor gespeicherte Anmeldeinformationen

Die folgenden Eigenschaften werden unterstützt, wenn Sie ein Feld in einem verknüpften Dienst konfigurieren, das auf ein Geheimnis im Schlüsseltresor verweist:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die "type"-Eigenschaft des Felds muss auf **AzureKeyVaultSecret** festgelegt werden. | Ja |
| secretName | Der Name des Geheimnisses im Azure-Schlüsseltresor. | Ja |
| secretVersion | Die Version des Geheimnisses im Azure-Schlüsseltresor.<br/>Falls nicht angegeben, wird immer die neueste Version des Geheimnisses verwendet.<br/>Falls angegeben, wird die angegebene Version verwendet.| Nein |
| store | Verweist auf einen mit Azure Key Vault verknüpften Dienst, in dem Sie die Anmeldeinformationen speichern. | Ja |

**Beispiel: (siehe den Abschnitt „Kennwort“)**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="generate-service-identity"></a>Erstellen der Dienstidentität

Sollte Ihrer Data Factory nach dem Schritt [Abrufen einer Dienstidentität](#retrieve-service-identity) keine Dienstidentität zugeordnet sein, können Sie die Data Factory programmgesteuert mit dem Identitätsinitiator aktualisieren.

> [!NOTE]
> - **Die Dienstidentität kann nicht geändert werden.** Die Aktualisierung einer Data Factory, die bereits über eine Dienstidentität verfügt, hat keine Auswirkungen. Die Dienstidentität bleibt unverändert.
> - **Die Dienstidentität kann nicht gelöscht werden.** Wenn Sie eine Data Factory, die bereits über eine Dienstidentität verfügt, aktualisieren, ohne den Parameter „identity“ im Factory-Objekt oder den Abschnitt „identity“ im REST-Anforderungstext anzugeben, erhalten Sie einen Fehler.

Die folgenden Abschnitte enthalten einige Beispiele zum Erstellen einer Dienstidentität für die Factory, falls noch keine vorhanden ist.

### <a name="using-powershell"></a>Verwenden von PowerShell

Rufen Sie erneut den Befehl **Set-AzureRmDataFactoryV2** auf. Die Identity-Felder werden neu erstellt:

```powershell
PS C:\WINDOWS\system32> Set-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="using-rest-api"></a>Verwenden der REST-API

Rufen Sie die unten angegebene API mit dem Abschnitt „identity“ im Anforderungstext auf:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2017-09-01-preview
```

**Anforderungstext:** Fügen Sie „identity": { "type": "SystemAssigned" }‘“ hinzu.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Antwort:** Die Dienstidentität wird automatisch erstellt, und der Abschnitt „identity“ wird entsprechend aufgefüllt.

```json
{
    "name": "ADFV2DemoFactory",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2017-09-01-preview"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "EastUS"
}
```

### <a name="using-sdk"></a>Verwenden des SDK

Rufen Sie die Data Factory-Funktion zum Erstellen oder Aktualisieren mit „Identity=new FactoryIdentity()“ auf. Beispielcode unter Verwendung von .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).