---
title: Installieren von HANA unter SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Installieren von HANA unter SAP HANA in Azure (große Instanzen)
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
ms.openlocfilehash: 76a7ce99799b85d81aa6e127ebe1e57e2df3e59a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167609"
---
# <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Beispiel für eine SAP HANA-Installation für HANA (große Instanzen)

In diesem Abschnitt wird veranschaulicht, wie Sie SAP HANA auf einer Einheit von HANA (große Instanzen) installieren. Wir verfügen über den folgenden Anfangszustand:

- Sie haben Microsoft alle Daten geliefert, die auf einer Einheit von SAP HANA (große Instanzen) bereitgestellt werden sollen.
- Sie haben von Microsoft die Einheit von SAP HANA (große Instanzen) erhalten.
- Sie haben ein Azure VNet erstellt, das mit Ihrem lokalen Netzwerk verbunden ist.
- Sie haben die ExpressRoute-Verbindung für HANA (große Instanzen) mit demselben Azure VNet hergestellt.
- Sie haben eine Azure-VM installiert, die Sie als Jumpbox für HANA (große Instanzen) verwenden.
- Sie haben sichergestellt, dass Sie eine Verbindung von der Jumpbox zur Einheit von HANA (große Instanzen) und umgekehrt herstellen können.
- Sie haben überprüft, ob alle erforderlichen Pakete und Patches installiert sind.
- Sie haben die SAP-Hinweise und die Dokumentation zur HANA-Installation auf dem von Ihnen verwendeten Betriebssystem gelesen und sich vergewissert, dass das gewünschte HANA-Release für die Betriebssystemversion unterstützt wird.

In den nächsten Sequenzen wird das Herunterladen der HANA-Installationspakete auf die Jumpbox-VM (hier unter einem Windows-Betriebssystem), das Kopieren der Pakete auf die Einheit von HANA (große Instanzen) und die Setup-Sequenz veranschaulicht.

## <a name="download-of-the-sap-hana-installation-bits"></a>Herunterladen der Daten für die SAP HANA-Installation
Da für die Einheiten von HANA (große Instanzen) keine direkte Verbindung mit dem Internet besteht, können Sie die Installationspakete nicht direkt von SAP auf die VM mit HANA (große Instanzen) herunterladen. Sie benötigen die Jumpbox, um das Fehlen der direkten Internetverbindung auszugleichen. Sie laden die Pakete auf die Jumpbox-VM herunter.

Zum Herunterladen der HANA-Installationspakete benötigen Sie einen SAP S-Benutzer oder einen anderen Benutzer, mit dem Sie auf den SAP Marketplace zugreifen können. Durchlaufen Sie nach dem Anmelden die folgende Bildschirmsequenz:

