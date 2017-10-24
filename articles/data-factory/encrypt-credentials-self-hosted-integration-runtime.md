---
title: "Verschlüsseln von Anmeldeinformationen in Azure Data Factory | Microsoft-Dokumentation"
description: "Erhalten Sie Informationen zum Verschlüsseln und Speichern von Anmeldeinformationen für Ihre lokalen Datenspeicher auf einem Computer mit selbstgehosteter Integrationslaufzeit."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: abnarain
ms.openlocfilehash: 72a928455e4710b43553fc596a94f6c55a6b5bfa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Verschlüsseln von Anmeldeinformationen für lokale Datenspeicher in Azure Data Factory
Sie können Anmeldeinformationen für Ihre lokalen Datenspeicher (verknüpfte Dienste mit vertraulichen Informationen) auf einem Computer mit selbstgehosteter Integrationslaufzeit verschlüsseln und speichern. 

Sie übergeben eine JSON-Definitionsdatei mit Anmeldeinformationen an das <br/>[**New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential**](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0)-Cmdlet, um eine JSON-Ausgabedefinitionsdatei mit den verschlüsselten Anmeldeinformationen zu erstellen. Verwenden Sie dann die aktualisierte JSON-Definition, um die verknüpften Dienste zu erstellen.

## <a name="author-sql-server-linked-service"></a>Verfassen eines mit SQL Server verknüpften Diensts
Erstellen Sie in einem beliebigen Ordner eine JSON-Datei namens **SqlServerLinkedService.json** mit dem folgenden Inhalt:  

Ersetzen Sie `<servername>`, `<databasename>`, `<username>` und `<password>` durch Werte für Ihren SQL Server, bevor die Datei gespeichert wird. Ersetzen Sie zudem `<integration runtime name>` durch den Namen Ihrer Integrationslaufzeit. 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
            }
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Verschlüsseln von Anmeldeinformationen
Um vertrauliche Daten aus der JSON-Nutzlast in einer lokalen selbstgehosteten Integrationslaufzeit zu verschlüsseln, führen Sie **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** aus, und übergeben Sie die JSON-Nutzlast. Dieses Cmdlet stellt sicher, dass die Anmeldeinformationen per DPAPI verschlüsselt und lokal auf dem Knoten der selbstgehosteten Integrationslaufzeit gespeichert werden. Die Ausgabenutzlast kann an eine weitere JSON-Datei umgeleitet werden (in diesem Fall „encryptedLinkedService.json“), die verschlüsselte Anmeldeinformationen enthält.

```powershell
New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Verwenden der JSON-Datei mit verschlüsselten Anmeldeinformationen
Jetzt verwenden Sie die JSON-Ausgabedatei aus dem vorherigen Befehl, der die verschlüsselten Anmeldeinformationen enthält, um **SqlServerLinkedService** einzurichten.

```powershell
Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Nächste Schritte
Informationen zu Sicherheitsüberlegungen für das Verschieben von Daten finden Sie unter [Sicherheitsüberlegungen zur Datenverschiebung](data-movement-security-considerations.md).

