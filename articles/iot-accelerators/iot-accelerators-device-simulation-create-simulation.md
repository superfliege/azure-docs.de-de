---
title: Erstellen einer neuen IoT-Simulation – Azure | Microsoft-Dokumentation
description: In diesem Tutorial verwenden Sie die Gerätesimulation, um eine neue Simulation zu erstellen und auszuführen.
author: troyhopwood
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/08/2019
ms.author: troyhop
ms.openlocfilehash: 09a6920e0d3a50da1bdacbf2bc7a80396c885897
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58180703"
---
# <a name="tutorial-create-and-run-an-iot-device-simulation"></a>Tutorial: Erstellen und Ausführen einer IoT-Gerätesimulation

In diesem Tutorial verwenden Sie die Gerätesimulation, um eine neue IoT-Simulation mit einem oder mehreren simulierten Geräten zu erstellen und auszuführen.

In diesem Tutorial führen Sie Folgendes durch:

>[!div class="checklist"]
> * Anzeigen aller aktiven und historischen Simulationen
> * Erstellen und Ausführen einer neuen Simulation
> * Anzeigen von Metriken für eine Simulation
> * Beenden einer Simulation

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine in Ihrem Azure-Abonnement bereitgestellte Instanz der Gerätesimulation.

Falls Sie die Gerätesimulation noch nicht bereitgestellt haben, absolvieren Sie zunächst die [Schnellstartanleitung zum Bereitstellen und Ausführen einer IoT-Gerätesimulation in Azure](quickstart-device-simulation-deploy.md) aus.

## <a name="open-device-simulation"></a>Öffnen der Gerätesimulation

Um die Gerätesimulation in Ihrem Browser auszuführen, navigieren Sie zunächst zu [Azure IoT solution accelerators](https://www.azureiotsolutions.com) (Solution Accelerators für Azure IoT). 

Sie werden unter Umständen aufgefordert, sich mit den Anmeldeinformationen Ihres Azure-Abonnements anzumelden.

Klicken Sie anschließend auf der Kachel für die Gerätesimulation, die Sie im Rahmen der [Schnellstartanleitung](quickstart-device-simulation-deploy.md) bereitgestellt haben, auf **Starten**.

## <a name="view-simulations"></a>Anzeigen von Simulationen

Wählen Sie auf der Menüleiste die Option **Simulationen** aus. Auf der Seite **Simulationen** werden Informationen zu allen verfügbaren Simulationen angezeigt. Auf dieser Seite sehen Sie Simulationen, die derzeit ausgeführt werden und in der Vergangenheit ausgeführt wurden. Wenn Sie eine bereits ausgeführte Simulation erneut ausführen möchten, klicken Sie auf die Simulationskachel, um die Simulationsdetails zu öffnen:

![Simulationen](media/iot-accelerators-device-simulation-create-simulation/dashboard.png)

## <a name="create-a-simulation"></a>Erstellen einer Simulation

Klicken Sie zum Erstellen einer Simulation in der rechten oberen Ecke auf **+ Neue Simulation**.

Eine Simulation beinhaltet mindestens ein Gerätemodell. Das Gerätemodell definiert das Verhalten, die Telemetrie und das Nachrichtenformat des zu simulierenden Geräts.

Klicken Sie zum Hinzufügen eines Gerätemodells auf **+ Add a device type** (+ Gerätetyp hinzufügen), und wählen Sie in der Liste das gewünschte Gerätemodell aus. Ihre Simulation kann mehrere Gerätemodelle enthalten. Jedes Gerätemodell kann über eine andere Geräteanzahl und über eine andere Nachrichtenhäufigkeit verfügen.

Wenn Sie das Formular für die neue Simulation fertig ausgefüllt haben, klicken Sie auf **Simulation starten**, um die Simulation zu starten.

Abhängig von der Anzahl der simulierten Geräte kann es etwas dauern, bis Ihre Simulation konfiguriert und gestartet wurde:

![Neue Simulation](media/iot-accelerators-device-simulation-create-simulation/newsimulation.png)

## <a name="stop-a-simulation"></a>Beenden einer Simulation

Wenn Sie auf **Simulation starten** klicken, wird die Seite mit den Simulationsdetails angezeigt. Diese Seite enthält aktuelle Simulationsstatistikdaten und Metriken von IoT Hub. Sie können auch auf der Seite **Simulationen** auf die Kachel der Simulation klicken, um zu dieser Detailseite zu gelangen.

Wenn Sie eine Simulation beenden möchten, klicken Sie rechts oben auf der Aktionsleiste auf **Simulation beenden**.

![Simulation beenden](media/iot-accelerators-device-simulation-create-simulation/simulationdetails.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine Simulation erstellen, ausführen und beenden. Außerdem haben Sie gelernt, wie Sie die Simulationsdetails anzeigen. Weitere Informationen zum Ausführen von Simulationen finden Sie im nächsten Tutorial:

> [!div class="nextstepaction"]
> [Erstellen eines benutzerdefinierten simulierten Geräts](iot-accelerators-device-simulation-create-custom-device.md)
