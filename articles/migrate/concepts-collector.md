---
title: Informationen zur Collectorappliance in Azure Migrate | Microsoft-Dokumentation
description: Informationen zur Collectorappliance in Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: 81e6731068db84f02073f02c49bea9a8fb7c7c70
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241190"
---
# <a name="about-the-collector-appliance"></a>Informationen zur Collectorappliance

 Dieser Artikel enthält Informationen zum Azure Migrate-Collector.

Der Azure Migrate-Collector ist eine einfache Appliance, die zum Ermitteln einer lokalen vCenter-Umgebung verwendet werden kann, damit vor der Migration zu Azure eine Überprüfung mit dem [Azure Migrate](migrate-overview.md)-Dienst durchgeführt werden kann.  

## <a name="discovery-methods"></a>Ermittlungsmethoden

Es gibt zwei Optionen für die Collectorappliance: einmalige Ermittlung und kontinuierliche Ermittlung.

### <a name="one-time-discovery"></a>Einmalige Ermittlung

Die Collectorappliance kommuniziert einmalig mit vCenter Server, um Metadaten über die VMs zu sammeln. Er verwendet diese Methode:

- Die Appliance ist nicht kontinuierlich mit dem Azure Migrate-Projekt verbunden.
- In der lokalen Umgebung vorgenommene Änderungen werden nach Abschluss der Ermittlung in Azure Migrate nicht widergespiegelt. Sie müssen dieselbe Umgebung in demselben Projekt erneut ermitteln, um sämtliche Änderungen widerspiegeln zu können.
- Beim Sammeln von Leistungsdaten für eine VM verwendet die Appliance die in vCenter Server gespeicherten Verlaufsleistungsdaten. Sie erfasst den Leistungsverlauf des letzten Monats.
- Für die Sammlung historischer Leistungsdaten müssen Sie die Statistikeinstellungen in vCenter Server auf Ebene 3 festlegen. Nach dem Festlegen der Ebene 3 müssen Sie mindestens einen Tag warten, damit vCenter Leistungsindikatoren sammeln kann. Daher wird empfohlen, die Ermittlung erst nach mindestens einem Tag durchzuführen. Wenn Sie die Umgebung anhand der Leistungsdaten für eine Woche oder einen Monat bewerten möchten, müssen Sie entsprechend lange warten.
- Bei dieser Ermittlungsmethode erfasst Azure Migrate Durchschnittswerte für jede Metrik (keine Spitzenwerte). Dies kann zu Unterdimensionierung führen. Es wird empfohlen, die kontinuierliche Erkennung zu verwenden, um genauere Ergebnisse für die Größenfestlegung zu erhalten.

### <a name="continuous-discovery"></a>Kontinuierliche Ermittlung

Die Collectorappliance ist kontinuierlich mit dem Azure Migrate-Projekt verbunden und sammelt kontinuierlich Leistungsdaten von VMs.

- Der Collector profiliert kontinuierlich die lokale Umgebung, um alle 20 Sekunden Echtzeit-Verwendungsdaten zu sammeln.
- Die Appliance führt für die 20-Sekunden-Stichproben ein Rollup aus und erstellt alle 15 Minuten einen einzelnen Datenpunkt.
- Zum Erstellen des Datenpunkts wählt die Appliance den Spitzenwert aus den 20-Sekunden-Stichproben aus und sendet diesen an Azure.
- Dieses Modell ist für die Sammlung von Leistungsdaten nicht von den Statistikeinstellungen von vCenter Server abhängig.
- Sie können die kontinuierliche Profilerstellung jederzeit über den Collector beenden.

Beachten Sie, dass die Appliance nur Leistungsdaten kontinuierlich erfasst. Sie erkennt keine Konfigurationsänderungen in der lokalen Umgebung (etwa hinzugefügte VMs, Löschvorgänge, hinzugefügte Datenträger etc.). Wenn sich die Konfiguration in der lokalen Umgebung ändert, können Sie wie folgt vorgehen, damit die Änderungen im Portal berücksichtigt werden:

