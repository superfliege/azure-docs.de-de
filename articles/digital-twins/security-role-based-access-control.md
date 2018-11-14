---
title: Grundlegendes zu rollenbasierter Zugriffssteuerung in Azure Digital Twins | Microsoft-Dokumentation
description: Informationen zur Authentifizierung in Digital Twins mit rollenbasierter Zugriffssteuerung.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: b9ccdb9030a24520be8f24f757c279241f3a07e1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014787"
---
# <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Azure Digital Twins ermöglicht präzise Zugriffssteuerung für bestimmte Daten, Ressourcen und Aktionen in Ihrem Raumgraphen. Dies geschieht durch eine granulare Rollen- und Berechtigungsverwaltung namens „Rollenbasierte Zugriffssteuerung“ (Role-Based Access Control, RBAC). RBAC besteht aus _Rollen_ und _Rollenzuweisungen_. Rollen bestimmen die Berechtigungsebene. Rollenzuweisungen ordnen einem Benutzer oder Gerät eine Rolle zu.

Mit RBAC können Berechtigungen zugewiesen werden für:

- Einen Benutzer
- Ein Gerät
- Ein Dienstprinzipal
- Eine benutzerdefinierte Funktion 
- Alle Benutzer, die einer Domäne angehören. 
- Einen Mandanten
 
Die Zugriffsebene kann ebenfalls optimiert werden.

RBAC ist besonders, weil Berechtigungen im Raumgraphen von oben nach vererbt werden.

## <a name="what-can-i-do-with-rbac"></a>Welche Möglichkeiten bietet RBAC?

Ein Entwickler kann RBAC für folgende Aufgaben verwenden:

* Einem Benutzer die Berechtigung gewähren, Geräte für ein ganzes Gebäude oder nur für einen bestimmten Raum oder eine bestimmte Etage zu verwalten
* Einem Administrator globalen Zugriff auf alle Raumgraphknoten für einen gesamten Graphen oder nur für einen Abschnitt des Graphen gewähren
* Einem Supportspezialisten Lesezugriff auf den Graphen gewähren, wobei Zugriffsschlüssel ausgenommen sind
* Jedem Mitglied einer Domäne Lesezugriff auf alle Graphobjekte gewähren

## <a name="rbac-best-practices"></a>Best Practices für RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Rollen

### <a name="role-definitions"></a>Rollendefinitionen

Eine Rollendefinition ist eine Sammlung von Berechtigungen und wird gelegentlich als Rolle bezeichnet. Eine Rollendefinition enthält die zulässigen Vorgänge, wozu „Erstellen“, „Lesen“, „Aktualisieren“ und „Löschen“ gehören. In ihr ist auch angegeben, für welche Objekte diese Berechtigungen gelten.

Die folgenden Rollen sind in Azure Digital Twins verfügbar:

* **Raumadministrator**: Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen für den angegebenen Raum und alle Knoten darunter. Globale Berechtigung.
* **Benutzeradministrator**: Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Benutzern und benutzerbezogenen Objekten. Leseberechtigung für Räume.
* **Geräteadministrator**: Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Geräten und gerätebezogenen Objekten. Leseberechtigung für Räume.
* **Schlüsseladministrator**: Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Zugriffsschlüsseln. Leseberechtigung für Räume.
* **Tokenadministrator**: Berechtigung zum Lesen und Aktualisieren von Zugriffsschlüsseln. Leseberechtigung für Räume.
* **Benutzer**: Leseberechtigung für Räume, Sensoren und Benutzer, einschließlich der dazugehörigen Objekte.
* **Support Specialist**: (Supportspezialist) Leseberechtigung für alle Objekte außer Zugriffsschlüsseln.
* **Geräteinstallierer**: Berechtigung zum Lesen und Aktualisieren von Geräten und Sensoren, einschließlich der dazugehörigen Objekte. Leseberechtigung für Räume.
* **Gateway Device**: (Gateway-Gerät) Berechtigung zum Erstellen von Sensoren. Leseberechtigung für Geräte und Sensoren, einschließlich der dazugehörigen Objekte.

>[!NOTE]
> Um die vollständigen Definitionen für die vorherigen Rollen abzurufen, fragen Sie die System-/Rollen-API ab.

### <a name="object-types"></a>Objekttypen

`ObjectIdType` bezieht sich auf den Typ der Identität, der eine Rolle zugewiesen wird. Mit Ausnahme der Typen `DeviceId` und `UserDefinedFunctionId` entsprechen die Typen einer Eigenschaft eines Azure Active Directory-Objekts (Azure AD-Objekt):
  
* Mit dem Typ `UserId` wird einem Benutzer eine Rolle zugewiesen.
* Mit dem Typ `DeviceId` wird einem Gerät eine Rolle zugewiesen.
* Mit dem Typ `DomainName` wird einem Domänennamen eine Rolle zugewiesen. Jeder Benutzer mit dem angegebenen Domänennamen hat die Zugriffsrechte der entsprechenden Rolle.
* Mit dem Typ `TenantId` wird einem Mandanten eine Rolle zugewiesen. Jeder Benutzer, der zu der angegebenen Azure AD-Mandanten-ID gehört, hat die Zugriffsrechte der entsprechenden Rolle.
* Mit dem Typ `ServicePrincipalId` wird einer Dienstprinzipalobjekt-ID eine Rolle zugewiesen.
* Mit dem Typ `UserDefinedFunctionId` wird einer benutzerdefinierten Funktion (User-defined Function, UDF) eine Rolle zugewiesen.

> [!div class="nextstepaction"]
> [Abfragen der Objekt-ID eines Benutzers](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Abrufen der Objekt-ID eines Dienstprinzipals](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Abrufen der Objekt-ID eines Azure AD-Mandanten](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Rollenzuweisungen

Um einem Empfänger Berechtigungen zuzuweisen, erstellen Sie eine Rollenzuweisung. Um die Berechtigung zu widerrufen, entfernen Sie die Rollenzuweisung. Eine Azure Digital Twins-Rollenzuweisung verknüpft ein Objekt, z.B. einen Benutzer oder einen Azure AD-Mandant, mit einer Rolle und einem Raum. Berechtigungen werden für alle Objekte gewährt, die zu diesem Raum gehören. Der Raum umfasst den gesamten Raumgraphen darunter.

Angenommen, ein Benutzer hat eine Rollenzuweisung mit der Rolle `DeviceInstaller` für den Stammknoten eines Raumgraphen, der einem Gebäude entspricht. Der Benutzer kann dann Geräte für diesen Knoten und alle anderen untergeordneten Räume im Gebäude lesen und aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Sicherheit von Azure Digital Twins finden unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md).
