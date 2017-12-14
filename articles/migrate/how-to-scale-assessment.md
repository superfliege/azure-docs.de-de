---
title: Skalieren von Ermittlung und Bewertung mit Azure Migrate | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie eine große Anzahl lokaler Computer mit dem Azure Migrate-Dienst bewerten."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: e28a2144dd102fcd2ec05531432cac0df250ae01
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2017
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Ermitteln und Bewerten einer umfangreichen VMware-Umgebung

In diesem Artikel erfahren Sie, wie Sie eine große Anzahl lokaler Computer mit [Azure Migrate](migrate-overview.md) bewerten. Azure Migrate bewertet Computer, um zu prüfen, ob diese für die Migration zu Azure geeignet sind, und gibt Größen- und Kostenschätzungen für die Ausführung des jeweiligen Computers in Azure ab.

## <a name="prerequisites"></a>Voraussetzungen

- **VMware:** Sie benötigen mindestens einen virtuellen VMware-Computer auf einem ESXi-Host oder Cluster mit der Version 5.0 oder höher. Der Host oder Cluster muss über einen vCenter-Server mit Version 5.5 oder 6.0 verwaltet werden.
- **vCenter-Konto:** Sie benötigen ein schreibgeschütztes Konto mit Administratoranmeldeinformationen für den vCenter-Server. Dieses Konto wird in Azure Migrate zum Ermitteln virtueller Computer verwendet.
- **Berechtigungen:** Auf dem vCenter-Server benötigen Sie Berechtigungen zum Erstellen eines virtuellen Computers durch Importieren einer Datei im OVA-Format.
- **Statistikeinstellungen:** Die Statistikeinstellungen für den vCenter-Server müssen vor der Bereitstellung mindestens auf Ebene 2 festgelegt werden.

## <a name="plan-azure-migrate-projects"></a>Planen von Azure Migrate-Projekten

Im Rahmen eines Azure Migrate-Projekts können bis zu 1.500 Computer bewertet werden. Mit einer einzelnen Ermittlung in einem Projekt können bis zu 1.000 Computer ermittelt werden.

- Wenn weniger als 1.000 Computer ermittelt werden müssen, benötigen Sie ein einzelnes Projekt mit einer einzelnen Ermittlung.
- Wenn Sie über 1.000 bis 1.500 Computer verfügen, benötigen Sie ein einzelnes Projekt mit zwei Ermittlungen.
- Wenn Sie über mehr als 1.500 Computer verfügen, müssen Sie mehrere Projekte erstellen und ggf. mehrere Ermittlungen durchführen. Beispiel:
    - Wenn Sie über 3.000 Computer verfügen, können Sie zwei Projekte mit zwei Ermittlungen oder drei Projekte mit jeweils einer einzelnen Ermittlung einrichten.
    - Bei 5.000 Computern können Sie vier Projekte einrichten: zwei mit einer Ermittlung von 1.500 Computern und eins mit einer Ermittlung von 500 Computern. Alternativ können Sie fünf Projekte mit jeweils einer einzelnen Ermittlung einrichten. 
- Bei Ermittlungen in Azure Migrate können Sie den Ermittlungsbereich auf einen VMware-Ordner, auf ein Datencenter oder auf einen Cluster festlegen.
- Wenn Sie mehrere Ermittlungen durchführen, vergewissern Sie sich in vCenter, dass sich die zu ermittelnden virtuellen Computer in Ordnern, Datencentern oder Clustern befinden, die die Beschränkung auf 1.000 Computer unterstützen.
- Zur Bewertung sollten sich Computer mit Abhängigkeiten im gleichen Projekt und in der gleichen Bewertung befinden. Achten Sie daher in vCenter darauf, dass sich abhängige Computer zur Bewertung im gleichen Ordner, Datencenter oder Cluster befinden.


## <a name="create-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein für Ihre Anforderungen geeignetes Azure Migrate-Projekt.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Suchen Sie nach **Azure Migrate**, und wählen Sie den Dienst (**Azure Migrate (Vorschau)**) in den Suchergebnissen aus. Klicken Sie dann auf **Erstellen**.
3. Geben Sie einen Projektnamen und das Azure-Abonnement für das Projekt an.
4. Erstellen Sie eine neue Ressourcengruppe.
5. Geben Sie die Region an, in der das Projekt erstellt wird, und klicken Sie dann auf **Erstellen**. Die von lokalen virtuellen Computern erfassten Metadaten werden in dieser Region gespeichert.

