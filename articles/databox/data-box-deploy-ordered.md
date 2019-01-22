---
title: Bestellen einer Microsoft Azure Data Box | Microsoft-Dokumentation
description: Erfahren Sie, welche Voraussetzungen für die Bereitstellung gelten und wie Sie eine Azure Data Box bestellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: alkohli
ms.openlocfilehash: 5ce8217629cff702ecd3ec274d77918e54efacc6
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359118"
---
# <a name="tutorial-order-azure-data-box"></a>Tutorial: Bestellen von Azure Data Box

Azure Data Box ist eine Hybridlösung, mit der Sie Ihre lokalen Daten auf schnelle, einfache und zuverlässige Weise in Azure importieren können. Sie übertragen Ihre Daten auf ein von Microsoft bereitgestelltes Speichergerät mit 80 TB nutzbarer Kapazität, und anschließend senden Sie das Gerät zurück. Diese Daten werden dann in Azure hochgeladen.

In diesem Tutorial wird beschrieben, wie Sie eine Azure Data Box bestellen können. In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
> * Voraussetzungen für die Bereitstellung von Data Box
> * Bestellen einer Data Box
> * Nachverfolgen der Bestellung
> * Stornieren der Bestellung

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie das Gerät bereitstellen, müssen die folgenden Konfigurationsvoraussetzungen für den Data Box-Dienst und das Data Box-Gerät erfüllt sein.

### <a name="for-service"></a>Für den Dienst

