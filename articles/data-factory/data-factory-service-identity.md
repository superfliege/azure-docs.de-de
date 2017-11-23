---
title: "Azure Data Factory-Dienstidentität | Microsoft-Dokumentation"
description: "Erhalten Sie Informationen zur Data Factory-Dienstidentität in Azure Data Factory."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: jingwang
ms.openlocfilehash: 4b970341f9511a889bfcc288756b1be6a4f4647e
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="azure-data-factory-service-identity"></a>Azure Data Factory-Dienstidentität

Dieser Artikel hilft Ihnen zu verstehen, was die Data Factory-Dienstidentität ist und wie diese funktioniert.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

## <a name="overview"></a>Übersicht

Wenn Sie eine Data Factory erstellen, kann beim Erstellen der Factory auch eine Dienstidentität erstellt werden. Die Dienstidentität ist eine verwaltete Anwendung, die im Azure Active Directory registriert ist und diese spezifische Data Factory darstellt.

Die Data Factory-Dienstidentität profitiert von den folgenden beiden Features:

- [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md): In diesem Fall wird die Data Factory-Dienstidentität für die Azure Key Vault-Authentifizierung verwendet.
- [Kopieren von Daten von/zu Azure Data Lake Store](connector-azure-data-lake-store.md): In diesem Fall kann die Data Factory-Dienstidentität als einer der unterstützten Data Lake Store-Authentifizierungstypen verwendet werden.

## <a name="generate-service-identity"></a>Erstellen der Dienstidentität

Die Data Factory-Dienstidentität wird wie folgt generiert:

- Beim Erstellen einer Data Factory über das **Azure-Portal oder PowerShell** wird seit der öffentlichen ADF V2-Vorschauversion die Dienstidentität immer automatisch erstellt.
- Wenn Sie eine Data Factory über das **SDK** erstellen, wird die Dienstidentität nur erstellt, wenn Sie im Factory-Objekt für die Erstellung „Identity = new FactoryIdentity()“ angeben. Sehen Sie sich das Beispiel unter [.NET-Schnellstart – Erstellen einer Data Factory](quickstart-create-data-factory-dot-net.md#create-a-data-factory) an.
- Beim Erstellen einer Data Factory mithilfe der **REST-API** wird die Dienstidentität nur erstellt, wenn Sie im Anforderungstext den Abschnitt „identity“ angeben. Sehen Sie sich das Beispiel unter [REST-Schnellstart – Erstellen einer Data Factory](quickstart-create-data-factory-rest-api.md#create-a-data-factory) an.

Sollte Ihrer Data Factory nach dem Schritt [Abrufen einer Dienstidentität](#retrieve-service-identity) keine Dienstidentität zugeordnet sein, können Sie diese explizit durch das programmgesteuerte Aktualisieren der Data Factory mit dem Identitätsinitiator erstellen:

- [Erstellen der Dienstidentität mit PowerShell](#generate-service-identity-using-powershell)
- [Erstellen der Dienstidentität mit REST-API](#generate-service-identity-using-rest-api)
- [Erstellen der Dienstidentität mit SDK](#generate-service-identity-using-sdk)

>[!NOTE]
>- Die Dienstidentität kann nicht geändert werden. Die Aktualisierung einer Data Factory, die bereits über eine Dienstidentität verfügt, hat keine Auswirkungen. Die Dienstidentität bleibt unverändert.
>- Wenn Sie eine Data Factory, die bereits über eine Dienstidentität verfügt, aktualisieren, ohne den Parameter „identity“ im Factory-Objekt oder den Abschnitt „identity“ im REST-Anforderungstext anzugeben, erhalten Sie einen Fehler.
>- Wenn Sie eine Data Factory löschen, wird die zugehörige Dienstidentität ebenfalls gelöscht.

### <a name="generate-service-identity-using-powershell"></a>Erstellen der Dienstidentität mit PowerShell

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

### <a name="generate-service-identity-using-rest-api"></a>Erstellen der Dienstidentität mit REST-API

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

### <a name="generate-service-identity-using-sdk"></a>Erstellen der Dienstidentität mit SDK

Rufen Sie die Data Factory-Funktion zum Erstellen oder Aktualisieren mit „Identity=new FactoryIdentity()“ auf. Beispielcode unter Verwendung von .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-service-identity"></a>Abrufen der Dienstidentität

Sie können die Dienstidentität über das Azure-Portal oder programmgesteuert abrufen. Die folgenden Abschnitte enthalten einige Beispiele.

>[!TIP]
> Wird die Dienstidentität nicht angezeigt, können Sie durch Aktualisieren der Factory die [Dienstidentität erstellen](#generate-service-identity).

### <a name="retrieve-service-identity-using-azure-portal"></a>Abrufen der Dienstidentität über das Azure-Portal

Sie finden die Informationen zur Dienstidentität im Azure-Portal unter „Ihre Data Factory“ > „Einstellungen“ > „Eigenschaften“:

- DIENSTIDENTITÄTS-ID
- DIENSTIDENTITÄTSMANDANT
- **DIENSTIDENTITÄTSANWENDUNGS-ID** > diesen Wert kopieren

![Abrufen der Dienstidentität](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-service-identity-using-powershell"></a>Abrufen der Dienstidentität mit PowerShell

Die Dienstidentitätsprinzipal-ID und die Mandanten-ID werden zurückgegeben, wenn Sie wie folgt eine bestimmte Data Factory abrufen:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Kopieren Sie die Prinzipal-ID, und führen Sie den unten angegebenen Azure Active Directory-Befehl mit der Prinzipal-ID als Parameter aus, um **ApplicationId** abzurufen. Diesen Wert verwenden Sie dann zum Gewähren des Zugriffs:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Themen, die Ihnen zeigen, wann und wie Sie die Data Factory-Dienstidentität verwenden können:

- [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md)
- [Kopieren von Daten von/zu Azure Data Lake Store mit verwalteter Dienstidentitätsauthentifizierung](connector-azure-data-lake-store.md)