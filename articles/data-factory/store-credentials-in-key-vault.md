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
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: jingwang
ms.openlocfilehash: f12c3a2a1193adc08c408292a4e0eca2e2458085
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34620665"
---
# <a name="store-credential-in-azure-key-vault"></a>Speichern von Anmeldeinformationen in Azure Key Vault

Sie können Anmeldeinformationen für Datenspeicher und Computervorgänge in einer [Azure Key Vault](../key-vault/key-vault-whatis.md)-Instanz speichern. Azure Data Factory ruft die Anmeldeinformationen ab, wenn eine Aktivität ausgeführt wird, die den Datenspeicher/Computevorgänge verwendet.

Derzeit unterstützen mit Ausnahme von benutzerdefinierten Aktivitäten alle Aktivitätstypen dieses Feature. Lesen Sie insbesondere für die Connectorkonfiguration die Details im Abschnitt „Eigenschaften des verknüpften Diensts“ in den [Themen zu den einzelnen Connectors](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> Dieser Artikel bezieht sich auf Version 2 von Data Factory, die zurzeit als Vorschau verfügbar ist. Wenn Sie die allgemein verfügbare Version 1 (GA) des Data Factory-Diensts verwenden, helfen Ihnen die Informationen unter [Dokumentation zur Version 1 von Data Factory](v1/data-factory-introduction.md) weiter.

## <a name="prerequisites"></a>Voraussetzungen

Diese Funktion basiert auf der Data Factory-Dienstidentität. Informationen zur Funktionsweise finden Sie unter [Data factory service identity](data-factory-service-identity.md) (Data Factory-Dienstidentität). Stellen Sie sicher, dass Ihrer Data Factory eine Dienstidentität zugeordnet ist.

>[!TIP]
>Wenn Sie in Azure Key Vault ein Geheimnis erstellen, **verwenden Sie den gesamten Wert einer Geheimniseigenschaft, die der verknüpfte ADF-Dienst anfordert (z.B. Verbindungszeichenfolge/Kennwort/Dienstprinzipalschlüssel/usw.)**. Verwenden Sie z.B. für den verknüpften Azure Storage-Dienst `DefaultEndpointsProtocol=http;AccountName=myAccount;AccountKey=myKey;` als AKV-Geheimnis, und verweisen Sie im Feld „connectionString“ von ADF darauf; verwenden Sie für den verknüpften Dynamics-Dienst `myPassword` als AKV-Geheimnis, und verweisen Sie im Feld „paassword“ von ADF darauf. Weitere Informationen zu den unterstützten Eigenschaften finden Sie in den einzelnen Connector-/Computeartikeln.

## <a name="steps"></a>Schritte

Führen Sie die folgenden Schritte aus, um auf in Azure Key Vault gespeicherte Anmeldeinformationen zu verweisen:

1. **Rufen Sie die Data Factory-Dienstidentität** ab, indem Sie den Wert von „DIENSTIDENTITÄTSANWENDUNGS-ID“ kopieren, der zusammen mit der Factory generiert wurde. Bei Verwendung der ADF-Erstellungsbenutzeroberfläche wird die Dienstidentitäts-ID im Erstellungsfenster des verknüpften Azure Key Vault-Diensts angezeigt. Sie können sie auch vom Azure-Portal aus abrufen, siehe [Abrufen der Dienstidentität](data-factory-service-identity.md#retrieve-service-identity).
2. **Gewähren Sie der Dienstidentität Zugriff auf Ihren Azure Key Vault-Tresor.** Suchen Sie in Ihrem Schlüsseltresor unter „Zugriffsrichtlinien“ -> „Neue hinzufügen“ nach dieser Dienstidentitätsanwendungs-ID, um ihr in der Dropdownliste „Berechtigungen für Geheimnis“ die **Abrufberechtigung** zu erteilen. Dies ermöglicht der angegebenen Data Factory den Zugriff auf das Geheimnis im Schlüsseltresor.
3. **Erstellen Sie einen verknüpften Dienst, der auf Ihren Azure Key Vault-Tresor verweist.** Siehe [Mit Azure Key Vault verknüpfter Dienst](#azure-key-vault-linked-service).
4. **Erstellen Sie einen mit einem Datenspeicher verknüpften Dienst, in dem Sie auf das entsprechende Geheimnis verweisen, das im Schlüsseltresor gespeichert ist.** Informationen finden Sie unter [Verweisen auf ein im Schlüsseltresor gespeichertes Geheimnis](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Mit Azure Key Vault verknüpfter Dienst

Folgende Eigenschaften werden für den mit Azure Key Vault verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die "type"-Eigenschaft muss auf **AzureKeyVault** festgelegt werden. | Ja |
| baseUrl | Geben Sie die Azure Key Vault-URL an. | Ja |

**Verwendung der Erstellungsbenutzeroberfläche:**

Klicken Sie auf **Verbindungen** -> **Verknüpfte Dienste** -> **+Neu**, und suchen Sie nach „Azure Key Vault“:

![Nach AKV suchen](media/store-credentials-in-key-vault/search-akv.png)

Wählen Sie die bereitgestellte Azure Key Vault-Instanz aus, in der Ihre Anmeldeinformationen gespeichert sind. Sie können die **Verbindung testen**, um sicherzustellen, dass Ihre AKV Verbindung gültig ist. 

![AKV konfigurieren](media/store-credentials-in-key-vault/configure-akv.png)

**JSON-Beispiel**:

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

**Verwendung der Erstellungsbenutzeroberfläche:**

Wählen Sie beim Erstellen der Verbindung mit Ihrem Datenspeicher/Computedienst **Azure Key Vault** für Geheimnisfelder aus. Wählen Sie den bereitgestellten verknüpften Azure Key Vault-Dienst aus, und geben Sie den **Geheimnisnamen** an. Optional können Sie auch eine Geheimnisversion bereitstellen. 

![AKV-Geheimnis konfigurieren](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON-Beispiel: (siehe den Abschnitt „Kennwort“)**

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
