---
title: Aktualisieren eines bestehenden Angebots für Azure Marketplace
description: Aktualisieren eines bestehenden Angebots für Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5633392bdf1293ee9196fafe67cf901e0d8c8014
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806188"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Aktualisieren eines bestehenden Angebots für Azure Marketplace 
==============================================

Es gibt verschiedene Arten von Updates, die Sie nach der Live-Schaltung Ihres Angebots vornehmen sollten.

1.  Hinzufügen neuer \"Pakete\" zu einer vorhandenen SKU
2.  Hinzufügen neuer SKUs zu einem vorhandenen Angebot
3.  Aktualisieren von Angebots-/SKU-Marketplace-Metadaten

Sie müssen Ihr Angebot im Cloud-Partnerportal aktualisieren und erneut veröffentlichen. In diesem Artikel werden die verschiedenen Aspekte zur Aktualisierung Ihres Azure-Anwendungsangebots beschrieben.

<a name="unpermitted-changes-to-azure-application-offersku"></a>Unzulässige Änderungen an Azure-Anwendungsangeboten/-SKUs 
--------------------------------------------------

Einige Attribute von Azure-Anwendungsangeboten/-SKUs können nicht mehr geändert werden, nachdem das Angebot im Azure Marketplace live geschaltet wurde.

1.  Angebots-ID und Herausgeber-ID des Angebots
2.  SKU-ID von vorhandenen SKUs
3.  Aktualisieren eines Pakets, das veröffentlicht wurde

<a name="adding-a-new-package-to-an-existing-sku"></a>Hinzufügen neuer Pakete zu einer vorhandenen SKU 
---------------------------------------

Der Herausgeber möchte eventuell eine neue Version des Pakets hinzufügen, um ein vorhandenes Paket zu aktualisieren. Dies kann durch Hochladen eines neuen Pakets mit einer anderen Versionsnummer erfolgen.

1.  Melden Sie sich beim [Cloud-Partnerportal](http://cloudpartner.azure.com) an.
2.  Suchen Sie unter „Alle Angebote“ nach dem Angebot, das aktualisiert werden soll.
3.  Klicken Sie im Formular „SKUs“ auf die SKU, deren Paket aktualisiert werden soll.
4.  Klicken Sie auf \"Neues Paket\", und geben Sie eine neue Version an.
5.  Laden Sie eine neue ZIP-Datei mit der aktualisierten Vorlagendatei hoch.
6.  Klicken Sie auf „Veröffentlichen“, um den Veröffentlichungsworkflow zu initiieren und Ihre neue SKU live zu schalten.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Hinzufügen neuer SKUs zu einem vorhandenen Angebot
-------------------------------------

Sie können eine neue SKU für Ihr vorhandenes Angebot zur Verfügung stellen. Führen Sie hierzu die folgenden Schritte aus:

1.  Melden Sie sich beim [Cloud Partner-Portal](http://cloudpartner.azure.com) an.
2.  Suchen Sie unter „Alle Angebote“ nach dem Angebot, das aktualisiert werden soll.
3.  Klicken Sie im Formular „SKUs“ auf „Neue SKU hinzufügen“ und geben Sie im Popupfenster eine SKU-ID an.
4.  Führen Sie die verbleibenden [hier](./cloud-partner-portal-managed-app-publish.md) angegebenen Schritte aus.
5.  Klicken Sie auf „Veröffentlichen“, um den Veröffentlichungsworkflow zu initiieren und Ihre neue SKU live zu schalten.

<a name="updating-offer-marketplace-metadata"></a>Aktualisieren von Marketplace-Metadaten eines Angebots 
-----------------------------------

In bestimmten Szenarien müssen Sie möglicherweise die mit Ihrem Angebot verknüpften Marketplace-Metadaten aktualisieren, wie etwa bei der Aktualisierung Ihrer Unternehmenslogos. Führen Sie die folgenden Schritte aus.

1.  Anmelden am Cloud-Partnerportal
2.  Suchen Sie unter „Alle Angebote“ nach dem Angebot, das aktualisiert werden soll.
3.  Navigieren Sie zum Formular „Marketplace“, und befolgen Sie [diese Anweisungen](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-push-to-staging), um Änderungen vorzunehmen.
4.  Klicken Sie auf „Veröffentlichen“, um den Veröffentlichungsworkflow zu initiieren und Ihre Änderungen live zu schalten.
