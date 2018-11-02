---
title: Grundlegendes zu Azure Digital Twins-Gerätekonnektivität und -Authentifizierung | Microsoft-Dokumentation
description: Verwenden von Azure Digital Twins, um Geräte zu verbinden und zu authentifizieren
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: adfb4c369ea1b324da8562a5b0b245ebdecff602
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323792"
---
# <a name="create-and-manage-role-assignments"></a>Erstellen und Verwalten von Rollenzuweisungen

In Azure Digital Twins wird rollenbasierte Zugriffssteuerung ([Role-Based Access Control, RBAC](./security-role-based-access-control.md)) verwendet, um Zugriffe auf Ressourcen zu verwalten.

Jede Rollenzuweisung enthält:

* Einen **Objektbezeichner** (eine Azure Active Directory-ID, eine Dienstprinzipalobjekt-ID oder ein Domänenname)
* Einen **Objektbezeichnertyp**
* Eine **Rollendefinitions-ID**
* Einen **Raumpfad**
* (In den meisten Fällen) eine Azure Active Directory-**Mandanten-ID**

## <a name="role-definition-identifiers"></a>Rollendefinitionsbezeichner

In der folgenden sind die Bezeichner aufgeführt, die durch Abfragen der System/Rollen-API abgerufen werden:

| **Rolle** | **Bezeichner** |
| --- | --- |
| Space Administrator | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Benutzeradministrator| dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Device Administrator | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Key Administrator | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Token Administrator | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Benutzer | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Support Specialist | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Device Installer | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| GatewayDevice | d4c69766-e9bd-4e61-bfc1-d8b6e686c7a8 |

## <a name="supported-objectidtypes"></a>Unterstützte Objekt-ID-Typen (ObjectIdTypes)

Es werden folgende `ObjectIdTypes` unterstützt:

* `UserId`
* `DeviceId`
* `DomainName`
* `TenantId`
* `ServicePrincipalId`
* `UserDefinedFunctionId`

## <a name="create-a-role-assignment"></a>Erstellen einer Rollenzuweisung

```plaintext
HTTP POST /api/v1.0/roleassignments
```

| **Name** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| roleId| JA |Zeichenfolge | Der Bezeichner der Rollendefinition. Rollendefinitionen und deren Bezeichner (IDs) können durch Abfragen der System-API ermittelt werden. |
| objectId | JA |Zeichenfolge | Die Objekt-ID für die Rollenzuweisung, die entsprechend dem ihr zugeordneten Typ formatiert sein muss. Für den `DomainName`-objectIdType muss objectId mit dem `“@”`-Zeichen beginnen. |
| objectIdType | JA |Zeichenfolge | Der Typ der Rollenzuweisung. Muss eine der folgenden Zeilen in dieser Tabelle sein. |
| tenantId | Variabel | Zeichenfolge |Der Mandantenbezeichner. Nicht zulässig für den `DeviceId`- und den `TenantId`-objectIdType. Erforderlich für den `UserId`- und den `ServicePrincipalId`-objectIdType. Optional für den DomainName-objectIdType. |
| path* | JA | Zeichenfolge |Der vollständige Zugriffspfad für das `Space`-Objekt. Beispiel: `/{Guid}/{Guid}` Wenn für einen Bezeichner die Rollenzuweisung für den gesamten Graphen erforderlich ist, geben Sie `"/"` an (wodurch der Stammknoten angegeben ist). Diese Art der Angabe ist jedoch nicht zu empfehlen, und **Sie sollten immer das Prinzip der geringsten Rechte befolgen**. |

## <a name="sample-configuration"></a>Beispielkonfiguration

Ein Benutzer benötigt Administratorzugriff auf eine Etage eines Mandantenraums:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : " 0fc863bb-eb51-4704-a312-7d635d70e599",
      "ObjectIdType" : "UserId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/ 091e349c-c0ea-43d4-93cf-6b57abd23a44/ d84e82e6-84d5-45a4-bd9d-006a118e3bab"
    }
  ```

Eine Anwendung, die Testszenarien ausführt, in denen Geräte und Sensoren simuliert werden:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : "cabf7acd-af0b-41c5-959a-ce2f4c26565b",
      "ObjectIdType" : "ServicePrincipalId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/"
    }
  ```

Alle Benutzer, die zu einer Domäne gehören, erhalten Lesezugriff für Räume, Sensoren und Benutzer, was auch deren zugehörige Objekte einschließt:

  ```JSON
    {
      "RoleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
      "ObjectId" : "@microsoft.com",
      "ObjectIdType" : "DomainName",
      "Path": "/091e349c-c0ea-43d4-93cf-6b57abd23a44"
    }
  ```

So rufen Sie mit GET eine Rollenzuweisung ab:

```plaintext
HTTP GET /api/v1/roleassignments?path={path}
```

| **Name** | **In** | **Erforderlich** |    **Typ** |  **Beschreibung** |
| --- | --- | --- | --- | --- |
| path | path | True | Zeichenfolge | Der vollständige Pfad zum Raum |

So löschen Sie mit DELETE eine Rollenzuweisung:

```plaintext
HTTP DELETE /api/v1/roleassignments/{id}
```

| **Name** | **In** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- | --- |
| ID | path | True | Zeichenfolge |   Rollenzuweisungs-ID |

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Azure Digital Twins-Sicherheit finden Sie unter [API-Authentifizierung](./security-authenticating-apis.md).
