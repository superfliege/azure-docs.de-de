---
title: Suchen von Benutzeraktivitätsberichten von Azure Active Directory im Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wo sich die Azure Active Directory-Benutzeraktivitätsberichte im Azure-Portal befinden.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: fab94088d1d54012a955b0663b078d03b13d6299
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624911"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Speicherorte von Aktivitätsberichten im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie Benutzeraktivitätsberichte von Azure Active Directory (Azure AD) im Azure-Portal finden.

## <a name="audit-logs-report"></a>Bericht „Überwachungsprotokolle“

Im Bericht „Überwachungsprotokolle“ sind mehrere Berichte zu Anwendungsaktivitäten in einer gemeinsamen Ansicht für die kontextbasierte Berichterstellung kombiniert. Greifen Sie wie folgt auf den Bericht „Überwachungsprotokolle“ zu:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Wählen Sie oben rechts Ihr Verzeichnis aus, und wählen Sie dann im linken Navigationsbereich das Blatt **Azure Active Directory**.
3. Wählen Sie auf dem Blatt „Azure Active Directory“ im Abschnitt **Aktivität** die Option **Überwachungsprotokolle**. 

    ![Überwachungsprotokolle](./media/howto-find-activity-reports/482.png "Überwachungsprotokolle")

Im Bericht „Überwachungsprotokolle“ sind die folgenden Berichte zusammengefasst:

* Überwachungsbericht
* Kennwortzurücksetzungsaktivität
* Aktivität "Registrierung für Zurücksetzen des Kennworts"
* Self-Service-Gruppenaktivität
* Namensänderungen für Office 365-Gruppen
* Kontobereitstellungsaktivität
* Status des Kennwortrollovers
* Kontobereitstellungsfehler

### <a name="filtering-on-audit-logs"></a>Filtern nach Überwachungsprotokollen

Sie können die erweiterte Filterung im Überwachungsbericht verwenden, um auf eine bestimmte Kategorie von Überwachungsdaten zuzugreifen, indem Sie diese im Filter **Aktivitätskategorie** angeben. Wenn Sie beispielsweise alle Aktivitäten anzeigen möchten, die sich auf die Self-Service-Kennwortzurücksetzung beziehen, wählen Sie die Kategorie **Self-Service-Kennwortzurücksetzung**. 

    ![Category options on the Filter Audit Logs page](./media/howto-find-activity-reports/06.png "Category options on the Filter Audit Logs page")

Zu Aktivitätskategorien zählen:

- Kernverzeichnis
- Self-Service-Kennwortverwaltung
- Self-Service-Gruppenverwaltung
- Kontobereitstellung


## <a name="sign-ins-report"></a>Bericht zu Anmeldeaktivitäten 

Die Ansicht **Anmeldungen** enthält alle Benutzeranmeldungen und den Bericht **Anwendungsnutzung**. Informationen zur Anwendungsnutzung finden Sie zudem in der Übersicht über die **Unternehmensanwendungen** im Abschnitt **Verwalten**.

    ![Enterprise applications](./media/howto-find-activity-reports/484.png "Enterprise applications")

Greifen Sie wie folgt auf den Bericht „Anmeldungen“ zu:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Wählen Sie oben rechts Ihr Verzeichnis aus, und wählen Sie dann im linken Navigationsbereich das Blatt **Azure Active Directory**.
3. Wählen Sie auf dem Blatt „Azure Active Directory“ im Abschnitt **Aktivität** die Option **Anmeldungen**. 

    ![Ansicht „Anmeldungen“](./media/howto-find-activity-reports/483.png "Ansicht „Anmeldungen“")


### <a name="filtering-on-application-name"></a>Filtern nach Anwendungsname

Sie können den Bericht „Anmeldungen“ verwenden, um Details zur Anwendungsnutzung anzuzeigen, indem Sie nach dem Benutzernamen oder Anwendungsnamen filtern.

![Seite mit Filter nach Anmeldeereignissen](./media/howto-find-activity-reports/07.png "Seite mit Filter nach Anmeldeereignissen")

## <a name="security-reports"></a>Sicherheitsberichte

### <a name="anomalous-activity-reports"></a>Berichte zu anomalen Aktivitäten

Mit Berichten zu anomalen Aktivitäten werden Informationen zu sicherheitsbezogenen Risikoereignissen bereitgestellt, die von Azure AD erkannt und für die Berichte erstellt werden können.

Die folgende Tabelle listet die Sicherheitsberichte zur anormalen Aktivität von Azure AD und die entsprechenden Risikoereignistypen im Azure-Portal auf. Weitere Informationen finden Sie unter [Azure Active Directory-Risikoereignisse](concept-risk-events.md).  


| Bericht zur anormalen Aktivität von Azure AD |  Typ des Identity Protection-Risikoereignisses|
| :--- | :--- |
| Benutzer mit kompromittierten Anmeldeinformationen | Kompromittierte Anmeldeinformationen |
| Irreguläre Anmeldeaktivitäten | Unmöglicher Ortswechsel zu atypischen Orten |
| Anmeldungen von möglicherweise infizierten Geräten | Anmeldungen von infizierten Geräten|
| Anmeldungen von unbekannten Quellen | Anmeldungen von anonymen IP-Adressen |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten | Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |
| - | Anmeldungen von unbekannten Standorten |

Folgende Sicherheitsberichte zur anormalen Aktivität von Azure AD gehören nicht zu Risikoereignissen im Azure-Portal:

* Anmeldungen nach mehreren Fehlern
* Anmeldungen aus mehreren geografischen Regionen


### <a name="detected-risk-events"></a>Erkannte Risikoereignisse

Sie können im [Azure-Portal](https://portal.azure.com) auf dem Blatt **Azure Active Directory** im Abschnitt **Sicherheit** auf die Berichte zu erkannten Risikoereignissen zugreifen. Die erkannten Risikoereignisse werden in den folgenden Berichten nachverfolgt:   

- [Gefährdete Benutzer](concept-user-at-risk.md)
- [Riskante Anmeldungen](concept-risky-sign-ins.md)

    ![Sicherheitsberichte](./media/howto-find-activity-reports/04.png "Sicherheitsberichte")

## <a name="next-steps"></a>Nächste Schritte

* [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](concept-audit-logs.md)
* [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](concept-sign-ins.md)
* [Azure Active Directory-Risikoereignisse](concept-risk-events.md)
