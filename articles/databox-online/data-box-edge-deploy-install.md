---
title: 'Tutorial: Installieren eines physischen Azure Data Box Edge-Geräts | Microsoft-Dokumentation'
description: Das zweite Tutorial zur Installation von Azure Data Box Edge enthält Informationen zum Auspacken, zur Rackmontage und zur Verkabelung des physischen Geräts.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/01/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 243697c20f10a019a73326c3bd79e23fc3342640
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113599"
---
# <a name="tutorial-install-azure-data-box-edge-preview"></a>Tutorial: Installieren von Azure Data Box Edge (Vorschauversion)

In diesem Tutorial erfahren Sie, wie Sie ein physisches Data Box Edge-Gerät installieren. Die Installation umfasst das Auspacken, die Rackmontage und die Verkabelung des Geräts. 

Die Installation dauert voraussichtlich etwa zwei Stunden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Auspacken des Geräts
> * Montieren des Geräts in einem Rack
> * Verkabeln des Geräts

> [!IMPORTANT]
> Die Data Box Edge-Lösung befindet sich in der Vorschauphase. Lesen Sie die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Installation eines physischen Geräts wird Folgendes vorausgesetzt:

### <a name="for-the-data-box-edge-resource"></a>Für die Data Box Edge-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie haben alle Schritte des Tutorials [Vorbereiten der Bereitstellung von Azure Data Box Edge](data-box-edge-deploy-prep.md) ausgeführt.
    * Sie haben eine Data Box Edge-Ressource für die Bereitstellung Ihres Geräts erstellt.
    * Sie haben den Aktivierungsschlüssel zum Aktivieren Ihres Geräts mit der Data Box Edge-Ressource erstellt.

 
### <a name="for-the-data-box-edge-physical-device"></a>Für das physische Data Box Edge-Gerät

Führen Sie folgende Schritte aus, bevor Sie ein Gerät bereitstellen:

- Achten Sie darauf, dass das Gerät sicher auf einer flachen, stabilen und ebenen Arbeitsfläche liegt.
- Vergewissern Sie sich, dass der geplante Aufstellungsort über Folgendes verfügt:
    - Standardmäßige unabhängige Wechselstromversorgung

        ODER
    - Rack-PDU (Power Distribution Unit) mit unterbrechungsfreier Stromversorgung (USV)
    - Freier Einschub mit 1 HE in dem Rack, in das Sie das Gerät einbauen möchten

### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Datencenter

Vorbereitungen

