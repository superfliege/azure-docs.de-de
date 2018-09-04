---
title: Verwalten von Artefaktmetadaten in Integrationskonten – Azure Logic Apps| Microsoft-Dokumentation
description: Hinzufügen oder Abrufen von Artefaktmetadaten aus Integrationskonten in Azure Logic Apps mit Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 02/23/2018
ms.openlocfilehash: 537014c2780fe94cfb35806759f8bcbd974c4c95
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128802"
---
# <a name="manage-artifact-metadata-from-integration-accounts-in-azure-logic-apps-with-enterprise-integration-pack"></a>Verwalten von Artefaktmetadaten aus Integrationskonten in Azure Logic Apps mit Enterprise Integration Pack

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