- Hinzugefügte Elemente (VMs, Datenträger, Kerne und Ähnliches): Damit diese Änderungen im Azure-Portal berücksichtigt werden, können Sie die Ermittlung über die Appliance beenden und anschließend wieder starten. Dadurch wird sichergestellt, dass die Änderungen im Azure Migrate-Projekt aktualisiert werden.

- Gelöschte VMs: Das Löschen von VMs wird aufgrund des Entwurfs der Appliance auch dann nicht berücksichtigt, wenn Sie die Ermittlung beenden und wieder starten. Das liegt daran, dass Daten nachfolgender Ermittlungen älteren Ermittlungen angefügt und nicht überschrieben werden. In diesem Fall können Sie die VM im Portal einfach ignorieren, indem Sie sie aus Ihrer Gruppe entfernen und die Bewertung neu berechnen.

> [!NOTE]
> Die Funktionalität der kontinuierlichen Ermittlung ist in der Vorschau. Es empfiehlt sich, diese Methode zu verwenden, da sie präzise Leistungsdaten erfasst und zu einer korrekten Größe führt.

## <a name="deploying-the-collector"></a>Bereitstellen des Collectors

Sie können die Collectorappliance mithilfe einer OVF-Vorlage bereitstellen:

- Laden Sie die OVF-Vorlage aus einem Azure Migrate-Projekt im Azure-Portal herunter. Importieren Sie die heruntergeladene Datei in vCenter Server, um die VM der Collectorappliance einzurichten.
- VMware richtet über die OVF-Vorlage eine VM mit 4 Kernen, 8 GB RAM und einem Datenträger mit 80 GB ein. Das Betriebssystem ist Windows Server 2012 R2 (64-Bit).
- Bei der Ausführung des Collectors werden einige Voraussetzungsprüfungen ausgeführt, um sicherzustellen, dass der Collector eine Verbindung mit Azure Migrate herstellen kann.

