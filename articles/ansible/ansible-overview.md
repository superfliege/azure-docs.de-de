---
title: Verwenden von Ansible mit Azure
description: Enthält eine Einführung in die Verwendung von Ansible zum Automatisieren von Cloudbereitstellung, Konfigurationsverwaltung und Anwendungsbereitstellungen.
ms.service: ansible
keywords: Ansible, Azure, DevOps, Übersicht, Cloudbereitstellung, Konfigurationsverwaltung, Anwendungsbereitstellung, Ansible-Module, Ansible-Playbooks
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: a7ce3c239a50462a9af137eb958268f72dbf79d1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="ansible-with-azure"></a>Ansible mit Azure

[Ansible](http://www.ansible.com) ist ein Open-Source-Produkt, mit dem die Cloudbereitstellung, Konfigurationsverwaltung und Anwendungsbereitstellungen automatisiert werden können. Mit Ansible können Sie virtuelle Computer, Container und Netzwerkumgebungen sowie auch vollständige Cloudinfrastrukturen bereitstellen. Ansible ermöglicht außerdem die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung.

Dieser Artikel enthält eine allgemeine Übersicht über die Vorteile der Verwendung von Ansible mit Azure.

## <a name="ansible-playbooks"></a>Ansible-Playbooks

[Ansible-Playbooks](http://docs.ansible.com/ansible/latest/playbooks.html) stellen für Ansible die Konfigurations-, Bereitstellungs- und Orchestrierungssprache dar. Sie können zum Beschreiben einer Richtlinie, die Sie für Ihre Remotesysteme erzwingen möchten, oder eine Reihe von Schritten eines allgemeinen IT-Prozesses verwendet werden. Wenn Sie ein Playbook erstellen, nutzen Sie YAML, um ein Modell für eine Konfiguration oder einen Prozess zu definieren.

## <a name="ansible-modules"></a>Ansible-Module

Ansible umfasst eine Sammlung von [Ansible-Modulen](http://docs.ansible.com/ansible/latest/modules_by_category.html), die direkt auf Remotehosts oder über [Playbooks](http://docs.ansible.com/ansible/latest/playbooks.html) ausgeführt werden können. Benutzer können auch ihre eigenen Module erstellen. Module können verwendet werden, um Systemressourcen zu steuern, z.B. Dienste, Pakete oder Dateien, oder Systembefehle auszuführen.

Für die Interaktion mit Azure-Diensten enthält Ansible eine Sammlung mit [Ansible-Cloudmodulen](http://docs.ansible.com/ansible/list_of_cloud_modules.html#azure), über die die Tools zum einfachen Erstellen und Orchestrieren Ihrer Infrastruktur in Azure bereitgestellt werden. 

## <a name="migrate-existing-workload-to-azure"></a>Migrieren einer vorhandenen Workload zu Azure

Nachdem Sie Ansible zum Definieren Ihrer Infrastruktur verwendet haben, können Sie das Playbook Ihrer Anwendung anwenden, damit Ihre Umgebung je nach Bedarf automatisch von Azure skaliert werden kann. 

## <a name="automate-cloud-native-application-in-azure"></a>Automatisieren einer nativen Cloudanwendung in Azure

Mit Ansible können Sie native Cloudanwendungen in Azure automatisieren, indem Sie Azure-Microservices wie [Azure Functions](https://azure.microsoft.com//services/functions/) und [Kubernetes in Azure](https://azure.microsoft.com/services/container-service/kubernetes/) verwenden.  

## <a name="manage-deployments-with-dynamic-inventory"></a>Verwalten von Bereitstellungen mit dynamischem Bestand
Mithilfe des Features [Dynamic Inventory](http://docs.ansible.com/ansible/intro_dynamic_inventory.html) (Dynamischer Bestand) können mit Ansible Bestandsdaten per Pullvorgang von Azure-Ressourcen abgerufen werden. Sie können Ihre vorhandenen Azure-Bereitstellungen dann mit Tags versehen und diese gekennzeichneten Bereitstellungen mit Ansible verwalten.

## <a name="additional-azure-marketplace-options"></a>Zusätzliche Azure Marketplace-Optionen
Das Azure Marketplace-Image [Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) von Red Hat hilft Organisationen beim Skalieren der IT-Automatisierung und beim Verwalten komplexer Bereitstellungen in physischen, virtuellen und Cloudinfrastrukturen. Ansible Tower enthält Funktionen, die zusätzliche Ebenen der Sichtbarkeit, Steuerung, Sicherheit und Effizienz für moderne Unternehmen bieten. Mit Ansible Tower werden Anmeldeinformationen verschlüsselt, z.B. Azure- und SSH-Schlüssel, damit Sie Aufträge an weniger erfahrene Mitarbeiter delegieren können, ohne die Offenlegung Ihrer Anmeldeinformationen zu riskieren.

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren von Ansible](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Erstellen eines virtuellen Linux-Computers](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)