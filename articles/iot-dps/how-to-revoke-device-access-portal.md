---
title: "Vorgehensweise zum Verwalten des Gerätezugriffs für den Azure IoT Hub Device Provisioning-Dienst | Microsoft-Dokumentation"
description: "Vorgehensweise zum Widerrufen des Gerätezugriffs auf Ihren DP-Dienst im Azure-Portal"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 12/22/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6f1dad648b228163219c8f722eed3897f4ba4d22
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="how-to-revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Vorgehensweise zum Widerrufen des Gerätezugriffs auf Ihren Bereitstellungsdienst im Azure-Portal

Für bedeutende Systeme wie IoT-Lösungen ist eine ordnungsgemäße Verwaltung der Geräteanmeldeinformationen von entscheidender Bedeutung. Für derartige Systeme hat es sich bewährt, einen klaren Plan für das Widerrufen des Gerätezugriffs zu besitzen, für den Fall, dass die Anmeldeinformationen – beispielsweise ein SAS-Token oder ein X.509-Zertifikat – gefährdet sind. In diesem Artikel wird beschrieben, wie beim Bereitstellungsschritt der Gerätezugriff widerrufen wird.

Weitere Informationen zum Widerrufen des Gerätezugriffs auf einen IoT Hub nach der Gerätebereitstellung finden Sie unter [Deaktivieren von Geräten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Achten Sie auf die Wiederholungsrichtlinie von Geräten, für die Sie den Zugriff widerrufen. Denn ein Gerät mit einer unbegrenzten Wiederholungsrichtlinie kann beispielsweise fortlaufend versuchen, sich beim Bereitstellungsdienst zu registrieren, wodurch Dienstressourcen verbraucht werden und die Leistung möglicherweise beeinträchtigt wird.

## <a name="blacklist-devices-with-an-individual-enrollment-entry"></a>Hinzufügen von Geräten mit einem Eintrag für die individuelle Registrierung zur Blacklist

Individuelle Registrierungen werden auf ein einzelnes Gerät angewendet und können X.509-Zertifikate oder SAS-Token (in einem physischen oder virtuellen TPM) als Nachweismechanismus einsetzen. (Geräte, die SAS-Token als Nachweismechanismus verwenden, können nur über eine individuelle Registrierung bereitgestellt werden.) Um ein Gerät mit einer individuellen Registrierung zur Blacklist hinzufügen, können Sie den jeweiligen Registrierungseintrag entweder deaktivieren oder löschen: 

- Um das Gerät vorübergehend zur Blacklist hinzuzufügen, können Sie den Registrierungseintrag deaktivieren. 

    1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
    2. Klicken Sie auf den Bereitstellungsdienst, für den Sie Ihr Gerät aus der Liste der Ressourcen zur Blacklist hinzufügen möchten.
    3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Individuelle Registrierungen**.
    4. Um einen Registrierungseintrag zu öffnen, klicken Sie auf den Eintrag für das Gerät, das zur Blacklist hinzugefügt werden soll. 
    5. Klicken Sie im unteren Bereich der Liste der Registrierungseinträge auf **Deaktivieren** und dann auf **Speichern**.  

        ![Deaktivieren eines Eintrags für eine individuelle Registrierung im Portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
- Um das Gerät dauerhaft zur Blacklist hinzuzufügen, können Sie den jeweiligen Registrierungseintrag deaktivieren.

    1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
    2. Klicken Sie auf den Bereitstellungsdienst, für den Sie Ihr Gerät aus der Liste der Ressourcen zur Blacklist hinzufügen möchten.
    3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Individuelle Registrierungen**.
    4. Aktivieren Sie das Kontrollkästchen neben dem Registrierungseintrag für das Gerät, das zur Blacklist hinzugefügt werden soll. 
    5. Klicken Sie im oberen Bereich des Fensters auf **Löschen** und anschließend auf **Ja**, um zu bestätigen, dass die Registrierung entfernt werden soll. 

        ![Löschen eines Eintrags für eine individuelle Registrierung im Portal](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
    6. Sobald die Aktion abgeschlossen ist, sehen Sie, dass Ihr Eintrag aus der Liste der individuellen Registrierungen entfernt wurde.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-using-an-enrollment-group"></a>Hinzufügen eines X.509-Zertifikats der Zwischen- oder Stammzertifizierungsstelle mithilfe einer Registrierungsgruppe

X.509-Zertifikate werden in der Regel in einer Zertifikatkette angeordnet. Wenn ein Zertifikat an irgendeiner Stelle in einer Kette gefährdet wird, besteht keine Vertrauensstellung mehr. Das Zertifikat muss dann zur Blacklist hinzugefügt werden, um zu verhindern, dass nachgeschaltete Geräte in einer Kette mit diesem Zertifikat vom Device Provisioning-Dienst bereitgestellt werden. Weitere Informationen zu X.509-Zertifikaten und ihrer Verwendung mit dem Bereitstellungsdienst finden Sie unter [X.509-Zertifikate](./concepts-security.md#x509-certificates). 

Eine Registrierungsgruppe ist ein Eintrag für Geräte mit einem gemeinsamen Nachweismechanismus von X.509-Zertifikaten, signiert von der gleichen Zwischen- oder Stammzertifizierungsstelle. Der Eintrag für die Registrierungsgruppe ist mit dem X.509-Zertifikat, das der Zwischen- oder Stammzertifizierungsstelle zugeordnet ist, sowie den Konfigurationswerten konfiguriert (z.B. Verbindungen zwischen Gerätezwillingen und IoT Hubs, die von Geräten mit diesem Zertifikat in der Zertifikatkette gemeinsam genutzt werden). Um das Zertifikat zur Blacklist hinzufügen, können Sie eine Registrierungsgruppe entweder deaktivieren oder löschen:

- Um das Zertifikat vorübergehend zur Blacklist hinzuzufügen, können Sie die Registrierungsgruppe deaktivieren. 

    1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
    2. Klicken Sie auf den Bereitstellungsdienst, für den Sie das Signaturzertifikat aus der Liste der Ressourcen zur Blacklist hinzufügen möchten.
    3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Registrierungsgruppen**.
    4. Um eine Registrierungsgruppe zu öffnen, klicken Sie auf die Gruppe für das Zertifikat, das zur Blacklist hinzugefügt werden soll.
    5. Klicken Sie oben links im Eintrag der Registrierungsgruppe auf **Gruppe bearbeiten**.
    6. Um den Registrierungseintrag zu deaktivieren, klicken Sie auf dem Umschalter **Eintrag aktivieren** auf **Deaktivieren** und dann auf **Speichern**.  

        ![Deaktivieren des Eintrags für die Registrierungsgruppe im Portal](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
- Um das Zertifikat dauerhaft zur Blacklist hinzuzufügen, können Sie die jeweilige Registrierungsgruppe löschen.

    1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
    2. Klicken Sie auf den Bereitstellungsdienst, für den Sie Ihr Gerät aus der Liste der Ressourcen zur Blacklist hinzufügen möchten.
    3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Registrierungsgruppen**.
    4. Aktivieren Sie das Kontrollkästchen neben der Registrierungsgruppe für das Zertifikat, das zur Blacklist hinzugefügt werden soll. 
    5. Klicken Sie im oberen Bereich des Fensters auf **Löschen** und anschließend auf **Ja**, um zu bestätigen, dass die Registrierungsgruppe entfernt werden soll. 

        ![Löschen des Eintrags für die Registrierungsgruppe im Portal](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

    6. Sobald die Aktion abgeschlossen ist, sehen Sie, dass Ihr Eintrag aus der Liste der Registrierungsgruppen entfernt wurde.  

> [!NOTE]
> Wenn Sie eine Registrierungsgruppe für ein Zertifikat löschen, können Geräte, die dieses Zertifikat in der Zertifikatkette enthalten, weiterhin registriert werden – vorausgesetzt eine aktivierte Registrierungsgruppe für das Stammzertifikat oder ein anderes Zwischenzertifikat ist weiter oben in der Zertifikatkette vorhanden.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Hinzufügen bestimmter Geräte in einer Registrierungsgruppe zur Blacklist

Geräte, die den X.509-Nachweismechanismus implementieren, werden anhand der Zertifikatkette und des privaten Schlüssels des Geräts authentifiziert. Wenn ein Gerät eine Verbindung mit dem Device Provisioning-Dienst herstellt und bei diesem authentifiziert wird, sucht der Dienst vor der Suche nach Registrierungsgruppen zuerst nach einer individuellen Registrierung, die den Anmeldeinformationen des Geräts entspricht, um zu ermitteln, ob das Gerät bereitgestellt werden kann. Wenn der Dienst eine deaktivierte individuelle Registrierung für das Gerät findet, verhindert es, dass das Gerät eine Verbindung herstellt, selbst wenn eine Registrierungsgruppe für eine Zwischen- oder Stammzertifizierungsstelle in der Zertifikatkette des Geräts aktiviert ist. Um ein einzelnes Gerät in einer Registrierungsgruppe zur Blacklist hinzuzufügen, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim Azure-Portal an, und klicken Sie im linken Menü auf **Alle Ressourcen**.
2. Klicken Sie in der Liste der Ressourcen auf den Bereitstellungsdienst, der die Registrierungsgruppe für das zur Blacklist hinzuzufügende Gerät enthält.
3. Klicken Sie in Ihrem Bereitstellungsdienst auf **Registrierungen verwalten**, und wählen Sie dann die Registerkarte **Individuelle Registrierungen**.
4. Klicken Sie ganz oben auf die Schaltfläche **Hinzufügen**. 
5. Wählen Sie **X.509** als Sicherheitsmechanismus für das Gerät aus, und laden Sie das Gerätezertifikat hoch. Dies ist das signierte Endeinheitszertifikat, das auf dem Gerät installiert ist und zum Generieren von Zertifikaten für die Authentifizierung verwendet wird.
6. Geben Sie die **IoT Hub-Geräte-ID** für das Gerät ein. 
7. Um den Registrierungseintrag zu deaktivieren, legen Sie den Umschalter **Eintrag aktivieren** auf **Deaktivieren** fest. 
8. Klicken Sie auf **Speichern**. Nach erfolgreicher Erstellung Ihrer Registrierung sollte Ihr Gerät auf der Registerkarte **Individuelle Registrierungen** angezeigt werden. 

    ![Deaktivieren eines Eintrags für eine individuelle Registrierung im Portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)




