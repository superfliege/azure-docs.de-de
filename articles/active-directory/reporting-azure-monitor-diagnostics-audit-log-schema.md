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
ms.openlocfilehash: e1ae8e2a4dc9ef9c21300ebfc4df8c0f1c5819f2
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240473"
---
# <a name="interpret-the-azure-active-directory-audit-logs-schema-in-azure-monitor-preview"></a>Interpretieren des Azure Active Directory-Überwachungsprotokollschemas in Azure Monitor (Vorschauversion)

Dieser Artikel beschreibt das Azure AD-Überwachungsprotokollschema in Azure Monitor. Jeder einzelne Protokolleintrag wird als Text gespeichert, als JSON-Blob, wie in den beiden folgenden Beispielen dargestellt. 

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

| Feldname | BESCHREIBUNG |
|------------|-------------|
| time       | Datum und Uhrzeit (UTC). |
| operationName | Name des Vorgangs. |
| operationVersion | Die vom Client angeforderte REST-API-Version |
| category | Derzeit wird nur *Audit* als Wert unterstützt. |
| tenantId | Mit den Protokollen verknüpfte Mandanten-GUID. |
| resultType | Das Vorgangsergebnis ist *Erfolgreich* oder *Fehler*. |
| resultSignature |  Dieser Wert wird nicht zugeordnet, sodass das Feld ignoriert werden kann. | 
| resultDescription | Zusätzliche Ergebnisbeschreibung (falls verfügbar). | 
| durationMs |  Dieser Wert wird nicht zugeordnet, sodass das Feld ignoriert werden kann. |
| callerIpAddress | IP-Adresse des Clients, der die Anforderung gestellt hat. | 
| correlationId | Optionale GUID, die vom Client übergeben wurde. Der Wert hilft dabei, clientseitige Vorgänge mit serverseitigen Vorgängen zu korrelieren und Protokolle zu verfolgen, die sich über mehrere Dienste erstrecken. |
| identity | Identität des Tokens, das beim Erstellen der Anforderung angegeben wurde. Es kann sich um ein Benutzerkonto, ein Systemkonto oder einen Dienstprinzipal handeln. |
| level | Meldungstyp. Bei Überwachungsprotokollen ist dies immer *Information*. |
| location | Standort des Rechenzentrums. |
| Eigenschaften | Listet die unterstützten Eigenschaften eines Überwachungsprotokolls auf. Weitere Informationen finden Sie in der Tabelle unten. | 


| Eigenschaftenname | BESCHREIBUNG |
|---------------|-------------|
| AuditEventCategory | Typ des Überwachungsereignisses. Dies kann *Benutzerverwaltung*, *Anwendungsverwaltung* etc. sein.|
| Identity Type | *Anwendung* oder *Benutzer*. |
| Vorgangstyp | Dies kann *Hinzufügen*, *Aktualisieren*, *Löschen* oder *Sonstiges* sein. |
| Target Resource Type | Gibt den Zielressourcentyp an, auf dem der Vorgang ausgeführt wurde. Dies kann *Anwendung*, *Benutzer*, *Rolle* oder *Richtlinie* sein. | 
| Target Resource Name | Name der Zielressource. Dies kann z. B. ein Anwendungsname, ein Rollenname, ein Benutzerprinzipalname oder ein Dienstprinzipalname sein. |
| additionalTargets | Listet alle zusätzlichen Eigenschaften für bestimmte Vorgänge auf. Beispielsweise werden bei einem Aktualisierungsvorgang die alten und die neuen Werte unter *targetUpdatedProperties* aufgeführt. | 

## <a name="next-steps"></a>Nächste Schritte

* [Interpretieren des Anmeldeprotokollschemas in Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Weitere Informationen zu Azure-Diagnoseprotokollen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Häufig gestellte Fragen und bekannte Probleme](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)