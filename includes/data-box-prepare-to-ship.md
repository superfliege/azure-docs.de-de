---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/07/2019
ms.author: alkohli
ms.openlocfilehash: c7e5231650ec1afb97a72ec0cf26cb8f80088b63
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54440434"
---
Der letzte Schritt ist die Vorbereitung des Geräts für den Versands. In diesem Schritt werden alle Gerätefreigaben offline geschaltet. Sobald Sie mit diesem Prozess begonnen haben, können Sie nicht mehr auf die Freigaben zugreifen.

> [!IMPORTANT]
> Im Rahmen der erforderlichen Versandvorbereitung werden Daten markiert, die nicht den Namenskonventionen von Azure entsprechen. Wenn Sie diesen Schritt überspringen, treten beim Hochladen möglicherweise Fehler aufgrund von nicht konformen Daten auf.

1. Wechseln Sie zu **Versandvorbereitung**, und klicken Sie auf **Vorbereitung starten**. Prüfsummen werden standardmäßig inline während der Vorbereitung für den Versand berechnet. Die Berechnung der Prüfsumme kann je nach Größe Ihrer Daten mehrere Stunden oder Tage dauern. 
   
    ![Versandvorbereitung 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

    Die Gerätevorbereitung kann bei Bedarf durch Klicken auf **Vorbereitung beenden** beendet werden. Sie können die Versandvorbereitung später fortsetzen.
        
    ![Versandvorbereitung 2](media/data-box-prepare-to-ship/prepare-to-ship2.png)
    
2. Die Versandvorbereitung beginnt, und die Gerätefreigaben werden offline geschaltet. Sobald das Gerät bereit ist, werden Sie daran erinnert, das Adressetikett herunterzuladen.

    ![Erinnerung an das Herunterladen des Adressetiketts](media/data-box-prepare-to-ship/download-shipping-label-reminder.png)

3. Nach Abschluss der Gerätevorbereitung ändert sich der Gerätestatus in *Bereit für den Versand*, und das Gerät wird gesperrt.
        
    ![Versandvorbereitung 3](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    Falls Sie weitere Daten auf das Gerät kopieren möchten, können Sie das Gerät entsperren, weitere Daten kopieren und die Versandvorbereitung erneut ausführen.

    Sollten bei diesem Schritt Fehler auftreten, müssen Sie das Fehlerprotokoll herunterladen und die Fehler beheben. Führen Sie nach Abschluss der Fehlerbehebung **Für Versand vorbereiten** aus.

4. Laden Sie im Anschluss an die (fehlerfreie) Versandvorbereitung das Manifest (Liste mit den Dateien, die im Rahmen dieses Prozesses kopiert wurden) herunter. Sie können diese Liste später verwenden, um die in Azure hochgeladenen Dateien zu überprüfen.
        
    ![Versandvorbereitung 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

5. Fahren Sie das Gerät herunter. Wechseln Sie zur Seite **Herunterfahren oder neu starten**, und klicken Sie auf **Herunterfahren**. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **OK**, um fortzufahren.

6. Entfernen Sie die Kabel. Im nächsten Schritt versenden Sie das Gerät an Microsoft.
