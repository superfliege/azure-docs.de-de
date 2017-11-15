---
title: "Anzeigen und Verwalten von StorSimple-Aufträgen | Microsoft Docs"
description: "Beschreibt die Seite „Aufträge“ des StorSimple Manager-Diensts und ihre Verwendung zum Nachverfolgen kürzlich ausgeführter, aktueller und geplanter Sicherungsaufträge."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 44b3550a2457a9f43a54117c6164858a0ef6f095
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>Anzeigen und Verwalten von StorSimple-Aufträgen mithilfe des StorSimple Manager-Diensts (Update 2)
> [!NOTE]
> Das klassische Portal für StorSimple ist veraltet. Ihre StorSimple-Geräte-Manager werden gemäß dem Zeitplan für die Abschaltung automatisch in das neue Azure-Portal verschoben. Sie erhalten zu dieser Verschiebung eine E-Mail und eine Portalbenachrichtigung. Dieses Dokument wird ebenfalls bald entfernt. Die Version dieses Artikel für das neue Azure-Portal finden Sie unter [Anzeigen und Verwalten von Aufträgen mithilfe des StorSimple-Geräte-Manager-Diensts (Update 3 und höher)](storsimple-8000-manage-jobs-u2.md). Antworten auf Fragen zu dieser Verschiebung finden Sie unter [Verschieben des StorSimple Device Manager-Diensts vom klassischen Portal in das Azure-Portal: häufig gestellte Fragen (FAQ)](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Übersicht
Die Seite **Aufträge** stellt ein zentrales Portal zum Anzeigen und Verwalten von Aufträgen bereit, die für Geräte gestartet wurden, die mit dem StorSimple Manager-Dienst verbunden sind. Sie können geplante, aktive, abgeschlossene, abgebrochene und fehlerhafte Aufträge für mehrere Geräte anzeigen. Die jeweiligen Ergebnisse werden in einem Tabellenformat angezeigt. 

![Seite „Aufträge“](./media/storsimple-manage-jobs-u2/jobs.png)

Sie können die Aufträge, an denen Sie interessiert sind, schnell finden, indem Sie nach folgenden Feldern filtern:

* **Status** – Aufträge können einen der folgenden Status haben: „Wird ausgeführt“, „Fehler“, „Abgeschlossen“, „Wird abgebrochen“, „Abgebrochen“ oder „Mit Fehlern abgeschlossen“.
* **Von und Bis** – Aufträge können entsprechend einem Datums- und Zeitbereich gefiltert werden.
* **Typ** – Aufträge können einen der folgenden Typen haben: „Sicherung“, „Manuelle Sicherung“, „Wiederherstellung“, „Klon“, „Gerätefailover“, „Create locally pinned volume“, „Volume ändern“, „Aktualisieren“, „Supportpaket“ oder „Virtuelle Gerätebereitstellung“.
* **Geräte** – Aufträge werden für ein bestimmtes Gerät ausgelöst, das mit Ihrem Dienst verbunden ist.
  Die gefilterten Aufträge werden dann anhand der folgenden Attribute tabellarisch aufgelistet:
  
  * **Typ** – „Sicherung“, „Manuelle Sicherung“, „Wiederherstellung“, „Klon“, „Gerätefailover“, „Create locally pinned volume“, „Volume ändern“, „Aktualisieren“, „Supportpaket“ oder „Virtuelle Gerätebereitstellung“.
  * **Status** – „Wird ausgeführt“, „Fehler“, „Abgeschlossen“, „Wird abgebrochen“, „Abgebrochen“ oder „Mit Fehlern abgeschlossen“.
  * **Entität** – Die Aufträge können einem Volume, einer Sicherungsrichtlinie oder einem Gerät zugeordnet sein. Ein Klonauftrag ist beispielsweise einem Volume zugeordnet, während ein geplanter Sicherungsauftrag einer Sicherungsrichtlinie zugeordnet ist. Ein Geräteauftrag wird aufgrund eines Notfallwiederherstellungs- oder eines Wiederherstellungsvorgangs erstellt.
  * **Gerät** – Der Name des Geräts, für den der Auftrag gestartet wurde.
  * **Gestartet am** – der Zeitpunkt, zu dem der Auftrag gestartet wurde.
  * **Status** – Der Prozentsatz, bis zu dem ein Auftrag abgearbeitet ist, der ausgeführt wird. Für einen abgeschlossenen Auftrag muss dieser Wert immer gleich 100 % sein.

Die Liste der Aufträge wird alle 30 Sekunden aktualisiert.

Sie können auf dieser Seite die folgenden auftragsbezogenen Aktionen ausführen:

* Anzeigen von Auftragsdetails
* Abbrechen eines Auftrags

## <a name="view-job-details"></a>Anzeigen von Auftragsdetails
Führen Sie die folgenden Schritte aus, um die Details eines Auftrags anzuzeigen.

#### <a name="to-view-job-details"></a>So zeigen Sie Auftragsdetails an
1. Zeigen Sie auf der Seite **Aufträge** die Aufträge an, an denen Sie interessiert sind, indem Sie eine Abfrage mit entsprechenden Filtern ausführen. Sie können nach abgeschlossenen, ausführenden oder abgebrochenen Aufträgen suchen.
2. Wählen Sie einen Auftrag aus.
3. Klicken Sie unten auf der Seite auf **Details**.
4. Im Dialogfeld **Sicherung Auftragsdetails** können Sie den Status, die Details, Zeitstatistiken und Datenstatistiken sehen.
   
    ![Seite „Auftragsdetails“](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Abbrechen eines Auftrags
Führen Sie die folgenden Schritte aus, um einen Auftrag abzubrechen, der momentan ausgeführt wird.

> [!NOTE]
> Einige Aufträge, wie z. B. das Ändern eines Volumetyps oder das Erweitern eines Volumes, können nicht abgebrochen werden.
> 
> 

### <a name="to-cancel-a-job"></a>So brechen Sie einen Auftrag ab
1. Zeigen Sie auf der Seite **Aufträge** die Aufträge an, die momentan ausgeführt werden und die Sie abbrechen möchten. Führen Sie dazu eine Abfrage mit entsprechenden Filtern aus.
2. Wählen Sie den Auftrag aus.
3. Klicken Sie unten auf der Seite auf **Abbrechen**.
4. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**.

Dieser Auftrag wird nun abgebrochen.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Verwalten von StorSimple-Sicherungsrichtlinien](storsimple-manage-backup-policies.md).
* Erfahren Sie mehr über das [Verwalten Ihres StorSimple-Geräts mithilfe des StorSimple Manager-Diensts](storsimple-manager-service-administration.md).