## <a name="set-up-the-collector-appliance"></a>Einrichten der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als Collectorappliance bezeichnet wird. Dieser virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Laden Sie zum Einrichten der Collectorappliance eine OVA-Datei herunter, und importieren Sie sie auf dem lokalen vCenter-Server, um den virtuellen Computer zu erstellen.

### <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Wenn Sie über mehrere Projekte verfügen, müssen Sie die Collectorappliance nur einmal auf den vCenter-Server herunterladen. Nach dem Herunterladen und Einrichten der Appliance müssen Sie diese für jedes Projekt ausführen und jeweils die eindeutige Projekt-ID und den Schlüssel angeben.

1. Klicken Sie im Azure Migrate-Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Klicken Sie unter **Computer ermitteln** auf **Herunterladen**, um die OVA-Datei herunterzuladen.
3. Kopieren Sie unter **Projektanmeldeinformationen kopieren** die ID und den Schlüssel für das Projekt. Diese Angaben benötigen Sie beim Konfigurieren des Collectors.

   
### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Überprüfen Sie, ob die OVA-Datei sicher ist, bevor Sie sie bereitstellen.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Der generierte Hash muss folgenden Einstellungen entsprechen.

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | c283f00f46484bf673dc8383b01d0741 
    SHA1 | 8f49b47f53d051af1780bbc725659ce64e717ed4
    SHA256 | 7aecdbdb2aea712efe99d0c1444503f52d16de5768e783465e226fbbca71501d

## <a name="create-the-collector-vm"></a>Erstellen des virtuellen Collectorcomputers

Importieren Sie die heruntergeladene Datei auf den vCenter-Server.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![Bereitstellen der OVF-Vorlage](./media/how-to-scale-assessment/vcenter-wizard.png)

2. Geben Sie im Assistenten „Deploy OVF Template“ (OVF-Vorlage bereitstellen) > **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Speicherort** einen Anzeigenamen für den virtuellen Collectorcomputer und das Inventarobjekt an, in dem der virtuelle Computer gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf dem der virtuelle Collectorcomputer ausgeführt wird.
7. Geben Sie im Speicher das Speicherziel für den virtuellen Collectorcomputer an.
8. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
9. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der virtuelle Collectorcomputer eine Verbindung herstellt. Das Netzwerk benötigt eine Internetverbindung, damit Metadaten an Azure gesendet werden können. 
10. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.

## <a name="identify-the-key-and-id-for-each-project"></a>Ermitteln von Schlüssel und ID für die einzelnen Projekte

Wenn Sie über mehrere Projekte verfügen, ermitteln Sie jeweils die ID und den Schlüssel. Der Schlüssel wird benötigt, wenn Sie den Collector ausführen, um die virtuellen Computer zu ermitteln.

1. Klicken Sie im Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Kopieren Sie unter **Projektanmeldeinformationen kopieren** die ID und den Schlüssel für das Projekt. 
    ![Projekt-ID](./media/how-to-scale-assessment/project-id.png)

## <a name="vcenter-statistics-level-to-collect-the-performance-counters"></a>vCenter-Statistikebene zum Erfassen der Leistungsindikatoren
Die folgende Liste enthält die Leistungsindikatoren, die im Rahmen der Ermittlung erfasst werden. Die Indikatoren sind standardmäßig auf verschiedenen Ebenen auf dem vCenter-Server verfügbar. Wir empfehlen, für die Statistik die höchste allgemeine Ebene (Ebene 3) festzulegen, damit alle Leistungsindikatoren ordnungsgemäß erfasst werden. Wenn Sie für vCenter eine niedrigere Ebene festgelegt haben, werden möglicherweise nur einige wenige Leistungsindikatoren vollständig erfasst, und der Rest ist auf 0 festgelegt. Dadurch erhalten Sie unter Umständen unvollständige Daten. Die folgende Tabelle gibt auch Aufschluss über die Bewertungsergebnisse, die betroffen sind, wenn ein bestimmter Indikator nicht erfasst wird.

