---
title: Administratorhandbuch für das Azure Data Box-Portal | Microsoft-Dokumentation
description: Beschreibt die Verwaltung von Azure Data Box über das Azure-Portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: overview
ms.date: 10/19/2018
ms.author: alkohli
ms.openlocfilehash: 1b228a66f2d59b3ff252df266783f7bd5d27139e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645438"
---
# <a name="use-the-azure-portal-to-administer-your-data-box"></a>Verwalten Ihrer Data Box im Azure-Portal

Dieser Artikel beschreibt einige der komplexen Workflows und Verwaltungsaufgaben, die auf der Data Box ausgeführt werden können. Sie können die Data Box im Azure-Portal oder auf der lokalen Webbenutzeroberfläche verwalten. 

Der Schwerpunkt dieses Artikels liegt auf den Aufgaben, die mithilfe des Azure-Portals ausgeführt werden können. Verwenden Sie das Azure-Portal, um Aufträge und die Data Box zu verwalten sowie den Auftragsstatus bis zum Abschluss nachzuverfolgen.


## <a name="cancel-an-order"></a>Abbrechen eines Auftrags

Das Abbrechen eines erteilten Auftrags kann aus verschiedenen Gründen erforderlich sein. Sie können den Auftrag nur stornieren, bevor er bearbeitet wird. Sobald der Auftrag bearbeitet und die Data Box vorbereitet ist, ist es nicht mehr möglich, den Auftrag zu stornieren. 

Führen Sie die folgenden Schritte aus, um einen Auftrag zu stornieren.

1.  Navigieren Sie zu **Übersicht > Abbrechen**. 

    ![Abbrechen eines Auftrags (1)](media/data-box-portal-admin/cancel-order1.png)

2.  Geben Sie einen Grund für den Abbruch an.  

    ![Abbrechen eines Auftrags (2)](media/data-box-portal-admin/cancel-order2.png)

3.  Nach Abbruch des Auftrags aktualisiert das Portal den Status des Auftrags und zeigt **Abgebrochen** an. 

## <a name="clone-an-order"></a>Klonen eines Auftrags

Klonen kann in bestimmten Situationen hilfreich sein. Angenommen, ein Benutzer hat mithilfe von Data Box einige Daten übertragen. Bei zunehmender Datenmenge ist eine weitere Data Box erforderlich, um die Daten in Azure zu übertragen. In diesem Fall kann der gleiche Auftrag einfach geklont werden.

Führen Sie die folgenden Schritte aus, um einen Auftrag zu klonen.

1.  Navigieren Sie zu **Übersicht > Klonen**. 

    ![Klonen eines Auftrags (1)](media/data-box-portal-admin/clone-order1.png)

2.  Die Auftragsdetails bleiben unverändert. Als Auftragsname wird der Name des ursprünglichen Auftrags mit dem Zusatz *-Clone* verwendet. Aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie die Datenschutzinformationen gelesen haben. Klicken Sie auf **Create**.

Der Klon wird in wenigen Minuten erstellt und im Portal angezeigt.


## <a name="delete-order"></a>Löschen eines Auftrags

Abgeschlossene Aufträge können bei Bedarf gelöscht werden. Der Auftrag enthält Ihre persönlichen Informationen wie Name, Adresse und Kontaktinformationen. Diese persönlichen Informationen werden zusammen mit dem Auftrag gelöscht.

Sie können nur Aufträge löschen, die abgeschlossen sind oder abgebrochen wurden. Führen Sie die folgenden Schritte aus, um einen Auftrag zu löschen.

1. Wechseln Sie zu **All resources** (Alle Ressourcen). Suchen Sie nach Ihrem Auftrag.

2. Klicken Sie auf den Auftrag, den Sie löschen möchten, und navigieren zu **Übersicht**. Klicken Sie auf der Befehlsleiste auf **Löschen**.

    ![Löschen eines Data Box Disk-Auftrags (1)](media/data-box-portal-admin/delete-order1.png)

3. Geben Sie bei entsprechender Aufforderung den Namen des Auftrags ein, um die Auftragslöschung zu bestätigen. Klicken Sie auf **Löschen**.

## <a name="download-shipping-label"></a>Versandetikett herunterladen

Möglicherweise müssen Sie das Adressetikett herunterladen, wenn das E-Ink-Display Ihrer Data Box nicht funktioniert und das Rücksendeetikett nicht angezeigt wird. 

