---
title: Bereitstellen eines Azure Machine Learning-Modells auf einem Azure IoT Edge-Gerät | Microsoft-Dokumentation
description: Dieses Dokument beschreibt, wie Azure Machine Learning-Modelle für Azure IoT Edge-Geräte bereitgestellt werden können.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: 7322b07740d5dec85b6217e122fb262647527c96
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258407"
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Bereitstellen eines Azure Machine Learning-Modells auf einem Azure IoT Edge-Gerät

Azure Machine Learning-Modelle können als Docker-basierte Webdiensts in Container gepackt werden. Azure IoT Edge ermöglicht Ihnen die Remotebereitstellung von Containern auf Geräten. Verwenden Sie diese Dienste gemeinsam, um Ihre Modelle im Edge-Bereich auszuführen und schnellere Antwortzeiten zu erzielen sowie die Datenübertragung zu reduzieren. 

Zusätzliche Skripts und Anweisungen finden Sie im [AI-Toolkit für Azure IoT Edge](https://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Operationalisieren des Modells

Azure IoT Edge-Module basieren auf Container-Images. Zum Bereitstellen Ihres Machine Learning-Modells auf einem IoT Edge-Gerät müssen Sie ein Docker-Image erstellen.

Operationalisieren Sie Ihr Modell entsprechend der Anleitung in [Bereitstellen eines Machine Learning-Modells als Webdienst](model-management-service-deploy.md), um ein Docker-Image mit Ihrem Modell zu erstellen.

## <a name="deploy-to-azure-iot-edge"></a>Bereitstellen für Azure IoT Edge

Sobald Sie das Image des Modells haben, können Sie es für jedes Azure IoT Edge-Gerät bereitstellen. Alle Machine Learning-Modelle können auf IoT Edge-Geräten ausgeführt werden. 

### <a name="set-up-an-iot-edge-device"></a>Einrichten eines IoT Edge-Geräts

Bereiten Sie ein Gerät mithilfe der Azure IoT Edge-Dokumentation vor. 

1. [Registrieren Sie ein Gerät bei Azure IoT Hub](../../iot-edge/how-to-register-device-portal.md). Die Ausgabe dieser Prozesse ist eine Verbindungszeichenfolge, mit der Sie Ihr physisches Gerät konfigurieren können. 
2. Installieren Sie die IoT Edge-Runtime auf Ihrem physischen Gerät, und konfigurieren Sie es mit einer Verbindungszeichenfolge. Sie können die Runtime auf [Windows](../../iot-edge/how-to-install-iot-edge-windows-with-windows.md)- oder [Linux](../../iot-edge/how-to-install-iot-edge-linux.md)-Geräten installieren.  


### <a name="find-the-machine-learning-container-image-location"></a>Navigieren zum Speicherort des Machine Learning-Containerimage
Sie benötigen den Speicherort Ihres Machine Learning-Containerimage. So finden Sie den Speicherort des Containerimage:

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com/) an.
2. Wählen Sie in **Azure Container Registry** die Registrierung aus, die Sie untersuchen möchten.
3. Klicken Sie unter „Registrierung“ auf **Repositorys**, um eine Liste aller Repositorys und Images anzuzeigen.

Während Sie Ihre Containerregistrierung im Azure-Portal anzeigen, rufen Sie die Anmeldeinformationen für die Containerregistrierung ab. Diese Anmeldeinformationen müssen dem IoT Edge-Gerät bereitgestellt werden, damit es das Image aus Ihrer privaten Registrierung abrufen kann. 

1. Klicken Sie in der Containerregistrierung auf **Zugriffsschlüssel**. 
2. **Aktivieren** Sie den Administratorbenutzer, falls er noch nicht aktiviert ist. 
3. Speichern Sie die Werte für **Anmeldeserver**, **Benutzername** und **Kennwort**. 

### <a name="deploy-the-container-image-to-your-device"></a>Bereitstellen des Containerimages auf Ihrem Gerät

Mit dem Containerimage und den Anmeldeinformationen für die Containerregistrierung sind Sie in der Lage, das Machine Learning-Modell auf Ihrem IoT Edge-Gerät bereitzustellen. 

Befolgen Sie die Anweisungen in [Bereitstellen von Azure IoT Edge-Modulen über das Azure-Portal](../../iot-edge/how-to-deploy-modules-portal.md) um Ihr Modell auf Ihrem IoT Edge-Gerät zu starten. 











