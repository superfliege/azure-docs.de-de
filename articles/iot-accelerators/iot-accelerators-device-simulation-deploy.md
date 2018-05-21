---
title: Bereitstellen der Gerätesimulationslösung – Azure | Microsoft-Dokumentation
description: Dieses Tutorial veranschaulicht, wie Sie die Gerätesimulationslösung über azureiotsuite.com bereitstellen.
services: iot device simulation
suite: iot-suite
author: troyhopwood
manager: timlt
ms.author: troyhop
ms.service: iot-suite
ms.date: 12/18/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 4d468c527c658707da2b5f35f43676626baf5ca2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="deploy-the-azure-iot-device-simulation-solution"></a>Bereitstellen der Azure-IoT-Gerätesimulationslösung

Dieses Tutorial veranschaulicht, wie Sie eine Gerätesimulationslösung bereitstellen. Sie stellen die Lösung über azureiotsuite.com bereit.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren der Gerätesimulationslösung
> * Bereitstellen der Gerätesimulationslösung
> * Anmelden bei der Gerätesimulationslösung

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Vor dem Bereitstellen der Lösung in Ihrem Azure-Abonnement müssen Sie einige Konfigurationsoptionen auswählen:

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto bei [azureiotsuite.com](https://www.azureiotsuite.com) an, und klicken Sie auf **+**, um eine neue Lösung zu erstellen:

    ![Erstellen einer neuen Lösung](./media/iot-accelerators-device-simulation-deploy/createnewsolution.png)

1. Klicken Sie auf der Kachel **Gerätesimulation** auf **Auswählen**:

    ![Auswählen der Gerätesimulation](./media/iot-accelerators-device-simulation-deploy/select.png)

1. Geben Sie auf der Seite **Gerätesimulationslösung erstellen** einen **Lösungsnamen** für Ihre Gerätesimulationslösung ein.

1. Wählen Sie die gewünschten Angaben für **Abonnement** und **Region**, um die Lösung bereitzustellen.

1. Geben Sie an, ob mit der Gerätesimulationslösung ein neuer IoT Hub bereitgestellt werden soll. Wenn Sie dieses Kontrollkästchen aktivieren, wird in Ihrem Abonnement ein neuer IoT Hub bereitgestellt. Unabhängig von der Auswahl kann Ihre Simulation auf jeden IoT Hub verweisen.

1. Klicken Sie auf **Lösung erstellen** , um den Bereitstellungsprozess zu beginnen. Dieser dauert in der Regel einige Minuten:

    ![Details der Gerätesimulationslösung](./media/iot-accelerators-device-simulation-deploy/createsolution.png)

## <a name="sign-in-to-the-solution"></a>Anmelden bei der Lösung

Nach Abschluss des Bereitstellungsvorgangs können Sie sich bei Ihrer Gerätesimulationslösung anmelden.

1. Klicken Sie auf der Seite **Bereitgestellte Lösungen** unterhalb Ihrer neuen Gerätesimulationslösung auf **Starten**:

    ![Auswählen der neuen Lösung](./media/iot-accelerators-device-simulation-deploy/newsolution.png)

1. Im angezeigten Bereich sehen Sie Informationen zu der Gerätesimulationslösung. Wählen Sie **Lösungsdashboard** aus, um eine Verbindung mit der Gerätesimulationslösung herzustellen.

    > [!NOTE]
    > Sie können die Gerätesimulationslösung aus diesem Bereich löschen, wenn Sie sie nicht mehr benötigen.

    ![Lösungsbereich](./media/iot-accelerators-device-simulation-deploy/properties.png)

1. Das Dashboard der Gerätesimulationslösung wird in Ihrem Browser angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren der Projektmappe
> * Bereitstellen der Lösung
> * Anmelden bei der Lösung

Nach dem Bereitstellen können Sie nun im nächsten Schritt [die Funktionen der Gerätesimulationslösung erkunden](iot-accelerators-device-simulation-explore.md).

<!-- Next tutorials in the sequence -->
