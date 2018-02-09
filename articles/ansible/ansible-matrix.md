---
title: "Matrix der Ansible-Module und -Versionen für Azure"
description: "Matrix der Ansible-Module und -Versionen für Azure"
ms.service: ansible
keywords: Ansible, Rollen, Matrix, Version, Azure, DevOps
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: f62cc2df9e4ce815c4427b80e271ddc672748e4f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="ansible-module-and-version-matrix"></a>Matrix der Ansible-Module und -Versionen

## <a name="ansible-modules-for-azure"></a>Ansible-Module für Azure
Zum Lieferumfang von Ansible gehören verschiedene Module, die direkt auf Remotehosts oder über Playbooks ausgeführt werden können.
In diesem Artikel werden die Ansible-Module für Azure aufgelistet, die Azure-Cloudressourcen wie z.B. virtuelle Computer, Netzwerk- und Containerdienste bereitstellen können. Sie können diese Module aus dem offiziellen Release von Ansible oder aus den folgenden von Microsoft veröffentlichten Playbookrollen abrufen.

| Ansible-Module für Azure                   |  Ansible 2.4 |  Playbookrolle [azure_module](#introduction-to-azuremodule) |  Playbookrolle [azure_preview_module](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|-----------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                                  | 
| azure_rm_availabilityset                    | Ja          | Ja                         | Ja                                 | 
| azure_rm_availabilityset_facts              | Ja          | Ja                         | Ja                                 | 
| azure_rm_deployment                         | Ja          | Ja                         | Ja                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Ja          | Ja                         | Ja                                 | 
| azure_rm_virtualmachineimage_facts          | Ja          | Ja                         | Ja                                 | 
| azure_rm_resourcegroup                      | Ja          | Ja                         | Ja                                 | 
| azure_rm_resourcegroup_facts                | Ja          | Ja                         | Ja                                 | 
| azure_rm_virtualmachine                     | Ja          | Ja                         | Ja                                 | 
| azure_rm_virtualmachine_extension           | Ja          | Ja                         | Ja                                 | 
| azure_rm_virtualmachine_scaleset            | Ja          | Ja                         | Ja                                 | 
| azure_rm_image                              |              | Ja                         | Ja                                 | 
| **Netzwerk**                    |           |                          |                                  | 
| azure_rm_virtualnetwork                     | Ja          | Ja                         | Ja                                 | 
| azure_rm_virtualnetwork_facts               | Ja          | Ja                         | Ja                                 | 
| azure_rm_subnet                             | Ja          | Ja                         | Ja                                 | 
| azure_rm_networkinterface                   | Ja          | Ja                         | Ja                                 | 
| azure_rm_networkinterface_facts             | Ja          | Ja                         | Ja                                 | 
| azure_rm_publicipaddress                    | Ja          | Ja                         | Ja                                 | 
| azure_rm_publicipaddress_facts              | Ja          | Ja                         | Ja                                 | 
| azure_rm_dnsrecordset                       | Ja          | Ja                         | Ja                                 | 
| azure_rm_dnsrecordset_facts                 | Ja          | Ja                         | Ja                                 | 
| azure_rm_dnszone                            | Ja          | Ja                         | Ja                                 | 
| azure_rm_dnszone_facts                      | Ja          | Ja                         | Ja                                 | 
| azure_rm_loadbalancer                       | Ja          | Ja                         | Ja                                 | 
| azure_rm_loadbalancer_facts                 | Ja          | Ja                         | Ja                                 | 
| azure_rm_appgw                              | -            | -                           | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | Ja                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | Ja                                 |
| azure_rm_appgwroutetable                    | -            | -                           | Ja                                 |
| azure_rm_securitygroup                      | Ja          | Ja                         | Ja                                 | 
| azure_rm_appgwroutetable_facts              | Ja          | Ja                         | Ja                                 | 
| **Speicher**                    |           |                          |                                  | 
| azure_rm_storageaccount                     | Ja          | Ja                         | Ja                                 | 
| azure_rm_storageaccount_facts               | Ja          | Ja                         | Ja                                 | 
| azure_rm_storageblob                        | Ja          | Ja                         | Ja                                 | 
| azure_rm_managed_disk                       | Ja          | Ja                         | Ja                                 | 
| azure_rm_managed_disk_facts                 | Ja          | Ja                         | Ja                                 | 
| **Container**                    |           |                          |                                  | 
| azure_rm_acs                                | Ja          | Ja                         | Ja                                 | 
| azure_rm_containerinstance                  | -            | Ja                         | Ja                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | Ja                                 | 
| azure_rm_containerregistry                  | -            | Ja                         | Ja                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | Ja                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | Ja                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | Ja                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | Ja                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | Ja                                 | 
| **Azure-Funktionen**                    |           |                          |                                  | 
| azure_rm_functionapp                        | Ja          | Ja                         | Ja                                 | 
| azure_rm_functionapp_facts                  | Ja          | Ja                         | Ja                                 | 
| **Datenbanken**                    |           |                          |                                  | 
| azure_rm_sqlserver                          | -            | Ja                         | Ja                                 | 
| azure_rm_sqlserver_facts                    | -            | -                           | Ja                                 | 
| azure_rm_sqldatabase                        | -            | Ja                         | Ja                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | Ja                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | Ja                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | Ja                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | Ja                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | Ja                                 | 
| azure_rm_mysqlserver                        | -            | Ja                         | Ja                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | Ja                                 | 
| azure_rm_mysqldatabase                      | -            | Ja                         | Ja                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | Ja                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | Ja                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | Ja                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | Ja                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | Ja                                 | 
| azure_rm_postgresqlserver                   | -            | Ja                         | Ja                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | Ja                                 | 
| azure_rm_postgresqldatabase                 | -            | Ja                         | Ja                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | Ja                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | Ja                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | Ja                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | Ja                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | Ja                                 | 
| **Schlüsseltresor**                    |           |                          |                                  | 
| azure_rm_keyvault                           | -            | -                           | Ja                                 |
| azure_rm_keyvault_facts                     | -            | -                           | Ja                                 |
| azure_rm_keyvaultkey                        | -            | -                           | Ja                                 |
| azure_rm_keyvaultsecret                     | -            | -                           | Ja                                 |

## <a name="introduction-to-azuremodule"></a>Einführung in „azure_module“
Die [Playbookrolle „azure_module“](https://galaxy.ansible.com/Azure/azure_modules/) enthält die neuesten Änderungen und Fehlerbehebungen für Azure-Module aus dem [devel-Branch des Ansible-Repositorys](https://github.com/ansible/ansible/tree/devel). Wenn Sie nicht auf das nächste Release von Ansible warten können, ist die Installation der Rolle „azure_module“ eine gute Wahl.

Die Playbookrolle „azure_module“ wird alle drei Wochen veröffentlicht.

## <a name="introduction-to-azurepreviewmodule"></a>Einführung in „azure_preview_module“
Die [Playbookrolle „azure_preview_module“](https://galaxy.ansible.com/Azure/azure_preview_modules/) ist die umfassendste Rolle, sie enthält alle neuesten Azure-Module. Die Updates und Fehlerbehebungen erfolgen schneller als für das offizielle Ansible-Release. Wenn Sie Ansible zum Bereitstellen von Azure-Ressourcen verwenden, wird empfohlen, die Rolle „azure_preview_module“ zu installieren.

Die Playbookrolle „azure_preview_module“ wird alle drei Wochen veröffentlicht.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Playbookrollen finden Sie unter [Creating Reusable Playbooks](http://docs.ansible.com/ansible/latest/playbooks_reuse.html) (Erstellen wiederverwendbarer Playbooks). 
