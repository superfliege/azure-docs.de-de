---
title: Ermitteln und Bewerten lokaler virtueller VMware-Computer für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie lokale virtuelle VMware-Computer für die Migration zu Azure mithilfe des Azure Migrate-Diensts ermittelt und bewertet werden.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 268ec150dbd4b15ad00a56b62b84e268c4469ebd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Ermitteln und Bewerten lokaler virtueller VMware-Computer für die Migration zu Azure

Mit dem [Azure Migrate](migrate-overview.md)-Dienst werden lokale Workloads für die Migration zu Azure bewertet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Kontos, das von Azure Migrate zum Ermitteln lokaler virtueller Computer verwendet wird
> * Erstellen eines Azure Migrate-Projekts
> * Einrichten eines lokalen virtuellen Collectorcomputers zum Ermitteln lokaler virtueller VMware-Computer für die Bewertung
> * Gruppieren virtueller Computer und Erstellen einer Bewertung


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

- **VMware**: Die virtuellen Computer, die Sie migrieren möchten, müssen von vCenter Server mit der Version 5.5, 6.0 oder 6.5 verwaltet werden. Zusätzlich benötigen Sie einen ESXi-Host mit der Version 5.0 oder höher, um die Collector-VM bereitzustellen.

> [!NOTE]
> Die Unterstützung für Hyper-V ist Teil der Roadmap und wird in Kürze verfügbar gemacht.

- **vCenter Server-Konto**: Sie benötigen ein schreibgeschütztes Konto, um auf den vCenter Server zugreifen zu können. Dieses Konto wird in Azure Migrate zum Ermitteln der lokalen virtuellen Computer verwendet.
- **Berechtigungen:** Auf dem vCenter Server benötigen Sie Berechtigungen zum Erstellen eines virtuellen Computers durch Importieren einer Datei im OVA-Format.
- **Statistikeinstellungen:** Die Statistikeinstellungen für den vCenter Server müssen vor der Bereitstellung auf Ebene 3 festgelegt werden. Bei einer niedrigeren Ebene als Ebene 3 wird die Bewertung zwar ausgeführt, die Leistungsdaten für den Speicher und das Netzwerk werden jedoch nicht erfasst. Die Größenempfehlungen in diesem Fall werden auf der Grundlage von Leistungsdaten für CPU und Arbeitsspeicher und Konfigurationsdaten für Datenträger- und Netzwerkadapter erstellt.

## <a name="create-an-account-for-vm-discovery"></a>Erstellen eines Kontos für die VM-Ermittlung

Azure Migrate benötigt Zugriff auf VMware-Server, um automatisch virtuelle Computer für die Bewertung ermitteln zu können. Erstellen Sie ein VMware-Konto mit folgenden Eigenschaften. Dieses Konto wird bei der Einrichtung von Azure Migrate angegeben.

- Benutzertyp: mindestens ein Benutzer mit Lesezugriff
- Berechtigungen: Data Center object (Rechenzentrumsobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle: schreibgeschützt)
- Details: Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.
- Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle „No access“ (Kein Zugriff) mit „Propagate to child object“ (Auf untergeordnetes Objekt übertragen) zu.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

## <a name="create-a-project"></a>Erstellen eines Projekts

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Suchen Sie nach **Azure Migrate**, und wählen Sie in den Suchergebnissen den Dienst **Azure Migrate** aus. Klicken Sie dann auf **Erstellen**.
3. Geben Sie einen Projektnamen und das Azure-Abonnement für das Projekt an.
4. Erstellen Sie eine neue Ressourcengruppe.
5. Geben Sie die Region an, in der das Projekt erstellt werden soll, und klicken Sie dann auf **Erstellen**. Azure Migrate-Projekte können nur in der Region „USA, Westen-Mitte“ oder „USA, Osten“ erstellt werden. Sie können Ihre Migration jedoch trotzdem für jeden beliebigen Zielort von Azure planen. Der für das Projekt angegebene Standort wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden.

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)



## <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als „Collectorappliance“ bezeichnet wird. Dieser virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Zum Einrichten der Collectorappliance laden Sie eine OVA-Datei herunter und importieren sie auf den lokalen vCenter-Server, um den virtuellen Computer zu erstellen.

