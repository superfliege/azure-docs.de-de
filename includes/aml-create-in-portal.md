---
title: Includedatei
description: Includedatei
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/24/2018
ms.openlocfilehash: 05331c710817e575deb7729189c9b2d8ccbafd7d
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54489561"
---
1. Melden Sie sich mit den Anmeldeinformationen, die Sie für das Azure-Abonnement verwenden, beim [Azure-Portal](https://portal.azure.com/) an. 

   ![Azure-Portal](./media/aml-create-in-portal/portal-dashboard.png)

1. Wählen Sie links oben im Portal **Ressource erstellen** aus.

   ![Erstellen einer Ressource im Azure-Portal](./media/aml-create-in-portal/portal-create-a-resource.png)

1. Geben Sie in der Suchleiste **Machine Learning** ein. Wählen Sie das Suchergebnis **Machine Learning-Dienstarbeitsbereich** aus.

   ![Suchen nach einem Arbeitsbereich](./media/aml-create-in-portal/allservices-search.PNG)

1. Scrollen Sie im Bereich **ML-Dienstarbeitsbereich** nach unten, und wählen Sie **Erstellen** aus, um zu beginnen.

   ![Erstellen](./media/aml-create-in-portal/portal-create-button.png)

1. Konfigurieren Sie Ihren Arbeitsbereich im Bereich **ML-Dienstarbeitsbereich**.

   Feld|BESCHREIBUNG
   ---|---
   Arbeitsbereichname |Geben Sie einen eindeutigen Namen ein, der Ihren Arbeitsbereich identifiziert. In diesem Beispiel verwenden wir **docs-ws**. Namen müssen in der Ressourcengruppe eindeutig sein. Verwenden Sie einen Namen, der leicht zu merken ist und sich von den von anderen Benutzern erstellten Arbeitsbereichen unterscheidet.  
   Abonnement |Wählen Sie das gewünschte Azure-Abonnement aus.
   Ressourcengruppe | Verwenden Sie eine vorhandene Ressourcengruppe in Ihrem Abonnement, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen. Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. In diesem Beispiel verwenden wir **docs-aml**. 
   Standort | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist. Dort wird der Arbeitsbereich erstellt.

   ![Arbeitsbereich erstellen](./media/aml-create-in-portal/workspace-create.png)

1. Wählen Sie **Erstellen** aus, um den Erstellungsprozess zu starten. Es kann einige Augenblicke dauern, bis der Arbeitsbereich erstellt wurde.

1. Um den Status der Bereitstellung zu überprüfen, wählen Sie das Benachrichtigungssymbol (**Glocke**) in der Symbolleiste aus.

1. Wenn der Vorgang abgeschlossen ist, wird eine Erfolgsmeldung zur Bereitstellung angezeigt. Diese finden Sie auch im Abschnitt „Benachrichtigungen“. Um den neuen Arbeitsbereich anzuzeigen, wählen Sie **Zu Ressource wechseln** aus.

   ![Status der Arbeitsbereichserstellung](./media/aml-create-in-portal/notifications.png)
