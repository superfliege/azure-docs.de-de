---
title: Bereitstellen von Avere vFXT für Azure
description: Schritte zum Bereitstellen des Avere vFXT-Clusters in Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: da329b5c50fe7c39d9773743b40c2f990e298963
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296374"
---
# <a name="deploy-the-vfxt-cluster"></a>Bereitstellen des vFXT-Clusters

Dieses Verfahren führt Sie durch die Verwendung des Bereitstellungsassistenten aus dem Azure Marketplace. Der Assistent stellt den Cluster automatisch mit einer Azure Resource Manager-Vorlage bereit. Nachdem Sie die Parameter in das Formular eingegeben und auf **Erstellen** geklickt haben, führt Azure diese Schritte automatisch aus: 

* Erstellen Sie den Clustercontroller, der eine einfache VM ist, die die Software enthält, die für die Bereitstellung und Verwaltung des Clusters erforderlich ist.
* Richten Sie die Ressourcengruppe und virtuellen Netzwerkinfrastruktur ein, einschließlich der Erstellung der neuen Elemente, falls erforderlich.
* Erstellen Sie die Clusterknoten-VMs, und konfigurieren Sie sie als die Avere-Cluster.
* Erstellen Sie bei Bedarf einen neuen Azure Blob-Container, und konfigurieren Sie ihn als Cluster-Kernspeichereinheit.

Nachdem Sie die Anweisungen in diesem Dokument befolgt haben, verfügen Sie über ein virtuelles Netzwerk, ein Subnetz, einen Controller und einen vFXT-Cluster, wie in der folgenden Abbildung dargestellt:

![Diagramm, das das VNet mit optionalem Blobspeicher und ein Subnetz mit drei gruppierten virtuellen Computern mit der Bezeichnung „vFXT-Knoten/vFXT-Cluster“ und einem virtuellen Computer mit der Bezeichnung „Clustercontroller“ zeigt.](media/avere-vfxt-deployment.png)

