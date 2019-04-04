---
title: 'Problembehandlung: Fehlende Daten in den heruntergeladenen Azure Active Directory-Aktivitätsprotokollen | Microsoft-Dokumentation'
description: Bietet Ihnen eine Lösung für fehlende Daten in heruntergeladenen Azure Active Directory-Aktivitätsprotokollen.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: cfb56ea81abeeba83bee73356c682b3e9fae866f
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58292910"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Ich finde nicht alle Daten in den Azure Active Directory-Aktivitätsprotokollen, die ich heruntergeladen habe.

## <a name="symptoms"></a>Symptome

Ich habe die Aktivitätsprotokolle (Überwachung oder Anmeldungen) heruntergeladen, und für den ausgewählten Zeitraum werden nicht alle Datensätze angezeigt. Warum? 

 ![Berichterstellung](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Ursache

Beim Herunterladen von Aktivitätsprotokollen im Azure-Portal gilt eine Obergrenze von 250.000 Datensätzen, die nach Aktualität sortiert werden. 

## <a name="resolution"></a>Lösung

Mithilfe von [Azure AD-Berichterstellungs-APIs](concept-reporting-api.md) können jederzeit bis zu einer Millionen Datensätze abgerufen werden.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Active Directory-Berichte: häufig gestellte Fragen](reports-faq.md)

