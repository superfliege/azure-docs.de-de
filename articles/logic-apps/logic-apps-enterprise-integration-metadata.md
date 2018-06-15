---
title: Verwalten von Artefaktmetadaten in Integrationskonten – Azure Logic Apps| Microsoft-Dokumentation
description: Hinzufügen oder Abrufen von Artefaktmetadaten aus Integrationskonten für Azure Logic Apps
author: padmavc
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/23/2018
ms.author: LADocs; padmavc
ms.openlocfilehash: 3e7ef6aef9bc1062ae0f76adfbaf086961fcaa94
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298364"
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Verwalten von Artefaktmetadaten in Integrationskonten für Logik-Apps

Sie können benutzerdefinierte Metadaten für Artefakte in Integrationskonten definieren und diese Metadaten während der Laufzeit der Logik-App abrufen. Beispielsweise können Sie Metadaten für Artefakte wie Partner, Vereinbarungen, Schemas und Zuordnungen angeben – alle speichern Metadaten mit Schlüssel-Wert-Paaren. 

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Hinzufügen von Metadaten zu Artefakten in Integrationskonten

1. Erstellen Sie im Azure-Portal ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md).

2. Fügen Sie Ihrem Integrationskonto ein Artefakt hinzu, z. B. [Partner](logic-apps-enterprise-integration-partners.md), [Vereinbarung](logic-apps-enterprise-integration-agreements.md) oder [Schema](logic-apps-enterprise-integration-schemas.md).

3. Wählen Sie das Artefakt und dann **Bearbeiten** aus, und geben Sie Metadatendetails ein.

   ![Eingeben von Metadaten](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Abrufen von Metadaten aus Artefakten für Logik-Apps

1. Erstellen Sie im Azure-Portal eine [Logik-App](quickstart-create-first-logic-app-workflow.md).

2. Erstellen Sie eine [Verknüpfung Ihrer Logik-App mit Ihrem Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

3. Fügen Sie Ihrer Logik-App im Logik-App-Designer einen Trigger wie **Anforderung** oder **HTTP** hinzu.

4. Wählen Sie unter dem Trigger **Neuer Schritt** > **Aktion hinzufügen** aus. Suchen Sie nach „Integrationskonto“, und wählen Sie dann die Aktion **Integrationskonto – Artefaktsuche für Integrationskonto** aus.

   ![Auswählen der Artefaktsuche für Integrationskonto](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Wählen Sie den **Artefakttyp** aus, und geben Sie den **Artefaktnamen** ein. Beispiel: 

   ![Auswählen des Artefakttyps und Eingeben des Namens des Artefakts](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Beispiel: Abrufen von Metadaten für Partner

Angenommen, dieser Partner enthält Metadaten mit Informationen zur `routingUrl`:

![Suchen von „RoutingURL“-Metadaten für Partner](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Fügen Sie Ihrer Logik-App Ihren Trigger, die Aktion **Integrationskonto – Artefaktsuche für Integrationskonto** für Ihren Partner und eine **HTTP**-Aktion hinzu. Beispiel:

   ![Hinzufügen von Trigger-, Artefaktsuche- und HTTP-Aktionen zur Logik-App](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Wählen Sie zum Abrufen des URI auf der Symbolleiste des Logik-App-Designers die **Codeansicht** für Ihre Logik-App aus. Die Definition der Logik-App sollte wie im folgenden Beispiel aussehen:

   ![Suchen Sie „lookup“ (Suchen).](media/logic-apps-enterprise-integration-metadata/image5.png)

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Vereinbarungen](logic-apps-enterprise-integration-agreements.md)
