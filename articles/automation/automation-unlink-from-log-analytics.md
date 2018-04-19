---
title: Aufheben der Verknüpfung eines Azure Automation-Kontos mit Log Analytics
description: Dieser Artikel bietet eine Übersicht über das Aufheben der Verknüpfung eines Azure Automation-Kontos mit einem Log Analytics-Arbeitsbereich.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/04/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 4928f1b92e84fc2b960c1f41e7531de9e346dfa2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Aufheben der Verknüpfung eines Automation-Kontos mit einem Log Analytics-Arbeitsbereich

Azure Automation ist in Log Analytics nicht nur zum Unterstützen der Überwachung Ihrer Runbookaufträge in allen Ihren Automation-Konten integriert, sondern auch erforderlich, wenn Sie die folgenden von Log Analytics abhängigen Lösungen importieren:

* [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md)
* [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md)
* [Starten und Beenden von VMs außerhalb der Kernzeit](automation-solution-vm-management.md)

Wenn Sie Ihr Automation-Konto nicht länger in Log Analytics integriert sein soll, können Sie die Verknüpfung direkt im Azure-Portal aufheben.  Bevor Sie fortfahren, müssen Sie zuerst die zuvor erwähnten Lösungen entfernen, da dieser Prozess andernfalls nicht fortgesetzt werden kann. Lesen Sie das Thema für die jeweilige Lösung, die Sie importiert haben, um die Schritte zu deren Entfernung zu verstehen.

Nach dem Entfernen dieser Lösungen können Sie die folgenden Schritte ausführen, um die Verknüpfung Ihres Automation-Kontos aufzuheben.

> [!NOTE]
> Einige Lösungen – einschließlich früherer Versionen der Azure SQL-Überwachungslösung – haben möglicherweise Automatisierungsressourcen erstellt und müssen möglicherweise vor dem Aufheben der Verknüpfung des Arbeitsbereichs entfernt werden.

## <a name="unlink-workspace"></a>Aufheben der Verknüpfung mit dem Arbeitsbereich

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und klicken Sie auf der Seite „Automation-Konto“ im Abschnitt **Zugehörige Ressourcen** auf **Verknüpfung des Arbeitsbereichs aufheben**.

   ![Option „Verknüpfung des Arbeitsbereichs aufheben“](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)

1. Klicken Sie auf der Seite „Verknüpfung des Arbeitsbereichs aufheben“ auf **Verknüpfung des Arbeitsbereichs aufheben**.

   ![Seite „Verknüpfung des Arbeitsbereichs aufheben“](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png)zu erstellen und zu verwalten.

   Sie werden gefragt, ob Sie fortfahren möchten.

1. Während Azure Automation versucht, die Verknüpfung des Kontos mit Ihrem Log Analytics-Arbeitsbereich aufzuheben, können Sie den Fortschritt unter **Benachrichtigungen** im Menü nachverfolgen.

Wenn Sie die Lösung „Updateverwaltung“ verwendet haben, können Sie optional die folgenden Elemente entfernen, die nach dem Entfernen der Lösung nicht mehr benötigt werden.

* Aktualisieren Sie Zeitpläne.  Diese verfügen über Namen, die den Updatebereitstellungen entsprechen, die Sie erstellt haben.

* Für die Lösung erstellte Hybrid Worker-Gruppen.  Jede wird ähnlich wie „machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8“ benannt.

Wenn Sie die Lösung „Starten und Beenden von VMs außerhalb der Kernzeit“ verwendet haben, können Sie optional die folgenden Elemente entfernen, die nach dem Entfernen der Lösung nicht mehr benötigt werden.

* Starten und beenden Sie Zeitpläne für VM-Runbooks.
* Starten und beenden Sie VM-Runbooks.
* Variables

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Neukonfigurieren Ihres Automation-Kontos für die Integration in Log Analytics finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics](automation-manage-send-joblogs-log-analytics.md).