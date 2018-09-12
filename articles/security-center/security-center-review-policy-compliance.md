---
title: Überprüfen der Richtlinienkonformität des Security Center mit der Azure-REST-API | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure-REST-APIs die derzeitige Konformität mit Security Center-Richtlinien überprüfen.
services: security-center
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: alleonar
ms.openlocfilehash: 1443486590859aac5591aff2ab0551bed9228d7b
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301748"
---
# <a name="review-security-center-policy-compliance-using-rest-apis"></a>Überprüfen der Richtlinienkonformität des Security Center mit REST-APIs

Das Security Center gleicht Ihre Azure-Ressourcen in regelmäßigen Abständen mit Ihren definierten Sicherheitsrichtlinien ab. Das Security Center stellt darüber hinaus eine REST-API zur Verfügung, mit der Sie die Konformität in Ihren eigenen Anwendungen überprüfen können. Sie können den Dienst direkt abfragen oder JSON-Ergebnisse in andere Anwendungen importieren. 

In dem vorliegenden Artikel erfahren Sie, wie die aktuelle Gruppe von Empfehlungen von allen Azure-Ressourcen, die einem Abonnement zugeordnet sind, abgerufen werden können.

So rufen Sie die aktuelle Gruppe von Empfehlungen ab
``` http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/tasks?api-version={api-version}
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Erstellen der Anforderung  

Der `{subscription-id}`-Parameter ist erforderlich und sollte die Abonnement-ID für das Azure-Abonnement enthalten, in dem die Richtlinien festgelegt sind. Wenn Sie über mehrere Abonnements verfügen, lesen Sie [Verwenden mehrerer Abonnements](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions):  

Der `api-version`-Parameter ist erforderlich. Diese Endpunkte werden zurzeit nur für `api-version=2015-06-01-preview` unterstützt. 

Die folgenden Header sind erforderlich: 

|Anforderungsheader|BESCHREIBUNG|  
|--------------------|-----------------|  
|*Inhaltstyp*:|Erforderlich. Legen Sie diese Option auf `application/json` fest.|  
|*Autorisierung*:|Erforderlich. Legen Sie diese Option auf ein gültiges `Bearer` [Zugriffstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients) fest. |  

## <a name="response"></a>response  

Bei einer erfolgreichen Antwort wird der Statuscode 200 (OK) zurückgegeben, der eine Liste der empfohlenen Aufgaben zum Schutz Ihrer Azure-Ressourcen enthält.

``` json
{  
  "value": [  
    {  
       "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
       "name": "{task_id}",
       "type": "Microsoft.Security/locations/{region}/tasks",
       "properties": {
       "state": "Active",
       "subState": "NA",
       "creationTimeUtc": "{create-time}",
       "lastStateChangeTimeUtc": "{last-state-change}",
       "securityTaskParameters": "{security-task-properties}"
    } 
  ]  
}  
```  

Jedes Element in **value** stellt eine Empfehlung dar:

|Response-Eigenschaft|BESCHREIBUNG|
|----------------|----------|
|**state** | Gibt an, ob eine Empfehlung den Status `active` oder `resolved` aufweist. |
|**creationTimeUtc** | Datum und die Uhrzeit in UTC, die angeben, wann die Empfehlung erstellt wurde. |
|**lastStateChangeUtc** | Datum und Uhrzeit in UTV der letzten Statusänderung, sofern vorhanden. |
|**securityTaskParameters** | Erläutert die Empfehlung, wobei die Eigenschaften je nach der zugrunde liegenden Empfehlung variiert. |
||
  
Die derzeit unterstützten Empfehlungen finden Sie unter [Verwalten von Sicherheitsempfehlungen im Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Andere Statuscodes geben Fehlerbedingungen an. In diesen Fällen beinhaltet das Antwortobjekt eine Beschreibung, die erläutert, warum bei der Anforderung ein Fehler aufgetreten ist.

``` json
{  
  "value": [  
    {  
      "description": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="example-response"></a>Beispielantwort  

``` json
{  
  "value": [  
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
            "name": "{task_id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "vmId": "/subscriptions/{subscription-id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "vmName": "{vm_name}",
                    "severity": "{severity}",
                    "isOsDiskEncrypted": {is_os_disk_encrypted},
                    "isDataDiskEncrypted": {is_data_disk_encrypted},
                    "name": "EncryptionOnVm",
                    "uniqueKey": "EncryptionOnVmTaskParameters_/subscriptions/{subscription-id}/resourceGroups/{resoource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "resourceId": "/subscriptions/{subscription_id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}"
                }
            }
        },
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{location}/tasks/{task-id}",
            "name": "{task-id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "serverName": "{sql-server-name}",
                    "serverId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}",
                    "name": "Enable auditing for the SQL server",
                    "uniqueKey": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}/auditingPolicies/Default",
                    "resourceId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}"
                }
            }
        }  ]  
}  
```  

Diese Antwort zeigt zwei Empfehlungen, wobei jedes Element in der Liste einer bestimmten Empfehlung entspricht. Die erste Empfehlung empfiehlt die Verschlüsselung des Speichers auf einem virtuellen Linux-Computer, während bei der zweiten Empfehlung vorgeschlagen wird, die Überwachung für einen SQL Server zu aktivieren.

Die Empfehlungen variieren je nach den Richtlinien, die Sie aktiviert haben. Weitere Informationen über die derzeit verfügbaren Empfehlungen finden Sie unter [Verwalten von Sicherheitsempfehlungen im Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).


## <a name="see-also"></a>Weitere Informationen  
- [Festlegen von Sicherheitsrichtlinien](https://docs.microsoft.com/azure/security-center/security-center-policies-overview)
- [Azure-Sicherheitsressourcenanbieter – REST-API](https://msdn.microsoft.com/library/azure/mt704034.aspx)   
- [Erste Schritte mit der Azure-REST-API](https://docs.microsoft.com/rest/api/azure/)   
- [Azure Security Center-PowerShell-Modul](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.22)
