---
title: Aktualisieren eines bestehenden Power BI-App-Angebots – Azure Marketplace | Microsoft-Dokumentation
description: Sie können ein Power BI-App-Angebot nach seiner Veröffentlichung im Marketplace von Microsoft AppSource aktualisieren.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
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
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 635e2e71bb952aaee761df6a1d5d87c46db531f6
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665701"
---
# <a name="update-an-existing-power-bi-app-offer"></a>Aktualisieren eines bestehenden Power BI App-Angebots

In diesem Artikel werden die verschiedenen Aspekte beim Aktualisieren Ihres Power BI App-Angebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) und beim anschließenden erneuten Veröffentlichen des Angebots erläutert.  Es kann eine Reihe von Gründen geben, warum Sie Ihr Angebot aktualisieren möchten, beispielweise folgende:

- Aktualisierung des Inhalts der App in Power BI und Abrufen einer neuen Installations-URL aus der neu gepackten App
- Aktualisieren von Marketplace-Metadaten für das Angebot in Bezug auf Vertrieb, Marketing, Supportinformationen und Ressourcen
 
Um Ihnen bei diesen Änderungen zu helfen, bietet das Portal die Features **Vergleichen** und **Verlauf**.


## <a name="unpermitted-changes-to-offer"></a>Unzulässige Änderungen am Angebot

Einige Attribute eines Power BI-App-Angebots können nicht geändert werden, sobald das Angebot im AppSource live geschaltet ist. Dies sind insbesondere **Angebots-ID** und **Herausgeber-ID**.


## <a name="common-update-operations"></a>Gängige Aktualisierungsvorgänge

Es gibt zwar eine Vielzahl von Merkmalen, die Sie in einem Power BI-App-Angebot ändern können, aber die folgenden Vorgänge kommen häufig vor.


### <a name="update-app-content-in-power-bi"></a>Aktualisieren von App-Inhalten in Power BI

Es ist üblich, dass die App in Power BI regelmäßig mit neuen Inhalten, Sicherheitspatches, zusätzlichen Features usw. aktualisiert wird. In solchen Szenarien müssen Sie die URL zur Installation der neuen Inhalte der App aktualisieren, indem Sie die folgenden Schritte ausführen:

1.  Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das Sie aktualisieren möchten.
3.  Geben Sie auf der Registerkarte **Technische Informationen** eine neue Installations-URL ein.
4.  Klicken Sie auf **Veröffentlichen**, um den Workflow zum Veröffentlichen Ihrer neuen App-Version in AppSource zu starten.


### <a name="update-offer-marketplace-metadata"></a>Aktualisieren der Marketplace-Metadaten eines Angebots

Führen Sie die folgenden Schritte aus, um die mit Ihrem Angebot verknüpften Marketplace-Metadaten – Firmenname, Logos usw. – zu aktualisieren:

1.  Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an.
2.  Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.
3.  Wechseln Sie zur Registerkarte **Details der digitalen Ladenzeile**, und befolgen Sie dann die Anweisungen im Artikel zur [Registerkarte „Details der digitalen Ladenzeile“ für Power BI-Apps](./cpp-storefront-details-tab.md), um die Metadatenänderungen vorzunehmen.
4.  Klicken Sie auf **Veröffentlichen**, um den Workflow zum Veröffentlichen Ihrer Änderungen zu starten.


## <a name="compare-feature"></a>Vergleichsfunktion

Wenn Sie Änderungen an einem bereits veröffentlichten Angebot vorgenommen haben, können Sie die **Vergleichen**-Funktion verwenden, um die vorgenommenen Änderungen zu prüfen. So verwenden Sie dieses Feature:

1.  Sie können zu jedem Zeitpunkt des Bearbeitungsprozesses auf die Schaltfläche **Vergleichen** für Ihr Angebot klicken.

    ![Schaltfläche des Features „Vergleichen“](./media/compare-feature-button.png)

2.  Zeigen Sie nebeneinander Versionen von Marketingressourcen und Metadaten an.


## <a name="history-of-publishing-actions"></a>Verlauf der Veröffentlichungsaktionen

Um bereits ausgeführte Veröffentlichungsaktivitäten anzuzeigen, klicken Sie im Cloud-Partnerportal in der linken Navigationsmenüleiste auf die Registerkarte **Verlauf**. Auf dieser Registerkarte werden mit Zeitstempeln versehene Aktionen angezeigt, die während der Gültigkeitsdauer Ihrer AppSource-Angebote erfolgt sind.


## <a name="next-steps"></a>Nächste Schritte

Sie sollten regelmäßig die Funktion [Seller Insights](../../cloud-partner-portal-orig/si-getting-started.md) im [Cloud-Partnerportal](https://cloudpartner.azure.com/#insights) nutzen, um Einblicke in Ihre Marketplace-Kunden und die Verwendung der App zu erhalten.  
