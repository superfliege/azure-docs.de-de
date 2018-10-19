---
title: Einrichten eines SMT-Servers für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Einrichten eines SMT-Servers für SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f387c1afe88f2bba476309b2e2e01942d2b7ae5b
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982624"
---
# <a name="setting-up-smt-server-for-suse-linux"></a>Einrichten eines SMT-Servers für SUSE Linux
Für SAP HANA (große Instanzen) besteht keine direkte Verbindung mit dem Internet. Daher ist das Registrieren der Einheit beim Betriebssystemanbieter und das Herunterladen und Anwenden von Patches ein etwas aufwändigerer Vorgang. Unter SUSE Linux ist eine mögliche Lösung ggf. die Einrichtung eines SMT-Servers auf einem virtuellen Azure-Computer. Die Azure-VM muss dagegen in einem Azure VNet gehostet werden, das mit der Einheit von HANA (große Instanzen) verbunden ist. Mit einem SMT-Server dieser Art ist die Einheit von HANA (große Instanzen) dazu in der Lage, Patches zu registrieren und herunterzuladen. 

Für SUSE ist unter [Subscription Management Tool for SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf) (Subscription Management Tool für SLES 12 SP2) ein umfangreicherer Leitfaden verfügbar. 

Als Voraussetzung für die Installation eines SMT-Servers, der diese Aufgabe für HANA (große Instanzen) übernimmt, benötigen Sie Folgendes:

- Ein Azure VNet, das mit der ER-Verbindung von HANA (große Instanzen) verbunden ist.
- Ein SUSE-Konto, das einer Organisation zugeordnet ist. Für die Organisation ist ein gültiges SUSE-Abonnement erforderlich.

## <a name="installation-of-smt-server-on-azure-vm"></a>Installation des SMT-Servers auf einer Azure-VM

