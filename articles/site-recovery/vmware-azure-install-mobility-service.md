---
title: Installieren des Mobilitätsdiensts für die Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Mobilitätsdienst-Agent installieren, um mit dem Azure Site Recovery-Dienst eine Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure auszuführen.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 30b177578464653499cdcde8cacf65defa5548ef
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846911"
---
# <a name="install-the-mobility-service-for-disaster-recovery-of-vmware-vms-and-physical-servers"></a>Installieren des Mobilitätsdiensts für die Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern

Wenn Sie die Notfallwiederherstellung für VMware-VMs und physische Server mithilfe von [Azure Site Recovery](site-recovery-overview.md) einrichten, installieren Sie den [Site Recovery-Mobilitätsdienst](vmware-physical-mobility-service-overview.md) auf jedem lokalen virtuellen VMware-Computer und physischen Server.  Der Mobilitätsdienst erfasst Datenschreibvorgänge auf dem Computer und leitet sie an den Site Recovery-Prozessserver weiter.

## <a name="install-on-windows-machine"></a>Installieren auf einem Windows-Computer

Führen Sie auf jedem Windows-Computer, den Sie schützen möchten, folgende Schritte aus:

1. Stellen Sie sicher, dass zwischen dem Computer und dem Prozessserver eine Netzwerkverbindung besteht. Wenn Sie keinen separaten Prozessserver eingerichtet haben, wird er standardmäßig auf dem Konfigurationsserver ausgeführt.
1. Erstellen Sie ein Konto, das vom Prozessserver zum Zugreifen auf den Computer verwendet werden kann. Das Konto muss über Administratorrechte verfügen (entweder lokal oder für die Domäne). Verwenden Sie dieses Konto nur für die Pushinstallation und für Agent-Updates.
2. Wenn Sie kein Domänenkonto verwenden, deaktivieren Sie die Zugriffssteuerung für Remotebenutzer auf dem lokalen Computer wie folgt:
    - Fügen Sie unter dem Registrierungsschlüssel „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System“ einen neuen DWORD-Eintrag hinzu: **LocalAccountTokenFilterPolicy**. Legen Sie den Wert auf **1** fest.
    -  Führen Sie den folgenden Befehl aus, wenn Sie eine Eingabeaufforderung verwenden möchten:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d
3. Wählen Sie in der Windows-Firewall des Computers, den Sie schützen möchten, **Allow an app or feature through Firewall** (App oder Feature durch die Windows-Firewall zulassen) aus. Aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation (WMI)**. Für Computer, die zu einer Domäne gehören, können Sie die Firewalleinstellungen konfigurieren, indem Sie ein Gruppenrichtlinienobjekt (Group Policy Object, GPO) verwenden.

   ![Firewalleinstellungen](./media/vmware-azure-install-mobility-service/mobility1.png)

4. Fügen Sie das Konto hinzu, das Sie im CSPSConfigtool erstellt haben. Melden Sie sich dazu beim Konfigurationsserver an.
5. Öffnen Sie die Datei **cspsconfigtool.exe**. Sie ist als Verknüpfung auf dem Desktop und im Ordner „%ProgramData%\home\svsystems\bin“ verfügbar.
6. Wählen Sie auf der Registerkarte **Konten verwalten** die Option **Konto hinzufügen**.
7. Fügen Sie das von Ihnen erstellte Konto hinzu.
8. Geben Sie die Anmeldeinformationen ein, die Sie verwenden, wenn Sie die Replikation für einen Computer aktivieren.

## <a name="install-on-linux-machine"></a>Installieren auf einem Linux-Computer

Führen Sie auf jedem Linux-Computer, den Sie schützen möchten, folgende Schritte aus:

1. Stellen Sie sicher, dass zwischen dem Linux-Computer und dem Prozessserver eine Netzwerkverbindung besteht.
2. Erstellen Sie ein Konto, das vom Prozessserver zum Zugreifen auf den Computer verwendet werden kann. Das Konto muss einem **Root**-Benutzer auf dem Linux-Quellserver entsprechen. Verwenden Sie dieses Konto nur für die Pushinstallation und für Updates.
3. Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, mit denen die Namen des lokalen Hosts den IP-Adressen zugeordnet werden, die allen Netzwerkkarten zugeordnet sind.
4. Installieren Sie die neuesten openssh-, openssh-server- und openssl-Pakete auf dem Computer, den Sie replizieren möchten.
5. Stellen Sie sicher, dass Secure Shell (SSH) auf Port 22 aktiviert ist und ausgeführt wird.
4. Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei „sshd_config“. Melden Sie sich dazu als **root** an.
5. Suchen Sie in der Datei **/etc/ssh/sshd_config** nach der Zeile, die mit **PasswordAuthentication** beginnt.
6. Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert in **yes**.
7. Suchen Sie die Zeile, die mit **Subsystem** beginnt, und heben Sie die Auskommentierung der Zeile auf.

      ![Linux](./media/vmware-azure-install-mobility-service/mobility2.png)

8. Starten Sie den Dienst **sshd** neu.
9. Fügen Sie das Konto hinzu, das Sie im CSPSConfigtool erstellt haben. Melden Sie sich dazu beim Konfigurationsserver an.
10. Öffnen Sie die Datei **cspsconfigtool.exe**. Sie ist als Verknüpfung auf dem Desktop und im Ordner „%ProgramData%\home\svsystems\bin“ verfügbar.
11. Wählen Sie auf der Registerkarte **Konten verwalten** die Option **Konto hinzufügen**.
12. Fügen Sie das von Ihnen erstellte Konto hinzu.
13. Geben Sie die Anmeldeinformationen ein, die Sie verwenden, wenn Sie die Replikation für einen Computer aktivieren.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Mobility Service installiert wurde, wählen Sie im Azure-Portal **+ Replizieren** aus, um den Schutz dieser virtuellen Computer zu starten. Erfahren Sie mehr über das Aktivieren der Replikation für [VMware-VMs]\(vmware-azure-enable-replication.md) und [physische Server](physical-azure-disaster-recovery.md#enable-replication).


