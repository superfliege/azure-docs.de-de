---
title: Tutorial zum Bestellen des Microsoft Azure Data Box-Datenträgers | Microsoft Docs
description: Verwenden Sie dieses Tutorial, um zu erfahren, wie Sie sich für einen Azure Data Box-Datenträger registrieren und diesen bestellen können, um Daten in Azure zu importieren.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/04/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 37130e946aad00ef4eca14b7ce7942a8e435e6cd
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782327"
---
# <a name="tutorial-order-an-azure-data-box-disk-preview"></a>Tutorial: Bestellen eines Azure Data Box-Datenträgers (Vorschau)

Azure Data Box Disk ist eine Hybrid Cloud-Lösung, die es Ihnen ermöglicht, Ihre lokalen Daten auf schnelle, einfache und zuverlässige Weise in Azure zu importieren. Sie übertragen Ihre Daten auf SSDs, die von Microsoft bereitgestellt werden, und senden die Datenträger dann zurück. Diese Daten werden dann in Azure hochgeladen. 

In diesem Tutorial wird beschrieben, wie Sie einen Azure Data Box-Datenträger bestellen können. In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
> * Registrieren für einen Data Box-Datenträger
> * Bestellen eines Data Box-Datenträgers
> * Nachverfolgen der Bestellung
> * Stornieren der Bestellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!IMPORTANT]
> - Data Box-Datenträger befinden sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 
> - Während der Vorschauphase kann Data Box Disk an Kunden in den USA, in West- und Nordeuropa, Kanada und Australien gesendet werden. Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](data-box-disk-overview.md#region-availability).

## <a name="sign-up"></a>Registrieren 

Der Data Box-Datenträger liegt in einer Vorschauversion vor, und Sie müssen sich für den Dienst registrieren. Führen Sie die folgenden Schritte aus, um sich für den Data Box-Dienst zu registrieren:

1. Melden Sie sich am Azure-Portal an unter: [https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs).
2. Wählen Sie das Abonnement aus, das Sie für die Vorschauversion aktivieren möchten. Beantworten Sie die Fragen in Bezug auf die Größe der Daten, das Land, in dem sich die Daten befinden, den Zeitrahmen und die Datenübertragungshäufigkeit. Klicken Sie auf **Registrieren**.
3. Nachdem Sie registriert und für die Vorschauversion aktiviert wurden, können Sie einen Data Box-Datenträger bestellen.

## <a name="order-data-box-disk"></a>Bestellen des Data Box-Datenträgers

Führen Sie die folgenden Schritte im [Azure-Portal](https://aka.ms/azuredataboxfromdiskdocs) aus, um den Data Box-Datenträger zu bestellen.

1. Klicken Sie in der oberen linken Ecke des Portals auf **Ressource erstellen**, und suchen Sie dann nach *Azure Data Box*. Klicken Sie auf **Azure Data Box**.
    
   ![Suchen nach Azure Data Box 1](media/data-box-disk-deploy-ordered/search-data-box11.png)

2. Klicken Sie auf **Create**.

3. Überprüfen Sie, ob der Data Box-Dienst in Ihrer Region verfügbar ist. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus. Klicken Sie dann auf **Übernehmen**.

    ![Auswählen der Option „Data Box-Datenträger“](media/data-box-disk-deploy-ordered/select-data-box-sku-1.png)

    |Einstellung|Wert|
    |---|---|
    |Abonnement|Wählen Sie ein Abonnement aus, für das der Data Box-Dienst aktiviert ist.<br> Das Abonnement ist mit Ihrem Abrechnungskonto verknüpft. |
    |Übertragungstyp| In Azure importieren|
    |Quellland/-region | Wählen Sie das Land aus, in dem sich Ihre Daten aktuell befinden.|
    |Azure-Zielregion|Wählen Sie die Azure-Region aus, in die Daten übertragen werden sollen.|

  
5.  Wählen Sie **Data Box-Datenträger** aus. Die maximale Kapazität der Lösung für eine einzelne Bestellung von 5 Datenträgern beträgt 35 TB. Sie können ggf. mehrere Bestellungen für größere Datenmengen erstellen. 

     ![Auswählen der Option „Data Box-Datenträger“](media/data-box-disk-deploy-ordered/select-data-box-sku-zoom.png)

6.  Geben Sie in **Bestellung** die **Bestelldetails** an. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus.

    |Einstellung|Wert|
    |---|---|
    |NAME|Geben Sie einen Anzeigenamen an, um die Bestellung nachzuverfolgen.<br> Der Name kann zwischen 3 und 24 Zeichen lang sein und darf nur Buchstaben, Zahlen und Bindestriche enthalten. <br> Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden. |
    |Ressourcengruppe| Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. <br> Eine Ressourcengruppe ist ein logischer Container für die Ressourcen, die zusammen verwaltet oder bereitgestellt werden können. |
    |Azure-Zielregion| Wählen Sie eine Region für Ihr Speicherkonto aus.<br> Zurzeit werden Speicherkonten in allen Regionen in den USA sowie in West- und Nordeuropa, Kanada und Australien unterstützt. |
    |Speicherkonten|Wählen Sie basierend auf der angegebenen Azure-Region aus der gefilterten Liste ein vorhandenes Speicherkonto aus. <br>Sie können auch ein neues universelles v1- oder v2-Konto erstellen. |
    |Geschätzte Datengröße in TB| Geben Sie einen geschätzten Wert in TB ein. <br>Basierend auf der Größe der Daten sendet Microsoft Ihnen eine geeignete Anzahl von SSDs mit jeweils 8 TB (7 TB nutzbare Kapazität) zu. <br>Die maximal nutzbare Kapazität von 5 Datenträgern beträgt bis zu 35 TB. |

13. Klicken Sie auf **Weiter**. 

    ![Angeben der Bestelldetails](media/data-box-disk-deploy-ordered/data-box-order-details.png)

14. Geben Sie auf der Registerkarte **Lieferadresse** Ihren Vor- und Nachnamen, den Namen und die Postanschrift des Unternehmens sowie eine gültige Telefonnummer an. Klicken Sie auf **Adresse überprüfen**. Der Dienst überprüft anhand der Lieferadresse die Verfügbarkeit des Diensts. Wenn der Dienst für die angegebene Lieferadresse verfügbar ist, erhalten Sie eine entsprechende Benachrichtigung. 

    ![Angeben der Lieferadresse](media/data-box-disk-deploy-ordered/data-box-shipping-address.png)
15. Geben Sie in den **Benachrichtigungsdetails** E-Mail-Adressen an. Der Dienst sendet E-Mail-Benachrichtigungen in Bezug auf Aktualisierungen des Auftragsstatus an die angegebenen E-Mail-Adressen. 

    Es wird empfohlen, eine E-Mail-Gruppenadresse zu verwenden, damit Sie weiterhin Benachrichtigungen erhalten, wenn ein Administrator die Gruppe verlässt.

16. Überprüfen Sie die Informationen der **Zusammenfassung** im Zusammenhang mit der Bestellung, den Kontakt, die Benachrichtigungen und die Datenschutzbestimmungen. Aktivieren Sie das Kontrollkästchen für die Zustimmung zu den Datenschutzbestimmungen.

17. Klicken Sie auf **Bestellen**. Die Erstellung des Auftrags dauert einige Minuten.

 
## <a name="track-the-order"></a>Nachverfolgen der Bestellung

Nachdem Sie die Bestellung aufgegeben haben, können Sie den Status des Auftrags im Azure-Vorschauportal nachverfolgen. Navigieren Sie zu Ihrer Bestellung, und navigieren Sie dann zu **Übersicht**, um den Status anzuzeigen. Das Portal zeigt den Auftrag im Zustand **Bestellt** an. 

![Status „Bestellt“ des Data Box-Datenträgers](media/data-box-disk-deploy-ordered/data-box-portal-ordered.png) 

Wenn die Datenträger nicht verfügbar sind, erhalten Sie eine Benachrichtigung. Wenn die Datenträger verfügbar sind, identifiziert Microsoft die Datenträger für den Versand und bereitet das Datenträgerpaket vor. Während der Datenträgervorbereitung werden folgende Aktionen ausgeführt:

- Die Datenträger werden mithilfe von AES-128-BitLocker-Verschlüsselung verschlüsselt.  
- Die Datenträger werden gesperrt, um einen nicht autorisierten Zugriff auf die Datenträger zu verhindern.
- Der Hauptschlüssel, der die Datenträger entsperrt, wird während dieses Vorgangs generiert.

Wenn die Datenträgervorbereitung abgeschlossen ist, zeigt das Portal die Bestellung im Zustand **Verarbeitet** an.

Microsoft bereitet dann Ihre Datenträger vor und versendet sie über einen regionalen Spediteur. Wenn die Datenträger versandt wurden, erhalten Sie eine Nachverfolgungsnummer. Das Portal zeigt die Bestellung im Zustand **Versandt** an.



## <a name="cancel-the-order"></a>Stornieren der Bestellung

Um diese Bestellung zu stornieren, navigieren Sie im Azure-Vorschauportal zu **Übersicht**, und klicken Sie dann auf der Befehlsleiste auf **Stornieren**. 

Sie können die Bestellung nur stornieren, wenn die Datenträger bestellt sind und die Bestellung für den Versand verarbeitet wird. Sobald die Bestellung verarbeitet wurde, können Sie sie nicht mehr stornieren. 

![Auftrag stornieren](media/data-box-disk-deploy-ordered/cancel-order1.png)

Um eine stornierte Bestellung zu löschen, navigieren Sie zu **Übersicht** und klicken dann auf der Befehlsleiste auf **Löschen**. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Registrieren für einen Data Box-Datenträger
> * Bestellen des Data Box-Datenträgers
> * Nachverfolgen der Bestellung
> * Stornieren der Bestellung

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihren Data Box-Datenträger einrichten.

> [!div class="nextstepaction"]
> [Einrichten des Azure Data Box-Datenträgers](./data-box-disk-deploy-set-up.md)


