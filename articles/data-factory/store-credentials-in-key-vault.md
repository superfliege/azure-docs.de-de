---
title: Speichern von Anmeldeinformationen in Azure Key Vault | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Anmeldeinformationen für verwendete Datenspeicher in einem Azure-Schlüsseltresor speichern können, die von Azure Data Factory zur Laufzeit automatisch abgerufen werden können."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: spelluru
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: fc8b4f1ecf664c7db5ab2e535245dee90415fa65
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="store-credential-in-azure-key-vault"></a>Speichern von Anmeldeinformationen in Azure Key Vault

Sie können Anmeldeinformationen für Datenspeicher in einem [Azure Key Vault](../key-vault/key-vault-whatis.md)-Tresor speichern. Azure Data Factory ruft die Anmeldeinformationen ab, wenn eine Aktivität erfolgt, die den Datenspeicher verwendet.

> [!NOTE]
> Derzeit unterstützt nur der [Dynamics-Connector](connector-dynamics-crm-office-365.md) diese Funktion. 

## <a name="steps"></a>Schritte

Wenn Sie eine Data Factory erstellen, wird beim Erstellen der Factory auch eine Dienstidentität erstellt. Die Dienstidentität ist eine verwaltete Anwendung, die im Azure Active Directory registriert ist und diese spezifische Data Factory darstellt. Sie finden die Informationen zur Dienstidentität im Azure-Portal unter „Ihre Data Factory“ > „Eigenschaften“: 

- DIENSTIDENTITÄTS-ID
- DIENSTIDENTITÄTSMANDANT
- DIENSTIDENTITÄTSANWENDUNGS-ID

Führen Sie die folgenden Schritte aus, um auf in Azure Key Vault gespeicherte Anmeldeinformationen zu verweisen:

1. Kopieren Sie die mit Ihrer Data Factory generierte „DIENSTIDENTITÄTSANWENDUNGS-ID“.
2. Gewähren Sie der Dienstidentität Zugriff auf Ihren Azure Key Vault-Tresor. Suchen Sie in Ihrem Schlüsseltresor unter „Zugriffssteuerung“ > „Hinzufügen“ diese Dienstidentitätsanwendungs-ID, und fügen Sie die Berechtigung „Leser“ hinzu. Dies ermöglicht der angegebenen Data Factory den Zugriff auf das Geheimnis im Schlüsseltresor.
3. Erstellen Sie einen verknüpften Dienst, der auf Ihren Azure Key Vault-Tresor verweist. Siehe [Mit Azure Key Vault verknüpfter Dienst](#azure-key-vault-linked-service).
4. Erstellen Sie einen mit einem Datenspeicher verknüpften Dienst, in dem Sie auf das entsprechende Geheimnis verweisen, das im Schlüsseltresor gespeichert ist. Siehe [Verweisen auf im Schlüsseltresor gespeicherte Anmeldeinformationen](#reference-credential-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Mit Azure Key Vault verknüpfter Dienst

Folgende Eigenschaften werden für den mit Azure Key Vault verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die "type"-Eigenschaft muss auf **AzureKeyVault** festgelegt werden. | Ja |
| baseUrl | Geben Sie die Azure Key Vault-URL (den DNS-Namen) an. | Ja |

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
| type | Die "type"-Eigenschaft des Felds muss auf **AzureKeyVaultSecret** festgelegt werden. | Ja |
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

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
