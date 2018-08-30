---
title: Matrix der Ansible-Module und -Versionen für Azure
description: Matrix der Ansible-Module und -Versionen für Azure
ms.service: ansible
keywords: Ansible, Rollen, Matrix, Version, Azure, DevOps
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 07/02/2018
ms.topic: article
ms.openlocfilehash: 8bbb2563991b45fe7a20ce243751c1c1a1f75b99
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094906"
---
# <a name="ansible-module-and-version-matrix"></a>Matrix der Ansible-Module und -Versionen

## <a name="ansible-modules-for-azure"></a>Ansible-Module für Azure
Zum Lieferumfang von Ansible gehören verschiedene Module, die direkt auf Remotehosts oder über Playbooks ausgeführt werden können.
In diesem Artikel werden die Ansible-Module für Azure aufgelistet, die Azure-Cloudressourcen wie z.B. virtuelle Computer, Netzwerk- und Containerdienste bereitstellen können. Sie können diese Module aus dem offiziellen Release von Ansible oder aus den folgenden von Microsoft veröffentlichten Playbookrollen abrufen.

| Ansible-Module für Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 |  Playbookrolle [azure_preview_module](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                          |                                  | 
| azure_rm_availabilityset                    | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_availabilityset_facts              | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_deployment                         | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_resource                           | -            | -                           | JA          | JA                                 | 
| azure_rm_resource_facts                     | -            | -                           | JA          | JA                                 | 
| azure_rm_virtualmachine_scaleset_facts      | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_virtualmachineimage_facts          | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_resourcegroup                      | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_resourcegroup_facts                | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_virtualmachine                     | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_virtualmachine_extension           | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_virtualmachine_scaleset            | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_image                              |              | JA                         | Ja          | JA                                 | 
| **Netzwerk**                    |           |                          |                          |                                  | 
| azure_rm_virtualnetwork                     | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_virtualnetwork_facts               | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_subnet                             | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_networkinterface                   | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_networkinterface_facts             | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_publicipaddress                    | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_publicipaddress_facts              | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_dnsrecordset                       | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_dnsrecordset_facts                 | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_dnszone                            | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_dnszone_facts                      | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_loadbalancer                       | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_loadbalancer_facts                 | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_appgw                              | -            | -                           | -            | JA                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | JA                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | JA                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | JA                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | JA                                 |
| azure_rm_securitygroup                      | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | JA                                 | 
| **Speicher**                    |           |                          |                          |                                  | 
| azure_rm_storageaccount                     | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_storageaccount_facts               | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_storageblob                        | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_managed_disk                       | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_managed_disk_facts                 | JA          | Ja                         | Ja          | JA                                 | 
| **Container**                    |           |                          |                          |                                  | 
| azure_rm_aks                                | -            | -                           | JA          | JA                                 | 
| azure_rm_aks_facts                          | -            | -                           | JA          | JA                                 | 
| azure_rm_acs                                | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_containerinstance                  | -            | JA                         | Ja          | JA                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -            | JA                                 | 
| azure_rm_containerregistry                  | -            | JA                         | Ja          | JA                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | JA                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | JA                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | JA                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | JA                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | JA                                 | 
| **Azure-Funktionen**                    |           |                          |                          |                                  | 
| azure_rm_functionapp                        | JA          | Ja                         | Ja          | JA                                 | 
| azure_rm_functionapp_facts                  | JA          | Ja                         | Ja          | JA                                 | 
| **Datenbanken**                    |           |                          |                          |                                  | 
| azure_rm_sqlserver                          | -            | JA                         | Ja          | JA                                 | 
| azure_rm_sqlserver_facts                    | -            | JA                         | Ja          | JA                                 | 
| azure_rm_sqldatabase                        | -            | JA                         | Ja          | JA                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | JA                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | JA                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | JA                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | JA                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | JA                                 | 
| azure_rm_mysqlserver                        | -            | JA                         | Ja          | JA                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | JA                                 | 
| azure_rm_mysqldatabase                      | -            | JA                         | Ja          | JA                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | JA                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | JA                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | JA                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | JA                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | JA                                 | 
| azure_rm_postgresqlserver                   | -            | JA                         | Ja          | JA                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | JA                                 | 
| azure_rm_postgresqldatabase                 | -            | JA                         | Ja          | JA                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | JA                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | JA                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | JA                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | JA                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | JA                                 | 
| **Schlüsseltresor**                    |           |                          |                          |                                  | 
| azure_rm_keyvault                           | -            | JA                         | Ja          | JA                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -            | JA                                 |
| azure_rm_keyvaultkey                        | -            | JA                         | Ja          | JA                                 |
| azure_rm_keyvaultsecret                     | -            | JA                         | Ja          | JA                                 |


## <a name="introduction-to-playbook-role-for-azure"></a>Einführung in die Playbookrolle für Azure
Die [Playbookrolle „azure_preview_module“](https://galaxy.ansible.com/Azure/azure_preview_modules/) ist die umfassendste Rolle, sie enthält alle neuesten Azure-Module. Die Updates und Fehlerbehebungen erfolgen schneller als für das offizielle Ansible-Release. Wenn Sie Ansible zum Bereitstellen von Azure-Ressourcen verwenden, wird empfohlen, die Rolle „azure_preview_module“ zu installieren.

Die Playbookrolle „azure_preview_module“ wird alle drei Wochen veröffentlicht.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Playbookrollen finden Sie unter [Creating Reusable Playbooks](http://docs.ansible.com/ansible/latest/playbooks_reuse.html) (Erstellen wiederverwendbarer Playbooks). 