Führen Sie die folgenden Schritte aus, um ein Adressetikett herunterzuladen.

1.  Navigieren Sie zu **Übersicht > Versandetikett herunterladen**. Diese Option steht erst zur Verfügung, nachdem das Gerät versendet wurde. 

    ![Versandetikett herunterladen](media/data-box-portal-admin/download-shipping-label.png)

2.  Dadurch wird das folgende Rücksendeetikett heruntergeladen. Speichern Sie das Etikett, und drucken Sie es aus. Falten Sie das Etikett, und stecken Sie es in die Klarsichthülle am Gerät. Vergewissern Sie sich, dass das Etikett sichtbar ist. Entfernen Sie alle Etiketten, die vom vorherigen Versand noch auf dem Gerät vorhanden sind.

    ![Beispiel für Versandetikett](media/data-box-portal-admin/example-shipping-label.png)

## <a name="edit-shipping-address"></a>Bearbeiten der Lieferadresse

Unter Umständen müssen Sie nach Erteilung des Auftrags die Lieferadresse bearbeiten. Dies ist nur bis zum Versand des Geräts möglich. Nach dem Versand des Geräts ist diese Option nicht mehr verfügbar.

Führen Sie die folgenden Schritte aus, um den Auftrag zu bearbeiten.

1. Navigieren Sie zu **Auftragsdetails > Lieferadresse bearbeiten**.

    ![Bearbeiten der Lieferadresse (1)](media/data-box-portal-admin/edit-shipping-address1.png)

2. Bearbeiten und bestätigen Sie die Lieferadresse, und speichern Sie die Änderungen.

    ![Bearbeiten der Lieferadresse (2)](media/data-box-portal-admin/edit-shipping-address2.png)

## <a name="edit-notification-details"></a>Bearbeiten der Benachrichtigungsdetails

Unter Umständen müssen Sie die Benutzer ändern, die die E-Mails zum Auftragsstatus erhalten sollen. So muss beispielsweise ein Benutzer informiert werden, wenn das Gerät geliefert oder abgeholt wird. Ein anderer Benutzer muss ggf. informiert werden, wenn der Datenkopiervorgang abgeschlossen ist, damit er sich vor dem Löschen aus der Quelle vergewissern kann, dass sich die Daten im Azure-Speicherkonto befinden. In diesen Fällen können Sie die Benachrichtigungsdetails bearbeiten.

Führen Sie die folgenden Schritte aus, um die Benachrichtigungsdetails zu bearbeiten.

1. Navigieren Sie zu **Auftragsdetails > Benachrichtigungsdetails bearbeiten**.

    ![Bearbeiten der Benachrichtigungsdetails (1)](media/data-box-portal-admin/edit-notification-details1.png)

2. Nun können Sie die Benachrichtigungsdetails bearbeiten und die Änderungen speichern.
 
    ![Bearbeiten der Benachrichtigungsdetails (2)](media/data-box-portal-admin/edit-notification-details2.png)


## <a name="download-order-history"></a>Herunterladen des Auftragsverlaufs

Nach Abschluss des Data Box-Auftrags werden die Daten auf den Gerätedatenträgern gelöscht. Ist die Gerätebereinigung abgeschlossen, können Sie den Auftragsverlauf im Azure-Portal herunterladen.

Führen Sie die folgenden Schritte aus, um den Auftragsverlauf herunterzuladen:

1. Navigieren Sie in Ihrem Data Box-Auftrag zu **Übersicht**. Vergewissern Sie sich, dass der Auftrag abgeschlossen ist. Sind Auftrag und Gerätebereinigung abgeschlossen, navigieren Sie zu **Auftragsdetails**. Die Option **Bestellverlauf herunterladen** ist verfügbar.

    ![Herunterladen des Auftragsverlaufs](media/data-box-portal-admin/download-order-history-1.png)

