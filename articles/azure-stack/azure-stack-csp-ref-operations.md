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
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: ef7ca59647a1f8c15d85c809609060a5945bedde
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Verwalten der Mandantenregistrierung in Azure Stack

*Gilt für: Integrierte Azure Stack-Systeme*

Dieser Artikel enthält Details zu Vorgängen zum Verwalten von Mandantenregistrierungen und zur Nachverfolgung der Mandantennutzung. Sie finden ausführliche Informationen zum Hinzufügen, Auflisten oder Entfernen von Mandantenzuordnungen. Sie können PowerShell oder die Abrechnungs-API-Endpunkte zum Verwalten Ihrer Nutzungsnachverfolgung verwenden.

## <a name="add-tenant-to-registration"></a>Hinzufügen von Mandanten zur Registrierung

Verwenden Sie diesen Vorgang, wenn Sie einen neuen Mandanten Ihrer Registrierung hinzufügen möchten, sodass dessen Nutzung unter einem Azure-Abonnement gemeldet wird, das mit ihrem Azure Active Directory-Mandanten (Azure AD) verbunden ist.

Sie können diesen Vorgang auch verwenden, wenn Sie das einem Mandanten zugeordnete Abonnement ändern möchten; Sie können PUT/New-AzureRMResource erneut aufrufen. Die alte Zuordnung wird überschrieben.

Beachten Sie, dass nur ein einziges Azure-Abonnement einem Mandanten zugeordnet werden kann. Wenn Sie versuchen, einem vorhandenen Mandanten ein zweites Abonnement hinzuzufügen, wird das erste Abonnement überschrieben. 


| Parameter                  | BESCHREIBUNG |
|---                         | --- |
| registrationSubscriptionID | Das Azure-Abonnement, das für die anfängliche Registrierung verwendet wurde. |
| customerSubscriptionID     | Das Azure-Abonnement (nicht Azure Stack), das zu dem Kunden gehört, der registriert werden soll. Muss im Cloud-Dienstanbieter-Angebot (Cloud Service Provider, CSP) erstellt werden. In der Praxis über das Partner Center. Wenn ein Kunde über mehrere Mandanten verfügt, muss dieses Abonnement in dem Mandanten erstellt werden, der zum Anmelden bei Azure Stack verwendet wird. |
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

Verwenden Sie das Cmdlet Get-AzureRmResovurce, um alle registrierten Mandanten aufzulisten. Melden Sie sich bei Azure (`Add-AzureRmAccount`) mit dem Konto an, das Sie für die anfängliche Registrierung verwendet haben. Hier ist ein Beispiel für das Hinzufügen eines Mandanten:

```powershell
  Get-AzureRmResovurce -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
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

 - Weitere Informationen zum Abrufen von Ressourcennutzungsinformationen aus Azure Stack finden Sie unter [Verbrauch und Abrechnung in Azure Stack](/azure-stack-billing-and-chargeback.md).
