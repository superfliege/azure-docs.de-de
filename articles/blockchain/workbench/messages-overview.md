---
title: Übersicht über die Integration von Nachrichten in Azure Blockchain Workbench
description: Übersicht über die Verwendung von Nachrichten in Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/12/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: f8f3584475415cf9ca19458f6da78d34df37f438
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614360"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Integration von Nachrichten in Azure Blockchain Workbench

Neben der Bereitstellung einer REST-API bietet Azure Blockchain Workbench auch eine nachrichtenbasierte Integration. Workbench veröffentlicht ledgerorientierte Ereignisse über Azure Event Grid, sodass Downstreamconsumer Daten erfassen oder basierend auf diesen Ereignissen Aktionen ausführen können. Für Clients, die eine zuverlässige Nachrichtenübermittlung erfordern, liefert Azure Blockchain Workbench zudem Nachrichten an einen Azure Service Bus-Endpunkt.

## <a name="input-apis"></a>Eingabe-APIs

Wenn Sie Transaktionen von externen Systemen initiieren möchten, um Benutzer oder Verträge zu erstellen und Verträge zu aktualisieren, können Sie Transaktionen mithilfe von Messagingeingabe-APIs in einem Ledger durchführen. Ein Beispiel zu Eingabe-APIs finden Sie unter [Beispiele zur Integration von Nachrichten](https://aka.ms/blockchain-workbench-integration-sample).

Im Folgenden werden die derzeit verfügbaren Eingabe-APIs aufgeführt.

### <a name="create-user"></a>Benutzer erstellen

Diese Anforderung erstellt einen neuen Benutzer.

Für die Anforderung sind folgende Felder erforderlich:

| **Name**             | **Beschreibung**                                      |
|----------------------|------------------------------------------------------|
| requestId            | Vom Client bereitgestellte GUID                                |
| firstName            | Vorname des Benutzers                              |
| lastName             | Nachname des Benutzers                               |
| emailAddress         | E-Mail-Adresse des Benutzers                           |
| externalId           | Azure AD-Objekt-ID des Benutzers                      |
| connectionId         | Eindeutiger Bezeichner für die Blockchainverbindung |
| messageSchemaVersion | Schemaversion für Nachrichten                            |
| messageName          | **CreateUserRequest**                               |

Beispiel:

``` json
{
    "requestId": "e2264523-6147-41fc-bbbb-edba8e44562d",
    "firstName": "Ali",
    "lastName": "Alio",
    "emailAddress": "aa@contoso.com",
    "externalId": "6a9b7f65-ffff-442f-b3b8-58a35abd1bcd",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateUserRequest"
}
```

Blockchain Workbench gibt eine Antwort mit den folgenden Feldern zurück:

| **Name**              | **Beschreibung**                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| requestId             | Vom Client bereitgestellte GUID |
| userId                | ID des erstellten Benutzers |
| userChainIdentifier   | Adresse des Benutzers, der im Blockchainnetzwerk erstellt wurde. In Ethereum ist die Adresse des Benutzers  **on-chain** . |
| connectionId          | Eindeutiger Bezeichner für die Blockchainverbindung|
| messageSchemaVersion  | Schemaversion für Nachrichten |
| messageName           | **CreateUserUpdate** |
| status                | Status der Anforderung zum Erstellen des Benutzers.  Bei einer erfolgreichen Ausführung lautet der Wert **Success**. Bei einem Fehler lautet der Wert **Failure**.     |
| additionalInformation | Weitere Informationen basierend auf dem Status |

Beispiel für eine erfolgreiche Antwort auf **create user** von Blockchain Workbench:

``` json
{ 
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Success", 
    "additionalInformation": { } 
} 
```

Wenn die Anforderung nicht erfolgreich war, werden Details zum Fehler bei den zusätzlichen Informationen bereitgestellt.

``` json
{
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": null, 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Failure", 
    "additionalInformation": { 
        "errorCode": 4000, 
        "errorMessage": "User cannot be provisioned on connection." 
    }
}
```

### <a name="create-contract"></a>Erstellen eines Vertrags

Diese Anforderung erstellt einen neuen Vertrag.

Für die Anforderung sind folgende Felder erforderlich:

| **Name**             | **Beschreibung**                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId            | Vom Client bereitgestellte GUID |
| userChainIdentifier  | Adresse des Benutzers, der im Blockchainnetzwerk erstellt wurde. In Ethereum ist diese Adresse die **on chain**-Adresse des Benutzers. |
| applicationName      | Name der Anwendung |
| workflowName         | Name des Workflows |
| Parameter           | Parametereingabe für die Vertragserstellung |
| connectionId         | Eindeutiger Bezeichner für die Blockchainverbindung |
| messageSchemaVersion | Schemaversion für Nachrichten |
| messageName          | **CreateContractRequest** |

Beispiel:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer", 
    "workflowName": "AssetTransfer", 
    "parameters": [ 
        { 
            "name": "description", 
            "value": "a 1969 dodge charger" 
        }, 
        { 
            "name": "price", 
            "value": "12345" 
        } 
    ], 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateContractRequest" 
}
```

Blockchain Workbench gibt eine Antwort mit den folgenden Feldern zurück:

| **Name**                 | **Beschreibung**                                                                   |
|--------------------------|-----------------------------------------------------------------------------------|
| requestId                | Vom Client bereitgestellte GUID                                                             |
| contractId               | Eindeutiger Bezeichner für den Vertrag innerhalb von Azure Blockchain Workbench. |
| contractLedgerIdentifier | Adresse des Vertrags im Ledger                                            |
| connectionId             | Eindeutiger Bezeichner für die Blockchainverbindung                               |
| messageSchemaVersion     | Schemaversion für Nachrichten                                                         |
| messageName              | **CreateContractUpdate**                                                      |
| status                   | Status der Anforderung zum Erstellen des Vertrags.  Mögliche Werte: **Submitted**, **Committed**, **Failure**.  |
| additionalInformation    | Weitere Informationen basierend auf dem Status                              |

Beispiel für eine übermittelte Antwort auf **create contract** von Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Submitted"
    "additionalInformation": { }
}
```

