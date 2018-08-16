---
title: Interpretieren des Azure Active Directory-Überwachungsprotokollschemas in Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Weitere Informationen zum Azure AD-Überwachungsprotokollschema für die Verwendung in Azure Monitor (Vorschauversion)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 87799cf5dde9039d3e7b386d726812600a4bbc69
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502915"
---
# <a name="interpret-the-azure-ad-audit-logs-schema-in-azure-monitor-preview"></a>Interpretieren des Azure AD-Überwachungsprotokollschemas in Azure Monitor (Vorschauversion)

Dieser Artikel beschreibt das Azure Active Directory-Überwachungsprotokollschema (Azure AD) in Azure Monitor. Jeder einzelne Protokolleintrag wird als Text gespeichert und als JSON-Blob formatiert, wie in den beiden folgenden Beispielen dargestellt: 

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-17T00:14:31.2585575Z", 
        "operationName": "Change password (self-service)",
        "operationVersion": "1.0",
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "resultDescription": "None", 
        "durationMs": "-1", 
        "correlationId": "60d5e89a-b890-413f-9e25-a047734afe9f", 
        "identity": "sreens@wingtiptoysonline.com", 
        "Level": "Informational", 
        "location": "WUS", 
        "properties": { 
            "identityType": "UPN", 
            "operationType": "Update", 
            "additionalDetails": "None", 
            "additionalTargets": "", 
            "targetUpdatedProperties": "", 
            "targetResourceType": "UPN__TenantContextID__PUID__ObjectID__ObjectClass", 
            "targetResourceName": "sreens@wingtiptoysonline.com__bf85dc9d-cb43-44a4-80c4-469e8c58249e__1003BFFD9FEB17DB__7a408bdd-7d97-4574-8511-dd747b56465d__User", 
            "auditEventCategory": "UserManagement" 
        } 
    } 
    ] 
} 
```

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-18T19:47:43.0368859Z", 
        "operationName": "Update service principal.", 
        "operationVersion": "1.0", 
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "durationMs": "-1", 
        "callerIpAddress": "<null>", 
        "correlationId": "14916c7a-5a7d-44e8-9b06-74b49efb08ee", 
        "identity": "NA", 
        "Level": "Informational", 
        "properties": { 
            "identityType": "NA", 
            "operationType": "Update", 
            "additionalDetails": {}, 
            "additionalTargets": "", 
            "targetUpdatedProperties": [ 
            { 
                "Name": "Included Updated Properties", 
                "OldValue": null, 
                "NewValue": "" 
            }, 
            { 
                "Name": "TargetId.ServicePrincipalNames", 
                "OldValue": null, 
                "NewValue": "http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29" 
            } 
            ], 
        "targetResourceType": "Other__ObjectID__ObjectClass__Name__AppId__SPN", 
        "targetResourceName": "ServicePrincipal_ea70a262-4da3-440a-b396-9734ddfd9df2__ea70a262-4da3-440a-b396-9734ddfd9df2__ServicePrincipal__Salesforce__cd3ed3de-93ee-400b-8b19-b61ef44a0f29__http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29", 
        "auditEventCategory": "ApplicationManagement" 
      } 
    } 
    ] 
} 
```

## <a name="field-and-property-descriptions"></a>Beschreibungen der Felder und Eigenschaften

| Feldname | BESCHREIBUNG |
|------------|-------------|
| time       | Das Datum und die Uhrzeit (UTC). |
| operationName | Der Name des Vorgangs. |
| operationVersion | Die vom Client angeforderte REST-API-Version. |
| category | Derzeit ist *Audit* der einzige unterstützte Wert. |
| tenantId | Die mit den Protokollen verknüpfte Mandanten-GUID. |
| resultType | Das Ergebnis des Vorgangs. Das Ergebnis kann *Erfolg* oder *Fehler* sein. |
| resultSignature |  Dieses Feld wird nicht zugeordnet, sodass es ignoriert werden kann. | 
| resultDescription | Eine zusätzliche Ergebnisbeschreibung (falls verfügbar). | 
| durationMs |  Dieses Feld wird nicht zugeordnet, sodass es ignoriert werden kann. |
| callerIpAddress | Die IP-Adresse des Clients, der die Anforderung gestellt hat. | 
| correlationId | Eine optionale GUID, die vom Client übergeben wird. Sie kann dabei helfen, clientseitige Vorgänge mit serverseitigen Vorgängen zu korrelieren und Protokolle zu verfolgen, die sich über mehrere Dienste erstrecken. |
| identity | Die Identität des Tokens, das beim Erstellen der Anforderung angegeben wurde. Die Identität kann ein Benutzerkonto, ein Systemkonto oder ein Dienstprinzipal sein. |
| level | Der Nachrichtentyp. Bei Überwachungsprotokollen ist die Ebene immer *Information*. |
| location | Der Standort des Rechenzentrums. |
| Eigenschaften | Listet die unterstützten Eigenschaften auf, die mit einem Überwachungsprotokoll verknüpft sind. Weitere Informationen finden Sie in der nächsten Tabelle. | 

<br>

| Eigenschaftenname | BESCHREIBUNG |
|---------------|-------------|
| AuditEventCategory | Der Typ des Überwachungsereignisses. Dies kann *Benutzerverwaltung*, *Anwendungsverwaltung* oder ein anderer Typ sein.|
| Identity Type | Der Typ kann *Anwendung* oder *Benutzer* sein. |
| Vorgangstyp | Der Typ kann *Hinzufügen*, *Aktualisieren*, *Löschen* sein. *Sonstiges* ist auch möglich. |
| Target Resource Type | Gibt den Zielressourcentyp an, auf dem der Vorgang ausgeführt wurde. Der Typ kann *Anwendung*, *Benutzer*, *Rolle* oder *Richtlinie* sein. | 
| Target Resource Name | Der Name der Zielressource. Dies kann ein Anwendungsname, ein Rollenname, ein Benutzerprinzipalname oder ein Dienstprinzipalname sein. |
| additionalTargets | Listet alle zusätzlichen Eigenschaften für bestimmte Vorgänge auf. Beispielsweise werden bei einem Aktualisierungsvorgang die alten und die neuen Werte unter *targetUpdatedProperties* aufgeführt. | 

## <a name="next-steps"></a>Nächste Schritte

* [Interpretieren des Anmeldeprotokollschemas in Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Häufig gestellte Fragen und bekannte Probleme](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)
