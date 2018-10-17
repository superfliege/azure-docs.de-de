---
title: Ermitteln und Bewerten lokaler virtueller VMware-Computer für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie lokale virtuelle VMware-Computer für die Migration zu Azure mithilfe des Azure Migrate-Diensts ermittelt und bewertet werden.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 09/21/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b2bb6636aef9e26a81988d344f04f23c23ea1622
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161878"
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
- **vCenter Server-Konto**: Sie benötigen ein schreibgeschütztes Konto, um auf den vCenter Server zugreifen zu können. Dieses Konto wird in Azure Migrate zum Ermitteln der lokalen virtuellen Computer verwendet.
- **Berechtigungen:** Auf dem vCenter Server benötigen Sie Berechtigungen zum Erstellen eines virtuellen Computers durch Importieren einer Datei im OVA-Format.
- **Statistikeinstellungen**: Diese Voraussetzung gilt nur für das Modell für einmalige Ermittlung. Für die einmalige Ermittlung sollten die Statistikeinstellungen für vCenter Server vor der Bereitstellung auf Ebene 3 festgelegt werden. Bei einer niedrigeren Ebene als Ebene 3 wird die Bewertung zwar ausgeführt, die Leistungsdaten für den Speicher und das Netzwerk werden jedoch nicht erfasst. Die Größenempfehlungen in diesem Fall werden auf der Grundlage von Leistungsdaten für CPU und Arbeitsspeicher und Konfigurationsdaten für Datenträger- und Netzwerkadapter erstellt.

## <a name="create-an-account-for-vm-discovery"></a>Erstellen eines Kontos für die VM-Ermittlung

Azure Migrate benötigt Zugriff auf VMware-Server, um automatisch virtuelle Computer für die Bewertung ermitteln zu können. Erstellen Sie ein VMware-Konto mit folgenden Eigenschaften. Dieses Konto wird bei der Einrichtung von Azure Migrate angegeben.

- Benutzertyp: mindestens ein Benutzer mit Lesezugriff
- Berechtigungen: Data Center object (Rechenzentrumsobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle: schreibgeschützt)
- Details: Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.
- Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle „No access“ (Kein Zugriff) mit „Propagate to child object“ (An untergeordnetes Objekt weitergeben) zu.


## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-project"></a>Erstellen eines Projekts

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Suchen Sie nach **Azure Migrate**, und wählen Sie in den Suchergebnissen den Dienst **Azure Migrate** aus. Klicken Sie dann auf **Erstellen**.
3. Geben Sie einen Projektnamen und das Azure-Abonnement für das Projekt an.
4. Erstellen Sie eine neue Ressourcengruppe.
5. Geben Sie die Region an, in der das Projekt erstellt werden soll, und klicken Sie dann auf **Erstellen**. Sie können ein Azure Migrate-Projekt nur in den USA erstellen. Sie können Ihre Migration jedoch trotzdem für jeden beliebigen Zielort von Azure planen. Die für das Projekt angegebene Region wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden.

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als „Collectorappliance“ bezeichnet wird. Dieser virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Zum Einrichten der Collectorappliance laden Sie eine OVA-Datei herunter und importieren sie auf den lokalen vCenter-Server, um den virtuellen Computer zu erstellen.

1. Klicken Sie im Azure Migrate-Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. In **Computer ermitteln** stehen zwei Optionen für die Appliance zur Verfügung – klicken Sie zum Herunterladen der Ihren Anforderungen entsprechenden Appliance auf **Herunterladen**.

    a. **Einmalige Ermittlung:** Die Appliance für dieses Modell kommuniziert mit vCenter Server, um Metadaten über die virtuellen Computer zu sammeln. Für die Sammlung von Leistungsdaten der virtuellen Computer bezieht es sich auf die in vCenter Server gespeicherten historischen Leistungsdaten und sammelt den Leistungsverlauf des letzten Monats. In diesem Modell sammelt Azure Migrate durchschnittliche Leistungsindikatoren (im Vergleich zu Spitzenleistungsindikatoren) für jede Metrik, [Weitere Informationen] (https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected). Da es nur eine einmalige Ermittlung ist, spiegeln sich Änderungen in der lokalen Umgebung nicht wider, sobald die Ermittlung abgeschlossen ist. Wenn sich die Änderungen widerspiegeln sollen, müssen Sie eine neue Ermittlung derselben Umgebung für dasselbe Projekt durchführen.

    b. **Kontinuierliche Ermittlung:** Die Appliance für dieses Modell erstellt kontinuierlich Profile der lokalen Umgebung zum Sammeln von Echtzeit-Nutzungsdaten für jeden virtuellen Computer. In diesem Modell werden Spitzenleistungsindikatoren für jede Metrik (CPU-Auslastung, Arbeitsspeicherauslastung usw.) gesammelt. Dieses Modell ist nicht von den Statistikeinstellungen von vCenter Server für die Sammlung von Leistungsdaten abhängig. Sie können die kontinuierliche Profilerstellung jederzeit von der Appliance aus beenden.

    > [!NOTE]
    > Die Funktionalität der kontinuierlichen Ermittlung ist in der Vorschau.

