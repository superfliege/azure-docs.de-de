---
title: Referenz zur Azure Blockchain Workbench-Konfiguration
description: Übersicht über die Anwendungskonfiguration in Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 3/20/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 6318b6aafda972242239ce91abb0f67a15721e1e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="azure-blockchain-workbench-configuration-reference"></a>Referenz zur Azure Blockchain Workbench-Konfiguration

 Azure Blockchain Workbench-Anwendungen sind Mehrparteienworkflows, die durch Konfigurationsmetadaten und intelligenten Vertragscode definiert sind. Konfigurationsmetadaten definieren die allgemeinen Workflows und das Interaktionsmodell der Blockchainanwendung. Intelligente Verträge stellen die Geschäftslogik der Blockchainanwendung dar. Mithilfe von Konfigurationsdaten und intelligentem Vertragscode generiert Workbench Benutzeroberflächen von Blockchainanwendungen.

In den Konfigurationsmetadaten sind für jede Blockchainanwendung die folgenden Informationen festgelegt: 

* Name und Beschreibung der Blockchainanwendung
* Eindeutige Rollen für Benutzer, die innerhalb der Blockchainanwendung agieren oder teilnehmen können.
* Mindestens ein Workflow. Jeder Workflow fungiert als Zustandsautomat, um den Ablauf der Geschäftslogik zu steuern. Workflows können unabhängig sein oder miteinander interagieren.

Jeder definierte Workflow legt Folgendes fest:

* Name und Beschreibung des Workflows
* Zustände des Workflows.  Jeder Zustand ist eine Stufe in der Ablaufsteuerung der Geschäftslogik. 
* Aktionen für den Übergang zum nächsten Zustand
* Benutzerrollen, die jede Aktion initiieren dürfen.
* Intelligente Verträge, die die Geschäftslogik in Codedateien darstellen.

## <a name="application"></a>Anwendung