|Indikator                                  |Ebene    |Ebene pro Gerät  |Auswirkungen auf die Bewertung                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|cpu.usage.average                        | 1       |NA                |Empfohlene VM-Größe und Kosten                    |
|mem.usage.average                        | 1       |NA                |Empfohlene VM-Größe und Kosten                    |
|virtualDisk.read.average                 | 2       |2                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|virtualDisk.write.average                | 2       |2                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Datenträgergröße, Speicherkosten und VM-Größe         |
|net.received.average                     | 2       |3                 |VM-Größe und Netzwerkkosten                        |
|net.transmitted.average                  | 2       |3                 |VM-Größe und Netzwerkkosten                        |

> [!WARNING]
> Nach dem Festlegen einer höheren Statistikebene kann es bis zu einem Tag dauern, bis Leistungsindikatoren generiert wurden. Die Ermittlung sollte daher erst nach einem Tag durchgeführt werden.

## <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

Für die einzelnen Ermittlungen müssen Sie jeweils den Collector ausführen, um virtuelle Computer im erforderlichen Bereich zu ermitteln. Führen Sie die Ermittlungen nacheinander durch. Parallele Ermittlungen werden nicht unterstützt, und jede Ermittlung muss einen anderen Bereich umfassen.

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und klicken Sie auf **Konsole öffnen**.
2. Geben Sie für die Appliance die Einstellungen für Sprache, Zeitzone und Kennwort an.
3. Klicken Sie auf dem Desktop auf die Verknüpfung **Collector ausführen**.
4. Öffnen Sie im Azure Migrate-Collector die Option **Set Up Prerequisites** (Voraussetzungen einrichten).
    - Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - Der Collector überprüft, ob der virtuelle Computer über Internetzugriff verfügt.
    - Wenn der virtuelle Computer über einen Proxy auf das Internet zugreift, klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an. Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
    - Der Collector überprüft, ob der Windows-Profiler-Dienst ausgeführt wird. Der Dienst wird standardmäßig auf dem virtuellen Collectorcomputer installiert.
    - Laden Sie die VMware PowerCLI herunter, und installieren Sie sie.
. Gehen Sie in **Computer ermitteln** wie folgt vor:
    - Geben Sie den Namen (FQDN) oder die IP-Adresse des vCenter-Servers an.
    - Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das der Collector virtuelle Computer auf dem vCenter-Server ermittelt.
    - Wählen Sie unter **Sammlungsbereich** einen Bereich für die Ermittlung virtueller Computer aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Er sollte nicht mehr als 1.000 virtuelle Computer umfassen. 
    - Wählen Sie unter n **Tag category for grouping** (Tagkategorie für Gruppierung) die Option **Keine** aus.

        ![Auswählen des Bereichs](./media/how-to-scale-assessment/select-scope.png)

1. Geben Sie unter **Projekt auswählen** die ID und den Schlüssel für das Projekt an. Falls Sie diese Angaben nicht kopiert haben, öffnen Sie das Azure-Portal über den virtuellen Collectorcomputer. Klicken Sie auf der Seite **Übersicht** des Projekts auf **Computer ermitteln**, und kopieren Sie die Werte.  
Überwachen Sie den Ermittlungsprozess unter **Complete Discovery** (Ermittlung abschließen), und vergewissern Sie sich, dass sich die von den virtuellen Computern erfassten Metadaten innerhalb des zulässigen Bereichs befinden. Der Collector gibt eine ungefähre Ermittlungszeit an.


### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Die Ermittlungszeit hängt von der Anzahl der ermittelten virtuellen Computer ab. Normalerweise dauert der Abschluss der Ermittlung bei 100 virtuellen Computern ungefähr eine Stunde, nachdem der Collector ausgeführt wurde. 

1. Klicken Sie im Migration Planner-Projekt auf **Verwalten** > **Computer**.
2. Überprüfen Sie, ob die zu ermittelnden virtuellen Computer im Portal angezeigt werden.


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Erstellen einer Gruppe](how-to-create-a-group.md) für die Bewertung.
- Lesen Sie die [weiteren Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen.