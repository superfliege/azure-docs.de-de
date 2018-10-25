---
title: API für das Abrufen von Angeboten | Microsoft-Dokumentation
description: Die API ruft eine zusammengefasste Liste der Angebote unter einem Herausgebernamespace ab.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 3429917fcee520ae932253f1fdfead4ffb6535e6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806019"
---
<a name="retrieve-offers"></a>Abrufen von Angeboten
===============

Ruft eine zusammengefasste Liste der Angebote unter einem Herausgebernamespace ab.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`


<a name="uri-parameters"></a>URI-Parameter
--------------

| ** Name**        |  **Beschreibung**                         |  **Datentyp** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | Herausgeber-ID, z.B. `contoso` |   Zeichenfolge    |
|  api-version     | Aktuelle Version der API                    |    Datum        |
|  |  |


<a name="header"></a>Header
------

|  **Name**        |         **Wert**       |
|  --------------- |       ----------------  |
|  Content-Typ    | `application/json`      |
|  Autorisierung   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>Beispiel für den Text
------------

### <a name="response"></a>response

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Eigenschaften des Antworttexts

|  **Name**       |       **Beschreibung**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | Gibt den Typ des Angebots an                                                                                           |
|  publisherId    | Bezeichner, der den Herausgeber eindeutig identifiziert                                                                      |
|  status         | Status des Angebots. Die Liste der möglichen Werte finden Sie weiter unten unter [Angebotsstatus](#offer-status).                         |
|  id             | GUID, die das Angebot im Herausgebernamespace eindeutig identifiziert.                                                    |
|  Version        | Aktuelle Version des Angebots. Die version-Eigenschaft kann vom Client nicht geändert werden. Sie wird nach jeder Veröffentlichung inkrementiert. |
|  Definition     | Enthält eine zusammengefasste Ansicht der tatsächlichen Definition der Workload. Für eine ausführliche Definition verwenden Sie die API zum [Abrufen eines bestimmten Angebots] (./cloud-partner-portal-api-retrieve-specific-offer.md). |
|  changedTime    | Uhrzeit (UTC), zu der das Angebot letztmalig geändert wurde                                                                              |
|  |  |


### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code**  |  **Beschreibung**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK`: Die Anforderung wurde erfolgreich verarbeitet, und alle Angebote unter dem Herausgeber wurden an den Client zurückgegeben.  |
|  400      | `Bad/Malformed request`: Der Fehlerantworttext enthält möglicherweise weitere Informationen.                                    |
|  403      | `Forbidden`: Der Client hat keinen Zugriff auf den angegebenen Namespace.                                          |
|  404      | `Not found`: Die angegebene Entität ist nicht vorhanden.                                                                 |
|  |  |


### <a name="offer-status"></a>Angebotsstatus

|  **Name**                    | **Beschreibung**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | Das Angebot wurde nie veröffentlicht.                  |
|  NotStarted                  | Das Angebot ist neu, aber nicht gestartet.                 |
|  WaitingForPublisherReview   | Das Angebot wartet auf die Herausgebergenehmigung.         |
|  Wird ausgeführt                     | Die Angebotsübermittlung wird verarbeitet.             |
|  Succeeded                   | Die Verarbeitung der Angebotsübermittlung ist abgeschlossen.       |
|  Canceled                    | Die Angebotsübermittlung wurde abgebrochen.                   |
|  Fehler                      | Fehler bei der Angebotsübermittlung.                         |
|  |  |
