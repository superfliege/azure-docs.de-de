---
title: HANA-Sicherung und -Wiederherstellung in SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine HANA-Sicherung und -Wiederherstellung in SAP HANA in Azure (große Instanzen) durchführen.
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/28/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2a72fade57b070ac2ac1aea28cbec92700c3797f
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452546"
---
# <a name="backup-and-restore"></a>Sichern und Wiederherstellen

>[!IMPORTANT]
>Diese Dokumentation ist kein Ersatz für die Verwaltungsdokumentation zu SAP HANA oder für SAP Notes. Es wird erwartet, dass der Leser über solide Kenntnisse in Bezug auf die SAP HANA-Verwaltung und den Betrieb verfügt, vor allem in den Bereichen Sicherung, Wiederherstellung, Hochverfügbarkeit und Notfallwiederherstellung. In dieser Dokumentation werden Screenshots von SAP HANA Studio verwendet. Inhalt, Struktur und grundlegender Aufbau der Bildschirme in den SAP-Verwaltungstools sowie auch die Tools selbst können sich von Release zu Release von SAP HANA ändern.

Es ist wichtig, dass Sie alle Schritte und Verfahren in Ihrer Umgebung und mit Ihren HANA-Versionen und -Releases ausführen. Einige in dieser Dokumentation beschriebene Vorgänge sind für ein besseres allgemeines Verständnis vereinfacht dargestellt und nicht darauf ausgelegt, als detaillierte Schritte für schließlich vorliegende Betriebshandbücher verwendet zu werden. Wenn Sie Betriebshandbücher für Ihre Konfigurationen erstellen möchten, müssen Sie Ihre Verfahren testen und umsetzen und in Bezug auf Ihre Konfigurationen dokumentieren. 

Einer der wichtigsten Aspekte beim Betrieb von Datenbanken ist der Schutz dieser Datenbanken vor Katastrophenfällen. Diese Ereignisse können sowohl durch Naturkatastrophen als auch einfache Benutzerfehler verursacht werden.

Durch das Sichern einer Datenbank mit der Möglichkeit einer Wiederherstellung zu einem beliebigen Zeitpunkt (z.B. vor dem Zeitpunkt des Löschens kritischer Daten) kann ein Status wiederhergestellt werden, der möglichst genau dem Status vor der Störung entspricht.

Für optimale Ergebnisse müssen zwei Arten von Sicherungen erfolgen:

- Datenbanksicherungen: vollständig, inkrementell oder differenziell
- Transaktionsprotokollsicherungen

Zusätzlich zu vollständigen Datenbanksicherungen auf Anwendungsebene können Sie Sicherungen mit Speichermomentaufnahmen erstellen. Speichermomentaufnahmen sind aber kein Ersatz für Transaktionsprotokollsicherungen. Transaktionsprotokollsicherungen werden auch weiterhin für Point-in-Time-Datenbankwiederherstellungen sowie zum Leeren der Protokolle von Transaktionen, für die bereits ein Commit ausgeführt wurde, benötigt. Speichermomentaufnahmen können allerdings die Wiederherstellung durch schnelles Bereitstellen eines Rollforwardimages der Datenbank beschleunigen. 

Es gibt zwei Sicherungs- und Wiederherstellungsoptionen für SAP HANA in Azure (große Instanzen):

- Eigenständig. Nachdem genügend vorhandener Speicherplatz berechnet wurde, führen Sie vollständige Datenbank- und Protokollsicherungen mit einer der folgenden Datenträger-Sicherungsmethoden durch. Sie können entweder direkt auf Volumes sichern, die an die HANA-Einheiten (große Instanz) angefügt sind, oder auf Netzwerkdateifreigaben (Network File Share, NFS), die auf einem virtuellen Azure-Computer (VM) eingerichtet sind. In letzterem Fall richten Kunden eine Linux-VM in Azure ein, fügen Azure Storage an die VM an und geben den Speicher über einen konfigurierten NFS-Server auf dieser VM frei. Wenn Sie die Sicherung mit Volumes durchführen, die direkt an Einheiten von HANA (große Instanzen) angefügt sind, müssen Sie die Sicherungen (nach Einrichtung einer Azure-VM, die NFS-Freigaben auf der Grundlage von Azure Storage exportiert) in ein Azure Storage-Konto kopieren. Außerdem können Sie einen Azure-Sicherungstresor oder Azure Cold Storage verwenden. 

   Alternativ können die Sicherungen nach dem Kopieren in ein Azure Storage-Konto mithilfe eines Drittanbieter-Datenschutztools gespeichert werden. Die eigenständige Sicherungsoption kann auch bei Daten erforderlich sein, die zu Konformitäts- oder Auditzwecken über einen längeren Zeitraum gespeichert werden müssen. In allen Fällen werden die Sicherungen in NFS-Freigaben kopiert, die durch eine VM und Azure Storage dargestellt werden.

- Funktionalität für Sicherungen und Wiederherstellungen der Infrastruktur. Sie können auch die Sicherungs- und Wiederherstellungsfunktionalität verwenden, mit der die zugrunde liegende Infrastruktur von SAP HANA in Azure (große Instanzen) bereitgestellt wird. Diese Option sorgt für die nötigen Sicherungen und ermöglicht schnelle Wiederherstellungen. Im restlichen Teil dieses Abschnitts werden die Sicherungs- und Wiederherstellungsfunktionen behandelt, die von HANA (große Instanzen) bereitgestellt werden. In diesem Abschnitt wird auch die Beziehung der Sicherung und Wiederherstellung zur Notfallwiederherstellungsfunktionalität von HANA (große Instanzen) behandelt.

>   [!NOTE]
>   Die Momentaufnahmetechnologie, die von der zugrunde liegenden Infrastruktur von HANA (große Instanzen) verwendet wird, ist von SAP HANA-Momentaufnahmen abhängig. An dieser Stelle können SAP HANA-Momentaufnahmen nicht in Verbindung mit mehreren Mandanten von mehrinstanzenfähigen SAP HANA-Datenbankcontainern verwendet werden. Wenn nur ein Mandant bereitgestellt wird, funktionieren SAP HANA-Momentaufnahmen, und diese Methode kann verwendet werden.

## <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Verwenden von Speicher-Snapshots von SAP HANA in Azure (große Instanzen)

Die zugrunde liegende Speicherinfrastruktur von SAP HANA in Azure (große Instanzen) unterstützt Speichermomentaufnahmen von Volumes. Sicherung und Wiederherstellung von Volumes werden unterstützt, dabei ist jedoch Folgendes zu beachten:

- Anstelle von vollständigen Datenbanksicherungen werden in regelmäßigen Abständen Speichervolume-Momentaufnahmen erstellt.
- Beim Auslösen einer Momentaufnahme für die Volumes „/hana/data“ und „/hana/shared“ (einschließlich „/usr/sap“) initiiert die Technologie für Momentaufnahmen vor dem Ausführen der Speichermomentaufnahme eine SAP HANA-Momentaufnahme. Diese SAP HANA-Momentaufnahme ist der Einrichtungspunkt für nachfolgende Wiederherstellungen von Protokollen, nachdem die Speichermomentaufnahme wiederhergestellt wurde. Für erfolgreiche HANA-Momentaufnahmen benötigen Sie eine aktive HANA-Instanz.  Im HSR-Szenario werden Speichermomentaufnahmen auf aktuellen sekundären Knoten, auf denen HANA Momentaufnahmen nicht ausgeführt werden können, nicht unterstützt.
- Nach der erfolgreichen Erstellung der Speichermomentaufnahme wird die SAP HANA-Momentaufnahme gelöscht.
- Transaktionsprotokollsicherungen werden in regelmäßigen Abständen erstellt und auf dem Volume „/hana/logbackups“ oder in Azure gespeichert. Sie können für das Volume „/hana/logbackups“ mit den Transaktionsprotokollsicherungen eine separate Momentaufnahmenerstellung auslösen. In diesem Fall müssen Sie keine HANA-Momentaufnahme erstellen.
- Wenn Sie eine Datenbank zu einem bestimmten Zeitpunkt wiederherstellen müssen, fordern Sie beim Microsoft Azure-Support (bei einem Produktionsausfall) oder beim Dienstverwaltungsteam für SAP HANA in Azure die Wiederherstellung einer bestimmten Speichermomentaufnahme an. Ein Beispiel ist eine geplante Wiederherstellung eines Sandkastensystems in seinen ursprünglichen Zustand.
- Die in der Speichermomentaufnahme enthaltene SAP HANA-Momentaufnahme ist ein Ausgangspunkt für die Anwendung von Transaktionsprotokollsicherungen, die nach Erstellung der Speichermomentaufnahme erstellt und gespeichert wurden.
- Diese Transaktionsprotokollsicherungen werden erstellt, um eine Point-in-Time-Wiederherstellung der Datenbank zu ermöglichen.

Sie können Speichermomentaufnahmen für drei Volumeklassen durchführen:

- Eine kombinierte Momentaufnahme für „/hana/data“ und „/hana/shared“ (einschließlich „/usr/sap“). Diese Momentaufnahme erfordert die Erstellung einer SAP HANA-Momentaufnahme als Vorbereitung für die Speichermomentaufnahme. Die SAP HANA-Momentaufnahme stellt sicher, dass die Datenbank aus Speichersicht in einem konsistenten Zustand verbleibt und dass für die Wiederherstellung ein Punkt vorhanden ist, an dem sie beginnen kann.
- Eine separate Momentaufnahme für „/hana/logbackups“
- Eine Betriebssystempartition.

