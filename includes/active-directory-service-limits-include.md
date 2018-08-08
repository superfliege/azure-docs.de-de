---
title: Includedatei
description: Includedatei
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 07/30/2018
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: a2055c3f00306fbfdad3028a16bb49d919e1e251
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39361431"
---
Nachstehend finden Sie die Verwendungs- und andere Diensteinschränkungen für den Azure Active Directory-Dienst (Azure AD).

| Category (Kategorie) | Grenzen |
| --- | --- |
| Verzeichnisse | Ein einzelner Benutzer kann als Mitglied oder Gast bis zu 500 Azure AD-Verzeichnissen angehören.<br/>Ein einzelner Benutzer kann maximal 20 Verzeichnisse erstellen. |
| Domänen | Sie können nicht mehr als 900 verwaltete Domänennamen hinzufügen. Wenn Sie all Ihre Domänen für den Verbund mit der lokalen Active Directory-Instanz einrichten möchten, können Sie in jedem Verzeichnis maximal 450 Domänennamen hinzufügen. |
| Objekte |<ul><li>Im Tarif Free von Azure Active Directory können Benutzer in einem Verzeichnis maximal 500.000 Objekte erstellen.</li><li>Ein Benutzer ohne Administratorrechte kann maximal 250 Objekte erstellen.</li></ul> |
| Schemaerweiterungen |<ul><li>Erweiterungen des Typs "String" sind auf maximal 256 Zeichen begrenzt. </li><li>Erweiterungen des Typs "Binary" sind auf 256 Bytes beschränkt.</li><li>100 Erweiterungswerte (für ALLE Typen und ALLE Anwendungen) können in jedes einzelne Objekt geschrieben werden.</li><li>Nur die Entitäten „User“, „Group“, „TenantDetail“, „Device“, „Application“ und „ServicePrincipal“ mit dem Typ „String“ oder „Binary“ können mit Einzelwertattributen erweitert werden.</li><li>Schemaerweiterungen sind nur in der Graph-API-Version "1.21-preview" verfügbar. Der Anwendung muss Schreibzugriff zum Registrieren einer Erweiterung gewährt werden.</li></ul> |
| ANWENDUNGEN |Maximal 100 Benutzer können Besitzer einer einzelnen Anwendung sein. |
| Gruppen |<ul><li>Maximal 100 Benutzer können Besitzer einer einzelnen Gruppe sein.</li><li>Eine beliebige Anzahl von Objekten kann einer einzelnen Gruppe in Azure Active Directory angehören.</li><li>Die Anzahl von Mitgliedern einer Gruppe, die Sie über Ihre lokale Active Directory-Instanz mit Azure Active Directory synchronisieren können, ist auf 50.000 beschränkt (bei Verwendung von Azure AD Connect).</li></ul> |
| Anpassung des Zugriffsbereichs |<ul><li>Für Benutzer mit Lizenzen für Azure AD Premium oder Enterprise Mobility Suite gibt es keine Beschränkung der Anzahl von Anwendungen, die pro Endbenutzer im Zugriffsbereich angezeigt werden.</li><li>Für Benutzer mit Lizenzen für die Edition „Free“ oder „Azure AD Basic“ von Azure Active Directory werden maximal zehn App-Kacheln (z.B. „Box“, „Salesforce“ oder „Dropbox“) im Zugriffsbereich angezeigt. Diese Beschränkung gilt nicht für Administratorkonten.</li></ul> |
| Berichte | In einem Bericht können maximal 1.000 Zeilen angezeigt oder heruntergeladen werden. Weitere Daten werden abgeschnitten. |
| Verwaltungseinheiten | Ein Objekt kann höchstens 30 Verwaltungseinheiten angehören. |
