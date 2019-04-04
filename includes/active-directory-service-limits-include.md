---
title: Includedatei
description: Includedatei
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/21/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 38f2dd301ddc2a5f8d28322856b2011bd2034c30
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554680"
---
Nachstehend finden Sie die Verwendungs- und andere Diensteinschränkungen für den Azure Active Directory-Dienst (Azure AD).

| Category (Kategorie) | Grenzen |
| --- | --- |
| Verzeichnisse | Ein einzelner Benutzer kann als Mitglied oder Gast bis zu 500 Azure AD-Verzeichnissen angehören.<br/>Ein einzelner Benutzer kann maximal 20 Verzeichnisse erstellen. |
| Domänen | Sie können nicht mehr als 900 verwaltete Domänennamen hinzufügen. Wenn Sie alle Ihre Domänen für den Verbund mit der lokalen Active Directory-Instanz einrichten möchten, können Sie in jedem Verzeichnis maximal 450 Domänennamen hinzufügen. |
| Objekte |<ul><li>Im Tarif Free von Azure Active Directory können Benutzer in einem Verzeichnis maximal 500.000 Objekte erstellen.</li><li>Ein Benutzer ohne Administratorrechte kann maximal 250 Objekte erstellen. Zu diesem Kontingent zählen sowohl aktive Objekte als auch gelöschte Objekte, die zum Wiederherstellen verfügbar sind. Nur gelöschte Objekte, die vor weniger als 30 Tagen gelöscht wurden, stehen für die Wiederherstellung bereit. Gelöschte Objekte, die nicht mehr für die Wiederherstellung verfügbar sind, zählen für 30 Tage mit einem Viertelwert zu diesem Kontingent. Sie können eventuell Benutzern, die keine Administratoren sind, [eine Administratorrolle zuweisen](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md), falls diese dieses Kontingent bei ihrer normalen Arbeit sehr wahrscheinlich regelmäßig überschreiten.</li></ul> |
| Schemaerweiterungen |<ul><li>Erweiterungen des Typs „String“ sind auf maximal 256 Zeichen begrenzt. </li><li>Erweiterungen des Typs „Binary“ sind auf 256 Byte beschränkt.</li><li>Es können maximal 100 Erweiterungswerte (für *alle* Typen und *alle* Anwendungen) in jedes einzelne Objekt geschrieben werden.</li><li>Nur die Entitäten „User“, „Group“, „TenantDetail“, „Device“, „Application“ und „ServicePrincipal“ mit dem Typ „String“ oder „Binary“ können mit Einzelwertattributen erweitert werden.</li><li>Schemaerweiterungen sind nur in der Graph-API-Version „1.21-preview“ verfügbar. Der Anwendung muss Schreibzugriff zum Registrieren einer Erweiterung gewährt werden.</li></ul> |
| ANWENDUNGEN |Maximal 100 Benutzer können Besitzer einer einzelnen Anwendung sein. |
| Gruppen |<ul><li>Maximal 100 Benutzer können Besitzer einer einzelnen Gruppe sein.</li><li>Eine beliebige Anzahl von Objekten kann einer einzelnen Gruppe angehören.</li><li>Ein Benutzer kann ein Mitglied einer beliebigen Anzahl von Gruppen sein.</li><li>Die Anzahl von Mitgliedern einer Gruppe, die Sie über Ihre lokale Active Directory-Instanz mit Azure Active Directory synchronisieren können, ist bei Verwendung von Azure AD Connect auf 50.000 beschränkt.</li></ul> |
| Anpassung des Zugriffsbereichs |<ul><li>Es gibt keine Beschränkung für die Anzahl der Anwendungen, die im Zugriffsbereich pro Benutzer angezeigt werden können. Dies gilt für Benutzer mit zugewiesenen Lizenzen für Azure AD Premium oder die Enterprise Mobility Suite.</li><li>Maximal 10 App-Kacheln können für jeden Benutzer im Zugriffsbereich angezeigt werden. Dieser Grenzwert gilt für Benutzer, denen Lizenzen für die kostenlose Edition oder Azure AD Basic von Azure Active Directory zugewiesen wurden. Beispiele für App-Kacheln sind Box, Salesforce oder Dropbox. Diese Beschränkung gilt nicht für Administratorkonten.</li></ul> |
| Berichte | In einem Bericht können maximal 1.000 Zeilen angezeigt oder heruntergeladen werden. Weitere Daten werden abgeschnitten. |
| Verwaltungseinheiten | Ein Objekt kann höchstens 30 Verwaltungseinheiten angehören. |
| Administratorrollen und -berechtigungen | <li>Eine Gruppe kann nicht als Besitzer hinzugefügt werden.<li>Eine Gruppe kann nicht einer Rolle zugewiesen werden.<li>Die Standardbenutzerberechtigungen können nicht geändert werden. Dies gilt jedoch nicht für Mandantenschalter, die in Azure AD Benutzereinstellungen sind. |