In diesem Schritt installieren Sie den SMT-Server auf einer Azure-VM. Die erste Maßnahme ist die Anmeldung beim [SUSE Customer Center](https://scc.suse.com/).

Navigieren Sie nach dem Anmelden zu „Organization“ > „Organization Credentials“. In diesem Abschnitt sind die Anmeldeinformationen enthalten, die zum Einrichten des SMT-Servers erforderlich sind.

Im dritten Schritt installieren Sie eine SUSE Linux-VM im Azure VNet. Verwenden Sie zum Bereitstellen des virtuellen Computers ein SLES 12 SP2-Katalogimage von Azure (wählen Sie das BYOS SUSE-Image aus). Definieren Sie während des Bereitstellungsprozesses keinen DNS-Namen, und verwenden Sie keine statischen IP-Adressen (siehe Screenshot).

![VM-Bereitstellung für SMT-Server](./media/hana-installation/image3_vm_deployment.png)

Die bereitgestellte VM war eine kleinere VM und hat im Azure VNet die interne IP-Adresse 10.34.1.4 erhalten. Der Name der VM lautet „smtserver“. Nach der Installation wurde die Konnektivität mit den Einheiten von HANA (große Instanzen) überprüft. Je nachdem, wie Sie die Namensauflösung organisiert haben, müssen Sie unter Umständen die Auflösung der Einheiten von HANA (große Instanzen) auf der Azure-VM unter „etc/hosts“ konfigurieren. Fügen Sie der VM einen zusätzlichen Datenträger hinzu, der für Patches verwendet werden soll. Der Startdatenträger selbst ist unter Umständen zu klein. Im gezeigten Fall wurde der Datenträger unter „/srv/www/htdocs“ bereitgestellt. Dies ist im folgenden Screenshot dargestellt. Ein Datenträger mit 100 GB sollte hierfür ausreichen.

![VM-Bereitstellung für SMT-Server](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Melden Sie sich bei der bzw. den Einheiten von HANA (große Instanzen) an, verwalten Sie „/etc/hosts“, und überprüfen Sie, ob Sie den virtuellen Azure-Computer erreichen können, auf dem der SMT-Server über das Netzwerk ausgeführt werden soll.

Nachdem diese Überprüfung erfolgreich abgeschlossen wurde, müssen Sie sich bei dem virtuellen Azure-Computer anmelden, auf dem der SMT-Server ausgeführt werden soll. Wenn Sie putty für die VM-Anmeldung verwenden, müssen Sie diese Befehlssequenz in Ihrem Bash-Fenster ausführen:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Starten Sie nach dem Ausführen dieser Befehle Ihre Bash-Instanz neu, um die Einstellungen zu aktivieren. Starten Sie anschließend YAST.

Verbinden Sie Ihren virtuellen Computer („smtserver“) mit der SUSE-Website.

```
smtserver:~ # SUSEConnect -r <registration code> -e s<email address> --url https://scc.suse.com
Registered SLES_SAP 12.2 x86_64
To server: https://scc.suse.com
Using E-Mail: email address
Successfully registered system.
```

Nachdem Sie den virtuellen Computer mit der SUSE-Website verbunden haben, installieren Sie die SMT-Pakete. Verwenden Sie den folgenden putty-Befehl, um die SMT-Pakete zu installieren.

```
smtserver:~ # zypper in smt
Refreshing service 'SUSE_Linux_Enterprise_Server_for_SAP_Applications_12_SP2_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...
```


Sie können auch YaST verwenden, um die SMT-Pakete zu installieren. Navigieren Sie in YAST zu „Software Maintenance“, und suchen Sie nach „smt“. Wählen Sie „smt“ aus, um automatisch zu „yast2-smt“ zu wechseln. Dies ist unten dargestellt.

![SMT in YAST](./media/hana-installation/image5_smt_in_yast.PNG)


Übernehmen Sie die Auswahl für die Installation von smtserver. Navigieren Sie nach Abschluss der Installation zur SMT-Serverkonfiguration, und geben Sie die Anmeldeinformationen für die Organisation aus dem SUSE Customer Center ein, die Sie weiter oben abgerufen haben. Geben Sie außerdem Ihren Azure-VM-Hostnamen als SMT Server-URL ein. In dieser Demonstration war dies „https://smtserver“, wie in den nächsten Grafiken dargestellt.

![Konfiguration des SMT-Servers](./media/hana-installation/image6_configuration_of_smtserver1.png)

Im nächsten Schritt testen Sie, ob die Verbindung mit dem SUSE Customer Center funktioniert. Wie in den folgenden Grafiken zu sehen ist, wurde die Verbindung für die Demonstration erfolgreich hergestellt.

![Testen der Verbindung mit dem SUSE Customer Center](./media/hana-installation/image7_test_connect.png)

Nach dem Start des SMT-Setups müssen Sie ein Datenbankkennwort angeben. Da es sich um eine neue Installation handelt, müssen Sie dieses Kennwort wie in den nächsten Grafiken definieren.

![Definieren des Kennworts für die Datenbank](./media/hana-installation/image8_define_db_passwd.PNG)

Die nächste Interaktion ist erforderlich, wenn ein Zertifikat erstellt wird. Navigieren Sie wie unten gezeigt durch das Dialogfeld, um den Schritt auszuführen.

![Erstellen eines Zertifikats für den SMT-Server](./media/hana-installation/image9_certificate_creation.PNG)

Es kann sein, dass der Schritt „Run synchronization check“ am Ende der Konfiguration einige Minuten dauert. Nach der Installation und Konfiguration des SMT-Servers sollten sich das Verzeichnisrepository unter dem Bereitstellungspunkt „/srv/www/htdocs/“ und einige Unterverzeichnisse unter dem Repository befinden. 

Starten Sie den SMT-Server und die dazugehörigen Dienste mit diesen Befehlen neu.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-of-packages-onto-smt-server"></a>Herunterladen von Paketen auf den SMT-Server

Wählen Sie nach dem Neustart aller Dienste unter SMT Management per Yast die entsprechenden Pakete aus. Die Paketauswahl hängt vom Betriebssystemimage des Servers für HANA (große Instanzen) und nicht vom SLES-Release bzw. der -Version der VM ab, auf der der SMT-Server ausgeführt wird. Unten ist ein Beispiel für den Auswahlbildschirm angegeben.

![Auswählen von Paketen](./media/hana-installation/image10_select_packages.PNG)

Nachdem Sie die Paketauswahl abgeschlossen haben, müssen Sie den ersten Kopiervorgang für die ausgewählten Pakete auf den eingerichteten SMT-Server starten. Dieser Kopiervorgang wird in der Shell ausgelöst, indem Sie wie unten gezeigt den Befehl „smt-mirror“ verwenden.


![Herunterladen von Paketen auf den SMT-Server](./media/hana-installation/image11_download_packages.PNG)

Wie oben dargestellt, sollten die Pakete in die Verzeichnisse kopiert werden, die unter dem Bereitstellungspunkt „/srv/www/htdocs“ erstellt wurden. Dieser Vorgang kann eine Weile dauern. Je nach Anzahl von ausgewählten Paketen kann eine Stunde oder mehr Zeit benötigt werden.
Nachdem dieser Vorgang abgeschlossen ist, führen Sie als Nächstes das Setup für den SMT-Client durch. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Einrichten des SMT-Clients auf Einheiten von HANA (große Instanzen)

Die Clients sind in diesem Fall die Einheiten von HANA (große Instanzen). Beim Setup für den SMT-Server wurde das Skript „clientSetup4SMT.sh“ auf die Azure-VM kopiert. Kopieren Sie dieses Skript auf die Einheit von HANA (große Instanzen), für die Sie eine Verbindung mit Ihrem SMT-Server herstellen möchten. Starten Sie das Skript mit der Option „-h“, und verwenden Sie den Namen Ihres SMT-Servers als Parameter. In diesem Beispiel ist dies „smtserver“.

![Konfigurieren des SMT-Clients](./media/hana-installation/image12_configure_client.PNG)

Es kann zu einem Szenario kommen, bei dem das Laden des Zertifikats vom Server durch den Client erfolgreich ist, für die Registrierung aber ein Fehler auftritt. Dies ist unten dargestellt.

![Fehler bei der Registrierung des Clients](./media/hana-installation/image13_registration_failed.PNG)

Lesen Sie dieses [Dokument des SUSE-Supports](https://www.suse.com/de-de/support/kb/doc/?id=7006024), und führen Sie die hier beschriebenen Schritte aus, falls für die Registrierung ein Fehler auftritt.

> [!IMPORTANT] 
> Als Servername müssen Sie den Namen der VM (hier „smtserver“) ohne den vollqualifizierten Domänennamen angeben. Es funktioniert, wenn Sie nur den VM-Namen angeben. 

Nachdem diese Schritte ausgeführt wurden, müssen Sie den folgenden Befehl auf der Einheit von HANA (große Instanzen) ausführen:

```
SUSEConnect –cleanup
```

> [!Note] 
> Bei unseren Tests mussten wir nach diesem Schritt immer einige Minuten warten. Die sofortige Ausführung von „clientSetup4SMT.sh“ laut den Korrekturmaßnahmen im SUSE-Artikel hat zu Meldungen geführt, dass das Zertifikat noch nicht gültig ist. Nach einer Wartezeit von fünf bis zehn Minuten und der Ausführung von „clientSetup4SMT.sh“ ist die Clientkonfiguration aber normalerweise erfolgreich.

Falls ein Problem auftritt, bei dem Sie gemäß den Schritten im SUSE-Artikel eine Korrektur durchführen müssen, müssen Sie „clientSetup4SMT.sh“ für die Einheit von HANA (große Instanzen) noch einmal neu starten. Der Vorgang sollte jetzt wie unten dargestellt erfolgreich abgeschlossen werden können.

![Clientregistrierung erfolgreich](./media/hana-installation/image14_finish_client_config.PNG)

Mit diesem Schritt haben Sie den SMT-Client für die Einheit von HANA (große Instanzen) konfiguriert, und die Verbindung mit dem auf der Azure-VM installierten SMT-Server kann hergestellt werden. Sie können nun „zypper up“ oder „zypper in“ verwenden, um Betriebssystempatches für HANA (große Instanzen) oder weitere Pakete zu installieren. Hierbei ist klar, dass Sie nur Patches nutzen können, die Sie vorher auf dem SMT-Server heruntergeladen haben.

**Nächste Schritte**
- Lesen Sie [HANA-Installation unter HLI](hana-example-installation.md).











