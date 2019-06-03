---
title: Aktualisieren eines vorhandenen Azure-SaaS-Anwendungsangebots | Azure Marketplace
description: Aktualisieren eines vorhandenen SaaS-Anwendungsangebots im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: pbutlerm
ms.openlocfilehash: 2195c9a5e1f0d3683ea8cf6564d97cbabd072f81
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834197"
---
# <a name="update-an-existing-saas-application-offer"></a>Aktualisieren eines vorhandenen SaaS-Anwendungsangebots

Es gibt verschiedene Arten von Aktualisierungen, die Sie möglicherweise an Ihrem Angebot vornehmen möchten, nachdem es veröffentlicht und freigeschaltet wurde. Jede Änderung, die Sie an der neuen Version Ihres Angebots vorgenommen haben, sollte gespeichert und veröffentlicht werden, damit sie im Marketplace wirksam wird. In diesem Artikel werden die verschiedenen Aspekte erläutert, die es für ein Aktualisieren Ihres SaaS-Angebots im [Cloud-Partnerportal](https://cloudpartner.azure.com/) gibt.

> [!IMPORTANT] 
> Die SaaS-Angebotsfunktionalität wird zum [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory) migriert.  Alle neuen Herausgeber müssen das Partner Center für die Erstellung neuer SaaS-Angebote und die Verwaltung bestehender Angebote verwenden.  Aktuelle Herausgeber mit SaaS-Angeboten werden schrittweise vom Cloud-Partnerportal zum Partner Center migriert.  Das Cloud-Partnerportal zeigt Statusmeldungen an, um anzugeben, wann bestimmte bestehende Angebote migriert wurden.

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