---
title: Bereitstellen eines Geräts mithilfe des Azure IoT Hub Device Provisioning-Diensts | Microsoft-Dokumentation
description: Bereitstellen Ihres Geräts für eine einzelne IoT Hub-Instanz mithilfe des Azure IoT Hub Device Provisioning-Diensts
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 03/28/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 4d98ce103bed7f9d14eb45422b70ceca1328afaa
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Bereitstellen des Geräts für eine IoT Hub-Instanz mithilfe des Azure IoT Hub Device Provisioning-Diensts

Im vorherigen Tutorial haben Sie erfahren, wie ein Gerät eingerichtet wird, um eine Verbindung mit Ihrem Device Provisioning-Dienst herzustellen. In diesem Tutorial erfahren Sie, wie Sie mithilfe dieses Diensts Ihr Gerät durch **_Registrierungslisten_** für eine einzelne IoT Hub-Instanz bereitstellen. Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Registrieren des Geräts
> * Starten des Geräts
> * Sicherstellen, dass das Gerät registriert ist

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie fortfahren, konfigurieren Sie auf jeden Fall Ihr Gerät wie in dem Tutorial [Einrichten eines bereitzustellenden Geräts mithilfe des Azure IoT Hub Device Provisioning-Diensts](./tutorial-set-up-device.md) erläutert.

Wenn Sie mit der automatischen Bereitstellung nicht vertraut sind, lesen Sie die Informationen unter [Konzepte für die automatische Bereitstellung](concepts-auto-provisioning.md), bevor Sie fortfahren.

<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Registrieren des Geräts

