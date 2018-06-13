---
title: Azure Stack-Schnellstart – Erstellen einer VM mit dem Portal
description: Azure Stack-Schnellstart – Erstellen einer Linux-VM mit dem Portal
services: azure-stack
cloud: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 2ea07f04d4c566c0add39d75cad3d3a4ed81c6c8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32152219"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-with-the-azure-stack-portal"></a>Schnellstart: Erstellen eines virtuellen Linux-Servers mit dem Azure Stack-Portal

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können einen virtuellen Computer mit Ubuntu Server 16.04 LTS über das Azure Stack-Portal erstellen. Führen Sie die Schritte in diesem Artikel aus, um einen virtuellen Computer zu erstellen und zu verwenden. Darüber hinaus erfahren Sie in diesem Artikel, wie Sie folgende Schritte ausführen:

* Herstellen einer Verbindung mit dem virtuellen Computer über einen Remoteclient
* Installieren eines NGINX-Webservers
* Bereinigen Ihrer Ressourcen

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Linux-Image im Azure Stack-Marketplace**

   Ein Linux-Image ist standardmäßig nicht im Azure Stack-Marketplace enthalten. Um einen virtuellen Linux-Server erstellen zu können, muss der Azure Stack-Bediener das benötigte Image für **Ubuntu Server 16.04 LTS** bereitstellen. Hierzu kann der Bediener die Schritte des Artikels [Herunterladen von Marketplace-Elementen von Azure in Azure Stack](../azure-stack-download-azure-marketplace-item.md) ausführen.