2. Klicken Sie auf **Bestellverlauf herunterladen**. Im heruntergeladenen Verlauf sehen Sie einen Eintrag für die Sendungsverfolgungsprotokolle des Zustelldiensts. Unten in diesem Protokoll finden Sie Links zu den folgenden Elementen:
    
    - **Kopierprotokolle:** Diese Protokolle enthalten die Liste der Dateien, bei denen während des Kopierens von Daten von Data Box in Ihr Azure-Speicherkonto Fehler aufgetreten sind.
    - **Überwachungsprotokolle:** Diese Protokolle enthalten Informationen zum Einschalten und zum Zugriff auf Data Box, wenn sich der Dienst außerhalb des Azure-Rechenzentrums befindet.
    - **BOM-Dateien:** Diese Dateien enthalten die Dateiliste (auch als Dateimanifest bezeichnet), die Sie bei der **Versandvorbereitung** herunterladen können und die die Dateinamen, Dateigrößen und Dateiprüfsummen enthält.

        ```
        -------------------------------
        Microsoft Data Box Order Report
        -------------------------------
        
        Name                                               : eastusdryrun                                      
        StartTime(UTC)                                     : 9/6/2018 12:54:47 PM +00:00                       
        DeviceType                                         : ImolaPod                                          
        
        -------------------
        Data Box Activities
        -------------------
        
        Time(UTC)             | Activity                       | Status          | Description                                                                                                                                           
        
        9/6/2018 12:54:51 PM  | OrderCreated         | Completed  |                                                                                                                              
        9/11/2018 8:57:38 PM  | DevicePrepared       | Completed  |                                                                                                                                                       
        9/12/2018 7:28:15 PM  | ShippingToCustomer   | InProgress | Pickup Scan. Local Time : 9/12/2018 2:52:31 PM at Chantilly                                                                                           
        9/13/2018 2:33:04 AM  | ShippingToCustomer   | InProgress | Departure Scan. Local Time : 9/12/2018 9:00:00 PM at Chantilly                                                                                                                                                                                                                                                              
        9/13/2018 12:40:31 PM | ShippingToCustomer   | InProgress | Arrival Scan. Local Time : 9/13/2018 5:00:00 AM at Oakland                                                                                            
        9/13/2018 2:42:10 PM  | ShippingToCustomer   | InProgress | Departure Scan. Local Time : 9/13/2018 6:08:00 AM at Oakland                                                                                          
        9/13/2018 3:42:12 PM  | ShippingToCustomer   | InProgress | Destination Scan. Local Time : 9/13/2018 8:14:08 AM at Sunnyvale                                                                                      
        9/13/2018 4:43:05 PM  | ShippingToCustomer   | InProgress | Destination Scan. Local Time : 9/13/2018 8:56:54 AM at Sunnyvale                                                                                      
        9/13/2018 4:43:05 PM  | ShippingToCustomer   | InProgress | Out For Delivery Today. Local Time : 9/13/2018 9:11:21 AM at Sunnyvale                                                                                
        9/13/2018 5:43:07 PM  | ShippingToCustomer   | Completed  | Delivered. Local Time : 9/13/2018 9:44:17 AM at SUNNYVALE                                                                                             
        9/14/2018 11:48:35 PM | ShippingToDataCenter | InProgress | Pickup Scan. Local Time : 9/14/2018 3:55:37 PM at Sunnyvale                                                                                                                                                                                 
        9/15/2018 1:52:35 AM  | ShippingToDataCenter | InProgress | Arrival Scan. Local Time : 9/14/2018 6:31:00 PM at San Jose                                                                                           
        9/15/2018 2:52:39 AM  | ShippingToDataCenter | InProgress | Departure Scan. Local Time : 9/14/2018 7:17:00 PM at San Jose                                                                                                                                                                             
        9/17/2018 8:23:31 AM  | ShippingToDataCenter | InProgress | Destination Scan. Local Time : 9/17/2018 4:14:37 AM at Chantilly                                                                                      
        9/17/2018 12:24:42 PM | ShippingToDataCenter | InProgress | Loaded on Delivery Vehicle. Local Time : 9/17/2018 7:45:36 AM at Chantilly                                                                            
        9/17/2018 1:25:11 PM  | ShippingToDataCenter | InProgress | Out For Delivery Today. Local Time : 9/17/2018 8:27:11 AM at Chantilly                                                                                
        9/17/2018 2:25:51 PM  | ShippingToDataCenter | Completed | Delivered. Local Time : 9/17/2018 9:56:32 AM at STERLING                                                                                              
        9/18/2018 9:55:41 PM  | DeviceBoot           | Completed | Appliance booted up successfully                                                                                                                      
        9/18/2018 11:00:25 PM | DataCopy             | Started   |                                                                                                                                                       
        9/18/2018 11:01:33 PM | DataCopy             | Completed | Copy Completed.                                                                                                                                       
        9/18/2018 11:20:58 PM | SecureErase          | Started   |                                                                                                                                                       
        9/18/2018 11:28:46 PM | SecureErase          | Completed | Azure Data Box:BY506B4B616700 has been sanitized according to NIST 800 -88 Rev 1.                                                                     
        
        ----------------------
        Data Box Job Log Links
        ----------------------
        
        Account Name         : eastusdryrun                                         
        Copy Logs Path       : copylog/copylogd695869a2a294396b7b903296c208388.xml                                                                                                                                                     
        Audit Logs Path      : azuredatabox-chainofcustodylogs\3b4cf163-f1af-475c-a391-f8afea3fa327\by506b4b616700                                                                                                                     
        BOM Files Path       : azuredatabox-chainofcustodylogs\3b4cf163-f1af-475c-a391-f8afea3fa327\by506b4b616700
        ```
