---
title: Installieren des Azure Stack Development Kits (ASDK) | Microsoft-Dokumentation
description: In diesem Artikel wird die Installation des Azure Stack Development Kits (ASDK) beschrieben.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 7b8fe61731a9412c61152bc58e55deebb611d011
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="install-the-azure-stack-development-kit-asdk"></a>Installieren des Azure Stack Development Kits (ASDK)
Nachdem Sie den [ASDK-Hostcomputer vorbereitet](asdk-prepare-host.md) haben, kann das ASDK mit den folgenden Schritten in diesem Artikel im Image „CloudBuilder.vhdx“ bereitgestellt werden.

## <a name="install-the-asdk"></a>Installieren des ASDK
Die Schritte in diesem Artikel zeigen, wie das ASDK mit einer grafischen Benutzeroberfläche (GUI) bereitgestellt werden kann, die durch Herunterladen und Ausführen des PowerShell-Skripts **asdk-installer.ps1** bereitgestellt werden kann.

> [!NOTE]
> Die Benutzeroberfläche des Installers für das Azure Stack Development Kit ist ein Open Source-Skript, das auf WCF und PowerShell basiert.


1. Nachdem der Hostcomputer im Image „CloudBuilder.vhdx“ erfolgreich gestartet wurde, können Sie sich mit den Administratoranmeldeinformationen anmelden, die Sie bei der [Vorbereitung des Development Kit-Hostcomputers](asdk-prepare-host.md) für die ASDK-Installation angegeben haben. Diese sollten mit den Anmeldeinformationen für den lokalen Administrator des Development Kit-Hosts identisch sein.
2. Öffnen Sie eine PowerShell-Konsole mit erhöhten Rechten, und führen Sie das Skript **&lt;Laufwerkbuchstabe>\AzureStack_Installer\asdk-installer.ps1** aus (dieses befindet sich jetzt im Image „CloudBuilder.vhdx“ ggf. auf einem anderen Laufwerk als C:\). Klicken Sie auf **Installieren**.

    ![](media/asdk-install/1.PNG) 

3. Wählen Sie im Dropdownfeld **Typ** des Identitätsanbieters die Option **Azure Cloud** oder **AD FS** aus. Geben Sie unter **Lokales Administratorkennwort** im Feld **Kennwort** das lokale Administratorkennwort ein (das mit dem derzeit konfigurierten lokalen Administratorkennwort übereinstimmen muss), und klicken Sie dann auf **Weiter**.
    - **Azure Cloud**: Konfiguriert Azure Active Directory (Azure AD) als Identitätsanbieter. Für die Verwendung dieser Option benötigen Sie eine Internetverbindung, den vollständigen Namen eines Azure AD-Verzeichnismandanten im Format „*Domänenname*.onmicrosoft.com“ oder einen von Azure AD überprüften benutzerdefinierten Domänennamen und globale Administratoranmeldeinformationen für das angegebene Verzeichnis. 
    - **AD FS**: Der standardmäßige Stampverzeichnisdienst wird als Identitätsanbieter verwendet. Das Standardkonto für die Anmeldung lautet azurestackadmin@azurestack.local, und das dazugehörige Kennwort haben Sie im Rahmen des Setupvorgangs angegeben.

    ![](media/asdk-install/2.PNG) 
    
    > [!NOTE]
    > Um auch dann optimale Ergebnisse zu erzielen, wenn Sie eine nicht verbundene Azure Stack-Umgebung mit AD FS als Identitätsanbieter verwenden möchten, wird empfohlen, das ASDK bei bestehender Internetverbindung zu installieren. Auf diese Weise kann die Evaluierungsversion von Windows Server 2016, die Teil der Development Kit-Installation ist, während der Bereitstellung aktiviert werden.
4. Wählen Sie einen für das Development Kit zu verwendenden Netzwerkadapter aus, und klicken Sie dann auf **Weiter**.

    ![](media/asdk-install/3.PNG)

