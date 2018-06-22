---
title: Skalieren von Ermittlung und Bewertung mit Azure Migrate | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine große Anzahl lokaler Computer mit dem Azure Migrate-Dienst bewerten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/04/2018
ms.author: raynew
ms.openlocfilehash: 89c9cfd4bdc1c483764983c886ba9f96cc75c69e
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736829"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Ermitteln und Bewerten einer umfangreichen VMware-Umgebung

Bei Azure Migrate gilt eine Beschränkung von 1500 Computern pro Projekt. In diesem Artikel erfahren Sie, wie Sie eine große Anzahl lokaler virtueller Computer (VMs) mit [Azure Migrate](migrate-overview.md) bewerten.   

## <a name="prerequisites"></a>Voraussetzungen

- **VMware**: Die virtuellen Computer, die Sie migrieren möchten, müssen über die vCenter Server-Version 5.5, 6.0 oder 6.5 verwaltet werden. Zusätzlich benötigen Sie einen ESXi-Host mit der Version 5.0 oder höher, um die Collector-VM bereitzustellen.
- **vCenter-Konto**: Sie benötigen ein schreibgeschütztes Konto für den Zugriff auf vCenter Server. Dieses Konto wird in Azure Migrate zum Ermitteln der lokalen virtuellen Computer verwendet.
- **Berechtigungen:** In vCenter Server benötigen Sie Berechtigungen zum Erstellen eines virtuellen Computers durch Importieren einer Datei im OVA-Format.
- **Statistikeinstellungen:** Die Statistikeinstellungen für vCenter Server müssen vor der Bereitstellung auf Ebene 3 festgelegt werden. Bei einer niedrigeren Ebene wird die Bewertung zwar ausgeführt, die Leistungsdaten für den Speicher und das Netzwerk werden jedoch nicht erfasst. Die Größenempfehlungen werden in diesem Fall auf der Grundlage von Leistungsdaten für CPU und Arbeitsspeicher sowie basierend auf den Konfigurationsdaten für Datenträger- und Netzwerkadapter erstellt.

## <a name="plan-your-migration-projects-and-discoveries"></a>Planen von Migrationsprojekten und Ermittlungen

Ein einziger Azure Migrate-Collector unterstützt Ermittlungen mehrerer vCenter Server-Instanzen (nacheinander) sowie mehrerer Migrationsprojekte (nacheinander). Für den Collector gilt das Prinzip eines Fire-and-Forget-Modells, d.h. nach Abschluss einer Ermittlung kann derselbe Collector verwendet werden, um Daten einer anderen vCenter Server-Instanz zu sammeln oder sie an ein anderes Migrationsprojekt zu senden.

Planen Sie Ihre Ermittlungen und Bewertungen basierend auf den folgenden Limits:

| **Entität** | **Limit für Computer** |
| ---------- | ----------------- |
| Project    | 1.500             |
| Ermittlung  | 1.500             |
| Bewertung | 1.500             |

Berücksichtigen Sie bei der Planung Folgendes:

- Wenn Sie eine Ermittlung mit dem Azure Migrate-Collector durchführen, können Sie den Ermittlungsbereich auf einen vCenter Server-Order, ein Rechenzentrum, einen Cluster oder einen Host festlegen.
- Wenn Sie mehrere Ermittlungen durchführen, vergewissern Sie sich in vCenter Server, dass sich die zu ermittelnden VMs in Ordnern, Rechenzentren, Clustern oder Hosts befinden, die die Beschränkung auf 1.500 Computer unterstützen.
- Zur Bewertung sollten sich Computer mit Abhängigkeiten im gleichen Projekt und in der gleichen Bewertung befinden. Achten Sie daher in vCenter Server darauf, dass sich abhängige Computer zur Bewertung im gleichen Ordner, Rechenzentrum, Cluster oder Host befinden.

Je nach Szenario können Sie Ihre Ermittlungen wie folgt aufteilen:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Bei mehreren vCenter Server-Instanzen mit weniger als 1500 VMs

