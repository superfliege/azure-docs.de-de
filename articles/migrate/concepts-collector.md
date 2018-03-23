---
title: Collectorappliance in Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Collectorappliance und deren Konfiguration.
author: ruturaj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 01/23/2017
ms.author: ruturajd
services: azure-migrate
ms.openlocfilehash: 49f3d5ba55a9c1abfcd6dcb50058ed7a001a2eec
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="collector-appliance"></a>Collectorappliance

[Azure Migrate](migrate-overview.md) bewertet lokale Workloads für die Migration zu Azure. Dieser Artikel enthält Informationen zur Verwendung der Collectorappliance.



## <a name="overview"></a>Übersicht

Ein Azure Migrate-Collector ist eine einfache Appliance, die zum Ermitteln Ihrer lokalen vCenter-Umgebung genutzt werden kann. Diese Appliance ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst.

Die Collectorappliance ist ein OVF-Paket, das Sie aus dem Azure Migrate-Projekt herunterladen können. Es instanziiert einen virtuellen VMware-Computer mit 4 Kernen, 8 GB RAM und einem Datenträger mit 80 GB. Das Betriebssystem der Appliance ist Windows Server 2012 R2 (64-Bit).

Sie können den Collector erstellen, indem Sie die Schritte in [Erstellen der Collector-VM](tutorial-assessment-vmware.md#create-the-collector-vm) ausführen.

## <a name="collector-communication-diagram"></a>Abbildung zur Kommunikation zwischen Collectors

![Abbildung zur Kommunikation zwischen Collectors](./media/tutorial-assessment-vmware/portdiagram.PNG)


| Komponente      | Für die Kommunikation mit   | Erforderlicher Port                            | Grund                                   |
| -------------- | --------------------- | ---------------------------------------- | ---------------------------------------- |
| Collector      | Azure Migrate-Dienst | TCP 443                                  | Der Collector sollte über SSL-Port 443 mit dem Dienst kommunizieren können. |
| Collector      | vCenter Server        | Standard 443                             | Der Collector sollte über vCenter Server mit dem Dienst kommunizieren können. Standardmäßig stellt er über 443 eine Verbindung mit vCenter her. Wenn vCenter an einem anderen Port lauscht, muss dieser Port auf dem Collector als ausgehender Port konfiguriert werden. |
| Collector      | RDP|   | TCP 3389 | Damit soll Ihnen die Verwendung des RDP auf dem Collectorcomputer ermöglicht werden. |





## <a name="collector-pre-requisites"></a>Voraussetzungen für den Collector

Der Collector muss einige Prüfungen auf erforderliche Komponenten bestehen, um sicherzustellen, dass er eine Verbindung mit dem Azure Migrate-Dienst herstellen und die ermittelten Daten hochladen kann. In diesem Artikel werden die einzelnen erforderlichen Komponenten behandelt und erläutert, warum sie erforderlich sind.

### <a name="internet-connectivity"></a>Internetkonnektivität

Die Collectorappliance muss mit dem Internet verbunden sein, um die ermittelten Computerinformationen zu senden. Der Computer kann mit einer der beiden folgenden Methoden mit dem Internet verbunden werden.

1. Sie können den Collector so konfigurieren, dass er über direkten Internetzugriff verfügt.
2. Sie können den Collector für die Verbindung über einen Proxyserver konfigurieren.
    * Wenn der Proxyserver eine Authentifizierung erfordert, können Sie den Benutzernamen und das Kennwort in den Verbindungseinstellungen angeben.
    * Die IP-Adresse/der FQDN des Proxyservers muss im Format „http://IPadresse“ oder „http://FQDN“ angegeben sein. Nur HTTP-Proxys werden unterstützt.

> [!NOTE]
> HTTPS-basierte Proxyserver werden vom Collector nicht unterstützt.

#### <a name="whitelisting-urls-for-internet-connection"></a>Verwenden von URL-Whitelists für die Internetverbindung

Die Überprüfung der Voraussetzungen ist erfolgreich, wenn der Collector über die angegebenen Einstellungen eine Verbindung mit dem Internet herstellen kann. Die Konnektivitätsprüfung wird durch das Herstellen einer Verbindung mit einer Liste von URLs wie in der folgenden Tabelle durchgeführt. Wenn Sie URL-basierte Firewallproxys zur Steuerung ausgehender Verbindungen verwenden, stellen Sie sicher, dass die folgenden erforderlichen Dienst-URLs zur Whitelist hinzugefügt werden:

**URL** | **Zweck**  
--- | ---
*.portal.azure.com | Erforderlich, um die Konnektivität mit dem Azure-Dienst zu überprüfen und Probleme mit der Zeitsynchronisation zu überprüfen.

Bei der Überprüfung wird außerdem versucht, die Konnektivität mit den folgenden URLs zu überprüfen, die Überprüfung schlägt jedoch nicht fehl, wenn kein Zugriff möglich ist. Das Konfigurieren der Whitelist für die folgenden URLs ist optional, Sie müssen jedoch manuelle Schritte ausführen, um die Überprüfung der Voraussetzungen zu beschränken.

**URL** | **Zweck**  | **Was geschieht ohne Whitelist?**
--- | --- | ---
*.oneget.org:443 | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen. | PowerCLI-Installation schlägt fehl. Installieren Sie das Modul manuell.
*.windows.net:443 | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen. | PowerCLI-Installation schlägt fehl. Installieren Sie das Modul manuell.
*.windowsazure.com:443 | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen. | PowerCLI-Installation schlägt fehl. Installieren Sie das Modul manuell.
*.powershellgallery.com:443 | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen. | PowerCLI-Installation schlägt fehl. Installieren Sie das Modul manuell.
*.msecnd.net:443 | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen. | PowerCLI-Installation schlägt fehl. Installieren Sie das Modul manuell.
*.visualstudio.com:443 | Erforderlich, um das auf PowerShell basierende vCenter PowerCLI-Modul herunterzuladen. | PowerCLI-Installation schlägt fehl. Installieren Sie das Modul manuell.

### <a name="time-is-in-sync-with-the-internet-server"></a>Mit dem Internetzeitserver synchronisierte Uhrzeit

Der Collector muss mit dem Internetzeitserver synchronisiert sein, um sicherzustellen, dass die Anforderungen an den Dienst authentifiziert werden. Die URL „portal.azure.com“ muss über den Collector erreichbar sein, damit die Uhrzeit überprüft werden kann. Wenn der Computer nicht synchronisiert ist, müssen Sie die Uhrzeit auf dem virtuellen Collectorcomputer folgendermaßen an die aktuelle Uhrzeit anpassen:

1. Öffnen Sie auf dem virtuellen Computer eine Administratoreingabeaufforderung.
1. Führen Sie „w32tm /tz“ aus, um die Zeitzone zu überprüfen.
1. Führen Sie „w32tm /resync“ aus, um die Zeit zu synchronisieren.

### <a name="collector-service-should-be-running"></a>Ausführung des Collectordiensts

Der Azure Migrate-Collectordienst muss auf dem Computer ausgeführt werden. Dieser Dienst wird beim Hochfahren des Computers automatisch gestartet. Wenn der Dienst nicht ausgeführt wird, können Sie den Dienst *Azure Migrate-Collector* über die Systemsteuerung starten. Der Collectordienst ist dafür zuständig, eine Verbindung mit dem vCenter-Server herzustellen, die Meta- und Leistungsdaten von Computern zu erfassen und diese an den Dienst zu senden.

### <a name="vmware-powercli-65"></a>VMware PowerCLI 6.5 

Das Powershell-Modul „VMware PowerCLI“ muss installiert sein, damit der Collector mit dem vCenter-Server kommunizieren und Abfragen für die Details und Leistungsdaten von Computern durchführen kann. Das Powershell-Modul wird im Rahmen der Überprüfung der Voraussetzungen automatisch heruntergeladen und installiert. Der automatische Download erfordert einige URLs in der Whitelist. Andernfalls müssen Sie den Zugriff bereitstellen, indem Sie sie entweder auf die Whitelist setzen oder das Modul manuell installieren.

Installieren Sie das Modul mit den folgenden Schritten manuell:

1. Zum Installieren von PowerCLI im Collector ohne Internetverbindung führen Sie die Schritte unter [diesem Link](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html) durch.
2. Nachdem Sie das PowerShell-Modul auf einem anderen Computer mit Internetzugriff installiert haben, kopieren Sie die Dateien vom Typ „VMware.*“ von diesem Computer auf den Collectorcomputer.
3. Starten Sie die Überprüfung der Voraussetzungen neu, und vergewissern Sie sich, dass PowerCLI installiert ist.

## <a name="connecting-to-vcenter-server"></a>Herstellen einer Verbindung mit vCenter Server

Der Collector muss eine Verbindung mit vCenter Server herstellen und in der Lage sein, eine Abfrage für die virtuellen Computer, deren Metadaten und Leistungsindikatoren durchzuführen. Diese Daten werden vom Projekt zum Berechnen einer Bewertung verwendet.

1. Zur Verbindung mit vCenter Server kann ein schreibgeschütztes Konto mit Berechtigungen wie in der folgenden Tabelle verwendet werden, um die Ermittlung durchzuführen. 

    |Aufgabe  |Erforderliche Rolle/erforderliches Konto  |Berechtigungen  |
    |---------|---------|---------|
    |Collectorappliance-basierte Ermittlung    | Sie benötigen mindestens einen Benutzer mit Lesezugriff.        |Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt)         |