Navigieren Sie zu [SAP Service Marketplace](https://support.sap.com/en/index.html). Klicken Sie auf „Download Software“ > „Installations and Upgrade“ > „By Alphabetical Index“ > „Under H – SAP HANA Platform Edition“ > „SAP HANA Platform Edition 2.0“ > „Installation“, und laden Sie die folgenden Dateien herunter:

![Herunterladen der HANA-Installation](./media/hana-installation/image16_download_hana.PNG)

Für die Demonstration haben wir SAP HANA 2.0-Installationspakete heruntergeladen. Auf der Azure-Jumpbox-VM erweitern Sie die selbstextrahierenden Archive wie unten gezeigt in das Verzeichnis.

![Extrahieren der HANA-Installation](./media/hana-installation/image17_extract_hana.PNG)

Kopieren Sie nach dem Extrahieren der Archive das Verzeichnis, das bei der Extraktion extrahiert wurde (im obigen Fall „51052030“), auf die Einheit von HANA (große Instanzen) im Volume „/hana/shared“ des von Ihnen erstellten Verzeichnisses.

> [!Important]
> Kopieren Sie die Installationspakete NICHT in die Stamm- oder Start-LUN, da der Speicherplatz begrenzt ist und auch von anderen Prozessen genutzt werden muss.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>Installieren von SAP HANA auf der Einheit von HANA (große Instanzen)
Für die Installation von SAP HANA müssen Sie sich als Stammbenutzer (root) anmelden. Nur der Stammbenutzer verfügt über ausreichende Berechtigungen zum Installieren von SAP HANA.
Als Erstes müssen Sie Berechtigungen für das Verzeichnis festlegen, das Sie in „/hana/shared“ kopiert haben. Legen Sie die Berechtigungen wie folgt fest:

```
chmod –R 744 <Installation bits folder>
```

Wenn Sie SAP HANA per grafischem Setup installieren möchten, muss das Paket gtk2 auf der Einheit von HANA (große Instanzen) installiert werden. Überprüfen Sie mit dem folgenden Befehl, ob es installiert ist:

```
rpm –qa | grep gtk2
```

In den weiteren Schritten wird das SAP HANA-Setup mit der grafischen Benutzeroberfläche veranschaulicht. Im nächsten Schritt navigieren Sie zum Installationsverzeichnis und dann in das Unterverzeichnis „HDB_LCM_LINUX_X86_64“. Starten Sie

```
./hdblcmgui 
```
in diesem Verzeichnis. Sie werden nun durch eine Bildschirmsequenz geführt, in der Sie die Daten für die Installation angeben müssen. Im demonstrierten Fall installieren wir den SAP HANA-Datenbankserver und die SAP HANA-Clientkomponenten. Daher wählen wir „SAP HANA Database“ aus, wie unten gezeigt.

![Auswählen von HANA bei der Installation](./media/hana-installation/image18_hana_selection.PNG)

Wählen Sie auf dem nächsten Bildschirm die Option „Install New System“.

![Auswählen der neuen HANA-Installation](./media/hana-installation/image19_select_new.PNG)

Nach diesem Schritt müssen Sie zwischen mehreren weiteren Komponenten wählen, die zusätzlich zum SAP HANA-Datenbankserver installiert werden können.

![Auswählen zusätzlicher HANA-Komponenten](./media/hana-installation/image20_select_components.PNG)

Im Rahmen dieser Dokumentation haben wir den SAP HANA-Client und SAP HANA Studio ausgewählt. Außerdem haben wir eine Instanz für das zentrale Hochskalieren installiert. Wählen Sie im nächsten Bildschirmfenster also „Single-Host System“. 

![Auswählen der Installation für zentrales Hochskalieren](./media/hana-installation/image21_single_host.PNG)

Im nächsten Bildschirmfenster müssen Sie einige Daten angeben.

![Bereitstellen der SAP HANA-SID](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Als HANA-SID (System-ID) müssen Sie die gleiche SID angeben, die Sie für Microsoft bereitgestellt haben, als Sie die Bereitstellung von HANA (große Instanzen) bestellt haben. Wenn Sie eine andere SID wählen, tritt für die Installation ein Fehler auf, da sich für die verschiedenen Volumes Probleme mit der Zugriffsberechtigung ergeben.

Als Installationsverzeichnis verwenden Sie das Verzeichnis „/hana/shared“. Im nächsten Schritt geben Sie die Speicherorte für die HANA-Datendateien und die HANA-Protokolldateien an.


![Angeben des HANA-Protokollspeicherorts](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Für die Daten- und Protokolldateien sollten Sie die Volumes angeben, die bereits Teil der Bereitstellungspunkte waren. Diese enthalten die SID, die Sie im Bildschirmfenster weiter oben ausgewählt haben. Falls die SID nicht mit der oben eingegebenen SID übereinstimmt, können Sie zurückgehen und die SID an den Wert anpassen, der in den Bereitstellungspunkten verwendet wird.

Überprüfen Sie im nächsten Schritt den Hostnamen, und korrigieren Sie ihn bei Bedarf. 

![Überprüfen des Hostnamens](./media/hana-installation/image24_review_host_name.PNG)

Im nächsten Schritt müssen Sie auch Daten abrufen, die Sie für Microsoft bereitgestellt haben, als Sie die Bereitstellung von HANA (große Instanzen) bestellt haben. 

![Angeben von UID und GID für Systembenutzer](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Sie müssen die gleiche Systembenutzer-ID und Benutzergruppen-ID angeben, die Sie gegenüber Microsoft beim Bestellen der Einheitenbereitstellung angegeben haben. Wenn die IDs nicht identisch sind, tritt für die Installation von SAP HANA auf der Einheit von HANA (große Instanzen) ein Fehler auf.

In den nächsten beiden Bildschirmfenstern, die wir in dieser Dokumentation nicht aufführen, müssen Sie das Kennwort für den SYSTEM-Benutzer der SAP HANA-Datenbank und das Kennwort für den Benutzer „sapadm“ angeben. Das zweite Kennwort wird für den SAP-Host-Agent verwendet, der im Rahmen der SAP HANA-Datenbankinstanz installiert wird.

Nach dem Definieren des Kennworts wird ein Bestätigungsfenster angezeigt. Überprüfen Sie alle aufgeführten Daten, und fahren Sie mit der Installation fort. Sie gelangen zu einem Statusfenster, auf dem der Installationsstatus angezeigt wird (siehe unten).

![Überprüfen des Installationsstatus](./media/hana-installation/image27_show_progress.PNG)

Nach Abschluss der Installation sollte etwa Folgendes angezeigt werden:

![Abgeschlossene Installation](./media/hana-installation/image28_install_finished.PNG)

Die SAP HANA-Instanz sollte jetzt ausgeführt werden und betriebsbereit sein. Sie sollten von SAP HANA Studio aus eine Verbindung damit herstellen können. Stellen Sie auch sicher, dass Sie eine Prüfung auf die neuesten Patches von SAP HANA durchführen und diese dann anwenden.


**Nächste Schritte**

- Lesen Sie [Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md).