3. Kopieren Sie in **Projektanmeldeinformationen kopieren** die Projekt-ID und den Projektschlüssel. Diese benötigen Sie beim Konfigurieren des Collectors.

    ![Herunterladen der OVA-Datei](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Überprüfen Sie, ob die OVA-Datei sicher ist, bevor Sie sie bereitstellen.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Der generierte Hash muss folgenden Einstellungen entsprechen.

#### <a name="one-time-discovery"></a>Einmalige Ermittlung

  Für OVA-Version 1.0.9.14

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
  SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
  SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

  Für OVA-Version 1.0.9.12

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | d0363e5d1b377a8eb08843cf034ac28a
  SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
  SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

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

#### <a name="continuous-discovery"></a>Kontinuierliche Ermittlung

  Für OVA-Version 1.0.10.4

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be

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
4. Klicken Sie auf der oberen Leiste der Collector-Benutzeroberfläche auf **Nach Updates suchen**, und vergewissern Sie sich, dass die neueste Version des Collectors ausgeführt wird. Andernfalls können Sie über den Link das neueste Upgradepaket herunterladen und den Collector aktualisieren.
5. Öffnen Sie im Azure Migrate-Collector die Option **Erforderliche Komponenten einrichten**.
    - Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - Der Collector überprüft, ob der virtuelle Computer über Internetzugriff verfügt.
    - Wenn der virtuelle Computer über einen Proxy auf das Internet zugreift, klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an. Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert. [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity) über die Anforderungen für Internetkonnektivität und die Liste der URLs, auf die der Collector zugreift.

    > [!NOTE]
    > Die Proxyadresse muss im Format http://ProxyIPAddress oder http://ProxyFQDN eingegeben werden. Es werden nur HTTP-Proxys unterstützt. Bei Verwendung eines abfangenden Proxys tritt bei der Internetverbindung unter Umständen zunächst ein Fehler auf, wenn Sie das Proxyzertifikat nicht importiert haben. Wie Sie dieses Problem beheben, indem Sie das Proxyzertifikat als vertrauenswürdiges Zertifikat auf dem virtuellen Collectorcomputer importieren, erfahren Sie [hier](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity-with-intercepting-proxy).

    - Der Collector überprüft, ob der Collector-Dienst ausgeführt wird. Der Dienst wird standardmäßig auf dem virtuellen Collectorcomputer installiert.
    - Laden Sie die VMware PowerCLI herunter, und installieren Sie sie.

6. Nehmen Sie in **vCenter Server-Details angeben** die folgenden Einstellungen vor:
    - Geben Sie den Namen (FQDN) oder die IP-Adresse des vCenter-Servers an.
    - Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das der Collector virtuelle Computer auf dem vCenter-Server ermittelt.
    - Wählen Sie unter **Sammlungsbereich** einen Bereich für die Ermittlung virtueller Computer aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Er sollte nicht mehr als 1500 VMs umfassen. [Erfahren Sie mehr](how-to-scale-assessment.md) über die Möglichkeiten zum Erkennen einer größeren Umgebung.

7. Geben Sie unter **Migrationsprojekt angeben** die ID und den Schlüssel für das Azure Migrate-Projekt an, die sie im Portal kopiert haben. Wenn Sie diese Angaben nicht kopiert haben, öffnen Sie das Azure-Portal über den virtuellen Collectorcomputer. Klicken Sie auf der Seite **Übersicht** des Projekts auf **Computer ermitteln**, und kopieren Sie die Werte.  
8. Überwachen Sie in **Sammlungsfortschritt anzeigen** den Ermittlungsstatus. Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected) über die vom Azure Migrate-Collector gesammelten Daten.

> [!NOTE]
> Der Collector unterstützt nur „Englisch (USA)“ als Sprache des Betriebssystems und die Sprache der Collectorschnittstelle.
> Wenn Sie die Einstellungen auf einem Computer ändern, auf den Sie zugreifen möchten, lösen Sie erneut die Ermittlung aus, bevor Sie die Bewertung ausführen. Verwenden Sie dazu im Collector die Option **Sammlung erneut starten**. Nachdem die Sammlung abgeschlossen ist, wählen Sie im Portal die Option **Neu berechnen** für die Bewertung aus, um die aktualisierten Bewertungsergebnisse zu erhalten.



### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Die Ermittlungszeit der einmaligen Ermittlunghängt von der Anzahl der ermittelten virtuellen Computer ab. In der Regel dauert es bei 100 virtuellen Computern nach Abschluss der Ausführung des Collectors ungefähr eine Stunde, bis die Konfiguration und Sammlung von Leistungsdaten abgeschlossen ist. Sie können sofort nach Abschluss der Ermittlung (leistungsbasierte und lokale) Bewertungen erstellen.

Für die (als Vorschau verfügbare) kontinuierliche Ermittlung stellt der Collector kontinuierlich das Profil der lokalen Umgebung her und sendet Leistungsdaten in einem stündlichen Intervall. Sie können die Computer im Portal eine Stunde nach Start der Ermittlung überprüfen. Sie sollten mindestens einen Tag warten, bevor Sie leistungsbasierte Bewertungen für die virtuellen Computer erstellen.

1. Klicken Sie im Migrationsprojekt auf **Verwalten** > **Computer**.
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

> [!NOTE]
> Bei der kontinuierlichen Ermittlung sollten Sie unbedingt nach dem Start der Ermittlung mindestens einen Tag warten, bevor Sie eine Bewertung erstellen. Eine vorhandene Bewertung können Sie mithilfe des Befehls **Neu berechnen** für die Bewertung mit den neuesten Leistungsdaten aktualisieren.

### <a name="assessment-details"></a>Bewertungsdetails

Eine Bewertung enthält Informationen dazu, ob die lokalen virtuellen Computer mit Azure kompatibel sind, welche VM-Größe für die Ausführung des virtuellen Computers in Azure geeignet ist und wie hoch die voraussichtlichen monatlichen Azure-Kosten sind.

![Bewertungsbericht](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-Bereitschaft

Die Azure-Bereitschaftsansicht in der Bewertung gibt Aufschluss über den Bereitschaftsstatus der einzelnen virtuellen Computer. Jeder virtuelle Computer kann abhängig von seinen Eigenschaften eine der folgenden Kennzeichnungen erhalten:
- Bereit für Azure
- Bedingt bereit für Azure
- Nicht bereit für Azure
- Bereitschaft unbekannt

Für virtuelle Computer, die bereit sind, empfiehlt Azure Migrate eine VM-Größe in Azure. Die Größenempfehlung von Azure Migrate basiert auf dem in den Bewertungseigenschaften angegebenen Größenkriterium. Bei Verwendung des leistungsbasierten Größenkriteriums wird bei der Größenempfehlung der Leistungsverlauf der virtuellen Computer (CPU und Arbeitsspeicher) sowie der Datenträger (IOPS und Durchsatz) berücksichtigt. Wenn als Größenkriterium „Wie lokal“ verwendet wird, berücksichtigt Azure Migrate keine Leistungsdaten für den virtuellen Computer und die Datenträger. Für die VM-Größenempfehlung in Azure wird die Größe des virtuellen Computers in der lokalen Umgebung herangezogen, und die Größe des virtuellen Computers orientiert sich an dem Speichertyp, der in den Bewertungseigenschaften angegeben ist (standardmäßig Premiumdatenträger). Weitere Informationen zur Größenanpassung in Azure Migrate finden Sie [hier](concepts-assessment-calculation.md).

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

Jeder leistungsbasierten Bewertung in Azure Migrate wird eine Zuverlässigkeitsstufe zugeordnet. Die Skala reicht von einem Stern (niedrigster Wert) bis zu fünf Sternen (höchster Wert). Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind. Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen. Die Zuverlässigkeitsstufe gilt nicht für lokale Bewertungen.

Bei Verwendung der leistungsbasierten Größenanpassung benötigt Azure Migrate Nutzungsdaten für CPU und Arbeitsspeicher des virtuellen Computers. Außerdem werden für jeden an den virtuellen Computer angefügten Datenträger Informationen zu IOPS und Durchsatzdaten benötigt. Analog dazu benötigt Azure Migrate für jeden Netzwerkadapter, der an einen virtuellen Computer angefügt ist, Informationen zu ein- und ausgehenden Netzwerkdaten, um die leistungsbasierte Größenanpassung durchführen zu können. Steht eine der oben aufgeführten Nutzungsangaben in vCenter Server nicht zur Verfügung, ist die Größenempfehlung von Azure Migrate unter Umständen nicht zuverlässig. Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent), wie es nachfolgend angegeben ist:

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0 % bis 20 % | Ein Stern
   21 % bis 40 % | Zwei Sterne
   41 % bis 60 % | Drei Sterne
   61 % bis 80 % | Vier Sterne
   81 % bis 100 % | Fünf Sterne

