---
title: Bereitstellen von Azure IoT Edge-Modulen (Portal) | Microsoft-Dokumentation
description: Bereitstellen von Modulen auf einem IoT Edge-Gerät über das Azure-Portal
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 83f199c49209210ec577017534f93e36d05bd70a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620364"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Bereitstellen von Azure IoT Edge-Modulen über das Azure-Portal

Nachdem Sie IoT Edge-Module mit Ihrer Geschäftslogik erstellen, sollten Sie sie auf Ihren Geräten für den Betrieb im Edge-Bereich bereitstellen. Wenn bei Ihnen mehrere Module gemeinsam Daten erfassen und verarbeiten, können Sie alle auf einmal bereitstellen und die Routingregeln, mit denen sie verbunden werden, deklarieren. 

In diesem Artikel wird gezeigt, wie das Azure-Portal Sie durch das Erstellen eines Bereitstellungsmanifests und die Übertragung der Bereitstellung auf einem IoT Edge-Gerät führt. Informationen zum Erstellen einer Bereitstellung, die basierend auf freigegebenen Tags auf mehrere Geräte ausgerichtet ist, finden Sie unter [Bereitstellen und Überwachen von IoT Edge-Modulen im großen Maßstab](how-to-deploy-monitor.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement. 
* Ein [IoT Edge-Gerät](how-to-register-device-portal.md) mit installierter IoT Edge-Runtime. 

## <a name="select-your-device"></a>Auswählen Ihres Geräts

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.
2. Wählen Sie im Menü **IoT Edge** aus.
3. Klicken Sie in der Liste der Geräte auf die ID des Zielgeräts. 
4. Wählen Sie **Module festlegen** aus.

## <a name="configure-a-deployment-manifest"></a>Konfigurieren eines Bereitstellungsmanifests

Ein Bereitstellungsmanifest ist ein JSON-Dokument, das beschreibt, welche Module bereitgestellt werden sollen, wie Daten zwischen den Modulen übermittelt werden und welche Eigenschaften die Modulzwillinge aufweisen sollen. Weitere Informationen zur Funktionsweise und Erstellung von Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können](module-composition.md).

Das Azure-Portal verfügt über einen Assistenten, der Sie durch das Erstellen des Bereitstellungsmanifests führt, sodass Sie das JSON-Dokument nicht manuell erstellen müssen. Es umfasst drei Schritte: **Hinzufügen von Modulen**, **Angeben von Routen** und **Überprüfen der Bereitstellung**. 

### <a name="add-modules"></a>Hinzufügen von Modulen

1. Geben Sie im Abschnitt **Registrierungseinstellungen** der Seite die Anmeldeinformationen für den Zugriff auf private Containerregistrierungen mit Ihren Modulimages an. 
2. Wählen Sie im Abschnitt **Bereitstellungsmodule** der Seite die Option **Hinzufügen** aus. 
3. Sehen Sie sich die Modultypen in der Dropdownliste an: 
   * **IoT Edge-Modul:** Standardoption.
   * **Azure Stream Analytics-Modul:** nur Module, die aus einer Azure Stream Analytics-Workload generiert wurden. 
4. Wählen Sie das **IoT Edge-Modul** aus.
5. Geben Sie einen Namen für das Modul und dann das Containerimage an. Beispiel:  
   * **Name:** tempSensor
   * **Bild-URI:** mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
6. Füllen Sie bei Bedarf die optionalen Felder aus. Weitere Informationen zu Containererstellungsoptionen, Neustartrichtlinien und gewünschtem Status finden Sie unter [Gewünschte EdgeAgent-Eigenschaften](module-edgeagent-edgehub.md#edgeagent-desired-properties). Weitere Informationen zum Modulzwilling finden Sie unter [Definieren oder Aktualisieren gewünschter Eigenschaften](module-composition.md#define-or-update-desired-properties).
7. Wählen Sie **Speichern**aus.
8. Wiederholen Sie die Schritte 2 bis 6, um Ihrer Bereitstellung weitere Module hinzuzufügen. 
9. Klicken Sie auf **Weiter**, um mit dem Abschnitt über Routen fortzufahren.

### <a name="specify-routes"></a>Angeben von Routen

Standardmäßig erhalten Sie vom Assistenten eine Route mit dem Namen **route**, die als **FROM /\* INTO $upstream** definiert ist. Dies bedeutet, dass alle Nachrichten, die von Modulen ausgegeben werden, an Ihren IoT Hub gesendet werden.  

Fügen Sie die Routen mit Informationen aus dem Abschnitt [Deklarieren von Routen](module-composition.md#declare-routes) hinzu, oder aktualisieren Sie diese, und wählen Sie dann **Weiter** aus, um mit dem Abschnitt zur Überprüfung fortzufahren.

### <a name="review-deployment"></a>Überprüfen der Bereitstellung

Im Abschnitt zur Überprüfung wird das JSON-Bereitstellungsmanifest angezeigt, das anhand Ihrer Auswahl in den vorherigen beiden Abschnitten erstellt wurde. Beachten Sie, dass zwei Module deklariert wurden, die Sie nicht hinzugefügt haben: **$edgeAgent** und **$edgeHub**. Diese beiden Module bilden die [IoT Edge-Runtime](iot-edge-runtime.md) und sind in jeder Bereitstellung erforderliche Standardvorgaben. 

Überprüfen Sie die Bereitstellungsinformationen, und klicken Sie dann auf **Senden**. 

## <a name="view-modules-on-your-device"></a>Anzeigen von Modulen auf dem Gerät

Nachdem Sie die Module auf Ihrem Gerät bereitgestellt haben, können Sie sie auf der Seite **Gerätedetails** des Portals anzeigen. Auf dieser Seite werden die Namen der einzelnen bereitgestellten Modul sowie nützliche Informationen wie der Bereitstellungsstatus und der Exitcode angezeigt. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [IoT Edge-Module im großen Maßstab bereitstellen und überwachen](how-to-deploy-monitor.md).
