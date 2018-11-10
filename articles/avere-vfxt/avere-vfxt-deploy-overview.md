---
title: Übersicht über die Bereitstellung – Avere vFXT für Azure
description: Übersicht über das Bereitstellen von Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: aa5737d67ea2c9cb8cc7c7098764ae67fc91137d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669817"
---
# <a name="avere-vfxt-for-azure---deployment-overview"></a>Avere vFXT für Azure – Übersicht über die Bereitstellung

Dieser Artikel bietet eine Übersicht über die Schritte, die erforderlich sind, um einen Avere vFXT für Azure-Cluster in Betrieb zu nehmen.

Wenn Sie zum ersten Mal ein Avere vFXT-System bereitstellen, werden Sie feststellen, dass mehr Schritte erforderlich sind als bei der Bereitstellung der meisten anderen Azure-Tools. Ein sicheres Gespür für den gesamten Prozess hilft Ihnen, den erforderlichen Aufwand zu minimieren. Nach der Inbetriebnahme des Systems wird seine Leistungsfähigkeit bei der Beschleunigung von cloudbasierten Computeaufgaben den Aufwand wert sein.

## <a name="deployment-steps"></a>Bereitstellungsschritte

Nach der [Planung Ihres Systems](avere-vfxt-deploy-plan.md) können Sie mit der Erstellung des Avere vFXT-Clusters beginnen. 

Beginnen Sie mit der Erstellung einer Clustercontroller-VM, mit der der vFXT-Cluster erstellt wird.

Nachdem der vFXT-Cluster in Betrieb genommen wurde, möchten Sie sicherlich wissen, wie Sie Clients damit verbinden und, falls erforderlich, Ihre Daten in den neuen Blob-Speichercontainer verschieben können.  

Hier folgt eine Übersicht über alle Schritte.

1. Konfigurieren der Voraussetzungen 

   Bevor Sie einen virtuellen Computer erstellen, müssen Sie ein neues Abonnement für das Avere vFXT-Projekt erstellen, den Abonnementbesitz konfigurieren, Kontingente prüfen und bei Bedarf eine Erhöhung anfordern sowie Bedingungen für die Nutzung der Avere vFXT-Software akzeptieren. Ausführliche Anweisungen finden Sie unter [Vorbereiten der Avere vFXT-Erstellung](avere-vfxt-prereqs.md).

1. Erstellen des Clustercontrollers

   Der *Clustercontroller* ist ein einfacher virtueller Computer, der sich im gleichen virtuellen Netzwerk wie der Avere vFXT-Cluster befindet. Der Controller erstellt die vFXT-Knoten und bildet den Cluster, und er stellt auch eine Befehlszeilenschnittstelle zur Verfügung, um den Cluster während seiner Lebensdauer zu verwalten.

   Wenn Sie Ihren Controller mit einer öffentlichen IP-Adresse konfigurieren, kann er auch als Jump-Host für die Verbindung mit dem Cluster von außerhalb des VNets dienen.

   Sämtliche Software, die zur Erstellung des vFXT-Clusters und zur Verwaltung seiner Knoten erforderlich ist, ist auf dem Clustercontroller vorinstalliert.

   Weitere Informationen finden Sie unter [Erstellen der Clustercontroller-VM](avere-vfxt-deploy.md#create-the-cluster-controller-vm).

1. Erstellen einer Runtimerolle für die Clusterknoten 

   Azure verwendet die [rollenbasierte Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC), um die virtuellen Clusterknotencomputer für die Ausführung bestimmter Aufgaben zu autorisieren. Die Clusterknoten müssen z. B. in der Lage sein, anderen Clusterknoten IP-Adressen zuzuordnen oder neu zuzuordnen. Bevor Sie den Cluster erstellen, müssen Sie eine Rolle definieren, die ihm ausreichende Berechtigungen zuweist.

   Die vorinstallierte Software des Clustercontrollers umfasst eine Prototyprolle, die Sie anpassen können. Anweisungen finden Sie unter [Erstellen der Zugriffsrolle für Clusterknoten](avere-vfxt-deploy.md#create-the-cluster-node-access-role).

1. Erstellen des Avere vFXT-Clusters 

   Bearbeiten Sie auf dem Controller das entsprechende Skript zur Clustererstellung und führen Sie es aus, um den Cluster zu erstellen. Das [Bearbeiten des Bereitstellungsskripts](avere-vfxt-deploy.md#edit-the-deployment-script) umfasst ausführliche Anweisungen. 

1. Konfigurieren des Clusters 

   Stellen Sie eine Verbindung zur Avere vFXT-Konfigurationsschnittstelle (Avere-Systemsteuerung) her, um die Einstellungen des Clusters anzupassen. Melden Sie sich für die Supportüberwachung an, und fügen Sie Ihr Speichersystem hinzu, wenn Sie ein lokales Rechenzentrum verwenden.

   * [Zugreifen auf den vFXT-Cluster](avere-vfxt-cluster-gui.md)
   * [Aktivieren des Supports](avere-vfxt-enable-support.md)
   * [Konfigurieren des Speichers](avere-vfxt-add-storage.md) (falls erforderlich)

1. Einbinden von Clients

   Befolgen Sie die Anweisungen unter [Einbinden des Avere vFXT-Clusters](avere-vfxt-mount-clients.md), um mehr über den Lastausgleich und darüber zu erfahren, wie Clientcomputer den Cluster einbinden sollten.

1. Hinzufügen von Daten (falls erforderlich)

   Da es sich bei Avere vFXT um einen skalierbaren Cache mit mehreren Clients handelt, ist der beste Weg, Daten in einen neuen Back-End-Speichercontainer zu verschieben, die Strategie der Verwendung mehrerer Clients und mehrerer Threads. Weitere Informationen finden Sie unter [Verschieben von Daten in den vFXT-Cluster](avere-vfxt-data-ingest.md).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit [Vorbereiten der Avere vFXT-Erstellung](avere-vfxt-prereqs.md) fort, um vorbereitende Aufgaben für die Bereitstellung von Avere vFXT für Azure abzuschließen. 