1. Klicken Sie im Azure Migrate-Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Klicken Sie in **Computer ermitteln** auf **Herunterladen**, um die OVA-Datei herunterzuladen.
3. Kopieren Sie in **Projektanmeldeinformationen kopieren** die Projekt-ID und den Projektschlüssel. Diese benötigen Sie beim Konfigurieren des Collectors.

    ![Herunterladen der OVA-Datei](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Überprüfen Sie, ob die OVA-Datei sicher ist, bevor Sie sie bereitstellen.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Der generierte Hash muss folgenden Einstellungen entsprechen.

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

    Für OVA-Version 1.0.8.59:

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | 71139e24a532ca67669260b3062c3dad
    SHA1 | 1bdf0666b3c9c9a97a07255743d7c4a2f06d665e
    SHA256 | 6b886d23b24c543f8fc92ff8426cd782a77efb37750afac397591bda1eab8656  

    Für OVA-Version 1.0.8.49:
    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    Für OVA-Version 1.0.8.40:

    **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Erstellen der Collector-VM

Importieren Sie die heruntergeladene Datei auf den vCenter Server.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![Bereitstellen der OVF-Vorlage](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. Geben Sie im Assistenten „Deploy OVF Template“ (OVF-Vorlage bereitstellen) > **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Speicherort** einen Anzeigenamen für den virtuellen Collectorcomputer und das Inventarobjekt an, in dem der virtuelle Computer gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf dem der virtuelle Collectorcomputer ausgeführt wird.
7. Geben Sie im Speicher das Speicherziel für den virtuellen Collectorcomputer an.
8. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
9. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der virtuelle Collectorcomputer eine Verbindung herstellt. Das Netzwerk benötigt eine Internetverbindung, damit Metadaten an Azure gesendet werden können.
10. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.

## <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie für die Appliance die Einstellungen für Sprache, Zeitzone und Kennwort an.
3. Klicken Sie auf dem Desktop auf die Verknüpfung **Collector ausführen**.
4. Öffnen Sie im Azure Migrate-Collector die Option **Erforderliche Komponenten einrichten**.
    - Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - Der Collector überprüft, ob der virtuelle Computer über Internetzugriff verfügt.
    - Wenn der virtuelle Computer über einen Proxy auf das Internet zugreift, klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an. Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert. [Erfahren Sie mehr](https://docs.microsoft.com/en-us/azure/migrate/concepts-collector#internet-connectivity) über die Anforderungen für Internetkonnektivität und die Liste der URLs, auf die der Collector zugreift.

    > [!NOTE]
    > Die Proxyadresse muss im Format http://ProxyIPAddress oder http://ProxyFQDN eingegeben werden. Es werden nur HTTP-Proxys unterstützt.

    - Der Collector überprüft, ob der Collector-Dienst ausgeführt wird. Der Dienst wird standardmäßig auf dem virtuellen Collectorcomputer installiert.
    - Laden Sie die VMware PowerCLI herunter, und installieren Sie sie.

5. Nehmen Sie in **vCenter Server-Details angeben** die folgenden Einstellungen vor:
    - Geben Sie den Namen (FQDN) oder die IP-Adresse des vCenter-Servers an.
    - Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das der Collector virtuelle Computer auf dem vCenter-Server ermittelt.
    - Wählen Sie unter **Sammlungsbereich** einen Bereich für die Ermittlung virtueller Computer aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Er sollte nicht mehr als 1500 VMs umfassen. [Erfahren Sie mehr](how-to-scale-assessment.md) über die Möglichkeiten zum Erkennen einer größeren Umgebung.

6. Geben Sie unter **Migrationsprojekt angeben** die ID und den Schlüssel für das Azure Migrate-Projekt an, die sie im Portal kopiert haben. Wenn Sie diese Angaben nicht kopiert haben, öffnen Sie das Azure-Portal über den virtuellen Collectorcomputer. Klicken Sie auf der Seite **Übersicht** des Projekts auf **Computer ermitteln**, und kopieren Sie die Werte.  
7. Überwachen Sie in **Sammlungsfortschritt anzeigen** die Ermittlung, und vergewissern Sie sich, dass sich die von den VMs erfassten Metadaten innerhalb des zulässigen Bereichs befinden. Der Collector gibt eine ungefähre Ermittlungszeit an. [Erfahren Sie mehr](https://docs.microsoft.com/en-us/azure/migrate/concepts-collector#what-data-is-collected) über die vom Azure Migrate-Collector gesammelten Daten.

> [!NOTE]
> Der Collector unterstützt nur „Englisch (USA)“ als Sprache des Betriebssystems und die Sprache der Collectorschnittstelle. Die Unterstützung für weitere Sprachen ist in Kürze verfügbar.


### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Die Ermittlungszeit hängt von der Anzahl der ermittelten virtuellen Computer ab. Normalerweise dauert der Abschluss der Ermittlung bei 100 virtuellen Computern ungefähr eine Stunde, nachdem der Collector ausgeführt wurde.

1. Klicken Sie im Migration Planner-Projekt auf **Verwalten** > **Computer**.
2. Überprüfen Sie, ob die zu ermittelnden virtuellen Computer im Portal angezeigt werden.


## <a name="create-and-view-an-assessment"></a>Erstellen und Anzeigen einer Bewertung

Nach der Ermittlung der virtuellen Computer gruppieren Sie sie und erstellen eine Bewertung.

1. Klicken Sie auf der Seite **Übersicht** des Projekts auf **+ Bewertung erstellen**.
2. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.
3. Erstellen Sie die Gruppe, und geben Sie einen Gruppennamen an.
4. Wählen Sie die Computer aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe und die Bewertung zu erstellen.
6. Zeigen Sie die Bewertung nach der Erstellung in **Übersicht** > **Dashboard** an.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.

### <a name="assessment-details"></a>Bewertungsdetails

Eine Bewertung enthält Informationen dazu, ob die lokalen virtuellen Computer mit Azure kompatibel sind, welche VM-Größe für die Ausführung des virtuellen Computers in Azure geeignet ist und wie hoch die voraussichtlichen monatlichen Azure-Kosten sind.

![Bewertungsbericht](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-Bereitschaft

Die Azure-Bereitschaftsansicht in der Bewertung gibt Aufschluss über den Bereitschaftsstatus der einzelnen virtuellen Computer. Jeder virtuelle Computer kann abhängig von seinen Eigenschaften eine der folgenden Kennzeichnungen erhalten:
- Bereit für Azure
- Bedingt bereit für Azure
- Nicht bereit für Azure
- Bereitschaft unbekannt

Für virtuelle Computer, die bereit sind, empfiehlt Azure Migrate eine VM-Größe in Azure. Die Größenempfehlung von Azure Migrate basiert auf dem in den Bewertungseigenschaften angegebenen Größenkriterium. Bei Verwendung des leistungsbasierten Größenkriteriums wird bei der Größenempfehlung der Leistungsverlauf der virtuellen Computer berücksichtigt. Bei Verwendung des Größenkriteriums „Wie lokal“ ist bei der Größenempfehlung für den virtuellen Computer in Azure die Größe des lokalen virtuellen Computers ausschlaggebend. Daten zur CPU- und Speicherauslastung des virtuellen Computers werden in Hinsicht auf die Größe des virtuellen Computers nicht berücksichtigt. Allerdings wird die Größe der Datenträger bei einer Größenanpassung vom Typ „Wie lokal“ unter Berücksichtigung der Leistungsdaten angepasst.  Weitere Informationen zur Größenanpassung in Azure Migrate finden Sie [hier](concepts-assessment-calculation.md).

Bei virtuellen Computern, die nicht oder nur bedingt für Azure bereit sind, informiert Azure Migrate über die Bereitschaftsprobleme sowie über mögliche Abhilfemaßnahmen.

Virtuelle Computer, für die Azure Migrate die Azure-Bereitschaft aufgrund fehlender Daten nicht bestimmen kann, werden mit „Bereitschaft unbekannt“ gekennzeichnet.

Neben den Angaben zu Azure-Bereitschaft und Größe schlägt Azure Migrate auch Tools vor, die Sie für die Migration des virtuellen Computers verwenden können. Dies erfordert eine tiefergehende Ermittlung in der lokalen Umgebung. Lesen Sie [hier](how-to-get-migration-tool.md), wie Sie durch Installieren von Agents auf den lokalen Computern eine tiefergehende Ermittlung durchführen können. Wenn die Agents nicht auf den lokalen Computern installiert sind, wird eine Lift & Shift-Migration mit [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) empfohlen. Wenn die Agents auf dem lokalen Computer installiert sind, untersucht Azure Migrate die auf dem Computer ausgeführten Prozesse und ermittelt, ob es sich um einen Datenbankcomputer handelt. Ist der Computer ein Datenbankcomputer, wird [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) empfohlen. Andernfalls wird Azure Site Recovery als Migrationstool empfohlen.

  ![Bewertungsbereitschaft](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Geschätzte monatliche Kosten

Diese Ansicht zeigt die gesamten Compute- und Speicherkosten für den Betrieb der virtuellen Computer in Azure zusammen mit den Details für die einzelnen Computer. Kostenschätzungen werden unter Berücksichtigung der Azure Migrate-Größenempfehlungen für einen Computer sowie unter Berücksichtigung seiner Datenträger und der Bewertungseigenschaften berechnet.

> [!NOTE]
> Die Kostenschätzung von Azure Migrate bezieht sich auf die Ausführung der lokalen virtuellen Computer als virtuelle Azure IaaS-Computer (Infrastructure-as-a-Service). Darin werden keine PaaS- (Platform-as-a-Service) oder SaaS-Kosten (Software-as-a-Service) berücksichtigt.

Die geschätzten monatlichen Kosten für Compute und Speicher werden für alle virtuellen Computer in der Gruppe aggregiert.

![Bewertung der Kosten für die virtuellen Computer](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Zuverlässigkeitsstufe

Jeder Bewertung in Azure Migrate wird eine Zuverlässigkeitsstufe zugeordnet. Die Skala reicht von einem Stern (niedrigster Wert) bis zu fünf Sternen (höchster Wert). Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind. Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen.

Bei Verwendung der leistungsbasierten Größenanpassung des virtuellen Computers benötigt Azure Migrate Nutzungsdaten für CPU und Arbeitsspeicher. Außerdem werden bei der Größenanpassung für jeden an den virtuellen Computer angefügten Datenträger Informationen zu den Lese-/Schreib-IOPS sowie zum Durchsatz benötigt. Analog dazu benötigt Azure Migrate für jeden Netzwerkadapter, der an den virtuellen Computer angefügt ist, Informationen zu ein- und ausgehenden Netzwerkdaten, um die leistungsbasierte Größenanpassung durchführen zu können. Steht eine der oben aufgeführten Nutzungsangaben in vCenter Server nicht zur Verfügung, ist die Größenempfehlung von Azure Migrate unter Umständen nicht zuverlässig. Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent), wie es nachfolgend angegeben ist:

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0 % bis 20 % | Ein Stern
   21 % bis 40 % | Zwei Sterne
   41 % bis 60 % | Drei Sterne
   61 % bis 80 % | Vier Sterne
   81 % bis 100 % | Fünf Sterne

Dass für eine Bewertung nicht alle Datenpunkte verfügbar sind, kann folgende Ursachen haben:
- Die Statistikeinstellung in vCenter Server ist nicht auf Stufe 3 festgelegt. Wenn die Statistikeinstellung in vCenter Server auf einen Wert unter 3 festgelegt ist, werden von vCenter Server keine Leistungsdaten für Datenträger und Netzwerk erfasst. In diesem Fall ist die Datenträger- und Netzwerkempfehlung von Azure Migrate nicht nutzungsbasiert. Da ohne Berücksichtigung der IOPS/des Durchsatzes nicht ermittelt werden kann, ob der Datenträger einen Premium-Datenträger in Azure benötigt, empfiehlt Azure Migrate Standarddatenträger für alle Datenträger.
- Die Statistikeinstellung in vCenter Server wurde vor dem Start der Ermittlung kurzzeitig auf Stufe 3 festgelegt. Ein Beispiel: Angenommen, Sie haben die Statistikeinstellung heute auf Stufe 3 festgelegt und starten die Ermittlung morgen (also 24 Stunden später) mithilfe der Collectorappliance. Wenn Sie eine Bewertung für einen einzelnen Tag erstellen, verfügen Sie über alle Datenpunkte, und die Zuverlässigkeitsstufe für die Bewertung beträgt fünf Sterne. Wenn Sie die Leistungsdauer in den Bewertungseigenschaften jedoch auf einen Monat festlegen, führt dies zu einer niedrigeren Zuverlässigkeitsstufe, da für den letzten Monat keine Datenträger- und Netzwerkleistungsdaten vorliegen. Wenn Sie die Leistungsdaten des letzten Monats berücksichtigen möchten, empfiehlt es sich, die Statistikeinstellung in vCenter Server vor Beginn der Ermittlung einen Monat lang auf Stufe 3 festzulegen.
- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn ein virtueller Computer für eine gewisse Zeit heruntergefahren wird, liegen vCenter Server für diesen Zeitraum keine Leistungsdaten vor.
- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, erstellt. Ein Beispiel: Angenommen, Sie erstellen eine Bewertung für den Leistungsverlauf des letzten Monats, und in der Umgebung wurden letzte Woche einige virtuelle Computer erstellt. In solchen Fällen liegt für die neuen virtuellen Computer kein Leistungsverlauf für den gesamten Zeitraum vor.

> [!NOTE]
> Bei einer Zuverlässigkeitsstufe von weniger als vier Sternen empfiehlt es sich, die Statistikeinstellungen in vCenter Server auf Stufe 3 festzulegen und mit der Ermittlung und Bewertung so lange zu warten, bis Daten für den gewünschten Zeitraum (ein Tag/eine Woche/ein Monat) vorliegen. Sollte das nicht möglich sein, ist die leistungsbasierte Größenanpassung möglicherweise nicht zuverlässig, und es empfiehlt sich, in den Bewertungseigenschaften stattdessen die Größenanpassung vom Typ *Wie lokal* festzulegen.

## <a name="next-steps"></a>Nächste Schritte

- [Informationen](how-to-modify-assessment.md) zum Anpassen einer Bewertung entsprechend Ihren Anforderungen
- Informationen zum Erstellen vertrauenswürdiger Bewertungsgruppen mithilfe der [Zuordnung von Computerabhängigkeiten](how-to-create-group-machine-dependencies.md)
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
- [Weitere Informationen](how-to-scale-assessment.md) zum Ermitteln und Bewerten einer umfangreichen VMware-Umgebung