Bei diesem Schritt werden die einzigartigen Sicherheitsartefakte des Geräts zum Device Provisioning-Dienst hinzugefügt. Diese Sicherheitsartefakte basieren auf dem [Nachweismechanismus](concepts-device.md#attestation-mechanism) des Geräts:

- Für TPM-basierte Geräte benötigen Sie Folgendes:
    - Der *Endorsement Key* ist für jeden TPM-Chip bzw. jede Simulation eindeutig, und Sie erhalten ihn vom Hersteller des TPM-Chips.  Weitere Informationen finden Sie im Artikel [Grundlegendes zum TPM Endorsement Key](https://technet.microsoft.com/library/cc770443.aspx).
    - Die *Registrierungs-ID*, die zur eindeutigen Identifizierung eines Geräts im Namespace/Bereich verwendet wird. Diese ID ist eventuell mit der Geräte-ID identisch. Die ID ist für jedes Gerät erforderlich. Bei TPM-basierten Geräten kann die Registrierungs-ID vom TMP selbst abgeleitet werden, z.B. ein SHA-256-Hash des TPM Endorsement Key.

    ![Registrierungsinformationen für das TPM im Portal](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)

- Für X.509-basierte Geräte benötigen Sie Folgendes:
    - Das [für den X.509-Chip oder die X.509-Simulation ausgestellte Zertifikat](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx) in Form einer *PEM*- oder *CER*-Datei. Für die individuelle Registrierung müssen Sie für Ihr X.509-System das *Signaturzertifikat* auf Gerätebasis verwenden, für Registrierungsgruppen dagegen das *Stammzertifikat*. 

    ![Registrierungsinformationen für X.509 im Portal](./media/tutorial-provision-device-to-hub/x509-device-enrollment.png)

Es gibt zwei Möglichkeiten zum Registrieren des Geräts beim Device Provisioning-Dienst:

- **Registrierungsgruppen** Diese stellen Gruppen von Geräten dar, die einen bestimmten Nachweismechanismus gemeinsam nutzen. Es wird empfohlen, eine Registrierungsgruppe für eine große Anzahl von Geräten, die eine gewünschte Erstkonfiguration gemeinsam nutzen, oder für Geräte zu verwenden, die alle demselben Mandanten zugeordnet sind.

    ![Registrierungsgruppen für X.509 im Portal](./media/tutorial-provision-device-to-hub/x509-enrollment-groups.png)

- **Individuelle Registrierungen** Diese stellen Einträge für ein einzelnes Gerät dar, das beim Device Provisioning-Dienst registriert werden kann. Individuelle Registrierungen verwenden entweder X.509-Zertifikate oder SAS-Token (in einem physischen oder virtuellen TPM) als Nachweismechanismen. Individuelle Registrierungen sollten für Geräte, die besondere Erstkonfigurationen erfordern, und für Geräte verwendet werden, die nur SAS-Token über das TPM oder das virtuelle TPM als Nachweismechanismus verwenden können. Bei individuellen Registrierungen ist möglicherweise die gewünschte IoT Hub-Geräte-ID angegeben.

Nun registrieren Sie das Gerät mithilfe der erforderlichen Sicherheitsartefakte basierend auf dem Nachweismechanismus des Geräts bei der Device Provisioning-Dienstinstanz: 

1. Melden Sie sich beim Azure-Portal an, klicken Sie im Menü auf der linken Seite auf die Schaltfläche **Alle Ressourcen**, und öffnen Sie Ihren Device Provisioning-Dienst.

2. Wählen Sie auf dem Zusammenfassungsblatt des Device Provisioning-Diensts die Option **Registrierungen verwalten** aus. Wählen Sie je nach Ihrer Geräteinstallation entweder die Registerkarte **Individuelle Registrierungen** oder **Registrierungsgruppen**. Klicken Sie oben auf die Schaltfläche **Hinzufügen**. Wählen Sie **TPM** oder **X.509** als *Mechanismus* zum Identitätsnachweis aus, und geben Sie wie zuvor erläutert die entsprechenden Sicherheitsartefakte ein. Sie können eine neue **IoT Hub-Geräte-ID** eingeben. Klicken Sie abschließend auf die Schaltfläche **Speichern**. 

3. Wenn das Gerät erfolgreich registriert ist, sollte es wie folgt im Portal angezeigt werden:

    ![Erfolgreiche TPM-Registrierung im Portal](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)

Nach der Registrierung wartet der Bereitstellungsdienst, bis das Gerät gestartet und zu einem beliebigen Zeitpunkt eine Verbindung mit diesem hergestellt wird. Wenn Ihr Gerät zum ersten Mal gestartet wird, interagiert die Client-SDK-Bibliothek zur Extraktion der Sicherheitsartefakte vom Gerät mit Ihrem Chip und überprüft die Registrierung bei Ihrem Device Provisioning-Dienst. 

## <a name="start-the-device"></a>Starten des Geräts

An dieser Stelle ist Folgendes für die Geräteregistrierung eingerichtet:

1. Ihr Gerät oder eine Gruppe von Geräten ist bei Ihrem Device Provisioning-Dienst registriert, und 
2. Für Ihr Gerät wurde der Nachweismechanismus konfiguriert, und über die Anwendung kann mithilfe des Client-SDK des Device Provisioning-Diensts darauf zugegriffen werden.

Starten Sie das Gerät, damit Ihre Clientanwendung die Registrierung bei Ihrem Device Provisioning-Dienst starten kann.  

## <a name="verify-the-device-is-registered"></a>Sicherstellen, dass das Gerät registriert ist

Nachdem Ihr Gerät gestartet wurde, sollten folgende Aktionen durchgeführt werden. Weitere Einzelheiten finden Sie in der TPM-Simulatorbeispielanwendung [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c). 

1. Das Gerät sendet eine Registrierungsanforderung an Ihren Device Provisioning-Dienst.
2. Bei TPM-Geräten sendet der Device Provisioning-Dienst eine Registrierungsherausforderung zurück, auf die Ihr Gerät antwortet. 
3. Nach erfolgreicher Registrierung sendet der Device Provisioning-Dienst den IoT Hub-URI, die Geräte-ID und den verschlüsselten Schlüssel an das Gerät zurück. 
4. Die IoT Hub-Clientanwendung auf dem Gerät stellt dann eine Verbindung mit Ihrem Hub her. 
5. Nachdem erfolgreich eine Verbindung mit dem Hub hergestellt wurde, sollte das Gerät im **Device Explorer** von IoT Hub angezeigt werden. 

    ![Erfolgreiche Herstellung einer Verbindung mit dem Hub im Portal](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren des Geräts
> * Starten des Geräts
> * Sicherstellen, dass das Gerät registriert ist

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie mehrere Geräte für Hubs mit Lastausgleich bereitgestellt werden. 

> [!div class="nextstepaction"]
> [Bereitstellen von Geräten für IoT Hubs mit Lastausgleich](./tutorial-provision-multiple-hubs.md)
