---
title: Gerätekonnektivität in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel werden die wichtigsten Konzepte in Bezug auf die Gerätekonnektivität in Azure IoT Central vorgestellt.
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 7e90fb6bcfa1bfab59177cbc6c717fefc163a67a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960095"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Gerätekonnektivität in Azure IoT Central

In diesem Artikel werden die wichtigsten Konzepte in Bezug auf die Gerätekonnektivität in Microsoft Azure IoT Central vorgestellt.

Für Azure IoT Central wird der [Azure IoT Hub Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) genutzt, damit für IoT Central das bedarfsgesteuerte Onboarding und die Verbindungsherstellung unterstützt werden können.

-   Kunden können jetzt Geräteanmeldeinformationen generieren und die Geräte offline konfigurieren, ohne diese zuerst unter IoT Central registrieren zu müssen.
-   IoT Central unterstützt die Geräteverbindung mit für die Branche empfohlener Konnektivität auf Basis von X.509-Zertifikaten, während die SAS-Konnektivität (Shared Access Signatures) weiter unterstützt und verbessert wird.
-   IoT Central-Kunden können jetzt ihre eigenen Geräte-IDs nutzen, um Geräte in IoT Central zu registrieren und so die einfache Integration in vorhandene Backoffice-Systeme zu ermöglichen.
-   Es gibt eine einheitliche Möglichkeit zum Verbinden von Geräten mit IoT Central. 

