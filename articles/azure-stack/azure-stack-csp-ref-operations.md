---
title: Registrieren von Mandanten für die Nutzungsnachverfolgung in Azure Stack | Microsoft-Dokumentation
description: Details zu Vorgängen zum Verwalten von Mandantenregistrierungen und zur Nachverfolgung der Mandantennutzung in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 67e1e22bc5569e7d6e20332ee86ffe4c7dd6a354
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343842"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Verwalten der Mandantenregistrierung in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme*

Dieser Artikel enthält Details zu Registrierungsvorgängen. Sie können diese Vorgänge für die folgenden Aufgaben verwenden:
- Verwalten von Mandantenregistrierungen
- Verwalten der Nutzungsnachverfolgung von Mandanten

Sie finden ausführliche Informationen zum Hinzufügen, Auflisten oder Entfernen von Mandantenzuordnungen. Sie können PowerShell oder die Abrechnungs-API-Endpunkte zum Verwalten Ihrer Nutzungsnachverfolgung verwenden. Sie finden ausführliche Informationen zum Hinzufügen, Auflisten oder Entfernen von Mandantenzuordnungen. Sie können PowerShell oder die Abrechnungs-API-Endpunkte zum Verwalten Ihrer Nutzungsnachverfolgung verwenden.

## <a name="add-tenant-to-registration"></a>Hinzufügen von Mandanten zur Registrierung

Sie verwenden den Vorgang, wenn Sie Ihrer Registrierung einen neuen Mandanten hinzufügen möchten. Die Mandantennutzung wird unter einem Azure-Abonnement gemeldet, das mit dem Azure AD-Mandanten (Azure Active Directory) verbunden ist.

Sie können den Vorgang auch verwenden, wenn Sie das einem Mandanten zugeordnete Abonnement ändern möchten. Rufen Sie PUT/New-AzureRMResource auf, um die vorherige Zuordnung zu überschreiben.

Sie können einem Mandanten ein einzelnes Azure-Abonnement zuweisen. Wenn Sie versuchen, einem vorhandenen Mandanten ein zweites Abonnement hinzuzufügen, wird das erste Abonnement überschrieben.

### <a name="use-api-profiles"></a>Verwenden von API-Profilen

Die Registrierungs-Cmdlets erfordern, dass Sie beim Ausführen von PowerShell ein API-Profil angeben. API-Profile stellen mehrere Azure-Ressourcenanbieter und ihre API-Versionen dar. Sie unterstützen Sie dabei, bei der Interaktion mit mehreren Azure-Clouds die richtige Version der API zu verwenden. Beispielsweise arbeiten Sie mit mehreren Clouds, wenn Sie mit Azure (global) und Azure Stack arbeiten. Profile geben einen Namen an, der ihrem Veröffentlichungsdatum entspricht. Sie müssen das Profil **2017-09-03** verwenden.

Weitere Informationen zu Azure Stack und API-Profilen finden Sie unter [Verwalten von API-Versionsprofilen in Azure Stack](user/azure-stack-version-profiles.md). Anweisungen für die ersten Schritte mit API-Profilen und PowerShell finden Sie unter [Verwenden von API-Versionsprofilen für PowerShell in Azure Stack](user/azure-stack-version-profiles-powershell.md).

### <a name="parameters"></a>Parameter

| Parameter                  | BESCHREIBUNG |
|---                         | --- |
| registrationSubscriptionID | Das Azure-Abonnement, das für die anfängliche Registrierung verwendet wurde. |
| customerSubscriptionID     | Das Azure-Abonnement (nicht Azure Stack), das zu dem Kunden gehört, der registriert werden soll. Muss im CSP-Angebot (Cloud Service Provider, Clouddienstanbieter) über Partner Center erstellt werden. Wenn ein Kunde über mehrere Mandanten verfügt, wurde ein Abonnement für den Mandanten zum Anmelden bei Azure Stack erstellt. |
| Ressourcengruppe              | Die Ressourcengruppe in Azure, in dem Ihre Registrierung gespeichert ist. |
| registrationName           | Der Name der Registrierung Ihrer Azure Stack-Instanz. Dies ist ein in Azure gespeichertes Objekt. Der Name hat in der Regel die Form azurestack-CloudID, wobei CloudID die Cloud-ID Ihrer Azure Stack-Bereitstellung ist. |

