---
title: 'Tutorial: Installieren des physischen Azure Data Box Edge-Geräts | Microsoft-Dokumentation'
description: Das zweite Tutorial zur Installation von Azure Data Box Edge enthält Informationen zum Auspacken, zur Rackmontage und zur Verkabelung des physischen Geräts.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 21ac3de793f5ce559c3a03de2a09f11ccb86b12a
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167357"
---
# <a name="tutorial-install-azure-data-box-edge-preview"></a>Tutorial: Installieren von Azure Data Box Edge (Vorschauversion)

In diesem Tutorial erfahren Sie, wie Sie ein physisches Data Box Edge-Gerät installieren. Die Installation umfasst das Auspacken, die Rackmontage und die Verkabelung des Geräts. 

Die Installation dauert etwa zwei Stunden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Auspacken des Geräts
> * Montieren des Geräts in einem Rack
> * Verkabeln des Geräts

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Installieren eines physischen Geräts müssen folgende Anforderungen erfüllt werden:

### <a name="for-the-data-box-edge-resource"></a>Für die Data Box Edge-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie haben alle Schritte aus dem [Tutorial zum Vorbereiten des Portals für Data Box Edge](data-box-edge-deploy-prep.md) ausgeführt.
    * Sie haben die Data Box Edge-Ressource für die Bereitstellung Ihres Geräts erstellt.
    * Sie haben den Aktivierungsschlüssel zum Aktivieren Ihres Geräts mit der Data Box Edge-Ressource erstellt.

 
### <a name="for-the-data-box-edge-physical-device"></a>Für das physische Data Box Edge-Gerät

Führen Sie folgende Schritte aus, bevor Sie ein Gerät bereitstellen:

- Vergewissern Sie sich, dass das Gerät sicher auf einer flachen, stabilen und ebenen Arbeitsfläche (oder ähnlich) liegt.
- Vergewissern Sie sich, dass der geplante Aufstellungsort über Folgendes verfügt:
    - Standardmäßige unabhängige Wechselstromversorgung. Oder:
    - Rack-PDU (Power Distribution Unit) mit unterbrechungsfreier Stromversorgung (USV).
- Vergewissern Sie sich, dass das Rack, in das Sie das Gerät einbauen möchten, Platz für einen Einschub mit 1 HE bietet.

### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Datencenter

Vorbereitungen