Beispiel für eine committete Antwort auf **create contract** von Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

Wenn die Anforderung nicht erfolgreich war, werden Details zum Fehler bei den zusätzlichen Informationen bereitgestellt.

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": null,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract cannot be provisioned on connection."
    }
}
```

### <a name="create-contract-action"></a>Aktion zum Erstellen eines Vertrags

Diese Anforderung erstellt eine neue Vertragsaktion.

Für die Anforderung sind folgende Felder erforderlich:

| **Name**                 | **Beschreibung**                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId                | Vom Client bereitgestellte GUID |
| userChainIdentifier      | Adresse des Benutzers, der im Blockchainnetzwerk erstellt wurde. In Ethereum ist dies die **on chain**-Adresse des Benutzers. |
| contractLedgerIdentifier | Adresse des Vertrags im Ledger |
| workflowFunctionName     | Name der Workflowfunktion |
| Parameter               | Parametereingabe für die Vertragserstellung |
| connectionId             | Eindeutiger Bezeichner für die Blockchainverbindung |
| messageSchemaVersion     | Schemaversion für Nachrichten |
| messageName              | **CreateContractActionRequest** |

Beispiel:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "workflowFunctionName": "modify",
    "parameters": [
        {
            "name": "description",
            "value": "a 1969 dodge charger"
        },
        {
            "name": "price",
            "value": "12345"
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionRequest"
}
```

Blockchain Workbench gibt eine Antwort mit den folgenden Feldern zurück:

| **Name**              | **Beschreibung**                                                                   |
|-----------------------|-----------------------------------------------------------------------------------|
| requestId             | Vom Client bereitgestellte GUID|
| contractId            | Eindeutiger Bezeichner für den Vertrag innerhalb von Azure Blockchain Workbench. |
| connectionId          | Eindeutiger Bezeichner für die Blockchainverbindung |
| messageSchemaVersion  | Schemaversion für Nachrichten |
| messageName           | **CreateContractActionUpdate** |
| status                | Status der Anforderung zum Erstellen der Vertragsaktion. Mögliche Werte: **Submitted**, **Committed**, **Failure**.                         |
| additionalInformation | Weitere Informationen basierend auf dem Status |

Beispiel für eine übermittelte Antwort auf **create contract action** von Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Beispiel für eine committete Antwort auf **create contract action** von Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Committed"
    "additionalInformation": { }
}
```

Wenn die Anforderung nicht erfolgreich war, werden Details zum Fehler bei den zusätzlichen Informationen bereitgestellt.

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract action cannot be provisioned on connection."
    }
}
```

### <a name="input-api-error-codes-and-messages"></a>Eingabe-API-Fehlercodes und -meldungen