- [Erfahren Sie mehr](tutorial-assessment-vmware.md#create-the-collector-vm) über das Erstellen des Collectors.


## <a name="collector-prerequisites"></a>Voraussetzungen für den Collector

Der Collector muss einige Voraussetzungsprüfungen bestehen, um sicherzustellen, dass er über das Internet eine Verbindung mit dem Azure Migrate-Dienst herstellen und ermittelte Daten hochladen kann.

- **Internetverbindung prüfen**: Der Collector kann eine direkte Verbindung mit dem Internet herstellen oder über einen Proxy.
    - Bei der Voraussetzungsprüfung wird die Konnektivität mit [erforderlichen und optionalen URLs](#connect-to-urls) überprüft.
    - Wenn Sie eine direkte Verbindung mit dem Internet haben, müssen Sie lediglich sicherstellen, dass der Collector die erforderlichen URLs erreichen kann.
    - Wenn Sie eine Verbindung über einen Proxy herstellen, sollten Sie die [nachfolgenden Anforderungen](#connect-via-a-proxy) beachten.
- **Zeitsynchronisierung überprüfen**: Der Collector muss mit dem Internetzeitserver synchronisiert sein, um sicherzustellen, dass die Anforderungen an den Dienst authentifiziert werden.
    - Die URL „portal.azure.com“ muss über den Collector erreichbar sein, damit die Uhrzeit überprüft werden kann.
    - Wenn der Computer nicht synchronisiert wird, müssen Sie die Uhrzeit auf der Collector-VM an die aktuelle Uhrzeit anpassen. Öffnen Sie hierzu auf der VM eine Eingabeaufforderung für Administratoren, und führen Sie **w32tm /tz** zum Überprüfen der Zeitzone aus. Führen Sie **w32tm /resync** zum Synchronisieren der Zeit aus.
- **Ausführung des Collectordiensts überprüfen**: Der Azure Migrate-Collectordienst sollte auf der Collector-VM ausgeführt werden.
    - Dieser Dienst wird beim Hochfahren des Computers automatisch gestartet.
    - Starten Sie den Dienst über die Einstellungen, falls er nicht ausgeführt wird.
    - Der Collectordienst stellt eine Verbindung mit vCenter Server her, sammelt die VM-Metadaten und -Leistungsdaten und sendet diese an den Azure Migrate-Dienst.
- **Installation von VMware PowerCLI 6.5 überprüfen**: Das PowerShell-Modul VMware PowerCLI 6.5 muss auf der Collector-VM installiert sein, damit eine Kommunikation mit vCenter Server möglich ist.
    - Wenn der Collector auf die URLs zugreifen kann, die für die Installation des Moduls erforderlich sind, erfolgt die Installation automatisch während der Bereitstellung des Collectors.
    - Wenn der Collector das Modul während der Bereitstellung nicht installieren kann, müssen Sie es [manuell installieren](#install-vwware-powercli-module-manually).
- **Verbindung mit vCenter Server überprüfen**: Der Collector muss eine Verbindung mit vCenter Server herstellen und VMs, die zugehörigen Metadaten und Leistungsindikatoren abfragen können. [Überprüfen Sie die Voraussetzungen](#connect-to-vcenter-server) zum Herstellen einer Verbindung.


### <a name="connect-to-the-internet-via-a-proxy"></a>Herstellen einer Verbindung mit dem Internet über einen Proxy

- Wenn der Proxyserver eine Authentifizierung erfordert, können Sie den Benutzernamen und das Kennwort bei der Einrichtung des Collectors angeben.
- Die IP-Adresse bzw. der FQDN des Proxyservers sollte das Format *http://IPaddress* oder *http://FQDN* aufweisen.
- Es werden nur HTTP-Proxys unterstützt. HTTPS-basierte Proxyserver werden vom Collector nicht unterstützt.
- Wenn es sich bei dem Proxyserver um einen abfangenden Proxy handelt, müssen Sie das Proxyzertifikat auf der Collector-VM importieren.
    1. Rufen Sie auf der Collector-VM den Eintrag **Startmenü** > **Computerzertifikate verwalten** auf.
    2. Suchen Sie im Tool „Zertifikate“ unter **Zertifikate – Lokaler Computer** nach **Vertrauenswürdige Herausgeber** > **Zertifikate**.

        ![Tool „Zertifikate“](./media/concepts-intercepting-proxy/certificates-tool.png)

    3. Kopieren Sie das Proxyzertifikat auf die Collector-VM. Möglicherweise müssen Sie das Zertifikat über Ihren Netzwerkadministrator abrufen.
    4. Doppelklicken Sie auf das Zertifikat, um es zu öffnen, und klicken Sie anschließend auf **Zertifikat installieren**.
    5. Wählen Sie im Zertifikatimport-Assistenten die Option **Lokaler Computer** als Speicherort aus.

    ![Zertifikatspeicherort](./media/concepts-intercepting-proxy/certificate-store-location.png)

    6. Wählen Sie **Alle Zertifikate in folgendem Speicher speichern** > **Durchsuchen** > **Vertrauenswürdige Herausgeber** aus. Klicken Sie auf **Fertig stellen**, um das Zertifikat zu importieren.

    ![Zertifikatspeicher](./media/concepts-intercepting-proxy/certificate-store.png)

    7. Überprüfen Sie, ob das Zertifikat wie erwartet importiert wurde und ob die Voraussetzungsprüfung für die Internetkonnektivität wie erwartet funktioniert.




### <a name="connect-to-urls"></a>Verbinden mit URLs

Die Konnektivitätsprüfung wird durch das Herstellen einer Verbindung mit einer Liste von URLs durchgeführt.

**URL** | **Details**  | **Voraussetzungsprüfung**
--- | --- | ---
*.portal.azure.com | Überprüft die Konnektivität mit dem Azure-Dienst und die Zeitsynchronisierung. | Zugriff auf URLs ist erforderlich.<br/><br/> Die Voraussetzungsprüfung schlägt fehl, wenn keine Konnektivität besteht.
*.oneget.org:443<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *.powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443| Wird zum Herunterladen des auf PowerShell basierenden vCenter PowerCLI-Moduls verwendet. | Zugriff auf URLs ist optional.<br/><br/> Die Voraussetzungsprüfung schlägt nicht fehl.<br/><br/> Die automatische Installation des Moduls auf der Collector-VM schlägt fehl. Sie müssen das Modul manuell installieren.


### <a name="install-vmware-powercli-module-manually"></a>Manuelles Installieren des VMware PowerCLI-Moduls

1. Installieren Sie das Modul mit den [folgenden Schritten](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). In diesen Schritten wird sowohl die Online- als auch die Offline-Installation beschrieben.
2. Wenn die Collector-VM offline ist und das Modul auf einem anderen Computer mit Internetzugriff installiert wird, müssen Sie die VMware.*-Dateien von diesem Computer auf die Collector-VM kopieren.
3. Nach der Installation können Sie die Voraussetzungsprüfungen neu starten, um sicherzustellen, dass PowerCLI installiert wurde.

### <a name="connect-to-vcenter-server"></a>Herstellen einer Verbindung mit vCenter Server

Der Collector stellt eine Verbindung mit vCenter Server her und fragt VM-Metadaten und Leistungsindikatoren ab. Für das Herstellen der Verbindung benötigen Sie Folgendes:

- Nur die vCenter Server-Versionen 5.5, 6.0 und 6.5 werden unterstützt.
- Für die Ermittlung benötigen Sie ein schreibgeschütztes Konto mit den nachfolgend zusammengefassten Berechtigungen. Für die Ermittlung sind nur die Rechenzentren zugänglich, auf die mit dem Konto zugegriffen werden kann.
- Sie stellen standardmäßig über einen FQDN oder eine IP-Adresse eine Verbindung mit vCenter Server her. Wenn vCenter Server auf einem anderen Port lauscht, stellen Sie mithilfe des Formulars *IPAddress:Port_Number* oder *FQDN:Port_Number* eine Verbindung her.
- Die Statistikeinstellungen für vCenter Server müssen auf Ebene 3 festgelegt sein, damit Leistungsdaten für Speicher und Netzwerk gesammelt werden können.
- Bei einer niedrigeren Ebene ist eine Ermittlung möglich, die Leistungsdaten werden jedoch nicht gesammelt. Einige Leistungsindikatoren werden möglicherweise gesammelt, andere werden jedoch auf 0 (null) festgelegt.
- Wenn keine Leistungsdaten für Speicher und Netzwerk gesammelt werden, werden die Größenempfehlungen für die Bewertung basierend auf Leistungsdaten für CPU und Arbeitsspeicher sowie auf Konfigurationsdaten für Datenträger- und Netzwerkadapter erstellt.
- Der Collector benötigt eine „Sichtverbindung“ mit dem vCenter-Server über das Netzwerk.

#### <a name="account-permissions"></a>Kontoberechtigungen

**Konto** | **Berechtigungen**
--- | ---
Mindestens ein Benutzerkonto mit Lesezugriff | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt)   


## <a name="collector-communications"></a>Kommunikation des Collectors

In dem folgenden Diagramm und der folgenden Tabelle finden Sie eine Übersicht über die Kommunikation des Collectors.

![Abbildung zur Kommunikation zwischen Collectors](./media/tutorial-assessment-vmware/portdiagram.PNG)


**Collector kommuniziert mit** | **Port** | **Details**
--- | --- | ---
Azure Migrate-Dienst | TCP 443 | Der Collector kommuniziert über SSL-Port 443 mit dem Azure Migrate-Dienst.
vCenter Server | TCP 443 | Der Collector muss mit vCenter Server kommunizieren können.<br/><br/> Standardmäßig stellt er über Port 443 eine Verbindung mit vCenter her.<br/><br/> Wenn vCenter Server an einem anderen Port lauscht, sollte dieser Port auf dem Collector als ausgehender Port verfügbar sein.
RDP | TCP 3389 |


## <a name="securing-the-collector-appliance"></a>Sichern der Collectorappliance

Es wird empfohlen, die folgenden Schritte zum Sichern der Collectorappliance auszuführen:

- Teilen Sie Administratorkennwörter nicht unbefugten Personen mit, und verlieren Sie sie nicht.
- Fahren Sie die Appliance herunter, wenn Sie sie nicht verwenden.
- Ordnen Sie die Appliance in einem geschützten Netzwerk an.
- Löschen Sie nach Abschluss der Migration die Appliance-Instanz.
- Darüber hinaus sollten Sie die Sicherungsdateien der Datenträger (VMDKs) löschen, da darauf möglicherweise vCenter-Anmeldeinformationen zwischengespeichert werden.

## <a name="os-license-in-the-collector-vm"></a>Betriebssystemlizenz auf der Collector-VM

Der Collector ist mit einer Windows Server 2012 R2-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum für Ihre Collector-VM abläuft, wird empfohlen, eine neue OVA-Datei herunterzuladen und eine neue Appliance zu erstellen.

## <a name="updating-the-os-of-the-collector-vm"></a>Aktualisieren des Betriebssystems der Collector-VM

Obwohl die Collectorappliance über eine 180 Tage lang gültige Evaluierungslizenz verfügt, müssen Sie das Betriebssystem auf der Appliance in regelmäßigen Zeitabständen aktualisieren, um zu verhindern, dass die Appliance automatisch heruntergefahren wird.

- Wird der Collector 60 Tage lang nicht aktualisiert, beginnt er, den Computer automatisch herunterzufahren.
- Während einer Ermittlung wird der Computer nicht ausgeschaltet, auch nicht nach 60 Tagen. Nach Abschluss der Ermittlung wird der Computer ausgeschaltet.
- Wenn Sie den Collector länger als 60 Tage verwenden, wird empfohlen, den Computer durch die Ausführung von Windows-Updates stets auf dem neuesten Stand zu halten.

## <a name="upgrading-the-collector-appliance-version"></a>Durchführen eines Upgrades für die Version der Collectorappliance

Sie können ein Upgrade für den Collector auf die neueste Version durchführen, ohne die OVA-Datei erneut herunterladen zu müssen.

1. Laden Sie das [aktuelle Upgradepaket](concepts-collector-upgrade.md) herunter
2. Um sicherzustellen, dass der heruntergeladene Hotfix sicher ist, öffnen Sie das Befehlsfenster als Administrator, und führen Sie den folgenden Befehl zum Generieren des Hash für die ZIP-Datei aus. Der generierte Hash sollte mit dem für die betreffende Version genannten Hash übereinstimmen:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (Beispiel: C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Kopieren Sie die ZIP-Datei auf den virtuellen Azure Migrate-Collectorcomputer (Collectorappliance).
4. Klicken Sie mit der rechten Maustaste auf die ZIP-Datei, und wählen Sie „Alle extrahieren“ aus.
5. Klicken Sie mit der rechten Maustaste auf „Setup.ps1“, und klicken Sie auf „Mit PowerShell ausführen“. Befolgen Sie dann die Anweisungen auf dem Bildschirm, um das Update zu installieren.

## <a name="discovery-process"></a>Ermittlungsprozess

Nach der Einrichtung der Appliance können Sie die Ermittlung durchführen. So funktioniert es:

- Sie führen eine Ermittlung nach Bereich durch. Alle VMs im angegebenen vCenter-Inventarpfad werden ermittelt.
    - Sie legen jeweils einen Bereich fest.
    - Der Bereich kann bis zu 1.500 VMs umfassen.
    - Der Bereich kann entweder ein Rechenzentrum, ein Ordner oder ein ESXi-Host sein.
- Nach dem Herstellen einer Verbindung mit vCenter Server stellen Sie eine Verbindung her, indem Sie ein Migrationsprojekt für die Sammlung angeben.
- VMs werden ermittelt, und die zugehörigen Metadaten und Leistungsdaten werden an Azure gesendet. Diese Aktionen sind Teil eines Sammlungsauftrags.
    - Der Collectorappliance wird eine bestimmte Collector-ID zugeordnet, die ermittlungsübergreifend für einen bestimmten Computer beständig ist.
    - Einem aktuell ausgeführten Sammlungsauftrag wird eine bestimmte Sitzungs-ID zugeordnet. Die ID ändert sich bei jedem Sammlungsauftrag und kann zur Problembehandlung verwendet werden.

### <a name="collected-metadata"></a>Gesammelte Metadaten

Die Collectorappliance ermittelt folgende statische Metadaten für VMs:

- Anzeigename der VM (in vCenter Server)
- Inventarpfad der VM (der Host/Ordner in vCenter Server)
- IP-Adresse
- MAC-Adresse
- Betriebssystem
- Anzahl der Kerne, Datenträger, NICs
- Arbeitsspeichergröße, Datenträgergrößen
- Leistungsindikatoren der VM, Datenträger und Netzwerk.

#### <a name="performance-counters"></a>Leistungsindikatoren

- **Einmalige Ermittlung**: Beachten Sie Folgendes, wenn die Leistungsindikatoren für eine einmalige Ermittlung gesammelt werden:

    - Es kann bis zu 15 Minuten dauern, bis Konfigurationsmetadaten gesammelt und an das Projekt gesendet werden.
    - Nach der Sammlung der Konfigurationsdaten kann es bis zu einer Stunde dauern, bis Leistungsdaten im verfügbar sind.
    - Wenn die Metadaten im Portal verfügbar sind, wird die Liste der VMs angezeigt, und Sie können mit dem Erstellen von Gruppen für die Bewertung beginnen.
- **Kontinuierliche Ermittlung**: Beachten Sie bei der kontinuierlichen Ermittlung Folgendes:
    - Konfigurationsdaten für die VM sind eine Stunde nach Beginn der Ermittlung verfügbar
    - Leistungsdaten sind nach zwei Stunden verfügbar.
    - Warten Sie nach dem Starten der Ermittlung mindestens einen Tag, bis die Appliance ein Profil für die Umgebung erstellt hat, bevor Sie Bewertungen erstellen.

**Leistungsindikator** | **Level** | **Ebene pro Gerät** | **Auswirkung auf die Bewertung**
--- | --- | --- | ---
cpu.usage.average | 1 | Nicht verfügbar | Empfohlene VM-Größe und -Kosten  
mem.usage.average | 1 | Nicht verfügbar | Empfohlene VM-Größe und -Kosten  
virtualDisk.read.average | 2 | 2 | Berechnet die Datenträgergröße, Speicherkosten und die VM-Größe
virtualDisk.write.average | 2 | 2  | Berechnet die Datenträgergröße, Speicherkosten und die VM-Größe
virtualDisk.numberReadAveraged.average | 1 | 3 |  Berechnet die Datenträgergröße, Speicherkosten und die VM-Größe
virtualDisk.numberWriteAveraged.average | 1 | 3 |   Berechnet die Datenträgergröße, Speicherkosten und die VM-Größe
net.received.average | 2 | 3 |  Berechnet die VM-Größe                          |
net.transmitted.average | 2 | 3 | Berechnet die VM-Größe     

## <a name="next-steps"></a>Nächste Schritte

[Einrichten einer Bewertung für lokale virtuelle VMware-Computer](tutorial-assessment-vmware.md)
