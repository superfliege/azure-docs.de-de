---
title: Azure AD Connect Health – Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“ | Microsoft Docs
description: In diesem Dokument wird beschrieben, warum die Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“ angezeigt wird und welche Schritte zur Problembehandlung Sie ausführen können.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 430ea5f0a6f737d7632a4352c24d893368b80558
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46310379"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Warnung „Die Daten des Integritätsdiensts sind nicht aktuell“

## <a name="overview"></a>Übersicht
<li>Azure AD Connect Health generiert diese Warnung über Daten, wenn Connect Health zwei Stunden lang nicht alle Datenpunkte vom Server empfangen konnte. Der Titel der Warnung lautet **Die Daten des Integritätsdiensts sind nicht aktuell**. </li>
<li>Die Statusbenachrichtigung **Warnung** wird ausgelöst, wenn Connect Health zwei Stunden lang keine partiellen Datenelemente empfängt, die vom Server gesendet wurden. Die Statusbenachrichtigung „Warnung“ löst keine E-Mail-Benachrichtigungen an den Mandantenadministrator aus. </li>
<li>Die Statusbenachrichtigung **Fehler** wird ausgelöst, wenn Connect Health zwei Stunden lang keine vom Server gesendeten Datenelemente empfängt. Die Statusbenachrichtigung „Fehler“ löst E-Mail-Benachrichtigungen an den Mandantenadministrator aus. </li>

>[!IMPORTANT] 
> Diese Warnung folgt der [Datenaufbewahrungsrichtlinie](reference-connect-health-user-privacy.md#data-retention-policy) von Connect Health.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung 
* Lesen Sie unbedingt den [Abschnitt „Anforderungen“](how-to-connect-health-agent-install.md#requirements), und stellen Sie sicher, dass Sie die Anforderungen erfüllen.
* Verwenden Sie das [Tool zum Testen der Konnektivität](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service), um Konnektivitätsprobleme zu ermitteln.


## <a name="next-steps"></a>Nächste Schritte
* [Azure AD Connect Health – FAQ](reference-connect-health-faq.md)
