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
ms.date: 03/26/2019
ms.author: pbutlerm
ms.openlocfilehash: c7da46984d592abc6ed97d7490fde732bf26b0ba
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798730"
---
# <a name="saas-fulfillment-apis"></a>SaaS-Fulfillment-APIs

Mit den SaaS-Fulfillment-APIs können unabhängige Softwarehersteller (ISVs) ihre SaaS-Anwendungen in den Azure Marketplace integrieren. Mit diesen APIs können ISVs für ihre Anwendungen alle Handelskanäle nutzen: direkt, über Partner (Reseller) und über den Vertrieb.  Sie sind eine Voraussetzung für die Auflistung transaktionsfähiger SaaS-Angebote im Azure Marketplace.

> [!WARNING]
> Die aktuelle Version dieser API ist Version 2, die für alle neuen SaaS-Angebote verwendet werden sollte.  Version 1 der API ist veraltet und wird zur Unterstützung vorhandener Angebote noch gepflegt.


## <a name="business-model-support"></a>Geschäftsmodellunterstützung

Diese API unterstützt die folgenden Geschäftsmodellfunktionen für folgende Aufgaben:

* Definieren mehrerer Pläne für ein Angebot. Diese Pläne haben unterschiedliche Funktionen und werden möglicherweise unterschiedlich berechnet.
* Bereitstellen eines Angebots auf Website- oder Benutzer-Abrechnungsbasis.
* Bereitstellen monatlicher und jährlicher (Vorauszahlung) Abrechnungsoptionen.
* Bereitstellen eines privaten Preismodells für einen Kunden auf der Grundlage einer ausgehandelten Geschäftsvereinbarung.


## <a name="next-steps"></a>Nächste Schritte

Wenn nicht bereits geschehen, registrieren Sie Ihre SaaS-Anwendung im [Azure-Portal](https://ms.portal.azure.com) wie unter [Registrieren einer Azure AD-Anwendung](./cpp-saas-registration.md) erklärt.  Verwenden Sie anschließend für die Entwicklung die aktuelle Version dieser Schnittstelle: [SaaS Fulfillment API Version 2](./cpp-saas-fulfillment-api-v2.md).
