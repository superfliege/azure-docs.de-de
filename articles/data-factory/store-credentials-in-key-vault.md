---
title: Speichern von Anmeldeinformationen in Azure Key Vault | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anmeldeinformationen für verwendete Datenspeicher in einem Azure-Schlüsseltresor speichern können, die von Azure Data Factory zur Laufzeit automatisch abgerufen werden können.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2017
ms.author: jingwang
ms.openlocfilehash: 9a71a455ac4f406695edf722bc83604539eccaa9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="store-credential-in-azure-key-vault"></a>Speichern von Anmeldeinformationen in Azure Key Vault

Sie können Anmeldeinformationen für Datenspeicher und Computervorgänge in einer [Azure Key Vault](../key-vault/key-vault-whatis.md)-Instanz speichern. Azure Data Factory ruft die Anmeldeinformationen ab, wenn eine Aktivität ausgeführt wird, die den Datenspeicher/Computevorgänge verwendet.

Derzeit unterstützen mit Ausnahme von benutzerdefinierten Aktivitäten alle Aktivitätstypen dieses Feature. Lesen Sie insbesondere für die Connectorkonfiguration die Details im Abschnitt „Eigenschaften des verknüpften Diensts“ in den [Themen zu den einzelnen Connectors](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

## <a name="prerequisites"></a>Voraussetzungen

Diese Funktion basiert auf der Data Factory-Dienstidentität. Informationen zur Funktionsweise finden Sie unter [Data factory service identity](data-factory-service-identity.md) (Data Factory-Dienstidentität). Stellen Sie sicher, dass Ihrer Data Factory eine Dienstidentität zugeordnet ist.

## <a name="steps"></a>Schritte

Führen Sie die folgenden Schritte aus, um auf in Azure Key Vault gespeicherte Anmeldeinformationen zu verweisen:

1. **[Rufen Sie die Data Factory-Dienstidentität ab](data-factory-service-identity.md#retrieve-service-identity)**, indem Sie den Wert von „DIENSTIDENTITÄTSANWENDUNGS-ID“ kopieren, der zusammen mit der Factory generiert wurde.
2. **Gewähren Sie der Dienstidentität Zugriff auf Ihren Azure Key Vault-Tresor.** Suchen Sie in Ihrem Schlüsseltresor unter „Zugriffsrichtlinien“ -> „Neue hinzufügen“ nach dieser Dienstidentitätsanwendungs-ID, um ihr in der Dropdownliste „Berechtigungen für Geheimnis“ die **Abrufberechtigung** zu erteilen. Dies ermöglicht der angegebenen Data Factory den Zugriff auf das Geheimnis im Schlüsseltresor.
3. **Erstellen Sie einen verknüpften Dienst, der auf Ihren Azure Key Vault-Tresor verweist.** Siehe [Mit Azure Key Vault verknüpfter Dienst](#azure-key-vault-linked-service).
4. **Erstellen Sie einen mit einem Datenspeicher verknüpften Dienst, in dem Sie auf das entsprechende Geheimnis verweisen, das im Schlüsseltresor gespeichert ist.** Informationen finden Sie unter [Verweisen auf ein im Schlüsseltresor gespeichertes Geheimnis](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Mit Azure Key Vault verknüpfter Dienst

Folgende Eigenschaften werden für den mit Azure Key Vault verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die "type"-Eigenschaft muss auf **AzureKeyVault** festgelegt werden. | Ja |
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

## <a name="reference-secret-stored-in-key-vault"></a>Verweisen auf ein im Schlüsseltresor gespeichertes Geheimnis

Die folgenden Eigenschaften werden unterstützt, wenn Sie ein Feld in einem verknüpften Dienst konfigurieren, das auf ein Geheimnis im Schlüsseltresor verweist:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die "type"-Eigenschaft des Felds muss auf **AzureKeyVaultSecret** festgelegt werden. | Ja |
| secretName | Der Name des Geheimnisses im Azure-Schlüsseltresor. | Ja |
| secretVersion | Die Version des Geheimnisses im Azure-Schlüsseltresor.<br/>Falls nicht angegeben, wird immer die neueste Version des Geheimnisses verwendet.<br/>Falls angegeben, wird die angegebene Version verwendet.| Nein  |
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