---
title: Anmerkungen zu dieser Vorschauversion von Azure Data Box Gateway | Microsoft-Dokumentation
description: In diesem Artikel werden offene Probleme und Lösungen in der Vorschauversion von Azure Data Box Gateway beschrieben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/29/2019
ms.author: alkohli
ms.openlocfilehash: 34e7a578fb04b617221376f5d1065997b12d2613
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302120"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Anmerkungen zu dieser Vorschauversion von Azure Data Box Gateway

## <a name="overview"></a>Übersicht

In den folgenden Anmerkungen werden die kritischen offenen und die behobenen Probleme für die Vorschauversion von Microsoft Azure Data Box Gateway beschrieben.

Die Versionshinweise werden fortlaufend aktualisiert, und wenn schwerwiegende Probleme festgestellt werden, die eine Problemumgehung erfordern, werden sie hinzugefügt. Lesen Sie vor der Bereitstellung Ihres Data Box Gateways die Informationen in den Anmerkungen zu dieser Version sorgfältig durch.

Die Vorschauversion entspricht der Softwareversion **Data Box Gateway Preview-Version 2.0**.

## <a name="issues-fixed-in-preview-release"></a>In der Vorschauversion behobene Probleme

Die folgende Tabelle enthält eine Zusammenfassung der Probleme, die in dieser Version behoben wurden:

|  Nein. | Problem |
| --- | --- |
| **1.** | Wenn in dieser Version eine Datei, die von einem anderen Tool (AzCopy) hochgeladen wurde, aktualisiert und dann so aktualisiert wird, dass sich die Dateigröße erhöht, tritt der folgende Fehler auf: *Fehler 400: InvalidBlobOrBlock (Der angegebene Blob- oder Blockinhalt ist ungültig.)*|
| **2.** |Aufgrund eines Fehlers in dieser Version kann der Fehlercode 110 in *error.xml* mit nicht erkennbaren Elementnamen auftreten. | 
| **3.** |Aufgrund eines Fehlers in dieser Version kann während des Uploads bestimmter Dateien der Fehlercode 2003 auftreten. | 
| **4.** |In dieser Version können Sie immer nur eine Freigabe gleichzeitig aktualisieren. | 


## <a name="known-issues-in-preview-release"></a>Bekannte Probleme in dieser Vorschauversion

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Vorschauversion von Data Box Gateway.

|  Nein. | Feature | Problem | Problemumgehung/Kommentare |
| --- | --- | --- | --- |
| **1.** |Aktualisierungen |Die in den früheren Vorschauversionen erstellten Data Box Gateway-Geräte können nicht auf diese Version aktualisiert werden. |Laden Sie die Images der virtuellen Datenträger aus der neuen Version herunter, konfigurieren Sie neue Geräte, und stellen Sie sie bereit. Weitere Informationen finden Sie unter [Vorbereiten der Bereitstellung von Azure Data Box Gateway](data-box-gateway-deploy-prep.md). |
| **2.** |Bereitgestellter Datenträger |Nachdem Sie einen Datenträger einer bestimmten Größe bereitgestellt und das entsprechende Data Box Gateway erstellt haben, dürfen Sie den Datenträger nicht verkleinern. Der Versuch, den Datenträger zu verkleinern, führt zum Verlust aller lokalen Daten auf dem Gerät. | |
| **3.** |Umbenennen |Das Umbenennen von Objekten wird nicht unterstützt. |Kontaktieren Sie den Microsoft-Support, falls dies für Ihren Workflow erforderlich ist. |
| **4.** |Kopieren| Wenn eine schreibgeschützte Datei auf das Gerät kopiert wird, bleibt der Schreibschutz nicht erhalten. | |
| **5.** |Dateitypen | Die folgenden Linux-Dateitypen werden nicht unterstützt: Zeichendateien, Blockdateien, Sockets, Pipes, symbolische Verknüpfungen.  |Das Kopieren dieser Dateien führt dazu, dass auf der NFS-Freigabe Dateien mit Nulllänge erstellt werden. Diese Dateien verbleiben in einem Fehlerzustand und werden außerdem in *error.xml* gemeldet. |
| **6.** |Löschen | Aufgrund eines Fehlers in dieser Version wird beim Löschen einer NFS-Freigabe die Freigabe möglicherweise nicht gelöscht. Als Status der Freigabe wird *Wird gelöscht* angezeigt.  |Dies erfolgt nur, wenn die Freigabe einen nicht unterstützten Dateinamen verwendet. |
| **7.** |Aktualisieren | Berechtigungen und Zugriffssteuerungslisten (ACLs) werden über einen Aktualisierungsvorgang hinaus nicht beibehalten.  | |
| **8.** |Kopieren | Beim Kopieren von Daten wird folgender Fehler ausgegeben:  Der angeforderte Vorgang konnte aufgrund einer Dateisystemeinschränkung nicht ausgeführt werden.  |Dieser Fehler tritt auf, wenn der mit der Datei verknüpfte alternative Datenstrom 128 KB überschreitet (Höchstwert für ReFS).  |
| **9.** |Symbolische Links |Symbolische Verknüpfungen werden nicht unterstützt.  |Symbolische Verknüpfungen mit Verzeichnissen führen dazu, dass Verzeichnisse niemals als offline markiert werden. Infolgedessen wird möglicherweise kein graues Kreuz auf den Verzeichnissen angezeigt, mit dem gekennzeichnet wird, dass die Verzeichnisse offline sind und alle zugehörigen Inhalte vollständig nach Azure hochgeladen wurden. |
| **10.** |Onlinehilfe |Die Links des Typs „Hilfe“ im Azure-Portal sind möglicherweise nicht mit der Dokumentation verknüpft.|Diese Links werden in der allgemein verfügbaren Version funktionieren. |



## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Data Box Gateway](data-box-gateway-deploy-prep.md)