- Lesen Sie die Netzwerkanforderungen für die Bereitstellung einer Data Box Edge-Lösung, und konfigurieren Sie das Netzwerk des Datencenters gemäß den Anforderungen. Weitere Informationen finden Sie unter [Netzwerkanforderungen](data-box-edge-system-requirements.md#networking-port-requirements).

- Vergewissern Sie sich, dass die Internetbandbreite mindestens 20 MBit/s beträgt, damit das Gerät optimal funktioniert.


## <a name="unpack-the-device"></a>Auspacken des Geräts

Dieses Gerät wird in einem einzelnen Karton ausgeliefert. Führen Sie die folgenden Schritte aus, um das Gerät auszupacken. 

1. Legen Sie den Karton auf einen flachen, ebenen Untergrund.
2. Überprüfen Sie den Karton und das Verpackungsmaterial auf Risse, Schnitte, Wasserschäden oder anderweitige offensichtliche Beschädigungen. Sollte der Karton oder die Verpackung stark beschädigt sein, öffnen Sie den Karton bzw. die Verpackung nicht. Wenden Sie sich an den Microsoft-Support, um zu ermitteln, ob das Gerät funktionstüchtig ist.
3. Packen Sie den Karton aus. Stellen Sie nach dem Auspacken des Kartons sicher, dass Folgendes vorhanden ist:
    - Ein Edgegerät (einzelnes Gehäuse)
    - Zwei Netzkabel
    - Ein ohne Werkzeug montierbares Kit für die Rackmontage (einschließlich zwei Seitenschienen und Montagematerial)

Sollten Sie nicht alle oben aufgeführten Teile erhalten haben, wenden Sie sich an den Data Box Edge-Support. Im nächsten Schritt wird das Gerät in ein Rack eingebaut.


## <a name="rack-the-device"></a>Montieren des Geräts in einem Rack

Das Gerät muss in einem standardmäßigen 19-Zoll-Rack installiert werden. Gehen Sie wie folgt vor, um Ihr Gerät in einem standardmäßigen 19-Zoll-Rack mit Ständern an der Vorder- und Rückseite zu montieren.

> [!IMPORTANT]
> Data Box Edge-Geräte müssen für den ordnungsgemäßen Betrieb in ein Rack eingebaut werden.


1. Ziehen Sie an der vorderen Entriegelung der Schienenbaugruppe, um die innere Schiene zu lösen. Lösen Sie die Arretierung, und schieben Sie die mittlere Schiene nach innen, um die Schiene einzufahren.  
    Die innere und äußere Schiene sollten nun getrennt sein.

    ![Installieren der Schienen für die Rackmontage](./media/data-box-edge-deploy-install/rack-mount-rail-1.png)

2. Installieren Sie nun die äußeren Schienen an den vertikalen Blechwinkeln des Rackschranks. Die Gleitschienen sind jeweils mit **Front** (Vorne) gekennzeichnet. Diese Seite muss an der Vorderseite des Gehäuses befestigt werden.    
    1. Suchen Sie die Fixierungsstifte, die sich vorne und hinten an der Schienenbaugruppe befinden. Fahren Sie die Schiene so weit aus, dass sie zwischen die Rackständer passt. Befestigen Sie zuerst die äußere Schiene an der Rückseite des Racks. Positionieren Sie die hintere Befestigungsklammer auf der Innenseite der hinteren Befestigungslöcher des Racks.   

    2. Halten Sie den Auslöser an der hinteren Klammer gedrückt, um die Metallhaken auszufahren. Richten Sie die hintere Klammer aus, führen Sie sie in die Befestigungslöcher ein, und lassen Sie anschließend den Auslöser los.

    3. Richten Sie die vordere Klammer an den Befestigungslöchern aus.

    4. Die vordere Klammer sollte nun am Rack befestigt sein. Bei Bedarf können die Schienen mit Schrauben vom Typ „M5 X 10L“ an den Ständern befestigt werden. 

    ![Installieren der Schienen für die Rackmontage](./media/data-box-edge-deploy-install/rack-mount-rail-2.png)

3. Wenn Sie die innere Schiene am Gehäuse befestigen, müssen die Schlüssellochöffnungen der inneren Schiene an den Fixierstiften an der Gehäuseseite ausgerichtet werden. Stellen Sie sicher, dass die Köpfe der Fixierstifte des Gehäuses durch die Schlüssellochöffnungen der inneren Schiene ragen. Ziehen Sie die Schiene in Richtung Gehäusefront, bis die Schiene hörbar einrastet. Wiederholen Sie den Vorgang mit der anderen inneren Schiene. Schieben Sie das Gehäuse mit der inneren Schiene in den Schlitten, um die Rackmontage abzuschließen.

    ![Installieren der Schienen für die Rackmontage](./media/data-box-edge-deploy-install/rack-mount-rail-3.png)

## <a name="cable-the-device"></a>Verkabeln des Geräts

In diesem Abschnitt erfahren Sie, wie Sie Ihr Edgegerät an die Stromversorgung und an das Netzwerk anschließen.

Bevor Sie mit der Verkabelung Ihres Geräts beginnen, benötigen Sie Folgendes:

- Das physische Edgegerät (ausgepackt und im Rack montiert)
- Zwei Netzkabel 
- Mindestens ein Netzwerkkabel (1 GbE, RJ45) zum Anschließen an die Verwaltungsschnittstelle. Das Gerät verfügt über zwei 1-GbE-Netzwerkschnittstellen: eine für die Verwaltung und eine für Daten.
- Ein SFP+-Kupferkabel (25 GbE) für jede zu konfigurierende Datennetzwerkschnittstelle. Mindestens eine Datennetzwerkschnittstelle („PORT 2“, „PORT 3“, „PORT 4“, „PORT 5“ oder PORT 6“) muss mit dem Internet verbunden sein und über Azure-Konnektivität verfügen.  
- Zugang zu zwei PDUs (Power Distribution Units) (empfohlen)

> [!NOTE]
> - Wenn Sie nur eine einzelne Datennetzwerkschnittstelle anschließen, sollten Sie eine 25-GbE-Netzwerkschnittstelle wie „PORT 3“, „PORT 4“, „PORT 5“ oder „PORT 6“ verwenden, um Daten an Azure zu senden. 
> - Aus Leistungsgründen und für die Verarbeitung großer Datenmengen empfiehlt es sich, alle Datenports zu verwenden.
> - Das Edgegerät sollte mit dem Datencenternetzwerk verbunden sein, um Daten von Datenquellservern erfassen zu können. 

Ihr Edgegerät verfügt über acht NVMe-SSDs. Am vorderen Bedienfeld befinden sich außerdem Status-LEDs und Netzschalter. An der Rückseite des Geräts befinden sich redundante Netzteile (Power Supply Units, PSUs). Ihr Gerät verfügt über sechs Netzwerkschnittstellen: zwei mit 1 GBit/s und vier mit 25 GBit/s. Ihr Gerät ist mit einem Baseboard-Verwaltungscontroller (Baseboard Management Controller, BMC) ausgestattet. Machen Sie sich mit den verschiedenen Anschlüssen an der Geräterückseite vertraut.
 
  ![Rückseite eines verkabelten Geräts](./media/data-box-edge-deploy-install/backplane-cabled.png)
 
Gehen Sie wie folgt vor, um Ihr Gerät an die Stromversorgung und an das Netzwerk anzuschließen:

1. Schließen Sie die Netzkabel an die PSUs des Gehäuses an. Zur Gewährleistung hoher Verfügbarkeit müssen beide PSUs installiert und an unterschiedliche Stromquellen angeschlossen werden.

2. Schließen Sie die Netzkabel an die PDUs des Racks an. Stellen Sie sicher, dass die beiden PSUs separate Stromquellen verwenden.

3. Verbinden Sie die 1-GbE-Netzwerkschnittstelle „PORT 1“ mit dem Computer, der zum Konfigurieren des physischen Geräts verwendet wird. „PORT 1“ ist die dedizierte Verwaltungsschnittstelle.

4. Verbinden Sie mindestens einen der Ports 2, 3, 4, 5 oder 6 mit dem Datencenternetzwerk/Internet. Wenn Sie sich für „PORT 2“ entscheiden, verwenden Sie das RJ45-Netzwerkkabel. Verwenden Sie für die 25-GbE-Netzwerkschnittstellen die SFP+-Kupferkabel.  


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Auspacken des Geräts
> * Montieren des Geräts in einem Rack
> * Verkabeln des Geräts

Im nächsten Tutorial erfahren Sie, wie Sie Ihr Gerät verbinden, einrichten und aktivieren.

> [!div class="nextstepaction"]
> [Tutorial: Verbinden, Einrichten und Aktivieren von Azure Data Box Edge (Vorschauversion)](./data-box-edge-deploy-connect-setup-activate.md)


