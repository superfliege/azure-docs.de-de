---
title: Verbinden mit und Konfigurieren und Aktivieren von Azure Data Box Edge im Azure-Portal | Microsoft-Dokumentation
description: Im dritten Tutorial zur Bereitstellung von Data Box Edge erfahren Sie, wie Sie Ihr physisches Gerät verbinden, einrichten und aktivieren können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 3d6742bec54e612b5dca6d9ef6c4f67a33929448
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166575"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-edge-preview"></a>Tutorial: Verbinden, Einrichten und Aktivieren von Azure Data Box Edge (Vorschau) 

In diesem Tutorial wird beschrieben, wie Sie sich mit Ihrem Data Box Edge-Gerät über die lokale Webbenutzeroberfläche verbinden und wie Sie es einrichten und aktivieren. 

Der Einrichtungs- und Aktivierungsvorgang nimmt ungefähr 20 Minuten in Anspruch. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit dem physischen Gerät
> * Einrichten und Aktivieren des physischen Geräts

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 


## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie Data Box Edge konfigurieren und einrichten:

* Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Installieren von Data Box Edge](data-box-edge-deploy-install.md) installiert.
* Sie verfügen über den Aktivierungsschlüssel vom Data Box Edge-Dienst, den Sie zum Verwalten von Data Box Edge-Geräten erstellt haben. Weitere Informationen finden Sie unter [Vorbereiten der Bereitstellung von Azure Data Box Edge](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Herstellen einer Verbindung mit der lokalen Webbenutzeroberfläche 

1. Konfigurieren Sie den Ethernet-Adapter auf dem Computer, über den Sie eine Verbindung mit dem Edge-Gerät herstellen, mit der statischen IP-Adresse 192.168.100.5 und dem Subnetz 255.255.255.0.
2. Verbinden Sie den Computer mit PORT 1 auf Ihrem Gerät. 
3. Öffnen Sie ein Browserfenster, und greifen Sie unter https://192.168.100.10 auf die lokale Webbenutzeroberfläche des Geräts zu. Diese Aktion dauert einige Minuten, nachdem Sie das Gerät aktiviert haben. 
4. Es wird eine Fehlermeldung oder eine Warnung mit dem Hinweis angezeigt, dass ein Problem mit dem Sicherheitszertifikat der Website vorliegt. Klicken Sie auf **Mit dieser Webseite fortfahren**. (Diese Schritte können je nach Browser unterschiedlich sein.)
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

2. Melden Sie sich bei der Webbenutzeroberfläche des Geräts an. Das Standardkennwort lautet *Password1*. 
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

3. Sie werden aufgefordert, das Geräteadministratorkennwort zu ändern. Geben Sie ein neues Kennwort mit 8 bis 16 Zeichen ein. Das Kennwort muss drei der folgenden Zeichen enthalten: Großbuchstaben, Kleinbuchstaben, Ziffern und Sonderzeichen.

Sie befinden sich jetzt im **Dashboard** Ihres Geräts.

## <a name="set-up-and-activate-the-physical-device"></a>Einrichten und Aktivieren des physischen Geräts
 
1. Im Dashboard können Sie zu den verschiedenen Einstellungen navigieren, die zum Konfigurieren und Registrieren des physischen Geräts beim Data Box Edge-Dienst erforderlich sind. Der **Gerätename**, die **Netzwerkeinstellungen**, die **Webproxyeinstellungen** und die **Uhrzeiteinstellungen** sind optional. Die einzigen erforderlichen Einstellungen sind **Cloudeinstellungen**.
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

2. Geben Sie auf der Seite **Gerätename** einen Anzeigenamen für Ihr Gerät an. Der Anzeigename kann 1 bis 15 Zeichen lang sein und Buchstaben, Ziffern und Bindestriche enthalten.

    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

3. (Optional) Konfigurieren Sie Ihre **Netzwerkeinstellungen**. Auf dem physischen Gerät werden sechs Netzwerkschnittstellen angezeigt. PORT 1 und PORT 2 sind 1-Gbit/s-Netzwerkschnittstellen. PORT 3, PORT 4, PORT 5 und PORT 6 sind alle 25-Gbit/s-Netzwerkschnittstellen. PORT 1 wird automatisch als reiner Verwaltungsport konfiguriert, während PORT 2 bis PORT 6 alle Datenports sind. Die Seite **Netzwerkeinstellungen** wie unten gezeigt.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Denken Sie an Folgendes, wenn Sie Netzwerkeinstellungen konfigurieren:

    - Falls DHCP in Ihrer Umgebung aktiviert ist, werden Netzwerkschnittstellen automatisch konfiguriert. IP-Adresse, Subnetz, Gateway und DNS werden automatisch zugewiesen.
    - Wenn DHCP nicht aktiviert ist, können Sie bei Bedarf statische IP-Adressen zuweisen.
    - Sie können die Netzwerkschnittstelle als IPv4 konfigurieren.
   
4. Optional: Konfigurieren Sie Ihren Webproxyserver. Die Webproxykonfiguration ist optional. Bei Verwendung eines Webproxys kann dieser jedoch nur hier konfiguriert werden.
   
   ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   Auf der Seite **Webproxy** :
   
   1. Geben Sie die **Webproxy-URL** im folgenden Format an: `http://host-IP address or FDQN:Port number`. HTTPS-URLs werden nicht unterstützt.
   2. Geben Sie unter **Authentifizierung** die Option **Einfach** oder **Keine** an.
   3. Wenn Sie die Authentifizierung verwenden, müssen Sie auch einen **Benutzernamen** und ein **Kennwort** angeben.
   4. Klicken Sie auf **Übernehmen**, um die konfigurierten Webproxyeinstellungen zu überprüfen und anzuwenden.

5. Optional: Konfigurieren Sie die Zeiteinstellungen für Ihr Gerät, z. B. die Zeitzone und die primären und sekundären NTP-Server. NTP-Server sind für die Zeitsynchronisierung erforderlich, damit Ihr Gerät bei den Clouddienstanbietern authentifiziert werden kann.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)
    
    Auf der Seite **Uhrzeiteinstellungen** :
    
    1. Legen Sie über die Dropdownliste die **Zeitzone** basierend auf dem geografischen Standort fest, an dem das Gerät bereitgestellt wird. Die Standardzeitzone für Ihr Gerät ist „PST“. Ihr Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.
    2. Geben Sie einen **primären NTP-Server** für das Gerät an, oder übernehmen Sie den Standardwert „time.windows.com“. Stellen Sie sicher, dass Ihr Netzwerk NTP-Datenverkehr vom Rechenzentrum ins Internet zulässt.
    3. Geben Sie optional einen **sekundären NTP-Server** für Ihr Gerät an.
    4. Klicken Sie auf **Übernehmen**, um die konfigurierten Uhrzeiteinstellungen zu überprüfen und anzuwenden.

6. Aktivieren Sie Ihr Gerät im Azure-Portal auf der Seite **Cloudeinstellungen** für den Data Box Edge-Dienst.
    
    1. Geben Sie den **Aktivierungsschlüssel** ein, den Sie in [Aktivierungsschlüssel abrufen](data-box-edge-deploy-prep.md#get-the-activation-key) für Data Box Edge abgerufen haben.

    2. Klicken Sie auf **Anwenden**. 
       
         ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)
    
    3. Nachdem das Gerät erfolgreich aktiviert wurde, werden Optionen für den Verbindungsmodus angezeigt. Diese Einstellungen werden konfiguriert, wenn Sie mit dem Gerät im teilweise nicht verbundenen Modus oder im getrennten Modus arbeiten müssen. 

        ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)    

Die Einrichtung des Geräts ist abgeschlossen. Sie können nun Freigaben auf Ihrem Gerät hinzufügen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit dem physischen Gerät
> * Einrichten und Aktivieren des physischen Geräts


Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Daten mit Data Box Edge übertragen.

> [!div class="nextstepaction"]
> [Übertragen von Daten mit Data Box Edge](./data-box-edge-deploy-add-shares.md).