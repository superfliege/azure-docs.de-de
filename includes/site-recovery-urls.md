---
title: Includedatei
description: Includedatei
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: ae8eebf9667f2bc03fdc1082fb38c19c5c645c10
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060593"
---
**Name** | **Kommerzielle URL**  | **Behördliche URL** | **Beschreibung** |
--- | --- | --- | ---
Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Für Zugriffssteuerung und Identitätsverwaltung mit AAD verwendet 
Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Für die Übertragung und Koordinierung von Replikationsdaten verwendet 
Replikation | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet 
Speicher | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Für den Zugriff auf das Speicherkonto zum Speichern von replizierten Daten verwendet 
Telemetrie (optional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Für Telemetrie verwendet 
Zeitsynchronisierung | ``time.windows.com`` | ``time.nist.gov`` | Ssed werden zum Überprüfen der Zeitsynchronisierung zwischen Systemzeit und globaler Zeit in allen Bereitstellungen verwendet.


