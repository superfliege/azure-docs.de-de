---
title: Bereitstellen einer cloudbasierten IoT Predictive Maintenance-Lösung in Azure | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung stellen Sie den Azure IoT Solution Accelerator für Predictive Maintenance bereit und melden sich am Lösungsdashboard an, um es zu nutzen.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/07/2018
ms.author: dobett
ms.openlocfilehash: 3671f63b9e27cb6af55c31e3e61dc6d19932f54c
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972912"
---
# <a name="quickstart-deploy-a-cloud-based-solution-to-run-a-predictive-maintenance-analysis-on-my-connected-devices"></a>Schnellstart: Bereitstellen einer cloudbasierten Lösung zum Ausführen einer Predictive Maintenance-Analyse auf meinen verbundenen Geräten

In dieser Schnellstartanleitung wird beschrieben, wie Sie den Azure IoT Solution Accelerator für Predictive Maintenance als cloudbasierte Lösung für die Predictive Maintenance Ihrer IoT-Geräte nutzen. Nachdem Sie den Solution Accelerator bereitgestellt haben, verwenden Sie die Seite **Dashboard** der Lösung, um eine Simulation mithilfe der Daten von Flugzeugtriebwerken auszuführen.

Fabrikam ist eine regionale Fluggesellschaft, deren Schwerpunkt auf hervorragender Kundenzufriedenheit zu wettbewerbsfähigen Preisen liegt. Eine Ursache von Flugverspätungen sind Wartungsprobleme, und insbesondere die Wartung von Flugzeugtriebwerken ist anspruchsvoll. Fabrikam muss unter allen Umständen einen Triebwerksausfall während des Flugs vermeiden. Daher setzt Fabrikam auf eine regelmäßige Inspektion und planmäßige Wartungsarbeiten. Jedes Flugzeugtriebwerk ist jedoch anders. Bisweilen werden unnötige Wartungsarbeiten an Triebwerken durchgeführt. Vor allem können jedoch Umstände auftreten, die ein Flugzeug am Fliegen hindern, bis eine Wartung durchgeführt wird. Dies kann besonders dann teuer sein, wenn sich das Flugzeug an einem Ort befindet, an dem die benötigten Techniker oder Ersatzteile nicht verfügbar sind.

Die Triebwerke der Fabrikam-Flotte sind mit Sensoren ausgestattet, die den Triebwerkszustand während des Flugs erfassen. Nach jahrelanger Erfassung der Betriebs- und Fehlerdaten haben die Datenanalysten von Fabrikam ein Modell zur Vorhersage der Restnutzungsdauer (Remaining Useful Life, RUL) eines Flugzeugtriebwerks entwickelt. Das Modell nutzt eine Korrelation zwischen den Daten von vier der Triebwerksensoren und dem Triebwerkverschleiß, der letztlich zum Ausfall führen kann. Obwohl Fabrikam die reguläre Inspektion aus Sicherheitsgründen fortsetzt, kann das Unternehmen jetzt anhand der Modelle nach jedem Flug die Restnutzungsdauer für jedes Triebwerk berechnen. Dadurch kann Fabrikam künftige Problemstellen prognostizieren und Wartungspläne ausarbeiten, um die Standzeiten der Flugzeuge minimieren und die Betriebskosten zu senken, ohne die Sicherheit von Passagieren und Crew zu beeinträchtigen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Beim Bereitstellen des Solution Accelerators in Ihrem Azure-Abonnement müssen Sie einige Konfigurationsoptionen festlegen.

Melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen an [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) an.

Klicken Sie auf der Kachel **Predictive Maintenance** auf **Jetzt ausprobieren**.

![Auswählen von Predictive Maintenance](./media/quickstart-predictive-maintenance-deploy/predictivemaintenance.png)

Geben Sie auf der Seite **Create Predictive Maintenance solution** (Predictive Maintenance-Lösung erstellen) einen eindeutigen **Lösungsnamen** für den Solution Accelerator für Predictive Maintenance ein.

