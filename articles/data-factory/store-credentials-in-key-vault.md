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
ms.date: 11/09/2017
ms.author: jingwang
ms.openlocfilehash: f7604e251bd62ec382ac9ace3de058e345abb863
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Speichern von Anmeldeinformationen in Azure Key Vault

Sie können Anmeldeinformationen für Datenspeicher in einem [Azure Key Vault](../key-vault/key-vault-whatis.md)-Tresor speichern. Azure Data Factory ruft die Anmeldeinformationen ab, wenn eine Aktivität erfolgt, die den Datenspeicher verwendet. Derzeit unterstützen nur der [Dynamics-Connector](connector-dynamics-crm-office-365.md) und der [Salesforce-Connector](connector-salesforce.md) diese Funktion.

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Einführung in Azure Data Factory](v1/data-factory-introduction.md) weiter.

## <a name="prerequisites"></a>Voraussetzungen

Diese Funktion basiert auf der Data Factory-Dienstidentität. Informationen zur Funktionsweise finden Sie unter [Data factory service identity](data-factory-service-identity.md) (Data Factory-Dienstidentität). Stellen Sie sicher, dass Ihrer Data Factory eine Dienstidentität zugeordnet ist.

## <a name="steps"></a>Schritte

Führen Sie die folgenden Schritte aus, um auf in Azure Key Vault gespeicherte Anmeldeinformationen zu verweisen:

1. [Rufen Sie die Data Factory-Dienstidentität](data-factory-service-identity.md#retrieve-service-identity) ab, indem Sie den Wert von „DIENSTIDENTITÄTSANWENDUNGS-ID“ kopieren, der zusammen mit der Factory generiert wurde.
2. Gewähren Sie der Dienstidentität Zugriff auf Ihren Azure Key Vault-Tresor. Suchen Sie in Ihrem Schlüsseltresor unter „Zugriffssteuerung“ > „Hinzufügen“ diese Dienstidentitätsanwendungs-ID, und fügen Sie mindestens die Berechtigung **Leser** hinzu. Dies ermöglicht der angegebenen Data Factory den Zugriff auf das Geheimnis im Schlüsseltresor.
3. Erstellen Sie einen verknüpften Dienst, der auf Ihren Azure Key Vault-Tresor verweist. Siehe [Mit Azure Key Vault verknüpfter Dienst](#azure-key-vault-linked-service).
4. Erstellen Sie einen mit einem Datenspeicher verknüpften Dienst, in dem Sie auf das entsprechende Geheimnis verweisen, das im Schlüsseltresor gespeichert ist. Informationen finden Sie unter [Verweisen auf im Schlüsseltresor gespeicherte Anmeldeinformationen](#reference-credential-stored-in-key-vault).

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
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).