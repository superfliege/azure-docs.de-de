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
ms.openlocfilehash: 6d6e6a2aea867c5b603d950a4bbaa33f14695f45
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47010972"
---
Melden Sie sich mit den Anmeldeinformationen, die Sie für das Azure-Abonnement verwenden, beim [Azure-Portal](https://portal.azure.com/) an. Wenn Sie kein Azure-Abonnement besitzen, können Sie jetzt ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

Das Arbeitsbereichdashboard im Portal wird nur in den Browsern Edge, Chrome und Firefox unterstützt.

   ![Azure-Portal](./media/aml-create-in-portal/portal-dashboard.png)

Klicken Sie in der linken oberen Ecke des Portals auf die Schaltfläche **Ressource erstellen** (+).

   ![Erstellen einer Ressource im Azure-Portal](./media/aml-create-in-portal/portal-create-a-resource.png)

Geben Sie in der Suchleiste **Machine Learning** ein. Wählen Sie das Suchergebnis mit dem Namen **Machine Learning-Arbeitsbereich** aus.

   ![Suchen nach Arbeitsbereich](./media/aml-create-in-portal/allservices-search.PNG)

Scrollen Sie im Bereich **Machine Learning-Arbeitsbereich** nach unten, und wählen Sie **Erstellen** aus, um zu beginnen.

   ![create](./media/aml-create-in-portal/portal-create-button.png)

Konfigurieren Sie Im Bereich **ML-Arbeitsbereich** Ihren Arbeitsbereich.

   Feld|BESCHREIBUNG
   ---|---
   Arbeitsbereichname |Geben Sie einen eindeutigen Namen ein, der Ihren Arbeitsbereich identifiziert.  Hier verwenden wir „docs-ws“. Namen müssen in der Ressourcengruppe eindeutig sein. Verwenden Sie einen Namen, der leicht zu merken ist und sich von den von anderen Benutzern erstellten Arbeitsbereichen unterscheidet.  
   Abonnement |Wählen Sie das gewünschte Azure-Abonnement aus. Wählen Sie bei mehreren Abonnements das Abonnement aus, über das die Ressource abgerechnet wird.
   Ressourcengruppe | Verwenden Sie eine vorhandene Ressourcengruppe in Ihrem Abonnement, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen. Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält.  Hier verwenden wir „docs-aml“. 
   Standort | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist. Dort wird der Arbeitsbereich erstellt.

   ![Arbeitsbereich erstellen](./media/aml-create-in-portal/workspace-create.png)

Wählen Sie **Erstellen**, um den Erstellungsprozess zu starten.  Es kann einige Augenblicke dauern, bis der Arbeitsbereich erstellt wurde.

   Um den Status der Bereitstellung zu überprüfen, wählen Sie das Benachrichtigungssymbol (Glocke) in der Symbolleiste aus.

   ![Arbeitsbereich erstellen](./media/aml-create-in-portal/notifications.png)

   Wenn Sie fertig sind, wird eine Erfolgsmeldung zur Bereitstellung angezeigt.  Diese ist auch im Abschnitt „Benachrichtigungen“ vorhanden.   Klicken Sie auf die Schaltfläche **Zu Ressource wechseln**, um den neuen Arbeitsbereich anzuzeigen.
