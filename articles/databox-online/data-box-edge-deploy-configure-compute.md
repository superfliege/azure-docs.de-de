---
title: Transformieren von Daten mit Azure Data Box Edge | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Computerolle in Data Box Edge konfigurieren und verwenden, um Daten vor dem Senden an Azure zu transformieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: c52c311f1e1cd1335ea5797eadacd0bc89e1b36c
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "54402114"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Tutorial: Transformieren von Daten mit Azure Data Box Edge (Vorschauversion)

In diesem Tutorial wird beschrieben, wie Sie eine Computerolle auf Ihrem Azure Data Box Edge-Gerät konfigurieren. Nach dem Konfigurieren der Computerolle kann Data Box Edge Daten vor dem Senden an Azure transformieren.

Dieser Vorgang kann ca. 30 bis 45 Minuten dauern.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Azure IoT Hub-Ressource
> * Einrichten einer Computerolle
> * Hinzufügen eines Computemoduls
> * Überprüfen der Datentransformation und -übertragung

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen.
 
## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie eine Computerolle auf Ihrem Data Box Edge-Gerät einrichten:

* Das Data Box Edge-Gerät ist aktiviert, wie unter [Tutorial: Verbinden, Einrichten und Aktivieren von Azure Data Box Edge (Vorschau)](data-box-edge-deploy-connect-setup-activate.md) beschrieben.


## <a name="create-an-iot-hub-resource"></a>Erstellen einer IoT Hub-Ressource

Bevor Sie eine Computerolle für Data Box Edge einrichten, müssen Sie eine IoT Hub-Ressource erstellen.

Eine ausführliche Anleitung finden Sie unter [Erstellen eines IoT Hubs](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub). Verwenden Sie dasselbe Abonnement und dieselbe Ressourcengruppe wie für Ihre Data Box Edge-Ressource.

