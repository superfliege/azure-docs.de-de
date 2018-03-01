---
title: Azure AD Connect Health und die Datenschutz-Grundverordnung | Microsoft-Dokumentation
description: "Dieses Dokument beschreibt, wie Sie DSGVO-Konformität mit Azure AD Connect erzielen."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2018
ms.author: billmath
ms.openlocfilehash: b9a0b9027bbead00300040186e453933b3a7f46b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="gdpr-compliance-and-azure-ad-connect-health"></a>DSGVO-Konformität und Azure AD Connect Health 

Die [Datenschutz-Grundverordnung (DSGVO)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm) ist ein Datenschutzgesetz der Europäischen Union (EU). Die DSGVO enthält neue Vorschriften für Unternehmen, Regierungsbehörden, gemeinnützige Organisationen und andere Organisationen, die Menschen in der EU Waren und Dienstleistungen anbieten oder Daten in Bezug auf EU-Bürger sammeln und analysieren. 

Ihnen stehen schon heute Microsoft-Produkte und -Dienste zur Verfügung, mit denen Sie die DSGVO erfüllen können. Weitere Informationen zur Microsoft-Datenschutzrichtlinie finden Sie im [Trust Center](https://www.microsoft.com/trustcenter).

Azure AD Connect Health überwacht Ihre lokale Identitätsinfrastruktur und Synchronisierungsdienste. Darüber hinaus erhalten Sie Einblicke und Warnungen. Microsoft ist bestrebt, die Clouddienste bei Inkrafttreten der DSGVO im Mai 2018 DSGVO-konform zu gestalten und DSGVO-bezogene Zusicherungen in den eigenen vertraglichen Verpflichtungen bereitzustellen. 

>[!NOTE] 
> In diesem Artikel wird die DSGVO-Konformität in Azure AD Connect Health beschrieben. Weitere Informationen zur DSGVO-Konformität in Azure AD Connect finden Sie unter [DSGVO-Konformität und Azure AD Connect](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="gdpr-classification"></a>DSGVO-Klassifizierung
Azure AD Connect Health fällt in die Kategorie **Datenverarbeitung** der DSGVO-Klassifizierung. Als eine Datenverarbeitungs-Pipeline stellt der Dienst Datenverarbeitungsdienste für wichtige Partner und Endbenutzer bereit. Azure AD Connect Health generiert keine Benutzerdaten und kann nicht unabhängig steuern, welche personenbezogenen Daten erfasst und wie diese verwendet werden. Datenabruf, Aggregation, Analyse und Berichterstellung in Azure AD Connect Health basieren auf vorhandenen lokalen Daten. 

## <a name="data-retention-policy"></a>Datenaufbewahrungsrichtlinie
Azure AD Connect Health erstellt keine Berichte, führt keine Analysen aus und bietet keine Einblicke über 30 Tage hinaus. Aus diesem Grund erfolgt in Azure AD Connect Health keine Speicherung, Verarbeitung oder Beibehaltung von Daten über 30 Tage hinaus. Dieser Entwurf ist mit den DSGVO-Vorschriften, den Microsoft-Richtlinien für Datenschutz und Compliance und den Azure AD-Richtlinien zur Datenaufbewahrung konform. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Deaktivieren der Datensammlung und Überwachung in Azure AD Connect Health
Mithilfe von Azure AD Connect Health können Sie die Datensammlung für jeden überwachten Server einzeln oder für eine Instanz eines überwachten Diensts beenden. Sie können beispielsweise die Datensammlung für einzelne AD FS-Server (Active Directory Federation Services) beenden, die mit Azure AD Connect Health überwacht werden. Sie können auch die Datensammlung für die gesamte AD FS-Instanz beenden, die unter Verwendung von Azure AD Connect Health überwacht wird. In diesem Fall werden die entsprechenden Server nach dem Beenden der Datensammlung aus dem Azure AD Connect Health-Portal gelöscht. 

>[!IMPORTANT]
> Sie benötigen globale Administratorberechtigungen für Azure AD oder die Rolle „Mitwirkender“ in RBAC zum Löschen überwachter Server aus Azure AD Connect Health.
>
> Das Entfernen eines Servers oder einer Dienstinstanz aus Azure AD Connect Health kann nicht rückgängig gemacht werden. 

### <a name="what-to-expect"></a>Was können Sie erwarten?
Wenn Sie die Datensammlung und die Überwachung für einen einzelnen überwachten Server oder eine Instanz eines überwachten Diensts beenden möchten, beachten Sie Folgendes:

- Wenn Sie eine Instanz eines überwachten Diensts löschen, wird die Instanz aus der Liste des Azure AD Connect Health-Überwachungsdiensts im Portal entfernt. 
- Wenn Sie einen überwachten Server oder eine Instanz eines überwachten Diensts löschen, wird der Health-Agent NICHT deinstalliert oder von Ihren Servern entfernt. Der Health-Agent ist nicht für das Senden von Daten an Azure AD Connect Health konfiguriert. Sie müssen den Health-Agent manuell von zuvor überwachten Servern deinstallieren.
- Wenn Sie vor Ausführung dieses Schritts den Connect Health-Agent nicht deinstalliert haben, werden auf dem Server bzw. den Servern möglicherweise Fehlerereignisse in Zusammenhang mit dem Connect Health-Agent angezeigt.
- Alle Daten der Instanz des überwachten Diensts werden gemäß der Microsoft Azure-Datenaufbewahrungsrichtlinie gelöscht.

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Deaktivieren der Datensammlung und Überwachung für einen überwachten Server
Weitere Informationen finden Sie unter [Entfernen eines Servers aus Azure AD Connect Health](active-directory-aadconnect-health-operations.md#to-delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Deaktivieren der Datensammlung und Überwachung für eine Instanz eines überwachten Diensts
Weitere Informationen finden Sie unter [Entfernen einer Dienstinstanz aus Azure AD Connect Health](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).


## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Erneutes Aktivieren der Datensammlung und Überwachung in Azure AD Connect Health
Um die Überwachung in Azure AD Connect Health für einen zuvor gelöschten überwachten Dienst erneut zu aktivieren, müssen Sie den Health-Agent auf allen Servern deinstallieren und [erneut installieren](active-directory-aadconnect-health-agent-install.md).


## <a name="next-steps"></a>Nächste Schritte
* [Die Microsoft-Datenschutzrichtlinie im Trust Center](https://www.microsoft.com/trustcenter)
* [Azure AD Connect und die DSGVO](../../active-directory/connect/active-directory-aadconnect-gdpr.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
