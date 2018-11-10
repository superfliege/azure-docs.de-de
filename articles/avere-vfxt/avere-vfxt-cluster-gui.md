---
title: Zugreifen auf die Avere vFXT-Systemsteuerung – Azure
description: Herstellen einer Verbindung mit dem vFXT-Cluster und mit der browserbasierten Avere-Systemsteuerung zum Konfigurieren von Avere vFXT
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: c48f0d8f7ad34db585f4deae566641b6453357e8
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670057"
---
# <a name="access-the-vfxt-cluster"></a>Zugreifen auf den vFXT-Cluster

Verwenden Sie die Avere-Systemsteuerung, um Einstellungen zu ändern und den Avere vFXT-Cluster zu überwachen. Die Avere-Systemsteuerung ist eine browserbasierte grafische Oberfläche für den Cluster.

Da sich der vFXT-Cluster in einem privaten virtuellen Netzwerk befindet, müssen Sie einen SSH-Tunnel erstellen oder eine andere Methode verwenden, um die IP-Adresse für die Clusterverwaltung zu erreichen. Es gibt zwei grundlegende Schritte: 

1. Erstellen einer Verbindung zwischen Ihrem Arbeitsplatz und dem privaten VNet 
1. Verwenden der IP-Adresse zur Clusterverwaltung, um die Systemsteuerung in einem Webbrowser zu laden 

> [!NOTE] 
> In diesem Artikel wird davon ausgegangen, dass Sie eine öffentliche IP-Adresse auf dem Clustercontroller oder auf einem anderen virtuellen Computer innerhalb des virtuellen Netzwerks Ihres Clusters festgelegt haben. Wenn Sie ein VPN oder ExpressRoute für den VNet-Zugriff verwenden, wechseln Sie zu [Herstellen einer Verbindung mit der Avere-Systemsteuerung](#connect-to-the-avere-control-panel-in-a-browser).

Stellen Sie vor dem Herstellen der Verbindung sicher, dass das SSH-Schlüsselpaar „public/private“, das Sie beim Erstellen des Clustercontrollers verwendet haben, auf Ihrem lokalen Computer installiert ist. Lesen Sie die Dokumentation zu SSH-Schlüsseln für [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) oder für [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows), wenn Sie Hilfe benötigen.  

## <a name="access-with-a-linux-host"></a>Zugreifen mit einem Linux-Host

über dieses Formular: 

ssh -L *local_port*:*cluster_mgmt_ip*:443 *controller_username*@*controller_public_IP* 

Dieser Befehl stellt über die IP-Adresse des Clustercontrollers eine Verbindung mit der IP-Adresse für die Clusterverwaltung her.

Beispiel:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

Die Authentifizierung erfolgt automatisch, wenn Sie Ihren öffentlichen SSH-Schlüssel zur Erstellung des Clusters verwendet haben und der passende Schlüssel auf dem Clientsystem installiert ist.


## <a name="access-with-a-windows-host"></a>Zugreifen mit einem Windows-Host

Wenn Sie PuTTY verwenden, füllen Sie das Feld **Hostname** mit dem Benutzernamen des Clustercontrollers und seiner IP-Adresse aus: *Ihr_Benutzername*@*Öffentliche_IP_Controller*.

Beispiel: ``azureuser@203.0.113.51``

Im Fenster **Konfiguration**:

1. Erweitern Sie **Verbindung** > **SSH** auf der linken Seite. 
1. Klicken Sie auf **Tunnel**. 
1. Geben Sie einen Quellport ein, z. B. 8443. 
1. Geben Sie für das Ziel die IP-Adresse für die Verwaltung des vFXT-Clusters und den Port 443 ein. 
   Beispiel: ``203.0.113.51:443``
1. Klicken Sie auf **Hinzufügen**.
1. Klicken Sie auf **Öffnen**.

![Screenshot der Putty-Anwendung, der zeigt, wo Sie klicken müssen, um einen Tunnel hinzuzufügen](media/avere-vfxt-ptty-numbered.png)

Die Authentifizierung erfolgt automatisch, wenn Sie Ihren öffentlichen SSH-Schlüssel zur Erstellung des Clusters verwendet haben und der passende Schlüssel auf dem Clientsystem installiert ist.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Herstellen einer Verbindung mit der Avere-Systemsteuerung in einem Browser

Dieser Schritt verwendet einen Webbrowser, um eine Verbindung mit dem Konfigurationsprogramm herzustellen, das auf dem vFXT-Cluster ausgeführt wird.

Öffnen Sie Ihren Webbrowser, und navigieren Sie zu https://127.0.0.1:8443. 

Je nach Browser müssen Sie möglicherweise auf **Erweitert** klicken und überprüfen, ob die Weiterleitung zur Seite sicher ist.

Geben Sie den Benutzernamen `admin` und das Kennwort ein, das Sie beim Erstellen des Clusters angegeben haben.

![Screenshot der Avere-Anmeldeseite, die mit dem Benutzernamen „admin“ und einem Kennwort gefüllt ist](media/avere-vfxt-gui-login.png)

Klicken Sie auf **Anmelden**, oder drücken Sie die EINGABETASTE auf Ihrer Tastatur.

## <a name="next-steps"></a>Nächste Schritte

Aktivieren Sie jetzt, da Sie auf den Cluster zugreifen können, die Option [Support](avere-vfxt-enable-support.md).
