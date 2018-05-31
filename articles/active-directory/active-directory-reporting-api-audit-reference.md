---
title: Referenz zur Azure Active Directory-Überwachungs-API | Microsoft Docs
description: Vorgehensweise zum Einstieg in die Azure Active Directory-Überwachungs-API
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2018
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: e620a7f488e51a60bff6943135831eea0d12816d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34158046"
---
# <a name="azure-active-directory-audit-api-reference"></a>Referenz zur Azure Active Directory-Überwachungs-API

> [!TIP] 
> Informieren Sie sich über die neue Microsoft Graph-API für die [Berichterstellung](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit), die diese API letztendlich ersetzen wird. 


Dieser Artikel ist Bestandteil einer Sammlung von Artikeln zur Azure Active Directory-Berichterstellungs-API (Azure AD). Die Azure AD-Berichterstellung bietet eine API, mit der Sie unter Verwendung von Code oder zugehörigen Tools auf Überwachungsdaten zugreifen können.
In diesem Artikel finden Sie Referenzinformationen zur **Überwachungs-API**.

Siehe:

* [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#activity-reports) erhalten Sie konzeptionelle Informationen.

* [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md) finden Sie weitere Informationen zur Berichterstellungs-API.


Informationen:

- Häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen](active-directory-reporting-faq.md). 

- Bei Problemen können Sie ein [Supportticket erstellen](active-directory-troubleshooting-support-howto.md). 


## <a name="who-can-access-the-data"></a>Wer kann auf die Daten zugreifen?
* Benutzer mit den Rollen „Sicherheitsadministrator“ oder der Berechtigung „Sicherheit lesen“
* Globale Administratoren
* Jede App mit Autorisierung für den Zugriff auf die API (die App-Autorisierung kann nur basierend auf der Berechtigung eines globalen Administrators eingerichtet werden)

## <a name="prerequisites"></a>Voraussetzungen
Um über die Berichterstellungs-API auf diesen Bericht zugreifen zu können, müssen folgende Bedingungen erfüllt sein:

* Sie verfügen über die [Free Edition von Azure Active Directory oder eine höhere Edition](active-directory-whatis.md)
* Die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](active-directory-reporting-api-prerequisites.md)sind erfüllt. 

