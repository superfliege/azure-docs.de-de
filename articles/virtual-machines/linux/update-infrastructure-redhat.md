---
title: Red Hat-Updateinfrastruktur | Microsoft-Dokumentation
description: "Hier finden Sie Informationen zur Red Hat-Updateinfrastruktur für On-Demand-Red Hat Enterprise Linux-Instanzen in Microsoft Azure."
services: virtual-machines-linux
documentationcenter: 
author: BorisB2015
manager: timlt
editor: 
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/10/2017
ms.author: borisb
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: ea9a5cbd9b9b7b67ceb131cb8ba1d2476dbd5f72
ms.contentlocale: de-de
ms.lasthandoff: 09/27/2017

---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat-Updateinfrastruktur für virtuelle On-Demand-Red Hat Enterprise Linux-VMs in Azure
 Mit der [Red Hat-Updateinfrastruktur](https://access.redhat.com/products/red-hat-update-infrastructure) können Cloudanbieter (z. B. Azure) in Red Hat gehostete Repositoryinhalte spiegeln, benutzerdefinierte Repositorys mit Azure-spezifischem Inhalt erstellen und diese für Endbenutzer-VMs zur Verfügung stellen.

RHEL-Images (Red Hat Enterprise Linux) mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG) sind bereits für den Zugriff auf die Azure-RHUI vorkonfiguriert. Es ist keine zusätzliche Konfiguration erforderlich. Führen Sie zum Abrufen der neuesten Updates `sudo yum update` aus, nachdem Ihre RHEL-Instanz bereit ist. Dieser Dienst ist in den RHEL PAYG-Softwaregebühren inbegriffen.

