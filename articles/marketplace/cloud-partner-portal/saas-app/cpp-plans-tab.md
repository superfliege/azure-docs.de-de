---
title: Azure-SaaS-Anwendungsangebotspläne | Azure Marketplace
description: Erstellen eines Plans für ein SaaS-Anwendungsangebot im Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pabutler
ms.openlocfilehash: 2ff86b39f67b170ce99b045f5cfa888e06057bbe
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943344"
---
# <a name="saas-application-plans-tab"></a>Registerkarte „Pläne“ für SaaS-Anwendung

Verwenden Sie die Registerkarte „Pläne“, um einen neuen Plan zu erstellen. Es muss mindestens 1 Plan hinzugefügt werden, wenn Sie für Ihre SaaS-App die Option „Über Microsoft verkaufen“ verwenden.

![Erstellen eines neuen Plans](./media/saas-plans-new-plan.png)

## <a name="create-a-new-plan"></a>Erstellen eines neuen Plans

So erstellen Sie einen neuen Plan:

1. Wählen Sie unter **Pläne** die Option **+ Neuer Plan** aus.
2. Geben Sie im Popupfenster **Neuer Plan** eine **Plan-ID** ein. Die maximale Länge ist 50 Zeichen. Diese ID darf nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche enthalten. Sie können diese ID nicht ändern, nachdem das Angebot veröffentlicht wurde.
3. Wählen Sie **OK** aus, um die Plan-ID zu speichern.

   ![Plan-ID für einen neuen Plan](./media/saas-plans-plan-ID.png)

### <a name="to-configure-the-plan"></a>So konfigurieren Sie den Plan:

1. Geben Sie unter **Plandetails** Informationen in die folgenden Felder ein:

   - Titel: Geben Sie einen Titel für den Plan ein. Der Titel darf maximal 50 Zeichen lang sein.
   - Beschreibung: Geben Sie eine Beschreibung an. Die Beschreibung darf maximal 500 Zeichen lang sein.
   - Ist dies ein privater Plan? : Ist der Plan nur für eine ausgewählte Gruppe von Kunden verfügbar, wählen Sie **Ja** aus.
   - Verfügbarkeit in Land/Region: Der Plan muss in mindestens 1 Land oder Region verfügbar sein. Klicken Sie auf **Regionen auswählen**. Wählen Sie ein Land/eine Region in der Liste **Verfügbarkeit in Land/Region auswählen** aus, und wählen Sie dann **OK** aus. 
   - Legacypreis: Geben Sie die Kosten in USD pro Monat an.

2. Geben Sie unter **Vereinfachte Währungspreise** die folgende Informationen an:

   - Abrechnungszeitraum: „Preis pro Monat“ ist standardmäßig aktiviert. Sie können auch den jährlichen Preise angeben.
   - Preis pro Monat: Geben Sie den monatlichen Preis an, der mit dem Legacypreis übereinstimmen muss.

   >[!NOTE] 
   >Wenn Sie „Preis pro Jahr“ zum Abrechnungszeitraum hinzufügen, werden Sie dazu aufgefordert, den **Preis pro Jahr** in USD anzugeben.

3. Wählen Sie **Speichern** aus, um das Konfigurieren des Plans abzuschließen.

   >[!NOTE] 
   >Nachdem Sie Ihre Preisänderungen gespeichert haben, können Sie die Preisdaten exportieren/importieren.

![Plandetails konfigurieren](./media/saas-plans-plan-details.png)

## <a name="next-steps"></a>Nächste Schritte

[Registerkarte „Channel Info“ (Kanalinformationen)](./cpp-channel-info-tab.md)

