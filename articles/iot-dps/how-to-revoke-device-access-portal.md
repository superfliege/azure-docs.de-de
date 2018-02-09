---
title: "Verwalten des Gerätezugriffs für den Azure IoT Hub Device Provisioning-Dienst | Microsoft-Dokumentation"
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
ms.openlocfilehash: 12aebf3a56aa7469a765ab6fc67aa65b254db71a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Widerrufen des Gerätezugriffs auf Ihren Bereitstellungsdienst im Azure-Portal

Für bedeutende Systeme wie IoT-Lösungen ist eine ordnungsgemäße Verwaltung der Geräteanmeldeinformationen von entscheidender Bedeutung. Für derartige Systeme hat es sich bewährt, einen klaren Plan für das Widerrufen des Gerätezugriffs zu besitzen, für den Fall, dass die Anmeldeinformationen – beispielsweise ein SAS-Token (Shared Access Signature) oder ein X.509-Zertifikat – gefährdet sind. In diesem Artikel wird beschrieben, wie beim Bereitstellungsschritt der Gerätezugriff widerrufen wird.

Weitere Informationen zum Widerrufen des Gerätezugriffs auf einen IoT Hub nach der Gerätebereitstellung finden Sie unter [Deaktivieren von Geräten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Achten Sie auf die Wiederholungsrichtlinie von Geräten, für die Sie den Zugriff widerrufen. Ein Gerät, das beispielsweise eine unbegrenzte Wiederholungsrichtlinie aufweist, führt kontinuierlich eine Registrierung beim Bereitstellungsdienst aus. Hierbei werden Dienstressourcen verbraucht, was sich möglicherweise auf die Leistung auswirkt.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Hinzufügen von Geräten durch einen Eintrag für die individuelle Registrierung zur Blacklist

Individuelle Registrierungen werden auf ein einzelnes Gerät angewendet und können X.509-Zertifikate oder SAS-Token (in einem physischen oder virtuellen TPM) als Nachweismechanismus einsetzen. (Geräte, die SAS-Token als Nachweismechanismus verwenden, können nur über eine individuelle Registrierung bereitgestellt werden.) Um ein Gerät mit einer individuellen Registrierung zur Blacklist hinzuzufügen, können Sie den jeweiligen Registrierungseintrag entweder deaktivieren oder löschen. 

So fügen Sie das Gerät durch Deaktivieren des Registrierungseintrags vorübergehend zur Blacklist hinzu: 

1. Melden Sie sich beim Azure-Portal an, und wählen Sie im linken Menü **Alle Ressourcen** aus.
2. Wählen Sie in der Liste der Ressourcen den Bereitstellungsdienst aus, für den Sie Ihr Gerät zur Blacklist hinzufügen möchten.
3. Wählen Sie in Ihrem Bereitstellungsdienst **Registrierungen verwalten** und dann die Registerkarte **Individuelle Registrierungen** aus.
4. Wählen Sie den Eintrag für das Gerät aus, das zur Blacklist hinzugefügt werden soll. 
5. Wählen Sie **Deaktivieren** für den Umschalter **Eintrag aktivieren** und dann **Speichern** aus.  

   ![Deaktivieren eines Eintrags für eine individuelle Registrierung im Portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
So fügen Sie das Gerät durch Löschen des jeweiligen Registrierungseintrags dauerhaft zur Blacklist hinzu

1. Melden Sie sich beim Azure-Portal an, und wählen Sie im linken Menü **Alle Ressourcen** aus.
2. Wählen Sie in der Liste der Ressourcen den Bereitstellungsdienst aus, für den Sie Ihr Gerät zur Blacklist hinzufügen möchten.
3. Wählen Sie in Ihrem Bereitstellungsdienst **Registrierungen verwalten** und dann die Registerkarte **Individuelle Registrierungen** aus.
4. Aktivieren Sie das Kontrollkästchen neben dem Registrierungseintrag für das Gerät, das zur Blacklist hinzugefügt werden soll. 
5. Wählen Sie im oberen Bereich des Fensters **Löschen** und anschließend **Ja** aus, um zu bestätigen, dass die Registrierung entfernt werden soll. 

   ![Löschen eines Eintrags für eine individuelle Registrierung im Portal](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
Nachdem Sie den Vorgang abgeschlossen haben, sollten Sie sehen, dass Ihr Eintrag aus der Liste der individuellen Registrierungen entfernt wurde.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Hinzufügen eines X.509-Zertifikats der Zwischen- oder Stammzertifizierungsstelle mithilfe einer Registrierungsgruppe

X.509-Zertifikate werden in der Regel in einer Zertifikatkette angeordnet. Wenn ein Zertifikat an irgendeiner Stelle in einer Kette gefährdet ist, besteht keine Vertrauensstellung. Das Zertifikat muss zur Blacklist hinzugefügt werden, um zu verhindern, dass nachfolgend in der jeweiligen Kette Geräte mit Zertifikaten vom Device Provisioning-Dienst bereitgestellt werden. Weitere Informationen zu X.509-Zertifikaten und ihrer Verwendung mit dem Bereitstellungsdienst finden Sie unter [X.509-Zertifikate](./concepts-security.md#x509-certificates). 

Eine Registrierungsgruppe ist ein Eintrag für Geräte mit einem gemeinsamen Nachweismechanismus von X.509-Zertifikaten, signiert von der gleichen Zwischen- oder Stammzertifizierungsstelle. Der Registrierungsgruppeneintrag ist mit dem X.509-Zertifikat konfiguriert, das einer Zwischen- oder Stammzertifizierungsstelle zugeordnet ist. Der Eintrag ist außerdem mit Konfigurationswerten konfiguriert (z.B. Verbindungen zwischen Gerätezwillingen und IoT Hubs, die von Geräten mit diesem Zertifikat in der Zertifikatkette gemeinsam genutzt werden). Um das Zertifikat zur Blacklist hinzuzufügen, können Sie eine Registrierungsgruppe entweder deaktivieren oder löschen.

So fügen Sie das Zertifikat durch Deaktivieren der Registrierungsgruppe vorübergehend zur Blacklist hinzu 

1. Melden Sie sich beim Azure-Portal an, und wählen Sie im linken Menü **Alle Ressourcen** aus.
2. Wählen Sie in der Liste der Ressourcen den Bereitstellungsdienst aus, für den Sie das Signaturzertifikat zur Blacklist hinzufügen möchten.
3. Wählen Sie in Ihrem Bereitstellungsdienst **Registrierungen verwalten** und dann die Registerkarte **Registrierungsgruppen** aus.
4. Wählen Sie die Registrierungsgruppe für das Zertifikat aus, das zur Blacklist hinzugefügt werden soll.
5. Wählen Sie im Registrierungsgruppeneintrag **Gruppe bearbeiten** aus.
6. Wählen Sie **Deaktivieren** für den Umschalter **Eintrag aktivieren** und dann **Speichern** aus.  

   ![Deaktivieren des Eintrags für die Registrierungsgruppe im Portal](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
So fügen Sie das Zertifikat durch Löschen der Registrierungsgruppe dauerhaft zur Blacklist hinzu

1. Melden Sie sich beim Azure-Portal an, und wählen Sie im linken Menü **Alle Ressourcen** aus.
2. Wählen Sie in der Liste der Ressourcen den Bereitstellungsdienst aus, für den Sie Ihr Gerät zur Blacklist hinzufügen möchten.
3. Wählen Sie in Ihrem Bereitstellungsdienst **Registrierungen verwalten** und dann die Registerkarte **Registrierungsgruppen** aus.
4. Aktivieren Sie das Kontrollkästchen neben der Registrierungsgruppe für das Zertifikat, das zur Blacklist hinzugefügt werden soll. 
5. Wählen Sie im oberen Bereich des Fensters **Löschen** und anschließend **Ja** aus, um zu bestätigen, dass die Registrierungsgruppe entfernt werden soll. 

   ![Löschen des Eintrags für die Registrierungsgruppe im Portal](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Nachdem Sie den Vorgang abgeschlossen haben, sollten Sie sehen, dass Ihr Eintrag aus der Liste der Registrierungsgruppen entfernt wurde.  

> [!NOTE]
> Wenn Sie eine Registrierungsgruppe für ein Zertifikat löschen, können Geräte, die dieses Zertifikat in der Zertifikatkette enthalten, weiterhin registriert werden – vorausgesetzt eine aktivierte Registrierungsgruppe für das Stammzertifikat oder ein anderes Zwischenzertifikat ist weiter oben in der Zertifikatkette vorhanden.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Hinzufügen bestimmter Geräte in einer Registrierungsgruppe zur Blacklist

Geräte, die den X.509-Nachweismechanismus implementieren, werden anhand der Zertifikatkette und des privaten Schlüssels des Geräts authentifiziert. Wenn ein Gerät eine Verbindung herstellt und beim Device Provisioning-Dienst authentifiziert wird, sucht der Dienst zuerst eine individuelle Registrierung, die den Anmeldeinformationen des Geräts entspricht. Der Dienst sucht dann Registrierungsgruppen, um zu bestimmen, ob das Gerät bereitgestellt werden kann. Wenn der Dienst eine deaktivierte individuelle Registrierung für das Gerät findet, wird das Gerät daran gehindert, eine Verbindung herzustellen. Der Dienst verhindert einen Verbindungsaufbau, selbst wenn eine Registrierungsgruppe für eine Zwischen- oder Stammzertifizierungsstelle in der Zertifikatkette des Geräts vorhanden ist. 

Um ein einzelnes Gerät in einer Registrierungsgruppe zur Blacklist hinzuzufügen, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim Azure-Portal an, und wählen Sie im linken Menü **Alle Ressourcen** aus.
2. Wählen Sie in der Liste der Ressourcen den Bereitstellungsdienst aus, der die Registrierungsgruppe für das zur Blacklist hinzuzufügende Gerät enthält.
3. Wählen Sie in Ihrem Bereitstellungsdienst **Registrierungen verwalten** und dann die Registerkarte **Individuelle Registrierungen** aus.
4. Klicken Sie ganz oben auf die Schaltfläche **Hinzufügen**. 
5. Wählen Sie als Sicherheitsmechanismus für das Gerät **X.509** aus, und laden Sie das Gerätezertifikat hoch. Dies ist das signierte auf dem Gerät installierte Endeinheitszertifikat. Das Gerät verwendet diese zum Generieren von Zertifikaten für die Authentifizierung.
6. Geben Sie unter **IoT Hub-Geräte-ID** die ID für das Gerät ein. 
7. Wählen Sie **Deaktivieren** für den Umschalter **Eintrag aktivieren** und dann **Speichern** aus. 

   ![Deaktivieren eines Eintrags für eine individuelle Registrierung im Portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)

Nach erfolgreicher Erstellung Ihrer Registrierung sollte Ihr Gerät auf der Registerkarte **Individuelle Registrierungen** angezeigt werden.


