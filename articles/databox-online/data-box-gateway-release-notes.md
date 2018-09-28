---
title: Anmerkungen zu dieser Vorschauversion von Azure Data Box Gateway | Microsoft-Dokumentation
description: In diesem Artikel werden offene Probleme und Lösungen in der Vorschauversion von Azure Data Box Gateway beschrieben.
services: databox-edge-gateway
documentationcenter: ''
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 7619056ace5d9b3cf083a40a6cfa06a0cac0561e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949688"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Anmerkungen zu dieser Vorschauversion von Azure Data Box Gateway

## <a name="overview"></a>Übersicht

In den folgenden Anmerkungen werden die kritischen offenen und die behobenen Probleme für die Vorschauversion von Microsoft Azure Data Box Gateway beschrieben.

Die Versionshinweise werden fortlaufend aktualisiert, und wenn schwerwiegende Probleme festgestellt werden, die eine Problemumgehung erfordern, werden sie hinzugefügt. Lesen Sie vor der Bereitstellung Ihres Data Box Gateways die Informationen in den Anmerkungen zu dieser Version sorgfältig durch.

Die Vorschauversion entspricht der Softwareversion **Data Box Gateway Preview-Version 2.0**.

## <a name="issues-fixed-in-preview-release"></a>In der Vorschauversion behobene Probleme

Die folgende Tabelle enthält eine Zusammenfassung der Probleme, die in dieser Version behoben wurden:

| Nr. | Problem |
| --- | --- |
| 1 | Wenn in dieser Version eine Datei, die von einem anderen Tool (AzCopy) hochgeladen wurde, aktualisiert und dann so aktualisiert wird, dass sich die Dateigröße erhöht, tritt der folgende Fehler auf: *Fehler 400: InvalidBlobOrBlock (Der angegebene Blob- oder Blockinhalt ist ungültig.)*|


## <a name="known-issues-in-preview-release"></a>Bekannte Probleme in dieser Vorschauversion

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Vorschauversion von Data Box Gateway.

| Nr. | Feature | Problem | Problemumgehung/Kommentare |
| --- | --- | --- | --- |
| **1.** |Aktualisierungen |Die in den früheren Vorschauversionen erstellten Data Box Gateway-Geräte können nicht auf diese Version aktualisiert werden. |Laden Sie die Images der virtuellen Datenträger aus der neuen Version herunter, konfigurieren Sie neue Geräte, und stellen Sie sie bereit. Weitere Informationen finden Sie unter [Vorbereiten der Bereitstellung von Azure Data Box Gateway](data-box-gateway-deploy-prep.md). |
| **2.** |Bereitgestellter Datenträger |Nachdem Sie einen Datenträger einer bestimmten Größe bereitgestellt und das entsprechende Data Box Gateway erstellt haben, dürfen Sie den Datenträger nicht verkleinern. Der Versuch, den Datenträger zu verkleinern, führt zum Verlust aller lokalen Daten auf dem Gerät. | |
| **3.** |Aktualisieren |In dieser Version können Sie immer nur eine Freigabe gleichzeitig aktualisieren. | |
| **4.** |Umbenennen |Das Umbenennen von Objekten wird nicht unterstützt. |Kontaktieren Sie den Microsoft-Support, falls dies für Ihren Workflow erforderlich ist. |
| **5.** |Kopieren| Wenn eine schreibgeschützte Datei auf das Gerät kopiert wird, bleibt der Schreibschutz nicht erhalten. | |
| **6.** |Protokolle| Aufgrund eines Fehlers in dieser Version kann der Fehlercode 110 in *error.xml* mit nicht erkennbaren Elementnamen auftreten. | |
| **7.** |Hochladen | Aufgrund eines Fehlers in dieser Version kann während des Uploads bestimmter Dateien der Fehlercode 2003 auftreten. | |
| **8.** |Löschen | Aufgrund eines Fehlers in dieser Version wird beim Löschen einer NFS-Freigabe die Freigabe möglicherweise nicht gelöscht. Als Status der Freigabe wird *Wird gelöscht* angezeigt.  |Dies erfolgt nur, wenn die Freigabe einen nicht unterstützten Dateinamen verwendet. |
| **9.** |Onlinehilfe |Die Links des Typs „Hilfe“ im Azure-Portal sind möglicherweise nicht mit der Dokumentation verknüpft.|Diese Links werden in der allgemein verfügbaren Version funktionieren. |



## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Data Box Gateway](data-box-gateway-deploy-prep.md)


