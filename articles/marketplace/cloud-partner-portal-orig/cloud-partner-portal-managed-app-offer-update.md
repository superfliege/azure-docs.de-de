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
ms.openlocfilehash: 2bfa10441cf5531c9383527a21b033da26322b34
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073237"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Aktualisieren eines bestehenden Angebots für Azure Marketplace 
==============================================

Es gibt verschiedene Arten von Aktualisierungen, die Sie nach der Liveschaltung Ihres Angebots vornehmen können, beispielsweise:

1.  Hinzufügen neuer \"Pakete\" zu einer vorhandenen SKU
2.  Hinzufügen neuer SKUs zu einem vorhandenen Angebot
3.  Aktualisieren von Angebots-/SKU-Marketplace-Metadaten

Sie verwenden das Cloud-Partnerportal, um diese Änderungen zu implementieren und Ihr Angebot dann erneut zu veröffentlichen. In diesem Artikel werden die verschiedenen Aspekte der Aktualisierung Ihres Azure-Anwendungsangebots beschrieben.

<a name="unpermitted-changes-to-azure-application-offer"></a>Unzulässige Änderungen an Azure-Anwendungsangeboten 
-----------------------------------------------

Einige Attribute von Azure-Anwendungsangeboten/-SKUs können nicht mehr geändert werden, nachdem das Angebot im Azure Marketplace live geschaltet wurde.

* Angebots-ID und Herausgeber-ID des Angebots
* SKU-ID von vorhandenen SKUs
* Aktualisieren eines Pakets, das veröffentlicht wurde

<a name="adding-a-new-package-to-an-existing-sku"></a>Hinzufügen neuer Pakete zu einer vorhandenen SKU 
---------------------------------------

Der Herausgeber möchte eventuell eine neue Version des Pakets hinzufügen, um ein vorhandenes Paket zu aktualisieren. Zum Hinzufügen einer Version kann ein neues Paket mit einer anderen Versionsnummer hochgeladen werden.

1.  Melden Sie sich beim [Cloud-Partnerportal](http://cloudpartner.azure.com) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das Sie aktualisieren möchten.
3.  Klicken Sie im Formular **SKUs** auf die SKU, deren Paket Sie aktualisieren möchten.
4.  Klicken Sie auf \"Neues Paket\", und geben Sie eine neue Version an.
5.  Laden Sie eine neue ZIP-Datei mit der aktualisierten Vorlagendatei hoch.
6.  Klicken Sie auf „Veröffentlichen“, um den Veröffentlichungsworkflow zu initiieren und Ihre neue SKU live zu schalten.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Hinzufügen neuer SKUs zu einem vorhandenen Angebot
-------------------------------------

Sie können wie folgt eine neue SKU für Ihr vorhandenes Angebot zur Verfügung stellen:

1.  Melden Sie sich beim [Cloud-Partnerportal](http://cloudpartner.azure.com) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das Sie aktualisieren möchten.
3.  Klicken Sie im Formular **SKUs** auf „Neue SKU hinzufügen“, und geben Sie im Popupfenster eine SKU-ID an.
4.  Führen Sie die verbleibenden [hier](./cloud-partner-portal-managed-app-publish.md) angegebenen Schritte aus.
5.  Klicken Sie auf **Veröffentlichen**, um den Veröffentlichungsworkflow zu starten und Ihre neue SKU live zu schalten.

<a name="updating-offer-marketplace-metadata"></a>Aktualisieren von Marketplace-Metadaten eines Angebots 
-----------------------------------

In bestimmten Szenarien müssen Sie möglicherweise die mit Ihrem Angebot verknüpften Marketplace-Metadaten aktualisieren, wie etwa bei der Aktualisierung Ihrer Unternehmenslogos. Führen Sie die folgenden Schritte aus.

1.  Melden Sie sich beim Cloud-Partnerportal an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das Sie aktualisieren möchten.
3.  Navigieren Sie zum Formular „Marketplace“, und befolgen Sie [diese Anweisungen](../cloud-partner-portal/azure-applications/cpp-marketplace-tab.md), um Änderungen vorzunehmen.
4.  Klicken Sie auf „Veröffentlichen“, um den Veröffentlichungsworkflow zu initiieren und Ihre Änderungen live zu schalten.