2. Für die Ermittlung kann nur auf Rechenzentren zugegriffen werden, die für das angegebene vCenter-Konto zugänglich sind.
3. Sie müssen den FQDN/die IP-Adresse für vCenter angeben, um die Verbindung mit dem vCenter-Server herzustellen. Die Verbindung wird standardmäßig über Port 443 hergestellt. Wenn Sie vCenter zum Lauschen an einer anderen Portnummer konfiguriert haben, können Sie diese als Teil der Serveradresse im Format „IPAdresse:Portnummer“ oder „FQDN:Portnummer“ angeben.
4. Die Statistikeinstellungen für vCenter Server müssen vor der Bereitstellung auf Ebene 3 festgelegt werden. Bei einer niedrigeren Ebene wird die Ermittlung zwar abgeschlossen, die Leistungsdaten für Speicher und Netzwerk werden jedoch nicht erfasst. Die Größenempfehlungen für die Bewertung werden in diesem Fall basierend auf Leistungsdaten für CPU und Arbeitsspeicher sowie nur auf Konfigurationsdaten für Datenträger- und Netzwerkadapter erstellt. [Erfahren Sie mehr](./concepts-collector.md) dazu, welche Daten erfasst werden und wie sie sich auf die Bewertung auswirken.
5. Der Collector benötigt eine „Sichtverbindung“ mit dem vCenter-Server über das Netzwerk.