Nach der Erstellung des Clusters sollten Sie [einen Speicherendpunkt](#create-a-storage-endpoint-if-using-azure-blob) in Ihrem virtuellen Netzwerk erstellen, wenn Sie Blob-Speicher verwenden. 

Vergewissern Sie sich vor der Verwendung der Erstellungsvorlage, dass diese Voraussetzungen erfüllt sind:  

1. [Neues Abonnement](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Berechtigungen des Abonnementbesitzers](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Kontingent für den vFXT-Cluster](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
1. [Benutzerdefinierte Zugriffsrollen](avere-vfxt-prereqs.md#create-access-roles) – Für die Zuordnung zu den Clusterknoten müssen Sie eine rollenbasierte Zugriffskontrollrolle anlegen. Sie haben die Möglichkeit, auch eine benutzerdefinierte Zugriffsrolle für den Clustercontroller zu erstellen, aber die meisten Benutzer übernehmen standardmäßig die Rolle des Besitzers, die dem Controller Rechten entsprechend einem Ressourcengruppenbesitzer einräumt. Weitere Informationen finden Sie unter [Integrierte Rollen für Azure-Ressourcen](../role-based-access-control/built-in-roles.md#owner).

Weitere Informationen zu den Schritten und der Planung der Clusterbereitstellung finden Sie unter [Planen Ihres Avere vFXT-Systems](avere-vfxt-deploy-plan.md) und [Übersicht über die Bereitstellung](avere-vfxt-deploy-overview.md).

## <a name="create-the-avere-vfxt-for-azure"></a>Erstellen des Avere vFXT for Azure

Rufen sie die Erstellungsvorlage im Azure-Portal auf, indem Sie nach Avere suchen und „Avere vFXT für die Azure-Bereitstellung“ auswählen. <!-- xxx update if that name changes xxx --> 

<!-- **[XXX need production image of template deploy in search and/or entry page of template deploy XXX]** -->

Klicken Sie zum Beginnen auf **Erstellen**. 

![Azure Marketplace mit der Anzeige der ersten Seite der Bereitstellungvorlage](media/avere-vfxt-deploy-first.png)

Die Vorlage ist in vier Schritte unterteilt – zwei Seiten zur Informationsbeschaffung sowie Validierungs- und Bestätigungsschritte. 

* Seite 1 konzentriert sich auf die Einstellungen für die Clustercontroller-VM. 
* Seite 2 sammelt Parameter für die Erstellung des Clusters und der zugehörigen Ressourcen wie Subnetze und Speicher. 
* Auf Seite 3 werden die Einstellungen zusammengefasst und die Konfiguration überprüft. 
* Seite 4 erläutert die Softwarebedingungen und ermöglicht es Ihnen, den Erstellungsprozess des Clusters zu starten. 

## <a name="page-one-parameters---cluster-controller-information"></a>Parameter auf Seite 1 – Clustercontrollerinformationen

Die erste Seite der Bereitstellungsvorlage sammelt Informationen über den Clustercontroller. 

![Erste Seite der Bereitstellungsvorlage](media/avere-vfxt-deploy-1.png)

Geben Sie die folgenden Informationen ein:

* **Name des Clustercontrollers** – Legen Sie den Namen für die Clustercontroller-VM fest.

* **Controllerbenutzername** – Geben Sie den Root-Benutzernamen für die Clustercontroller-VM ein. 

* **Authentifizierungstyp** – Wählen Sie entweder das Kennwort oder die Authentifizierung mit öffentlichem SSH-Schlüssel für die Verbindung mit dem Controller. Die Authentifizierungsmethode mit öffentlichem SSH-Schlüssel wird empfohlen. Lesen Sie dazu [Erstellen und Verwenden von SSH-Schlüsseln](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows), wen Sie Hilfe benötigen.

* **Kennwort** oder **Öffentlicher SSH-Schlüssel** – Je nach Authentifizierungstyp, den Sie ausgewählt haben, müssen Sie einen öffentlichen RSA-Schlüssel oder ein Kennwort in den nächsten Feldern angeben. Diese Anmeldeinformationen werden in Kombination mit dem Benutzernamen verwenden, der zuvor bereitgestellt wurde.

* **ID der Erstellungsrolle für Avere-Cluster** – Verwenden Sie dieses Feld zum Festlegen der Zugriffssteuerungsrolle für den Clustercontroller. Der Standardwert ist die integrierte Rolle [Besitzer](../role-based-access-control/built-in-roles.md#owner). Besitzerberechtigungen für den Clustercontroller sind auf die Ressourcengruppe des Clusters beschränkt. 

  Sie müssen den Globally Unique Identifier (GUID) verwenden, der der Rolle entspricht. Für den Standardwert (Besitzer) lautet der GUID 8e3af657-a8ff-443-c-a75c-2fe8c4bcb635. Um den GUID für eine benutzerdefinierte Rolle zu ermitteln, verwenden Sie diesen Befehl aus: 

  ```azurecli
  az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
  ```

* **Abonnement** – Wählen Sie das Avere vFXT-Abonnement aus. 

* **Ressourcengruppe** – Wählen Sie die Ressourcengruppe für den Avere vFXT-Cluster aus, oder klicken Sie auf „Neu erstellen“, und geben Sie einen neuen Ressourcengruppennamen ein. 

* **Speicherort** – Wählen Sie den Azure-Speicherort für Ihren Cluster und die Ressourcen.

Klicken Sie anschließend auf **OK**. 

> [!NOTE]
> Wenn Sie möchten, dass der Clustercontroller eine öffentlich zugängliche IP-Adresse hat, erstellen Sie ein neues virtuelles Netzwerk für den Cluster, anstatt ein bestehendes Netzwerk auszuwählen. Diese Einstellung finden Sie auf der zweiten Seite.

## <a name="page-two-parameters---vfxt-cluster-information"></a>Parameter auf Seite 2 – vFXT-Clusterinformationen

Auf der zweiten Seite der Bereitstellungsvorlage können Sie unter anderem Clustergröße, Knotentyp, Cachegröße und Speicherparameter festlegen. 

![Zweite Seite der Bereitstellungsvorlage](media/avere-vfxt-deploy-2.png)

* **Anzahl der Avere vFXT-Clusterknoten** – Wählen Sie die Anzahl der zu verwendenden Knoten im Cluster aus. Der Mindestwert ist drei Knoten, der Maximalwert 12. 

* **Cluster-Verwaltungskennwort** – Erstellen Sie das Kennwort für die Clusterverwaltung. Dieses Passwort wird mit dem Benutzernamen ```admin``` verwendet, um sich bei der Clustersteuerung anzumelden, um den Cluster zu überwachen und Einstellungen zu konfigurieren.

* **Betriebsrolle für den Avere-Cluster** – Geben Sie den Namen der Zugriffssteuerungsrolle für die Clusterknoten ein. Dies ist eine benutzerdefinierte Rolle, die als ein erforderlicher Schritt erstellt wurde. 

  Bei dem in [Erstellen der Zugriffsrolle für Clusterknoten](avere-vfxt-prereqs.md#create-the-cluster-node-access-role) wird die Datei als ```avere-operator.json``` gespeichert und der dazugehörige Rollenname ist ```avere-operator```.

* **Avere vFXT-Clustername** – Geben Sie dem Cluster einen eindeutigen Namen. 

* **Größe** – Geben Sie dem beim Erstellen der Clusterknoten zu verwendenden VM-Typ an. 

* **Cachegröße pro Knoten** – Der Clustercache ist auf die Clusterknoten verteilt, sodass die gesamte Cachegröße auf Ihrem Avere vFXT-Cluster die Cachegröße pro Knoten multipliziert mit der Anzahl der Knoten ist. 

  Die empfohlene Konfiguration ist die Verwendung von 1 TB pro Knoten bei Verwendung von Standard_D16s_v3-Clusterknoten und von 4 TB pro Knoten bei Verwendung von Standard_E32s_v3-Knoten.

* **Virtuelles Netzwerk** – Wählen Sie ein vorhandenes Vnet, um den Cluster unterzubringen, oder definieren Sie ein neues Vnet, das erstellt werden soll. 

  > [!NOTE]
  > Wenn Sie ein neues VNet erstellen, wird dem Clustercontroller eine öffentliche IP-Adresse zugewiesen, sodass Sie auf das neue private Netzwerk zugreifen können. Wenn Sie ein bestehendes Vnet auswählen, wird der Clustercontroller ohne eine öffentliche IP-Adresse konfiguriert. 
  > 
  > Eine öffentlich sichtbare IP-Adresse auf dem Clustercontroller ermöglicht einen einfacheren Zugriff auf den vFXT-Cluster, birgt aber ein leichtes Sicherheitsrisiko. 
  >  * Eine öffentliche IP-Adresse auf dem Clustercontroller ermöglicht es Ihnen, ihn als Jump-Host zu verwenden, um sich von außerhalb des privaten Subnetzes mit dem Avere vFXT-Cluster zu verbinden.
  >  * Wenn Sie keine öffentliche IP-Adresse auf dem Controller einrichten, müssen Sie für den Zugriff auf den Cluster einen anderen Jump-Host, eine VPN-Verbindung oder ExpressRoute verwenden. Erstellen Sie z. B. den Controller in einem virtuellen Netzwerk, in dem bereits eine VPN-Verbindung konfiguriert ist.
  >  * Wenn Sie einen Controller mit einer öffentlichen IP-Adresse erstellen, sollten Sie den virtuellen Controllercomputer mit einer Netzwerksicherheitsgruppe schützen. Standardmäßig erstellt die Avere vFXT for Azure-Bereitstellung eine Netzwerksicherheitsgruppe und beschränkt den eingehenden Zugriff nur auf Port 22 für Controller mit öffentlichen IP-Adressen. Sie können das System weiter schützen, indem Sie den Zugriff auf Ihren Bereich von IP-Quelladressen sperren, d.h. nur Verbindungen von Computern erlauben, die Sie für den Clusterzugriff verwenden möchten.

* **Subnetz** – Wählen Sie ein Subnetz aus Ihrem vorhandenen virtuellen Netzwerk aus, oder erstellen Sie ein neues. 

* **Blob-Speicher verwenden** – Wählen Sie, ob Sie einen neuen Azure Blob-Container erstellen und ihn als Back-End-Speicher für den neuen Avere vFXT-Cluster konfigurieren möchten oder nicht. Wenn Sie sich für das Erstellen eines neuen Containers entscheiden, müssen Sie das Speicherkonto für diesen Container angeben. Wenn Sie keinen neuen Blob-Container erstellen möchten, müssen Sie nach der Erstellung des Clusters Speicher anhängen (Anweisungen finden Sie in [Konfigurieren des Speichers](avere-vfxt-add-storage.md)). Legen Sie für dieses Feld **false** fest, wenn Sie keinen neuen Container erstellen möchten.

* **Speicherkonto** – Wenn Sie einen neuen Azure Blob-Container erstellen, geben Sie hier den Namen des Speicherkontos ein. Das Speicherkonto muss ein allgemeines V2-Standardkonto sein, das mit lokal redundantem Speicher und der heißen Speicherebene konfiguriert ist. Im Artikel [Konfigurieren des Speichers](avere-vfxt-add-storage.md#azure-storage-cloud-core-filer) finden Sie weitere Details zu den Anforderungen an das Speicherkonto.

## <a name="validation-and-purchase"></a>Überprüfung und Kauf

Die dritte Seite enthält eine Zusammenfassung der Konfiguration und überprüft die Parameter. Wenn die Überprüfung erfolgreich war, klicken Sie auf **OK**, um fortzufahren. 

![Dritte Seite der Bereitstellungsvorlage – Überprüfung](media/avere-vfxt-deploy-3.png)

Klicken Sie auf Seite 4 auf die Schaltfläche **Erstellen**, um den Bedingungen zustimmen, und erstellen die Avere vFXT for Azure-Cluster. 

![Vierte Seite der Bereitstellungsvorlage – Bedingungen, Schaltfläche „Erstellen“](media/avere-vfxt-deploy-4.png)

Die Bereitstellung des Clusters dauert 15 bis 20 Minuten.

## <a name="gather-template-output"></a>Sammeln der Vorlagenausgabe

Wenn die Avere vFXT-Vorlage die Erstellung des Clusters abgeschlossen hat, gibt sie einige wichtige Informationen über den neuen Cluster aus. 

> [!TIP]
> Stellen Sie sicher, dass Sie die Verwaltungs-IP-Adresse aus der Vorlagenausgabe kopieren. Sie benötigen diese Adresse zum Verwalten des Clusters.

Gehen Sie folgendermaßen vor, um diese Informationen zu suchen:

1. Navigieren Sie zu der Ressourcengruppe für Ihren Clustercontroller.

1. Klicken Sie auf der linken Seite auf **Bereitstellungen** und dann auf **microsoft-avere.vfxt-template**.

   ![Portalseite „Ressourcengruppe“ mit auf der linken Seite ausgewählten Bereitstellungen und der Anzeige von „microsoft-avere.vfxt-template“ in einer Tabelle unter dem Bereitstellungsnamen](media/avere-vfxt-outputs-deployments.png)

1. Klicken Sie auf der linken Seite auf **Ausgaben**. Kopieren Sie die Werte in die einzelnen Felder. 

   ![Ausgabeseite mit der Anzeige von Werten für SSHSTRING, RESOURCE_GROUP, LOCATION, NETWORK_RESOURCE_GROUP, NETWORK, SUBNET, SUBNET_ID, VSERVER_IPs und MGMT_IP in Feldern rechts neben den Bezeichnungen](media/avere-vfxt-outputs-values.png)


## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Erstellen eines Speicherendpunkts (bei Verwendung von Azure-BLOB)

Wenn Sie Azure-BLOB-Speicher für Ihren Back-End-Datenspeicher verwenden, sollten Sie einen Speicherdienstendpunkt in Ihrem virtuellen Netzwerk erstellen. Dieser [Dienstendpunkt](../virtual-network/virtual-network-service-endpoints-overview.md) sorgt dafür, dass der Azure-BLOB-Datenverkehr lokal bleibt, anstatt ihn über das Internet weiterzuleiten.

1. Klicken Sie im Portal links auf **Virtuelle Netzwerke**.
1. Wählen Sie das VNet für Ihren Controller aus. 
1. Klicken Sie links auf **Dienstendpunkte**.
1. Klicken Sie oben auf **Hinzufügen**.
1. Belassen Sie für den Dienst ``Microsoft.Storage``, und wählen Sie das Subnetz des Controllers aus.
1. Klicken Sie unten auf **Hinzufügen**.

  ![Screenshot für das Azure-Portal mit Anmerkungen für die Schritte zum Erstellen des Dienstendpunkts](media/avere-vfxt-service-endpoint.png)

## <a name="next-step"></a>Nächster Schritt

Nachdem der Cluster ausgeführt wird und Sie seine Verwaltungs-IP-Adresse kennen, können Sie sich mit dem [Clusterkonfigurationstool verbinden](avere-vfxt-cluster-gui.md), um den Support zu aktivieren, bei Bedarf Speicher hinzuzufügen und andere Clustereinstellungen anzupassen.
