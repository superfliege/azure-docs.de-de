---
title: "Aufheben der Bereitstellung von Geräten, die mit dem Azure IoT Hub Device Provisioning-Dienst registriert wurden | Microsoft-Dokumentation"
description: "Anleitung zum Aufheben der Bereitstellung von Geräten, die per DPS im Azure-Portal registriert wurden"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 01/08/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 83ed72c0f2eb342c372ef97e5443d60eab1e2174
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Aufheben der Bereitstellung von Geräten, die durch Ihren Bereitstellungsdienst registriert wurden

Manchmal muss unter Umständen die Bereitstellung von Geräten aufgehoben werden, die über den Device Provisioning-Dienst bereitgestellt wurden. Dies kann beispielsweise erforderlich sein, wenn ein Gerät verkauft oder in eine andere IoT Hub-Instanz verschoben wird oder wenn es verloren gegangen ist, gestohlen wurde oder anderweitig gefährdet ist. 

Die Aufhebung der Bereitstellung eines Geräts umfasst im Allgemeinen zwei Schritte:

1. Widerrufen des Zugriffs auf Ihren Bereitstellungsdienst für das Gerät. Je nachdem, ob Sie den Zugriff vorübergehend oder dauerhaft widerrufen möchten (oder im Falle des X.509-Nachweismechanismus auf der Grundlage der Hierarchie Ihrer vorhandenen Registrierungsgruppen), können Sie einen Registrierungseintrag entweder deaktivieren oder löschen. 
 
   - Unter [Vorgehensweise zum Widerrufen des Gerätezugriffs auf Ihren Bereitstellungsdienst im Azure-Portal](how-to-revoke-device-access-portal.md) erfahren Sie, wie Sie den Gerätezugriff über das Portal widerrufen.
   - Unter [Gewusst wie: Verwalten von Geräteregistrierungen mit Azure Device Provisioning Service-SDKs](how-to-manage-enrollments-sdks.md) erfahren Sie, wie Sie den Gerätezugriff programmgesteuert mithilfe eines der Bereitstellungsdienst-SDKs widerrufen.

2. Deaktivieren oder Löschen des Eintrags für das Gerät in der Identitätsregistrierung für die IoT Hub-Instanz, in der es bereitgestellt wurde. Weitere Informationen finden Sie in der Dokumentation zu Azure IoT Hub unter [Verwalten von Geräteidentitäten](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices). 

Die exakten Schritte zum Aufheben der Bereitstellung eines Geräts hängen vom verwendeten Nachweismechanismus ab.

Geräte mit TPM-Nachweis oder X.509-Nachweis und einem selbstsignierten untergeordneten Zertifikat (keine Zertifikatkette) werden über einen individuellen Registrierungseintrag bereitgestellt. Für diese Geräte können Sie den Eintrag löschen, um den Bereitstellungsdienstzugriff des Geräts dauerhaft zu widerrufen, oder den Eintrag deaktivieren, um den Zugriff vorübergehend zu widerrufen, und das Gerät anschließend in der Identitätsregistrierung der IoT Hub-Instanz, für die es bereitgestellt war, entweder löschen oder deaktivieren.

Bei Verwendung des X.509-Nachweises können Geräte auch über eine Registrierungsgruppe bereitgestellt werden. Registrierungsgruppen werden mit einem Signaturzertifikat (Zwischenzertifikat oder Zertifikat der Stammzertifizierungsstelle) konfiguriert und steuern den Zugriff auf den Bereitstellungsdienst für Geräte mit diesem Zertifikat in ihrer Zertifikatkette. Weitere Informationen zu Registrierungsgruppen und X.509-Zertifikaten mit dem Bereitstellungsdienst finden Sie unter [X.509-Zertifikate](concepts-security.md#x509-certificates). 

Eine Liste mit Geräten, die über eine Registrierungsgruppe bereitgestellt wurden, finden Sie in den Details der Registrierungsgruppe. So können Sie ganz einfach nachvollziehen, für welche IoT Hub-Instanz die einzelnen Geräte bereitgestellt wurden. So zeigen Sie die Geräteliste an: 

1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
2. Klicken Sie in der Ressourcenliste auf Ihren Bereitstellungsdienst.
3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Registrierungsgruppen**.
4. Klicken Sie auf die Registrierungsgruppe, um sie zu öffnen.

   ![Anzeigen des Registrierungsgruppeneintrags über das Portal](./media/how-to-unprovision-devices/view-enrollment-group.png)

Mit Registrierungsgruppen sind zwei Szenarien denkbar:

- Wenn Sie die Bereitstellung aller Geräte aufheben möchten, die über eine Registrierungsgruppe bereitgestellt wurden, müssen Sie zuerst die Registrierungsgruppe deaktivieren, um deren Signaturzertifikat der Blacklist hinzuzufügen. Anschließend können Sie mithilfe der Liste bereitgestellter Geräte für diese Registrierungsgruppe jedes Gerät aus der Identitätsregistrierung der entsprechenden IoT Hub-Instanz deaktivieren oder löschen. Nach dem Deaktivieren oder Löschen aller Geräte aus den entsprechenden IoT Hub-Instanzen können Sie optional die Registrierungsgruppe löschen. Hinweis: Wenn Sie die Registrierungsgruppe löschen und für ein übergeordnetes Signaturzertifikat in der Zertifikatkette mindestens eines der Geräte eine aktivierte Registrierungsgruppe vorhanden ist, können sich die entsprechenden Geräte erneut registrieren. 
- Wenn Sie die Bereitstellung eines einzelnen Geräts aus einer Registrierungsgruppe aufheben möchten, müssen Sie zuerst eine deaktivierte individuelle Registrierung für das dazugehörige ungeordnete (gerätespezifische) Zertifikat erstellen. Dadurch wird er Zugriff auf den Bereitstellungsdienst für dieses Gerät aufgehoben. Anderen Geräten, deren Zertifikatkette das Signaturzertifikat der Registrierungsgruppe enthält, wird dagegen weiterhin Zugriff gewährt. Anschließend können Sie in den Details der Registrierungsgruppe anhand der Liste bereitgestellter Geräte die IoT Hub-Instanz ermitteln, für die das Gerät bereitgestellt wurde, und es deaktivieren oder aus der Identitätsregistrierung des Hubs löschen. Löschen Sie die deaktivierte individuelle Registrierung für das Gerät nicht. Andernfalls kann sich das Gerät über die Registrierungsgruppe erneut registrieren. 