Rufen Sie die neuesten Momentaufnahmeskripts und -dokumente bei [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) ab. Beim Herunterladen des Momentaufnahme-Skriptpakets von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) erhalten Sie außerdem die PDF-Dokumentation für die Skripte als Teil des Skriptpakets. Jedes Skriptpaket hat seine eigene PDF-Dokumentation.

## <a name="storage-snapshot-considerations"></a>Aspekte von Speichermomentaufnahmen

>[!NOTE]
>Speichermomentaufnahmen beanspruchen Speicherplatz, der den Einheiten von HANA (große Instanzen) zugeteilt wurde. Bei der Planung von Speichermomentaufnahmen und der Festlegung der Anzahl von Speichermomentaufnahmen sollten die folgenden Aspekte berücksichtigt werden. 

Zur besonderen Funktionsweise von Speichermomentaufnahmen von SAP HANA in Azure (große Instanzen) gehört Folgendes:

- Eine individuelle Speichermomentaufnahme (zum Zeitpunkt der Erstellung) beansprucht nur wenig Speicherplatz.
- Wenn sich Dateninhalte und der Inhalt in SAP HANA-Datendateien auf dem Speichervolume ändern, muss die Momentaufnahme den ursprünglichen Blockinhalt speichern und die Daten ändern.
- Die Speichermomentaufnahme vergrößert sich infolgedessen. Je länger die Momentaufnahme vorhanden ist, desto größer wird die Speichermomentaufnahme.
- Je mehr Änderungen am SAP HANA-Datenbankvolume während der Lebensdauer einer Speichermomentaufnahme erfolgen, desto mehr Speicherplatz belegt die Speichermomentaufnahme.

SAP HANA in Azure (große Instanzen) bietet standardmäßig feste Volumegrößen für die SAP HANA-Daten- und Protokollvolumes. Die Erstellung von Momentaufnahmen dieser Volumes verringert Ihren Volumespeicherplatz. Sie müssen festlegen, wann geplante Speichermomentaufnahmen erstellt werden sollen. Darüber hinaus müssen Sie auch den Speicherplatzverbrauch der Speichervolumes überwachen und die Anzahl von Momentaufnahmen, die Sie speichern, verwalten. Sie können Speichermomentaufnahmen deaktivieren, wenn Sie Unmengen von Daten importieren oder andere wichtige Änderungen an der HANA-Datenbank vornehmen. 


Die folgenden Abschnitte enthalten Informationen zum Erstellen dieser Momentaufnahmen sowie allgemeine Empfehlungen:

- Obwohl die Hardware 255 Snapshots pro Volume unterstützen kann, ist es ratsam, unter diesem Wert zu bleiben. Die Empfehlung sind höchstens 250.
- Überwachen und verfolgen Sie den freien Speicherplatz, bevor Sie Speichermomentaufnahmen erstellen.
- Verringern Sie die Anzahl von Speichermomentaufnahmen basierend auf dem freien Speicherplatz. Sie können die Anzahl der gespeicherten Momentaufnahmen verringern oder die Volumes erweitern. Sie können zusätzlichen Speicher in 1-TB-Schritten anfordern.
- Deaktivieren Sie Speichermomentaufnahmen für das Volume „/hana/data“ bei Aktivitäten wie dem Verschieben von Daten in SAP HANA mit SAP-Plattformmigrationstools (R3load) oder dem Wiederherstellen von SAP HANA-Datenbanken aus Sicherungen. 
- Bei größeren Neuorganisationen von SAP HANA-Tabellen sollten Speichermomentaufnahmen nach Möglichkeit vermieden werden.
- Speichermomentaufnahmen sind eine Voraussetzung, um von den Notfallwiederherstellungsfunktionen von SAP HANA in Azure (große Instanzen) profitieren zu können.

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Voraussetzungen für die Nutzung von Self-Service-Speichermomentaufnahmen

Um zu gewährleisten, dass das Momentaufnahmeskript ausgeführt werden kann, stellen Sie sicher, dass Perl im Linux-Betriebssystem auf dem Server mit HANA (große Instanzen) installiert ist. Perl ist auf Ihrer HANA-Einheit (große Instanzen) vorinstalliert. Verwenden Sie folgenden Befehl, um die Perl-Version zu überprüfen:

`perl -v`

![Der öffentliche Schlüssel wird durch Ausführen dieses Befehls kopiert.](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Einrichten von Speichermomentaufnahmen

Führen Sie diese Schritte aus, um Speichermomentaufnahmen mit HANA (große Instanzen) einzurichten:
1. Stellen Sie sicher, dass Perl im Linux-Betriebssystem auf dem Server mit HANA (große Instanzen) installiert ist.
1. Fügen Sie „/etc/ssh/ssh\_config“ die Zeile _MACs hmac-sha1_ hinzu.
1. Erstellen Sie ein SAP HANA-Sicherungsbenutzerkonto auf dem Masterknoten für jede SAP HANA-Instanz, die Sie ausführen (falls zutreffend).
1. Installieren Sie den SAP HANA HDB-Client auf allen Servern mit SAP HANA (große Instanzen).
1. Erstellen Sie auf dem ersten Server mit SAP HANA (große Instanzen) in jeder Region einen öffentlichen Schlüssel, um auf die zugrunde liegende Speicherinfrastruktur zuzugreifen, die die Erstellung von Momentaufnahmen steuert.
1. Kopieren Sie die Skripts und die Konfigurationsdatei von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) im Speicherort **hdbsql** der SAP HANA-Installation.
1. Ändern Sie bei Bedarf die Datei *HANABackupDetails.txt* gemäß den jeweiligen Kundenvorgaben.

Rufen Sie die neuesten Momentaufnahmeskripts und -dokumente bei [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) ab. Beim Herunterladen des Momentaufnahme-Skriptpakets von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) erhalten Sie außerdem die PDF-Dokumentation für die Skripte als Teil des Skriptpakets. Jedes Skriptpaket hat seine eigene PDF-Dokumentation.

