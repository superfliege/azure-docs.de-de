---
title: SaaS Fulfillment APIs – Azure Marketplace | Microsoft-Dokumentation
description: Vorgestellt werden die Versionen der Fulfillment-APIs, mit denen Sie Ihre SaaS-Angebote in den Azure Marketplace integrieren können.
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
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: pbutlerm
ms.openlocfilehash: 9c3fbe7cd7ebd41f59be360f40d66b8d38dbce5e
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2019
ms.locfileid: "57319275"
---
# <a name="saas-fulfillment-apis"></a>SaaS-Fulfillment-APIs

Mit der SaaS-Fulfillment-API können unabhängige Softwarehersteller (ISVs) ihre SaaS-Anwendungen in den Azure Marketplace integrieren. Mit dieser API können ISVs für ihre Anwendungen alle Handelskanäle nutzen: direkt, über Partner (Reseller) und über den Vertrieb.  Diese API ist eine Voraussetzung für die Auflistung transaktionsfähiger SaaS-Angebote im Azure Marketplace.

Die aktuelle Version dieser API ist Version 2, die für alle neuen SaaS-Angebote verwendet werden sollte.  Version 1 der API ist veraltet und wird zur Unterstützung vorhandener Angebote noch gepflegt.


## <a name="business-model-support"></a>Geschäftsmodellunterstützung

Diese API unterstützt die folgenden Geschäftsmodellfunktionen für folgende Aufgaben:

* Definieren mehrerer Pläne für ein Angebot. Diese Pläne haben unterschiedliche Funktionen und werden möglicherweise unterschiedlich berechnet.
* Bereitstellen eines Angebots auf Website- oder Benutzer-Abrechnungsbasis.
* Bereitstellen monatlicher und jährlicher (Vorauszahlung) Abrechnungsoptionen.
* Bereitstellen eines privaten Preismodells für einen Kunden auf der Grundlage einer ausgehandelten Geschäftsvereinbarung.


## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie für die Entwicklung die aktuelle Version dieser Schnittstelle: [SaaS Fulfillment API Version 2](./cpp-saas-fulfillment-api-v2.md).