> [!Note]  
> Mandanten müssen bei jeder Azure Stack-Instanz registriert werden, die sie verwenden. Wenn ein Mandant mehrere Azure Stack-Instanzen verwendet, müssen Sie die anfängliche Registrierung jeder Bereitstellung mit dem Mandantenabonnement aktualisieren.

### <a name="powershell"></a>PowerShell

Verwenden Sie das Cmdlet New-AzureRmResource, um die Registrierungsressource zu aktualisieren. Melden Sie sich bei Azure (`Add-AzureRmAccount`) mit dem Konto an, das Sie für die anfängliche Registrierung verwendet haben. Hier ist ein Beispiel für das Hinzufügen eines Mandanten:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>API-Aufruf

**Vorgang**: PUT  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwort**: 201 Created  
**Antworttext**: Leer  

## <a name="list-all-registered-tenants"></a>Auflisten aller registrierten Mandanten

Rufen Sie eine Liste aller Mandanten ab, die einer Registrierung hinzugefügt wurden.

 > [!Note]  
 > Wenn keine Mandanten registriert wurden, erhalten Sie keine Antwort.

### <a name="parameters"></a>Parameter

| Parameter                  | BESCHREIBUNG          |
|---                         | ---                  |
| registrationSubscriptionID | Das Azure-Abonnement, das für die anfängliche Registrierung verwendet wurde.   |
| Ressourcengruppe              | Die Ressourcengruppe in Azure, in dem Ihre Registrierung gespeichert ist.    |
| registrationName           | Der Name der Registrierung Ihrer Azure Stack-Instanz. Dies ist ein in Azure gespeichertes Objekt. Der Name hat in der Regel die Form **azurestack**-***CloudID***, wobei ***CloudID*** die Cloud-ID Ihrer Azure Stack-Bereitstellung ist.   |

### <a name="powershell"></a>PowerShell

Verwenden Sie das Cmdlet „Get-AzureRmResource“, um alle registrierten Mandanten aufzulisten. Melden Sie sich bei Azure (`Add-AzureRmAccount`) mit dem Konto an, das Sie für die anfängliche Registrierung verwendet haben. Hier ist ein Beispiel für das Hinzufügen eines Mandanten:

```powershell
  Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API-Aufruf

Sie können eine Liste aller Mandantenzuordnungen mithilfe des GET-Vorgangs abrufen.

**Vorgang**: GET  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Antwort**: 200  
**Antworttext**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}”,
            "name": " cspSubscriptionId 1",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}”,
            "name": " cspSubscriptionId2 ",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## <a name="remove-a-tenant-mapping"></a>Entfernen einer Mandantenzuordnung

Sie können einen Mandanten entfernen, der einer Registrierung hinzugefügt wurde. Wenn dieser Mandant weiterhin Ressourcen in der Azure Stack-Instanz verwendet, wird deren Verwendung dem Abonnement in Rechnung gestellt, das zur anfänglichen Azure Stack-Registrierung verwendet wurde.

### <a name="parameters"></a>Parameter

| Parameter                  | BESCHREIBUNG          |
|---                         | ---                  |
| registrationSubscriptionID | Abonnement-ID für die Registrierung.   |
| Ressourcengruppe              | Die Ressourcengruppe für die Registrierung.   |
| registrationName           | Der Name der Registrierung.  |
| customerSubscriptionID     | Die Kundenabonnement-ID.  |

### <a name="powershell"></a>PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API-Aufruf

Sie können Mandantenzuordnungen mit dem DELETE-Vorgang entfernen.

**Vorgang**: DELETE  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwort**: 204 No Content  
**Antworttext**: Leer

## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen zum Abrufen von Ressourcennutzungsinformationen aus Azure Stack finden Sie unter [Verbrauch und Abrechnung in Azure Stack](azure-stack-billing-and-chargeback.md).