### <a name="consideration-for-mcod-scenarios"></a>Überlegung für MCOD-Szenarien
Wenn Sie ein Betriebssystem mit einem [MCOD-Szenario](https://launchpad.support.sap.com/#/notes/1681092) mit mehreren SAP HANA-Instanzen auf einer HANA-Einheit (große Instanzen) ausführen, verfügen Sie über separat bereitgestellte Speichervolumes für jede einzelne SAP HANA-Instanz. In der aktuellen Version der Automatisierung für Self-Service-Momentaufnahmen können nicht für jede System-ID (SID) der HANA-Instanz separate Momentaufnahmen initiiert werden. Mit den Funktionen werden die registrierten SAP HANA-Instanzen des Servers in der Konfigurationsdatei überprüft (siehe weiter unten in diesem Artikel), und es wird eine gleichzeitige Momentaufnahme der Volumes aller für die Einheit registrierten Instanzen ausgeführt.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Schritt 1: Installieren des SAP HANA HDB-Clients

Das für SAP HANA in Azure (große Instanzen) installierte Linux-Betriebssystem enthält die Ordner und Skripts, die benötigt werden, um die SAP HANA-Speichermomentaufnahmen für die Sicherung und Notfallwiederherstellung auszuführen. Überprüfen Sie, ob auf [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) neuere Versionen zur Verfügung stehen. Die neueste Skriptversion ist 3.x. Andere Skripts weisen möglicherweise abweichende Nebenversionen innerhalb derselben Hauptversion auf.

>[!IMPORTANT]
>Beachten Sie, dass sich beim Wechsel von Version 2.1 zu Version 3.x der Skripts die Struktur der Konfigurationsdatei und ein Teil der Syntax geändert haben. Weitere Informationen finden Sie in den Erläuterungen der entsprechenden Abschnitte. 

Sie müssen bei der Installation von SAP HANA den SAP HANA HDB-Client für die Einheiten von HANA (große Instanzen) installieren.

### <a name="step-2-change-the-etcsshsshconfig"></a>Schritt 2: Ändern von „/etc/ssh/ssh\_config“

Ändern Sie „/etc/ssh/ssh`/etc/ssh/ssh_config`config“ wie folgt durch Hinzufügen der Zeile _MACs hmac-sha1_:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Schritt 3: Erstellen eines öffentlichen Schlüssels

Um den Zugriff auf die Speichermomentaufnahme-Schnittstellen Ihres HANA-Mandanten (große Instanzen) zu ermöglichen, müssen Sie ein Anmeldeverfahren einrichten, das auf einem öffentlichen Schlüssel basiert. Erstellen Sie in Ihrem Mandanten auf dem ersten Server mit SAP HANA in Azure (große Instanzen) einen öffentlichen Schlüssel, der für den Zugriff auf die Speicherinfrastruktur verwendet wird. Der öffentliche Schlüssel sorgt dafür, dass für die Anmeldung bei den Speichermomentaufnahme-Schnittstellen kein Kennwort erforderlich ist. Die Erstellung eines öffentlichen Schlüssels bedeutet auch, dass Sie keine Kennwortanmeldeinformationen verwalten müssen. Führen Sie unter Linux auf dem Server mit SAP HANA (große Instanzen) den folgenden Befehl aus, um den öffentlichen Schlüssel zu generieren:
```
  ssh-keygen –t dsa –b 1024
```
Der neue Speicherort ist **_/root/.ssh/id\_dsa.pub**. Geben Sie kein Kennwort ein, da dies sonst bei jeder Anmeldung erforderlich ist. Drücken Sie stattdessen zweimal die **EINGABETASTE**, um die Kennworteingabeaufforderung bei der Anmeldung zu entfernen.

Vergewissern Sie sich, dass der öffentliche Schlüssel wie erwartet korrigiert wurde, indem Sie zu den Ordnern unter **/root/.ssh/** navigieren und dann den Befehl `ls` ausführen. Wenn der Schlüssel vorhanden ist, können Sie ihn mit dem folgenden Befehl kopieren:

![Der öffentliche Schlüssel wird durch Ausführen dieses Befehls kopiert.](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Wenden Sie sich als Nächstes an das Dienstverwaltungsteam für SAP HANA in Azure, und geben Sie den öffentlichen Schlüssel an. Der Servicemitarbeiter verwendet den öffentlichen Schlüssel für die Registrierung in der zugrunde liegenden Speicherinfrastruktur, die für Ihren HANA-Mandanten (große Instanzen) eingerichtet ist.

### <a name="step-4-create-an-sap-hana-user-account"></a>Schritt 4: Erstellen eines SAP HANA-Benutzerkontos

Um die Erstellung von SAP HANA-Momentaufnahmen zu initiieren, müssen Sie in SAP HANA ein Benutzerkonto erstellen, das die Speichermomentaufnahmeskripts verwenden können. Erstellen Sie hierzu in SAP HANA Studio ein SAP HANA-Benutzerkonto. Der Benutzer muss unter der SYSTEMDB erstellt werden, NICHT unter der SID-Datenbank für MDC. In einer Umgebung mit einem einzelnen Container wird der Benutzer unter der Mandantendatenbank eingerichtet. Dieses Konto benötigt die folgenden Berechtigungen: **Backup Admin** und **Catalog Read**. In diesem Beispiel lautet der Benutzername **SCADMIN**. Beim Namen des in HANA Studio erstellten Benutzerkontos wird die Groß-/Kleinschreibung beachtet. Stellen Sie sicher, dass für die Aufforderung des Benutzers zum Ändern des Kennworts bei der nächsten Anmeldung **Nein** ausgewählt ist.

![Erstellen eines Benutzers in HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

Bei Verwendung von MCOD-Bereitstellungen mit mehreren SAP HANA-Instanzen in einer Einheit müssen Sie diesen Schritt für jede SAP HANA-Instanz wiederholen.

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Schritt 5: Autorisieren des SAP HANA-Benutzerkontos

In diesem Schritt autorisieren Sie das von Ihnen erstellte SAP HANA-Benutzerkonto, damit die Skripts zur Laufzeit keine Kennwörter übermitteln müssen. Mit dem SAP HANA-Befehl `hdbuserstore` können Sie einen SAP HANA-Benutzerschlüssel erstellen, der auf mindestens einem SAP HANA-Knoten gespeichert wird. Der Benutzerschlüssel ermöglicht dem Benutzer den Zugriff auf SAP HANA ohne Kennwortverwaltung im Rahmen des Skriptprozesses. Der Skriptprozess wird weiter unten in diesem Artikel erläutert.

>[!IMPORTANT]
>Führen Sie den folgenden Befehl als der Benutzer aus, mit dem die Skripts ausgeführt werden sollen. Andernfalls funktioniert das Skript nicht richtig.

Geben Sie den Befehl `hdbuserstore` wie folgt ein:

**Für HANA-Setup ohne MDC**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**Für HANA-Setup mit MDC**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

Im folgenden Beispiel lautet der Benutzer **SCADMIN01**, der Hostname **lhanad01** und die Instanznummer **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Wenn Sie eine HANA-MCOD-Bereitstellung mit mehreren SAP HANA-Instanzen in einer Einheit verwenden, müssen Sie den Schritt für jede SAP HANA-Instanz und den zugeordneten Sicherungsbenutzer in der Einheit wiederholen.

Bei einer SAP HANA-Konfiguration für horizontales Hochskalieren müssen sämtliche Skripts über einen einzelnen Server verwaltet werden. In diesem Beispiel muss der SAP HANA-Schlüssel **SCADMIN01** für jeden Host so angepasst werden, dass angezeigt wird, welcher Host zum Schlüssel passt. Passen Sie das SAP HANA-Sicherungskonto mit der Instanznummer der HANA-Datenbank an. Der Schlüssel benötigt Administratorrechte auf dem Host, dem er zugewiesen ist. Der Sicherungsbenutzer für Konfigurationen für horizontales Hochskalieren benötigt Zugriffsrechte für alle SAP HANA-Instanzen. Wenn die drei Knoten für horizontales Hochskalieren **lhanad01**, **lhanad02** und **lhanad03** heißen, ergibt sich folgende Befehlsfolge:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Schritt 6: Abrufen der Momentaufnahmeskripts, Konfigurieren der Momentaufnahmen und Testen der Konfiguration und Konnektivität

Laden Sie von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) die neueste Version der Skripts herunter. Kopieren Sie die heruntergeladenen Skripts und die Textdatei in das Arbeitsverzeichnis für **hdbsql**. Bei aktuellen HANA-Installationen hat dieses Verzeichnis das folgende Format: /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl
azure_hana_dr_failover.pl
azure_hana_test_dr_failover.pl 
HANABackupCustomerDetails.txt 
``` 

Bei Verwendung von Perl-Skripts: 

- Ändern Sie niemals die Skripts, sofern Sie nicht von Microsoft Operations dazu angewiesen werden.
- Wenn Sie aufgefordert werden, das Skript oder eine Parameterdatei zu ändern, sollten Sie immer einen Linux-Text-Editor wie „vi“ und keinen Windows-Editor (z.B. Editor) verwenden. Bei Verwendung eines Windows-Editors kann das Dateiformat beschädigt werden.
- Verwenden Sie immer die neuesten Skripts. Sie können die neueste Version von GitHub herunterladen.
- Verwenden Sie dieselbe Version der Skripts für alle Aufgaben.
- Testen Sie die Skripts, und machen Sie sich mit den erforderlichen Parametern und der Ausgabe der Skripts vertraut, bevor Sie sie direkt im Produktionssystem verwenden.
- Ändern Sie nicht den Namen des Bereitstellungspunkts des von Microsoft Operations bereitgestellten Servers. In diesen Skripts wird für eine erfolgreiche Ausführung davon ausgegangen, dass diese Standardbereitstellungspunkte verfügbar sind.


Die einzelnen Skripts und Dateien haben folgende Zwecke:

- **azure\_hana\_backup.pl**: Dieses Skript wird mit dem Linux-Cron-Hilfsprogramm für die Zeitplanung geplant, um Speichermomentaufnahmen entweder für HANA-Daten und freigegebene Volumes, das Volume „/hana/logbackups“ oder das Betriebssystem auszuführen.
- **azure\_hana\_replication\_status.pl**: Mit diesem Skript werden grundlegende Replikationsstatusdetails des Produktionsstandorts für den Notfallwiederherstellungsstandort bereitgestellt. Das Skript dient zur Überwachung, um sicherzustellen, dass die Replikation stattfindet, und um die Größe der replizierten Elemente anzuzeigen. Darüber hinaus liefert es Anweisungen für den Fall, dass eine Replikation zu lange dauert oder der Link ausgefallen ist.
- **azure\_hana\_snapshot\_details.pl**: Dieses Skript liefert eine Liste mit grundlegenden Details zu allen Momentaufnahmen pro Volume, die in Ihrer Umgebung vorhanden sind. Es kann auf dem primären Server oder in einer Servereinheit am Standort für die Notfallwiederherstellung ausgeführt werden. Das Skript enthält die folgenden Informationen, die nach den einzelnen Volumes mit Momentaufnahmen aufgeschlüsselt sind:
   * Größe der gesamten Momentaufnahmen in einem Volume
   * Jede Momentaufnahme in diesem Volume enthält die folgenden Details: 
      - Name der Momentaufnahme 
      - Erstellungszeitpunkt 
      - Größe der Momentaufnahme
      - Häufigkeit der Momentaufnahme
      - HANA-Sicherungs-ID im Zusammenhang mit dieser Momentaufnahme, sofern relevant
- **azure\_hana\_snapshot\_delete.pl**: Dieses Skript löscht eine Speichermomentaufnahme oder eine Gruppe von Momentaufnahmen. Sie können die SAP HANA-Sicherungs-ID in HANA Studio oder den Namen der Speichermomentaufnahme verwenden. Die Sicherungs-ID ist derzeit nur mit den Momentaufnahmen verknüpft, die für die HANA-Daten-/Protokoll-/Freigabevolumes erstellt werden. Ansonsten gilt: Wenn die Momentaufnahme-ID eingegeben wird, werden alle Momentaufnahmen gesucht, die der eingegebenen Momentaufnahme-ID entsprechen.  
- **testHANAConnection.pl**: Dieses Skript testet die Verbindung mit der SAP HANA-Instanz und wird zum Einrichten der Speichermomentaufnahmen benötigt.
- **testStorageSnapshotConnection.pl**: Dieses Skript hat zwei Aufgaben. Erstens stellt es sicher, dass die Einheit von HANA (große Instanzen), die die Skripts ausführt, Zugriff auf den zugewiesenen virtuellen Speichercomputer und auf die Speichermomentaufnahmen-Schnittstelle Ihrer großen HANA-Instanzen hat. Zum anderen erstellt es eine temporäre Momentaufnahme der HANA-Instanz, die Sie testen. Dieses Skript muss für jede HANA-Instanz auf einem Server ausgeführt werden, um sicherzustellen, dass die Sicherungsskripts wie erwartet funktionieren.
- **removeTestStorageSnapshot.pl**: Dieses Skript löscht die Testmomentaufnahme, die mit dem Skript **testStorageSnapshotConnection.pl** erstellt wurde.
- **azure\_hana\_dr\_failover.pl:** Mit diesem Skript wird ein Notfallwiederherstellungs-Failover in eine andere Region initiiert. Das Skript muss auf der Einheit von HANA (große Instanzen) in der Region für die Notfallwiederherstellung oder auf der Einheit, die als Ziel für das Failover dienen soll, ausgeführt werden. Dieses Skript beendet die Speicherreplikation von der primären Seite auf die sekundäre Seite, stellt die letzte Momentaufnahme auf den Notfallwiederherstellungsvolumes wieder her und stellt die Bereitstellungspunkte für die Notfallwiederherstellungsvolumes bereit.
- **azure\_hana\_test\_dr\_failover.pl:** Mit diesem Skript wird ein Testfailover auf den Notfallwiederherstellungs-Standort durchgeführt. Im Gegensatz zum Skript „azure_hana_dr_failover.pl“ wird bei dieser Ausführung die Speicherreplikation vom primären zum sekundären Replikat nicht unterbrochen. Stattdessen werden Klone der replizierten Speichervolumes auf der Notfallwiederherstellungsseite erstellt, und die Bereitstellungspunkte der geklonten Volumes werden bereitgestellt. 
- **HANABackupCustomerDetails.txt**: Bei dieser Datei handelt es sich um eine bearbeitbare Konfigurationsdatei, die Sie an Ihre SAP HANA-Konfiguration anpassen müssen. Die Datei *HANABackupCustomerDetails.txt* ist die Steuerungs- und Konfigurationsdatei für das Skript, das die Speichermomentaufnahmen erstellt. Passen Sie die Datei für Ihre Zwecke und Einrichtung an. Die Angaben für **Storage Backup Name** (Name der Speichersicherung) und **Storage IP Address** (IP-Adresse des Speichers) erhalten Sie vom Dienstverwaltungsteam für SAP HANA in Azure, wenn Ihre Instanzen bereitgestellt werden. Reihenfolge, Sortierung und Abstand der Variablen in dieser Datei dürfen nicht verändert werden. Andernfalls werden die Skripts nicht richtig ausgeführt. Darüber hinaus erhalten Sie vom Dienstverwaltungsteam für SAP HANA in Azure die IP-Adresse des Knotens für zentrales Hochskalieren oder die IP-Adresse des Masterknotens (bei horizontalem Hochskalieren). Zudem ist Ihnen auch die Anzahl von HANA-Instanzen bekannt, die Sie während der Installation von SAP HANA erhalten. Nun müssen Sie der Konfigurationsdatei einen Sicherungsnamen hinzufügen.

Bei einer Bereitstellung für zentrales oder horizontales Hochskalieren sieht die Konfigurationsdatei nach dem Angeben des Servernamens der HANA-Einheit für große Instanzen und der IP-Adresse des Servers wie im folgenden Beispiel aus. Füllen Sie alle erforderlichen Felder für jede SAP HANA-SID aus, die Sie sichern oder wiederherstellen möchten.

Sie können auch Zeilen von Instanzen, die für eine bestimmte Zeit nicht gesichert werden sollen, auskommentieren, indem Sie einem erforderlichen Feld ein „#“ voranstellen. Außerdem müssen Sie nicht alle SAP HANA-Instanzen eingeben, die sich auf einem Server befinden, wenn die entsprechenden Instanzen nicht gesichert oder wiederhergestellt werden müssen. Das Format muss stets für alle Felder beibehalten werden, da ansonsten alle Skripts eine Fehlermeldung ausgeben und beendet werden. Sie können die erforderlichen zusätzlichen Datenzeilen für nicht verwendete SID-Informationen nach der letzten verwendeten SAP HANA-Instanz löschen. Alle Zeilen müssen entweder ausgefüllt, auskommentiert oder gelöscht werden.

>[!IMPORTANT]
>Die Struktur der Datei wurde beim Wechsel von Version 2.1 zu Version 3.x geändert. Wenn Sie die Skripts der Version 3.x verwenden möchten, müssen Sie die Struktur der Konfigurationsdatei anpassen. 


```
HANA Server Name: testing01
HANA Server IP Address: 172.18.18.50
```

Für jede auf der HANA-Einheit für große Instanzen konfigurierte Instanz oder für die Konfiguration mit horizontaler Skalierung müssen Sie die Daten wie folgt definieren:

    
```
######***SID #1 Information***#####
SID1: h01
###Provided by Microsoft Operations###
SID1 Storage Backup Name: clt1h01backup
SID1 Storage IP Address: 172.18.18.11
######     Customer Provided    ######
SID1 HANA instance number: 00
SID1 HANA HDBuserstore Name: SCADMINH01
```
Wiederholen Sie diese Konfigurationsschritte für die horizontale Skalierung und die HANA-Systemreplikation auf jedem einzelnen Knoten. Durch diese Maßnahme wird sichergestellt, dass bei Ausfällen die Sicherungen und die letztliche Speicherreplikation weiterhin funktionieren.   

Nachdem Sie alle Konfigurationsdaten in die Datei *HANABackupCustomerDetails.txt* eingegeben haben, sollten Sie überprüfen, ob die Konfigurationen für die HANA-Instanzdaten korrekt sind. Verwenden Sie das Skript `testHANAConnection.pl`. Hierbei spielt es keine Rolle, ob es sich um eine SAP HANA-Konfiguration für zentrales oder horizontales Hochskalieren handelt.

```
testHANAConnection.pl
```

Bei einer SAP HANA-Konfiguration für horizontales Hochskalieren benötigt die HANA-Masterinstanz Zugriff auf alle erforderlichen HANA-Server und -Instanzen. Für das Testskript sind zwar keine Parameter verfügbar, aber Sie müssen Ihre Daten in die Konfigurationsdatei *HANABackupCustomerDetails.txt* eingeben, damit das Skript richtig ausgeführt wird. Nur die Fehlercodes des Shellbefehls werden zurückgegeben. Daher ist es für das Skript nicht möglich, jede Instanz auf Fehler zu prüfen. Mit dem Skript werden trotzdem einige hilfreiche Kommentare bereitgestellt, die Sie überprüfen können.

Geben Sie den folgenden Befehl ein, um das Skript auszuführen:
```
 ./testHANAConnection.pl
```
Wenn das Skript den Status der HANA-Instanz erfolgreich abruft, wird eine Nachricht mit dem Hinweis angezeigt, dass die HANA-Verbindung hergestellt wurde.


Im nächsten Schritt wird anhand der Daten, die Sie in der Konfigurationsdatei *HANABackupCustomerDetails.txt* angegeben haben, die Konnektivität mit dem Speicher überprüft und dann eine Testmomentaufnahme ausgeführt. Dieser Test muss vor der Ausführung des Skripts `azure_hana_backup.pl` ausgeführt werden. Falls ein Volume keine Momentaufnahmen enthält, kann nicht bestimmt werden, ob das Volume leer ist oder ob ein SSH-Fehler vorliegt und keine Momentaufnahmedetails abgerufen werden können. Aus diesem Grund werden mit dem Skript zwei Schritte ausgeführt:

- Es überprüft, ob die Skripts auf den virtuellen Speichercomputer des Mandanten und auf die Schnittstellen zugreifen können, um Momentaufnahmen auszuführen.
- Anhand der HANA-Instanz wird für jedes Volume ein Test, ein Dummy oder eine Momentaufnahme erstellt.

Aus diesem Grund ist die HANA-Instanz als ein Argument enthalten. Sollte die Ausführung nicht erfolgreich sein, kann keine Fehlerüberprüfung für die Speicherverbindung durchgeführt werden. Auch wenn keine Fehlerüberprüfung durchgeführt wird, bietet das Skript hilfreiche Hinweise.

1. Führen Sie die Befehlsfolge zum Ausführen dieser Tests aus:

   ```
   ssh <StorageUserName>@<StorageIP>
   ```

   Bei der Übergabe der HANA-Einheit für große Instanzen wurden Ihnen sowohl der Speicherbenutzername als auch die Speicher-IP-Adresse bekannt gegeben.

1. Führen Sie das Testskript aus:
   ```
    ./testStorageSnapshotConnection.pl
   ```

Das Skript versucht, sich mit dem öffentlichen Schlüssel aus den vorherigen Einrichtungsschritten und den konfigurierten Daten aus der Datei *HANABackupCustomerDetails.txt* beim Speicher anzumelden. Im Falle einer erfolgreichen Anmeldung wird Folgendes angezeigt:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Sollten beim Herstellen der Verbindung mit der Speicherkonsole Probleme auftreten, sieht die Ausgabe wie folgt aus:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Nach einer erfolgreichen Anmeldung bei den Schnittstellen der virtuellen Speichercomputer fährt das Skript mit Phase 2 fort und erstellt eine Testmomentaufnahme. Die folgende Ausgabe zeigt eine SAP HANA-Konfiguration mit drei Knoten für horizontales Hochskalieren:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Wenn die Testmomentaufnahme erfolgreich mit dem Skript erstellt wurde, können Sie mit dem Konfigurieren der eigentlichen Speichermomentaufnahmen fortfahren. Falls die Anmeldung nicht erfolgreich ist, sollten Sie die Probleme untersuchen, bevor Sie fortfahren. Bewahren Sie die Testmomentaufnahme, bis die ersten echten Momentaufnahmen fertig gestellt sind.


### <a name="step-7-perform-snapshots"></a>Schritt 7: Erstellen von Momentaufnahmen

Nach Abschluss der Vorbereitungsschritte können Sie damit beginnen, die eigentlichen Speichermomentaufnahmen zu konfigurieren. Das zu planende Skript kann für SAP HANA-Konfigurationen mit zentralem und horizontalem Hochskalieren verwendet werden. Um das Sicherungsskript regelmäßig normal auszuführen, planen Sie es mit dem Cron-Hilfsprogramm. 

Sie können drei Typen von Momentaufnahmesicherungen erstellen:
- **HANA**: Eine kombinierte Momentaufnahmesicherung, bei der die Volumes mit „/hana/data“ und „/hana/shared“ (einschließlich „/usr/sap“) durch die koordinierte Momentaufnahme abgedeckt sind. Auf der Grundlage dieser Momentaufnahme können einzelne Dateien wiederhergestellt werden.
- **Protokolle**: Eine Momentaufnahmesicherung des Volumes „/hana/logbackups“. Zur Erstellung dieser Speichermomentaufnahme wird keine HANA-Momentaufnahme ausgelöst. Dieses Speichervolume soll die SAP HANA-Transaktionsprotokollsicherungen enthalten. Diese werden häufiger ausgeführt, um die Protokollvergrößerung einzuschränken und potenzielle Datenverluste zu verhindern. Auf der Grundlage dieser Momentaufnahme können einzelne Dateien wiederhergestellt werden. Reduzieren Sie die Häufigkeit nicht auf einen Wert von weniger als drei Minuten.
- **Start**: Eine Momentaufnahme des Volumes mit der Start-LUN (Logical Unit Number, logische Gerätenummer) der großen HANA-Instanz. Diese Momentaufnahmesicherung steht nur für Typ I-SKUs großer HANA-Instanzen zur Verfügung. Auf der Grundlage der Momentaufnahme des Volumes mit der Start-LUN können keine einzelnen Dateien wiederhergestellt werden.


>[!NOTE]
> Die Aufrufsyntax für diese drei Typen von Momentaufnahmen wurde beim Wechsel zu Version 3.x der Skripts, die die MCOD-Bereitstellung unterstützen, geändert. Es besteht keine Notwendigkeit mehr, die HANA-SID einer Instanz anzugeben. Sie müssen sicherstellen, dass die SAP HANA-Instanzen einer Einheit in der Konfigurationsdatei *HANABackupCustomerDetails.txt* konfiguriert werden.

>[!NOTE]
> Wenn Sie das Skript zum ersten Mal ausführen, werden in der Multi-SID-Umgebung unter Umständen einige unerwartete Fehler angezeigt. Das Problem wird behoben, wenn Sie das Skript erneut ausführen.



Die neue Aufrufsyntax zum Ausführen von Speichermomentaufnahmen mit dem Skript *azure_hana_backup.pl* sieht wie folgt aus:

```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot <HANA Large Instance Type> <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

```

Die Details der Parameter lauten wie folgt: 

- Der erste Parameter gibt die Art der Momentaufnahmesicherung an. Die zulässige Werte lauten **hana**, **logs** und **boot**. 
- Der Parameter **<HANA Large Instance Type>** ist notwendig, damit nur Volumesicherungen gestartet werden. Es gibt zwei gültige Werte mit „TypeI“ bzw. „TypeII“, je nach der HANA-Einheit für große Instanzen. Informationen zum Typ Ihrer Einheit finden Sie unter [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).  
- Der Parameter **<snapshot_prefix>** ist eine Momentaufnahme- oder Sicherungsbezeichnung für die Art der Momentaufnahme. Sie hat zwei Aufgaben: Erstens können Sie hiermit einen Namen vergeben, damit Sie wissen, worum es sich bei diesen Momentaufnahmen handelt. Zweitens dient sie dazu, dem Skript *azure\_hana\_backup.pl* die Ermittlung der Anzahl von Speichermomentaufnahmen zu ermöglichen, die unter dieser bestimmten Bezeichnung beibehalten werden. Wenn Sie zwei Speichermomentaufnahme-Sicherungen desselben Typs (z.B. **hana**) mit zwei unterschiedlichen Bezeichnungen planen und dann definieren, dass 30 Momentaufnahmen pro Sicherung beibehalten werden sollen, erhalten Sie letztlich 60 Speichermomentaufnahmen der betroffenen Volumes. Nur alphanumerische Zeichen („A–Z, a–z, 0–9“), Unterstriche („_“) und Bindestriche („-“) sind zulässig. 
- Der Parameter **<snapshot_frequency>** ist für künftige Entwicklungen reserviert und hat keinen Einfluss. Legen Sie ihn auf „3min“ fest, wenn Sie Sicherungen des Typs **log** ausführen, und auf „15min“, wenn Sie die anderen Sicherungstypen ausführen.
- Der Parameter **<number of snapshots retained>** definiert indirekt die Aufbewahrung von Momentaufnahmen, indem die Anzahl von Momentaufnahmen mit dem gleichen Momentaufnahmepräfix (Bezeichnung) festgelegt wird. Dieser Parameter ist für geplante Ausführungen per Cron wichtig. Wenn die Anzahl von Momentaufnahmen mit dem gleichen „snapshot_prefix“ die durch diesen Parameter angegebene Anzahl übersteigt, wird die älteste Momentaufnahme gelöscht, bevor eine neue Speichermomentaufnahme ausgeführt wird.

Beim horizontalen Hochskalieren führt das Skript zusätzliche Überprüfungen durch, um sicherzustellen, dass Sie auf alle HANA-Server zugreifen können. Das Skript überprüft außerdem, ob alle HANA-Instanzen den richtigen Status der Instanzen zurückgeben, bevor es eine SAP HANA-Momentaufnahme erstellt. Nach der SAP HANA-Momentaufnahme folgt eine Speichermomentaufnahme.

Bei Ausführung des Skripts `azure_hana_backup.pl` wird die Speichermomentaufnahme in den folgenden drei Phasen erstellt:

1. Ausführen einer SAP HANA-Momentaufnahme
1. Ausführen einer Speichermomentaufnahme
1. Entfernen der SAP HANA-Momentaufnahme, die vor der Ausführung der Speichermomentaufnahme erstellt wurde

Um das Skript auszuführen, rufen Sie es im ausführbaren HDB-Ordner auf, in den es kopiert wurde. 

Die Aufbewahrungsdauer richtet sich nach der Anzahl von Momentaufnahmen, die bei der Skriptausführung als Parameter übergeben wird. Der von Speichermomentaufnahmen abgedeckte Zeitraum ist eine Funktion aus dem Ausführungszeitraum und der Anzahl von Momentaufnahmen, die bei der Skriptausführung als Parameter angegeben wird. Wenn die Anzahl gespeicherter Momentaufnahmen die beim Skriptaufruf als Parameter angegebene übersteigt, wird vor der Erstellung einer neuen Momentaufnahme die älteste Speichermomentaufnahme mit der gleichen Bezeichnung gelöscht. Die Anzahl, die Sie als letzten Parameter des Aufrufs angeben, ist die Anzahl der beibehaltenen Momentaufnahmen, die Sie steuern können. Mit dieser Anzahl können Sie indirekt auch den von Momentaufnahmen beanspruchten Speicherplatz steuern. 

> [!NOTE]
>Sobald Sie die Bezeichnung ändern, beginnt der Zählvorgang erneut. Sie müssen bei der Bezeichnung genau vorgehen, damit Ihre Momentaufnahmen nicht versehentlich gelöscht werden.

## <a name="snapshot-strategies"></a>Strategien für Momentaufnahmen
Die Momentaufnahmenhäufigkeit für die unterschiedlichen Typen hängt davon ab, ob Sie die Notfallwiederherstellung von HANA (große Instanzen) verwenden. Diese Funktionalität basiert auf Speichermomentaufnahmen, für die ggf. besondere Empfehlungen in Bezug auf die Häufigkeit und Ausführungszeiträume der Speichermomentaufnahmen erforderlich sind. 

Bei den folgenden Überlegungen und Empfehlungen wird davon ausgegangen, dass Sie die Notfallwiederherstellung von HANA (große Instanzen) *nicht* verwenden. Stattdessen verwenden Sie die Speichermomentaufnahmen, um über Sicherungen zu verfügen und eine Zeitpunktwiederherstellung für die letzten 30 Tage zu ermöglichen. Angesichts der Einschränkungen hinsichtlich der Anzahl von Momentaufnahmen und des Speicherplatzes haben Kunden folgende Anforderungen berücksichtigt:

- Zeitraum für Zeitpunktwiederherstellung
- Belegter Speicherplatz
- Recovery Point Objective (RPO) und Recovery Time Objective (RTO) für eine potenzielle Wiederherstellung nach einem Notfall.
- Tatsächliche Ausführung vollständiger HANA-Datenbanksicherungen für Datenträger. Immer wenn eine vollständige Datenbanksicherung für Datenträger oder die Schnittstelle **backint** erfolgt, tritt bei der Ausführung von Speichermomentaufnahmen ein Fehler auf. Wenn Sie zusätzlich zu Speichermomentaufnahmen vollständige Datenbanksicherungen ausführen möchten, sollten Sie sicherstellen, dass die Ausführung von Speichermomentaufnahmen während dieses Zeitraums deaktiviert ist.
- Die Anzahl von Momentaufnahmen pro Volume (auf 250 beschränkt).


Bei Kunden, die die Notfallwiederherstellung von HANA (große Instanzen) nicht verwenden, ist der Momentaufnahmezeitraum kürzer. In solchen Fällen führen Kunden die kombinierten Momentaufnahmen für „/hana/data“ und „/hana/shared“ (einschließlich „/usr/sap“) in einem Intervall von 12 oder 24 Stunden durch und bewahren diese Momentaufnahmen einen Monat lang auf. Gleiches gilt für die Momentaufnahmen des Protokollsicherungsvolumes. Im Gegensatz dazu werden SAP HANA-Transaktionsprotokollsicherungen für das Protokollsicherungsvolume in einem Intervall von 5 bis 15 Minuten ausgeführt.

Es ist am besten, geplante Speichermomentaufnahmen mit Cron durchzuführen. Verwenden Sie für alle Anforderungen in Bezug auf Sicherungen und die Notfallwiederherstellung dasselbe Skript, und passen Sie die Skripteingaben an die verschiedenen angeforderten Sicherungsdauern an. Diese Momentaufnahmen werden in Cron abhängig von ihrer Ausführungszeit jeweils unterschiedlich geplant: stündlich, alle 12 Stunden, täglich oder wöchentlich. 

Hier ist ein Beispiel für einen Cron-Zeitplan in „/etc/crontab“ angegeben:
```
00 1-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 46
10 00 * * *  ./azure_hana_backup.pl hana dailyhana 15min 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup.pl logs regularlogback 3min 28
22 12 * * *  ./azure_hana_backup.pl logs dailylogback 3min 28
30 00 * * *  ./azure_hana_backup.pl boot TypeI dailyboot 15min 28
```
Das vorherige Beispiel enthält eine stündliche kombinierte Momentaufnahme, die die Volumes mit den Speicherorten „/hana/data“ und „/hana/shared“ (einschließlich „/usr/sap“) abdeckt. Verwenden Sie diese Art von Momentaufnahme, um eine schnellere Zeitpunktwiederherstellung für die letzten zwei Tage zu erzielen. Darüber hinaus wird eine tägliche Momentaufnahme für diese Volumes erstellt. Somit verfügen Sie über stündliche Momentaufnahmen für einen Zeitraum von zwei Tagen sowie über tägliche Momentaufnahmen für einen Zeitraum von vier Wochen. Das Volume für die Transaktionsprotokollsicherung wird außerdem täglich gesichert. Diese Sicherungen werden ebenfalls vier Wochen aufbewahrt. In der dritten Zeile von „crontab“ sehen Sie, dass für die Sicherung des HANA-Transaktionsprotokolls ein Ausführungsintervall von fünf Minuten geplant ist. Die Startzeiten der verschiedenen Cron-Aufträge zur Erstellung von Speichermomentaufnahmen sind gestaffelt, damit die Momentaufnahmen nicht alle gleichzeitig zu einem bestimmten Zeitpunkt ausgeführt werden. 

Im folgenden Beispiel führen Sie stündlich eine kombinierte Momentaufnahme durch, die die Volumes mit den Speicherorten „/hana/data“ und „/hana/shared“ (einschließlich „/usr/sap“) abdeckt. Diese Momentaufnahmen werden zwei Tage lang aufbewahrt. Die Momentaufnahmen der Volumes mit der Transaktionsprotokollsicherung werden alle fünf Minuten ausgeführt und vier Stunden lang aufbewahrt. Das Ausführungsintervall für die Sicherung der HANA-Transaktionsprotokolldatei ist wie zuvor auf fünf Minuten festgelegt. Die Momentaufnahme des Volumes mit der Transaktionsprotokollsicherung wird jeweils zwei Minuten nach dem Start der Transaktionsprotokollsicherung erstellt. Nach diesen zwei Minuten sollte die Sicherung des SAP HANA-Transaktionsprotokolls für gewöhnlich abgeschlossen sein. Das Volume mit der Start-LUN wird wie zuvor jeweils einmal pro Tag durch eine Speichermomentaufnahme gesichert und vier Wochen lang aufbewahrt.

```
10 0-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup.pl logs regularlogback 3min 28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl logs logback 3min 48
30 00 * * *  ./azure_hana_backup.pl boot TypeII dailyboot 15min 28
```

Die folgende Grafik veranschaulicht die Sequenzen des vorherigen Beispiels ausgenommen der Start-LUN:

![Beziehung zwischen Sicherungen und Momentaufnahmen](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA führt regelmäßig Schreibvorgänge für das Volume „/hana/log“ aus, um die Datenbankänderungen, für die ein Commit ausgeführt wurde, zu dokumentieren. In regelmäßigen Abständen schreibt SAP HANA einen Sicherungspunkt auf das Volume „/hana/data“. Wie in „crontab“ angegeben wird alle fünf Minuten eine SAP HANA-Transaktionsprotokollsicherung ausgeführt. Aufgrund der Auslösung einer kombinierten Speichermomentaufnahme für die Volumes „/hana/data“ und „/hana/shared“ wird zudem stündlich eine SAP HANA-Momentaufnahme ausgeführt. Nach erfolgreicher Ausführung der HANA-Momentaufnahme wird die kombinierte Speichermomentaufnahme ausgeführt. Wie in „crontab“ angegeben wird alle fünf Minuten (etwa zwei Minuten nach der HANA-Transaktionsprotokollsicherung) die Speichermomentaufnahme des Volumes „/hana/logbackup“ ausgeführt.

> 

>[!IMPORTANT]
> Die Verwendung von Speichermomentaufnahmen für SAP HANA-Sicherungen ist nur sinnvoll, wenn diese in Verbindung mit SAP HANA-Transaktionsprotokollsicherungen erstellt werden. Diese Transaktionsprotokollsicherungen müssen die Zeiträume zwischen den Speichermomentaufnahmen abdecken. 

Folgendes ist erforderlich, wenn Sie für Benutzer eine Zeitpunktwiederherstellung für einen Zeitraum von 30 Tagen zugesagt haben:

- In Extremfällen müssen Sie auf eine kombinierte Speichermomentaufnahme für „/hana/data“ und „/hana/shared“ zugreifen können, die 30 Tage alt ist.
- Sie benötigen lückenlose Transaktionsprotokollsicherungen, die die Zeit zwischen den kombinierten Speichermomentaufnahmen abdecken. Die älteste Momentaufnahme des Volumes mit der Transaktionsprotokollsicherung muss also 30 Tage alt sein. Dies gilt nicht, wenn Sie die Transaktionsprotokollsicherungen auf eine andere NFS-Freigabe in Azure Storage kopieren. In diesem Fall können Sie ältere Transaktionsprotokollsicherungen von dieser NFS-Freigabe abrufen.

Um die Vorteile der Speichermomentaufnahmen und der letztlichen Speicherreplikation von Transaktionsprotokollsicherungen nutzen zu können, müssen Sie den Speicherort ändern, an den SAP HANA die Transaktionsprotokollsicherungen schreibt. Sie können diese Änderung in HANA Studio vornehmen. SAP HANA sichert zwar automatisch vollständige Protokollsegmente, trotzdem sollten Sie ein deterministisches Protokollsicherungsintervall angeben. Dies empfiehlt sich insbesondere bei Verwendung der Notfallwiederherstellungsoption, da Sie in der Regel Protokollsicherungen mit einem deterministischen Intervall ausführen. Im folgenden Fall werden 15 Minuten als Protokollsicherungsintervall festgelegt.

![Planen von SAP HANA-Sicherungsprotokollen in SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Sie können auch Sicherungen wählen, die häufiger als alle 15 Minuten erstellt werden. Eine Einstellung für eine höhere Häufigkeit wird oft in Verbindung mit der Notfallwiederherstellung von HANA (große Instanzen) verwendet. Einige Kunden führen Transaktionsprotokollsicherungen alle fünf Minuten durch.  

Falls die Datenbank noch nie gesichert wurde, wird im letzten Schritt eine dateibasierte Datenbanksicherung durchgeführt, um einen einzelnen Sicherungseintrag zu erstellen, der im Sicherungskatalog vorhanden sein muss. Andernfalls kann SAP HANA Ihre angegebenen Protokollsicherungen nicht initiieren.

![Erstellen einer dateibasierten Sicherung zum Hinzufügen eines einzelnen Sicherungseintrags](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Nach erfolgreicher Erstellung der ersten Speichermomentaufnahmen können Sie die in Schritt 6 erstellte Testmomentaufnahme löschen. Führen Sie hierzu das Skript `removeTestStorageSnapshot.pl` aus:
```
./removeTestStorageSnapshot.pl
```

Hier ist ein Beispiel für die Skriptausgabe angegeben:
```
Checking Snapshot Status for h80
**********************Checking access to Storage**********************
Storage Snapshot Access successful.
**********************Getting list of volumes that match HANA instance specified**********************
Collecting set of volumes hosting HANA matching pattern *h80* ...
Volume show completed successfully.
Adding volume hana_data_h80_mnt00001_t020_vol to the snapshot list.
Adding volume hana_log_backups_h80_t020_vol to the snapshot list.
Adding volume hana_shared_h80_t020_vol to the snapshot list.
**********************Adding list of snapshots to volume list**********************
Collecting set of snapshots for each volume hosting HANA matching pattern *h80* ...
**********************Displaying Snapshots by Volume**********************
hana_data_h80_mnt00001_t020_vol
Test_HANA_Snapshot.2018-02-06_1753.3
Test_HANA_Snapshot.2018-02-06_1815.2
….
Command completed successfully.
Exiting with return code: 0
Command completed successfully.
```


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Überwachen der Anzahl und Größe von Momentaufnahmen auf dem Datenträgervolume

Sie können die Anzahl von Momentaufnahmen und deren Speicherbedarf auf einem bestimmten Speichervolume überwachen. Mit dem Befehl `ls` werden keine Momentaufnahmenverzeichnisse oder -dateien angezeigt. Der Befehl `du` des Linux-Betriebssystems liefert jedoch Details zu den Speichermomentaufnahmen, da sie auf den gleichen Volumes gespeichert sind. Für den Befehl stehen folgende Optionen zur Verfügung:

- `du –sh .snapshot`: Mit dieser Option wird die Summe aller Momentaufnahmen im Verzeichnis für die Momentaufnahmen angegeben.
- `du –sh --max-depth=1`: Mit dieser Option werden alle Momentaufnahmen, die im Ordner **.snapshot** gespeichert sind, und die Größe der einzelnen Momentaufnahmen aufgelistet.
- `du –hc`: Mit dieser Option wird die Gesamtgröße des Speicherplatzes angegeben, der von allen Momentaufnahmen genutzt wird.

Verwenden Sie diese Befehle, um sicherzustellen, dass die erstellten und gespeicherten Momentaufnahmen nicht den gesamten Speicher auf den Volumes belegen.

>[!NOTE]
>Die Momentaufnahmen der Start-LUN werden bei Verwendung der vorherigen Befehle nicht angezeigt.

### <a name="getting-details-of-snapshots"></a>Abrufen von Details zu Momentaufnahmen
Um weitere Informationen zu Momentaufnahmen zu erhalten, können Sie auch das Skript `azure_hana_snapshot_details.pl` verwenden. Dieses Skript kann an beiden Standorten ausgeführt werden, sofern am Standort für die Notfallwiederherstellung ein aktiver Server vorhanden ist. Das Skript enthält die folgende Ausgabe, die nach den einzelnen Volumes mit Momentaufnahmen aufgeschlüsselt sind: 
   * Größe der gesamten Momentaufnahmen in einem Volume
   * Jede Momentaufnahme in diesem Volume enthält die folgenden Details: 
      - Name der Momentaufnahme 
      - Erstellungszeitpunkt 
      - Größe der Momentaufnahme
      - Häufigkeit der Momentaufnahme
      - HANA-Sicherungs-ID im Zusammenhang mit dieser Momentaufnahme, sofern relevant

Hier ist ein Beispiel für die Syntax der Skriptausführung angegeben:

```
./azure_hana_snapshot_details.pl 
```

Da das Skript versucht, die HANA-Sicherungs-ID abzurufen, muss es eine Verbindung mit der SAP HANA-Instanz herstellen. Für diese Verbindung muss die Konfigurationsdatei *HANABackupCustomerDetails.txt* richtig festgelegt sein. Eine Ausgabe zweier Momentaufnahmen eines Volumes kann beispielsweise wie folgt aussehen:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```



### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Verringern der Anzahl von Momentaufnahmen auf einem Server

Wie bereits beschrieben, können Sie die Anzahl von bestimmten Bezeichnungen von Momentaufnahmen reduzieren, die Sie speichern. Die beiden letzten Parameter des Befehls zum Initiieren einer Momentaufnahme sind die Bezeichnung und die Anzahl von Momentaufnahmen, die Sie beibehalten möchten.

```
./azure_hana_backup.pl hana dailyhana 15min 28
```

Im obigen Beispiel lautet die Bezeichnung der Momentaufnahme **dailyhana**, und die Anzahl zu speichernder Momentaufnahmen mit dieser Bezeichnung ist auf **28** festgelegt. Als Reaktion auf die Speicherplatzbelegung können Sie die Anzahl von gespeicherten Momentaufnahmen reduzieren. Die Anzahl von Momentaufnahmen kann ganz einfach auf beispielsweise 15 verringert werden, indem der letzte Parameter bei der Skriptausführung auf **15** festgelegt wird:

```
./azure_hana_backup.pl hana dailyhana 15min 15
```

Wenn Sie das Skript mit dieser Einstellung ausführen, beträgt die Anzahl von Momentaufnahmen „15“ (einschließlich der neuen Speichermomentaufnahme). Die 15 neuesten Momentaufnahmen werden gespeichert, und die 15 älteren Momentaufnahmen werden gelöscht.

 >[!NOTE]
 > Die Anzahl von Momentaufnahmen verringert sich durch dieses Skript aber nur, wenn Momentaufnahmen vorhanden sind, die älter als eine Stunde sind. Mit dem Skript werden Momentaufnahmen, die weniger als eine Stunde alt sind, nicht gelöscht. Diese Einschränkungen beziehen sich auf die bereitgestellten optionalen Funktionen für die Notfallwiederherstellung.

Falls Sie eine Gruppe von Momentaufnahmen mit der Sicherungsbezeichnung (in diesen Syntaxbeispielen: **hanadaily**) nicht mehr benötigen, können Sie das Skript mit **0** als zu speichernde Anzahl ausführen. Alle Momentaufnahmen, die für diese Bezeichnung eine Übereinstimmung ergeben, werden dann entfernt. Das Entfernen aller Momentaufnahmen kann sich aber auf die Funktionen für die Notfallwiederherstellung von großen HANA-Instanzen auswirken.

Alternativ können Sie bestimmte Momentaufnahmen auch mithilfe des Skripts `azure_hana_snapshot_delete.pl` löschen. Dieses Skript dient zum Löschen einer Momentaufnahme oder einer Gruppe von Momentaufnahmen – entweder anhand der HANA-Sicherungs-ID aus HANA Studio oder anhand des Namens der Momentaufnahme. Die Sicherungs-ID ist derzeit nur mit den Momentaufnahmen verknüpft, die für den Momentaufnahmetyp **hana** erstellt werden. Für Momentaufnahmesicherungen vom Typ **logs** und **boot** wird keine SAP HANA-Momentaufnahme erstellt, sodass für diese Momentaufnahmen keine Sicherungs-ID zu finden ist. Bei Eingabe des Namens der Momentaufnahme wird auf den verschiedenen Volumes nach allen Momentaufnahmen gesucht, die dem eingegebenen Namen entsprechen. 

Rufen Sie das erforderliche Skript auf, um die SID der HANA-Instanz über die Aufrufsyntax des Skripts anzugeben:

```
./azure_hana_snapshot_delete.pl <SID>

```

Führen Sie das Skript als Benutzer vom Typ **root** aus.

Wenn Sie eine Momentaufnahme auswählen, können Sie jede Momentaufnahme einzeln löschen. Zunächst geben Sie das Volume mit der Momentaufnahme an und anschließend den Namen der eigentlichen Momentaufnahme. Wenn die Momentaufnahme auf dem Volume vorhanden und über eine Stunde alt ist, wird sie gelöscht. Die Namen der Volumes und Momentaufnahmen können Sie durch Ausführen des Skripts `azure_hana_snapshot_details` ermitteln. 

>[!IMPORTANT]
>Falls Daten nur auf der zu löschenden Momentaufnahme vorhanden sind, sind diese Daten nach dem Löschen der Momentaufnahme unwiederbringlich verloren.

  
## <a name="file-level-restore-from-a-storage-snapshot"></a>Wiederherstellung auf Dateiebene basierend auf einer Speichermomentaufnahme
Bei den Momentaufnahmetypen **hana** und **logs** können Sie direkt auf den Volumes im Verzeichnis **.snapshot** auf die Momentaufnahmen zugreifen. Dort finden Sie jeweils ein Unterverzeichnis für die einzelnen Momentaufnahmen. Sie können jede Datei in dem Zustand, der bei der Erstellung der Momentaufnahme geherrscht hat, aus diesem Unterverzeichnis in die eigentliche Verzeichnisstruktur kopieren. In der aktuellen Version des Skripts wird **KEIN** Wiederherstellungsskript für die Wiederherstellung der Momentaufnahme als Self-Service bereitgestellt, die Wiederherstellung einer Momentaufnahme kann jedoch im Rahmen von Self-Service-DR-Skripts am DR-Standort während des Failovers ausgeführt werden. Sie müssen sich an das Microsoft-Betriebsteam wenden, indem Sie eine Serviceanfrage zum Wiederherstellen einer gewünschten Momentaufnahme aus den vorhandenen verfügbaren Momentaufnahmen öffnen.

>[!NOTE]
>Die Wiederherstellung einzelner Dateien funktioniert nicht für Momentaufnahmen der Start-LUN, unabhängig vom Typ der HANA-Einheiten (große Instanzen). Das Verzeichnis **.snapshot** wird in der Start-LUN nicht verfügbar gemacht. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>Wiederherstellen der letzten HANA-Momentaufnahme

Bei einem Ausfall der Produktionsumgebung kann die Wiederherstellung auf der Grundlage einer Speichermomentaufnahme als Kundenvorfall über den Microsoft Azure-Support initiiert werden. Eine äußerst dringliche Angelegenheit ist ein Szenario, bei dem Daten in einem Produktionssystem gelöscht wurden und die einzige Möglichkeit zu ihrer Wiedererlangung in der Wiederherstellung der Produktionsdatenbank besteht.

Eine Zeitpunktwiederherstellung kann dagegen eine niedrigere Dringlichkeit haben und Tage im Voraus geplant werden. Diese Wiederherstellung können Sie mit dem Dienstverwaltungsteam für SAP HANA in Azure planen, anstatt ein Flag mit hoher Priorität auszulösen. Beispielsweise planen Sie eventuell die Durchführung eines Upgrades für die SAP-Software, indem Sie ein neues Erweiterungspaket anwenden. Sie müssen dann eine Momentaufnahme wiederherstellen, die den Zustand vor dem Upgrade mit dem Erweiterungspaket darstellt.

Vor dem Senden der Anforderung müssen Sie diese vorbereiten. Das Dienstverwaltungsteam für SAP HANA in Azure kann die Anforderung dann bearbeiten und die wiederhergestellten Volumes bereitstellen. Anschließend stellen Sie die HANA-Datenbank basierend auf den Momentaufnahmen wieder her. 

Hier ist angegeben, wie Sie sich auf die Anforderung vorbereiten:

>[!NOTE]
>Ihre Benutzeroberfläche kann von den folgenden Screenshots abweichen. Dies richtet sich nach der von Ihnen verwendeten SAP HANA-Version.

1. Entscheiden Sie, welche Momentaufnahme wiederhergestellt werden soll. Falls nicht anders angegeben, wird nur das Volume „hana/data“ wiederhergestellt. 

1. Fahren Sie die HANA-Instanz herunter.

 ![Herunterfahren der HANA-Instanz](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf. Wenn die Datenvolumes noch in das Betriebssystem eingebunden sind, tritt bei der Wiederherstellung der Momentaufnahme ein Fehler auf.
 ![Heben Sie die Bereitstellung der Datenvolumes auf jedem Knoten der HANA-Datenbank auf](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Erstellen Sie eine Azure-Supportanfrage, und fügen Sie Anweisungen zur Wiederherstellung einer bestimmten Momentaufnahme ein.

 - Während der Wiederherstellung: Unter Umständen werden Sie vom Dienstverwaltungsteam für SAP HANA in Azure zu Koordinierungs-, Überprüfungs- und Bestätigungszwecken gebeten, an einer Telefonkonferenz teilzunehmen, um die Wiederherstellung der korrekten Speichermomentaufnahme sicherzustellen. 

 - Nach der Wiederherstellung: Sie werden von SAP HANA unter der Azure-Dienstverwaltung benachrichtigt, wenn die Speichermomentaufnahme wiederhergestellt wurde.

1. Stellen Sie nach Abschluss des Wiederherstellungsvorgangs alle Datenvolumes wieder bereit.

 ![Erneutes Bereitstellen aller Datenvolumes](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

1. Wählen Sie die Wiederherstellungsoptionen in SAP HANA Studio aus, sofern diese nicht automatisch beim Wiederherstellen der Verbindung mit der HANA-Datenbank über SAP HANA Studio angezeigt werden. Das folgende Beispiel zeigt eine Wiederherstellung der letzten HANA-Momentaufnahme. In einer Speichermomentaufnahme ist eine HANA-Momentaufnahme eingebettet. Wenn Sie die letzte Speichermomentaufnahme wiederherstellen, sollte es sich dabei um die neueste HANA-Momentaufnahme handeln. (Wenn Sie eine ältere Speichermomentaufnahme wiederherstellen, müssen Sie die HANA-Momentaufnahme basierend auf dem Zeitpunkt der Erstellung der Speichermomentaufnahme bestimmen.)

 ![Auswählen von Wiederherstellungsoptionen in SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

1. Wählen Sie **Recover the database to a specific data backup or storage snapshot** (Datenbank in eine bestimmte Datensicherung oder Speichermomentaufnahme wiederherstellen) aus.

 ![Fenster „Wiederherstellungstyp angeben“](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

1. Wählen Sie **Specify backup without catalog** (Sicherung ohne Katalog angeben).

 ![Fenster „Sicherungsspeicherort angeben“](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

1. Wählen Sie in der Liste **Destination Type** (Zieltyp) die Option **Snapshot** (Momentaufnahme).

 ![Fenster „Wiederherzustellende Sicherung angeben“](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

1. Wählen Sie **Fertig stellen**, um den Wiederherstellungsprozess zu starten.

 ![Wählen Sie „Fertig stellen“, um den Wiederherstellungsprozess zu starten.](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

1. Die HANA-Datenbank wird mithilfe der HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist.

 ![HANA-Datenbank wird über die HANA-Momentaufnahme wiederhergestellt](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recover-to-the-most-recent-state"></a>Wiederherstellen des letzten Zustands

Mit dem folgenden Prozess wird die HANA-Momentaufnahme wiederhergestellt, die in der Speichermomentaufnahme enthalten ist. Anschließend wird für die Transaktionsprotokollsicherungen der letzte Zustand der Datenbank wiederhergestellt, bevor die Speichermomentaufnahme wiederhergestellt wird.

>[!IMPORTANT]
>Stellen Sie sicher, dass Sie über eine vollständige und zusammenhängende Kette von Transaktionsprotokollsicherungen verfügen, bevor Sie fortfahren. Ohne diese Sicherungen können Sie den aktuellen Zustand der Datenbank nicht wiederherstellen.

1. Führen Sie unter [Wiederherstellen der letzten HANA-Momentaufnahme](#recovering-to-the-most-recent-hana-snapshot) die Schritte 1 bis 6 aus.

1. Wählen Sie die Option **Recover the database to its most recent state** (Letzten Zustand der Datenbank wiederherstellen).

 ![Auswählen von „Recover the database to its most recent state“ (Letzten Zustand der Datenbank wiederherstellen)](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

1. Geben Sie den Speicherort der letzten HANA-Protokollsicherungen an. Der Speicherort muss alle HANA-Transaktionsprotokollsicherungen aus der HANA-Momentaufnahme bis zum neuesten Zustand enthalten.

 ![Angeben des Speicherorts der letzten HANA-Protokollsicherungen](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

1. Wählen Sie eine Sicherung als Grundlage für die Wiederherstellung der Datenbank aus. In diesem Beispiel zeigt der Screenshot die HANA-Momentaufnahme, die in der Speichermomentaufnahme enthalten war. 

 ![Auswählen einer Sicherung als Grundlage zum Wiederherstellen der Datenbank](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

1. Deaktivieren Sie das Kontrollkästchen **Use Delta Backups** (Deltasicherungen verwenden), sofern diese nicht zwischen dem Zeitpunkt der HANA-Momentaufnahme und dem letzten Zustand vorhanden waren.

 ![Deaktivieren des Kontrollkästchens „Use Delta Backups“ (Deltasicherungen verwenden), falls keine Deltas vorhanden sind](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

1. Wählen Sie auf dem Bildschirm mit der Zusammenfassung **Fertig stellen**, um den Wiederherstellungsvorgang zu starten.

 ![Klicken auf „Finish“ (Fertig stellen) auf dem Bildschirm mit der Zusammenfassung](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recover-to-another-point-in-time"></a>Wiederherstellen eines anderen Zeitpunkts
Gehen Sie zum Durchführen einer Zeitpunktwiederherstellung zwischen der HANA-Momentaufnahme (die in der Speichermomentaufnahme enthalten ist) und einer späteren Zeitpunktwiederherstellung der HANA-Momentaufnahme wie folgt vor:

1. Stellen Sie sicher, dass Sie über alle Transaktionsprotokollsicherungen in der HANA-Momentaufnahme für den Zeitpunkt verfügen, den Sie wiederherstellen möchten.
1. Beginnen Sie mit dem Verfahren unter [Wiederherstellen des letzten Zustands](#recovering-to-the-most-recent-state).
1. Wählen Sie in Schritt 2 des Verfahrens im Fenster **Wiederherstellungstyp angeben** die Option **Datenbank für den folgenden Zeitpunkt wiederherstellen**, und geben Sie anschließend den Zeitpunkt an. 
1. Führen Sie die Schritte 3 bis 6 aus.

## <a name="monitor-the-execution-of-snapshots"></a>Überwachen der Ausführung von Momentaufnahmen

Bei Verwendung von Speichermomentaufnahmen von HANA (große Instanzen) müssen Sie auch die Erstellung dieser Momentaufnahmen überwachen. Das Skript, das eine Speichermomentaufnahme ausführt, schreibt die Ausgabe in eine Datei und speichert sie an demselben Speicherort wie die Perl-Skripts. Für jede Speichermomentaufnahme wird eine separate Datei geschrieben. Die Ausgabe jeder Datei zeigt die verschiedenen Phasen, die vom Momentaufnahmeskript ausgeführt werden:

1. Suchen nach den Volumes, für die eine Momentaufnahme erstellt werden muss
1. Suchen nach den Momentaufnahmen, die basierend auf diesen Volumes erstellt wurden
1. Löschen von eventuell vorhandenen Momentaufnahmen entsprechend der von Ihnen angegebenen Anzahl von Momentaufnahmen
1. Erstellen einer SAP HANA-Momentaufnahme
1. Volumeübergreifendes Erstellen der Speichermomentaufnahme
1. Löschen der SAP HANA-Momentaufnahme
1. Umbenennen der neuesten Momentaufnahme in **.0**

Der wichtigste Teil der Skript-CAB-Datei ist folgender:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Sie können anhand dieses Beispiels erkennen, wie das Skript die Erstellung der HANA-Momentaufnahme aufzeichnet. Bei horizontaler Skalierung wird dieser Prozess auf dem Masterknoten ausgelöst. Der Masterknoten initiiert die synchrone Erstellung der SAP HANA-Momentaufnahmen auf den einzelnen Workerknoten. Anschließend wird die Speichermomentaufnahme erstellt. Nach erfolgreicher Erstellung der Speichermomentaufnahmen wird die HANA-Momentaufnahme gelöscht. Das Löschen der HANA-Momentaufnahme wird basierend auf dem Masterknoten initiiert.


**Nächste Schritte**
- Siehe [Prinzipien und Vorbereitung für die Notfallwiederherstellung](hana-concept-preparation.md).