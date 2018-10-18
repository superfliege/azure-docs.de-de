---
title: 'Tutorial: Vorbereiten des Azure-Portals für die Bereitstellung von Data Box Edge | Microsoft-Dokumentation'
description: Im ersten Tutorial zum Bereitstellen von Azure Data Box Edge geht es um die Vorbereitung des Azure-Portals.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 35ac28d687c8bc6636a7d8e10f54ffb5b219a776
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167461"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge-preview"></a>Tutorial: Vorbereiten der Bereitstellung von Azure Data Box Edge (Vorschauversion)


Dies ist das erste Tutorial einer Reihe von Bereitstellungstutorials, die zum vollständigen Bereitstellen Ihrer Azure Data Box Edge-Ressource erforderlich sind. In diesem Tutorial erfahren Sie, wie Sie das Azure-Portal für die Bereitstellung Ihrer Data Box Edge-Ressource vorbereiten. 

Sie benötigen Administratorrechte, um die Einrichtung und Konfiguration abzuschließen. Die Vorbereitung des Portals erfordert weniger als 10 Minuten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Neue Ressource erstellen
> * Abrufen des Aktivierungsschlüssels

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 

### <a name="get-started"></a>Erste Schritte

Führen Sie zum Bereitstellen Ihrer Data Box Edge-Ressource die folgenden Tutorials in der angegebenen Reihenfolge aus:

| **#** | **Schritt** | **Dokumente** |
| --- | --- | --- | 
| 1. |**[Vorbereiten des Azure-Portals für Data Box Edge](data-box-edge-deploy-prep.md)** |Erstellen und konfigurieren Sie Ihre Data Box Edge-Ressource, bevor Sie ein physisches Data Box Edge-Gerät installieren. |
| 2. |**[Installieren von Data Box Edge](data-box-edge-deploy-install.md)**|Packen Sie das physische Data Box Edge-Gerät aus, montieren Sie es in einem Rack, und verkabeln Sie es.  |
| 3. |**[Verbinden, Einrichten und Aktivieren von Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)** |Stellen Sie eine Verbindung mit der lokalen Webbenutzeroberfläche her, führen Sie die Einrichtung durch, und aktivieren Sie das Gerät. Das Gerät ist für die Einrichtung von SMB- oder NFS-Freigaben bereit.  |
| 4. |**[Übertragen von Daten mit Data Box Edge](data-box-edge-deploy-add-shares.md)** |Fügen Sie Freigaben hinzu, und stellen Sie über SMB oder NFS eine Verbindung mit Freigaben her. |
| 5. |**[Transformieren von Daten mit Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Konfigurieren Sie Edgemodule auf dem Gerät, um die Daten bei der Übertragung an Azure zu transformieren. |

Sie können jetzt mit der Einrichtung des Azure-Portals beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Im Folgenden finden Sie die Konfigurationsvoraussetzungen für die Data Box Edge-Ressource, das Data Box Edge-Gerät und das Netzwerk des Datencenters.

### <a name="for-the-data-box-edge-resource"></a>Für die Data Box Edge-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Ihr Microsoft Azure-Abonnement ist für die Data Box Edge-Ressource aktiviert.
* Sie besitzen ein Microsoft Azure-Speicherkonto mit Anmeldeinformationen für den Zugriff.

### <a name="for-the-data-box-edge-device"></a>Für das Data Box Edge-Gerät

Stellen Sie Folgendes sicher, bevor Sie ein physisches Gerät bereitstellen:

- Sie verfügen in Ihrem Datencenter über einen freien Einschub (1 HE) in einem 19-Zoll-Standardrack zur Rackmontage des Geräts. 
- Sie benötigen eine flache, stabile und ebene Arbeitsfläche, auf der das Gerät sicher abgelegt werden kann.
- Stellen Sie sicher, dass am vorgesehenen Ort für das Gerät eine Standardstromversorgung von einer unabhängigen Quelle oder eine Rack-PDU (Power Distribution Unit) mit unterbrechungsfreier Stromversorgung (USV) vorhanden ist.
- Sie verfügen über ein physisches Gerät.


### <a name="for-the-datacenter-network"></a>Für das Netzwerk des Rechenzentrums

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Das Netzwerk in Ihrem Datencenter ist gemäß den Netzwerkanforderungen für das Data Box Edge-Gerät konfiguriert. Weitere Informationen finden Sie unter [Data Box Edge – Systemanforderungen](data-box-gateway-system-requirements.md).

* Für Data Box Edge steht jederzeit eine dedizierte Internetbandbreite von mindestens 20 MBit/s zur Verfügung. Diese Bandbreite sollte nicht gemeinsam mit anderen Anwendungen genutzt werden. Bei Verwendung der Einschränkung der maximalen Bandbreite empfehlen wir die Verwendung einer Internetbandbreite von mindestens 32 MBit/s, damit die Einschränkung funktioniert.

## <a name="create-a-new-resource"></a>Neue Ressource erstellen

Führen Sie die folgenden Schritte aus, um eine neue Data Box Edge-Ressource zu erstellen. 

Falls Sie bereits über eine Data Box Edge-Ressource für die Verwaltung Ihres physischen Geräts verfügen, überspringen Sie diesen Schritt, und fahren Sie mit [Abrufen des Aktivierungsschlüssels](#get-the-activation-key) fort.

Führen Sie die folgenden Schritte im Azure-Portal aus, um eine Data Box-Ressource zu erstellen.

1. Melden Sie sich mit Ihren Microsoft Azure-Anmeldeinformationen beim Azure-Vorschauportal unter der folgenden URL an: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Wählen Sie das Abonnement aus, das Sie für die Vorschauversion von Data Box Edge verwenden möchten. Wählen Sie die Region aus, in der Sie die Data Box Edge-Ressource bereitstellen möchten. Klicken Sie unter der Option **Data Box Edge** auf **Erstellen**.

    ![Suchen nach dem Data Box Edge-Dienst](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Geben Sie die folgenden Informationen für die neue Ressource ein, oder wählen Sie sie aus.
    
    |Einstellung  |Wert  |
    |---------|---------|
    |Ressourcenname   | Ein Anzeigename, der die Ressource identifiziert.<br>Der Ressourcenname muss 2 bis 50 Zeichen lang sein und darf nur Buchstaben, Zahlen und Bindestriche enthalten.<br> Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden.        |
    |Abonnement    |Das Abonnement ist mit Ihrem Abrechnungskonto verknüpft. |
    |Ressourcengruppe  |Wählen Sie eine vorhandene Gruppe aus, oder erstellen Sie eine neue Gruppe.<br>Erfahren Sie mehr über [Azure-Ressourcengruppen](../azure-resource-manager/resource-group-overview.md).     |
    |Standort     |In diesem Release sind „USA, Osten“, „USA, Westen 2“, „Asien, Südosten“ und „Europa, Westen“ verfügbar. <br> Wählen Sie den Standort aus, der der geografischen Region, in der Ihr Gerät bereitgestellt werden soll, am nächsten ist.|
    
    ![Erstellen der Data Box Edge-Ressource](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. Klicken Sie auf **OK**.
 
Die Erstellung der Ressource dauert einige Minuten. Nachdem die Ressource erfolgreich erstellt wurde, werden Sie entsprechend benachrichtigt.


## <a name="get-the-activation-key"></a>Abrufen des Aktivierungsschlüssels

Wenn die Data Box Edge-Ressource betriebsbereit ist, müssen Sie den Aktivierungsschlüssel abrufen. Dieser Schlüssel wird verwendet, um Ihr Data Box Edge-Gerät zu aktivieren und mit der Ressource zu verbinden. Sie können diesen Schlüssel jetzt abrufen, während Sie sich im Azure-Portal befinden.

1. Klicken Sie auf die Ressource, die Sie erstellt haben, und dann auf **Übersicht**.

2. Klicken Sie auf **Schlüssel generieren**, um einen Aktivierungsschlüssel zu erstellen. Klicken Sie auf das Kopiersymbol, um den Schlüssel zu kopieren und zur späteren Verwendung zu speichern.

    ![Abrufen des Aktivierungsschlüssels](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - Der Aktivierungsschlüssel läuft drei Tage nach der Generierung ab. 
> - Generieren Sie einen neuen Schlüssel, falls der Schlüssel abgelaufen ist. Der ältere Schlüssel ist nicht mehr gültig.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Neue Ressource erstellen
> * Abrufen des Aktivierungsschlüssels

Im nächsten Tutorial erfahren Sie, wie Sie Ihr Data Box Edge-Gerät installieren. 

> [!div class="nextstepaction"]
> [Installieren von Data Box Edge](./data-box-edge-deploy-install.md)