5. Wählen Sie DHCP oder statische Netzwerkkonfiguration für den virtuellen Computer BGPNAT01 aus.
    > [!TIP]
    > Die VM BGPNAT01 ist der Edgerouter, der NAT- und VPN-Funktionen für Azure Stack bietet.

    - **DHCP** (Standard): Der virtuelle Computer ruft die IP-Netzwerkkonfiguration vom DHCP-Server ab.
    - **Statisch**: Verwenden Sie diese Option nur, wenn per DHCP keine gültige IP-Adresse für Azure Stack zugewiesen werden kann. **Eine statische IP-Adresse muss mit der Länge der Subnetzmaske im CIDR-Format (z.B. 10.0.0.5/24) angegeben werden**.
    - Geben Sie eine gültige **Zeitserver-IP-Adresse** ein. In diesem Pflichtfeld wird der Zeitserver festgelegt, der vom Development Kit verwendet wird. Dieser Parameter muss als gültige Zeitserver-IP-Adresse angegeben werden. Servernamen werden nicht unterstützt.

      > [!TIP]
      > Besuchen Sie zum Ermitteln der IP-Adresse eines Zeitservers [pool.ntp.org](http:\\pool.ntp.org), oder pingen Sie „time.windows.com“. 

    - **Optional** legen Sie die folgenden Werte fest:
        - **VLAN-ID**: Legt die VLAN-ID fest. Verwenden Sie diese Option nur, wenn für Host und AzS-BGPNAT01 eine VLAN-ID für den Zugriff auf das physische Netzwerk (und das Internet) konfiguriert werden muss. 
        - **DNS-Weiterleitung**: Ein DNS-Server wird als Teil der Azure Stack-Bereitstellung erstellt. Damit Computer innerhalb der Lösung Namen außerhalb des Stamps auflösen können, stellen Sie Ihren vorhandenen Infrastruktur-DNS-Server bereit. Der DNS-Server innerhalb des Stamps leitet Auflösungsanforderungen, die unbekannte Namen betreffen, an diesen Server weiter.

    ![](media/asdk-install/4.PNG)

6. Auf der Seite **Überprüfen von Netzwerkadaptereigenschaften** sehen Sie eine Statusanzeige. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    ![](media/asdk-install/5.PNG)

9. Klicken Sie auf der Seite **Zusammenfassung** auf **Bereitstellen**, um die ASDK-Installation auf dem Development Kit-Hostcomputer zu starten.

    ![](media/asdk-install/6.PNG)

    > [!TIP]
    > Hier können Sie auch die PowerShell-Setupbefehle kopieren, die zum Installieren des Development Kit verwendet werden. Dies ist hilfreich, falls Sie [das ASDK erneut mithilfe von PowerShell auf dem Hostcomputer bereitstellen müssen](asdk-deploy-powershell.md).

10. Wenn Sie eine Azure AD-Bereitstellung durchführen, werden Sie einige Minuten nach dem Start des Setupvorgangs zum Eingeben der Anmeldeinformationen Ihres globalen Administratorkontos für Azure AD aufgefordert.

    ![](media/asdk-install/7.PNG)

11. Der Bereitstellungsprozess nimmt einige Stunden in Anspruch, und während dieses Zeitraums wird der Hostcomputer automatisch einmal neu gestartet. Falls Sie den Status der Bereitstellung überwachen möchten, können Sie sich als „azurestack\AzureStackAdmin“ anmelden, nachdem der Development Kit-Host neu gestartet wurde. Wenn die Bereitstellung erfolgreich verlaufen ist, zeigt die PowerShell-Konsole folgende Meldung an: **ABGESCHLOSSEN: Aktion 'Bereitstellung'**. 
    > [!IMPORTANT]
    > Wenn Sie sich als lokaler Administrator anmelden, nachdem der Computer der Domäne beigetreten ist, wird der Bereitstellungsstatus nicht angezeigt. Führen Sie die Bereitstellung nicht erneut aus, sondern melden Sie sich stattdessen als „azurestack\AzureStackAdmin“ an, um zu überprüfen, ob sie ausgeführt wird.

    ![](media/asdk-install/8.PNG)

Herzlichen Glückwunsch, Sie haben das ASDK erfolgreich installiert!

Wenn die Bereitstellung aus irgendeinem Grund nicht erfolgreich ist, können Sie [erneut eine Bereitstellung](asdk-redeploy.md) durchführen oder die folgenden PowerShell-Befehle im selben PowerShell-Fenster mit erhöhten Rechten verwenden, um den Bereitstellungsvorgang ab dem letzten erfolgreichen Schritt neu zu starten:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Nächste Schritte
[Konfiguration nach der Bereitstellung](asdk-post-deploy.md)