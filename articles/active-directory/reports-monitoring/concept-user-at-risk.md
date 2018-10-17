---
title: Sicherheitsbericht „Benutzer mit Risikomarkierung“ im Azure Active Directory-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über den Sicherheitsbericht „Benutzer mit Risikomarkierung“ im Azure Active Directory-Portal.
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/14/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: fde063cb593ca1f610dc35cd044fe41e34ab9202
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578358"
---
# <a name="users-flagged-for-risk-security-report-in-the-azure-active-directory-portal"></a>Sicherheitsbericht „Benutzer mit Risikomarkierung“ im Azure Active Directory-Portal

Mit den Sicherheitsberichten in Azure Active Directory (Azure AD) erhalten Sie Einblicke in die Wahrscheinlichkeit für kompromittierte Benutzerkonten in Ihrer Umgebung. 

Azure Active Directory erkennt verdächtige Aktionen im Zusammenhang mit Ihren Benutzerkonten. Für jede erkannte Aktion wird ein Datensatz mit der Bezeichnung *Risikoereignis* erstellt. Weitere Informationen finden Sie unter [Azure Active Directory-Risikoereignisse](concept-risk-events.md). 

Die erkannten Risikoereignisse werden zum Berechnen folgender Werte verwendet:

- **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter [Gewusst wie: Konfigurieren von Richtlinien zum Anmelderisiko](../identity-protection/howto-sign-in-risk-policy.md). 

- **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter [Gewusst wie: Konfigurieren von Richtlinien zum Benutzerrisiko](../identity-protection/howto-user-risk-policy.md).  

Im Azure-Portal befinden sich die Sicherheitsberichte auf dem Blatt **Azure Active Directory** im Abschnitt **Sicherheit**.  

![Riskante Anmeldungen](./media/concept-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Welche Azure AD-Lizenz benötigen Sie für den Zugriff auf einen Sicherheitsbericht?  

In allen Editionen von Azure Active Directory stehen Sicherheitsberichte zu Benutzern mit Risikomarkierung zur Verfügung.  
Die Granularitätsebene von Berichten kann für die einzelnen Editionen aber variieren: 

- In den **Free- und Basic-Editionen von Azure Active Directory** erhalten Sie bereits eine Liste mit Benutzern mit Risikomarkierung. 

- Mit der Edition **Azure Active Directory Premium 1** wird dieses Modell erweitert, indem Sie zusätzlich jeweils einige zugrunde liegende Risikoereignisse untersuchen können, die für einen Bericht erkannt wurden. 

- In der Edition **Azure Active Directory Premium 2** erhalten Sie die ausführlichsten Informationen zu allen zugrunde liegenden Risikoereignissen, und Sie können Sicherheitsrichtlinien konfigurieren, mit denen automatisch auf konfigurierte Risikostufen reagiert wird.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory – Free und Basic Edition

Der Bericht „Benutzer mit Risikomarkierung“ in den Editionen Free und Basic von Azure Active Directory enthält eine Liste mit Benutzerkonten, die unter Umständen kompromittiert wurden. 


![Riskante Anmeldungen](./media/concept-user-at-risk/03.png)

Wenn Sie einen Benutzer auswählen, wird das entsprechende Blatt mit den Benutzerdaten geöffnet.
Sie können für gefährdete Benutzer den Anmeldeverlauf des Benutzers prüfen und bei Bedarf das Kennwort zurücksetzen.

![Riskante Anmeldungen](./media/concept-user-at-risk/46.png)


In diesem Dialogfenster steht Ihnen folgende Option zur Verfügung:

- Herunterladen des Berichts

- Suchen nach Benutzern

![Riskante Anmeldungen](./media/concept-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory – Premium Editionen

Der Bericht „Benutzer mit Risikomarkierung“ in den Premium-Editionen von Azure Active Directory enthält Folgendes:

- Eine Liste mit Benutzerkonten, die unter Umständen kompromittiert wurden 

- Aggregierte Informationen zu den erkannten [Risikoereignistypen](concept-risk-events.md)

- Option zum Herunterladen des Berichts

- Option zum Konfigurieren einer [Richtlinie zum Beheben des Benutzerrisikos](../identity-protection/howto-user-risk-policy.md)  


![Riskante Anmeldungen](./media/concept-user-at-risk/71.png)

Wenn Sie einen Benutzer auswählen, erhalten Sie eine ausführliche Berichtsansicht für diesen Benutzer mit folgenden Optionen:

- Öffnen der Ansicht „Alle Anmeldungen“

- Das Kennwort des Benutzers zurücksetzen

- Verwerfen aller Ereignisse

- Untersuchen der gemeldeten Risikoereignisse für den Benutzer 


![Riskante Anmeldungen](./media/concept-user-at-risk/324.png)


Um ein Risikoereignis zu untersuchen, wählen Sie es in der Liste aus, um das Blatt **Details** für dieses Risikoereignis zu öffnen. Auf dem Blatt **Details** können Sie ein Risikoereignis manuell schließen oder ein manuell geschlossenes Risikoereignis wieder aktivieren. 


![Riskante Anmeldungen](./media/concept-user-at-risk/325.png)



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Active Directory Identity Protection finden Sie unter [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

