---
title: Zurücksenden der Microsoft Azure Data Box | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Azure Data Box an Microsoft zurücksenden.
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
ms.date: 10/03/2018
ms.author: alkohli
ms.openlocfilehash: 05bfba9fffa7db75ef4e1a1167b3170a775e1d34
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785459"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Tutorial: Zurücksenden der Azure Data Box und Überprüfen des Datenuploads in Azure

In diesem Tutorial wird beschrieben, wie Sie die Azure Data Box zurücksenden und die in Azure hochgeladenen Daten überprüfen.

In diesem Tutorial werden folgende Themen behandelt:

> [!div class="checklist"]
> * Senden der Data Box an Microsoft
> * Überprüfen des Datenuploads in Azure
> * Löschen von Daten von der Data Box

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie das [Tutorial zum Kopieren von Daten auf die Azure Data Box und Durchführen der Überprüfung](data-box-deploy-copy-data.md) durchgearbeitet haben.

## <a name="ship-data-box-back"></a>Zurücksenden der Data Box

1. Stellen Sie sicher, dass das Gerät ausgeschaltet ist und die Kabel entfernt wurden. Wickeln Sie das mit dem Gerät gelieferte Netzkabel auf, und befestigen Sie es sicher an der Rückseite des Geräts.
2. Wenn das Gerät in den USA versendet wird, vergewissern Sie sich, dass das Adressetikett auf der E-Ink-Anzeige angezeigt wird, und vereinbaren Sie einen Abholtermin mit Ihrem Transportunternehmen. Falls das Adressetikett beschädigt oder nicht mehr vorhanden ist, können Sie es aus dem Azure-Portal herunterladen und dann am Gerät anbringen. Navigieren Sie zu **Übersicht > Versandetikett herunterladen**. 

    Wenn das Gerät in Europa versendet wird, wird das Adressetikett nicht auf der E-Ink-Anzeige angezeigt. Das Adressetikett für den Rückversand befindet sich stattdessen in einer durchsichtigen Tasche unter dem Adressetikett für den Hinversand. Entfernen Sie das alte Adressetikett, und achten Sie darauf, dass das Adressetikett gut sichtbar ist.
    
3. Planen Sie die Abholung durch UPS, falls Sie das Gerät in den USA zurücksenden. Wenn Sie das Gerät in Europa per DHL zurücksenden, können Sie die Abholung durch DHL anfordern, indem Sie auf der DHL-Website die Luftfrachtbrief-Nummer angeben. Greifen Sie auf die DHL Express-Website für Ihr Land zu, und wählen Sie **Abholung buchen > eReturn-Abholung buchen**. 

    Geben Sie die Luftfrachtbrief-Nummer an, und klicken Sie auf **Abholung beauftragen**, um die Abholung anzufordern.

4. Nachdem die Data Box vom Kurierdienst abgeholt und eingescannt wurde, wird der Status der Bestellung im Portal in **Abgeholt** geändert. Außerdem wird eine Nachverfolgungs-ID angezeigt.

## <a name="verify-data-upload-to-azure"></a>Überprüfen des Datenuploads in Azure

Nachdem das Gerät bei Microsoft eingegangen ist und gescannt wurde, wird der Status der Bestellung in **Empfangen** geändert. Das Gerät wird dann physisch auf Schäden oder Anzeichen einer Manipulation überprüft. 

Nach der Überprüfung wird die Data Box mit dem Netzwerk im Azure-Datencenter verbunden. Das Kopieren der Daten wird automatisch gestartet. Je nach Datengröße kann der Kopiervorgang einige Stunden oder auch einige Tage dauern. Sie können den Status des Kopiervorgangs im Portal verfolgen.

Nachdem der Kopiervorgang abgeschlossen ist, wird der Auftragsstatus in **Completed** (Abgeschlossen) geändert.

Stellen Sie sicher, dass sich Ihre Daten in den Speicherkonten befinden, bevor Sie sie aus der Quelle löschen. 

## <a name="erasure-of-data-from-data-box"></a>Löschen von Daten von der Data Box
 
 Nachdem die Daten in Azure hochgeladen wurden, löscht die Data Box die Daten auf den Datenträgern gemäß den [NIST-Richtlinien (SP 800-88 Revision 1)](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Senden der Data Box an Microsoft
> * Überprüfen des Datenuploads in Azure
> * Löschen von Daten von der Data Box

Im folgenden Artikel erfahren Sie, wie Sie Data Box über die lokale Webbenutzeroberfläche verwalten.

> [!div class="nextstepaction"]
> [Verwenden der lokalen Webbenutzeroberfläche zum Verwalten von Azure Data Box](./data-box-local-web-ui-admin.md)


