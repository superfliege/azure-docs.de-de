---
title: Diagnose und Fehlerbehebung für Azure Import/Export-Aufträge | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die ausführliche Protokollierung für Aufträge des Microsoft Azure Import/Export-Diensts aktivieren.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 2a54752f933b91265d0aa8add61ca0707615931b
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526326"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnose und Fehlerbehebung für Azure Import/Export-Aufträge
Für jeden verarbeiteten Datenträger wird vom Azure Import/Export-Dienst im zugeordneten Speicherkonto ein Fehlerprotokoll erstellt. Sie können die ausführliche Protokollierung auch aktivieren, indem Sie die `LogLevel`-Eigenschaft `Verbose` festlegen, wenn Sie die Vorgänge [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) oder [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) aufrufen.

 Standardmäßig werden Protokolle in einen Container mit dem Namen `waimportexport` geschrieben. Sie können einen anderen Namen angeben, indem Sie beim Aufrufen der Vorgänge `Put Job` oder `Update Job Properties` die `DiagnosticsPath`-Eigenschaft festlegen. Die Protokolle werden mit der folgenden Benennungskonvention als Blockblobs gespeichert: `waies/jobname_driveid_timestamp_logtype.xml`.

 Sie können den URI der Protokolle für einen Auftrag abrufen, indem Sie den Vorgang [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) aufrufen. Der URI für das ausführliche Protokoll wird in der `VerboseLogUri`-Eigenschaft für jedes Laufwerk zurückgegeben, während der URI für das Fehlerprotokoll in der `ErrorLogUri`-Eigenschaft zurückgegeben wird.

Sie können die Protokollierungsdaten verwenden, um die folgenden Probleme zu ermitteln.

## <a name="drive-errors"></a>Laufwerkfehler

Folgende Fehler sind als Laufwerkfehler klassifiziert:

-   Fehler beim Zugreifen auf oder Lesen der Manifestdatei

-   Fehlerhafte BitLocker-Schlüssel

-   Lese-/Schreibfehler des Laufwerks

## <a name="blob-errors"></a>Blobfehler

Folgende Fehler sind als Blobfehler klassifiziert:

-   Falsche oder in Konflikt stehende Blobs oder Namen

-   Fehlende Dateien

-   Nicht gefundenes Blob

-   Abgeschnittene Dateien (die Dateien auf dem Datenträger sind kleiner als im Manifest angegeben)

-   Beschädigter Dateiinhalt (für Importaufträge, Erkennung per MD5-Prüfsummenfehler)

-   Beschädigte Blobmetadaten und Eigenschaftsdateien (Erkennung per MD5-Prüfsummenfehler)

-   Fehlerhaftes Schema für die Blobeigenschaften bzw. Metadatendateien

Es kann vorkommen, dass einige Teile eines Import- oder Exportauftrags nicht vollständig abgeschlossen werden, während der Gesamtauftrag trotzdem abgeschlossen wird. In diesem Fall können Sie die fehlenden Teile der Daten über das Netzwerk hoch- oder herunterladen, oder Sie können einen neuen Auftrag erstellen, um die Daten zu übertragen. Informationen dazu, wie Sie die Daten über das Netzwerk reparieren, finden Sie unter [Azure Import/Export-Tool – Referenz](storage-import-export-tool-how-to-v1.md).

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)
