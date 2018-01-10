---
title: "Konvertieren von JSON-Daten mit Liquid-Transformationen – Azure Logic Apps | Microsoft Docs"
description: "Erstellen Sie mithilfe von Logic Apps und einer Liquid-Vorlage Transformationen oder Zuordnungen für erweiterte JSON-Transformationen."
services: logic-apps
documentationcenter: 
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: LADocs; divswa
ms.openlocfilehash: cd177b1ebcb5d236ce265dc153ee6a02125a69df
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2017
---
# <a name="advanced-json-transformations-using-liquid-template"></a>Erweiterte JSON-Transformationen mit Liquid-Vorlage
Azure Logic Apps unterstützt grundlegende JSON-Transformationen über systemeigene Datenvorgangsaktionen wie „JSON erstellen“ oder „JSON analysieren“. Logic Apps unterstützt jetzt auch erweiterte JSON-Transformationen mit Liquid-Vorlagen. [Liquid](https://shopify.github.io/liquid/) ist eine Open-Source-Vorlagensprache für flexible Web-Apps.
 
In diesem Artikel erfahren Sie, wie Sie eine Liquid-Zuordnung oder -Vorlage verwenden, die komplexere JSON-Transformationen wie Iterationen, Ablaufsteuerungen, Variablen usw. unterstützen kann. Sie müssen eine JSON-zu-JSON-Zuordnung mit dieser Liquid-Zuordnung definieren und die Zuordnung in Ihrem Integrationskonto speichern, bevor Sie eine Liquid-Transformation in Ihrer Logik-App ausführen können.

## <a name="prerequisites"></a>Voraussetzungen
Nachfolgend werden die Voraussetzungen für die Verwendung der Liquid-Aktion aufgeführt:

* Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie [mit einem kostenlosen Azure-Konto beginnen](https://azure.microsoft.com/free/). Sie können sich aber auch [für ein Abonnement mit nutzungsbasierter Bezahlung registrieren](https://azure.microsoft.com/pricing/purchase-options/).

* Grundlegende Kenntnisse über das [Erstellen von Logik-Apps](../logic-apps/logic-apps-create-a-logic-app.md).

* Ein Basic-[Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md).


## <a name="create-and-add-liquid-template-or-map-to-integration-account"></a>Erstellen einer Liquid-Vorlage oder -Zuordnung und Hinzufügen der Vorlage/Zuordnung zum Integrationskonto

1. Erstellen Sie die Liquid-Beispielvorlage für dieses Beispiel. Die Liquid-Vorlage definiert, wie JSON-Eingaben transformiert werden, wie hier beschrieben:

   ```
   {%- assign deviceList = content.devices | Split: ', ' -%}
    {
        "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
        "firstNameUpperCase": "{{content.firstName | Upcase}}",
        "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
        "devices" : [
        {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
    }
    ```
    > [!NOTE]
    > Wenn Ihre Liquid-Vorlage [Filter](https://shopify.github.io/liquid/basics/introduction/#filters) verwendet, müssen diese Filter mit Großbuchstaben beginnen. 

2. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.

3. Wählen Sie im Azure-Hauptmenü die Option **Alle Ressourcen** aus. 

4. Geben Sie im Suchfeld Ihr Integrationskonto an. Wählen Sie Ihr Konto aus.

   ![Auswählen des Integrationskontos](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  Wählen Sie auf der Kachel „Integrationskonto“ die Option **Zuordnungen** aus.

   ![Auswählen von Zuordnungen](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Wählen Sie **Hinzufügen** aus, und geben Sie dann die folgenden Details für die Zuordnung an:
  * **Name**: Der Name für Ihre Zuordnung, der in diesem Beispiel „JsontoJsonTemplate“ lautet.
  * **Zuordnungstyp**: Der Typ Ihrer Zuordnung. Bei JSON-zu-JSON-Transformationen müssen Sie **liquid** auswählen.
  * **Zuordnung**: Eine vorhandene Liquid-Vorlagen- oder -Zuordnungsdatei, die für die Transformation verwendet wird. In diesem Beispiel heißt die Datei „SimpleJsonToJsonTemplate.liquid“. Sie können die Dateiauswahl verwenden, um diese Datei zu suchen.

    ![Hinzufügen der Liquid-Vorlage](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)

    
## <a name="add-the-liquid-action-to-transform-json-in-your-logic-app"></a>Hinzufügen der Liquid-Aktion zum Transformieren von JSON in der Logik-App

1. [Erstellen Sie eine Logik-App](logic-apps-create-a-logic-app.md).

2. Fügen Sie Ihrer Logik-App den [Anforderungstrigger](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) hinzu.

3. Wählen Sie **+ Neuer Schritt > Aktion hinzufügen** aus. Suchen Sie im Suchfeld nach *liquid*. Wählen Sie **Liquid – Von JSON in JSON transformieren** aus.

  ![Suchaktion-liquid](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. Wählen Sie im Feld **Inhalt** in der Liste dynamischer Inhalte oder in der Parameterliste (je nachdem, welche Liste angezeigt wird) den Eintrag **Text** aus. 
  
  ![„Text“ auswählen](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Wählen Sie in der Dropdownliste **Zuordnung** Ihre Liquid-Vorlage aus, die in diesem Beispiel „JsonToJsonTemplate“ heißt.

  ![Zuordnung auswählen](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Wenn die Liste keine Einträge enthält, ist Ihre Logik-App höchstwahrscheinlich nicht mit Ihrem Integrationskonto verknüpft. Führen Sie die folgenden Schritte aus, um Ihre Logik-App mit dem Integrationskonto zu verknüpfen, das die Liquid-Vorlage oder -Zuordnung enthält:

   1. Wählen Sie im Menü Ihrer Logik-App die Option **Workfloweinstellungen** aus. 
   2. Wählen Sie in der Liste **Wählen Sie ein Integrationskonto aus** Ihr Integrationskonto aus, und wählen Sie dann **Speichern** aus.

     ![Verknüpfen der Logik-App mit dem Integrationskonto](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)


## <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

   Senden Sie die JSON-Eingabe über [Postman](https://www.getpostman.com/postman) oder ein ähnliches Tool an Ihre Logik-App. Die transformierte JSON-Ausgabe von der Logik-App sieht wie im folgenden Beispiel dargestellt aus:
  
   ![Beispielausgabe](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)


## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  
* [Weitere Informationen zu Zuordnungen](../logic-apps/logic-apps-enterprise-integration-maps.md "Informationen zu Zuordnungen für die Unternehmensintegration")  

