---
title: Installieren von MySQL auf einer OpenSUSE-VM in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie MySQL auf einem virtuellen OpenSUSE Linux-Computer in Azure installieren.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: cynthn
ms.openlocfilehash: 88bd895cb3a384f1ada0394fe2da206aca86b981
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38670929"
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Installieren von MySQL auf einem virtuellen Computer mit OpenSUSE Linux in Azure

[MySQL](http://www.mysql.com) ist eine beliebte Open Source-SQL-Datenbank. In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Computer mit OpenSUSE Linux erstellen und anschließend MySQL installieren.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Erstellen eines virtuellen Computers mit OpenSUSE Linux

Erstellen Sie zunächst eine Ressourcengruppe. In diesem Beispiel nennen wir die Ressourcengruppe *mySQSUSEResourceGroup* und erstellen sie in der Region *USA, Osten*.

```azurecli-interactive
az group create --name mySQLSUSEResourceGroup --location eastus
```

Erstellen Sie den virtuellen Computer. In diesem Beispiel nennen wir den virtuellen Computer *myVM*. Wir verwenden hier die VM-Größe *Standard_D2s_v3*, Sie sollten jedoch die [VM-Größe](sizes.md) verwenden, von der Sie glauben, dass sie sich für Ihre Workload am besten eignet.

```azurecli-interactive
az vm create --resource-group mySQLSUSEResourceGroup \
   --name myVM \
   --image openSUSE-Leap \
   --size Standard_D2s_v3 \
   --generate-ssh-keys
```

Sie müssen der Netzwerksicherheitsgruppe auch eine Regel hinzufügen, um Datenverkehr über Port 3306 für MySQL zuzulassen.

```azurecli-interactive
az vm open-port --port 3306 --resource-group mySQLSUSEResourceGroup --name myVM
```

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Zum Herstellen einer Verbindung mit dem virtuellen Computer verwenden Sie SSH. In diesem Beispiel lautet die öffentliche IP-Adresse der VM *10.111.112.113*. Sie können die IP-Adresse in der Ausgabe sehen, wenn Sie die VM erstellen.

```azurecli-interactive  
ssh 10.111.112.113
```

 
## <a name="update-the-vm"></a>Aktualisieren der VM
 
Nachdem Sie eine Verbindung mit der VM hergestellt haben, installieren Sie Systemupdates und Patches. 
   
```bash
sudo zypper update
```

Folgen Sie den Anweisungen, um Ihre VM zu aktualisieren.

## <a name="install-mysql"></a>Installieren von MySQL 


Installieren Sie MySQL über SSH auf der VM. Führen Sie bei Aufforderungen die erforderlichen Aktionen aus.

```bash
sudo zypper install mysql
```
 
Legen Sie fest, dass MySQL beim Start des Systems gestartet wird. 

```bash
sudo systemctl enable mysql
```
Überprüfen Sie, ob MySQL aktiviert ist.

```bash
systemctl is-enabled mysql
```

Die Ausgabe sollte „enabled“ lauten.


## <a name="mysql-password"></a>MySQL-Kennwort

Nach der Installation ist das MySQL-Stammkennwort standardmäßig leer. Führen Sie das Skript **mysql\_secure\_installation** aus, um MySQL zu sichern. Sie werden vom Skript aufgefordert, das MySQL-Stammkennwort zu ändern, anonyme Benutzerkonten zu entfernen, Remote-Stammanmeldeinformationen zu deaktivieren, Testdatenbanken zu entfernen und die Berechtigungstabelle erneut zu laden. 


```bash
mysql_secure_installation
```

## <a name="log-in-to-mysql"></a>Anmelden bei MySQL

Sie können sich jetzt anmelden und die MySQL-Eingabeaufforderung öffnen.

```bash  
mysql -u root -p
```
Damit werden Sie zur MySQL-Eingabeaufforderung weitergeleitet, in der Sie SQL-Anweisungen für die Interaktion mit der Datenbank eingeben können.

Erstellen Sie jetzt einen neuen MySQL-Benutzer.

```   
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Das Semikolon (;) am Ende der Zeile ist entscheidend für die Beendigung des Befehls.


## <a name="create-a-database"></a>Erstellen einer Datenbank


Erstellen Sie eine Datenbank, und gewähren Sie die Benutzerberechtigungen für `mysqluser`.

```   
CREATE DATABASE testdatabase;
GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Benutzernamen und Kennwörter der Datenbank werden nur von Skripts verwendet, die eine Verbindung mit der Datenbank herstellen.  Benutzernamen für Datenbankkonten sind nicht notwendigerweise tatsächliche Benutzerkonten im System.

Ermöglichen Sie die Anmeldung von einem anderen Computer aus. In diesem Beispiel ist *10.112.113.114* die IP-Adresse des Computers, von dem aus wir uns anmelden möchten.

```   
GRANT ALL ON testdatabase.* TO 'mysqluser'@'10.112.113.114' IDENTIFIED BY 'password';
```
   
Geben Sie Folgendes ein, um das Verwaltungsdienstprogramm für die MySQL-Datenbank zu beenden:

```    
quit
```


## <a name="next-steps"></a>Nächste Schritte
Details zu MySQL finden Sie in der [MySQL-Dokumentation](http://dev.mysql.com/doc/index-topic.html).