Wählen Sie die gewünschten Angaben für **Abonnement** und **Region**, um den Solution Accelerator bereitzustellen. Normalerweise wählen Sie die Region, die Ihnen am nächsten liegt. Sie müssen ein [globaler Administrator oder Benutzer](iot-accelerators-permissions.md) des Abonnements sein.

Klicken Sie auf **Lösung erstellen**, um mit der Bereitstellung zu beginnen. Dieser Prozess dauert mindestens fünf Minuten:

![Einzelheiten zur Predictive Maintenance-Lösung](./media/quickstart-predictive-maintenance-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Anmelden bei der Lösung

Nachdem die Bereitstellung für Ihr Azure-Abonnement abgeschlossen ist, können Sie sich am Dashboard des Solution Accelerators für Predictive Maintenance anmelden.

Klicken Sie auf der Seite **Bereitgestellte Lösungen** auf Ihren neuen Solution Accelerator für Predictive Maintenance. Dem daraufhin angezeigten Bereich können Sie Informationen zum Solution Accelerator entnehmen. Wählen Sie die Option **Lösungsdashboard** aus, um Ihren Solution Accelerator für Predictive Maintenance anzuzeigen:

![Lösungsbereich](./media/quickstart-predictive-maintenance-deploy/solutionpanel.png)

Klicken Sie auf **Akzeptieren**, um die Zustimmungsanforderung zu akzeptieren, die im Lösungsdashboard für Predictive Maintenance im Browser angezeigt wird:

![Lösungsdashboard](./media/quickstart-predictive-maintenance-deploy/solutiondashboard.png)

Klicken Sie auf **Simulation starten**, um die Simulation zu starten. Daraufhin füllt sich das Dashboard mit dem Sensorverlauf, der Restlebensdauer, den Zyklen und dem Verlauf der Restlebensdauer:

![Aktive Simulation](./media/quickstart-predictive-maintenance-deploy/simulationrunning.png)

Wenn der Wert für die Restlebensdauer kleiner als 160 ist (zu Demonstrationszwecken willkürlich gewählter Schwellenwert), erscheint im Lösungsportal neben der Anzeige der Restlebensdauer ein Warnsymbol. Außerdem wird das Flugzeugtriebwerk im Lösungsportal gelb hervorgehoben. Beachten Sie, dass die Werte für die Restlebensdauer im Allgemeinen einen Abwärtstrend aufweisen, aber trotzdem relativ stark variieren. Der Grund für dieses Verhalten sind die variierenden Zykluslängen und die Modellgenauigkeit.

![Simulationswarnung](./media/quickstart-predictive-maintenance-deploy/simulationwarning.png)

Die vollständige Simulation dauert ca. 35 Minuten, und es werden 148 Zyklen durchgeführt. Der Schwellenwert der Restlebensdauer von 160 wird zum ersten Mal nach ca. fünf Minuten erreicht, und für beide Triebwerke ist der Schwellenwert nach ca. acht Minuten erreicht.

Die Simulation durchläuft das vollständige Dataset für 148 Zyklen, und die endgültigen Werte für die Restlebensdauer und Zyklen werden festgelegt.

Sie können die Simulation jederzeit stoppen. Wenn Sie auf **Simulation starten** klicken, wird die Simulation aber ab dem Anfang des Datasets wiedergegeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mehr wissen möchten, lassen Sie den Solution Accelerator für Predictive Maintenance weiterhin bereitgestellt.

Falls Sie den Solution Accelerator nicht mehr benötigen, können Sie ihn auf der Seite [Bereitgestellte Lösungen](https://www.azureiotsolutions.com/Accelerators#dashboard) löschen:

![Löschen der Lösung](media/quickstart-predictive-maintenance-deploy/deletesolution.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie den Solution Accelerator für Predictive Maintenance bereitgestellt und eine Simulation ausgeführt.

Weitere Informationen zum Solution Accelerator finden Sie im folgenden Artikel.

> [!div class="nextstepaction"]
> [Übersicht über den Solution Accelerator für Predictive Maintenance](iot-accelerators-predictive-walkthrough.md)
