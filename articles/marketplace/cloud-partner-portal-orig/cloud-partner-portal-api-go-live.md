---
title: Live schalten | Microsoft-Dokumentation
description: Der Live schalten-API initiiert den Prozess, in dem das Angebot zur Auflistung freigeschaltet (live geschaltet) wird.
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
ms.openlocfilehash: ea3f26d70c4a4ce07c988612890687504a4cf5ac
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594677"
---
<a name="go-live"></a>Live schalten
=======

Diese API startet den Prozess zum Überführen einer App in den Produktionsstatus. Dieser Vorgang dauert üblicherweise ziemlich lange. In diesem Aufruf wird die E-Mail-Benachrichtigungsliste (notification-emails) aus dem Vorgang in der [Veröffentlichen](./cloud-partner-portal-api-publish-offer.md)-API verwendet.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>URI-Parameter
--------------

|  **Name**      |   **Beschreibung**                                                           | **Datentyp** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Herausgeber-ID für das abzurufende Angebot, z. B. `contoso`       |  Zeichenfolge       |
| offerId        | Angebots-ID des abzurufenden Angebots                                   |  Zeichenfolge       |
| api-version    | Aktuelle Version der API                                                   |  Datum         |
|  |  |  |


<a name="header"></a>Header
------

|  **Name**       |     **Wert**       |
|  ---------      |     ----------      |
| Content-Typ    | `application/json`  |
| Autorisierung   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Beispiel für Hauptteil
------------

### <a name="response"></a>response

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Antwortheader

|  **Name**             |      **Wert**                                                            |
|  --------             |      ----------                                                           |
| Operation-Location    |  URL, die abgefragt wird, um den aktuellen Status des Vorgangs zu ermitteln            |
|  |  |


### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code** |  **Beschreibung**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`: Die Anforderung wurde erfolgreich angenommen. Die Antwort enthält eine Position, um den Vorgangsstatus nachzuverfolgen. |
|  400     | `Bad/Malformed request`: Zusätzliche Fehlerinformationen sind im Antworttext zu finden. |
|  404     |  `Not found` – die angegebene Entität ist nicht vorhanden.                                       |
|  |  |