- Lesen Sie die Netzwerkanforderungen für die Bereitstellung einer Data Box Edge-Lösung, und konfigurieren Sie das Netzwerk des Datencenters gemäß den Anforderungen. Weitere Informationen finden Sie unter [Netzwerkanforderungen](data-box-gateway-system-requirements.md#networking-requirements).
- Stellen Sie sicher, dass die Internetbandbreite mindestens 20 MBit/s beträgt, um die optimale Nutzung des Geräts zu ermöglichen.


## <a name="unpack-the-device"></a>Auspacken des Geräts

Dieses Gerät wird in einem einzelnen Karton ausgeliefert. Führen Sie die folgenden Schritte aus, um das Gerät auszupacken. 

1. Legen Sie den Karton auf einen flachen, ebenen Untergrund.
2. Überprüfen Sie den Karton und das Verpackungsmaterial auf Risse, Schnitte, Wasserschäden oder anderweitige offensichtliche Beschädigungen. Wenn der Karton oder die Verpackung stark beschädigt sind, öffnen Sie den Karton nicht. Wenden Sie sich an den Microsoft-Support, um zu ermitteln, ob das Gerät funktionstüchtig ist.
3. Packen Sie den Karton aus. Stellen Sie nach dem Auspacken des Kartons sicher, dass Folgendes vorhanden ist:
    - Ein Edgegerät (einzelnes Gehäuse)
    - Zwei Netzkabel
    - Ein ohne Werkzeug montierbares Kit für die Rackmontage (einschließlich zwei Seitenschienen und Montagematerial)
4. Sollten Sie eines der oben aufgeführten Teile nicht erhalten haben, wenden Sie sich an den Data Box Edge-Support. Im nächsten Schritt bauen Sie das Gerät in ein Rack ein.


## <a name="rack-the-device"></a>Montieren des Geräts in einem Rack

Das Gerät muss in einem standardmäßigen 19-Zoll-Rack installiert werden. Gehen Sie wie folgt vor, um Ihr Gerät in einem standardmäßigen 19-Zoll-Rack mit Ständern an der Vorder- und Rückseite zu montieren.

> [!IMPORTANT]
> Data Box Edge-Geräte müssen für den ordnungsgemäßen Betrieb in ein Rack eingebaut werden.


1. Ziehen Sie an der vorderen Entriegelung der Schienenbaugruppe, um die innere Schiene zu lösen. Lösen Sie die Arretierung, und schieben Sie die mittlere Schiene nach innen, um die Schiene einzufahren. Die innere und äußere Schiene sollten nun getrennt sein.

    ![Installieren der Schienen für die Rackmontage](./media/data-box-edge-deploy-install/rack-mount-rail-1.png)

2. Installieren Sie nun die äußeren Schienen an den vertikalen Blechwinkeln des Rackschranks. Die Gleitschienen sind jeweils mit „Front“ (Vorne) gekennzeichnet. Diese Seite muss an der Vorderseite des Gehäuses befestigt werden. 
    
    1. Suchen Sie die Fixierungsstifte, die sich vorne und hinten an der Schienenbaugruppe befinden. Fahren Sie die Schiene so weit aus, dass sie zwischen die Rackständer passt. Befestigen Sie zuerst die äußere Schiene an der Rückseite des Racks. Positionieren Sie die hintere Befestigungsklammer auf der Innenseite der hinteren Befestigungslöchern des Racks.   
    2. Halten Sie den Auslöser an der hinteren Klammer gedrückt, um die Metallhaken auszufahren. Richten Sie die Schiene korrekt aus, führen Sie die Metallhaken in die Befestigungslöcher ein, und lassen Sie anschließend den Auslöser los.
    3. Richten Sie die vordere Klammer an den Befestigungslöchern aus.
    4. Die vordere Klammer sollte nun am Rack befestigt sein. Bei Bedarf können die Schienen mit Schrauben vom Typ „M5 X 10L“ an den Ständern befestigt werden. 

    ![Installieren der Schienen für die Rackmontage](./media/data-box-edge-deploy-install/rack-mount-rail-2.png)

3. Wenn Sie die innere Schiene am Gehäuse befestigen, müssen die Schlüssellochöffnungen der inneren Schiene an den Fixierstiften an der Gehäuseseite ausgerichtet werden. Stellen Sie sicher, dass die Köpfe der Fixierstifte des Gehäuses durch die Schlüssellochöffnungen der inneren Schiene ragen. Ziehen Sie die Schiene in Richtung Gehäusefront, bis die Schiene hörbar einrastet. Wiederholen Sie den Vorgang mit der anderen inneren Schiene. Schieben Sie das Gehäuse mit der inneren Schiene in den Schlitten, um die Rackmontage abzuschließen.

    ![Installieren der Schienen für die Rackmontage](./media/data-box-edge-deploy-install/rack-mount-rail-3.png)

## <a name="cable-the-device"></a>Verkabeln des Geräts

In diesem Abschnitt erfahren Sie, wie Sie Ihr Edgegerät an die Stromversorgung und an das Netzwerk anschließen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit dem Verkabeln des Geräts beginnen können, benötigen Sie Folgendes:

- Das physische Edgegerät (ausgepackt und im Rack montiert)
- Zwei Netzkabel 
- Zwei Netzwerkkabel (1 GbE, RJ45) und vier Kupferkabel (25 GbE, SFP+)
- Zugang zu zwei PDUs (Power Distribution Units) (empfohlen)

Ihr Edgegerät verfügt über acht NVMe-SSDs. Am vorderen Bedienfeld befinden sich außerdem Status-LEDs und Netzschalter. An der Rückseite des Geräts befinden sich redundante Netzteile (Power Supply Units, PSUs). Ihr Gerät verfügt über sechs Netzwerkschnittstellen: zwei mit 1 GBit/s und vier mit 25 GBit/s. Ihr Gerät ist mit einem Baseboard-Verwaltungscontroller (Baseboard Management Controller, BMC) ausgestattet. Machen Sie sich mit den verschiedenen Anschlüssen an der Geräterückseite vertraut.
 
  ![Rückseite eines verkabelten Geräts](./media/data-box-edge-deploy-install/backplane-cabled.png)
 
Gehen Sie wie folgt vor, um Ihr Gerät an die Stromversorgung und an das Netzwerk anzuschließen.

1. Schließen Sie die Netzkabel an die PSUs des Gehäuses an. Zur Gewährleistung hoher Verfügbarkeit müssen beide PSUs installiert und an unterschiedliche Stromquellen angeschlossen werden.
2. Schließen Sie die Netzkabel an die PDUs des Racks an. Stellen Sie sicher, dass die beiden PSUs separate Stromquellen verwenden.
3. Verbinden Sie die 1-GbE-Netzwerkschnittstelle „PORT 1“ mit dem Computer, der zum Konfigurieren des physischen Geräts verwendet wird. „PORT 1“ ist die dedizierte Verwaltungsschnittstelle.
4. Verbinden Sie die 1-GbE-Netzwerkschnittstelle „PORT 2“ per RJ45-Netzwerkkabel mit dem Datencenternetzwerk/Internet. 
5. Verbinden Sie die vier 25-GbE-Netzwerkschnittstellen „PORT 3“, „PORT 4“, „PORT 5“ und „PORT 6“ per SFP+-Kupferkabel mit dem Datencenternetzwerk/Internet. 

> [!NOTE]
> - Mindestens eine Datennetzwerkschnittstelle („PORT 2“, „PORT 3“, „PORT 4“, „PORT 5“ oder PORT 6“) muss mit dem Internet (Azure-Konnektivität) verbunden sein. 
> - Wir empfehlen, eine 25-GbE-Netzwerkschnittstelle wie „PORT 3“, „PORT 4“, „PORT 5“ oder „PORT 6“ zu verwenden, um Daten an Azure zu senden. 
> - Das Edgegerät sollte mit dem Datencenternetzwerk verbunden sein, um Daten von Datenquellservern erfassen zu können.  


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Auspacken des Geräts
> * Montieren des Geräts in einem Rack
> * Verkabeln des Geräts

Im nächsten Tutorial erfahren Sie, wie Sie Ihr Gerät verbinden, einrichten und aktivieren.

> [!div class="nextstepaction"]
> [Verbinden und Einrichten Ihrer Data Box Edge-Lösung](./data-box-edge-deploy-connect-setup-activate.md)


