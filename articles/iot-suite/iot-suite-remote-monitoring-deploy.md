---
title: "Bereitstellen der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "Dieses Tutorial veranschaulicht, wie Sie die vorkonfigurierte Remoteüberwachungslösung über azureiotsuite.com bereitstellen."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 61d36113e60c6bb02ea053493ae461993b18c447
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>Bereitstellen der vorkonfigurierten Remoteüberwachungslösung

Dieses Tutorial zeigt, wie Sie eine vorkonfigurierte Lösung für die Remoteüberwachung bereitstellen. Sie stellen die Lösung über azureiotsuite.com bereit. Sie können die Lösung auch über die Befehlszeilenschnittstelle bereitstellen. Informationen zu dieser Möglichkeit finden Sie unter [Deploy a preconfigured solution from the command line](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploy-a-pcs-from-the-command-line) (Bereitstellen einer vorkonfigurierten Lösung über die Befehlszeile).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren der vorkonfigurierten Lösung
> * Bereitstellen der vorkonfigurierten Lösung
> * Anmelden bei der vorkonfigurierten Lösung

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-preconfigured-solution"></a>Bereitstellen der vorkonfigurierten Lösung

Vor dem Bereitstellen der vorkonfigurierten Lösung in Ihrem Azure-Abonnement müssen Sie einige Konfigurationsoptionen auswählen:

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto bei [azureiotsuite.com](https://www.azureiotsuite.com) an, und klicken Sie auf **+**, um eine neue Lösung zu erstellen:

    ![Erstellen einer neuen Lösung](media/iot-suite-remote-monitoring-deploy/createnewsolution.png)

1. Klicken Sie auf der Kachel **Remote monitoring preview** (Vorschauversion der Remoteüberwachung) auf **Auswählen**.

    ![Auswählen der Remoteüberwachung](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. Geben Sie auf der Seite **Create Remote Monitoring solution** (Remoteüberwachungslösung erstellen) einen **Lösungsnamen** für Ihre vorkonfigurierte Remoteüberwachungslösung ein.

1. Wählen Sie **Basic** oder **Standard** als Bereitstellungsart aus. Wenn Sie die Lösung bereitstellen, um sich mit ihr vertraut zu machen oder um sie zu Demonstrationszwecken auszuführen, wählen Sie zur Kostenminimierung die Option **Basic** aus.

1. Wählen Sie als Sprache entweder **Java** oder **.NET** aus. Alle Microservices sind als Java- oder .NET-Implementierungen verfügbar.

1. Im Bereich **Lösungsdetails** finden Sie weitere Informationen zu den verfügbaren Konfigurationsoptionen.

1. Wählen Sie die gewünschten Angaben für **Abonnement** und **Region**, um die Lösung bereitzustellen.

1. Klicken Sie auf **Lösung erstellen** , um den Bereitstellungsprozess zu beginnen. Dieser dauert in der Regel einige Minuten:

    ![Details zur Remoteüberwachungslösung](media/iot-suite-remote-monitoring-deploy/createform.png)

Informationen zur Problembehandlung finden Sie im GitHub-Repository unter [What to do when a deployment fails](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) (Vorgehensweise bei Fehlern bei der Bereitstellung).

## <a name="sign-in-to-the-preconfigured-solution"></a>Anmelden bei der vorkonfigurierten Lösung

Nach Abschluss des Bereitstellungsvorgangs können Sie sich bei der vorkonfigurieren Remoteüberwachungslösung anmelden.

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
> * Konfigurieren der vorkonfigurierten Lösung
> * Bereitstellen der vorkonfigurierten Lösung
> * Anmelden bei der vorkonfigurierten Lösung

Nach dem Bereitstellen der Remoteüberwachungslösung können Sie nun im nächsten Schritt [die Funktionen des Lösungsdashboards untersuchen](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->