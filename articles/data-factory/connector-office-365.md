---
title: Kopieren von Daten aus Office 365 mithilfe von Azure Data Factory (Vorschauversion) | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten aus Office 365 mithilfe einer Kopieraktivität in einer Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: jingwang
ms.openlocfilehash: 1ed5f3db3f9f8f7231d8f865f69cd11c2430054b
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024313"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory-preview"></a>Kopieren von Daten aus Office 365 mithilfe von Azure Data Factory (Vorschauversion) 

Mit Azure Data Factory können Sie die umfangreichen Unternehmensdaten in Ihrem Office 365-Mandanten auf skalierbare Weise in Azure aufnehmen und basierend auf diesen wertvollen Datenbeständen Analyseanwendungen erstellen und Erkenntnisse extrahieren. Die Integration in Privileged Access Management bietet sichere Zugriffssteuerung für die wertvollen zusammengestellten Daten in Office 365.  Weitere Informationen zu Microsoft Graph-Datenverbindungen finden Sie unter [diesem Link](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki).

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Office 365 zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Vorerst können Sie in einer einzelnen Kopieraktivität **Daten aus Office 365 nur im JSON-Format (setOfObjects-Typ) in [Azure Blob Storage](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) und [Azure Data Lake Storage Gen2 (Vorschau)](connector-azure-data-lake-storage.md)** kopieren. Wenn Sie Office 365-Daten in andere Typen von Datenspeichern oder in anderen Formaten laden möchten, können Sie die erste Kopieraktivität mit einer nachfolgenden Kopieraktivität verketten, um Daten in einen der [unterstützten ADF-Zielspeicher](copy-activity-overview.md#supported-data-stores-and-formats) zu laden (siehe Spalte „Als Senke unterstützt“ in der Tabelle „Unterstützte Datenspeicher und Formate“).

>[!IMPORTANT]
>- Das Azure-Abonnement mit der Data Factory und der Senkendatenspeicher müssen sich im gleichen Azure Active Directory-Mandanten (Azure AD) befinden wie der Office 365-Mandant.
>- Stellen Sie sicher, dass die für die Kopieraktivität verwendete Azure Integration Runtime-Region und das Ziel in der gleichen Region sind, in der sich das Postfach der Benutzer des Office 365-Mandanten befindet. [Hier](concepts-integration-runtime.md#integration-runtime-location) erfahren Sie, wie der Azure Integration Runtime-Standort bestimmt wird. In [dieser Tabelle](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#data-regions) finden Sie die Liste der unterstützten Office-Regionen und die entsprechenden Azure-Regionen.
>-  Wenn Sie Office 365-Daten in **Azure Blob Storage** als Ziel laden, stellen Sie sicher, dass Sie die **[Dienstprinzipalauthentifizierung](connector-azure-blob-storage.md#service-principal-authentication)** beim Definieren des verknüpften Diensts in Azure Blob Storage verwenden, nicht aber Authentifizierungen mit [Kontoschlüssel](connector-azure-blob-storage.md#account-key-authentication), [SAS](connector-azure-blob-storage.md#shared-access-signature-authentication) oder [verwalteten Identitäten für Azure-Ressourcen](connector-azure-blob-storage.md#managed-identity).
>-  Wenn Sie Office 365-Daten in **Azure Data Lake Storage Gen1** als Ziel laden, stellen Sie sicher, dass Sie die [**Dienstprinzipalauthentifizierung**](connector-azure-data-lake-store.md#use-service-principal-authentication) beim Definieren des verknüpften Diensts in Azure Data Lake Storage Gen1 verwenden, nicht aber die [Authentifizierung mit verwalteten Identitäten für Azure-Ressourcen](connector-azure-data-lake-store.md#managed-identity).

## <a name="prerequisites"></a>Voraussetzungen

Zum Kopieren von Daten aus Office 365 in Azure müssen Sie die folgenden Schritte ausführen:

- Ihr Office 365-Mandantenadministrator muss Onboardingaktionen ausführen, wie [hier](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/On-boarding) beschrieben.
- Erstellen und konfigurieren Sie eine Azure AD-Webanwendung in Azure Active Directory.  Anweisungen finden Sie unter [Erstellen einer Azure AD-Anwendung](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application).
- Notieren Sie sich die folgenden Werte, die Sie zum Definieren des verknüpften Diensts für Office 365 verwenden:
    - Mandanten-ID.  Anweisungen finden Sie unter [Abrufen der Mandanten-ID](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-id).
    - Anwendungs-ID und Authentifizierungsschlüssel.  Anweisungen finden Sie unter [Abrufen der Anwendungs-ID und des Authentifizierungsschlüssels](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key).
- Fügen Sie die Identität des Benutzers, der die Datenzugriffsanforderung erstellt, als Besitzer der Azure AD-Webanwendung hinzu (wählen Sie in der Azure AD-Webanwendung „Einstellungen > Besitzer > Besitzer hinzufügen“ aus).

## <a name="approving-new-data-access-requests"></a>Genehmigen neuer Datenzugriffsanforderungen

Wenn Sie erstmals Daten für diesen Kontext anfordern (also für eine Kombination daraus, auf welche Datentabelle zugegriffen wird, in welches Zielkonto die Daten geladen werden und welche Benutzeridentität die Datenzugriffsanforderung sendet), wird als Status der Kopieraktivität „In Bearbeitung“ angezeigt. Nur wenn Sie unter [„Aktionen“ auf den Link „Details“](copy-activity-overview.md#monitoring) klicken, wird als Status „RequestingConsent“ angezeigt.  Ein Mitglied aus der Gruppe der Datenzugriffsgenehmiger muss die Anforderung in Privileged Access Management genehmigen, bevor die Datenextraktion fortgesetzt werden kann.

[Hier](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Approving-data-access-requests) erfahren Sie, wie die genehmigende Person die Datenzugriffsanforderung genehmigen kann, und [hier](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#integration-with-privileged-access-management) finden Sie eine Erläuterung der allgemeinen Integration in Privileged Access Management, einschließlich Informationen zum Einrichten der Gruppe von genehmigenden Personen für den Datenzugriff.

## <a name="policy-validation"></a>Richtlinienüberprüfung

Wenn ADF als Teil einer verwalteten App erstellt wird und Azure-Richtlinienzuweisungen zu Ressourcen innerhalb der Verwaltungsressourcengruppe vorgenommen werden, prüft ADF für jede Ausführung der Kopieraktivität, ob die Richtlinienzuweisungen durchgesetzt werden. [Hier](https://github.com/OfficeDev/ManagedAccessMSGraph/wiki/Capabilities#policies) finden Sie eine Liste der unterstützten Richtlinien.

## <a name="getting-started"></a>Erste Schritte

>[!TIP]
>Eine exemplarische Vorgehensweise zur Verwendung des Office 365-Connectors finden Sie im Artikel [Laden von Daten aus Office 365](load-office-365-data.md).

Sie können mithilfe eines der folgenden Tools oder SDKs eine Pipeline mit der Copy-Aktivität erstellen. Klicken Sie auf einen Link, um ein Tutorial mit Schritt-für-Schritt-Anweisungen zum Erstellen einer Pipeline mit einer Copy-Aktivität aufzurufen. 

- [Azure-Portal](quickstart-create-data-factory-portal.md)
- [.NET SDK](quickstart-create-data-factory-dot-net.md)
- [Python SDK](quickstart-create-data-factory-python.md)
- [Azure PowerShell](quickstart-create-data-factory-powershell.md)
- [REST-API](quickstart-create-data-factory-rest-api.md)
- [Azure Resource Manager-Vorlage](quickstart-create-data-factory-resource-manager-template.md). 

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Office 365-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Office 365 verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **Office 365** | JA |
| office365TenantId | Die Azure-Mandanten-ID, zu der das Office 365-Konto gehört. | JA |
| servicePrincipalTenantId | Geben Sie die Mandanteninformationen Ihrer Azure AD-Webanwendung an. | JA |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | JA |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als „SecureString“, um es sicher in Data Factory zu speichern. | JA |
| connectVia | Die Integration Runtime, die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll.  Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Nein  |

>[!NOTE]
> Der Unterschied zwischen **office365TenantId** und **servicePrincipalTenantId** und der entsprechende anzugebende Wert:
>- Wenn Sie ein Unternehmensentwickler sind und eine Anwendung für Office 365-Daten für die Nutzung in Ihrer eigenen Organisation entwickeln, sollten Sie die gleiche Mandanten-ID für beide Eigenschaften angeben, und zwar die AAD-Mandanten-ID Ihrer Organisation.
>- Wenn Sie ein ISV-Entwickler sind und eine Anwendung für Ihre Kunden entwickeln, dann ist „office365TenantId“ die AAD-Mandanten-ID Ihres Kunden (der die Anwendung installiert), und „servicePrincipalTenantId“ ist die AAD-Mandanten-ID Ihres Unternehmens.

**Beispiel:**

```json
{
    "name": "Office365LinkedService",
    "properties": {
        "type": "Office365",
        "typeProperties": {
            "office365TenantId": "<Office 365 tenant id>",
            "servicePrincipalTenantId": "<AAD app service principal tenant id>",
            "servicePrincipalId": "<AAD app service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<AAD app service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Office 365-Dataset unterstützt werden.

Zum Kopieren von Daten aus Office 365 werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf Folgendes festgelegt werden: **Office365Table** | JA |
| tableName | Der Name des Datasets, das aus Office 365 extrahiert werden soll. [Hier](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#datasets) finden Sie die Liste der Office 365-Datasets, die für zum Extrahieren verfügbar sind. | JA |
| predicate | Ein Prädikatausdruck, der zum Filtern der Zeilen verwendet werden kann, die aus Office 365 extrahiert werden sollen.  [Hier](https://github.com/OfficeDev/MS-Graph-Data-Connect/wiki/Capabilities#filters) können Sie herausfinden, welche Spalten für die Prädikatfilter für jede Tabelle verwendet werden können und wie das Format des Filterausdrucks aussehen muss. | Nein <br>(Wenn kein Prädikat angegeben wird, ist die Standardeinstellung, Daten für die letzten 30 Tage zu extrahieren.) |

**Beispiel**

```json
{
    "name": "Office365Table1",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "structure": [
            {
                "name": "ReceivedDateTime",
                "type": "datetime"
            },
            {
                "name": "SentDateTime",
                "type": "datetime"
            },
            {
                "name": "HasAttachments",
                "type": "boolean"
            },
            {
                "name": "InternetMessageId",
                "type": "string"
            },
            {
                "name": "Subject",
                "type": "string"
            },
            {
                "name": "Importance",
                "type": "string"
            },
            {
                "name": "ParentFolderId",
                "type": "string"
            },
            {
                "name": "Sender",
                "type": "string"
            },
            {
                "name": "From",
                "type": "string"
            },
            {
                "name": "ToRecipients",
                "type": "string"
            },
            {
                "name": "CcRecipients",
                "type": "string"
            },
            {
                "name": "BccRecipients",
                "type": "string"
            },
            {
                "name": "ReplyTo",
                "type": "string"
            },
            {
                "name": "ConversationId",
                "type": "string"
            },
            {
                "name": "UniqueBody",
                "type": "string"
            },
            {
                "name": "IsDeliveryReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsReadReceiptRequested",
                "type": "boolean"
            },
            {
                "name": "IsRead",
                "type": "boolean"
            },
            {
                "name": "IsDraft",
                "type": "boolean"
            },
            {
                "name": "WebLink",
                "type": "string"
            },
            {
                "name": "CreatedDateTime",
                "type": "datetime"
            },
            {
                "name": "LastModifiedDateTime",
                "type": "datetime"
            },
            {
                "name": "ChangeKey",
                "type": "string"
            },
            {
                "name": "Categories",
                "type": "string"
            },
            {
                "name": "Id",
                "type": "string"
            },
            {
                "name": "Attachments",
                "type": "string"
            }
        ],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Contact_v0",
            "predicate": "CreatedDateTime >= 2018-07-11T16:00:00.000Z AND CreatedDateTime <= 2018-07-18T16:00:00.000Z"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Office 365-Quelle unterstützt werden.

### <a name="office-365-as-source"></a>Office 365 als Quelle

Legen Sie zum Kopieren von Daten aus Office 365 den Quelltyp in der Kopieraktivität auf **Office365Source** fest. Keine weiteren Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt.

**Beispiel:**

```json
"activities": [
    {
        "name": "CopyFromO365ToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Office 365 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "Office365Source"
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