Anschließend können Sie zu Ihrem Speicherkonto navigieren und die Kopierprotokolle anzeigen.

![Protokolle in Speicherkonten](media/data-box-portal-admin/logs-in-storage-acct-2.png)

Sie können auch die Protokolle zur Rückverfolgbarkeit anzeigen, zu denen die Überwachungsprotokolle und die BOM-Dateien zählen.

![Protokolle in Speicherkonten](media/data-box-portal-admin/logs-in-storage-acct-1.png)

## <a name="view-order-status"></a>Anzeigen des Auftragsstatus

Sobald sich der Gerätestatus im Portal ändert, werden Sie per E-Mail benachrichtigt.

|Auftragsstatus |BESCHREIBUNG |
|---------|---------|
|Bestellt     | Der Auftrag wurde erfolgreich erteilt. <br>Wenn das Gerät verfügbar ist, bestimmt Microsoft das Gerät für den Versand und bereitet es darauf vor. <br> Wenn das Gerät nicht sofort verfügbar ist, wird der Auftrag bearbeitet, sobald das Gerät verfügbar ist. Die Auftragsbearbeitung kann von mehreren Tagen bis zu einigen Monaten dauern. Wenn der Auftrag nicht binnen 90 Tagen erfüllt werden kann, wird er storniert, worüber Sie benachrichtigt werden.         |
|Verarbeitet     | Die Auftragsverarbeitung ist abgeschlossen. Gemäß Ihrem Auftrag wird das Gerät für den Versand im Rechenzentrum vorbereitet.         |
|Versandt     | Der Auftrag wurde versandt. Verwenden Sie die in Ihrem Auftrag im Portal angezeigte Sendungsverfolgungs-ID, um die Sendung zu verfolgen.        |
|Geliefert     | Der Auftrag wurde an die im Auftrag angegebene Adresse geliefert.        |
|Abgeholt     |Ihre Rücksendung wurde vom Zusteller abgeholt und gescannt.         |
|Empfangen     | Ihr Gerät wurde im Azure-Rechenzentrum empfangen und gescannt. <br> Nach Überprüfung der Sendung beginnt das Hochladen der Daten auf dem Gerät.      |
|Daten kopieren     | Die Daten werden gerade kopiert. Verfolgen Sie den Kopierfortschritt Ihres Auftrags im Azure-Portal. <br> Warten Sie, bis der Datenkopiervorgang abgeschlossen ist. |
|Abgeschlossen       |Der Auftrag wurde erfolgreich abgeschlossen.<br> Vergewissern Sie sich, dass sich Ihre Daten in Azure befinden, bevor Sie die lokalen Daten von Servern löschen.         |
|Mit Fehlern abgeschlossen| Die Datenkopie wurde abgeschlossen, aber es sind Fehler beim Kopiervorgang aufgetreten. <br> Überprüfen Sie im Azure-Portal im angegebenen Pfad die Kopierprotokolle.   |
|Canceled            |Der Auftrag wurde abgebrochen. <br> Entweder haben Sie den Auftrag abgebrochen, oder es ist ein Fehler aufgetreten, und der Auftrag wurde durch den Dienst abgebrochen. Wenn der Auftrag nicht binnen 90 Tagen erfüllt werden kann, wird er storniert, und Sie werden benachrichtigt.     |
|Bereinigen | Die Daten auf den Datenträgern des Geräts werden gelöscht. Die Gerätebereinigung gilt als abgeschlossen, wenn der Auftragsverlauf im Azure-Portal zum Download zur Verfügung steht.|



## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Behandeln von Data Box-Problemen](data-box-faq.md).