## <a name="accessing-the-api"></a>Zugriff auf die API
Sie können entweder über den [Graph-Tester](https://graphexplorer2.cloudapp.net) oder programmgesteuert, z.B. unter Verwendung von PowerShell, auf diese API zugreifen. Verwenden Sie das Graviszeichen als Escapezeichen für das $-Zeichen, damit PowerShell die in AAD Graph-REST-Aufrufen verwendete OData-Filtersyntax richtig interpretieren kann. Das Graviszeichen dient als [Escapezeichen in PowerShell](https://technet.microsoft.com/library/hh847755.aspx) und erlaubt PowerShell eine zeichengetreue Interpretation des Zeichens „$“, statt dieses Zeichen als PowerShell-Variablenname (z.B. „$filter“) zu betrachten.

Der Schwerpunkt dieses Artikels liegt auf dem Graph-Tester. Ein PowerShell-Beispiel finden Sie in diesem [PowerShell-Skript](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-Endpunkt

Sie können auf diese API mithilfe des folgenden URI zugreifen:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Es gibt keine Beschränkung für die Anzahl von Datensätzen, die von der Azure AD-Berichterstellungs-API (mithilfe der OData-Paginierung) zurückgegeben werden. Informationen zu den Beschränkungen für die Aufbewahrung von Berichtsdaten finden Sie unter [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md).

Der Aufruf gibt die Daten in Batches zurück. Jeder Batch umfasst maximal 1.000 Datensätze. Um den nächsten Batch an Datensätzen abzurufen, verwenden Sie den Link **Weiter**. Rufen Sie die Skiptoken-Informationen für den ersten Satz mit zurückgegebenen Datensätzen ab. Die Skiptoken-Informationen befinden sich am Ende des Resultsets.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Unterstützte Filter

Sie können die Anzahl von Datensätzen, die für einen API-Aufruf zurückgegeben werden, mit einem Filter eingrenzen.  
Für anmeldungsbezogene API-Daten werden die folgenden Filter unterstützt:

* **$top=\<Anzahl zurückzugebender Datensätze\>**: Zum Einschränken der Anzahl von Datensätzen, die zurückgegeben werden. Dies ist ein kostenintensiver Vorgang. Verwenden Sie diesen Filter nicht, wenn Tausende von Objekten zurückgegeben werden müssen.     
* **$filter=\<Ihre Filteranweisung\>**: Legt basierend auf den unterstützten Filterfeldern fest, an welcher Art von Datensätzen Sie interessiert sind.

## <a name="supported-filter-fields-and-operators"></a>Unterstützte Filterfelder und Operatoren
Um festzulegen, an welcher Art von Datensätzen Sie interessiert sind, können Sie eine Filteranweisung mit einem der folgenden Filterfelder oder einer Kombination daraus erstellen:

* [activityDate](#activitydate): Definiert ein Datum oder einen Datumsbereich.
* [category](#category): Definiert die Kategorie, nach der gefiltert werden soll.
* [activityStatus](#activitystatus): Definiert den Status einer Aktivität.
* [activityType](#activitytype): Definiert den Typ einer Aktivität.
* [activity](#activity) : Definiert die Aktivität als Zeichenfolge.  
* [actor/name](#actorname): Definiert den Actor in Form des Actornamens.
* [actor/objectid](#actorobjectid) : Definiert den Akteur in Form der ID des Akteurs.   
* [actor/upn](#actorupn): Definiert den Actor als Benutzerprinzipalname (UPN) des Actors. 
* [actor/name](#targetname): Definiert das Ziel in Form des Actornamens.
* [target/objectid](#targetobjectid) : Definiert das Ziel in Form der ID des Ziels.  
* [target/upn](#targetupn) : Definiert das Ziel als Benutzerprinzipalname (UPN) des Akteurs.   

- - -
### <a name="activitydate"></a>activityDate
**Unterstützte Operatoren**: eq, ge, le, gt, lt

**Beispiel**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Hinweise**:

datetime muss im UTC-Format angegeben werden.

- - -
### <a name="category"></a>category

**Unterstützte Werte**:

| Category (Kategorie)                         | Wert     |
| :--                              | ---       |
| Kernverzeichnis                   | Verzeichnis |
| Self-Service-Kennwortverwaltung | SSPR      |
| Self-Service-Gruppenverwaltung    | SSGM      |
| Kontobereitstellung             | Synchronisierung      |
| Automatisiertes Kennwortrollover      | Automatisiertes Kennwortrollover |
| Schutz der Identität (Identity Protection)              | IdentityProtection |
| Invited Users (Eingeladene Benutzer)                    | Invited Users (Eingeladene Benutzer) |
| MIM Service (MIM-Dienst)                      | MIM Service (MIM-Dienst) |



**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>activityStatus

**Unterstützte Werte**:

| Aktivitätsstatus | Wert |
| :--             | ---   |
| Erfolgreich         | 0     |
| Fehler         | - 1   |

**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>activityType
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=activityType eq 'User'    

**Hinweise**:

Erfordert eine Beachtung der Groß-/Kleinschreibung.

- - -
### <a name="activity"></a>activity
**Unterstützte Operatoren**: eq, contains, startsWith

**Beispiel**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Hinweise**:

Erfordert eine Beachtung der Groß-/Kleinschreibung.

- - -
### <a name="actorname"></a>actor/name
**Unterstützte Operatoren**: eq, contains, startsWith

**Beispiel**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Hinweise**:

Groß-/Kleinschreibung muss nicht beachtet werden.

- - -
### <a name="actorobjectid"></a>actor/objectid
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>actor/name
**Unterstützte Operatoren**: eq, contains, startsWith

**Beispiel**:

    $filter=targets/any(t: t/name eq 'some name')    

**Hinweise**:

Groß-/Kleinschreibung nicht beachten

- - -
### <a name="targetupn"></a>target/upn
**Unterstützte Operatoren**: eq, startsWith

**Beispiel**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Hinweise**:

* Groß-/Kleinschreibung nicht beachten
* Fügen Sie bei der Abfrage von „Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity“ den vollständigen Namespace hinzu.

- - -
### <a name="targetobjectid"></a>target/objectid
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>actor/upn
**Unterstützte Operatoren**: eq, startsWith

**Beispiel**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Hinweise**:

* Groß-/Kleinschreibung nicht beachten 
* Fügen Sie bei der Abfrage von „Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity“ den vollständigen Namespace hinzu.

- - -
## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie Beispiele für gefilterte Systemaktivitäten anzeigen? Sehen Sie sich die [Beispiele zur Azure Active Directory-Überwachungs-API](active-directory-reporting-api-audit-samples.md)an.

- Sie möchten mehr über die Azure AD-Berichterstellungs-API erfahren? Lesen Sie den Artikel [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md).

