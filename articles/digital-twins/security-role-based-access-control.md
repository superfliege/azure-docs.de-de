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
ms.openlocfilehash: 7a6d8565a0f85b4cb81d9f5f23b04fe6b2edc53e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323835"
---
# <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Azure Digital Twins ermöglicht präzise Zugriffssteuerung für bestimmte Daten, Ressourcen und Aktionen in Ihrem Raumgraphen. Dies geschieht durch eine granulare Rollen- und Berechtigungsverwaltung namens _Rollenbasierte Zugriffssteuerung_. Rollenbasierte Zugriffssteuerung besteht aus _Rollen_ (entsprechen Berechtigungsebenen) und aus _Rollenzuweisungen_ (entsprechen den Zuordnungen von Rollen zu Benutzern oder Geräten).

Durch Verwenden von rollenbasierter Zugriffssteuerung kann eine Berechtigung einem Benutzer, einem Gerät, einem Dienstprinzipal, einer benutzerdefinierten Funktion, allen Benutzern, die zu einer Domäne gehören, oder einem Mandanten gewährt werden. Darüber hinaus kann der Grad des Zugriffs feinjustiert werden.

Rollenbasierte Zugriffssteuerung ist besonders, weil Berechtigungen im Raumgraphen von oben nach vererbt werden.

## <a name="what-can-i-do-with-role-based-access-control"></a>Welche Möglichkeiten bietet rollenbasierte Zugriffssteuerung?

Ein Entwickler kann rollenbasierte Zugriffssteuerung zu Folgendem verwenden:

* Einem Benutzer die Berechtigung gewähren, Geräte für ein ganzes Gebäude oder nur für einen bestimmten Raum oder eine bestimmte Etage zu verwalten
* Einem Administrator globalen Zugriff auf alle Raumgraphknoten für einen gesamten Graphen oder nur für einen Abschnitt des Graphen gewähren
* Einem Supportspezialisten Lesezugriff auf den Graphen gewähren, wobei Zugriffsschlüssel ausgenommen sind
* Jedem Mitglied einer Domäne Lesezugriff auf alle Graphobjekte gewähren

## <a name="role-based-access-control-best-practices"></a>Rollenbasierte Zugriffssteuerung – bewährte Methoden

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Rollen

### <a name="role-definitions"></a>Rollendefinitionen

Eine **Rollendefinition** ist eine Sammlung von Berechtigungen und wird gelegentlich als **Rolle** bezeichnet. Eine Rollendefinition enthält die zulässigen Vorgänge, wozu *Erstellen*, *Lesen*, *Aktualisieren* und *Löschen* gehören. In ihr ist auch angegeben, für welche Objekte diese Berechtigungen gelten.

Die folgenden Rollen sind in Azure Digital Twins verfügbar:

* **Space Administrator**: (Raumadministrator) Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen für den angegebenen Raum und alle Knoten darunter. Globale Berechtigung.
* **User Administrator**: (Benutzeradministrator) Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Benutzern und benutzerbezogenen Objekten. Leseberechtigung für Räume.
* **Device Administrator**: (Geräteadministrator) Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Geräten und gerätebezogenen Objekten. Leseberechtigung für Räume.
* **Key Administrator**: (Schlüsseladministrator) Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Zugriffsschlüsseln. Leseberechtigung für Räume.
* **Token Administrator**: (Tokenadministrator) Berechtigung zum Lesen und Aktualisieren von Zugriffsschlüsseln. Leseberechtigung für Räume.
* **Benutzer**: Leseberechtigung für Räume, Sensoren und Benutzer, einschließlich der dazugehörigen Objekte.
* **Support Specialist**: (Supportspezialist) Leseberechtigung für alle Objekte außer Zugriffsschlüsseln.
* **Device Installer**: (Geräteinstallierer) Berechtigung zum Lesen und Aktualisieren von Geräten und Sensoren, einschließlich der dazugehörigen Objekte. Leseberechtigung für Räume.
* **Gateway Device**: (Gateway-Gerät) Berechtigung zum Erstellen von Sensoren. Leseberechtigung für Geräte und Sensoren, einschließlich der dazugehörigen Objekte.

>[!NOTE]
> *Die vollständigen Definitionen für die hier aufgeführten Rollen können abgerufen werden, indem die System/Rollen-API abgefragt wird.*

### <a name="object-types"></a>Objekttypen

`ObjectIdType` bezieht sich auf den Typ der Identität, der eine Rolle zugewiesen wird. Mit Ausnahme der Typen `DeviceId` und `UserDefinedFunctionId` entsprechen die Typen einer Eigenschaft eines Azure Active Directory-Objekts (Azure AD-Objekt):
  
* Mit dem Typ `UserId` wird einem Benutzer eine Rolle zugewiesen.
* Mit dem Typ `DeviceId` wird einem Gerät eine Rolle zugewiesen.
* Mit dem Typ `DomainName` wird einem Domänennamen eine Rolle zugewiesen. Jeder Benutzer mit dem angegebenen Domänennamen hat die Zugriffsrechte der entsprechenden Rolle.
* Mit dem Typ `TenantId` wird einem Mandanten eine Rolle zugewiesen. Jeder Benutzer, der zu der angegebenen Azure AD-Mandanten-ID gehört, hat die Zugriffsrechte der entsprechenden Rolle.
* Mit dem Typ `ServicePrincipalId` wird einer Dienstprinzipalobjekt-ID eine Rolle zugewiesen.
* Mit dem Typ `UserDefinedFunctionId` wird einer benutzerdefinierten Funktion eine Rolle zugewiesen.

> [!div class="nextstepaction"]
> [Abfragen der Objekt-ID eines Benutzers](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Abrufen der Objekt-ID eines Dienstprinzipals](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Abrufen der Objekt-ID eines Azure AD-Mandanten](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Rollenzuweisungen

Berechtigungen werden einem Empfänger durch Erstellen einer Rollenzuweisung erteilt und durch Entfernen einer Rollenzuweisung widerrufen. Eine Azure Digital Twins-Rollenzuweisung verknüpft ein Objekt (Benutzer, Azure AD-Mandant usw.), eine Rolle und einen Raum. Berechtigungen werden dann allen Objekten erteilt, die zu diesem Raum gehören, einschließlich des gesamten Raumgraphen darunter.

Angenommen, ein Benutzer hat eine Rollenzuweisung mit der Rolle `DeviceInstaller` für den Stammknoten eines Raumgraphen, der einem Gebäude entspricht. Der Benutzer kann dann nicht nur alle Geräte dieses Knotens lesen und aktualisieren, sondern auch die aller anderen untergeordneten Räume des Gebäudes.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Sicherheit von Azure Digital Twins finden unter [Erstellen und Verwalten von Rollenzuweisungen](./security-create-manage-role-assignments.md).