Eine Blockchainanwendung enthält Konfigurationsmetadaten, Workflows und Benutzerrollen, die innerhalb der Anwendung agieren oder teilnehmen können.

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| ApplicationName | Eindeutiger Anwendungsname. Der entsprechende intelligente Vertrag muss den gleichen **Anwendungsnamen** für die jeweilige Vertragsklasse verwenden.  | Ja |
| DisplayName | Der Anzeigename der Anwendung | Ja |
| BESCHREIBUNG | Beschreibung der Anwendung | Nein  |
| ApplicationRoles | Sammlung von [Anwendungsrollen](#application-roles). Benutzerrollen, die innerhalb der Anwendung agieren oder teilnehmen können.  | Ja |
| Workflows | Sammlung von [Workflows](#workflows). Jeder Workflow fungiert als Zustandsautomat, um den Ablauf der Geschäftslogik zu steuern. | Ja |

Ein Beispiel finden Sie unter [Beispielkonfigurationsdatei](#configuration-file-example).

## <a name="workflows"></a>Workflows

Die Geschäftslogik einer Anwendung kann als Zustandsautomat modelliert werden, bei der eine Aktion bewirkt, dass während des Ablaufs der Geschäftslogik von einem Zustand in einen anderen gewechselt wird. Ein Workflow ist eine Sammlung solcher Zustände und Aktionen. Jeder Workflow besteht aus mindestens einem intelligenten Vertrag, der die Geschäftslogik in Codedateien darstellt. Bei einem ausführbaren Vertrag handelt es sich um eine Instanz eines Workflows.

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| NAME | Eindeutiger Workflowname. Der entsprechende intelligente Vertrag muss den gleichen **Namen** für die jeweilige Vertragsklasse verwenden. | Ja |
| DisplayName | Der Anzeigename des Workflows | Ja |
| BESCHREIBUNG | Die Beschreibung des Workflows | Nein  |
| Initiators | Sammlung von [Anwendungsrollen](#application-roles). Rollen, die Benutzern zugeordnet sind, die zum Erstellen von Verträgen im Workflow berechtigt sind. | Ja |
| StartState | Der Name des Anfangszustands des Workflows | Ja |
| Eigenschaften | Sammlung von [Bezeichnern](#identifiers). Stellt Daten dar, die Off-Chain gelesen oder in einem Tool zum Erstellen von Benutzeroberflächen visualisiert werden können. | Ja |
| Konstruktor | Definiert Eingabeparameter für die Erstellung einer Instanz des Workflows. | Ja |
| Functions | Eine Sammlung von [Funktionen](#functions), die im Workflow ausgeführt werden können. | Ja |
| Zustände | Eine Sammlung von [Zuständen](#states) des Workflows. | Ja |

Ein Beispiel finden Sie unter [Beispielkonfigurationsdatei](#configuration-file-example).

## <a name="type"></a>Typ

Unterstützte Datentypen:

| Typ | BESCHREIBUNG |
|-------|-------------|
| address  | Blockchainadresstyp, z.B. *Verträge* oder *Benutzer* |
| bool     | Boolean-Datentyp |
| contract | Adresse des Vertragstyps |
| int      | Integer-Datentyp |
| money    | Money-Datentyp |
| state    | Workflowstatus |
| Zeichenfolge   | String-Datentyp |
| user     | Adresse des Benutzertyps |
| time     | Time-Datentyp |
|`[ Application Role Name ]`| Ein beliebiger in der Anwendungsrolle festgelegter Name. Legt fest, dass Benutzer zu diesem Rollentyp gehören müssen. |

## <a name="constructor"></a>Konstruktor

Definiert Eingabeparameter für eine Instanz eines Workflows.

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| Parameter | Sammlung von [Bezeichnern](#identifiers), die zum Initiieren eines intelligenten Vertrags erforderlich sind. | Ja |

### <a name="constructor-example"></a>Konstruktorbeispiel

``` json
{
  "Parameters": [
    {
      "Name": "description",
      "Description": "The description of this asset",
      "DisplayName": "Description",
      "Type": {
        "Name": "string"
      }
    },
    {
      "Name": "price",
      "Description": "The price of this asset",
      "DisplayName": "Price",
      "Type": {
        "Name": "money"
      }
    }
  ]
}

```

## <a name="functions"></a>Functions

Definiert Funktionen, die im Workflow ausgeführt werden können.

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| NAME | Der eindeutige Name der Funktion. Der entsprechende intelligente Vertrag muss den gleichen **Namen** für die jeweilige Funktion verwenden. | Ja |
| DisplayName | Der Anzeigename der Funktion | Ja |
| BESCHREIBUNG | Beschreibung der Funktion | Nein  |
| Parameter | Sammlung von [Bezeichnern](#identifiers), die den Parametern der Funktion entsprechen. | Ja |

### <a name="functions-example"></a>Funktionsbeispiel

``` json
"Functions": [
  {
    "Name": "Modify",
    "DisplayName": "Modify",
    "Description": "Modify the description/price attributes of this asset transfer instance",
    "Parameters": [
      {
        "Name": "description",
        "Description": "The new description of the asset",
        "DisplayName": "Description",
        "Type": {
          "Name": "string"
        }
      },
      {
        "Name": "price",
        "Description": "The new price of the asset",
        "DisplayName": "Price",
        "Type": {
          "Name": "money"
        }
      }
    ]
  },
  {
    "Name": "Terminate",
    "DisplayName": "Terminate",
    "Description": "Used to cancel this particular instance of asset transfer",
    "Parameters": []
  }
]

```

## <a name="states"></a>Zustände

Eine Sammlung von eindeutigen Zuständen innerhalb eines Workflows. Jeder Zustand erfasst einen Schritt in der Ablaufsteuerung der Geschäftslogik. 

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| NAME | Eindeutiger Name des Zustands. Der entsprechende intelligente Vertrag muss den gleichen **Namen** für den jeweiligen Zustand verwenden. | Ja |
| DisplayName | Der Anzeigename des Zustands | Ja |
| BESCHREIBUNG | Beschreibung des Zustands | Nein  |
| PercentComplete | Ein ganzzahliger Wert in der Blockchain Workbench-Benutzeroberfläche, der den Fortschritt der Ablaufsteuerung innerhalb der Geschäftslogik anzeigt. | Ja |
| Style | Visueller Hinweis, ob es sich um einen erfolgreichen oder fehlerhaften Zustand handelt. Es gibt zwei gültige Werte: `Success` oder `Failure`. | Ja |
| Transitions | Sammlung von verfügbaren [Übergängen](#transitions) von dem aktuellen Zustand in die nächsten Zustände. | Nein  |

### <a name="states-example"></a>Beispiel zu Zuständen (States)

``` json
"States": [
    {
      "Name": "Active",
      "DisplayName": "Active",
      "Description": "The initial state of the asset transfer workflow",
      "PercentComplete": 20,
      "Style": "Success",
      "Transitions": [
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Cancels this instance of asset transfer",
          "Function": "Terminate",
          "NextStates": [ "Terminated" ],
          "DisplayName": "Terminate Offer"
        },
        {
          "AllowedRoles": [ "Buyer" ],
          "AllowedInstanceRoles": [],
          "Description": "Make an offer for this asset",
          "Function": "MakeOffer",
          "NextStates": [ "OfferPlaced" ],
          "DisplayName": "Make Offer"
        },
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Modify attributes of this asset transfer instance",
          "Function": "Modify",
          "NextStates": [ "Active" ],
          "DisplayName": "Modify"
        }
      ]
    },
    {
      "Name": "Accepted",
      "DisplayName": "Accepted",
      "Description": "Asset transfer process is complete",
      "PercentComplete": 100,
      "Style": "Success",
      "Transitions": []
    },
    {
      "Name": "Terminated",
      "DisplayName": "Terminated",
      "Description": "Asset transfer has been cancelled",
      "PercentComplete": 100,
      "Style": "Failure",
      "Transitions": []
    }
  ]
```

## <a name="transitions"></a>Transitions

Verfügbare Aktionen zum Erreichen des nächsten Zustands. Eine oder mehrere Benutzerrollen können in jedem Zustand eine Aktion ausführen, wobei eine Aktion einen Zustand in einen anderen Zustand im Workflow überführen kann. 

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| AllowedRoles | Liste der Anwendungsrollen, die einen Übergang initiieren dürfen. Alle Benutzer der angegebenen Rolle können die Aktion möglicherweise ausführen. | Nein  |
| AllowedInstanceRoles | Liste der Benutzerrollen, die am intelligenten Vertrag teilnehmen oder darin festgelegt sind, um den Übergang zu initiieren. Instanzrollen werden in den **Eigenschaften** innerhalb von Workflows definiert. Diese Benutzer repräsentieren im Gegensatz zu allen Benutzern eines Rollentyps einen am intelligenten Vertrag teilnehmenden oder darin festgelegten Benutzer. | Nein  |
| DisplayName | Der Anzeigename des Übergangs | Ja |
| BESCHREIBUNG | Die Beschreibung des Übergangs | Nein  |
| Funktion | Der Name der Funktion zum Initiieren des Übergangs | Ja |
| NextStates | Eine Sammlung möglicher nächster Zustände nach einem erfolgreichen Übergang | Ja |

### <a name="transitions-example"></a>Beispiel zu Übergängen (Transitions)

``` json
"Transitions": [
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Cancels this instance of asset transfer",
    "Function": "Terminate",
    "NextStates": [ "Terminated" ],
    "DisplayName": "Terminate Offer"
  },
  {
    "AllowedRoles": [ "Buyer" ],
    "AllowedInstanceRoles": [],
    "Description": "Make an offer for this asset",
    "Function": "MakeOffer",
    "NextStates": [ "OfferPlaced" ],
    "DisplayName": "Make Offer"
  },
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Modify attributes of this asset transfer instance",
    "Function": "Modify",
    "NextStates": [ "Active" ],
    "DisplayName": "Modify"
  }
]

```

## <a name="application-roles"></a>Anwendungsrollen

Anwendungsrollen definieren eine Reihe von Rollen, die Benutzern zugewiesen werden können, die innerhalb der Anwendung agieren oder teilnehmen möchten. Anwendungsrollen können verwendet werden, um Aktionen und Teilnahme innerhalb der Blockchainanwendung und entsprechender Workflows einzuschränken. 

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| NAME | Der eindeutige Name der Anwendungsrolle. Der entsprechende intelligente Vertrag muss den gleichen **Namen** für die jeweilige Rolle verwenden. Basistypnamen sind reserviert. Einer Anwendungsrolle kann nicht der Name eines [Typs](#type) gegeben werden.| Ja |
| BESCHREIBUNG | Die Beschreibung der Anwendungsrolle | Nein  |

### <a name="application-roles-example"></a>Beispiel zu Anwendungsrollen (ApplicationRoles)

``` json
"ApplicationRoles": [
  {
    "Name": "Appraiser",
    "Description": "User that signs off on the asset price"
  },
  {
    "Name": "Buyer",
    "Description": "User that places an offer on an asset"
  }
]
```
## <a name="identifiers"></a>Bezeichner

Bezeichner stellen eine Sammlung von Informationen zur Beschreibung von Workfloweigenschaften, Konstruktor und Funktionsparametern dar. 

| Feld | BESCHREIBUNG | Erforderlich |
|-------|-------------|:--------:|
| NAME | Der eindeutige Name der Eigenschaft bzw. des Parameters. Der entsprechende intelligente Vertrag muss den gleichen **Namen** für die jeweilige Eigenschaft bzw. den jeweiligen Parameter verwenden. | Ja |
| DisplayName | Der Anzeigename der Eigenschaft bzw. des Parameters | Ja |
| BESCHREIBUNG | Die Beschreibung der Eigenschaft bzw. des Parameters | Nein  |

### <a name="identifiers-example"></a>Beispiel zu Bezeichnern

``` json
"Properties": [
  {
    "Name": "State",
    "DisplayName": "State",
    "Description": "Holds the state of the contract",
    "Type": {
      "Name": "state"
    }
  },
  {
    "Name": "Description",
    "DisplayName": "Description",
    "Description": "Describes the asset being sold",
    "Type": {
      "Name": "string"
    }
  }
]
```

## <a name="configuration-file-example"></a>Beispielkonfigurationsdatei

Das folgende Beispiel definiert eine allgemeine Anforderung/Antwort-Anwendung, in der ein Anforderer eine Anforderung sendet und ein Antwortdienst eine Antwort auf die Anforderung sendet.

``` json
{
  "ApplicationName": "HelloBlockchain",
  "DisplayName": "Hello, Blockchain!",
  "Description": "A simple application to send request and get response",
  "ApplicationRoles": [
    {
      "Name": "Requestor",
      "Description": "A person sending a request."
    },
    {
      "Name": "Responder",
      "Description": "A person responding to a request"
    }
  ],
  "Workflows": [
    {
      "Name": "RequestResponse",
      "DisplayName": "Request Response",
      "Description": "A simple workflow to send a request and receive a response.",
      "Initiators": [ "Requestor" ],
      "StartState": "Request",
      "Properties": [
        {
          "Name": "State",
          "DisplayName": "State",
          "Description": "Holds the state of the contract.",
          "Type": {
            "Name": "state"
          }
        },
        {
          "Name": "Requestor",
          "DisplayName": "Requestor",
          "Description": "A person sending a request.",
          "Type": {
            "Name": "Requestor"
          }
        },
        {
          "Name": "Responder",
          "DisplayName": "Responder",
          "Description": "A person sending a response.",
          "Type": {
            "Name": "Responder"
          }
        },
        {
          "Name": "RequestMessage",
          "DisplayName": "Request Message",
          "Description": "A request message.",
          "Type": {
            "Name": "string"
          }
        },
        {
          "Name": "ResponseMessage",
          "DisplayName": "Response Message",
          "Description": "A response message.",
          "Type": {
            "Name": "string"
          }
        }
      ],
      "Constructor": {
        "Parameters": [
          {
            "Name": "message",
            "Description": "...",
            "DisplayName": "Request Message",
            "Type": {
              "Name": "string"
            }
          }
        ]
      },
      "Functions": [
        {
          "Name": "SendRequest",
          "DisplayName": "Request",
          "Description": "...",
          "Parameters": [
            {
              "Name": "requestMessage",
              "Description": "...",
              "DisplayName": "Request Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        },
        {
          "Name": "SendResponse",
          "DisplayName": "Response",
          "Description": "...",
          "Parameters": [
            {
              "Name": "responseMessage",
              "Description": "...",
              "DisplayName": "Response Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        }
      ],
      "States": [
        {
          "Name": "Request",
          "DisplayName": "Request",
          "Description": "...",
          "PercentComplete": 50,
          "Value": 0,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": ["Responder"],
              "AllowedInstanceRoles": [],
              "Description": "...",
              "Function": "SendResponse",
              "NextStates": [ "Respond" ],
              "DisplayName": "Send Response"
            }
          ]
        },
        {
          "Name": "Respond",
          "DisplayName": "Respond",
          "Description": "...",
          "PercentComplete": 90,
          "Value": 1,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": ["Requestor"],
              "Description": "...",
              "Function": "SendRequest",
              "NextStates": [ "Request" ],
              "DisplayName": "Send Request"
            }
          ]
        }
      ]
    }
  ]
}
```
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bereitstellen von Azure Blockchain Workbench](blockchain-workbench-deploy.md)