>[!NOTE]
>Für IoT Central wird der Azure IoT Device Provisioning Service (DPS) im Hintergrund für alle Geräteregistrierungen und -verbindungen verwendet. [Hier finden Sie weitere Informationen zum DPS](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

Befolgen Sie basierend auf Ihrem Anwendungsfall die Anleitung zum Herstellen der Verbindung von Geräten mit IoT Central.
1. [Schnelles Verbinden eines einzelnen Geräts (per Shared Access Signatures)](#connect-a-single-device)
1. [Bedarfsabhängiges Verbinden von Geräten per Shared Access Signatures (SAS)](#connect-devices-at-scale-using-shared-access-signatures)
1. [Bedarfsabhängiges Verbinden von Geräten mit X.509-Zertifikaten](#connect-devices-using-x509-certificates) **(für Produktionsworkloads empfohlen)**
1. [Verbinden ohne vorherige Registrierung der Geräte](#connect-without-first-registering-devices) 


>[!NOTE]
>Hier ist der globale Endpunkt für die Verbindungsherstellung und Bereitstellung von Geräten angegeben: **global.azure-devices-provisioning.net**.

## <a name="connect-a-single-device"></a>Verbinden eines einzelnen Geräts
Das Herstellen einer Verbindung für ein einzelnes Gerät mit IoT Central per SAS ist einfach und erfordert nur wenige Schritte. 
1. Fügen Sie über den Device Explorer ein **echtes Gerät** hinzu. Klicken Sie auf **+ Neu > Real** (Echt), um ein echtes Gerät hinzuzufügen.
    * Geben Sie die Geräte-ID **<span style="color:Red">(muss in Kleinbuchstaben angegeben werden)</span>** ein, oder verwenden Sie die vorgeschlagene Geräte-ID.
    * Geben Sie den Gerätenamen ein, oder verwenden Sie den vorgeschlagenen Namen.   
    ![Gerät hinzufügen](media/concepts-connectivity/add-device.png)
1. Rufen Sie Verbindungsdetails wie **Bereichs-ID, Geräte-ID und Primärschlüssel** für das hinzugefügte Gerät ab, indem Sie auf der Geräteseite auf **Verbinden** klicken.
    * Die **[Bereichs-ID](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope)** gilt für die IoT Central-App und wird vom DPS generiert, um für eine App eine eindeutige Geräte-ID sicherzustellen.
    * Die **Geräte-ID** ist die eindeutige Geräte-ID für die App. Das Gerät muss die Geräte-ID im Rahmen des Registrierungsaufrufs senden.   
    * Der **Primärschlüssel** ist ein SAS-Token, das von IoT Central für das jeweilige Gerät erstellt wird. 
    ![Verbindungsdetails](media/concepts-connectivity/device-connect.PNG)
1. Verwenden Sie die Verbindungsdetails **Geräteidentität, Gerätename und Primärschlüssel des Geräts** in Ihrem Gerätecode, um das Gerät bereitzustellen und die Verbindung dafür herzustellen. Sie können den Datenfluss dann sofort verfolgen. Wenn Sie das MxChip-Gerät verwenden, können Sie [diese Schritt-für-Schritt-Anleitung](howto-connect-devkit.md#add-a-real-device) befolgen. Beginnen Sie beim Abschnitt **Vorbereiten des DevKit-Geräts**.   

    Unten sind die Verweise für andere Sprachen angegeben, die Sie ggf. nutzen möchten.

    *   **Programmiersprache C:** Nutzen Sie bei Verwendung von C [dieses C-Beispiel für einen Geräteclient](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md), um ein Beispielgerät zu verbinden. Verwenden Sie im Beispiel die folgenden Einstellungen.   

         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
         
         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";

         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```

    *   **Node.js:**  Wenn Sie Node.js verwenden möchten, können Sie [diese Schritt-für-Schritt-Anleitung](tutorial-add-device.md#prepare-the-client-code) befolgen. Beginnen Sie beim Abschnitt **Vorbereiten des Clientcodes**.



## <a name="connect-devices-at-scale-using-shared-access-signatures"></a>Bedarfsabhängiges Verbinden von Geräten per Shared Access Signature

Es sind zwei Schritte erforderlich, um Geräte bedarfsabhängig per SAS mit IoT Central zu verbinden: 
1. **Geräteregistrierung:** Importieren Sie die Geräte über eine CSV-Datei in IoT Central, und exportieren Sie sie mit den Details für die Geräteverbindung, um diese zum Herstellen der Verbindung für Ihre Geräte zu verwenden.
1. **Geräteinstallation:** Das Gerät wurde mit den Verbindungsdetails programmiert (**Bereichs-ID, Geräte-ID und Primärschlüssel**), sodass der Bereitstellungsdienst aufgerufen werden kann, um beim Einschalten die Verbindungsinformationen bzw. die Zuweisung der IoT Central-App zu erhalten.

>[!NOTE]
>Es ist auch eine erweiterte Option verfügbar, bei der Sie Geräte verbinden können, ohne dass Sie diese zuerst in IoT Central registrieren müssen. [Weitere Informationen finden Sie hier](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

**Registrieren von Geräten**

Um eine Verbindung einer großen Anzahl von Geräten mit Ihrer Anwendung herzustellen, ermöglicht Azure IoT Central das Massenimportieren von Geräten über eine CSV-Datei. 

Anforderungen an die CSV-Datei: Die CSV-Datei sollte die folgenden Spalten (und Kopfzeilen) aufweisen:
1.  IOTC_DeviceID **<span style="color:Red">(Kleinbuchstaben)</span>**
1.  IOTC_DeviceName (optional)



Importieren von Geräten für die Registrierung in Ihrer Anwendung
1.  Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus.
1.  Wählen Sie im linken Bereich die Gerätevorlage aus, für die Sie das Massenerstellen der Geräte ausführen möchten. 
1.  Klicken Sie auf **Importieren**, und wählen Sie die CSV-Datei aus, die die Liste mit den zu importierenden Geräte-IDs enthält.
Die CSV-Datei sollte die folgenden Spalten (und Kopfzeilen) aufweisen:
    *   IOTC_DeviceID **<span style="color:Red">(Kleinbuchstaben)</span>**
    *   IOTC_DeviceName (optional)
1.  Nach Abschluss des Imports wird eine Erfolgsmeldung auf dem Geräteraster angezeigt.

Exportieren Sie die Geräte, um die Verbindungsdetails zu erhalten. Beim Export wird eine CSV-Datei mit der Geräte-ID, dem Gerätenamen und dem Geräteschlüssel erstellt. Verwenden Sie diese Informationen, um für das Gerät eine Verbindung mit IoT Central herzustellen.
So führen Sie den Massenexport von Geräten aus Ihrer Anwendung durch:
1.  Wählen Sie im Navigationsmenü auf der linken Seite die Option **Explorer** aus.
1.  Wählen Sie die Geräte aus, die Sie exportieren möchten, und klicken Sie dann auf die Aktion **Exportieren**.
1.  Sobald der Exportvorgang abgeschlossen ist, wird eine Erfolgsmeldung zusammen mit einem Link zum Download der generierten Datei angezeigt.
1.  Klicken Sie auf die Erfolgsmeldung, um die Datei in einen lokalen Ordner auf dem Datenträger herunterzuladen.
1.  Die exportierte CSV-Datei enthält die folgenden Spalteninformationen: **Geräte-ID, Name des Geräts, Primärschlüssel und sekundärer Schlüssel des Geräts sowie primärer und sekundärer Zertifikatfingerabdruck**
    *   IOTC_DEVICEID
    *   IOTC_DEVICENAME
    *   IOTC_SASKEY_PRIMARY
    *   IOTC_SASKEY_SECONDARY
    *   IOTC_X509THUMBPRINT_PRIMARY 
    *   IOTC_X509THUMBPRINT_SECONDARY


**Geräteinstallation**

Verwenden Sie die Verbindungsdetails **Geräteidentität (IOTC_DEVICEID), Primärschlüssel des Geräts (IOTC_SASKEY_PRIMARY) und Bereichs-ID** in Ihrem Gerätecode, um Ihr Gerät bereitzustellen und zu verbinden. Rufen Sie, falls erforderlich, die **Bereichs-ID** aus Ihrer IoT Central-App ab, indem Sie **Verwaltung > Geräteverbindung > Bereichs-ID** wählen.
Wenn Sie das **MxChip**-Gerät für die Verbindungsherstellung verwenden, können Sie [diese Schritt-für-Schritt-Anleitung](howto-connect-devkit.md#add-a-real-device) befolgen. Beginnen Sie beim Abschnitt **Vorbereiten des DevKit-Geräts**.   

Unten sind die Verweise für andere Sprachen angegeben, die Sie ggf. nutzen möchten.

   *   **Programmiersprache C:** Nutzen Sie bei Verwendung von C [dieses C-Beispiel für einen Geräteclient](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md), um ein Beispielgerät zu verbinden. Verwenden Sie im Beispiel die folgenden Einstellungen.   
         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";
         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```
    * **Node.js:**  Wenn Sie Node.js verwenden möchten, können Sie [diese Schritt-für-Schritt-Anleitung](tutorial-add-device.md#prepare-the-client-code) befolgen. Beginnen Sie beim Abschnitt **Vorbereiten des Clientcodes**.


## <a name="connect-devices-using-x509-certificates"></a>Verbinden von Geräten mit X.509-Zertifikaten

Die Verwendung von X.509-Zertifikaten als Nachweismechanismus ist eine exzellente Möglichkeit, die **Produktion** zu skalieren und die Gerätebereitstellung zu vereinfachen. X.509-Zertifikate sind üblicherweise in einer Zertifikatvertrauenskette angeordnet, in der jedes Zertifikat in der Kette durch den privaten Schlüssel des nächsthöheren Zertifikats signiert ist. Den Abschluss der Kette bildet ein selbstsigniertes Stammzertifikat. So entsteht eine delegierte Vertrauenskette vom Stammzertifikat, das durch eine vertrauenswürdige Stammzertifizierungsstelle generiert wird, über jede Zwischenzertifizierungsstelle bis zum untergeordneten Zertifikat für die endgültige Entität, das auf einem Gerät installiert ist. Weitere Informationen finden Sie unter [Geräteauthentifizierung mit X.509-Zertifikaten](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Es sind drei Hauptschritte erforderlich, um für Geräte mit X.509-Zertifikaten eine Verbindung mit IoT Central herzustellen: 
1. **Konfigurieren der Verbindungseinstellungen:** Führen Sie dies in der IoT Central-App durch, indem Sie das X.509-Stamm- oder -Zwischenzertifikat hinzufügen bzw. verifizieren, das zum Generieren der Gerätezertifikate verwendet wird.  Es sind zwei Schritte zum Konfigurieren der Verbindungseinstellungen für X.509-Zertifikate erforderlich.  

    *   **Hinzufügen des X.509-Stamm- oder -Zwischenzertifikats:** Fügen Sie das Zertifikat hinzu, das Sie zum Generieren der Blattgerätezertifikate verwenden. Navigieren Sie zu „Verwaltung“ > „Geräteverbindung“ > „Zertifikate“. 
    
        ![Verbindungseinstellungen](media/concepts-connectivity/connection-settings.PNG)
    *   **Zertifikatverifizierung:** Durch das Verifizieren des Zertifikatbesitzes wird sichergestellt, dass der Benutzer, der das Zertifikat hochgeladen hat, im Besitz des privaten Schlüssels für das Zertifikat ist. So verifizieren Sie das Zertifikat
        *  Generieren des Prüfcodes: Klicken Sie auf die Schaltfläche neben dem Feld „Prüfcode“, um den Prüfcode zu generieren. 
        *  Erstellen Sie ein X.509-Verifizierungszertifikat mit dem Prüfcode, und speichern Sie das Zertifikat als CER-Datei. 
        *  Laden Sie das signierte Verifizierungszertifikat hoch, und klicken Sie auf „Überprüfen“.

        ![Verbindungseinstellungen](media/concepts-connectivity/verify-cert.png)
    *   **Sekundäres Zertifikat:** Während des Lebenszyklus Ihrer IoT-Lösung müssen Sie Zertifikate „rollen“ (festlegen, dass ein neues Zertifikat ab einem bestimmten Zeitpunkt zum aktuellen Zertifikat wird). Zwei der wichtigsten Gründe für das Rollen von Zertifikaten sind Sicherheitsverletzungen und der Ablauf von Zertifikaten. Sekundäre Zertifikate werden verwendet, um Ausfallzeiten für Geräte zu reduzieren, die bereitgestellt werden sollen, während Sie das primäre Zertifikat aktualisieren.

    **NUR FÜR TESTZWECKE** 
    
    Unten sind einige Hilfstools für die Befehlszeile angegeben, die Sie zum Generieren von Zertifizierungsstellenzertifikaten und Gerätezertifikaten verwenden können.

    * Bei Verwendung von MxChip können Sie [dieses Befehlszeilentool](https://aka.ms/iotcentral-docs-dicetool) nutzen, um Zertifizierungsstellenzertifikate zu generieren. Fügen Sie es Ihrer IoT Central-App hinzu, und verifizieren Sie die Zertifikate. 

    *   Verwenden Sie [dieses Befehlszeilentool](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) für Folgendes:
        * Erstellen Sie die Zertifikatskette (Schritt 2 in den GitHub-Dokumenten). 
         Speichern Sie die Zertifikate als CER-Dateien, und laden Sie sie in IoT Central (primär) hoch.   
        * Rufen Sie den Prüfcode aus der IoT Central-App ab, generieren Sie das Zertifikat (Schritt 3 in den GitHub-Dokumenten), und laden Sie es für die Verifizierung hoch. 
        * Erstellen Sie untergeordnete Zertifikate mit Ihrer Geräte-ID als Parameter für das Tool (Schritt 4). Speichern Sie das Zertifikat, und verwenden Sie es auf Ihrem Gerät.     

1. **Registrieren Sie Geräte**, indem Sie sie über eine CSV-Datei in IoT Central importieren.

1. **Einrichtung des Geräts:** Generieren Sie die untergeordneten Zertifikate, indem Sie das hochgeladene Stammzertifikat verwenden. Achten Sie darauf, dass Sie die **Geräte-ID** als CNAME in den untergeordneten Zertifikaten verwenden (in **Kleinbuchstaben**). Verwenden Sie [dieses Befehlszeilentool](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ), um untergeordnete Zertifikate bzw. Gerätezertifikate **NUR FÜR TESTZWECKE** zu generieren.

    Programmieren Sie das Gerät mit den Informationen des Bereitstellungsdiensts, damit beim Einschalten die Verbindungsdetails verfügbar sind und die Zuweisung der IoT Central-App erfolgen kann.    

    **Weitere Referenzen** 
    *   Beispielimplementierung für [Raspberry Pi](https://aka.ms/iotcentral-docs-Raspi-releases).  

    *   [Beispiel für einen Geräteclient in C](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md).

>[!NOTE]
>Verwenden Sie die **Geräte-ID** als CNAME, wenn Sie die untergeordneten Zertifikate für die Geräte generieren.

>[!NOTE]
>Die **Geräte-ID** muss aus Kleinbuchstaben bestehen. 
 
## <a name="connect-without-first-registering-devices"></a>Verbinden ohne vorherige Registrierung der Geräte
Eines der wichtigsten Szenarien, das mit IoT Central ermöglicht wird, ist Folgendes: OEMs können Geräte in Massenproduktion herstellen und die Anmeldeinformationen im Werk generieren und konfigurieren, ohne die Geräte zuerst bei IoT Central registrieren zu müssen. Nachdem die Geräte eingeschaltet wurden und eine Verbindung mit IoT Central hergestellt wurde, genehmigt der Bediener das Gerät, damit die Verbindung mit der IoT Central-App hergestellt werden kann.

Unten ist der Ablauf zum Herstellen der Verbindung für Geräte mit diesem Feature angegeben:

![Verbindungseinstellungen](media/concepts-connectivity/device-connection-flow.PNG)


Befolgen Sie die Schritte basierend auf Ihrem gewählten Schema für die Geräteauthentifizierung (X.509/SAS).

1. **Verbindungseinstellungen** 
    * **X509-Zertifikate:** [Fügen Sie das Stamm- bzw. Zwischenzertifikat hinzu, und verifizieren Sie es](#connect-devices-using-x509-certificates). Verwenden Sie es dann im nächsten Schritt, um die Gerätezertifikate zu generieren.
    * **SAS:** Kopieren Sie den Primärschlüssel (dieser Schlüssel ist der SAS-Gruppenschlüssel für diese IoT Central-Anwendung), und verwenden Sie ihn im nächsten Schritt, um die SAS-Schlüssel zu generieren. 
![Verbindungseinstellungen für SAS](media/concepts-connectivity/connection-settings-sas.png)

1. **Generieren von Geräteanmeldeinformationen** 
    *   **X509-Zertifikate:** Generieren Sie die untergeordneten Zertifikate für Ihre Geräte, indem Sie das Stamm- bzw. Zwischenzertifikat verwenden, das Sie dieser App hinzugefügt haben. Achten Sie darauf, dass Sie die **Geräte-ID** als CNAME in den untergeordneten Zertifikaten verwenden **<span style="color:Red">(in Kleinbuchstaben)</span>**. Verwenden Sie [dieses Befehlszeilentool](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ), um untergeordnete Zertifikate bzw. Gerätezertifikate für Testzwecke zu generieren.
    *   **SAS**: SAS-Geräteschlüssel können mit [diesem Befehlszeilentool](https://www.npmjs.com/package/dps-keygen) generiert werden. Verwenden Sie den SAS-Primärschlüssel (SAS-Gruppenschlüssel) aus dem vorherigen Schritt. Achten Sie darauf, dass die Geräte-ID nur **<span style="color:Red">Kleinbuchstaben</span>** enthält.

        Verwenden Sie die unten angegebene Anleitung, um den SAS-Geräteschlüssel zu generieren.           

        ```
        npm i -g dps-keygen
        ```
    
        **Verwendung**
                        
        ```
        dps-keygen <Primary_Key(GroupSAS)> <device_id>
        ```

1. **Geräteinstallation** 
    
     Aktualisieren Sie das Gerät per Flashvorgang mit der **Bereichs-ID, Geräte-ID und dem Gerätezertifikat/SAS-Schlüssel**, und schalten Sie das Gerät dann ein, um die Verbindung mit der IoT Central-App herzustellen.

1. **Verbinden des Geräts mit IoT Central:** Nachdem die Geräte eingeschaltet wurden, werden sie mit Microsoft System Center Data Protection Manager/IoT Central verbunden, um die Registrierung durchzuführen.

1. **Zuordnen eines Geräts zu einer Vorlage:** Das verbundene Gerät wird im **Device Explorer** unter **Nicht zugeordnete Geräte** angezeigt. Der Bereitstellungsstatus des Geräts lautet **Registriert**. Sie können das Gerät nun der entsprechenden Gerätevorlage **zuordnen** und es genehmigen, um die Verbindung mit der IoT Central-App herzustellen. Das Gerät ruft die Verbindungsdetails für die IoT Central-App ab, stellt eine Verbindung her und beginnt mit dem Senden von Daten. Die Gerätebereitstellung ist jetzt abgeschlossen, und der Status *Bereitstellung* ändert sich in **Bereitgestellt**.

## <a name="device-provisioning-status"></a>Device Provisioning-Status
Wenn für ein echtes Gerät eine Verbindung mit Azure IoT Central hergestellt wird, müssen einige Schritte ausgeführt werden. 
1. **Registriert**: Das Gerät hat zuerst den Status **Registriert**. Dies bedeutet, dass das Gerät in IoT Central erstellt wurde und über eine Geräte-ID verfügt.
Ein Gerät wird in folgenden Fällen registriert:  
    *   Ein neues echtes Gerät wird im **Explorer** hinzugefügt.
    *   Eine Gruppe von Geräten wird im **Explorer** mit der Option **Importieren** hinzugefügt.
    *   Für ein Gerät, das noch nicht registriert wurde, wird mit gültigen Anmeldeinformationen eine Verbindung hergestellt, und es wird unter **Nicht zugeordnete Geräte** angezeigt. 

    In allen obigen Fällen ist der *Bereitstellungsstatus* auf **Registriert** festgelegt.

1. **Bereitgestellt**: Wenn das Gerät mit gültigen Anmeldeinformationen eine Verbindung herstellt, führt IoT Central als nächsten Schritt die Bereitstellung durch (indem das Gerät in IoT Hub erstellt wird). Anschließend wird die Verbindungszeichenfolge an das Gerät zurückgegeben und mit dem Senden von Daten begonnen. Der *Bereitstellungsstatus* wird für das Gerät jetzt von **Registriert** in **Bereitgestellt** geändert.

1.  **Blockiert**: Der Bediener kann ein Gerät blockieren. Das Gerät kann dann keine Daten an IoT Central senden und muss zurückgesetzt werden. Für blockierte Geräte ist der *Bereitstellungsstatus* auf **Blockiert** festgelegt. Der Operator kann die Blockierung des Geräts auch aufheben. Nachdem die Blockierung des Geräts aufgehoben wurde, verfügt es wieder über den vorherigen *Bereitstellungsstatus* („Registriert“ oder „*Bereitgestellt*“). 

## <a name="getting-device-connection-string"></a>Abrufen der Geräte-Verbindungszeichenfolge
Sie können die IoT Hub-Geräte-Verbindungszeichenfolge in Azure IoT Hub abrufen, indem Sie die folgenden Schritte ausführen: 
1. Rufen Sie die Verbindungsdetails, z.B. **Bereichs-ID, Geräte-ID, Primärschlüssel für das Gerät**, von der Geräteseite ab. (Navigieren Sie zur Geräteseite, und klicken Sie auf „Verbinden“.) 

    ![Verbindungsdetails](media/concepts-connectivity/device-connect.PNG)

1. Rufen Sie die Geräte-Verbindungszeichenfolge ab, indem Sie das unten angegebene Befehlszeilentool verwenden.
    Befolgen Sie die Anleitung unten, um die Geräte-Verbindungszeichenfolge zu erhalten:  

    ```cmd/sh
    npm i -g dps-keygen
    ```
    **Verwendung**

    Suchen Sie nach der Binärdatei im Ordner „/bin“, um eine Verbindungszeichenfolge zu erstellen.
    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```
    Weitere Informationen zum Tool „dps-keygen“ finden Sie [hier](https://www.npmjs.com/package/dps-keygen).

## <a name="sdk-support"></a>SDK-Unterstützung

Die Azure-Geräte-SDKs bieten Ihnen die einfachste Möglichkeit zum Implementieren des Codes auf Ihren Geräten, über den eine Verbindung mit Ihrer Azure IoT Central-Anwendung hergestellt wird. Derzeit sind folgende Geräte-SDKs verfügbar:

- [Azure IoT-SDK für C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT-SDK für Python](https://github.com/azure/azure-iot-sdk-python)
- [Azure IoT-SDK für Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Azure IoT-SDK für Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT-SDK für .NET](https://github.com/azure/azure-iot-sdk-csharp)

Jedes Gerät stellt eine Verbindung über eine eindeutige Verbindungszeichenfolge her, die das Gerät identifiziert. Ein Gerät kann sich nur mit dem IoT Hub verbinden, bei dem es registriert ist. Wenn Sie ein reales Gerät in Ihrer Azure IoT Central-Anwendung erstellen, generiert die Anwendung eine Verbindungszeichenfolge.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK-Features und IoT Hub-Konnektivität

Bei der gesamten Gerätekommunikation mit IoT Hub werden die folgenden IoT Hub-Konnektivitätsoptionen verwendet:

- [Nachrichten, die von Geräten an die Cloud gesendet werden](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Gerätezwillinge](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

In der folgenden Tabelle wird die Zuordnung von Azure IoT Central-Gerätefeatures zu IoT Hub-Features zusammengefasst:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Messung: Telemetrie | Nachrichten, die von Geräten an die Cloud gesendet werden |
| Geräteeigenschaften | Gemeldete Eigenschaften von Gerätezwillingen |
| Einstellungen | Gewünschte und gemeldete Eigenschaften von Gerätezwillingen |

Weitere Informationen zur Verwendung der Geräte-SDKs sowie Beispielcode finden Sie in einem der folgenden Artikel:

- [Verbinden eines generischen Node.js-Clients mit Ihrer Azure IoT Central-Anwendung](howto-connect-nodejs.md)
- [Verbinden eines Raspberry Pi-Geräts mit Ihrer Azure IoT Central-Anwendung](howto-connect-raspberry-pi-python.md)
- [Verbinden eines DevDiv-Kit-Geräts mit Ihrer Azure IoT Central-Anwendung](howto-connect-devkit.md)


## <a name="protocols"></a>Protokolle

Die Geräte-SDKs unterstützen die folgenden Netzwerkprotokolle zum Herstellen einer Verbindung mit einem IoT Hub:

- MQTT
- AMQP
- HTTPS

Informationen zu diesen verschiedenen Protokollen sowie eine Anleitung zu deren Auswahl finden Sie unter [Auswählen eines Kommunikationsprotokolls](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Wenn Ihr Gerät keines der unterstützten Protokolle verwenden kann, können Sie mithilfe von Azure IoT Edge eine Protokollkonvertierung durchführen. IoT Edge unterstützt weitere Intelligence-on-the-Edge-Szenarien, um die Verarbeitung von der Azure IoT Central-Anwendung auf den Edge auszulagern.

## <a name="security"></a>Sicherheit

Alle Daten, die zwischen Geräten und Azure IoT Central ausgetauscht werden, werden verschlüsselt. IoT Hub authentifiziert jede Anforderung von einem Gerät, das eine Verbindung mit einem der geräteseitigen IoT Hub-Endpunkte herstellt. Um den unverschlüsselten Austausch von Anmeldeinformationen zu vermeiden, verwendet ein Gerät signierte Token zur Authentifizierung. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Derzeit müssen Geräte, die eine Verbindung mit Azure IoT Central herstellen, SAS-Token verwenden. X.509-Zertifikate werden für Geräte, die eine Verbindung mit Azure IoT Central herstellen, nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit der Gerätekonnektivität in Azure IoT Central vertraut gemacht haben, werden Ihnen als Nächstes die folgenden Schritte empfohlen:

- [Vorbereiten und Verbinden eines DevKit-Geräts](howto-connect-devkit.md)
- [Vorbereiten und Verbinden eines Raspberry Pi](howto-connect-raspberry-pi-python.md)
- [Verbinden eines generischen Node.js-Clients mit Ihrer Azure IoT Central-Anwendung](howto-connect-nodejs.md)