![Erstellen einer IoT Hub-Ressource](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Wurde keine Edge-Computerolle eingerichtet, gelten die folgenden Einschränkungen:

- Die IoT Hub-Ressource verfügt nicht über Azure IoT-Geräte oder Azure IoT Edge-Geräte.
- Sie können keine lokalen Edge-Freigaben erstellen. Beim Hinzufügen einer Freigabe ist die Option zum Erstellen einer lokalen Freigabe für Edge-Compute nicht aktiviert.


## <a name="set-up-compute-role"></a>Einrichten einer Computerolle

Wenn die Edge-Computerolle auf dem Edge-Gerät eingerichtet ist, werden zwei Geräte erstellt: ein IoT-Gerät und ein IoT Edge-Gerät. Beide Geräte können in der IoT Hub-Ressource angezeigt werden.

Führen Sie die folgenden Schritte aus, um die Computerolle auf dem Gerät einzurichten:

1. Navigieren Sie zur Data Box Edge-Ressource, und wählen Sie **Übersicht** und dann **Computerolle einrichten** aus. 

    ![Link „Übersicht“ im linken Bereich](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    Optional können Sie zu **Module** navigieren und **Compute konfigurieren** auswählen.

    ![Links „Module“ und „Compute konfigurieren“](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
1. Wählen Sie in der Dropdownliste die **IoT Hub-Ressource** aus, die Sie im vorherigen Schritt erstellt haben.  
    Derzeit ist für Ihr IoT Edge-Gerät nur die Linux-Plattform verfügbar. 
    
1. Klicken Sie auf **Create**.

    ![Die Schaltfläche „Erstellen“.](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
    Es dauert einige Minuten, bis die Computerolle erstellt wurde. Aufgrund eines Fehlers in dieser Version wird der Bildschirm nicht aktualisiert, wenn die Erstellung der Computerolle abgeschlossen ist. Wenn Sie überprüfen möchten, ob die Edge-Computerolle konfiguriert wurde, navigieren Sie zu **Module**.  

    ![Geräteliste „Edgecomputing konfigurieren“](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

1. Navigieren Sie erneut zu **Übersicht**.  
    Der Bildschirm wurde aktualisiert, und die konfigurierte Computerolle wird angezeigt.

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
1. Wechseln Sie in der IoT Hub-Instanz, die Sie beim Erstellen der Edge-Computerolle verwendet haben, zu **IoT-Geräte**.  
    Ein IoT-Gerät ist jetzt aktiviert. 

    ![Seite „IoT-Geräte“](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

1. Wählen Sie im linken Bereich die Option **IoT Edge** aus.  
    Nun ist außerdem ein IoT Edge-Gerät aktiviert.

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
1. Wählen Sie das IoT Edge-Gerät aus, und klicken Sie darauf.  
    Ein Edge-Agent wird auf diesem IoT Edge-Gerät ausgeführt. 

    ![Seite „Gerätedetails“](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

    Auf diesem Edge-Gerät sind keine benutzerdefinierten Module vorhanden, Sie können nun jedoch ein benutzerdefiniertes Modul hinzufügen. Informationen zum Erstellen eines benutzerdefinierten Moduls finden Sie unter [Entwickeln eines C#-IoT Edge-Moduls zum Verschieben von Dateien in Data Box Edge (Vorschau)](data-box-edge-create-iot-edge-module.md).


## <a name="add-a-custom-module"></a>Hinzufügen eines benutzerdefinierten Moduls

In diesem Abschnitt fügen Sie dem IoT Edge-Gerät, das Sie unter [Entwickeln eines C#-IoT Edge-Moduls zum Verschieben von Dateien in Data Box Edge (Vorschau)](data-box-edge-create-iot-edge-module.md) erstellt haben, ein benutzerdefiniertes Modul hinzu. 

Im folgenden Verfahren wird ein Beispiel verwendet, in dem für das benutzerdefinierte Modul Dateien von einer lokalen Freigabe auf dem Edge-Gerät auf eine Cloudfreigabe auf dem Gerät verschoben werden. Die Cloudfreigabe überträgt die Dateien dann per Pushvorgang an das Azure-Speicherkonto, das der Cloudfreigabe zugeordnet ist. 

1. Fügen Sie wie folgt auf dem Edge-Gerät eine lokale Dateifreigabe hinzu:

    a. Navigieren Sie auf Ihrer Data Box Edge-Ressource zu **Freigaben**. 
    
    b. Wählen Sie **Freigabe hinzufügen** aus, geben Sie dann den Freigabenamen an, und wählen Sie den Freigabetyp aus. 
    
    c. Aktivieren Sie das Kontrollkästchen **Als lokale Edge-Freigabe konfigurieren**, um eine lokale Freigabe zu erstellen. 
    
    d. Wählen Sie **Neues Element erstellen** oder **Vorhandenes Element verwenden** und dann **Erstellen** aus.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Wenn Sie eine lokale NFS-Freigabe erstellt haben, können Sie die folgende rsync-Befehlsoption verwenden, um Dateien auf die Freigabe zu kopieren:

    `rsync --inplace <source file path> < destination file path>`

    Weitere Informationen zum rsync-Befehl finden Sie in der [Dokumentation zu rsync](https://www.computerhope.com/unix/rsync.htm). 

    Die lokale Freigabe wird erstellt, und Sie erhalten eine Benachrichtigung zur erfolgreichen Erstellung. Die Freigabeliste wird unter Umständen aktualisiert. Sie müssen jedoch warten, bis die Erstellung der Freigabe abgeschlossen ist.
    
1. Navigieren Sie zur Liste der Freigaben. 

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
1. Wählen Sie die neu erstellte lokale Freigabe aus, um ihre Eigenschaften anzuzeigen. 

1. Kopieren Sie im Feld **Lokaler Bereitstellungspunkt für Edgecomputingmodule** den Wert für diese Freigabe.  
    Sie verwenden diesen lokalen Bereitstellungspunkt beim Bereitstellen des Moduls.

    ![Feld „Lokaler Bereitstellungspunkt für Edgecomputingmodule“](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
1. Kopieren Sie auf einer vorhandenen Cloudfreigabe, die auf Ihrem Data Box Edge-Gerät erstellt wurde, im Feld **Lokaler Bereitstellungspunkt für Edgecomputingmodule** den lokalen Bereitstellungspunkt für Edgecomputingmodule für diese Cloudfreigabe.  
    Sie verwenden diesen lokalen Bereitstellungspunkt beim Bereitstellen des Moduls.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

1. Navigieren Sie zu Ihrer IoT Hub-Ressource und dann zum **IoT Edge-Gerät**, um dem IoT Edge-Gerät ein benutzerdefiniertes Modul hinzuzufügen. 

1. Wählen Sie das Gerät und dann unter **Gerätedetails** die Option **Module festlegen** aus. 

    ![Link „Module festlegen“](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

1. Führen Sie unter **Module hinzufügen** die folgenden Schritte aus:

    a. Geben Sie für das benutzerdefinierte Modul in den Containerregistrierungseinstellungen den Namen, die Adresse, den Benutzernamen und das Kennwort ein.  
    Der Name, die Adresse und die aufgeführten Anmeldeinformationen werden verwendet, um Module mit einer übereinstimmenden URL abzurufen. Wählen Sie zum Bereitstellen dieses Moduls unter **Bereitstellungsmodule** die Option **IoT Edge-Modul**. Dieses IoT Edge-Modul ist ein Docker-Container, den Sie auf dem IoT Edge-Gerät bereitstellen können, das Ihrem Data Box Edge-Gerät zugeordnet ist.

    ![Seite „Module festlegen“](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    b. Geben Sie die Einstellungen für das benutzerdefinierte IoT Edge-Modul an, indem Sie den Namen Ihres Moduls und den Image-URI für das entsprechende Containerimage eingeben. 
    
    ![Seite „Benutzerdefinierte IoT Edge-Module“](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    c. Geben Sie im Feld **Optionen für Containererstellung** die lokalen Bereitstellungspunkte für die Edge-Module ein, die Sie im vorherigen Schritt für die Cloudfreigabe und die lokale Freigabe kopiert haben.
    > [!IMPORTANT]
    > Erstellen Sie keine neuen Pfade, sondern verwenden Sie die kopierten Pfade. Die lokalen Bereitstellungspunkte werden dem entsprechenden Eingabeordnerpfad (**InputFolderPath**) und Ausgabeordnerpfad (**OutputFolderPath**) zugeordnet, die Sie beim [Aktualisieren des Moduls mit benutzerdefiniertem Code](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code) im Modul angegeben haben. 
    
    Im Feld **Optionen für Containererstellung** können Sie das folgende Beispiel einfügen: 
    
    ```
    {
        "HostConfig": {
        "Binds": [
        "/home/hcsshares/mysmblocalshare:/home/LocalShare",
        "/home/hcsshares/mysmbshare1:/home/CloudShare"
        ]
        }
    }
    ```

    Geben Sie hier auch alle Umgebungsvariablen für Ihr Modul an.

    ![Feld „Optionen für Containererstellung“](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    d. Konfigurieren Sie die erweiterten Einstellungen für die Edge-Laufzeit, falls erforderlich, und klicken Sie dann auf **Weiter**.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
1.  Legen Sie unter **Routen angeben** die Routen zwischen den Modulen fest.  
    Geben Sie in diesem Beispiel den Namen der lokalen Freigabe ein, über die Daten per Pushvorgang an die Cloudfreigabe übertragen werden.

    Sie können die *Route* durch die folgende Routenzeichenfolge ersetzen:       `"route": "FROM /* WHERE topic = 'mysmblocalshare' INTO BrokeredEndpoint(\"/modules/filemovemodule/inputs/input1\")"`

    ![Abschnitt „Routen angeben“](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 

1. Klicken Sie auf **Weiter**. 

1.  Sehen Sie sich unter **Bereitstellung prüfen** alle Einstellungen an, und wählen Sie dann **Übermitteln**, um das Modul für die Bereitstellung zu übermitteln.

    ![Seite „Module festlegen“](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
    Mit dieser Aktion wird die Modulbereitstellung gestartet, wie in der folgenden Abbildung gezeigt:

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Überprüfen der Datentransformation und -übertragung

Im letzten Schritt wird sichergestellt, dass das Modul verbunden ist und wie erwartet ausgeführt wird. Der Laufzeitstatus des Moduls sollte für Ihr IoT Edge-Gerät auf der IoT Hub-Ressource „Wird ausgeführt“ lauten.

![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
Führen Sie die folgenden Schritte aus, um zu überprüfen, ob das Modul ausgeführt wird:

1. Wählen Sie das Modul aus, und sehen Sie sich den Zwilling der Modulidentität an.  
    Der Clientstatus für das Edge-Gerät und das Modul sollte *Verbunden* lauten.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
    Wenn das Modul ausgeführt wird, wird es auch in der Liste mit den Edge-Modulen auf Ihrer Data Box Edge-Ressource angezeigt. Der Laufzeitstatus des hinzugefügten Moduls lautet *Wird ausgeführt*.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
1.  Stellen Sie im Datei-Explorer eine Verbindung mit den lokalen Freigaben und Cloudfreigaben her, die Sie zuvor erstellt haben.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
1.  Fügen Sie der lokalen Freigabe Daten hinzu.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
    Die Daten werden auf die Cloudfreigabe verschoben.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

    Anschließend werden die Daten per Pushvorgang von der Cloudfreigabe in das Speicherkonto übertragen. Wechseln Sie zum Speicher-Explorer, um die Daten anzuzeigen.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Sie haben den Überprüfungsvorgang abgeschlossen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer IoT Hub-Ressource
> * Einrichten einer Computerolle
> * Hinzufügen eines Computemoduls
> * Überprüfen der Datentransformation und -übertragung

Weitere Informationen zum Verwalten Ihres Data Box Edge-Geräts finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Verwenden der lokalen Webbenutzeroberfläche zum Verwalten einer Data Box Edge-Instanz](https://aka.ms/dbg-docs)


