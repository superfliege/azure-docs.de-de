---
title: Matrix der Ansible-Module und -Versionen für Azure
description: Matrix der Ansible-Module und -Versionen für Azure
ms.service: ansible
keywords: Ansible, Rollen, Matrix, Version, Azure, DevOps
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 09/22/2018
ms.topic: article
ms.openlocfilehash: 149f37d0231ecc0547e8dc7937d22a9cc38b7df3
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55810217"
---
# <a name="ansible-module-and-version-matrix"></a>Matrix der Ansible-Module und -Versionen

## <a name="ansible-modules-for-azure"></a>Ansible-Module für Azure
Zum Lieferumfang von Ansible gehören verschiedene Module, die direkt auf Remotehosts oder über Playbooks ausgeführt werden können.
In diesem Artikel werden die Ansible-Module für Azure aufgelistet, die Azure-Cloudressourcen wie z.B. virtuelle Computer, Netzwerk- und Containerdienste bereitstellen können. Sie können diese Module aus dem offiziellen Release von Ansible oder aus den folgenden von Microsoft veröffentlichten Playbookrollen abrufen.

| Ansible-Module für Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | Ansible-Rolle | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                          |                            |                                | 
| azure_rm_availabilityset                    | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_availabilityset_facts              | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_deployment                         | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_resource                           | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_resource_facts                     | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachineimage_facts          | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_resourcegroup                      | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_resourcegroup_facts                | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine                     | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine_facts               | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_virtualmachine_extension           | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine_scaleset            | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_image                              |              | Ja                         | Ja          | Ja          | Ja                                 | 
| **Netzwerk**                    |           |                          |                          |                             |                               | 
| azure_rm_virtualnetwork                     | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualnetwork_facts               | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_subnet                             | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_networkinterface                   | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_networkinterface_facts             | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_publicipaddress                    | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_publicipaddress_facts              | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnsrecordset                       | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnsrecordset_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnszone                            | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnszone_facts                      | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_loadbalancer                       | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_loadbalancer_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_appgateway                         | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Ja                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | Ja                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | Ja                                 |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_securitygroup                      | Ja          | Ja                         | Ja          | Ja          | Ja                                 |
| azure_rm_route                              | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_routetable                         | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_routetable_facts                   | -            | -                           | -            | Ja          | Ja                                 | 
| **Speicher**                    |           |                          |                          |                             |                               | 
| azure_rm_storageaccount                     | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_storageaccount_facts               | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_storageblob                        | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_managed_disk                       | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_managed_disk_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| **Container**                    |           |                          |                          |                            |                                | 
| azure_rm_aks                                | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_aks_facts                          | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_acs                                | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_containerinstance                  | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -              | -            | Ja                                 | 
| azure_rm_containerregistry                  | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | Ja                                 | 
| **Azure-Funktionen**                    |           |                          |                          |                            |                                | 
| azure_rm_functionapp                        | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_functionapp_facts                  | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| **Datenbanken**                    |           |                          |                          |                             |                               | 
| azure_rm_sqlserver                          | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_sqlserver_facts                    | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_sqldatabase                        | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlserver                        | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_mysqldatabase                      | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlserver                   | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_postgresqldatabase                 | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | Ja                                 | 
| **Schlüsseltresor**                    |           |                          |                          |                             |                               | 
| azure_rm_keyvault                           | -            | Ja                         | Ja          | Ja          | Ja                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -              | Ja                               |
| azure_rm_keyvaultkey                        | -            | Ja                         | Ja          | Ja          | Ja                                 |
| azure_rm_keyvaultsecret                     | -            | Ja                         | Ja          | Ja          | Ja                                 |
| **Web-Apps**                    |           |                          |                          |                             |                               | 
| azure_rm_appserviceplan                          | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_appserviceplan_facts                    | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_webapp                                  | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_webapp_facts                            | -            | -                         | -          | Ja          | Ja                                 | 
| **Traffic Manager**                    |           |                          |                          |                             |                               | 
| azure_rm_trafficmanagerendpoint                  | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_trafficmanagerendpoint_facts            | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_trafficmanagerprofile                   | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_trafficmanagerprofile_facts             | -            | -                         | -          | Ja          | Ja                                 | 
| **AutoScale**                    |           |                          |                          |                             |                               | 
| azure_rm_autoscale                  | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_autoscale_facts            | -            | -                         | -          | Ja          | Ja                                 | 

## <a name="introduction-to-playbook-role-for-azure"></a>Einführung in die Playbookrolle für Azure
Die [Playbookrolle „azure_preview_module“](https://galaxy.ansible.com/Azure/azure_preview_modules/) ist die umfassendste Rolle, sie enthält alle neuesten Azure-Module. Die Updates und Fehlerbehebungen erfolgen schneller als für das offizielle Ansible-Release. Wenn Sie Ansible zum Bereitstellen von Azure-Ressourcen verwenden, wird empfohlen, die Playbookrolle „azure_preview_module“ zu installieren.

Die Playbookrolle „azure_preview_module“ wird alle drei Wochen veröffentlicht.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Playbookrollen finden Sie unter [Creating Reusable Playbooks](https://docs.ansible.com/ansible/latest/playbooks_reuse.html) (Erstellen wiederverwendbarer Playbooks). 
