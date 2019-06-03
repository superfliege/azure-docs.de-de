---
title: Einrichten von MySQL auf einem virtuellen Linux-Computer in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Stapel auf einem virtuellen Linux-Computer (Betriebssystem der Ubuntu- oder Red Hat-Familie) in Azure installieren.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: 21ad3f9baf4b8e117f881d9a36fc606af04e17a5
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158440"
---
# <a name="how-to-install-mysql-on-azure"></a>Installieren von MySQL in Azure
In diesem Artikel erfahren Sie, wie Sie MySQL auf einem virtuellen Azure-Computer, auf dem Linux ausgeführt wird, installieren und konfigurieren.


> [!NOTE]
> Sie benötigen einen virtuellen Microsoft Azure-Computer mit Linux, um dieses Lernprogramm auszuführen. Bevor Sie fortfahren, informieren Sie sich im [Tutorial zu Azure Linux-VMs](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), wie ein virtueller Linux-Computer mit `mysqlnode` als VM-Namen und `azureuser` als Benutzer erstellt und eingerichtet wird.
> 
> 

Verwenden Sie in diesem Fall Port 3306 als MySQL-Port.  

Wir verwenden zur Veranschaulichung in diesem Artikel das Repositorypaket, um MySQL 5.6 zu installieren. MySQL 5.6 zeichnet sich im Vergleich zu MySQL 5.5 durch mehr Leistung aus.  Weitere Informationen finden Sie [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

## <a name="install-mysql56-on-ubuntu"></a>Installieren von MySQL 5.6 unter Ubuntu
Wir verwenden eine Linux-VM mit Ubuntu.


### <a name="install-mysql"></a>Installieren von MySQL

Installieren Sie MySQL Server 5.6 durch Wechseln zum `root`-Benutzer:

```bash  
sudo su -
```

Installieren von MySQL Server 5.6:

```bash  
apt-get update
apt-get -y install mysql-server-5.6
```

  
Während der Installation werden Sie in einem Dialogfeld aufgefordert, das MySQL-Kennwort für „root“ festzulegen. Dies muss hier geschehen.
  
![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

Geben Sie das Kennwort zur Bestätigung erneut ein.

![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

### <a name="sign-in"></a>Anmelden
  
Der MySQL-Dienst wird automatisch gestartet, sobald die MySQL-Serverinstallation abgeschlossen ist. Sie können sich bei MySQL Server als `root`-Benutzer anmelden und Ihr Kennwort eingeben.

```bash  
mysql -uroot -p
```


### <a name="manage-the-mysql-service"></a>Verwalten des MySQL-Diensts

Abrufen des Status des MySQL-Diensts

```bash   
service mysql status
```
  
Starten des MySQL-Diensts

```bash  
service mysql start
```
  
Beenden des MySQL-Diensts

```bash  
service mysql stop
```
  
Neustarten des MySQL-Diensts

```bash  
service mysql restart
```

## <a name="install-mysql-on-red-hat-os-centos-oracle-linux"></a>Installieren von MySQL unter einem Red Hat-Betriebssystem, CentOS oder Oracle Linux
Wir verwenden hier eine Linux-VM mit CentOS oder Oracle Linux.

### <a name="add-the-mysql-yum-repository"></a>Hinzufügen des MySQL Yum-Repositorys
    
Wechseln Sie zum `root`-Benutzer:

```bash  
sudo su -
```

Laden Sie das Paket der gewünschten MySQL-Version herunter, und installieren Sie es:

```bash  
wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
yum localinstall -y mysql-community-release-el6-5.noarch.rpm
```

### <a name="enable-the-mysql-repository"></a>Aktivieren des MySQL-Repositorys
Bearbeiten Sie die nachstehende Datei, um das MySQL-Repository für das Herunterladen des MySQL 5.6.-Pakets vorzubereiten.

```bash  
vim /etc/yum.repos.d/mysql-community.repo
```

  
Ändern Sie die einzelnen Werte dieser Datei wie folgt:

```  
\# *Enable to use MySQL 5.6*
  
[mysql56-community]
name=MySQL 5.6 Community Server
  
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
enabled=1
  
gpgcheck=1
  
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

### <a name="install-mysql"></a>Installieren von MySQL 

Installieren Sie MySQL aus dem Repository.

```bash  
yum install mysql-community-server
```
  
Das MySQL-RPM-Paket und alle zugehörigen Pakete werden installiert.


## <a name="manage-the-mysql-service"></a>Verwalten des MySQL-Diensts
  
Überprüfen Sie den Dienststatus des MySQL-Servers:

```bash  
service mysqld status\
```
  
Überprüfen Sie, ob der Standardport des MySQL-Servers aktiv ist:

```bash  
netstat  –tunlp|grep 3306
```

Starten Sie den MySQL-Server:

```bash
service mysqld start
```

Beenden Sie den MySQL-Server:

```bash
service mysqld stop
```

Legen Sie fest, dass MySQL beim Starten des Systems gestartet wird:

```bash
chkconfig mysqld on
```

## <a name="install-mysql-on-suse-linux"></a>Installieren von MySQL unter Suse Linux

Wir verwenden hier eine Linux-VM mit OpenSUSE.

### <a name="download-and-install-mysql-server"></a>Herunterladen und Installieren von MySQL Server
  
Wechseln Sie über folgenden Befehl zum Benutzer `root` :  

```bash  
sudo su -
```
  
Herunterladen und Installieren des MySQL-Pakets:

```bash  
zypper update
zypper install mysql-server mysql-devel mysql
```

### <a name="manage-the-mysql-service"></a>Verwalten des MySQL-Diensts
  
Überprüfen Sie den Status des MySQL-Servers:

```bash  
rcmysql status
```
  
Überprüfen, Sie ob der Standardport des MySQL-Servers aktiv ist:

```bash  
netstat  –tunlp|grep 3306
```

Starten Sie den MySQL-Server:

```bash
rcmysql start
```

Beenden Sie den MySQL-Server:

```bash
rcmysql stop
```

Legen Sie fest, dass MySQL beim Starten des Systems gestartet wird:

```bash
insserv mysql
```

## <a name="next-step"></a>Nächster Schritt
Weitere Informationen finden Sie auf der [MySQL](https://www.mysql.com/)-Website.

