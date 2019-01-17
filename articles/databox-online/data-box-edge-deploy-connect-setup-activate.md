---
title: Verbinden, Konfigurieren und Aktivieren eines Azure Data Box Edge-Geräts im Azure-Portal | Microsoft-Dokumentation
description: Im dritten Tutorial zur Bereitstellung von Data Box Edge erfahren Sie, wie Sie Ihr physisches Gerät verbinden, einrichten und aktivieren können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 01/09/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: e5477807e4cff6dc745ccfd0383fd216719a7aa5
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259611"
---
# <a name="tutorial-connect-set-up-and-activate-azure-data-box-edge-preview"></a>Tutorial: Verbinden, Einrichten und Aktivieren von Azure Data Box Edge (Vorschauversion) 

In diesem Tutorial wird die Verbindung, Einrichtung und Aktivierung Ihres Azure Data Box Edge-Geräts über die lokale Webbenutzeroberfläche beschrieben. 

Der Einrichtungs- und Aktivierungsvorgang nimmt ungefähr 20 Minuten in Anspruch. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit einem physischen Gerät
> * Einrichten und Aktivieren des physischen Geräts

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 


## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie ein Data Box Edge-Gerät konfigurieren und einrichten:

* Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Installieren von Data Box Edge](data-box-edge-deploy-install.md) installiert.
* Sie verfügen über den Aktivierungsschlüssel vom Data Box Edge-Dienst, den Sie zum Verwalten von Data Box Edge-Geräten erstellt haben. Weitere Informationen finden Sie unter [Vorbereiten der Bereitstellung von Azure Data Box Edge](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Herstellen einer Verbindung mit der lokalen Webbenutzeroberfläche 

1. Konfigurieren Sie den Ethernet-Adapter auf Ihrem Computer, um das Edge-Gerät mit der statischen IP-Adresse 192.168.100.5 und dem Subnetz 255.255.255.0 zu verbinden.

1. Verbinden Sie den Computer mit PORT 1 auf Ihrem Gerät. 

1. Öffnen Sie ein Browserfenster, und greifen Sie unter https://192.168.100.10 auf die lokale Webbenutzeroberfläche des Geräts zu.  
    Diese Aktion dauert einige Minuten, nachdem Sie das Gerät aktiviert haben. 

    Es wird eine Fehlermeldung oder eine Warnung mit dem Hinweis angezeigt, dass ein Problem mit dem Sicherheitszertifikat der Website vorliegt. 
   
    ![Meldung zum Sicherheitszertifikatfehler auf der Website](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

1. Klicken Sie auf **Mit dieser Webseite fortfahren**.  
    Diese Schritte können sich je nach verwendetem Browser unterscheiden.

1. Melden Sie sich bei der Webbenutzeroberfläche des Geräts an. Das Standardkennwort lautet *Password1*. 
   
    ![Anmeldeseite für Data Box Edge-Gerät](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

1. Ändern Sie das Geräteadministratorkennwort in der Eingabeaufforderung.  
    Das neue Kennwort muss zwischen 8 und 16 Zeichen haben. Es muss drei der folgenden Zeichen enthalten: Großbuchstaben, Kleinbuchstaben, Ziffern und Sonderzeichen.

Sie befinden sich jetzt im Dashboard Ihres Geräts.

## <a name="set-up-and-activate-the-physical-device"></a>Einrichten und Aktivieren des physischen Geräts
 
Das Dashboard zeigt die verschiedenen Einstellungen an, die zum Konfigurieren und Registrieren des physischen Geräts beim Data Box Edge-Dienst erforderlich sind. Der **Gerätename**, die **Netzwerkeinstellungen**, die **Webproxyeinstellungen** und die **Uhrzeiteinstellungen** sind optional. Die einzigen erforderlichen Einstellungen sind **Cloudeinstellungen**.
   
![Das Dashboard für das Data Box Edge-Gerät](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. Klicken Sie im linken Bereich auf **Gerätename**, und geben Sie anschließend einen Anzeigenamen für Ihr Gerät ein.  
    Der Anzeigename muss aus 1 bis 15 Zeichen bestehen und Buchstaben, Zahlen und Bindestriche enthalten.

    ![Seite „Gerätename“](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

1. (Optional) Klicken Sie im linken Bereich auf **Netzwerkeinstellungen**, und konfigurieren Sie die Einstellungen anschließend.  
    Auf dem physischen Gerät sind sechs Netzwerkschnittstellen vorhanden. PORT 1 und PORT 2 sind 1-Gbit/s-Netzwerkschnittstellen. PORT 3, PORT 4, PORT 5 und PORT 6 sind alle 25-Gbit/s-Netzwerkschnittstellen. PORT 1 wird automatisch als reiner Verwaltungsport konfiguriert, während es sich bei PORT 2 bis PORT 6 um Datenports handelt. Die Seite **Netzwerkeinstellungen** wie unten gezeigt.
    
    ![Seite „Netzwerkeinstellungen“](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Beachten Sie Folgendes, wenn Sie Netzwerkeinstellungen konfigurieren:

    - Falls DHCP in Ihrer Umgebung aktiviert ist, werden Netzwerkschnittstellen automatisch konfiguriert. IP-Adresse, Subnetz, Gateway und DNS werden automatisch zugewiesen.
    - Wenn DHCP nicht aktiviert ist, können Sie bei Bedarf statische IP-Adressen zuweisen.
    - Sie können die Netzwerkschnittstelle als IPv4 konfigurieren.

    >[!NOTE] 
    > Wir empfehlen, die lokale IP-Adresse der Netzwerkschnittstelle nicht von statisch auf DCHP umzustellen, es sei denn, Sie haben eine andere IP-Adresse für die Verbindung zum Gerät. Wenn Sie eine Netzwerkschnittstelle verwenden und zu DHCP wechseln, gibt es keine Möglichkeit, die DHCP-Adresse zu bestimmen. Wenn Sie zu einer DHCP-Adresse wechseln möchten, warten Sie, bis sich das Gerät beim Dienst registriert hat, und ändern Sie dann. Sie können dann die IPs aller Adpater in den **Geräteeigenschaften** im Azure-Portal für Ihren Dienst anzeigen.

1. (Optional) Klicken Sie im linken Bereich auf **Webproxyeinstellungen**, und konfigurieren Sie anschließend den Webproxyserver. Die Webproxykonfiguration ist optional. Bei Verwendung eines Webproxys kann dieser jedoch nur auf dieser Seite konfiguriert werden.
   
   ![Seite „Webproxyeinstellungen“](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   Gehen Sie auf der Seite **Webproxyeinstellungen** folgendermaßen vor:
   
   a. Geben Sie im Feld **Webproxy-URL** die URL im folgenden Format ein: `http://host-IP address or FQDN:Port number`. HTTPS-URLs werden nicht unterstützt.

   b. Klicken Sie unter **Authentifizierung** auf **Keine** oder **NTLM**.

   c. Falls Sie eine Authentifizierung verwenden, geben Sie einen Benutzernamen und ein Kennwort ein.

   d. Klicken Sie auf **Einstellungen anwenden**, um die konfigurierten Webproxyeinstellungen zu überprüfen und anzuwenden.

1. (Optional) Klicken Sie im linken Bereich auf **Zeiteinstellungen**, und konfigurieren Sie die Zeitzone und die primären und sekundären NTP-Server für das Gerät.  
    NTP-Server sind für die Zeitsynchronisierung erforderlich, damit Ihr Gerät bei den Clouddienstanbietern authentifiziert werden kann.
    
    ![Seite „Uhrzeiteinstellungen“](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)
    
    Gehen Sie auf der Seite **Zeiteinstellungen** folgendermaßen vor:
    
    a. Wählen Sie aus der Dropdownliste die **Zeitzone** aus, die dem geografischen Standort entspricht, an dem das Gerät bereitgestellt wird.  
        Die Standardzeitzone für Ihr Gerät ist „PST“. Ihr Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.

    b. Geben Sie im Feld **Primärer NTP-Server** den primären Server für das Gerät ein, oder übernehmen Sie den Standardwert „time.windows.com“.  
        Stellen Sie sicher, dass Ihr Netzwerk NTP-Datenverkehr vom Rechenzentrum ins Internet zulässt.

    c. Geben Sie optional im Feld **Sekundärer NTP-Server** einen sekundären Server für Ihr Gerät ein.

    d. Klicken Sie auf **Übernehmen**, um die konfigurierten Uhrzeiteinstellungen zu überprüfen und anzuwenden.

6. Klicken Sie im linken Bereich auf **Cloudeinstellungen**, und aktivieren Sie Ihr Gerät im Azure-Portal für den Data Box Edge-Dienst.
    
    a. Geben Sie im Feld **Aktivierungsschlüssel** den Aktivierungsschlüssel ein, den Sie unter [Aktivierungsschlüssel abrufen](data-box-edge-deploy-prep.md#get-the-activation-key) für Data Box Edge abgerufen haben.

    b. Wählen Sie **Übernehmen**. 
       
    ![Seite „Cloudeinstellungen“](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)
    
    Nachdem das Gerät erfolgreich aktiviert wurde, werden Optionen für den Verbindungsmodus angezeigt. Diese Einstellungen werden konfiguriert, wenn Sie mit dem Gerät im teilweise nicht verbundenen Modus oder im getrennten Modus arbeiten müssen. 

    ![Bestätigung für die Aktivierung der Cloudeinstellungen](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)    

Die Einrichtung des Geräts ist abgeschlossen. Sie können nun Freigaben auf Ihrem Gerät hinzufügen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit einem physischen Gerät
> * Einrichten und Aktivieren des physischen Geräts


Weitere Informationen zum Übertragen von Daten mit Data Box Edge finden Sie unter:

> [!div class="nextstepaction"]
> [Übertragen von Daten mit Data Box Edge](./data-box-edge-deploy-add-shares.md).
