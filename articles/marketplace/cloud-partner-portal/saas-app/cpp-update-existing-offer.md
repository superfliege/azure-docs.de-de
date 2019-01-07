---
title: Aktualisieren eines vorhandenen Azure-SaaS-Anwendungsangebots | Microsoft-Dokumentation
description: Aktualisieren eines vorhandenen SaaS-Anwendungsangebots im Azure Marketplace.
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
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: 9d490a0ec09e351e4cee00e7c30c9161bdfac3e6
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196172"
---
# <a name="update-an-existing-saas-application-offer"></a>Aktualisieren eines vorhandenen SaaS-Anwendungsangebots

Es gibt verschiedene Arten von Aktualisierungen, die Sie möglicherweise an Ihrem Angebot vornehmen möchten, nachdem es veröffentlicht und freigeschaltet wurde. Jede Änderung, die Sie an der neuen Version Ihres Angebots vorgenommen haben, sollte gespeichert und veröffentlicht werden, damit sie im Marketplace wirksam wird. In diesem Artikel werden die verschiedenen Aspekte erläutert, die es für ein Aktualisieren Ihres SaaS-Angebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) gibt.

Sie könnten Ihr Angebot beispielsweise aus den folgenden Gründen aktualisieren:

- Hinzufügen einer neuen Version zu einer vorhandenen App
- Aktualisieren einer App
- Hinzufügen von neuen Funktionen zu einer App
- Aktualisieren der Marketplace-Metadaten für das Angebot

Um Ihnen bei diesen Änderungen zu helfen, bietet das Portal die Features **Vergleichen** und **Verlauf**.

## <a name="unpermitted-changes-to-a-saas-offer"></a>Unzulässige Änderungen für ein SaaS-Angebot

Es gibt Attribute eines SaaS-Angebots, die nicht geändert werden können, nachdem das Angebot im Azure Marketplace live geschaltet wurde. Sie können die folgenden Einstellungen nicht ändern:

- Angebots-ID und Herausgeber-ID des Angebots
- Versionstags, z.B.: `1.0.1`
- Abrechnungs-/Lizenzmodelländerungen an vorhandenen Angeboten

## <a name="common-update-operations"></a>Gängige Aktualisierungsvorgänge
 
Die folgenden Aktualisierungsvorgänge werden häufig durchgeführt.

### <a name="update-offer-contacts"></a>Aktualisieren von Angebotskontakten

Gehen Sie wie folgt vor, um die Supportkontakte Ihres Angebots zu aktualisieren.

1. Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an.
2. Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.
3. Wechseln Sie zur Registerkarte **Kontakte**. Aktualisieren Sie Ihre Kontakte.
4. Wählen Sie **Veröffentlichen** aus, um den Workflow zum Veröffentlichen Ihrer Änderungen zu starten.


### <a name="update-offer-marketplace-metadata"></a>Aktualisieren der Marketplace-Metadaten eines Angebots

Führen Sie die folgenden Schritte aus, um die mit Ihrem Angebot verknüpften Marketplace-Metadaten zu aktualisieren. (Beispiel: Firmenname, Logos usw.)

1. Melden Sie sich beim [Cloud-Partnerportal](https://cloudpartner.azure.com/) an.
2. Suchen Sie unter **Alle Angebote** nach dem Angebot, das aktualisiert werden soll.
3. Wechseln Sie zur Registerkarte **Details der digitalen Ladenzeile**. Gehen Sie anhand der Anweisungen im Artikel [Veröffentlichen eines SaaS-Anwendungsangebots](./cpp-publish-offer.md) vor, um Änderungen an den Metadaten vorzunehmen.
4. Wählen Sie **Veröffentlichen** aus, um den Workflow zum Veröffentlichen Ihrer Änderungen zu starten.

## <a name="compare-feature"></a>Vergleichsfunktion

Wenn Sie Änderungen an einem veröffentlichten Angebot vornehmen, können Sie die Funktion „Vergleichen“ verwenden, um die vorgenommenen Änderungen zu prüfen. In der nächsten Bildschirmaufnahme ist die „Vergleichen“-Option für ein veröffentlichtes Angebot gezeigt.

![„Vergleichen“ verwenden, um Angebotsänderungen im Cloud-Partnerportal anzuzeigen](./media/saas-offer-compare.png)

### <a name="to-use-the-compare-feature"></a>So verwenden Sie die Funktion „Vergleichen“

1. Wählen Sie zu einem beliebigen Zeitpunkt im Bearbeitungsprozess „Vergleichen“ für Ihr Angebot aus.
2. Sehen Sie sich die Versionen der Marketingressourcen und Metadaten nebeneinander an.

## <a name="publishing-history"></a>Veröffentlichungsverlauf

Um bereits ausgeführte Veröffentlichungsaktivitäten anzuzeigen, wählen Sie im Cloud-Partnerportal auf der linken Navigationsmenüleiste die Registerkarte **Verlauf** aus. Sie sehen mit Zeitstempeln versehene Aktionen, die während der Lebensdauer Ihrer Azure Marketplace-Angebote ausgeführt wurden.

![Angebotsverlauf im Cloud-Partnerportal anzeigen](./media/saas-offer-history.png)

Sie können auf der Seite „Überwachungsverlauf“ nach einem bestimmten Angebot suchen sowie Filter anwenden, etwa Herausgeber, Angebot und Ereignistyp (z. B. „OfferGoLiveRequested“.) Sie können den Angebotsverlauf auch als CSV-Datei herunterladen.


## <a name="next-steps"></a>Nächste Schritte

[SaaS-Anwendungsangebot](./cpp-saas-offer.md)