Stellen Sie Folgendes sicher, bevor Sie beginnen:
- Sie besitzen ein Microsoft Azure-Speicherkonto mit Anmeldeinformationen für den Zugriff.
- Sie verwenden ein Abonnement der folgenden Typen für den Data Box-Dienst:
    - Microsoft Enterprise Agreement (EA). Erfahren Sie mehr über [EA-Abonnements](https://azure.microsoft.com/pricing/enterprise-agreement/).
    - Cloud Solution Provider (CSP). Erfahren Sie mehr über das [Azure CSP-Programm](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview).
    - Microsoft Azure Sponsorship. Erfahren Sie mehr über das [Azure Sponsorship-Programm](https://azure.microsoft.com/offers/ms-azr-0036p/). 

- Sie haben Zugriff als Besitzer oder Mitwirkender auf das Abonnement (zum Erstellen einer Data Box-Bestellung erforderlich).

### <a name="for-device"></a>Für das Gerät

Stellen Sie Folgendes sicher, bevor Sie beginnen:
- Sie verfügen über einen Hostcomputer, der mit dem Netzwerk des Datencenters verbunden ist. Data Box kopiert die Daten von diesem Computer. Auf dem Hostcomputer muss ein unterstütztes Betriebssystem ausgeführt werden (siehe [Azure Data Box – Systemanforderungen](data-box-system-requirements.md)).
- Ihr Datencenter verfügt über ein Hochgeschwindigkeitsnetzwerk. Es wird dringend empfohlen, dass mindestens eine 10-GbE-Verbindung verfügbar ist. Falls keine 10-GbE-Verbindung verfügbar ist, kann eine 1-GbE-Datenverbindung verwendet werden, die Geschwindigkeit der Kopiervorgänge wird dadurch jedoch beeinträchtigt.


## <a name="order-data-box"></a>Bestellen einer Data Box

Führen Sie die folgenden Schritte im Azure-Portal aus, um ein Gerät zu bestellen.

1. Melden Sie sich mit Ihren Microsoft Azure-Anmeldeinformationen unter folgender URL an: [https://portal.azure.com](https://portal.azure.com).
2. Klicken Sie auf **+ Ressource erstellen**, und suchen Sie nach *Azure Data Box*. Klicken Sie auf **Azure Data Box**.
    
   [![Suchen nach Azure Data Box 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. Klicken Sie auf **Create**.

4. Überprüfen Sie, ob der Data Box-Dienst in Ihrer Region verfügbar ist. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus. Klicken Sie dann auf **Übernehmen**. 
    |Einstellung  |Wert  |
    |---------|---------|
    |Abonnement     | Wählen Sie ein EA-, CSP- oder Azure Sponsorship-Abonnement für den Data Box-Dienst aus. <br> Das Abonnement ist mit Ihrem Abrechnungskonto verknüpft.       |
    |Übertragungstyp     | Wählen Sie **Import in Azure** aus.        |
    |Quellland/-region     |   Wählen Sie das Land aus, in dem sich Ihre Daten aktuell befinden.         |
    |Azure-Zielregion     |     Wählen Sie die Azure-Region aus, in die Daten übertragen werden sollen.        |

5. Wählen Sie **Data Box** aus. Die maximale Kapazität der Lösung für eine einzelne Bestellung beträgt 80 TB. Sie können mehrere Bestellungen für größere Datenmengen erstellen.

      [![Auswählen der Data Box-Option 1](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

6. Geben Sie in **Bestellung** die **Bestelldetails** an. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, und klicken Sie dann auf **Weiter**.
    
    |Einstellung  |Wert  |
    |---------|---------|
    |NAME     |  Geben Sie einen Anzeigenamen an, um die Bestellung nachzuverfolgen. <br> Der Name kann zwischen 3 und 24 Zeichen lang sein und darf nur Buchstaben, Zahlen und Bindestriche enthalten. <br> Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden.      |
    |Ressourcengruppe     |   Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. <br> Eine Ressourcengruppe ist ein logischer Container für die Ressourcen, die zusammen verwaltet oder bereitgestellt werden können.         |
    |Azure-Zielregion     | Wählen Sie eine Region für Ihr Speicherkonto aus. <br> Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](data-box-overview.md#region-availability).        |
    |Speicherkonten     | Wählen Sie basierend auf der angegebenen Azure-Region mindestens ein Speicherkonto in der gefilterten Liste eines vorhandenen Speicherkontos aus. Data Box kann mit bis zu zehn Speicherkonten verknüpft werden. <br> Sie können auch ein neues **universelles v1**-, **universelles v2**- oder **BLOB-Speicherkonto** erstellen. Sie können keine Speicherkonten verwenden, für die Regeln konfiguriert sind. Die Speicherkonten müssen den **Zugriff von allen Netzwerken im Abschnitt mit Firewalls und virtuellen Netzwerken zulassen**.|
    
7. Geben Sie unter **Lieferadresse** Ihren Vor- und Nachnamen, den Namen und die Postanschrift des Unternehmens sowie eine gültige Telefonnummer an. Klicken Sie auf **Adresse überprüfen**. Der Dienst überprüft anhand der Lieferadresse die Verfügbarkeit des Diensts. Wenn der Dienst für die angegebene Lieferadresse verfügbar ist, erhalten Sie eine entsprechende Benachrichtigung. Klicken Sie auf **Weiter**.

8. Geben Sie in den **Benachrichtigungsdetails** E-Mail-Adressen an. Der Dienst sendet E-Mail-Benachrichtigungen in Bezug auf Aktualisierungen des Auftragsstatus an die angegebenen E-Mail-Adressen.

    Es wird empfohlen, eine E-Mail-Gruppenadresse zu verwenden, damit Sie weiterhin Benachrichtigungen erhalten, wenn ein Administrator die Gruppe verlässt.

9. Überprüfen Sie die Informationen der **Zusammenfassung** im Zusammenhang mit der Bestellung, den Kontakt, die Benachrichtigungen und die Datenschutzbestimmungen. Aktivieren Sie das Kontrollkästchen für die Zustimmung zu den Datenschutzbestimmungen.

10. Klicken Sie auf **Bestellen**. Die Erstellung des Auftrags dauert einige Minuten. 


## <a name="track-the-order"></a>Nachverfolgen der Bestellung

Nachdem Sie die Bestellung aufgegeben haben, können Sie ihren Status im Azure-Portal nachverfolgen. Navigieren Sie zu Ihrer Data Box-Bestellung, und navigieren Sie dann zu **Übersicht**, um den Status anzuzeigen. Die Bestellung wird im Portal mit dem Status **Bestellt** angezeigt.

Sollte das Gerät nicht verfügbar sein, erhalten Sie eine entsprechende Benachrichtigung. Wenn das Gerät verfügbar ist, identifiziert Microsoft das Gerät für den Versand und bereitet den Versand vor. Während der Vorbereitung des Geräts werden folgende Aktionen ausgeführt:

- Für jedes mit dem Gerät verknüpfte Speicherkonto werden SMB-Freigaben erstellt. 
- Für jede Freigabe werden Anmeldeinformationen wie Benutzername und Kennwort generiert.
- Ein Gerätekennwort zum Entsperren des Geräts wird ebenfalls generiert. 
- Die Data Box wird gesperrt, um nicht autorisierten Zugriff auf das Gerät zu verhindern.

Wenn die Vorbereitung des Geräts abgeschlossen ist, wird die Bestellung im Portal mit dem Status **Verarbeitet** angezeigt.

![Data Box-Bestellung verarbeitet](media/data-box-overview/data-box-order-status-processed.png)

Microsoft bereitet dann Ihr Gerät vor und versendet es über einen regionalen Kurierdienst. Sobald das Gerät versandt wurde, erhalten Sie eine Nachverfolgungsnummer. Das Portal zeigt die Bestellung im Zustand **Versandt** an.

![Data Box-Bestellung versendet](media/data-box-overview/data-box-order-status-dispatched.png)

## <a name="cancel-the-order"></a>Stornieren der Bestellung

Um die Bestellung zu stornieren, navigieren Sie im Azure-Portal zu **Übersicht**, und klicken Sie dann auf der Befehlsleiste auf **Stornieren**.

Solange eine Bestellung noch nicht mit dem Status „Verarbeitet“ angezeigt wird, können Sie sie jederzeit stornieren.
 
Um eine stornierte Bestellung zu löschen, navigieren Sie zu **Übersicht** und klicken dann auf der Befehlsleiste auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Voraussetzungen für die Bereitstellung von Data Box
> * Bestellen einer Data Box
> * Nachverfolgen der Bestellung
> * Stornieren der Bestellung

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Data Box einrichten.

> [!div class="nextstepaction"]
> [Einrichten der Azure Data Box](./data-box-deploy-set-up.md)


