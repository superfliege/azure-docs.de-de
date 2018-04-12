---
title: Azure Stack-Wartungsrichtlinie | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zur Azure Stack-Wartungsrichtlinie und erfahren, wie Sie den unterstützten Zustand eines integrierten Systems aufrechterhalten.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2018
ms.author: mabrigg
ms.openlocfilehash: e37b63580d8cea4b5772bc54f7b2f79980afc0bc
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack-Wartungsrichtlinie
Dieser Artikel beschreibt die Wartungsrichtlinie für integrierte Azure Stack-Systeme, und was Sie tun müssen, um den unterstützten Zustand Ihres Systems aufrechtzuerhalten. 

## <a name="update-package-types"></a>Updatepakettypen

Es gibt zwei Arten von Updatepaketen für integrierte Systeme: Softwareupdates von Microsoft und spezifische Updates Ihres Originalgerätehersteller-Hardwareanbieters (Original Equipment Manufacturer, OEM) wie Treiber und Firmware. Diese Updates werden als separate Azure Stack-Updatepakete bereitgestellt und unabhängig voneinander verwaltet.

- **Softwareupdates von Microsoft**. Microsoft ist für den End-to-End-Wartungslebenszyklus der Softwareupdatepakete von Microsoft verantwortlich. Diese Pakete können die neuesten Sicherheitsupdates von Windows Server, nicht sicherheitsrelevante Updates und Updates von Azure Stack-Feature enthalten. Sie können diese Updatepakete direkt von Microsoft herunterladen.
- **Updates des OEM-Hardwareanbieters**. Azure Stack-Hardwarepartner sind für den End-to-End-Wartungslebenszyklus (inklusive Leitung) der hardwarebezogenen Firmware- und Treiberupdatepakete verantwortlich. Darüber hinaus obliegt den Azure Stack-Hardwarepartnern die Leitung für die gesamte Software und Hardware des Hardwarelebenszyklus-Hosts. Die OEM-Hardwareanbieter hosten diese Updatepakete auf ihren Downloadwebsites.

## <a name="update-package-release-cadence"></a>Updatepaketrelease-Intervall

Microsoft veröffentlicht Softwareupdatepakete in der Regel monatlich. Allerdings ist es möglich, dass in einem Monat mehrere oder keine Updatereleases veröffentlicht werden. OEM-Hardwareanbieter veröffentlichen ihre Updates nach Bedarf.

Die folgenden Benennungskonventionen eines Microsoft-Updatepakets erleichtern Ihnen, das Veröffentlichungsdatum zu identifizieren:

*MajorProductVersion.MinorProductVersion.YYMMDD.BuildNumber*

Beispielsweise könnte ein am 15. Juni 2017 veröffentlichtes Microsoft-Softwareupdate die Versionsnummer „1.0.170615.1“ haben.

## <a name="keep-your-system-under-support"></a>Halten Sie den Support für Ihr System aufrecht
Um weiterhin Support zu erhalten, muss Ihre Azure Stack-Bereitstellung auf dem aktuellen Stand bleiben. Die Richtlinie für das Aufschieben von Updates ist, dass Support für Azure Stack nur bestehen bleibt, wenn die zuletzt veröffentlichte Updateversion oder eine der beiden vorherigen wesentlichen Updateversionen ausgeführt wird.  Hotfixes werden nicht als wesentliche Updateversionen angesehen.  Wenn in Ihrer Azure Stack-Cloud *mehr als zwei Updates* fehlen, wird sie als nicht kompatibel betrachtet und muss zumindest auf die unterstützte Mindestversion aktualisiert werden, um Support zu erhalten. 

Beispiel: Wenn die neueste verfügbare Updateversion 1805 ist und die beiden vorherigen Updatepakete die Versionen 1804 und 1803 waren, besteht für 1803 und 1804 Support. Allerdings gibt es für 1802 keinen Support. Die Richtlinie gilt, wenn es für einen oder zwei Monate kein Release gab. Beispiel: Wenn die aktuelle Version 1805 ist und es keine Version 1804 gab, besteht für die vorherigen beiden Updatepakete 1803 und 1802 weiterhin Support.

Microsoft-Softwareupdatepakete sind nicht kumulativ und setzen das vorherige Updatepaket voraus. Wenn Sie eines oder mehrere Updates aufschieben möchten, berücksichtigen Sie die gesamte Laufzeit, wenn Sie auf die neueste Version aktualisieren möchten. 


## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Updates in Azure Stack](azure-stack-updates.md)


