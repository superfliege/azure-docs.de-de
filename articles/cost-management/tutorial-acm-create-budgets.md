---
title: 'Tutorial: Erstellen und Verwalten von Azure-Budgets | Microsoft-Dokumentation'
description: Dieses Tutorial hilft bei der Planung und Abrechnung der Kosten für Azure-Dienste, die Sie in Anspruch nehmen.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: b41d086c092f3b18715d8fb70cd1a487a97c6869
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814043"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Tutorial: Erstellen und Verwalten von Azure-Budgets

Budgets in Cost Management helfen Ihnen, die organisatorische Verantwortlichkeit zu planen und zu steigern. Mit Budgets können Sie die Azure-Dienste abrechnen, die Sie in einem bestimmten Zeitraum in Anspruch nehmen oder abonnieren. Sie unterstützen Sie dabei, andere über ihre Ausgaben zu informieren, um die Kosten proaktiv zu steuern und die Entwicklung der Ausgaben im Laufe der Zeit zu überwachen. Wenn die erstellte Budgetschwellenwerte überschritten werden, werden nur Benachrichtigungen ausgelöst. Keines Ihrer Ressourcen wird beeinträchtigt, und die Nutzung wird nicht beendet. Sie können Budgets verwenden, um Ausgaben bei der Kostenanalyse zu vergleichen und zu verfolgen.

Monatliche Budgets werden alle vier Stunden mit den Ausgaben verglichen. Daten und Benachrichtigungen zu verbrauchten Ressourcen sind jedoch binnen acht Stunden verfügbar.  

Die Budgets werden am Ende eines Zeitraums (monatlich, vierteljährlich oder jährlich) auf den gleichen Budgetbetrag automatisch zurückgesetzt, wenn Sie ein zukünftiges Ablaufdatum auswählen. Da sie auf den gleichen Budgetbetrag zurückgesetzt werden, müssen Sie separate Budgets anlegen, wenn die budgetierten Währungsbeträge für zukünftige Zeiträume abweichen.

Die Beispiele in diesem Tutorial führen Sie durch die Erstellung und Bearbeitung eines Budgets für ein Abonnement von Azure Enterprise Agreement (EA).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Budgets im Azure-Portal
> * Bearbeiten eines Budgets

## <a name="prerequisites"></a>Voraussetzungen

Budgets werden für eine Vielzahl von Azure-Kontotypen unterstützt. Die vollständige Liste der unterstützten Kontotypen finden Sie unter [Grundlegendes zu Cost Management-Daten](understand-cost-mgt-data.md). Um Budgets anzeigen zu können, müssen Sie mindestens über Lesezugriff auf Ihr Azure-Konto verfügen.

 Zum Anzeigen von Budgets für Azure EA-Abonnements müssen Sie über Lesezugriff verfügen. Zum Erstellen und Verwalten von Budgets müssen Sie über die Berechtigung „Mitwirkender“ verfügen. Sie können individuelle Budgets für EA-Abonnements und Ressourcengruppen erstellen. Sie können jedoch keine Budgets für EA-Abrechnungskonten erstellen.

Die folgenden Azure-Berechtigungen werden pro Abonnement für Budgets nach Benutzer und Gruppe unterstützt:

- Besitzer – kann Budgets für ein Abonnement erstellen, ändern oder löschen.
- Mitwirkender und Mitwirkender für Cost Management – kann eigene Budgets erstellen, ändern oder löschen. Kann den Betrag für von anderen Personen erstellten Budgets ändern.
- Leser und Leser für Cost Management – kann Budgets anzeigen, für die er die Berechtigung hat.

