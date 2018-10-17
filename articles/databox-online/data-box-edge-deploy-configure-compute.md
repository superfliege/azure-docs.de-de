---
title: Transformieren von Daten mit Azure Data Box Edge | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Computerolle in Data Box Edge konfigurieren und verwenden, um Daten vor dem Senden an Azure zu transformieren.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: fd04c906f57a9378879ef6357e2724695533d370
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48832831"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Tutorial: Transformieren von Daten mit Azure Data Box Edge (Vorschauversion)

In diesem Tutorial wird beschrieben, wie Sie die Computerolle für Data Box Edge konfigurieren. Nachdem die Computerolle konfiguriert wurde, kann Data Box Edge die Daten vor dem Senden an Azure transformieren.

Dieser Vorgang kann ca. 30 bis 45 Minuten dauern. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer IoT Hub-Ressource
> * Einrichten einer Computerolle
> * Hinzufügen eines Computemoduls
> * Überprüfen der Datentransformation und -übertragung

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 
 
## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie Compute für Ihre Data Box Edge-Instanz einrichten:

* Ihr Data Box Edge-Gerät wird wie unter [Verbinden, Einrichten und Aktivieren von Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md) beschrieben aktiviert.


## <a name="create-an-iot-hub-resource"></a>Erstellen einer IoT Hub-Ressource

Bevor Sie eine Computerolle für Data Box Edge einrichten, müssen Sie eine IoT Hub-Ressource erstellen.

Eine ausführliche Anleitung finden Sie unter [Erstellen eines IoT Hubs](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub). Verwenden Sie dasselbe Abonnement und dieselbe Ressourcengruppe wie für Ihre Data Box Edge-Ressource.

