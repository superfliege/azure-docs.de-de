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
ms.openlocfilehash: 1d057a4df43cf25e6817672d198207d9a50e462e
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Aufheben der Bereitstellung von Geräten, die durch Ihren Bereitstellungsdienst registriert wurden

Manchmal muss unter Umständen die Bereitstellung von Geräten aufgehoben werden, die über den Device Provisioning-Dienst bereitgestellt wurden. Dies kann beispielsweise erforderlich sein, wenn ein Gerät verkauft oder in eine andere IoT Hub-Instanz verschoben wird oder wenn es verloren gegangen ist, gestohlen wurde oder anderweitig gefährdet ist. 

Die Aufhebung der Bereitstellung eines Geräts umfasst im Allgemeinen zwei Schritte:

1. Widerrufen des Zugriffs auf Ihren Bereitstellungsdienst für das Gerät. Je nachdem, ob Sie den Zugriff vorübergehend oder dauerhaft widerrufen möchten (oder im Falle des X.509-Nachweismechanismus auf der Grundlage der Hierarchie Ihrer vorhandenen Registrierungsgruppen), können Sie einen Registrierungseintrag entweder deaktivieren oder löschen. 
 
   - Unter [Vorgehensweise zum Widerrufen des Gerätezugriffs auf Ihren Bereitstellungsdienst im Azure-Portal](how-to-revoke-device-access-portal.md) erfahren Sie, wie Sie den Gerätezugriff über das Portal widerrufen.
   - Unter [Gewusst wie: Verwalten von Geräteregistrierungen mit Azure Device Provisioning Service-SDKs](how-to-manage-enrollments-sdks.md) erfahren Sie, wie Sie den Gerätezugriff programmgesteuert mithilfe eines der Bereitstellungsdienst-SDKs widerrufen.

2. Deaktivieren oder Löschen des Eintrags für das Gerät in der Identitätsregistrierung für die IoT Hub-Instanz, in der es bereitgestellt wurde. Weitere Informationen finden Sie in der Dokumentation zu Azure IoT Hub unter [Verwalten von Geräteidentitäten](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices). 

Welche Schritte Sie genau zum Aufheben der Bereitstellung eines Geräts ausführen, hängt von dessen Nachweismechanismus und dem zugehörigen Registrierungseintrag bei Ihrem Bereitstellungsdienst ab.

## <a name="individual-enrollments"></a>Individuelle Registrierungen
Geräte mit TPM-Nachweis oder X.509-Nachweis und einem untergeordneten Zertifikat werden über einen individuellen Registrierungseintrag bereitgestellt. 

So heben Sie die Bereitstellung eines Geräts auf, das über eine individuelle Registrierung verfügt: 
1. Bei Verwendung von Geräten mit TPM-Nachweis löschen Sie den individuellen Registrierungseintrag, um dauerhaft die Zugriffsberechtigung des Geräts auf den Bereitstellungsdienst zu widerrufen. Alternativ können Sie den Eintrag auch deaktivieren, um dessen Zugriffsberechtigung nur vorübergehend zu widerrufen. Für Geräte mit X.509-Nachweis können Sie den Eintrag entweder löschen oder deaktivieren. Denken Sie jedoch daran, dass ein Gerät mit X.509-Nachweis nicht erneut registriert werden kann, wenn Sie eine individuelle Registrierung für das Gerät löschen und eine aktivierte Registrierungsgruppe für ein Signaturzertifikat in der Zertifikatskette des Geräts vorhanden ist. Für solche Geräte kann es sicherer sein, den Registrierungseintrag zu deaktivieren. Dadurch wird eine erneute Registrierung des Geräts verhindert, unabhängig davon, ob eine aktivierte Registrierungsgruppe für eines der Signaturzertifikate vorhanden ist.
2. Deaktivieren oder löschen Sie das Gerät in der Identitätsregistrierung für die IoT Hub-Instanz, in der es bereitgestellt wurde. 


## <a name="enrollment-groups"></a>Registrierungsgruppen
Bei Verwendung des X.509-Nachweises können Geräte auch über eine Registrierungsgruppe bereitgestellt werden. Registrierungsgruppen werden mit einem Signaturzertifikat (Zwischenzertifikat oder Zertifikat der Stammzertifizierungsstelle) konfiguriert und steuern den Zugriff auf den Bereitstellungsdienst für Geräte mit diesem Zertifikat in ihrer Zertifikatkette. Weitere Informationen zu Registrierungsgruppen und X.509-Zertifikaten mit dem Bereitstellungsdienst finden Sie unter [X.509-Zertifikate](concepts-security.md#x509-certificates). 

Eine Liste mit Geräten, die über eine Registrierungsgruppe bereitgestellt wurden, finden Sie in den Details der Registrierungsgruppe. So können Sie ganz einfach nachvollziehen, für welche IoT Hub-Instanz die einzelnen Geräte bereitgestellt wurden. So zeigen Sie die Geräteliste an: 

1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
2. Klicken Sie in der Ressourcenliste auf Ihren Bereitstellungsdienst.
3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Registrierungsgruppen**.
4. Klicken Sie auf die Registrierungsgruppe, um sie zu öffnen.

   ![Anzeigen des Registrierungsgruppeneintrags über das Portal](./media/how-to-unprovision-devices/view-enrollment-group.png)

Mit Registrierungsgruppen sind zwei Szenarien denkbar:

- So heben Sie die Bereitstellung aller Geräte auf, die über eine Registrierungsgruppe bereitgestellt wurden:
  1. Deaktivieren Sie die Registrierungsgruppe, um dessen Signaturzertifikat auf die Blacklist zu setzen. 
  2. Verwenden Sie die Liste bereitgestellter Geräte für diese Registrierungsgruppe, um jedes Gerät aus der Identitätsregistrierung der entsprechenden IoT Hub-Instanz zu deaktivieren oder zu löschen. 
  3. Nach dem Deaktivieren oder Löschen aller Geräte aus den entsprechenden IoT Hub-Instanzen können Sie optional die Registrierungsgruppe löschen. Hinweis: Wenn Sie die Registrierungsgruppe löschen und für ein übergeordnetes Signaturzertifikat in der Zertifikatkette mindestens eines der Geräte eine aktivierte Registrierungsgruppe vorhanden ist, können sich die entsprechenden Geräte erneut registrieren. 
- So heben Sie die Bereitstellung eines einzelnen Geräts aus einer Registrierungsgruppe auf:
  1. Erstellen Sie eine deaktivierte individuelle Registrierung für das untergeordnete Zertifikat (des Geräts). Dadurch wird er Zugriff auf den Bereitstellungsdienst für dieses Gerät aufgehoben. Anderen Geräten, deren Zertifikatkette das Signaturzertifikat der Registrierungsgruppe enthält, wird dagegen weiterhin Zugriff gewährt. Löschen Sie die deaktivierte individuelle Registrierung für das Gerät nicht. Andernfalls kann sich das Gerät über die Registrierungsgruppe erneut registrieren. 
  2. Ermitteln Sie anhand der Liste bereitgestellter Geräte für die Registrierungsgruppe die IoT Hub-Instanz, für die das Gerät bereitgestellt wurde. Anschließend deaktivieren Sie das Gerät oder löschen es aus der Identitätsregistrierung des Hubs. 
  
  










