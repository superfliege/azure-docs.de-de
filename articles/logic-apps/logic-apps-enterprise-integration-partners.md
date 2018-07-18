---
title: Erstellen von Partnern für B2B-Nachrichten (Business-to-Business) – Azure Logic Apps | Microsoft Docs
description: Erfahren Sie, wie Sie Ihrem Integrationskonto mit dem Enterprise Integration Pack und Logic Apps Partner hinzufügen.
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: jeconnoc
editor: ''
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4abfdaa8145f14bc6496e57ebce0da10cdd2c365
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299738"
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Hinzufügen oder Aktualisieren von Partnern in B2B-Vereinbarungen in Ihrem Workflow

Partner sind Entitäten, die an B2B-Transaktionen (Business-To-Business) teilnehmen und gegenseitig Nachrichten austauschen. Bevor Sie Partner erstellen können, die Sie und eine andere Organisation in diesen Transaktionen darstellen, müssen Sie Informationen abstimmen, die zum Identifizieren und Überprüfen der gegenseitig gesendeten Nachrichten dienen. Sobald Sie diese Details abgestimmt haben und bereit sind, Ihre Geschäftsbeziehung zu starten, können Sie in Ihrem Integrationskonto Partner erstellen, die beiden Parteien darstellen.

## <a name="what-roles-do-partners-play-in-your-integration-account"></a>Welche Rollen haben Partner in Ihrem Integrationskonto?

Um Details zu den zwischen Partnern ausgetauschten Nachrichten zu definieren, erstellen Sie Vereinbarungen zwischen diesen Partnern. Bevor Sie jedoch eine Vereinbarung erstellen können, müssen Sie Ihrem Integrationskonto mindestens zwei Partner hinzugefügt haben. Ihre Organisation muss als der **Hostpartner** bei der Vereinbarung sein. Der andere Partner bzw. **Gastpartner** stellt die Organisation dar, die Nachrichten mit Ihrer Organisation austauscht. Der Gastpartner kann ein anderes Unternehmen oder sogar eine Abteilung innerhalb Ihrer eigenen Organisation sein.

Nachdem Sie diese Partner hinzugefügt haben, können Sie eine Vereinbarung erstellen.

Die Empfangs- und Sendeeinstellungen orientieren sich am Hostpartner. Die Empfangseinstellungen in einer Vereinbarung bestimmen beispielsweise, wie der Hostpartner Nachrichten von einem Gastpartner empfängt. Die Sendeeinstellungen in der Vereinbarung geben an, wie der Hostpartner Nachrichten an den Gastpartner sendet.

## <a name="create-partner"></a>Erstellen von Partnern

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im Azure-Hauptmenü die Option **Alle Dienste** aus. Geben Sie im Suchfeld das Wort „Integration“ ein, und wählen Sie dann **Integrationskonten** aus.

   ![Suchen von Integrationskonten](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. Wählen Sie unter **Integrationskonten** das Integrationskonto aus, dem Sie Ihre Partner hinzufügen möchten.

   ![Auswählen des Integrationskontos](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Wählen Sie die Kachel **Partner** aus.

   ![Auswählen von „Partner“](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Wählen Sie unter **Partner** die Option **Hinzufügen** aus.

   ![„Hinzufügen“ auswählen](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Geben Sie einen Namen für den Partner ein, und wählen Sie dann einen **Qualifizierer** aus. Geben Sie einen **Wert** zur Identifikation der Dokumente, die Ihre Apps empfangen, ein. Wählen Sie **OK** aus, wenn Sie fertig sind.

   ![Hinzufügen von Partnerdetails](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Wählen Sie erneut die Kachel **Partner** aus.

   ![Auswählen der Kachel „Partner“](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   Ihr neuer Partner wird jetzt angezeigt. 

   ![Anzeigen neuer Partner](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="edit-partner"></a>Bearbeiten von Partnern

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und wählen Sie es aus. Wählen Sie die Kachel **Partner** aus.

   ![Auswählen der Kachel „Partner“](./media/logic-apps-enterprise-integration-partners/edit.png)

2. Wählen Sie unter **Partner** den Partner aus, den Sie bearbeiten möchten.

   ![Auswählen eines Partners zum Löschen](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. Nehmen Sie unter **Partner aktualisieren** die gewünschten Änderungen vor.
Wenn Sie fertig sind, wählen Sie **Speichern** aus. 

   ![Vornehmen von Änderungen und Speichern](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   Um Ihre Änderungen rückgängig zu machen, wählen Sie **Verwerfen** aus.

## <a name="delete-partner"></a>Löschen eines Partners

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und wählen Sie es aus. Wählen Sie die Kachel **Partner** aus.

   ![Auswählen der Kachel „Partner“](./media/logic-apps-enterprise-integration-partners/delete.png)

2. Wählen Sie unter **Partner** den Partner aus, den Sie löschen möchten.
Wählen Sie **Löschen** aus.

   ![Löschen eines Partners](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  

