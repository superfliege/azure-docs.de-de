---
title: Verwalten eines Prozessservers in Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Einrichtung eines Prozessservers für eine VMware-VM verwalten und die Replikation eines physischen Servers in Azure Site Recovery durchführen.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/21/2018
ms.author: ramamill
ms.openlocfilehash: b3c6f1d130a6f5427fe8377bd70de743f0700052
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434839"
---
# <a name="manage-process-servers"></a>Verwalten von Prozessservern

Standardmäßig wird der Prozessserver, der bei der Replikation von VMware-VMs oder physischen Servern nach Azure verwendet wird, auf dem lokalen Computer des Konfigurationsservers installiert. Es gibt eine Reihe von Instanzen, in denen Sie einen separaten Prozessserver einrichten müssen:

- Bei umfangreichen Bereitstellungen benötigen Sie möglicherweise zusätzliche lokale Prozessserver zur Skalierung der Kapazität.
- Für das Failback benötigen Sie eine temporäre Prozessservereinrichtung in Azure. Sie können diese VM nach Abschluss des Failbacks löschen. 

In diesem Artikel werden typische Verwaltungstasks für diese zusätzlichen Prozessserver beschrieben.

## <a name="upgrade-a-process-server"></a>Durchführen eines Upgrades für einen Prozessserver

Führen Sie für Failbackzwecke ein Upgrade für einen lokal oder in Azure ausgeführten Prozessserver durch:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Wenn Sie mithilfe des Azure-Katalogimage einen Prozessserver in Azure für Failbackzwecke erstellen, wird in der Regel die neueste verfügbare Version ausgeführt. Die Site Recovery-Teams veröffentlichen in regelmäßigen Abständen Fehlerbehebungen und Verbesserungen, und es wird empfohlen, Prozessserver aktuell zu halten.



## <a name="reregister-a-process-server"></a>Erneutes Registrieren eines Prozessservers

Wenn Sie erneut einen lokal oder in Azure ausgeführten Prozessserver mit dem Konfigurationsserver registrieren müssen, führen Sie die folgenden Schritte durch:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Nachdem Sie die Einstellungen gespeichert haben, führen Sie folgende Schritte durch:

1. Öffnen Sie als Administrator eine Eingabeaufforderung auf dem Prozessserver.
2. Navigieren Sie zum Ordner **%PROGRAMDATA%\ASR\Agent**, und führen Sie den folgenden Befehl aus:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Ändern von Proxyeinstellungen für einen lokalen Prozessserver

Wenden Sie das nachfolgende Verfahren an, wenn der Prozessserver anhand eines Proxys eine Verbindung mit Site Recovery in Azure herstellt und Sie die vorliegenden Proxyeinstellungen ändern müssen.

1. Melden Sie sich beim Computer des Prozessservers an. 
2. Öffnen Sie als Administrator ein PowerShell-Eingabefenster, und führen Sie den folgenden Befehl aus:
  ```powershell
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
2. Navigieren Sie zum Ordner **%PROGRAMDATA%\ASR\Agent**, und führen Sie den folgenden Befehl aus:
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```


## <a name="remove-a-process-server"></a>Entfernen eines Prozessservers

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]