> [!NOTE]
> Nur die vCenter Server-Versionen 5.5, 6.0 und 6.5 werden offiziell unterstützt.

> [!IMPORTANT]
> Wir empfehlen, für die Statistik die höchste allgemeine Ebene (3) festzulegen, damit alle Leistungsindikatoren ordnungsgemäß erfasst werden. Wenn Sie für vCenter eine niedrigere Ebene festgelegt haben, werden möglicherweise nur einige Leistungsindikatoren vollständig erfasst, und der Rest ist auf 0 festgelegt. Dadurch erhalten Sie in der Bewertung möglicherweise unvollständige Daten. 

### <a name="selecting-the-scope-for-discovery"></a>Auswählen des Bereichs für die Ermittlung

Sobald die Verbindung mit vCenter besteht, können Sie einen Bereich für die Ermittlung auswählen. Durch die Auswahl eines Bereichs werden alle virtuellen Computer aus dem angegebenen vCenter-Inventurpfad ermittelt.

1. Der Bereich kann entweder ein Rechenzentrum, ein Ordner oder ein ESXi-Host sein. 
2. Sie können jeweils nur einen Bereich auswählen. Zum Auswählen weiterer virtueller Computer können eine Ermittlung abschließen und den Ermittlungsprozess mit einem neuen Bereich neu starten.
3. Sie können nur einen Bereich mit *weniger als 1000 virtuellen Computern* auswählen. Wenn Sie einen Bereich mit mehr als 1000 virtuellen Computern auswählen, müssen Sie den Bereich in kleinere Einheiten aufteilen, indem Sie Ordner erstellen. Als Nächstes müssen Sie unabhängige Ermittlungen der kleineren Ordner ausführen.

## <a name="specify-migration-project"></a>Festlegen des Migrationsprojekts

Sobald die lokale vCenter-Instanz verbunden und ein Bereich angegeben ist, können Sie jetzt die Details des Migrationsprojekt angeben, die für die Ermittlung und Bewertung verwendet werden müssen. Geben Sie Projekt-ID und -schlüssel an, und stellen Sie die Verbindung her.

## <a name="start-discovery-and-view-collection-progress"></a>Starten der Ermittlung und Anzeigen des Sammlungsfortschritts

Sobald die Ermittlung startet, werden die virtuellen vCenter-Computer ermittelt, und ihre Meta- und Leistungsdaten werden an den Server gesendet. Die Fortschrittsanzeige informiert Sie auch über die folgenden IDs:

1. Collector-ID: Eine eindeutige ID, die an Ihren Collectorcomputer vergeben wird. Diese ID wird für einen Computer für verschiedene Ermittlungen beibehalten. Sie können diese ID bei Fehlern für das Melden des Problems an den Microsoft-Support verwenden.
2. Sitzungs-ID: Eine eindeutige ID für den aktuell ausgeführten Sammlungsauftrag. Diese Sitzungs-ID finden Sie auch im Portal, wenn der Ermittlungsauftrag abgeschlossen wird. Diese ID wird für jeden Sammlungsauftrag geändert. Bei Fehlern können Sie diese ID dem Microsoft-Support melden.

### <a name="what-data-is-collected"></a>Welche Daten werden gesammelt?

Der Sammlungsauftrag ermittelt die folgenden statischen Metadaten zu den ausgewählten virtuellen Computern. 

