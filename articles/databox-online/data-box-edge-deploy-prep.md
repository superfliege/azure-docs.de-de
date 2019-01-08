---
title: 'Tutorial: Vorbereiten des Azure-Portals für die Bereitstellung von Data Box Edge | Microsoft-Dokumentation'
description: Im ersten Tutorial über Bereitstellen von Azure Data Box Edge geht es um die Vorbereitung des Azure-Portals.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 25f68b011d1fcba450903e9a691b98dfe9e87281
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726118"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Tutorial: Vorbereiten der Bereitstellung von Azure Data Box Edge  


Dies ist das erste Tutorial einer Reihe von Bereitstellungstutorials, die zum vollständigen Bereitstellen von Azure Data Box Edge erforderlich sind. In diesem Tutorial erfahren Sie, wie Sie das Azure-Portal für die Bereitstellung einer Data Box Edge-Ressource vorbereiten. 

Sie benötigen Administratorrechte, um die Einrichtung und Konfiguration abzuschließen. Die Vorbereitung des Portals erfordert weniger als 10 Minuten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Neue Ressource erstellen
> * Abrufen des Aktivierungsschlüssels

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen.  

### <a name="get-started"></a>Erste Schritte

Führen Sie zum Bereitstellen von Data Box Edge die folgenden Tutorials in der angegebenen Reihenfolge aus.

