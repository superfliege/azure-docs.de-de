---
title: 'Tutorial: Erstellen und Verwalten von Azure-Budgets | Microsoft-Dokumentation'
description: Dieses Tutorial hilft bei der Planung und Abrechnung der Kosten für Azure-Dienste, die Sie in Anspruch nehmen.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/01/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 50bd22559c3695ac4161932652eb191084e2b46e
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48017357"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Tutorial: Erstellen und Verwalten von Azure-Budgets

Budgets in Cost Management helfen Ihnen, die organisatorische Verantwortlichkeit zu planen und zu steigern. Mit Budgets können Sie die Azure-Dienste abrechnen, die Sie in einem bestimmten Zeitraum in Anspruch nehmen oder abonnieren. Sie unterstützen Sie dabei, andere über ihre Ausgaben zu informieren, um die Kosten proaktiv zu steuern und die Entwicklung der Ausgaben im Laufe der Zeit zu überwachen. Sie können sehen, wie sich die Ausgaben über einen bestimmten Zeitraum entwickeln. Wenn die erstellte Budgetschwellenwerte überschritten werden, werden nur Benachrichtigungen ausgelöst. Keines Ihrer Ressourcen wird beeinträchtigt, und die Nutzung wird nicht beendet. Sie können Budgets verwenden, um Ausgaben bei der Kostenanalyse zu vergleichen und zu verfolgen.

Die Budgets werden am Ende eines Zeitraums (monatlich, vierteljährlich oder jährlich) auf den gleichen Budgetbetrag automatisch zurückgesetzt, wenn Sie ein zukünftiges Ablaufdatum auswählen. Da sie auf den gleichen Budgetbetrag zurückgesetzt werden, müssen Sie separate Budgets anlegen, wenn die budgetierten Währungsbeträge für zukünftige Zeiträume abweichen.

Die Beispiele in diesem Tutorial führen Sie durch die Erstellung und Bearbeitung eines Budgets für ein Abonnement von Azure Enterprise Agreement (EA).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Budgets im Azure-Portal
> * Bearbeiten eines Budgets

## <a name="prerequisites"></a>Voraussetzungen

Budgets stehen allen Azure EA-Kunden zur Verfügung. Zum Erstellen und Verwalten von Budgets benötigen Sie Lesezugriff auf ein Azure EA-Abonnement. EA-Abrechnungskonten werden von Budgets nicht unterstützt.

Budgets werden einzeln entweder auf der Ebene des Abonnements oder der Ressourcengruppe angelegt. Die folgenden Azure-Berechtigungen werden pro Abonnement für Budgets nach Benutzer und Gruppe unterstützt:

- Besitzer – kann Budgets für ein Abonnement erstellen, ändern oder löschen.
- Mitwirkende – kann eigene Budgets erstellen, ändern oder löschen. Kann den Betrag für von anderen Personen erstellten Budgets ändern.
- Leser – kann Budgets planen, für die er die Berechtigung hat.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

- Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.

## <a name="create-a-budget-in-the-azure-portal"></a>Erstellen eines Budgets im Azure-Portal

Sie können ein Azure-Abonnementbudget für den Zeitraum eines Monats, Quartals oder Jahres erstellen. Ihr Navigationsinhalt im Azure-Portal bestimmt, ob Sie ein Budget für ein Abonnement oder für eine Ressourcengruppe anlegen.

Navigieren Sie im Azure-Portal zu **Cost Management + Abrechnung** &gt; **Abonnements** &gt; wählen Sie ein Abonnement aus &gt; **Budgets**. In diesem Beispiel erstellen Sie ein Budget für ein von Ihnen ausgewähltes Abonnement.

Nachdem Sie Budgets erstellt haben, wird eine einfache Ansicht Ihrer aktuellen Ausgaben im Abgleich mit dem Budget angezeigt.

Klicken Sie auf **Hinzufügen**.

![Cost Management-Budgets](./media/tutorial-acm-create-budgets/budgets01.png)

Geben Sie im Fenster **Budget erstellen** einen Namen und einen Betrag für das Budget ein. Wählen Sie anschließend eine Dauer von einem Monat, einem Quartal oder einem Jahr aus. Wählen Sie als Nächstes ein Enddatum aus. Budgets erfordern mindestens einen Kostenschwellenwert (% des Budgets) und eine entsprechende E-Mail-Adresse. Sie können optional bis zu fünf Schwellenwerte und fünf E-Mail-Adressen in ein einzelnes Budget aufnehmen. Wenn ein Budgetschwellenwert erreicht wird, werden E-Mail-Benachrichtigungen normalerweise in weniger als acht Stunden empfangen.

Hier ist ein Beispiel für das Erstellen einer Monatsbudget von 4.500 USD. Eine E-Mail-Warnung wird generiert, wenn 90 % des Budgets erreicht sind.

![Beispiel für ein Monatsbudget](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Die Erstellung eines Quartalsbudgets entspricht der eines Monatsbudget. Der Unterschied besteht darin, dass der Budgetbetrag für das Quartal gleichmäßig auf die drei Monate des Quartals verteilt wird. Erwartungsgemäß wird ein jährlicher Budgetbetrag gleichmäßig auf alle 12 Monate des Kalenderjahres verteilt.

Die aktuellen Ausgaben werden im Abgleich mit dem Budgets aktualisiert, wenn Cost Management aktualisierte Abrechnungsdaten erhält. In der Regel erfolgt dies täglich.

![Abgleich der aktuellen Ausgaben mit den Budgets](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Nach der Erstellung eines Budgets wird es in der Kostenanalyse angezeigt. Die Betrachtung Ihres Budgets in Bezug auf Ihren Ausgabentrend ist einer der ersten Schritte in der [Analyse Ihrer Kosten und Ausgaben](quick-acm-cost-analysis.md).

![In der Kostenanalyse angezeigtes Budget](./media/tutorial-acm-create-budgets/cost-analysis.png)

Im obigen Beispiel haben Sie ein Budget für ein Abonnement erstellt. Sie können jedoch auch ein Budget für eine Ressourcengruppe erstellen. Wenn Sie ein Budget für eine Ressourcengruppe erstellen möchten, navigieren Sie zu **Cost Management + Abrechnung** &gt; **Abonnements** &gt;wählen Sie ein Abonnement > **Ressourcengruppen** > wählen Sie eine Ressourcengruppe aus > **Budgets** > und denn Budget **hinzufügen**.

## <a name="edit-a-budget"></a>Bearbeiten eines Budgets

Abhängig von Ihrer Zugriffsebene können Sie ein Budget bearbeiten, um seine Eigenschaften zu ändern. Im folgenden Beispiel sind einige der Eigenschaften schreibgeschützt, da der Benutzer nur die Berechtigung „Mitwirkender“ für das Abonnement hat. Derzeit ist das **Ablaufdatum** deaktiviert und kann nach dem Festlegen nicht geändert werden.

![Budget bearbeiten – Berechtigung „Mitwirkender“](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Budgets im Azure-Portal
> * Bearbeiten eines Budgets

Fahren Sie mit dem nächsten Tutorial fort, um einen sich wiederholenden Export für Ihre Cost Management-Daten zu erstellen.

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von exportierten Daten](tutorial-export-acm-data.md)
