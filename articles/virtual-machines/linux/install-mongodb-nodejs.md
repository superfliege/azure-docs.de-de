---
title: "Installieren von MongoDB für einen Linux-VM mit der Azure-CLI-1.0 | Microsoft Docs"
description: Informationen Sie zum Installieren und Konfigurieren von MongoDB auf einem virtuellen Linux-Computer in Azure mithilfe der Ressourcen-Manager-Bereitstellungsmodell.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: c97ade0a3d95824f723aad55776de861fe49441f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm-using-the-azure-cli-10"></a>Zum Installieren und Konfigurieren von MongoDB für einen Linux-VM mit der Azure-CLI-1.0
[MongoDB](http://www.mongodb.org) ist eine beliebte Open Source, hohe Leistung NoSQL-Datenbank. Diesem Artikel erfahren Sie zum Installieren und Konfigurieren von MongoDB für einen Linux-VM in Azure mit dem Ressourcen-Manager-Bereitstellungsmodell. Beispiele sind von Detailfenstern gezeigt, wie auf:

* [Manuell installieren Sie und konfigurieren Sie eine grundlegende MongoDB-Instanz](#manually-install-and-configure-mongodb-on-a-vm)
* [Erstellen Sie eine grundlegende MongoDB-Instanz mit einer Ressourcen-Manager-Vorlage](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Erstellen Sie eine komplexe MongoDB mit Shards konfigurierten Cluster mit dem Replikat legt mithilfe einer Vorlage für die Ressourcen-Manager](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen, die Aufgabe abgeschlossen
Führen Sie die Aufgabe, die mit einer der folgenden CLI-Versionen:

- Azure-CLI 1.0 – unsere CLI für klassischen und Ressource Management Bereitstellungsmodellen (in diesem Artikel)
- [Azure-CLI-2.0](create-cli-complete-nodejs.md) -unsere nächste Generation CLI für das ressourcenbereitstellungsmodell für die Verwaltung


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Installieren und Konfigurieren von MongoDB auf einem virtuellen Computer manuell
MongoDB [bieten installationsanweisungen](https://docs.mongodb.com/manual/administration/install-on-linux/) für einschließlich Red Hat Linux-Distributionen / CentOS, SUSE, Ubuntu und Debian. Das folgende Beispiel erstellt eine *CentOS* virtuellen Computer mithilfe eines SSH-Schlüssels gespeichert *~/.ssh/id_rsa.pub*. Beantworten Sie die Anweisungen für Storage-Kontoname, DNS-Namen und Administrator-Anmeldeinformationen:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Melden Sie sich an den virtuellen Computer, die die öffentliche IP-Adresse am Ende des vorherigen Schritts der VM-Erstellung angezeigt:

```bash
ssh azureuser@40.78.23.145
```

Um den Installationsverzeichnissen für MongoDB hinzuzufügen, erstellen eine **Yum** Repositorydatei wie folgt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

Öffnen Sie die MongoDB-Repository-Datei zur Bearbeitung. Fügen Sie die folgenden Zeilen hinzu:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

Installieren Sie mithilfe von MongoDB **Yum** wie folgt:

```bash
sudo yum install -y mongodb-org
```

Standardmäßig ist SELinux CentOS-Images, die verhindert, dass Sie den Zugriff auf MongoDB durchgesetzt. Installieren Sie Gruppenrichtlinienverwaltungs-Tools zu, und konfigurieren Sie SELinux damit MongoDB am TCP-Standardport 27017 wie folgt ausgeführt werden können. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Starten Sie die MongoDB-Dienst wie folgt:

```bash
sudo service mongod start
```

Überprüfen Sie die MongoDB-Installation durch Herstellen einer Verbindung mit der lokalen `mongo` Client:

```bash
mongo
```

Jetzt testen Sie die MongoDB-Instanz, indem einige Daten hinzufügen und dann suchen:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Falls gewünscht, konfigurieren Sie die MongoDB für den automatischen start bei einem Neustart des Systems:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Erstellen Sie grundlegende MongoDB-Instanz auf CentOS, die mithilfe einer Vorlage
Sie können eine grundlegende MongoDB-Instanz auf einem einzelnen CentOS virtuellen Computer mithilfe der folgenden Azure-Schnellstart-Vorlage von GitHub erstellen. Diese Vorlage verwendet die benutzerdefinierte skripterweiterung für Linux, um das Hinzufügen einer `yum` Repository für Ihre neu erstellte virtueller CentOS-Computer und anschließendes Installieren MongoDB.

* [Grundlegende MongoDB-Instanz auf CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in die `eastus` Region. Geben Sie Ihre eigenen Werte wie folgt aus:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> Die Azure-CLI gibt Sie an einer Eingabeaufforderung innerhalb weniger Sekunden zum Erstellen der Bereitstellungsstatus, aber die Installation und Konfiguration kann einige Minuten dauern. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`, den Namen der Ressourcengruppe entsprechend eingeben. Warten Sie, bis die **ProvisioningState** zeigt *erfolgreich* bevor Sie versuchen, SSH eine Verbindung mit dem virtuellen Computer.

Nachdem die Bereitstellung abgeschlossen haben, SSH eine Verbindung mit dem virtuellen Computer. Die IP-Adresse Ihres virtuellen Computers mithilfe der `azure vm show` Befehl wie im folgenden Beispiel gezeigt:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Gegen Ende der Ausgabe ist die öffentliche IP-Adresse angezeigt. SSH eine Verbindung mit Ihrem virtuellen Computer mit der IP-Adresse Ihres virtuellen Computers:

```bash
ssh azureuser@138.91.149.74
```

Überprüfen Sie die MongoDB-Installation durch Herstellen einer Verbindung mit der lokalen `mongo` Client wie folgt:

```bash
mongo
```

Jetzt testen Sie die Instanz, indem einige Daten hinzufügen und Sie wie folgt suchen:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Erstellen Sie einen komplexen mit Shards MongoDB-Cluster auf CentOS, die mithilfe einer Vorlage
Sie können einen komplexe mithilfe der folgenden Azure-Schnellstart-Vorlage von GitHub mit Shards MongoDB-Cluster erstellen. Diese Vorlage folgt die [MongoDB mit Shards konfigurierten Cluster bewährte](https://docs.mongodb.com/manual/core/sharded-cluster-components/) um Redundanz und hohe Verfügbarkeit bereitzustellen. Die Projektvorlage erstellt zwei Shards mit drei Knoten in jeder Replikatsatz. Eine Config Server Replikatgruppe mit drei Knoten auch erstellt, plus 2 **Mongos** Routerservern für die Konsistenz von Anwendungen über die Shards hinweg bereitzustellen.

* [MongoDB Sharding Cluster auf CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Bereitstellen von diesem komplexen mit Shards MongoDB-Cluster müssen mehr als 20 Kerne, dem es sich gewöhnlich um die standardmäßige Anzahl von Kernen pro Region für ein Abonnement. Öffnen Sie eine Azure-Support-Anforderung an die Anzahl von Kernen erhöht.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *MyResourceGroup* in der *Eastus* Region. Geben Sie Ihre eigenen Werte wie folgt aus:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> Der Azure-CLI gibt Sie an einer Eingabeaufforderung innerhalb weniger Sekunden zum Erstellen der Bereitstellungsstatus, aber die Installation und Konfiguration können mehr als eine Stunde dauern, abgeschlossen. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`, den Namen der Ressourcengruppe entsprechend anpassen. Warten Sie, bis die **ProvisioningState** zeigt *erfolgreich* vor dem Herstellen einer Verbindung mit den virtuellen Computern.


## <a name="next-steps"></a>Nächste Schritte
In diesen Beispielen verbinden Sie mit der MongoDB-Instanz lokal von der virtuellen Maschine. Wenn Sie mit der MongoDB-Instanz aus einem anderen virtuellen Computer oder Netzwerk herstellen möchten, stellen Sie sicher das entsprechende [Netzwerksicherheitsgruppe Regeln erstellt werden](nsg-quickstart.md).

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter der [Azure Resource Manager Overview](../../azure-resource-manager/resource-group-overview.md).

Die Azure-Ressourcen-Manager-Vorlagen verwenden die benutzerdefinierte Skripterweiterung herunterladen und Ausführen von Skripts auf Ihren virtuellen Computern. Weitere Informationen finden Sie unter [verwenden die benutzerdefinierte Skripterweiterung von Azure mit virtuelle Linux-Computer](extensions-customscript.md).

