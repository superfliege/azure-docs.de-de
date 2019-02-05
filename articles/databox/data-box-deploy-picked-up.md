---
title: Zurücksenden der Microsoft Azure Data Box | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Azure Data Box an Microsoft zurücksenden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/24/2019
ms.author: alkohli
ms.openlocfilehash: 158c2a8919bccea03f5c7b67aef23cd07022c259
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900509"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Tutorial: Zurücksenden der Azure Data Box und Überprüfen des Datenuploads in Azure

In diesem Tutorial wird beschrieben, wie Sie die Azure Data Box zurücksenden und die in Azure hochgeladenen Daten überprüfen.

In diesem Tutorial werden folgende Themen behandelt:

> [!div class="checklist"]
> * Voraussetzungen
> * Vorbereiten des Versands
> * Senden der Data Box an Microsoft
> * Überprüfen des Datenuploads in Azure
> * Löschen von Daten von der Data Box

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

- Sie haben die Schritte ausgeführt in [Tutorial: Kopieren von Daten auf die Azure Data Box Disk und Durchführen der Überprüfung](data-box-deploy-copy-data.md). 
- Alle Kopieraufträge sind abgeschlossen. „Für Versand vorbereiten“ kann nicht ausgeführt werden, wenn noch Kopieraufträge ausgeführt werden.

## <a name="prepare-to-ship"></a>Vorbereiten des Versands

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

## <a name="ship-data-box-back"></a>Zurücksenden der Data Box

1. Stellen Sie sicher, dass das Gerät ausgeschaltet ist und die Kabel entfernt wurden. Wickeln Sie das mit dem Gerät gelieferte Netzkabel auf, und befestigen Sie es sicher an der Rückseite des Geräts.
2. Wenn das Gerät in den USA versendet wird, vergewissern Sie sich, dass das Adressetikett auf der E-Ink-Anzeige angezeigt wird, und vereinbaren Sie einen Abholtermin mit Ihrem Transportunternehmen. Falls das Adressetikett beschädigt ist oder nicht in der E-Ink-Anzeige angezeigt wird, navigieren Sie im Azure-Portal zu **Übersicht > Versandetikett herunterladen**. Laden Sie das Versandetikett herunter, und bringen Sie es am Gerät an.

    Wenn das Gerät in Europa versendet wird, wird das Adressetikett nicht auf der E-Ink-Anzeige angezeigt. Das Adressetikett für den Rückversand befindet sich stattdessen in einer durchsichtigen Tasche unter dem Adressetikett für den Hinversand. Entfernen Sie das alte Adressetikett, und achten Sie darauf, dass das Adressetikett gut sichtbar ist.
    
3. Planen Sie die Abholung durch UPS, falls Sie das Gerät zurücksenden. Rufen Sie zum Planen der Abholung eine UPS-Niederlassung in Ihrer Nähe (landesspezifische gebührenfreie Nummer) an, oder geben Sie die Data Box am nächstgelegenen Abgabeort ab.

4. Nachdem die Data Box vom Kurierdienst abgeholt und eingescannt wurde, wird der Status der Bestellung im Portal in **Abgeholt** geändert. Außerdem wird eine Nachverfolgungs-ID angezeigt.

## <a name="verify-data-upload-to-azure"></a>Überprüfen des Datenuploads in Azure

Nachdem das Gerät bei Microsoft eingegangen ist und gescannt wurde, wird der Status der Bestellung in **Empfangen** geändert. Das Gerät wird dann physisch auf Schäden oder Anzeichen einer Manipulation überprüft.

Nach der Überprüfung wird die Data Box mit dem Netzwerk im Azure-Datencenter verbunden. Das Kopieren der Daten wird automatisch gestartet. Je nach Datengröße kann der Kopiervorgang einige Stunden oder auch einige Tage dauern. Sie können den Status des Kopiervorgangs im Portal verfolgen.

Nachdem der Kopiervorgang abgeschlossen ist, wird der Auftragsstatus in **Completed** (Abgeschlossen) geändert.

Stellen Sie sicher, dass sich Ihre Daten in den Speicherkonten befinden, bevor Sie sie aus der Quelle löschen. Wenn Sie die Daten in Data Box kopieren, werden die Daten abhängig vom Typ in einen der folgenden Pfade in Ihrem Azure Storage-Konto hochgeladen:

- Blockblobs und Seitenblobs: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
- Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

Alternativ hierzu können Sie auch im Azure-Portal auf Ihr Azure-Speicherkonto zugreifen und von dort aus entsprechend navigieren.

## <a name="erasure-of-data-from-data-box"></a>Löschen von Daten von der Data Box
 
Nachdem die Daten in Azure hochgeladen wurden, löscht die Data Box die Daten auf den Datenträgern gemäß den [NIST-Richtlinien (SP 800-88 Revision 1)](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
> * Voraussetzungen
> * Vorbereiten des Versands
> * Senden der Data Box an Microsoft
> * Überprüfen des Datenuploads in Azure
> * Löschen von Daten von der Data Box

Im folgenden Artikel erfahren Sie, wie Sie Data Box über die lokale Webbenutzeroberfläche verwalten.

> [!div class="nextstepaction"]
> [Verwenden der lokalen Webbenutzeroberfläche zum Verwalten von Azure Data Box](./data-box-local-web-ui-admin.md)


