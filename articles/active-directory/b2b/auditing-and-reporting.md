---
title: Überwachung eines Azure Active Directory B2B-Zusammenarbeitsbenutzers und Berichterstellung | Microsoft-Dokumentation
description: Die Eigenschaften für Gastbenutzer sind in der Azure Active Directory B2B-Zusammenarbeit konfigurierbar.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fc36128f8f998d78dd2cf9ef112fe5961bbef5b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204609"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>Überwachung eines B2B-Zusammenarbeitsbenutzers und Berichterstellung
Für Gastbenutzer können Sie vergleichbare Überwachungsfunktionen einrichten wie für Mitglieder. 

## <a name="access-reviews"></a>Zugriffsüberprüfungen
Mithilfe von Zugriffsüberprüfungen können Sie in regelmäßigen Abständen überprüfen, ob Gastbenutzer weiterhin Zugriff auf Ihre Ressourcen benötigen. Das Feature **Zugriffsüberprüfungen** steht in **Azure Active Directory** unter **Verwalten** > **Organisationsbeziehungen** zur Verfügung. (Sie können auch im Azure-Portal unter **Alle Dienste** nach „Zugriffsüberprüfungen“ suchen.) Weitere Informationen zu Zugriffsüberprüfungen finden Sie unter [Verwalten des Gastzugriffs mit Azure AD-Zugriffsüberprüfungen](../governance/manage-guest-access-with-access-reviews.md).

## <a name="audit-logs"></a>Überwachungsprotokolle

Azure AD-Überwachungsprotokolle enthalten erfasste System- und Benutzeraktivitäten. Dazu zählen auch Aktivitäten, die von Gastbenutzern initiiert wurden. Die Überwachungsprotokolle finden Sie in **Azure Active Directory** unter **Überwachung** > **Überwachungsprotokolle**. Hier sehen Sie ein Beispiel für die Einladung und den Einlösungsverlauf des eingeladenen Benutzers „Sam Oogle“:

![Überwachungsprotokoll](./media/auditing-and-reporting/audit-log.png)

Sie können für jedes dieser Ereignisse weitere Details anzeigen. Sehen wir uns zum Beispiel einmal die Details zur Annahme an.

![Aktivitätsdetails](./media/auditing-and-reporting/activity-details.png)

Sie können diese Protokolle auch aus Azure AD exportieren und ein Berichterstellungstool Ihrer Wahl verwenden, um benutzerdefinierte Berichte zu erstellen.

### <a name="next-steps"></a>Nächste Schritte

- [Eigenschaften von B2B-Zusammenarbeitsbenutzern](user-properties.md)