Wenn es in Ihrer Umgebung mehrere vCenter Server-Instanzen gibt und die Gesamtzahl der virtuellen Computer niedriger als 1500 ist, können Sie zur Ermittlung aller virtuellen Computer über alle vCenter Server-Instanzen hinweg einen einzelnen Collector und ein einzelnes Migrationsprojekt verwenden. Da der Collector jeweils nur eine vCenter Server-Instanz ermittelt, können Sie denselben Collector nacheinander für alle vCenter Server-Instanzen ausführen und ihn auf dasselbe Migrationsprojekt verweisen. Nach Abschluss aller Ermittlungen lassen sich Bewertungen für die Computer erstellen.

### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Bei mehreren vCenter Server-Instanzen mit mehr als 1500 VMs

Wenn Sie mehrere vCenter Server-Instanzen mit weniger als 1500 virtuellen Computern pro vCenter Server-Instanz, aber mehr als 1500 VMs über alle vCenter Server-Instanzen hinweg haben, müssen Sie mehrere Migrationsprojekte erstellen, da ein einzelnes Migrationsprojekt nur 1500 VMs enthalten kann. Hierzu können Sie ein Migrationsprojekt pro vCenter Server-Instanz erstellen und die Ermittlungen aufteilen. Sie können jede vCenter Server-Instanz (nacheinander) mithilfe eines einzigen Collectors ermitteln. Wenn Sie möchten, dass die Ermittlungen zeitgleich beginnen, lassen sie sich durch die Bereitstellung mehrerer Appliances parallel ausführen.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Bei mehr als 1500 Computern in einer einzigen vCenter Server-Instanz

Wenn Sie mehr als 1500 virtuelle Computer in einer einzelnen vCenter Server-Instanz haben, müssen Sie die Ermittlungen auf mehrere Migrationsprojekte aufteilen. Zum Aufteilen der Ermittlungen können Sie in der Appliance das Feld „Bereich“ nutzen und die folgenden zu ermittelnden Daten angeben: Host, Cluster, Ordner oder Rechenzentrum. Angenommen, Sie haben zwei Ordner in der vCenter Server-Instanz – einen mit 1000 VMs (Ordner1) und einen anderen mit 800 VMs (Ordner2). In diesem Fall können Sie einen einzigen Collector zur Ausführung von zwei Ermittlungen verwenden. Für die erste Ermittlung können Sie Ordner1 als Bereich festlegen und ihn auf das erste Migrationsprojekt verweisen. Nach Abschluss der ersten Ermittlung lässt sich derselbe Collector verwenden. Ändern Sie dazu seinen Bereich zu Ordner2, passen Sie die Details zum Migrationsprojekt an das zweite Migrationsprojekt an und führen Sie die zweite Ermittlung aus.

### <a name="multi-tenant-environment"></a>Umgebungen mit mehreren Mandanten

Wenn Sie bei einer von mehreren Mandanten gemeinsam genutzten Umgebung keine VMs eines Mandanten im Abonnement eines anderen ermitteln möchten, können Sie das Feld „Bereich“ in der Collectorappliance nutzen, um den Bereich der Ermittlung festzulegen. Sollten Mandanten Hosts gemeinsam nutzen, erstellen Sie Anmeldeinformationen, die nur auf diejenigen VMs einen schreibgeschützten Zugriff ermöglichen, die dem bestimmten Mandanten gehören. Verwenden Sie dann diese Anmeldeinformationen in der Collectorappliance und geben Sie den Bereich als Host für die Ermittlung an. Alternativ können Sie auch Ordner in der vCenter Server-Instanz erstellen – z.B. Ordner1 für Mandant1 und Ordner2 für Mandant2 – und unter dem gemeinsam genutzten Host die VMs für Mandant1 in Ordner1 und für Mandant2 in Ordner2 verschieben. Legen Sie anschließend im Collector den Bereich für die Ermittlungen fest, indem Sie den entsprechenden Ordner angeben.

## <a name="discover-on-premises-environment"></a>Ermitteln der lokalen Umgebung

Ist Ihre Planung abgeschlossen, können Sie mit dem Ermitteln der lokalen virtuellen Computer beginnen:

### <a name="create-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein für Ihre Anforderungen geeignetes Azure Migrate-Projekt:

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Suchen Sie nach **Azure Migrate**, und wählen Sie den Dienst **Azure Migrate (Vorschau)** in den Suchergebnissen aus. Klicken Sie anschließend auf **Erstellen**.
3. Geben Sie einen Projektnamen und das Azure-Abonnement für das Projekt an.
4. Erstellen Sie eine neue Ressourcengruppe.
5. Geben Sie den Standort an, an dem das Projekt erstellt werden soll, und klicken Sie auf **Erstellen**. Beachten Sie, dass Sie Ihre VMs weiterhin für einen anderen Zielstandort bewerten können. Der für das Projekt angegebene Standort wird zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden.