| **#** | **Schritt** | **Dokumente** |
| --- | --- | --- | 
| 1. |**[Vorbereiten des Azure-Portals für Data Box Edge](data-box-edge-deploy-prep.md)** |Erstellen und konfigurieren Sie Ihre Data Box Edge-Ressource, bevor Sie ein physisches Data Box Edge-Gerät installieren. |
| 2. |**[Installieren von Data Box Edge](data-box-edge-deploy-install.md)**|Packen Sie das physische Data Box Edge-Gerät aus, montieren Sie es in einem Rack, und verkabeln Sie es.  |
| 3. |**[Verbinden, Einrichten und Aktivieren von Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)** |Stellen Sie eine Verbindung mit der lokalen Webbenutzeroberfläche her, führen Sie die Einrichtung durch, und aktivieren Sie das Gerät. Das Gerät ist für die Einrichtung von SMB- oder NFS-Freigaben bereit.  |
| 4. |**[Übertragen von Daten mit Data Box Edge](data-box-edge-deploy-add-shares.md)** |Fügen Sie Freigaben hinzu, und stellen Sie über SMB oder NFS eine Verbindung mit Freigaben her. |
| 5. |**[Transformieren von Daten mit Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Konfigurieren Sie Edgemodule auf dem Gerät, um die Daten bei der Übertragung an Azure zu transformieren. |

Sie können jetzt mit der Einrichtung des Azure-Portals beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Nachstehend finden Sie die Konfigurationsvoraussetzungen für die Data Box Edge-Ressource, das Data Box Edge-Gerät und das Netzwerk des Datencenters.

### <a name="for-the-data-box-edge-resource"></a>Für die Data Box Edge-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Ihr Microsoft Azure-Abonnement ist für eine Data Box Edge-Ressource aktiviert.
* Sie besitzen ein Microsoft Azure-Speicherkonto mit Anmeldeinformationen für den Zugriff.

### <a name="for-the-data-box-edge-device"></a>Für das Data Box Edge-Gerät

Stellen Sie Folgendes sicher, bevor Sie ein physisches Gerät bereitstellen:
- Sie verfügen in Ihrem Datencenter über einen freien Einschub (1 HE) in einem 19-Zoll-Standardrack zur Rackmontage des Geräts. 
- Sie haben Zugang zu einer flachen, stabilen und ebenen Arbeitsfläche, auf der das Gerät sicher abgelegt werden kann.
- Der Standort, an dem Sie das Gerät einrichten möchten, hat eine Standardstromversorgung von einer unabhängigen Quelle oder eine Rack-PDU (Power Distribution Unit) mit unterbrechungsfreier Stromversorgung (USV).
- Sie verfügen über ein physisches Gerät.


### <a name="for-the-datacenter-network"></a>Für das Netzwerk des Rechenzentrums

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Das Netzwerk in Ihrem Datencenter ist gemäß den Netzwerkanforderungen für das Data Box Edge-Gerät konfiguriert. Weitere Informationen finden Sie unter [Systemanforderungen für Azure Data Box Edge](data-box-gateway-system-requirements.md).

* Für Data Box Edge steht jederzeit eine dedizierte Internetbandbreite von mindestens 20 MBit/s zur Verfügung. Diese Bandbreite sollte nicht gemeinsam mit anderen Anwendungen genutzt werden. Wenn Sie mit Netzwerk-Bandbreiteneinschränkung arbeiten, sollten Sie eine Internetbandbreite von mindestens 32 MBit/s verwenden, damit die Einschränkung funktioniert.

## <a name="create-a-new-resource"></a>Neue Ressource erstellen

Führen Sie die folgenden Schritte aus, um eine neue Data Box Edge-Ressource zu erstellen. 

Falls Sie bereits über eine Data Box Edge-Ressource für die Verwaltung Ihres physischen Geräts verfügen, überspringen Sie diesen Schritt, und fahren Sie mit [Abrufen des Aktivierungsschlüssels](#get-the-activation-key) fort.

Um eine Data Box Edge-Ressource zu erstellen, führen Sie die folgenden Schritte im Azure-Portal aus.

1. Melden Sie sich mit Ihren Microsoft Azure-Anmeldeinformationen beim Azure-Vorschauportal unter der folgenden URL an: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Wählen Sie das Abonnement aus, das Sie für die Vorschauversion von Data Box Edge verwenden möchten. Wählen Sie die Region aus, in der Sie die Data Box Edge-Ressource bereitstellen möchten. Wählen Sie in der Option **Data Box Edge** den Eintrag **Erstellen** aus.

    ![Suchen nach dem Data Box Edge-Dienst](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Geben Sie die folgenden Informationen für die neue Ressource ein, oder wählen Sie sie aus.
    
    |Einstellung  |Wert  |
    |---------|---------|
    |Ressourcenname   | Ein Anzeigename, mit dem die Ressource erkannt wird.<br>Der Ressourcenname muss 2 bis 50 Zeichen lang sein und darf nur Buchstaben, Zahlen und Bindestriche enthalten.<br> Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden.        |
    |Abonnement    |Das Abonnement ist mit Ihrem Abrechnungskonto verknüpft. |
    |Ressourcengruppe  |Wählen Sie eine vorhandene Gruppe aus, oder erstellen Sie eine neue Gruppe.<br>Erfahren Sie mehr über [Azure-Ressourcengruppen](../azure-resource-manager/resource-group-overview.md).     |
    |Standort     |In diesem Release sind „USA, Osten“, „USA, Westen 2“, „Asien, Südosten“ und „Europa, Westen“ verfügbar. <br> Wählen Sie den Standort aus, der der geografischen Region, in der Ihr Gerät bereitgestellt werden soll, am nächsten ist.|
    
    ![Erstellen einer Data Box Edge-Ressource](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. Klicken Sie auf **OK**.
 
Die Erstellung der Ressource dauert einige Minuten. Nachdem die Ressource erfolgreich erstellt wurde, werden Sie entsprechend benachrichtigt.


## <a name="get-the-activation-key"></a>Abrufen des Aktivierungsschlüssels

Wenn die Data Box Edge-Ressource betriebsbereit ist, müssen Sie den Aktivierungsschlüssel abrufen. Dieser Schlüssel wird verwendet, um Ihr Data Box Edge-Gerät zu aktivieren und mit der Ressource zu verbinden. Sie können diesen Schlüssel jetzt abrufen, während Sie sich im Azure-Portal befinden.

1. Wählen Sie die Ressource aus, die Sie erstellt haben, und wählen Sie dann **Übersicht** aus.

2. Wählen Sie **Schlüssel generieren** aus, um einen Aktivierungsschlüssel zu erstellen. Wählen Sie das Kopiersymbol, um den Schlüssel zu kopieren und für die spätere Verwendung zu speichern.

    ![Abrufen des Aktivierungsschlüssels](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Der Aktivierungsschlüssel läuft drei Tage nach seiner Generierung ab. 
> - Generieren Sie einen neuen Schlüssel, falls der Schlüssel abgelaufen ist. Der ältere Schlüssel ist nicht mehr gültig.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Erstellen einer neuen Ressource
> * Abrufen des Aktivierungsschlüssels

Im nächsten Tutorial erfahren Sie, wie Sie Data Box Edge installieren. 

> [!div class="nextstepaction"]
> [Installieren von Data Box Edge](./data-box-edge-deploy-install.md)



