---
title: Bindungen zu externer Tabelle für Azure Functions (experimentell)
description: Verwenden von Bindungen zu externen Tabellen in Azure Functions
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 24728414747d8ad8a8d7ee0d8a21be2177a15ddd
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093811"
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Bindungen zu externer Tabelle für Azure Functions (experimentell)

In diesem Artikel wird erläutert, wie mit tabellarischen Daten aus SaaS-Anbietern, z. B. Sharepoint und Dynamics, in Azure Functions gearbeitet wird. Azure Functions unterstützt Eingabe- und Ausgabebindungen für externe Tabellen.

> [!IMPORTANT]
> Die Bindung zu externer Tabelle ist experimentell und erreicht möglicherweise nie den Status „Allgemein verfügbar“. Die Bindung ist nur in Azure Functions 1.x enthalten, und es ist nicht geplant, sie zu Azure Functions 2.x hinzuzufügen. Für Szenarien, in denen Zugriff auf Daten in SaaS-Anbietern erforderlich ist, empfiehlt sich das Verwenden von [Logik-Apps, aus denen Funktionen aufgerufen werden](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API-Verbindungen

Für Tabellenbindungen werden externe API-Verbindungen genutzt, um Authentifizierungen bei SaaS-Anbietern anderer Hersteller vorzunehmen. 

Wenn Sie eine Bindung zuweisen, können Sie entweder eine neue API-Verbindung erstellen oder eine vorhandene API-Verbindung in derselben Ressourcengruppe verwenden.

### <a name="available-api-connections-tables"></a>Verfügbare API-Verbindungen (Tabellen)

|Connector|Trigger|Eingabe|Output|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 for Operations](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Google Sheets](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 for Financials](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle-Datenbank](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Common Data Service](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x

> [!NOTE]
> Verbindungen mit externen Tabellen können auch in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list) verwendet werden.

## <a name="creating-an-api-connection-step-by-step"></a>Erstellen eine API-Verbindung: Schritt für Schritt

1. Wählen Sie auf der Azure-Portalseite für Ihre Funktions-App das Pluszeichen (**+**) aus, um eine Funktion zu erstellen.

1. Wählen Sie im Feld **Szenario** die Option **Experimentell** aus.

1. Wählen Sie **Externe Tabelle** aus.

1. Wählen Sie eine Spracheaus.

2. Wählen Sie unter **Verbindung mit externer Tabelle** eine vorhandene Verbindung aus, oder wählen Sie **Neu** aus.

1. Für eine neue Verbindung konfigurieren Sie die Einstellungen, und wählen Sie **Autorisieren** aus.

1. Wählen Sie **Erstellen** aus, um die Funktion zu erstellen.

1. Wählen Sie **Integrieren > Externe Tabelle** aus.

1. Konfigurieren Sie die Verbindung, die für die Zieltabelle verwendet werden soll. Diese Einstellungen sind je nach SaaS-Anbieter unterschiedlich. Im folgenden Abschnitt sind Beispiele enthalten.

## <a name="example"></a>Beispiel

In diesem Beispiel wird eine Verbindung mit einer Tabelle namens „Contact“ hergestellt, die die Spalten „Id“, „LastName“ und „FirstName“ hat. Im Code werden die in der Tabelle enthaltenen Kontaktelemente aufgelistet sowie die Vor- und Nachnamen protokolliert.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retrieve table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

### <a name="sql-server-data-source"></a>SQL Server-Datenquelle

Mit dem folgenden Skript kann in SQL Server eine Tabelle erstellt werden, die sich für das folgende Beispiel verwenden lässt. `dataSetName` ist gleich „default“.

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets-data-source"></a>Google Tabellen-Datenquelle

Um eine Tabelle zu erstellen, die mit diesem Beispiel in Google Docs verwendet werden kann, erstellen Sie ein Arbeitsblatt mit einem Tabellenblatt namens `Contact`. Der Connector kann den Anzeigenamen der Tabelle nicht verwenden. Der interne Name (in Fettschrift) muss als dataSetName verwendet werden. Beispiel: `docs.google.com/spreadsheets/d/` **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** Fügen Sie die Spaltennamen `Id`, `LastName`, `FirstName` zur erste Zeile hinzu, und füllen Sie dann die nachfolgenden Zeilen mit Daten.

### <a name="salesforce"></a>Salesforce

Um dieses Beispiel mit „Salesforce“ zu verwenden, ist `dataSetName` gleich „default“.

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen.

|Eigenschaft von „function.json“ | BESCHREIBUNG|
|---------|----------------------|
|**type** | Muss auf `apiHubTable` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen.|
|**direction** | Muss auf `in` festgelegt sein. Diese Eigenschaft wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** | Der Name der Variablen, die das Ereigniselement im Funktionscode darstellt. | 
|**Verbindung**| Kennzeichnet die App-Einstellung, in der die API-Verbindungszeichenfolge gespeichert wird. Die App-Einstellung wird automatisch erstellt, wenn Sie in der Benutzeroberfläche für „Integrieren“ eine API-Verbindung hinzufügen.|
|**dataSetName**|Der Name des Datasets, das die zu lesende Tabelle enthält.|
|**tableName**|Der Name der Tabelle.|
|**entityId**|Muss für Tabellenbindungen leer sein.

Ein tabellarischer Connector stellt Datasets bereit, und jedes Dataset enthält Tabellen. Der Name des Standarddatasets ist „default“. Die Titel für ein Dataset und eine Tabelle in verschiedene SaaS-Anbietern sind nachstehend aufgeführt:

|Connector|Datensatz|Table|
|:-----|:---|:---| 
|**SharePoint**|Website|SharePoint-Liste
|**SQL**|Datenbank|Table 
|**Google Sheet**|Spreadsheet|Worksheet 
|**Excel**|Excel-Datei|Tabelle 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