Dass für eine Bewertung nicht alle Datenpunkte verfügbar sind, kann folgende Ursachen haben:

**Einmalige Ermittlung**

- Die Statistikeinstellung in vCenter Server ist nicht auf Stufe 3 festgelegt. Da das Modell für einmalige Ermittlung von den Statistikeinstellungen in vCenter Server abhängt, werden von vCenter Server keine Leistungsdaten für Datenträger und Netzwerk erfasst, wenn die Statistikeinstellung in vCenter Server auf einen Wert unter 3 festgelegt ist. In diesem Fall ist die Datenträger- und Netzwerkempfehlung von Azure Migrate nicht nutzungsbasiert. Da ohne Berücksichtigung der IOPS/des Durchsatzes nicht ermittelt werden kann, ob der Datenträger einen Premium-Datenträger in Azure benötigt, empfiehlt Azure Migrate Standarddatenträger für alle Datenträger.
- Die Statistikeinstellung in vCenter Server wurde vor dem Start der Ermittlung kurzzeitig auf Stufe 3 festgelegt. Ein Beispiel: Angenommen, Sie haben die Statistikeinstellung heute auf Stufe 3 festgelegt und starten die Ermittlung morgen (also 24 Stunden später) mithilfe der Collectorappliance. Wenn Sie eine Bewertung für einen einzelnen Tag erstellen, verfügen Sie über alle Datenpunkte, und die Zuverlässigkeitsstufe für die Bewertung beträgt fünf Sterne. Wenn Sie die Leistungsdauer in den Bewertungseigenschaften jedoch auf einen Monat festlegen, führt dies zu einer niedrigeren Zuverlässigkeitsstufe, da für den letzten Monat keine Datenträger- und Netzwerkleistungsdaten vorliegen. Wenn Sie die Leistungsdaten des letzten Monats berücksichtigen möchten, empfiehlt es sich, die Statistikeinstellung in vCenter Server vor Beginn der Ermittlung einen Monat lang auf Stufe 3 festzulegen.

**Kontinuierliche Ermittlung**

- Sie haben für den Zeitraum, für den Sie die Bewertung erstellen, kein Profil Ihrer Umgebung erstellt. Wenn Sie z.B. die Bewertung mit einer auf 1 Tag festgelegten Leistungsdauer erstellen, müssen Sie bis mindestens einen Tag nach dem Start der Ermittlung warten, bis alle Datenpunkte eingesammelt sind.

**Häufige Gründe**  

- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn ein virtueller Computer für eine gewisse Zeit heruntergefahren wird, können wir für diesen Zeitraum keine Leistungsdaten sammeln.
- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, erstellt. Ein Beispiel: Angenommen, Sie erstellen eine Bewertung für den Leistungsverlauf des letzten Monats, und in der Umgebung wurden letzte Woche einige virtuelle Computer erstellt. In solchen Fällen liegt für die neuen virtuellen Computer kein Leistungsverlauf für den gesamten Zeitraum vor.

> [!NOTE]
> Bei einer Zuverlässigkeitsstufe von weniger als vier Sternen empfiehlt sich für das Modell für einmalige Ermittlung, die Statistikeinstellungen in vCenter Server auf Stufe 3 festzulegen und mit der Ermittlung und Bewertung so lange zu warten, bis Daten für den gewünschten Zeitraum (ein Tag/eine Woche/ein Monat) vorliegen. Warten Sie für das Modell für kontinuierliche Ermittlung mindestens einen Tag, damit die Appliance ein Profil der Umgebung erstellen und dann die Bewertung *Neu berechnen* kann. Sollte das nicht möglich sein, ist die leistungsbasierte Größenanpassung möglicherweise nicht zuverlässig, und es empfiehlt sich, in den Bewertungseigenschaften stattdessen die Größenanpassung vom Typ *Wie lokal* festzulegen.

## <a name="next-steps"></a>Nächste Schritte

- [Informationen](how-to-modify-assessment.md) zum Anpassen einer Bewertung entsprechend Ihren Anforderungen
- Informationen zum Erstellen vertrauenswürdiger Bewertungsgruppen mithilfe der [Zuordnung von Computerabhängigkeiten](how-to-create-group-machine-dependencies.md)
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
- [Weitere Informationen](how-to-scale-assessment.md) zum Ermitteln und Bewerten einer umfangreichen VMware-Umgebung
- [Weitere Informationen](resources-faq.md) zu den häufig gestellten Fragen für Azure Migrate