1. Anzeigename des virtuellen Computers (in vCenter)
2. Inventurpfad des virtuellen Computers (Host/Ordner in vCenter)
3. IP-Adresse
4. MAC-Adresse
5. Anzahl der Kerne, Datenträger, NICs
6. RAM, Datenträgergrößen
7. Leistungsindikatoren und des virtuellen Computers, Datenträgers und Netzwerks wie in der folgenden Tabelle aufgeführt

Die folgende Tabelle enthält die erfassten Leistungsindikatoren sowie die Bewertungsergebnisse, die betroffen sind, wenn ein bestimmter Indikator nicht erfasst wird.

|Indikator                                  |Ebene    |Pro Gerät  |Auswirkungen auf die Bewertung                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|cpu.usage.average                        | 1       |Nicht verfügbar                |Empfohlene VM-Größe und -Kosten                    |
|mem.usage.average                        | 1       |Nicht verfügbar                |Empfohlene VM-Größe und -Kosten                    |
|virtualDisk.read.average                 | 2       |2                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|virtualDisk.write.average                | 2       |2                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|net.received.average                     | 2       |3                 |VM-Größe und Netzwerkkosten                        |
|net.transmitted.average                  | 2       |3                 |VM-Größe und Netzwerkkosten                        |

> [!WARNING]
> Nach dem Festlegen einer höheren Statistikebene kann es bis zu einem Tag dauern, bis die Leistungsindikatordaten generiert werden. Die Ermittlung sollte deshalb erst nach einem Tag durchgeführt werden.

### <a name="time-required-to-complete-the-collection"></a>Der Zeitaufwand für die Sammlung

Der Collector ermittelt nur die Computerdaten und sendet sie an das Projekt. Das Projekt kann zusätzliche Zeit in Anspruch nehmen, bis die ermittelten Daten im Portal angezeigt werden und Sie mit dem Erstellen einer Bewertung beginnen können.

Je nach Anzahl der virtuellen Computer im ausgewählten Bereich dauert das Senden der statischen Metadaten an das Projekt bis zu 15 Minuten. Sobald die statischen Metadaten im Portal verfügbar sind, können Sie die Liste der Computer im Portal anzeigen und mit der Gruppenerstellung beginnen. Eine Bewertung kann erst erstellt werden, wenn der Auftrag abgeschlossen ist und das Projekt die Daten verarbeitet hat. Nachdem der Sammlungsauftrag auf dem Collector abgeschlossen ist, kann es bis zu einer Stunde dauern, bis die Leistungsdaten im Portal verfügbar sind. Die Dauer hängt von der Anzahl der virtuellen Computer im ausgewählten Bereich ab.

## <a name="how-to-upgrade-collector"></a>Gewusst wie: Durchführen eines Upgrades für den Collector

Sie können ein Upgrade für den Collector auf die neueste Version durchführen, ohne die OVA-Datei erneut herunterladen zu müssen.

1. Laden Sie das aktuelle [Upgradepaket](https://aka.ms/migrate/col/latestupgrade) herunter.
2. Um sicherzustellen, dass der heruntergeladene Hotfix sicher ist, öffnen Sie das Befehlsfenster als Administrator, und führen Sie den folgenden Befehl zum Generieren des Hash für die ZIP-Datei aus. Der generierte Hash sollte mit dem für die betreffende Version genannten Hash übereinstimmen:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    
    (Beispiel: „C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.5.zip SHA256“)
3. Kopieren Sie die ZIP-Datei auf den virtuellen Azure Migrate-Collectorcomputer (Collectorappliance).
4. Klicken Sie mit der rechten Maustaste auf die ZIP-Datei, und wählen Sie „Alle extrahieren“ aus.
5. Klicken Sie mit der rechten Maustaste auf „Setup.ps1“, und klicken Sie auf „Mit PowerShell ausführen“. Befolgen Sie dann die Anweisungen auf dem Bildschirm, um das Update zu installieren.

### <a name="list-of-updates"></a>Liste der Updates

#### <a name="upgrade-to-version-1095"></a>Upgrade auf Version 1.0.9.5

Laden Sie zum Durchführen eines Upgrade auf Version 1.0.9.5 das [Paket](https://aka.ms/migrate/col/upgrade_9_5) herunter.

**Algorithmus** | **Hashwert**
--- | ---
MD5 | d969ebf3bdacc3952df0310d8891ffdf
SHA1 | f96cc428eaa49d597eb77e51721dec600af19d53
SHA256 | 07c03abaac686faca1e82aef8b80e8ad8eca39067f1f80b4038967be1dc86fa1

## <a name="next-steps"></a>Nächste Schritte

[Einrichten einer Bewertung für lokale virtuelle VMware-Computer](tutorial-assessment-vmware.md)
