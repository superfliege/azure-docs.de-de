---
title: Bereitstellen der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie den Solution Accelerator für die Remoteüberwachung über „azureiotsuite.com“ bereitstellen.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: eb78ce91755c43f1c6fedf62a70238df911b940f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-the-remote-monitoring-solution-accelerator"></a>Bereitstellen des Solution Accelerators für die Remoteüberwachung

Dieses Tutorial zeigt, wie Sie den Solution Accelerator für die Remoteüberwachung bereitstellen. Sie stellen die Lösung über azureiotsuite.com bereit. Sie können die Lösung auch über die Befehlszeilenschnittstelle bereitstellen. Informationen zu dieser Möglichkeit finden Sie unter [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung über die Befehlszeilenschnittstelle](iot-suite-remote-monitoring-deploy-cli.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren des Solution Accelerators
> * Bereitstellen des Solution Accelerators
> * Anmelden beim Solution Accelerator

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-solution-accelerator"></a>Bereitstellen des Solution Accelerators

Vor dem Bereitstellen des Solution Accelerators in Ihrem Azure-Abonnement müssen Sie einige Konfigurationsoptionen auswählen:

1. Melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen bei [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) an.

1. Klicken Sie auf der Kachel **Remoteüberwachung** auf **Jetzt testen**.

    ![Auswählen der Remoteüberwachung](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. Geben Sie auf der Seite **Remoteüberwachungslösung erstellen** einen **Lösungsnamen** für Ihren Solution Accelerator für die Remoteüberwachung ein.

1. Wählen Sie **Basic** oder **Standard** als Bereitstellungsart aus. Wenn Sie die Lösung bereitstellen, um sich mit ihr vertraut zu machen oder um sie zu Demonstrationszwecken auszuführen, wählen Sie zur Kostenminimierung die Option **Basic** aus.

1. Wählen Sie als Sprache entweder **Java** oder **.NET** aus. Alle Microservices sind als Java- oder .NET-Implementierungen verfügbar.

1. Im Bereich **Lösungsdetails** finden Sie weitere Informationen zu den verfügbaren Konfigurationsoptionen.

1. Wählen Sie die gewünschten Angaben für **Abonnement** und **Region**, um die Lösung bereitzustellen.

1. Klicken Sie auf **Lösung erstellen** , um den Bereitstellungsprozess zu beginnen. Dieser dauert in der Regel einige Minuten:

    ![Details zur Remoteüberwachungslösung](media/iot-suite-remote-monitoring-deploy/createform.png)

Informationen zur Problembehandlung finden Sie im GitHub-Repository unter [What to do when a deployment fails](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) (Vorgehensweise bei Fehlern bei der Bereitstellung).

## <a name="sign-in-to-the-solution-accelerator"></a>Anmelden beim Solution Accelerator

Nach Abschluss des Bereitstellungsvorgangs können Sie sich beim Solution Accelerator für die Remoteüberwachung anmelden.

1. Wählen Sie auf der Seite **Bereitgestellte Lösungen** Ihre neue Remoteüberwachungslösung aus:

    ![Auswählen der neuen Lösung](media/iot-suite-remote-monitoring-deploy/choosenew.png)

1. Im dann angezeigten Bereich können Sie Informationen zu der Remoteüberwachungslösung einsehen. Wählen Sie **Lösungsdashboard** aus, um eine Verbindung mit der Remoteüberwachungslösung herzustellen.

    > [!NOTE]
    > Sie können die Remoteüberwachungslösung aus diesem Bereich löschen, wenn Sie sie nicht mehr benötigen.

    ![Lösungsbereich](media/iot-suite-remote-monitoring-deploy/solutionpanel.png)

1. Das Dashboard der Remoteüberwachungslösung wird in Ihrem Browser angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren des Solution Accelerators
> * Bereitstellen des Solution Accelerators
> * Anmelden beim Solution Accelerator

Nach dem Bereitstellen der Remoteüberwachungslösung können Sie nun im nächsten Schritt [die Funktionen des Lösungsdashboards untersuchen](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->