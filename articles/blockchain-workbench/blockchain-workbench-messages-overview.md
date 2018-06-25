---
title: Übersicht über die Integration von Nachrichten in Azure Blockchain Workbench
description: Übersicht über die Verwendung von Nachrichten in Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: f45396c3af285026e16ce641bd37bf0eadcee56d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607599"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Integration von Nachrichten in Azure Blockchain Workbench

Neben der Bereitstellung einer REST-API bietet Azure Blockchain Workbench auch eine nachrichtenbasierte Integration. Workbench veröffentlicht ledgerorientierte Ereignisse über Azure Event Grid, sodass Downstreamconsumer Daten erfassen oder basierend auf diesen Ereignissen Aktionen ausführen können. Für Clients, die eine zuverlässige Nachrichtenübermittlung erfordern, liefert Azure Blockchain Workbench zudem Nachrichten an einen Azure Service Bus-Endpunkt.

Entwickler haben auch Interesse an der Möglichkeit geäußert, dass externe Systeme Transaktionen initiieren, um Benutzer und Verträge zu erstellen sowie Verträge in einem Ledger zu aktualisieren. Obwohl diese Funktionalität derzeit nicht in der öffentlichen Vorschau enthalten ist, finden Sie ein Beispiel zu dieser Funktion unter [http://aka.ms/blockchain-workbench-integration-sample](http://aka.ms/blockchain-workbench-integration-sample).

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
| ChainID | Ein eindeutiger Bezeichner für die mit der Anforderung verknüpften Kette|
| BlockId | Der eindeutige Bezeichner für einen Block im Ledger|
| ContractId | Ein eindeutiger Bezeichner für den Vertrag|
| ContractAddress |       Die Adresse des Vertrags im Ledger|
| TransactionHash  |     Der Hash des Übergangs zum Ledger|
| OriginatingAddress |   Die Adresse des Erstellers des Übergangs|
| ActionName       |     Der Name der Aktion|
| IsUpdate        |      Identifiziert, ob es sich um ein Update handelt.|
| Parameter       |     Eine Liste von Objekten, die den Namen, den Wert und den Datentyp der an eine Aktion gesendeten Parameter identifizieren.|
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
| ContractActionId         | Der eindeutige Bezeichner für diese Vertragsaktion                                                                                                                                |
| ChainIdentifier          | Der eindeutige Bezeichner für diese Kette                                                                                                                                           |
| ConnectionId             | Der eindeutige Bezeichner für die Verbindung                                                                                                                                      |
| UserChainIdentifier      | Adresse des Benutzers, der im Blockchainnetzwerk erstellt wurde. In Ethereum wäre dies die „On-Chain“-Adresse des Benutzers.                                                     |
| ContractLedgerIdentifier | Die Adresse des Vertrags im Ledger                                                                                                                                        |
| WorkflowFunctionName     | Der Name der Workflowfunktion                                                                                                                                                |
| WorkflowName             | Der Name des Workflows                                                                                                                                                         |
| WorkflowBlobStorageURL   | Die URL des Vertrags im Blobspeicher                                                                                                                                      |
| ContractActionParameters | Parameter für die Vertragsaktion                                                                                                                                           |
| TransactionHash          | Der Hash des Übergangs zum Ledger                                                                                                                                    |
| Bereitstellungsstatus      | Der aktuelle Bereitstellungsstatus der Aktion</br>0 – Erstellt</br>1 – In Bearbeitung</br>2 – Abgeschlossen</br> Der Status „Abgeschlossen“ weist auf eine Bestätigung aus dem Ledger hin, dass dies erfolgreich hinzugefügt wurde.                                               |
|                          |                                                                                                                                                                               |

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
| ChainID | Der eindeutige Bezeichner für die Kette     |


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
| ChainId        | Der eindeutige Bezeichner der Kette, zu der der Block hinzugefügt wurde.             |
| BlockId        | Der eindeutige Bezeichner für den Block innerhalb von Azure Blockchain Workbench |
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
| ChainId         | Der eindeutige Bezeichner der Kette, zu der der Block hinzugefügt wurde.             |
| BlockId         | Der eindeutige Bezeichner für den Block innerhalb von Azure Blockchain Workbench |
| TransactionHash | Der Hash der Transaktion                                           |
| From            | Die Adresse des Erstellers des Übergangs                      |
| To              | Die Adresse des beabsichtigten Empfängers der Transaktion              |
| Wert           | Der in der Transaktion enthaltene Wert                                 |
| IsAppBuilderTx  | Identifiziert, ob dies eine Blockchain Workbench-Transkation ist.                         |

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
| ContractId      | Dies ist der eindeutige Bezeichner für den Vertrag innerhalb von Azure Blockchain Workbench. |
| ChainIdentifier | Dies ist der Bezeichner für den Vertrag in der Kette.                             |

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
| NameVorgang | Der Name des Vorgangs.           |
| RequestId     | Ein eindeutiger Bezeichner für die Anforderung. |

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
| NAME  | Der Name des Parameters.  |
| Wert | Der Wert des Parameters. |
| Typ  | Der Typ des Parameters.  |

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
| id    | Die ID der Eigenschaft.    |
| NAME  | Der Name der Eigenschaft.  |
| Wert | Der Wert der Eigenschaft. |
| Typ  | Der Typ der Eigenschaft.  |

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
> [Smart Contract-Integrationsmuster](blockchain-workbench-integration-patterns.md)