Weitere Informationen zum Zuweisen der Berechtigung für Cost Management-Daten finden Sie unter [Zuweisen des Zugriffs auf Daten in Cost Management](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

- Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-budget-in-the-azure-portal"></a>Erstellen eines Budgets im Azure-Portal

Sie können ein Azure-Abonnementbudget für den Zeitraum eines Monats, Quartals oder Jahres erstellen. Ihr Navigationsinhalt im Azure-Portal bestimmt, ob Sie ein Budget für ein Abonnement oder für eine Ressourcengruppe anlegen. Navigieren Sie beispielsweise im Azure-Portal zu **Abonnements**, wählen Sie ein Abonnement aus, und klicken Sie auf **Budgets**. In diesem Beispiel erstellen Sie ein Budget für ein von Ihnen ausgewähltes Abonnement. Wenn Sie ein Budget für eine Ressourcengruppe erstellen möchten, navigieren Sie zu **Ressourcengruppen**, wählen Sie eine Ressourcengruppe aus, und klicken Sie auf **Budgets**.

Nachdem Sie Budgets erstellt haben, wird eine einfache Ansicht Ihrer aktuellen Ausgaben im Abgleich mit dem Budget angezeigt.

Klicken Sie auf **Hinzufügen**.

![Im Azure-Portal angezeigte Cost Management-Budgets](./media/tutorial-acm-create-budgets/budgets01.png)

Geben Sie im Fenster **Budget erstellen** einen Namen und einen Betrag für das Budget ein. Wählen Sie anschließend eine Dauer von einem Monat, einem Quartal oder einem Jahr aus. Wählen Sie als Nächstes ein Enddatum aus. Budgets erfordern mindestens einen Kostenschwellenwert (% des Budgets) und eine entsprechende E-Mail-Adresse. Sie können optional bis zu fünf Schwellenwerte und fünf E-Mail-Adressen in ein einzelnes Budget aufnehmen. Wenn ein Budgetschwellenwert erreicht wird, werden E-Mail-Benachrichtigungen normalerweise in weniger als acht Stunden empfangen. Weitere Informationen zu Benachrichtigungen finden Sie unter [Verwenden von Kostenwarnungen](cost-mgt-alerts-monitor-usage-spending.md).

Hier ist ein Beispiel für das Erstellen einer Monatsbudget von 4.500 USD. Eine E-Mail-Warnung wird generiert, wenn 90 % des Budgets erreicht sind.

![Beispielinformationen im Feld „Budget erstellen“](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Die Erstellung eines Quartalsbudgets entspricht der eines Monatsbudget. Der Unterschied besteht darin, dass der Budgetbetrag für das Quartal gleichmäßig auf die drei Monate des Quartals verteilt wird. Erwartungsgemäß wird ein jährlicher Budgetbetrag gleichmäßig auf alle 12 Monate des Kalenderjahres verteilt.

Die aktuellen Ausgaben werden im Abgleich mit dem Budgets aktualisiert, wenn Cost Management aktualisierte Abrechnungsdaten erhält. In der Regel erfolgt dies täglich.

![Beispielinformationen mit aktuellen Ausgaben im Vergleich zu Budgets](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Nach der Erstellung eines Budgets wird es in der Kostenanalyse angezeigt. Die Betrachtung Ihres Budgets in Bezug auf Ihren Ausgabentrend ist einer der ersten Schritte in der [Analyse Ihrer Kosten und Ausgaben](quick-acm-cost-analysis.md).

![Beispiel für in der Kostenanalyse angezeigtes Budget und Ausgaben](./media/tutorial-acm-create-budgets/cost-analysis.png)

Im obigen Beispiel haben Sie ein Budget für ein Abonnement erstellt. Sie können jedoch auch ein Budget für eine Ressourcengruppe erstellen. Wenn Sie ein Budget für eine Ressourcengruppe erstellen möchten, navigieren Sie zu **Cost Management + Abrechnung** &gt; **Abonnements** &gt;wählen Sie ein Abonnement > **Ressourcengruppen** > wählen Sie eine Ressourcengruppe aus > **Budgets** > und denn Budget **hinzufügen**.

## <a name="edit-a-budget"></a>Bearbeiten eines Budgets

Abhängig von Ihrer Zugriffsebene können Sie ein Budget bearbeiten, um seine Eigenschaften zu ändern. Im folgenden Beispiel sind einige der Eigenschaften schreibgeschützt, da der Benutzer nur die Berechtigung „Mitwirkender“ für das Abonnement besitzt. Derzeit ist das **Ablaufdatum** deaktiviert und kann nach dem Festlegen nicht geändert werden.

![Beispiel für die Bearbeitung eines Budgets zum Ändern verschiedener Eigenschaften](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Budgets im Azure-Portal
> * Bearbeiten eines Budgets

Fahren Sie mit dem nächsten Tutorial fort, um einen sich wiederholenden Export für Ihre Cost Management-Daten zu erstellen.

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von exportierten Daten](tutorial-export-acm-data.md)