### <a name="set-up-the-collector-appliance"></a>Einrichten der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als „Collectorappliance“ bezeichnet wird. Diese VM ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Zum Einrichten der Collectorappliance laden Sie eine OVA-Datei herunter und importieren sie auf die lokale vCenter Server-Instanz.

#### <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Wenn Sie über mehrere Projekte verfügen, müssen Sie die Collectorappliance nur einmal auf die vCenter Server-Instanz herunterladen. Nach dem Herunterladen und Einrichten der Appliance müssen Sie diese für jedes Projekt ausführen und jeweils die eindeutige Projekt-ID und den Schlüssel angeben.

1. Klicken Sie im Azure Migrate-Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Klicken Sie in **Computer ermitteln** auf **Herunterladen**, um die OVA-Datei herunterzuladen.
3. Kopieren Sie unter **Projektanmeldeinformationen kopieren** die ID und den Schlüssel für das Projekt. Diese Angaben benötigen Sie beim Konfigurieren des Collectors.


#### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Überprüfen Sie, ob die OVA-Datei sicher ist, bevor Sie sie bereitstellen:

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.

2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Stellen Sie sicher, dass der generierte Hash mit den folgenden Einstellungen übereinstimmt.

    Für OVA-Version 1.0.9.8

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | b5d9f0caf15ca357ac0563468c2e6251
    SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
    SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

    Für OVA-Version 1.0.9.7

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

    Für OVA-Version 1.0.9.5

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  

    Für OVA-Version 1.0.9.2

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  

### <a name="create-the-collector-vm"></a>Erstellen der Collector-VM

Importieren Sie die heruntergeladene Datei auf die vCenter Server-Instanz:

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **OVF-Vorlage bereitstellen**.

    ![Bereitstellen der OVF-Vorlage](./media/how-to-scale-assessment/vcenter-wizard.png)

2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Speicherort** einen Anzeigenamen für den virtuellen Collectorcomputer und das Inventarobjekt an, in dem der virtuelle Computer gehostet wird.
4. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf dem der virtuelle Collectorcomputer ausgeführt wird.
5. Geben Sie im Speicher das Speicherziel für den virtuellen Collectorcomputer an.
6. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
7. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der virtuelle Collectorcomputer eine Verbindung herstellt. Das Netzwerk benötigt Internetkonnektivität, um Metadaten an Azure zu senden.
8. Überprüfen und bestätigen Sie die Einstellungen, und klicken Sie auf **Fertig stellen**.

### <a name="identify-the-id-and-key-for-each-project"></a>Ermitteln von ID und Schlüssel für jedes Projekt

Wenn Sie über mehrere Projekte verfügen, müssen Sie die ID und den Schlüssel für jedes der Projekte ermitteln. Der Schlüssel wird benötigt, wenn Sie den Collector ausführen, um die virtuellen Computer zu ermitteln.