**Fehlercode 4000: Fehler – Ungültige Anforderung**
- Ungültige Verbindungs-ID
- Fehler bei der Deserialisierung von CreateUserRequest
- Fehler bei der Deserialisierung von CreateContractRequest
- Fehler bei der Deserialisierung von CreateContractActionRequest
- Die Anwendung {identifiziert durch Anwendungsnamen} ist nicht vorhanden.
- Die Anwendung {identifiziert durch Anwendungsnamen} enthält keinen Workflow.
- UserChainIdentifier ist nicht vorhanden.
- Der Vertrag {identifiziert durch Ledger-ID} ist nicht vorhanden.
- Der Vertrag {identifiziert durch Ledger-ID} enthält keine Funktion {Name der Workflowfunktion}
- UserChainIdentifier ist nicht vorhanden.

**Fehlercode 4090: Konfliktfehler**
- Der Benutzer ist bereits vorhanden.
- Der Vertrag ist bereits vorhanden.
- Die Vertragsaktion ist bereits vorhanden.

**Fehlercode 5000: Interner Serverfehler**
- Ausnahmemeldungen

## <a name="event-notifications"></a>Ereignisbenachrichtigungen

Durch Ereignisbenachrichtigungen können Benutzer und Downstreamsysteme über Ereignisse informiert werden, die in Blockchain Workbench und dem damit verbundenen Blockchainnetzwerk auftreten. Ereignisbenachrichtigungen können direkt im Code genutzt oder mit Tools wie Logic Apps und Flow verwendet werden, um den Datenfluss zu Downstreamsystemen auszulösen.

