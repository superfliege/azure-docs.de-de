---
title: Löschen eines Angebots oder einer SKU im Azure Marketplace | Microsoft-Dokumentation
description: Schritte zum Löschen eines Angebots oder einer SKU.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: cc172e35e8964fad3b1a1410d1f1f3240c423ab3
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48806204"
---
<a name="delete-an-offer-or-sku-from-azure-marketplace"></a>Löschen eines Angebots oder einer SKU im Azure Marketplace
==========================================

Möglicherweise möchten Sie aus unterschiedlichen Gründen Ihr Angebot aus dem Marketplace entfernen. Durch das Entfernen eines Angebots wird sichergestellt, dass Ihr Angebot nicht mehr durch neue Kunden erworben oder bereitgestellt werden kann. Dies wirkt sich jedoch nicht auf bestehende Kunden aus.
Durch Beenden des Angebots wird der Dienst und/oder der Lizenzvertrag zwischen Ihnen und Ihren bestehenden Kunden beendet. Die Richtlinien für die Entfernung und Kündigung von Angeboten werden durch den [Microsoft Marketplace-Herausgebervertrag](http://go.microsoft.com/fwlink/?LinkID=699560) (siehe Abschnitt
7) und die [Teilnahmerichtlinien](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (siehe Abschnitt 6.2) geregelt. In diesem Artikel werden die unterschiedlichen unterstützten Szenarien zum Löschen und die dazu ausgeführten Schritte behandelt.

<a name="delete-a-live-sku-from-azure-marketplace"></a>Löschen einer Live-SKU im Azure Marketplace
----------------------------------------

Mit den folgenden Schritten können Sie eine Live-SKU im Azure Marketplace löschen:

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.

2.  Wählen Sie auf der Registerkarte **Alle Angebote** Ihr Angebot aus.

3.  Wählen Sie im Bereich auf der linken Bildschirmseite die Registerkarte **SKUs** aus.

4.  Wählen Sie die SKU aus, die Sie löschen möchten, und klicken Sie auf die Schaltfläche „Löschen“ für diese SKU.

5.  [Veröffentlichen Sie das Angebot erneut](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) im Azure Marketplace.

Nachdem das Angebot im Azure Marketplace live geschaltet wurde, wird die SKU im Azure Marketplace und Azure-Portal gelöscht.

<a name="roll-back-to-a-previous-sku-version"></a>Ausführen eines Rollbacks zu einer vorherigen SKU-Version
----------------------------------

Sie können die aktuelle Version einer Live-SKU mit den folgenden Schritten im Azure Marketplace löschen. Nachdem der Vorgang abgeschlossen ist, wird die SKU auf die vorherige Version zurückgesetzt.

1.  Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.

2.  Wählen Sie auf der Registerkarte **Alle Angebote** Ihr Angebot aus.

3.  Wählen Sie im Bereich auf der linken Bildschirmseite die Registerkarte **SKUs** aus.

4.  Löschen Sie die neueste **Datenträgerversion** in der Liste der veröffentlichten Datenträgerversionen.

5.  [Veröffentlichen Sie das Angebot erneut](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) im Azure Marketplace.

Nachdem das Angebot im Azure Marketplace live geschaltet wurde, wird die aktuelle Version der aufgelisteten SKU im Azure Marketplace und Azure-Portal gelöscht.
Die SKU wird auf die vorherige Version zurückgesetzt.

<a name="delete-a-live-offer"></a>Löschen eines Liveangebots
-------------------

Bei einer Anforderung zum Entfernen eines Liveangebots müssen verschiedene Aspekte berücksichtigt werden. Führen Sie die unten angegebenen Schritte aus, um vom Supportteam Hilfe beim Entfernen eines Liveangebots aus dem Azure Marketplace zu erhalten:

1.  Erstellen Sie über diesen [Link](https://go.microsoft.com/fwlink/?linkid=844975) oder durch Klicken auf „Support“ in der rechten oberen Ecke des Cloud-Partnerportals ein Supportticket.

2.  Wählen Sie den spezifischen Angebotstyp in der Liste **Problemtyp** und dann **Remove a published offer** (Entfernen eines veröffentlichten Angebots) in der Liste **Kategorie** aus.

3.  Senden Sie die Anforderung.

Das Supportteam führt Sie durch den Vorgang zum Löschen des Angebots.

>[!NOTE]
>Das Löschen einer SKU oder eines Angebots wirkt sich nicht auf aktuelle Käufe der SKU bzw. des Angebots aus. Diese SKUs und Angebote sind weiterhin funktionstüchtig. Sie können jedoch zukünftig nicht mehr erworben werden.