![Erstellen einer IoT Hub-Ressource](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Beachten Sie Folgendes, wenn die Edge-Computerolle nicht eingerichtet ist: 
- Die IoT Hub-Ressource verfügt nicht über IoT-Geräte oder IoT Edge-Geräte.
- Sie können keine lokalen Edge-Freigaben erstellen. Beim Hinzufügen einer Freigabe ist die Option zum Erstellen einer lokalen Freigabe für Edge-Compute nicht aktiviert.


## <a name="set-up-compute-role"></a>Einrichten einer Computerolle

Wenn die Edge-Computerolle auf dem Edge-Gerät eingerichtet ist, werden zwei Geräte erstellt: ein IoT-Gerät und ein IoT Edge-Gerät. Beide Geräte können in der IoT Hub-Ressource angezeigt werden.

Führen Sie die folgenden Schritte aus, um die Computerolle auf dem Gerät einzurichten.

1. Navigieren Sie zur Data Box Edge-Ressource und dann zu **Übersicht**, und klicken Sie auf **Computerolle einrichten**. 

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    Sie können auch zu **Module** navigieren und auf **Compute konfigurieren** klicken.

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
2. Wählen Sie in der Dropdownliste die **IoT Hub-Ressource** aus, die Sie im vorherigen Schritt erstellt haben. Derzeit ist für Ihr IoT Edge-Gerät nur die Linux-Plattform verfügbar. Klicken Sie auf **Create**.

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
3. Es dauert einige Minuten, bis die Computerolle erstellt wurde. Aufgrund eines Fehlers in dieser Version wird der Bildschirm nicht aktualisiert, wenn die Erstellung der Computerolle abgeschlossen ist. Wenn Sie zu **Module** navigieren, können Sie sehen, dass die Edge-Computerolle konfiguriert wurde.  

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

4. Wenn Sie jetzt wieder zu **Übersicht** navigieren, ist der Bildschirm aktualisiert, und die konfigurierte Computerolle wird angezeigt.

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
5. Navigieren Sie zu dem IoT Hub, den Sie beim Erstellen der Edge-Computerolle verwendet haben. Navigieren Sie zu **IoT-Geräte**. Sie sehen, dass jetzt ein IoT-Gerät aktiviert ist. 

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

6. Wenn Sie zu **IoT Edge** navigieren, sehen Sie, dass auch ein IoT Edge-Gerät aktiviert wurde.

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
7. Wählen Sie das IoT Edge-Gerät aus, und klicken Sie darauf. Ein Edge-Agent wird auf diesem IoT Edge-Gerät ausgeführt. 

    ![Einrichten einer Computerolle](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

Auf diesem Edge-Gerät sind aber keine benutzerdefinierten Module vorhanden. Sie können diesem Gerät jetzt ein benutzerdefiniertes Modul hinzufügen.


## <a name="add-a-custom-module"></a>Hinzufügen eines benutzerdefinierten Moduls

In diesem Abschnitt fügen Sie dem IoT Edge-Gerät ein benutzerdefiniertes Modul hinzu. 

In diesem Verfahren wird ein Beispiel verwendet, in dem für das verwendete benutzerdefinierte Modul Dateien von einer lokalen Freigabe auf dem Edge-Gerät auf eine Cloudfreigabe auf dem Gerät verschoben werden. Die Cloudfreigabe überträgt die Dateien dann per Pushvorgang an das Azure-Speicherkonto, das der Cloudfreigabe zugeordnet ist. 

1. Der erste Schritt ist das Hinzufügen einer lokalen Freigabe auf dem Edge-Gerät. Navigieren Sie auf Ihrer Data Box Edge-Ressource zu **Freigaben**. Klicken Sie auf **+ Freigabe hinzufügen**. Geben Sie den Freigabenamen an, und wählen Sie den Typ der Freigabe aus. Aktivieren Sie die Option **Als lokale Edge-Freigabe konfigurieren**, um eine lokale Freigabe zu erstellen. Wählen Sie einen **vorhandenen Benutzer** aus, oder **erstellen Sie einen neuen**. Klicken Sie auf **Create**.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Wenn Sie eine lokale NFS-Freigabe erstellt haben, können Sie die folgende rsync-Befehlsoption verwenden, um Dateien auf die Freigabe zu kopieren:

    `rsync --inplace <source file path> < destination file path>`

     Weitere Informationen zum rsync-Befehl finden Sie in der [Dokumentation zu rsync](https://www.computerhope.com/unix/rsync.htm). 

 
2. Nachdem die lokale Freigabe erstellt wurde und Sie eine Benachrichtigung über die erfolgreiche Erstellung erhalten haben (ggf. wird die Liste mit den Freigaben schon vorher aktualisiert, aber Sie müssen auf den Abschluss der Freigabenerstellung warten), können Sie zur Liste mit den Freigaben navigieren. 

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
3. Wählen Sie die neu erstellte lokale Freigabe aus, und klicken Sie darauf, um ihre Eigenschaften anzuzeigen. Kopieren und speichern Sie den **lokalen Bereitstellungspunkt für Edge-Module** für diese Freigabe.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
    Navigieren Sie zu einer vorhandenen Cloudfreigabe, die auf Ihrem Data Box Edge-Gerät erstellt wurde. Kopieren und speichern Sie den lokalen Bereitstellungspunkt für Edge-Computemodule für diese Cloudfreigabe. Diese lokalen Bereitstellungspunkte werden beim Bereitstellen des Moduls verwendet.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

4. Navigieren Sie zu Ihrer IoT Hub-Ressource und dann zum **IoT Edge-Gerät**, um dem IoT Edge-Gerät ein benutzerdefiniertes Modul hinzuzufügen. Wählen Sie das Gerät aus, und klicken Sie darauf. Klicken Sie unter **Gerätedetails** oben in der Befehlsleiste auf **Module festlegen**. 

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

5. Führen Sie unter **Module hinzufügen** die folgenden Schritte aus:

    1. Geben Sie für das benutzerdefinierte Modul in den **Container Registry-Einstellungen** Werte für **Name**, **Adresse**, **Benutzername** und **Kennwort** an. Der Name, die Adresse und die aufgeführten Anmeldeinformationen werden verwendet, um Module mit einer übereinstimmenden URL abzurufen. Wählen Sie zum Bereitstellen dieses Moduls unter **Bereitstellungsmodule** die Option **IoT Edge-Modul**. Dieses IoT Edge-Modul ist ein Docker-Container, den Sie auf dem IoT Edge-Gerät bereitstellen können, das Ihrem Data Box Edge-Gerät zugeordnet ist.

        ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    2. Geben Sie die Einstellungen für das benutzerdefinierte IoT Edge-Modul an. Geben Sie den **Namen** Ihres Moduls und den **Image-URI** an. 
    
        ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    3. Geben Sie unter **Optionen für Containererstellung** die lokalen Bereitstellungspunkte für die Edge-Module, die Sie in den vorherigen Schritten kopiert haben, für die Cloud und die lokale Freigabe an. (Es ist wichtig, dass Sie diese Pfade nutzen und keine neuen erstellen.) Diese Freigaben werden den entsprechenden Containerbereitstellungspunkten zugeordnet. Geben Sie hier auch alle Umgebungsvariablen für Ihr Modul an.

        ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    4. Verwenden Sie **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren**, falls erforderlich, und klicken Sie dann auf **Weiter**.

        ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
6.  Legen Sie unter **Routen angeben** die Routen zwischen den Modulen fest. Geben Sie in diesem Fall den Namen der lokalen Freigabe an, über die Daten per Pushvorgang an die Cloudfreigabe übertragen werden. Klicken Sie auf **Weiter**.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 
 
7.  Sehen Sie sich unter **Bereitstellung prüfen** alle Einstellungen an. Wenn Sie zufrieden sind, können Sie das Modul mit **Übermitteln** für die Bereitstellung übermitteln.

    ![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
Die Modulbereitstellung wird gestartet, wie im **Benutzerdefinierten IoT Edge-Modul** unter **Module** zu sehen ist.

![Hinzufügen eines benutzerdefinierten Moduls](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Überprüfen der Datentransformation und -übertragung

Im letzten Schritt wird sichergestellt, dass das Modul verbunden ist und wie erwartet ausgeführt wird. Führen Sie die folgenden Schritte aus, um zu überprüfen, ob das Modul ausgeführt wird.

1. Der Laufzeitstatus des Moduls sollte für Ihr IoT Edge-Gerät auf der IoT Hub-Ressource „Wird ausgeführt“ lauten.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
2. Wählen Sie das Modul aus, und klicken Sie darauf. Sehen Sie sich die Option **Zwilling der Modulidentität** an. Der Clientstatus für das Edge-Gerät und das Modul sollte **Verbunden** lauten.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
3.  Wenn das Modul ausgeführt wird, wird es auch in der Liste mit den Edge-Modulen auf Ihrer Data Box Edge-Ressource angezeigt. Der **Laufzeitstatus** des hinzugefügten Moduls lautet **Wird ausgeführt**.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
4.  Stellen Sie eine Verbindung mit den lokalen Freigaben und Cloudfreigaben her, die Sie im Datei-Explorer erstellt haben.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
5.  Fügen Sie der lokalen Freigabe Daten hinzu.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
6.  Die Daten werden auf die Cloudfreigabe verschoben.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

7.  Anschließend werden die Daten per Pushvorgang von der Cloudfreigabe in das Speicherkonto übertragen. Wechseln Sie zum Speicher-Explorer, um die Daten anzuzeigen.

    ![Überprüfen der Datentransformation](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Hiermit ist der Überprüfungsprozess abgeschlossen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Erstellen einer IoT Hub-Ressource
> * Einrichten einer Computerolle
> * Hinzufügen eines Computemoduls
> * Überprüfen der Datentransformation und -übertragung

Im nächsten Tutorial erfahren Sie, wie Sie Ihre Data Box Edge-Instanz verwalten.

> [!div class="nextstepaction"]
> [Verwenden der lokalen Webbenutzeroberfläche zum Verwalten einer Data Box Edge-Instanz](http://aka.ms/dbg-docs)