## <a name="important-information-about-azure-rhui"></a>Wichtige Informationen zur Azure-RHUI
* Azure-RHUI unterstützt derzeit nur die neueste Nebenversion in den einzelnen RHEL-Familien (RHEL6 oder RHEL7). Führen Sie `sudo yum update` aus, um für eine mit RHUI verbundene RHEL VM-Instanz ein Upgrade auf die neueste Nebenversion durchzuführen.

    Wenn Sie beispielsweise eine VM aus einem RHEL 7.2 PAYG-Image bereitstellen und `sudo yum update` ausführen, erhalten Sie letztlich eine RHEL 7.4-VM (die neueste Nebenversion der RHEL7-Familie).

    Um dieses Verhalten zu vermeiden, müssen Sie Ihr eigenes Image erstellen, wie im Artikel [Erstellen und Hochladen eines auf Red Hat basierenden virtuellen Computers für Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) beschrieben. Dann müssen Sie es mit einer anderen Updateinfrastruktur verbinden ([direkt auf Übermittlungsservern für Red Hat-Inhalte](https://access.redhat.com/solutions/253273) oder auf einem [Red Hat Satellite-Server](https://access.redhat.com/products/red-hat-satellite)).

* Der Zugriff auf die in Azure gehostete RHUI ist im Preis für das RHEL PAYG-Image enthalten. Wenn Sie die Registrierung eines virtuellen PAYG-RHEL-Computers bei der von Azure gehosteten RHUI aufheben, die den virtuellen Computer nicht in einen BYOL-Typ (Bring Your Own License) des virtuellen Computers konvertiert. Wenn Sie denselben virtuellen Computer mit einer anderen Updatequelle registrieren, fallen möglicherweise _indirekt_ doppelte Kosten an. Sie werden das erste Mal mit der Gebühr für die Azure RHEL-Software belastet. Sie werden das zweite Mal für Red Hat-Abonnements belastet, die zuvor gekauft wurden. Wenn Sie anstelle der in Azure gehosteten RHUI durchweg eine andere Updateinfrastruktur verwenden müssen, empfiehlt es sich unter Umständen, eigene (BYOL-)Images zu erstellen und bereitzustellen. Eine entsprechende Prozessbeschreibung finden Sie unter [Erstellen und Hochladen eines auf Red Hat basierenden virtuellen Computers für Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Zwei Klassen von RHEL PAYG-Images in Azure (RHEL for SAP HANA und RHEL for SAP Business Applications) sind mit dedizierten RHUI-Kanälen verbunden, die gemäß der SAP-Zertifizierung in der jeweiligen RHEL-Nebenversion verbleiben. 

* Der Zugriff auf die in Azure gehostete RHUI ist auf virtuelle Computer innerhalb der [IP-Bereiche des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) beschränkt. Wenn Sie für den gesamten VM-Datenverkehr über die lokale Netzwerkinfrastruktur die Proxyfunktion verwenden, müssen Sie möglicherweise benutzerdefinierte Routen für die RHEL PAYG-VMs einrichten, um auf die Azure-RHUI zuzugreifen.

### <a name="the-ips-for-the-rhui-content-delivery-servers"></a>IP-Adressen der Server für die RHUI-Inhaltsübermittlung

RHUI ist in allen Regionen verfügbar, in denen RHEL-On-Demand-Images verfügbar sind. Dazu zählen derzeit alle öffentlichen Regionen, die auf der Seite [Dashboard zum Azure-Status](https://azure.microsoft.com/status/) angegeben sind, sowie die Regionen „Azure US-Regierung“ und „Microsoft Azure Deutschland“. 

Wenn Sie den Zugriff von virtuellen RHEL-PAYG-Computern per Netzwerkkonfiguration beschränken, stellen Sie sicher, dass die folgenden IP-Adressen zugelassen sind, damit `yum update` funktioniert – abhängig von der Umgebung, in der Sie sich befinden: 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="rhui-azure-infrastructure-update"></a>RHUI – Update der Azure-Infrastruktur

Seit September 2016 haben wir eine aktualisierte Azure-RHUI bereitgestellt. Im April 2017 wurde die alte Azure-RHUI eingestellt. Wenn Sie die RHEL PAYG-Images (oder die zugehörigen Momentaufnahmen) ab September 2016 verwendet haben, werden Sie automatisch mit der neuen Azure-RHUI verbunden. Wenn Sie jedoch über ältere Momentaufnahmen auf Ihren virtuellen Computern verfügen, muss deren Konfiguration wie in einem der nachfolgenden Abschnitte beschrieben manuell aktualisiert werden, um den Zugriff auf die Azure-RHUI sicherzustellen.

Die neuen Azure-RHUI-Server werden mit [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) bereitgestellt. In Traffic Manager kann ein einzelner Endpunkt (rhui-1.microsoft.com) unabhängig von der Region von einem beliebigen virtuellen Computer verwendet werden. 

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Beheben von Problemen bei der Verbindung mit der Azure-RHUI
Wenn beim Herstellen einer Verbindung mit der Azure-RHUI von Ihrem virtuellen Azure-RHEL-PAYG-Computer Probleme auftreten, gehen Sie folgendermaßen vor:

1. Überprüfen Sie, welcher Azure-RHUI-Endpunkt für den virtuellen Computer konfiguriert ist:

    a. Überprüfen Sie, ob die Datei `/etc/yum.repos.d/rh-cloud.repo` in der `baseurl` im Abschnitt `[rhui-microsoft-azure-rhel*]` der Datei einen Verweis auf `rhui-[1-3].microsoft.com` enthält. Wenn dies der Fall ist, verwenden Sie die neue Azure-RHUI.

    b. Wenn auf einen Speicherort mit folgendem Muster, `mirrorlist.*cds[1-4].cloudapp.net`, verwiesen wird, muss die Konfiguration aktualisiert werden. Sie verwenden die alte Momentaufnahme des virtuellen Computers und müssen sie aktualisieren, damit sie auf die neue Azure-RHUI verweist.

2. Der Zugriff auf die in Azure gehostete RHUI ist auf virtuelle Computer innerhalb der [IP-Bereiche des Azure-Rechenzentrums] beschränkt (https://www.microsoft.com/download/details.aspx?id=41653).
 
3. Wenn Sie die neue Konfiguration verwenden und überprüft haben, ob der virtuelle Computer eine Verbindung über den IP-Bereich von Azure herstellt, und weiterhin keine Verbindung mit der Azure-RHUI hergestellt werden kann, senden Sie eine Supportanfrage an Microsoft oder Red Hat.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Manuelle Aktualisierung zur Verwendung der Azure-RHUI-Server
Dieses Verfahren wird nur zu Referenzzwecken bereitgestellt. RHEL PAYG-Images verfügen bereits über die richtige Konfiguration für die Verbindung mit der Azure-RHUI. Führen Sie die folgenden Schritte aus, um die Konfiguration zur Verwendung von Azure-RHUI-Servern manuell zu aktualisieren:

1. Laden Sie mithilfe von „curl“ die Signatur des öffentlichen Schlüssels herunter.

   ```bash
   curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
   ```

2. Überprüfen Sie die Gültigkeit des heruntergeladenen Schlüssels.

   ```bash
   gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
   ```

3. Überprüfen Sie die Ausgabe und dann die `keyid` und das `user ID packet`.

   ```bash
   Version: GnuPG v1.4.7 (GNU/Linux)
   :public key packet:
           version 4, algo 1, created 1446074508, expires 0
           pkey[0]: [2048 bits]
           pkey[1]: [17 bits]
           keyid: EB3E94ADBE1229CF
   :user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
   :signature packet: algo 1, keyid EB3E94ADBE1229CF
           version 4, created 1446074508, md5len 0, sigclass 0x13
           digest algo 2, begin of digest 1a 9b
           hashed subpkt 2 len 4 (sig created 2015-10-28)
           hashed subpkt 27 len 1 (key flags: 03)
           hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
           hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
           hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
           hashed subpkt 30 len 1 (features: 01)
           hashed subpkt 23 len 1 (key server preferences: 80)
           subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
           data: [2047 bits]
   ```

4. Installieren Sie den öffentlichen Schlüssel.

   ```bash
   sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
   sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
   ```

5. Laden Sie einen Client-RPM-Paket-Manager herunter. Anschließend überprüfen und installieren Sie ihn.
    
    >[!NOTE]
    >Die Paketversionen ändern sich. Wenn Sie manuell eine Verbindung mit der Azure-RHUI herstellen, können Sie die neueste Version des Clientpakets für die einzelnen RHEL-Familien durch Bereitstellen des neuesten Images aus dem Katalog beziehen.
  
   a. Herunterladen 
   
    - Download für RHEL 6:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.1-32.noarch.rpm 
        ```
    
    - Download für RHEL 7:
        ```bash
        curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.1-19.noarch.rpm  
        ```

   b. Überprüfen

   ```bash
   rpm -Kv azureclient.rpm
   ```

   c. Überprüfen Sie die Ausgabe, um eine ordnungsgemäße Signatur des Pakets sicherzustellen.

   ```bash
   azureclient.rpm:
       Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
       Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
       V3 RSA/SHA256 Signature, key ID be1229cf: OK
       MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
   ```

   d. Installieren Sie das RPM-Paket.

    ```bash
    sudo rpm -U azureclient.rpm
    ```

6. Vergewissern Sie sich nach Abschluss des Vorgangs, dass Sie über den virtuellen Computer auf die Azure-RHUI zugreifen können.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie einen virtuellen Red Hat Enterprise Linux-Computer auf der Grundlage eines Azure Marketplace-Images mit nutzungsbasierter Bezahlung (PAYG) erstellen und die in Azure gehostete RHUI verwenden möchten, besuchen Sie den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). 
