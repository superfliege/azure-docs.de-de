---
title: Matrix der Ansible-Module und -Versionen für Azure
description: Matrix der Ansible-Module und -Versionen für Azure
ms.service: ansible
keywords: Ansible, Rollen, Matrix, Version, Azure, DevOps
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 09/22/2018
ms.topic: article
ms.openlocfilehash: f574203f31a21f945115296693f04cff649614c6
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165635"
---
# <a name="ansible-module-and-version-matrix"></a>Matrix der Ansible-Module und -Versionen

## <a name="ansible-modules-for-azure"></a>Ansible-Module für Azure
Zum Lieferumfang von Ansible gehören verschiedene Module, die direkt auf Remotehosts oder über Playbooks ausgeführt werden können.
In diesem Artikel werden die Ansible-Module für Azure aufgelistet, die Azure-Cloudressourcen wie z.B. virtuelle Computer, Netzwerk- und Containerdienste bereitstellen können. Sie können diese Module aus dem offiziellen Release von Ansible oder aus den folgenden von Microsoft veröffentlichten Playbookrollen abrufen.

| Ansible-Module für Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | [Ansible-Rolle](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                          |                            |                                | 
| azure_rm_availabilityset                    | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_availabilityset_facts              | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_deployment                         | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_resource                           | -            | -                           | JA          | Ja          | JA                                 | 
| azure_rm_resource_facts                     | -            | -                           | JA          | Ja          | JA                                 | 
| azure_rm_virtualmachine_scaleset_facts      | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_virtualmachineimage_facts          | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_resourcegroup                      | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_resourcegroup_facts                | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_virtualmachine                     | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_virtualmachine_facts               | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_virtualmachine_extension           | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_virtualmachine_scaleset            | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_image                              |              | JA                         | Ja          | Ja          | JA                                 | 
| **Netzwerk**                    |           |                          |                          |                             |                               | 
| azure_rm_virtualnetwork                     | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_virtualnetwork_facts               | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_subnet                             | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_networkinterface                   | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_networkinterface_facts             | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_publicipaddress                    | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_publicipaddress_facts              | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_dnsrecordset                       | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_dnsrecordset_facts                 | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_dnszone                            | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_dnszone_facts                      | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_loadbalancer                       | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_loadbalancer_facts                 | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_appgateway                         | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | JA                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | JA                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | JA                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | JA                                 |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | JA                                 | 
| azure_rm_securitygroup                      | JA          | Ja                         | Ja          | Ja          | JA                                 |
| azure_rm_route                              | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_routetable                         | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_routetable_facts                   | -            | -                           | -            | JA          | JA                                 | 
| **Speicher**                    |           |                          |                          |                             |                               | 
| azure_rm_storageaccount                     | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_storageaccount_facts               | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_storageblob                        | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_managed_disk                       | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_managed_disk_facts                 | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| **Container**                    |           |                          |                          |                            |                                | 
| azure_rm_aks                                | -            | -                           | JA          | Ja          | JA                                 | 
| azure_rm_aks_facts                          | -            | -                           | JA          | Ja          | JA                                 | 
| azure_rm_acs                                | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_containerinstance                  | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -              | -            | JA                                 | 
| azure_rm_containerregistry                  | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | JA                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | JA                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | JA                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | JA                                 | 
| **Azure-Funktionen**                    |           |                          |                          |                            |                                | 
| azure_rm_functionapp                        | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| azure_rm_functionapp_facts                  | JA          | Ja                         | Ja          | Ja          | JA                                 | 
| **Datenbanken**                    |           |                          |                          |                             |                               | 
| azure_rm_sqlserver                          | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_sqlserver_facts                    | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_sqldatabase                        | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | JA                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | JA                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | JA                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | JA                                 | 
| azure_rm_mysqlserver                        | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_mysqldatabase                      | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | JA                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | JA                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | JA                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | JA                                 | 
| azure_rm_postgresqlserver                   | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_postgresqldatabase                 | -            | JA                         | Ja          | Ja          | JA                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | JA          | JA                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | JA                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | JA                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | JA                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | JA                                 | 
| **Schlüsseltresor**                    |           |                          |                          |                             |                               | 
| azure_rm_keyvault                           | -            | JA                         | Ja          | Ja          | JA                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -              | JA                               |
| azure_rm_keyvaultkey                        | -            | JA                         | Ja          | Ja          | JA                                 |
| azure_rm_keyvaultsecret                     | -            | JA                         | Ja          | Ja          | JA                                 |
| **Web-Apps**                    |           |                          |                          |                             |                               | 
| azure_rm_appserviceplan                          | -            | -                         | -          | JA          | JA                                 | 
| azure_rm_appserviceplan_facts                    | -            | -                         | -          | JA          | JA                                 | 
| azure_rm_webapp                                  | -            | -                         | -          | JA          | JA                                 | 
| azure_rm_webapp_facts                            | -            | -                         | -          | JA          | JA                                 | 
| **Traffic Manager**                    |           |                          |                          |                             |                               | 
| azure_rm_trafficmanagerendpoint                  | -            | -                         | -          | JA          | JA                                 | 
| azure_rm_trafficmanagerendpoint_facts            | -            | -                         | -          | JA          | JA                                 | 
| azure_rm_trafficmanagerprofile                   | -            | -                         | -          | JA          | JA                                 | 
| azure_rm_trafficmanagerprofile_facts             | -            | -                         | -          | JA          | JA                                 | 
| **AutoScale**                    |           |                          |                          |                             |                               | 
| azure_rm_autoscale                  | -            | -                         | -          | JA          | JA                                 | 
| azure_rm_autoscale_facts            | -            | -                         | -          | JA          | JA                                 | 

## <a name="introduction-to-playbook-role-for-azure"></a>Einführung in die Playbookrolle für Azure
Die [Playbookrolle „azure_preview_module“](https://galaxy.ansible.com/Azure/azure_preview_modules/) ist die umfassendste Rolle, sie enthält alle neuesten Azure-Module. Die Updates und Fehlerbehebungen erfolgen schneller als für das offizielle Ansible-Release. Wenn Sie Ansible zum Bereitstellen von Azure-Ressourcen verwenden, wird empfohlen, die Rolle „azure_preview_module“ zu installieren.

Die Playbookrolle „azure_preview_module“ wird alle drei Wochen veröffentlicht.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Playbookrollen finden Sie unter [Creating Reusable Playbooks](http://docs.ansible.com/ansible/latest/playbooks_reuse.html) (Erstellen wiederverwendbarer Playbooks). 
