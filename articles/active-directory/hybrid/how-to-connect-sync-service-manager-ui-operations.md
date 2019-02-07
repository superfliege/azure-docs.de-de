---
title: Azure AD Connect – Synchronization Service Manager-Vorgänge | Microsoft Docs
description: Grundlagen zur Registerkarte „Vorgänge“ in Synchronization Service Manager für Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7280314775856a438ee2084c93419a54d25996ed
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487335"
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Verwenden der Registerkarte „Vorgänge“ von Synchronization Service Manager

![Synchronization Service Manager](./media/how-to-connect-sync-service-manager-ui-operations/operations.png)

Die Registerkarte „Vorgänge“ zeigt die Ergebnisse der letzten Vorgänge. Diese Registerkarte ist sehr wichtig, um Probleme zu verstehen und zu beheben.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Grundlagen zu den Informationen, die in der Registerkarte „Vorgänge“ angezeigt werden
Die obere Hälfte zeigt alle Ausführungen in chronologischer Reihenfolge. Standardmäßig enthält das Vorgangsprotokoll Informationen der letzten sieben Tage. Diese Einstellung kann mit dem [Scheduler](how-to-connect-sync-feature-scheduler.md) geändert werden. Suchen Sie nach Ausführungen ohne Erfolgsstatus. Die Sortierung kann durch Klicken auf die Kopfzeilen geändert werden.

Die Spalte **Status** zeigt die wichtigste Information und das schwerwiegendste Problem einer Ausführung an. Hier folgt eine kurze Zusammenfassung der häufigsten Status, in der Reihenfolge ihrer Untersuchungspriorität (bei „*“ gibt es mehrere mögliche Fehlerzeichenfolgen).

| Status | Comment |
| --- | --- |
| stopped-\* |Die Ausführung konnte nicht abgeschlossen werden. Beispielsweise, wenn das Remotesystem ausgefallen ist und nicht kontaktiert werden kann. |
| stopped-error-limit |Es gibt mehr als 5.000 Fehler. Die Ausführung wurde aufgrund der großen Anzahl von Fehlern automatisch beendet. |
| completed-\*-errors |Die Ausführung wurde abgeschlossen, es sind jedoch Fehler (weniger als 5.000) aufgetreten, die untersucht werden sollten. |
| completed-\*-warnings |Die Ausführung wurde abgeschlossen, einige Daten weisen jedoch einen unerwarteten Zustand auf. Wenn Fehler auftreten, ist diese Meldung in der Regel nur ein Symptom. Bis Sie die Fehler behoben haben, sollten Sie keine Warnungen untersuchen. |
| Erfolg |Keine Probleme |

Wenn Sie eine Zeile auswählen, wird der untere Bereich aktualisiert, und die Details dieser Ausführung werden angezeigt. Ganz links neben dem unteren Teil erscheint möglicherweise eine Liste mit der **Schrittnummer**. Diese Liste wird nur angezeigt, wenn Ihre Gesamtstruktur mehrere Domänen enthält und jede Domäne als einzelner Schritt dargestellt wird. Den Domänennamen finden Sie unter der Überschrift **Partition**. Unter **Synchronization Statistics**(Synchronisierungsstatistik) finden Sie weitere Informationen zur Anzahl verarbeiteter Änderungen. Sie können auf die Links klicken, um eine Liste mit den geänderten Objekten anzuzeigen. Sind Objekte mit Fehlern vorhanden, werden diese Fehler unter **Synchronisierungsfehler**angezeigt.

Weitere Informationen finden Sie unter [Problembehandlung: Ein Objekt wird nicht synchronisiert](tshoot-connect-object-not-syncing.md)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](how-to-connect-sync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).