1. Klicken Sie im Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Kopieren Sie unter **Projektanmeldeinformationen kopieren** die ID und den Schlüssel für das Projekt.
    ![Projektanmeldeinformationen kopieren](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="set-the-vcenter-statistics-level"></a>Festlegen der vCenter-Statistikebene
Die folgende Liste enthält die Leistungsindikatoren, die im Rahmen der Ermittlung erfasst werden. Die Indikatoren sind standardmäßig auf verschiedenen Ebenen in vCenter Server verfügbar.

Wir empfehlen, für die Statistik die höchste allgemeine Ebene (3) festzulegen, damit alle Leistungsindikatoren ordnungsgemäß erfasst werden. Wenn Sie für vCenter eine niedrigere Ebene festgelegt haben, werden möglicherweise nur einige Leistungsindikatoren vollständig erfasst, und der Rest ist auf 0 festgelegt. Dadurch erhalten Sie in der Bewertung möglicherweise unvollständige Daten.

Die folgende Tabelle gibt auch Aufschluss über die betroffenen Bewertungsergebnisse, wenn ein bestimmter Indikator nicht erfasst wird.

| Indikator                                 | Ebene | Pro Gerät | Auswirkungen auf die Bewertung                    |
| --------------------------------------- | ----- | ---------------- | ------------------------------------ |
| cpu.usage.average                       | 1     | Nicht verfügbar               | Empfohlene VM-Größe und -Kosten         |
| mem.usage.average                       | 1     | Nicht verfügbar               | Empfohlene VM-Größe und -Kosten         |
| virtualDisk.read.average                | 2     | 2                | Datenträgergröße, Speicherkosten und VM-Größe |
| virtualDisk.write.average               | 2     | 2                | Datenträgergröße, Speicherkosten und VM-Größe |
| virtualDisk.numberReadAveraged.average  | 1     | 3                | Datenträgergröße, Speicherkosten und VM-Größe |
| virtualDisk.numberWriteAveraged.average | 1     | 3                | Datenträgergröße, Speicherkosten und VM-Größe |
| net.received.average                    | 2     | 3                | VM-Größe und Netzwerkkosten             |
| net.transmitted.average                 | 2     | 3                | VM-Größe und Netzwerkkosten             |

> [!WARNING]
> Nach dem Festlegen einer höheren Statistikebene kann es bis zu einem Tag dauern, bis die Leistungsindikatordaten generiert werden. Die Ermittlung sollte deshalb erst nach einem Tag durchgeführt werden.

### <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

Für die einzelnen Ermittlungen müssen Sie jeweils den Collector ausführen, um virtuelle Computer im erforderlichen Bereich zu ermitteln. Führen Sie die Ermittlungen nacheinander durch. Parallele Ermittlungen werden nicht unterstützt, und jede Ermittlung muss einen anderen Bereich umfassen.

1.  Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und klicken Sie auf **Konsole öffnen**.
2.  Geben Sie für die Appliance die Einstellungen für Sprache, Zeitzone und Kennwort an.
3.  Klicken Sie auf dem Desktop auf die Verknüpfung **Collector ausführen**.
4.  Öffnen Sie im Azure Migrate-Collector **Erforderliche Komponenten einrichten**, und gehen Sie dann wie folgt vor:

    a. Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.

    Der Collector überprüft, ob der virtuelle Computer über Internetzugriff verfügt.

    b. Wenn der virtuelle Computer über einen Proxy auf das Internet zugreift, klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an. Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.

    Der Collector überprüft, ob der Collector-Dienst ausgeführt wird. Der Dienst wird standardmäßig auf dem virtuellen Collectorcomputer installiert.

    c. Laden Sie die VMware PowerCLI herunter, und installieren Sie sie.

5.  Nehmen Sie in **vCenter Server-Details angeben** die folgenden Einstellungen vor:
    - Geben Sie den Namen (FQDN) oder die IP-Adresse der vCenter Server-Instanz an.
    - Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, mit dem der Collector VMs in vCenter Server ermittelt.
    - Wählen Sie unter **Bereich auswählen** einen Bereich für die Ermittlung virtueller Computer aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Der Bereich sollte nicht mehr als 1,000 virtuelle Computer umfassen.

6.  Geben Sie unter **Migrationsprojekt angeben** die ID und den Schlüssel für das Projekt an. Wenn Sie diese Angaben nicht kopiert haben, öffnen Sie das Azure-Portal über die Collector-VM. Klicken Sie auf der Seite **Übersicht** des Projekts auf **Computer ermitteln**, und kopieren Sie die Werte.  
7.  Überwachen Sie in **Sammlungsfortschritt anzeigen** den Ermittlungsprozess, und vergewissern Sie sich, dass sich die von den VMs erfassten Metadaten innerhalb des zulässigen Bereichs befinden. Der Collector gibt eine ungefähre Ermittlungszeit an.


#### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Die Ermittlungszeit hängt von der Anzahl der ermittelten virtuellen Computer ab. Normalerweise dauert die Ermittlung bei 100 virtuellen Computern nach Abschluss der Collector-Ausführung ungefähr eine Stunde.

1. Klicken Sie im Migration Planner-Projekt auf **Verwalten** > **Computer**.
2. Überprüfen Sie, ob die zu ermittelnden virtuellen Computer im Portal angezeigt werden.


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Erstellen einer Gruppe](how-to-create-a-group.md) für die Bewertung.
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
