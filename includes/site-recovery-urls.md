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
ms.openlocfilehash: f4cd9cad3813378fcdc3f06e8ab1c28eced4f93c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66116006"
---
NAME | Kommerzielle URL | Behördliche URL | BESCHREIBUNG
--- | --- | --- | ---
Azure Active Directory | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Die URLs werden für die Zugriffssteuerung und die Identitätsverwaltung durch Azure Active Directory verwendet. 
Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Wird für die Übertragung und Koordinierung von Replikationsdaten verwendet.
Replikation | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Wird für die Vorgänge und Koordinierung der Replikationsverwaltung verwendet.
Storage | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Wird für den Zugriff auf das Speicherkonto zum Speichern von replizierten Daten verwendet.
Telemetrie (optional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Für Telemetrie verwendet
Zeitsynchronisierung | ``time.windows.com`` | ``time.nist.gov`` | Zum Überprüfen der Zeitsynchronisierung zwischen Systemzeit und globaler Zeit in allen Bereitstellungen verwendet


