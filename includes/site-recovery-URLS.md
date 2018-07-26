---
title: Includedatei
description: Includedatei
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/28/2018
ms.author: raynew
ms.openlocfilehash: f7d6c3f68618fec839ccff06b73ba44d106999d2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38765537"
---
| NAME | Kommerzielle URL | Behördliche URL | BESCHREIBUNG |
|---|---|---|---|
| Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Für Zugriffssteuerung und Identitätsverwaltung mit AAD verwendet |
| Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Für die Übertragung und Koordinierung von Replikationsdaten verwendet |
| Replikation | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet |
| Speicher | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Für den Zugriff auf das Speicherkonto zum Speichern von replizierten Daten verwendet |
| Telemetrie (optional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Für Telemetrie verwendet |

``time.nist.gov`` und ``time.windows.com`` werden zum Überprüfen der Zeitsynchronisierung zwischen Systemzeit und globaler Zeit in allen Bereitstellungen verwendet.