Weitere Informationen zu den verschiedenen Nachrichten, die empfangen werden können, finden Sie unter [Referenz zur Benachrichtigungsnachricht](#notification-message-reference).

### <a name="consuming-event-grid-events-with-azure-functions"></a>Nutzen von Event Grid-Ereignissen mit Azure Functions

Wenn ein Benutzer mit Event Grid über Ereignisse in Blockchain Workbench benachrichtigt werden möchte, können Sie Ereignisse aus Event Grid mithilfe von Azure Functions nutzen.

1. Erstellen Sie im Azure-Portal eine **Azure-Funktionen-App**.
2. Erstellen Sie eine neue Funktion.
3. Suchen Sie die Vorlage für Event Grid. Der standardmäßige Vorlagencode zum Lesen der Nachricht wird angezeigt. Ändern Sie den Code nach Bedarf.
4. Speichern Sie die Funktion. 
5. Wählen Sie das Event Grid aus der Blockchain Workbench-Ressourcengruppe aus.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Nutzen von Event Grid-Ereignissen mit Logic Apps

1.  Erstellen Sie im Azure-Portal eine neue **Azure Logic-App**.
2.  Beim Öffnen der Azure Logic-App im Portal werden Sie aufgefordert, einen Trigger auszuwählen. Wählen Sie **Azure Event Grid – Wenn ein Ressourcenereignis eintritt** aus.
3. Wenn der Workflow-Designer angezeigt wird, werden Sie aufgefordert, sich anzumelden.
4. Wählen Sie das Abonnement aus. Ressource als **Microsoft.EventGrid.Topics**. Wählen Sie den **Ressourcennamen** aus dem Namen der Ressource aus der Azure Blockchain Workbench-Ressourcengruppe aus.
5. Wählen Sie das Event Grid aus der Blockchain Workbench-Ressourcengruppe aus.

## <a name="using-service-bus-topics-for-notifications"></a>Verwenden von Service Bus-Themen für Benachrichtigungen

Mithilfe von Service Bus-Themen können Benutzer über Ereignisse in Blockchain Workbench benachrichtigt werden. 

1.  Navigieren Sie zum Service Bus innerhalb der Workbench-Ressourcengruppe.
2.  Wählen Sie **Themen** aus.
3.  Wählen Sie **workbench-external** aus.
4.  Erstellen Sie ein neues Abonnement zu diesem Thema. Erhalten Sie dafür einen Schlüssel.
5.  Erstellen Sie ein Programm, das Ereignisse aus diesem Abonnement abonniert.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Nutzen von Service Bus-Nachrichten mit Logic Apps

1. Erstellen Sie im Azure-Portal eine neue **Azure Logic-App**.
2. Beim Öffnen der Azure Logic-App im Portal werden Sie aufgefordert, einen Trigger auszuwählen. Geben Sie in das Suchfeld **Service Bus** ein, und wählen Sie den passenden Trigger für den Typ der Interaktion mit dem Service Bus. Zum Beispiel, **Service Bus – Wenn eine Nachricht in einem Themenabonnement empfangen wird (automatisch abschließen)**.
3. Wenn der Workflow-Designer angezeigt wird, geben Sie die Verbindungsinformationen für den Service Bus an.
4. Wählen Sie Ihr Abonnement aus, und legen Sie das Thema **workbench-external** fest.
5. Entwickeln Sie die Logik für Ihre Anwendung, die die Nachricht von diesem Trigger verwendet.

## <a name="notification-message-reference"></a>Referenz zur Benachrichtigungsnachricht

Abhängig von **OperationName** weisen die Benachrichtigungen einen der folgenden Nachrichtentypen auf:

### <a name="accountcreated"></a>AccountCreated

Zeigt an, dass ein neues Konto zur angegebenen Blockchain hinzugefügt werden soll.

| NAME    | BESCHREIBUNG  |
|----------|--------------|
| UserId  | ID des erstellten Benutzers. |
| ChainIdentifier | Adresse des Benutzers, der im Blockchainnetzwerk erstellt wurde. In Ethereum wäre dies die **on-chain**-Adresse des Benutzers. |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Gibt an, dass eine Anforderung zum Einfügen oder Aktualisieren eines Vertrags in ein Distributed Ledger vorliegt.

| NAME | BESCHREIBUNG |
|-----|--------------|
| ChainID | Eindeutiger Bezeichner für die mit der Anforderung verknüpfte Kette |
| BlockId | Eindeutiger Bezeichner für einen Block im Ledger |
| ContractId | Ein eindeutiger Bezeichner für den Vertrag |
| ContractAddress |       Die Adresse des Vertrags im Ledger |
| TransactionHash  |     Der Hash der Transaktion im Ledger |
| OriginatingAddress |   Die Adresse des Erstellers der Transaktion |
| ActionName       |     Der Name der Aktion |
| IsUpdate        |      Identifiziert, ob es sich um ein Update handelt. |
| Parameter       |     Eine Liste von Objekten, die den Namen, den Wert und den Datentyp der an eine Aktion gesendeten Parameter identifizieren. |
| TopLevelInputParams |  In Szenarien, in denen ein Vertrag mit mindestens einem anderen Vertrag verbunden ist, sind dies die Parameter aus dem übergeordneten Vertrag. |

``` csharp
public class ContractInsertOrUpdateRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public int ContractId { get; set; }
    public string ContractAddress { get; set; }
    public string TransactionHash { get; set; }
    public string OriginatingAddress { get; set; }
    public string ActionName { get; set; }
    public bool IsUpdate { get; set; }
    public List<ContractProperty> Parameters { get; set; }
    public bool IsTopLevelUpdate { get; set; }
    public List<ContractInputParameter> TopLevelInputParams { get; set; }
}
```

#### <a name="updatecontractaction"></a>UpdateContractAction

Gibt an, dass eine Anforderung zum Ausführen einer Aktion für einen bestimmten Vertrag in einem Distributed Ledger vorliegt.

| NAME                     | BESCHREIBUNG                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | Eindeutiger Bezeichner für diese Vertragsaktion |
| ChainIdentifier          | Eindeutiger Bezeichner für die Kette |
| ConnectionId             | Eindeutiger Bezeichner für die Verbindung |
| UserChainIdentifier      | Adresse des Benutzers, der im Blockchainnetzwerk erstellt wurde. In Ethereum ist diese Adresse die **on chain**-Adresse des Benutzers. |
| ContractLedgerIdentifier | Adresse des Vertrags im Ledger |
| WorkflowFunctionName     | Name der Workflowfunktion |
| WorkflowName             | Name des Workflows |
| WorkflowBlobStorageURL   | Die URL des Vertrags in Blob Storage |
| ContractActionParameters | Parameter für die Vertragsaktion |
| TransactionHash          | Der Hash der Transaktion im Ledger |
| Bereitstellungsstatus      | Der aktuelle Bereitstellungsstatus der Aktion</br>0 – Erstellt</br>1 – In Bearbeitung</br>2 – Abgeschlossen</br> Der Status „Abgeschlossen“ weist auf eine Bestätigung aus dem Ledger hin, dass dies erfolgreich hinzugefügt wurde. |

```csharp
public class ContractActionRequest : MessageModelBase
{
    public int ContractActionId { get; set; }
    public int ConnectionId { get; set; }
    public string UserChainIdentifier { get; set; }
    public string ContractLedgerIdentifier { get; set; }
    public string WorkflowFunctionName { get; set; }
    public string WorkflowName { get; set; }
    public string WorkflowBlobStorageURL { get; set; }
    public IEnumerable<ContractActionParameter> ContractActionParameters { get; set; }
    public string TransactionHash { get; set; }
    public int ProvisioningStatus { get; set; }
}
```

### <a name="updateuserbalance"></a>UpdateUserBalance

Gibt an, dass eine Anforderung zum Aktualisieren des Benutzerguthabens in einem bestimmten Distributed Ledger gesendet wurde.

> [!NOTE]
> Diese Nachricht wird nur für diejenigen Ledger generiert, für die eine Kontenfinanzierung erforderlich ist.
> 

| NAME    | BESCHREIBUNG                              |
|---------|------------------------------------------|
| Adresse | Die Adresse des finanzierten Benutzers |
| Balance | Das Guthaben des Benutzers         |
| ChainID | Eindeutiger Bezeichner für die Kette     |


``` csharp
public class UpdateUserBalanceRequest : MessageModelBase
{
    public string Address { get; set; }
    public decimal Balance { get; set; }
    public int ChainID { get; set; }
}
```

### <a name="insertblock"></a>InsertBlock

Die Nachricht, die angibt, dass eine Anforderung zum Hinzufügen eines Blocks in einem Distributed Ledger vorliegt.

| NAME           | BESCHREIBUNG                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | Eindeutiger Bezeichner der Kette, zu der der Block hinzugefügt wurde             |
| BlockId        | Eindeutiger Bezeichner für den Block innerhalb von Azure Blockchain Workbench |
| BlockHash      | Der Hash des Blocks                                                 |
| BlockTimeStamp | Der Zeitstempel des Blocks                                            |

``` csharp
public class InsertBlockRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string BlockHash { get; set; }
    public int BlockTimestamp { get; set; }
}
```

### <a name="inserttransaction"></a>InsertTransaction

Die Nachricht, die Details über eine Anforderung zum Hinzufügen eines Übergangs in einem Distributed Ledger enthält.

| NAME            | BESCHREIBUNG                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | Eindeutiger Bezeichner der Kette, zu der der Block hinzugefügt wurde             |
| BlockId         | Eindeutiger Bezeichner für den Block innerhalb von Azure Blockchain Workbench |
| TransactionHash | Der Hash der Transaktion                                           |
| From            | Die Adresse des Erstellers der Transaktion                      |
| To              | Die Adresse des beabsichtigten Empfängers der Transaktion              |
| Wert           | Der in der Transaktion enthaltene Wert                                 |
| IsAppBuilderTx  | Identifiziert, ob dies eine Blockchain Workbench-Transkation ist                         |

``` csharp
public class InsertTransactionRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string TransactionHash { get; set; }
    public string From { get; set; }
    public string To { get; set; }
    public decimal Value { get; set; }
    public bool IsAppBuilderTx { get; set; }
}
```

### <a name="assigncontractchainidentifier"></a>AssignContractChainIdentifier

Enthält Details über die Zuordnung eines Kettenbezeichners für einen Vertrag. In einer Ethereum-Blockchain wäre dies beispielsweise die Adresse eines Vertrags im Ledger.

| NAME            | BESCHREIBUNG                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | Eindeutiger Bezeichner für den Vertrag innerhalb von Azure Blockchain Workbench. |
| ChainIdentifier | Bezeichner für den Vertrag in der Kette                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="classes-used-by-message-types"></a>Von Nachrichtentypen verwendete Klassen

### <a name="messagemodelbase"></a>MessageModelBase

Das Basismodell für alle Nachrichten.

| NAME          | BESCHREIBUNG                          |
|---------------|--------------------------------------|
| NameVorgang | Name des Vorgangs           |
| RequestId     | Eindeutiger Bezeichner für die Anforderung |

``` csharp
public class MessageModelBase
{
    public string OperationName { get; set; }
    public string RequestId { get; set; }
}
```

### <a name="contractinputparameter"></a>ContractInputParameter

Enthält Name, Wert und Typ eines Parameters.

| NAME  | BESCHREIBUNG                 |
|-------|-----------------------------|
| NAME  | Der Name des Parameters  |
| Wert | Der Wert des Parameters |
| Typ  | Der Typ des Parameters  |

``` csharp
public class ContractInputParameter
{
    public string Name { get; set; }
    public string Value { get; set; }
    public string Type { get; set; }
}
```

#### <a name="contractproperty"></a>ContractProperty

Enthält ID, Name, Wert und Typ einer Eigenschaft.

| NAME  | BESCHREIBUNG                |
|-------|----------------------------|
| id    | Die ID der Eigenschaft    |
| NAME  | Der Name der Eigenschaft.  |
| Wert | Der Wert der Eigenschaft. |
| Typ  | Der Typ der Eigenschaft  |

``` csharp
public class ContractProperty
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Value { get; set; }
    public string DataType { get; set; }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Smart Contract-Integrationsmuster](integration-patterns.md)