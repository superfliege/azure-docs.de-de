---
title: Veröffentlichen eines Angebots | Microsoft-Dokumentation
description: API zum Veröffentlichen des angegebenen Angebots.
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
ms.openlocfilehash: cb1293a771a137f4df7e36a2b412f68b384f16ef
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806728"
---
<a name="publish-an-offer"></a>Veröffentlichen eines Angebots
================

Startet den Veröffentlichungsprozess für das angegebene Angebot. Bei diesem Aufruf handelt es sich um einen Vorgang mit langer Ausführungsdauer.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

<a name="uri-parameters"></a>URI-Parameter
--------------

|  **Name**      |    **Beschreibung**                               |  **Datentyp** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Herausgeber-ID, z.B. `contoso`      |   Zeichenfolge       |
|  offerId       | Angebots-ID                                 |   Zeichenfolge       |
|  api-version   | Aktuelle Version der API                        |   Datum         |
|  |  |


<a name="header"></a>Header
------

|  **Name**        |    **Wert**          |
|  --------        |    ---------          |
|  Content-Typ    | `application/json`    |
|  Autorisierung   |  `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Beispiel für den Text
------------

### <a name="request"></a>Anforderung

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>Eigenschaften des Anforderungstexts

|  **Name**               |   **Beschreibung**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  notification-emails    | Kommagetrennte Liste der E-Mail-Adressen, die über den Fortschritt des Veröffentlichungsvorgangs benachrichtigt werden sollen. |
|  |  |


### <a name="response"></a>response

   `Operation-Location: /api/operations/contoso$56615b67-2185-49fe-80d2-c4ddf77bb2e8$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>Antwortheader

|  **Name**             |    **Wert**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Operation-Location    | URL, die abgefragt werden kann, um den aktuellen Status des Vorgangs zu ermitteln.    |
|  |  |


### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code** |  **Beschreibung**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted`: Die Anforderung wurde erfolgreich angenommen. Die Antwort enthält einen Speicherort, der zum Nachverfolgen des gestarteten Vorgangs verwendet werden kann. |
| 400   | `Bad/Malformed request`: Der Fehlerantworttext stellt möglicherweise weitere Informationen bereit.                                                               |
| 422   | `Un-processable entity`: Gibt an, dass die Validierung der zu veröffentlichenden Entität fehlgeschlagen ist.                                                        |
| 404   | `Not found`: Die vom Client angegebene Entität ist nicht vorhanden.                                                                              |
|  |  |
