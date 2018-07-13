---
title: Zugreifen auf andere APIs mithilfe von Connectors in Azure Content Moderator | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Connectors auf andere APIs für Ihre Content Moderator-Workflows zugreifen können.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: d8114457e7079ca8772cab830bd011dcddf372f5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373034"
---
# <a name="connectors"></a>Connectors

Azure Content Moderator-Workflows können neben Content Moderator-APIs andere APIs verwenden. Mithilfe eines Connectors in Content Moderator greifen Sie auf andere APIs zu. Der Connector stellt einen Link zu den anderen APIs bereit.

Content Moderator umfasst die folgenden Standardconnectors:

* Emotionen-API
* Gesichtserkennungs-API
* PhotoDNA-Clouddienst

![Für Content Moderator verfügbare Connectors](images/connectors-1.png)

## <a name="verify-your-credentials"></a>Überprüfen Ihrer Anmeldeinformationen 

Vergewissern Sie sich vor dem Definieren eines Workflows, dass Sie über gültige Anmeldeinformationen für die gewünschte Connector-API verfügen:

1.  Wählen Sie im Prüfungstool „Dashboard“ **Einstellungen** > **Connectors** aus.

  ![Auswahl von „Connectors“ in Content Moderator](images/connectors-2.png)

2.  Klicken Sie auf das **Bearbeitungssymbol** neben dem Connector, für den Sie Anmeldeinformationen überprüfen möchten.

  ![Auswahl des Bearbeitungssymbols in Content Moderator](images/connectors-3.png)

3.  Der Abonnementschlüssel wird angezeigt. Wenn Sie Änderungen vornehmen, klicken Sie danach auf **Speichern**.

  ![Seite „Connectors bearbeiten“ in Content Moderator](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>Hinzufügen eines Connectors

1.  Bevor Sie einen Connector hinzufügen, benötigen Sie einen Abonnementschlüssel. Wählen Sie im Prüfungstool „Dashboard“ **Einstellungen** > **Anmeldeinformationen** aus. Markieren Sie den Wert im Feld **Ocp-Admin-Subscription-Key**, und kopieren Sie ihn.

2.  Wählen Sie **Connectors** aus. Wählen Sie einen der verfügbaren Connectors aus, die im Prüfungstool „Dashboard“ angezeigt werden. Wählen Sie dann **Verbinden** aus. 

  ![Seite „Connector hinzufügen“ in Content Moderator](images/connectors-5.png)

3.  Fügen Sie in das Feld **Ocp-Admin-Subscription-Key** den Schlüssel ein, den Sie kopiert haben. Wählen Sie anschließend **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie über das [Definieren von benutzerdefinierten Workflows](workflows.md) mithilfe von Connectors.