* **Zugriff auf einen SSH-Client**

   Wenn Sie das Azure Stack Development Kit (ASDK) verwenden, haben Sie unter Umständen keinen Zugriff auf einen SSH-Client. Für den Fall, dass Sie einen Client benötigen, stehen mehrere Pakete zur Verfügung, die einen SSH-Client enthalten. So enthält beispielsweise PuTTY einen SSH-Client und einen SSH-Schlüsselgenerator (puttygen.exe). Weitere Informationen zu verfügbaren Paketen finden Sie im folgenden Azure-Artikel: [Gewusst wie: Verwenden von SSH-Schlüsseln mit Windows in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   In dieser Schnellstartanleitung wird PuTTY verwendet, um die SSH-Schlüssel zu generieren und eine Verbindung mit dem virtuellen Linux-Server herzustellen. Navigieren Sie zu [http://www.putty.org/](http://www.putty.org), um PuTTY herunterzuladen und zu installieren.

## <a name="create-an-ssh-key-pair"></a>Erstellen eines SSH-Schlüsselpaars

Für die Schritte in diesem Artikel benötigen Sie ein SSH-Schlüsselpaar. Falls Sie bereits über ein SSH-Schlüsselpaar verfügen, können Sie diesen Schritt überspringen.

1. Navigieren Sie zum PuTTY-Installationsordner (der Standardspeicherort ist ```C:\Program Files\PuTTY```), und führen Sie ```puttygen.exe``` aus.
2. Vergewissern Sie sich im Fenster „PuTTY Key Generator“, dass **Type of key to generate** auf **RSA** und die **Number of bits in a generated key** auf **2048** festgelegt ist. Klicken Sie abschließend auf **Generate** (Generieren).

   ![PuTTY Key Generator: Konfiguration](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Bewegen Sie den Mauszeiger nach Belieben im Fenster von PuTTY Key Generator, um einen Schlüssel zu generieren.
4. Klicken Sie nach Abschluss der Schlüsselgenerierung auf **Save public key** (Öffentlichen Schlüssel speichern) und anschließend auf **Save private key** (Privaten Schlüssel speichern), um Ihre Schlüssel in Dateien zu speichern.

   ![PuTTY Key Generator: Ergebnisse](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Anmelden beim Azure Stack-Portal

Melden Sie sich beim Azure Stack-Portal an. Die Adresse des Azure Stack-Portals hängt davon ab, mit welchem Azure Stack-Produkt Sie eine Verbindung herstellen:

* Navigieren Sie für das Azure Stack Development Kit (ASDK) zu folgender Adresse: https://portal.local.azurestack.external.
* Rufen Sie bei einem integrierten Azure Stack-System die vom Azure Stack-Operator bereitgestellte URL auf.

## <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers

1. Klicken Sie links oben im Azure Stack-Portal auf **Ressource erstellen**.

2. Wählen Sie **Compute** und dann **Ubuntu Server 16.04 LTS**.
3. Klicken Sie auf **Create**.

4. Geben Sie die Informationen zum virtuellen Computer ein. Wählen Sie unter **Authentifizierungstyp** die Option **Öffentlicher SSH-Schlüssel**. Fügen Sie den öffentlichen SSH-Schlüssel ein, den Sie zuvor gespeichert haben, und klicken Sie anschließend auf **OK**.

   >[!NOTE]
 Entfernen Sie alle voran- und nachgestellten Leerzeichen des Schlüssels.

   ![Bereich „Grundlagen“: Konfigurieren des virtuellen Computers](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Wählen Sie **D1_V2** für den virtuellen Computer aus.

   ![Bereich „Größe“: Auswählen einer VM-Größe](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Behalten Sie auf der Seite **Einstellungen** die Standardwerte bei, und klicken Sie auf **OK**.

7. Klicken Sie auf der Seite **Zusammenfassung** auf **OK**, um die Bereitstellung des virtuellen Computers zu starten.

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer

1. Klicken Sie auf der Seite des virtuellen Computers auf **Verbinden**. Die daraufhin angezeigte SSH-Verbindungszeichenfolge wird für die Verbindungsherstellung mit dem virtuellen Computer benötigt.

   ![Herstellen der Verbindung mit dem virtuellen Computer](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. Öffnen Sie PuTTY.
3. Im Bildschirm für die **PuTTY-Konfiguration**können Sie im Bereich mit den **Kategorien**nach oben oder unten scrollen. Scrollen Sie nach unten zu **SSH**, erweitern Sie **SSH**, und klicken Sie anschließend auf **Auth** (Authentifizieren). Klicken Sie auf **Browse** (Durchsuchen), und wählen Sie die Datei mit dem privaten Schlüssel aus, die Sie zuvor gespeichert haben.

   ![Auswählen des privaten PuTTY-Schlüssels](media/azure-stack-quick-linux-portal/Putty03.PNG)

4. Scrollen Sie im Bereich mit den **Kategorien**nach oben, und klicken Sie auf **Session** (Sitzung).
5. Fügen Sie im Feld **Host Name (or IP address)** (Hostname (oder IP-Adresse)) die Verbindungszeichenfolge aus dem Azure Stack-Portal ein. In diesem Beispiel ist dies ```asadmin@192.168.102.34```.

   ![PuTTY-Konfiguration: Verbindungszeichenfolge](media/azure-stack-quick-linux-portal/Putty04.PNG)

6. Klicken Sie auf **Open** (Öffnen), um eine Sitzung für den virtuellen Computer zu öffnen.

   ![Linux-Sitzung](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>Installieren des NGINX-Webservers

Verwenden Sie die folgenden Bash-Befehle, um Paketquellen zu aktualisieren und das neueste NGINX-Paket auf dem virtuellen Computer zu installieren.

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Beenden Sie die SSH-Sitzung nach Abschluss der NGINX-Installation, und öffnen Sie im Azure Stack-Portal die Übersichtsseite des virtuellen Computers.

## <a name="open-port-80-for-web-traffic"></a>Öffnen von Port 80 für Webdatenverkehr

Mit einer Netzwerksicherheitsgruppe (NSG) wird eingehender und ausgehender Datenverkehr geschützt. Bei der Erstellung eines virtuellen Computers im Azure Stack-Portal wird für SSH-Verbindungen an Port 22 eine Regel für eingehenden Datenverkehr erstellt. Da dieser virtuelle Computer einen Webserver hostet, muss eine NSG-Regel erstellt werden, die Webdatenverkehr am Port 80 zulässt.

1. Klicken Sie auf der Seite **Übersicht** des virtuellen Computers auf den Namen der **Ressourcengruppe**.
2. Wählen Sie die **Netzwerksicherheitsgruppe** für den virtuellen Computer aus. Die NSG können Sie anhand der Spalte **Typ** identifizieren.
3. Klicken Sie im Menü auf der linken Seite unter **Einstellungen** auf **Eingangssicherheitsregeln**.
4. Klicken Sie auf **Hinzufügen**.
5. Geben Sie unter **Name** den Text **http** ein. Stellen Sie sicher, dass **Portbereich** auf 80 und **Aktion** auf **Zulassen** festgelegt ist.
6. Klicken Sie auf **OK**

## <a name="view-the-nginx-welcome-page"></a>Anzeigen der NGINX-Willkommensseite

Nachdem NGINX installiert und der Port 80 auf dem virtuellen Computer geöffnet wurde, können Sie über die öffentliche IP-Adresse des virtuellen Computers auf den Webserver zugreifen. (Die öffentliche IP-Adresse wird auf der Übersichtsseite des virtuellen Computers angezeigt.)

Öffnen Sie einen Webbrowser, und gehen Sie zu ```http://<public IP address>```.

![Willkommensseite des NGINX-Webservers](media/azure-stack-quick-linux-portal/linux-04.PNG)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereinigen Sie die Ressourcen, die Sie nicht mehr benötigen. Wählen Sie auf der Seite des virtuellen Computers die Ressourcengruppe aus, und klicken Sie auf **Löschen**, um den virtuellen Computer und seine Ressourcen zu löschen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen einfachen virtuellen Linux-Server mit einem Webserver bereitgestellt. Um weitere Informationen zu virtuellen Computern unter Azure Stack zu erhalten, fahren Sie mit [Considerations for Virtual Machines in Azure Stack](azure-stack-vm-considerations.md) (Überlegungen zu virtuellen Computern in Azure Stack) fort.
