---
title: Hyperledger Fabric Consortium
description: Verwenden Sie die Projektmappenvorlage Hyperledger Fabric Consortium zum Bereitstellen und Konfigurieren eines Netzwerks mit einem einzelnen Element.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/29/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: c08557156848d4e7fcf0b1adbe6c8faa4ee00c82
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50231371"
---
# <a name="hyperledger-fabric-single-member-network"></a>Hyperledger Fabric-Einzelelement-Netzwerk

Sie können die Projektmappenvorlage Hyperledger Fabric Consortium zum Bereitstellen und Konfigurieren eines Hyperledger Fabric-Netzwerks mit einem einzelnen Element (mehrere Knoten) verwenden.

In diesem Artikel lernen Sie Folgendes:

- Kenntnisse im Umgang mit Blockchain, Hyperledger Fabric und komplizierteren Konsortiumsnetzwerk-Architekturen
- Bereitstellen und Konfigurieren eines Hyperledger Fabric-Konsortiumsnetzwerks mit einem einzelnen Member im Azure-Portal

## <a name="about-blockchain"></a>Über Blockchain

Neuen Mitgliedern der Blockchain-Community bietet diese Lösungsvorlage eine ausgezeichnete Möglichkeit, die Technologie auf einfache und konfigurierbare Weise in Azure kennenzulernen. Blockchain ist die Bitcoin zugrunde liegende Technologie. Sie ist allerdings viel mehr als nur ein Faktor für virtuelle Währungen. Es handelt sich um eine Kombination aus vorhandenen Technologien für Datenbanken, verteilte Systeme und Kryptografie, die eine sichere Berechnung durch mehrere Parteien und Garantien im Hinblick auf Unveränderlichkeit, Überprüfbarkeit, Nachverfolgbarkeit und Resilienz gegenüber Angriffen ermöglicht. Unterschiedliche Protokolle verwenden unterschiedliche Mechanismen, um diese Attribute bereitzustellen. [Hyperledger Fabric](https://github.com/hyperledger/fabric) ist ein solches Protokoll.

## <a name="consortium-architecture-on-azure"></a>Konsortiumsarchitektur bei Azure

Diese Vorlage stellt eine Topologie bereit, um Benutzer beim Testen und Simulieren der Produktion innerhalb einer einzelnen Organisation (einzelner Member) zu unterstützen. Diese Bereitstellung besteht aus einem Netzwerk mit mehreren Knoten in einer einzelnen Region, die bald auf mehrere Regionen ausgedehnt werden soll.

Das Netzwerk enthält drei Typen von Knoten:

1. **Memberknoten:** Knoten, auf dem der Fabric-Mitgliedschaftsdienst, der Mitglieder des Netzwerks registriert und verwaltet, ausgeführt wird. Dieser Knoten kann einem Cluster hinzugefügt werden, um Skalierbarkeit und Hochverfügbarkeit zu erzielen. In dieser Übung wird jedoch ein Einzelmemberknoten verwendet.
2. **Auftraggeberknoten:** Knoten, auf dem der Kommunikationsdienst zum Implementieren einer Zustellungsgarantie, z.B. als Gesamtauftragsübertragung oder als Einzeltransaktionen, ausgeführt wird.
3. **Peerknoten:** Knoten, der Transaktionen committet und den Status sowie eine Kopie des Distributed Ledger verwaltet.

## <a name="getting-started"></a>Erste Schritte

Damit Sie beginnen können, benötigen Sie ein Azure-Abonnement, das die Bereitstellung mehrerer virtueller Computer und Standardspeicherkonten unterstützen kann. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/).

Standardmäßig unterstützen die meisten Abonnementtypen eine kleine Bereitstellungstopologie und erfordern keine Kontingentvergrößerung. Die kleinstmögliche Bereitstellung für einen Member erfordert:

- 5 virtuelle Computer (5 Kerne)
- 1 VNET
- 1 Lastenausgleich
- 1 öffentliche IP-Adresse

Wenn Sie über ein Abonnement verfügen, wechseln Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie **+**, anschließend **Blockchain** und dann **Hyperledger Fabric Single Member Blockchain** (Hyperledger Fabric – Blockchain für Einzelmember) aus.

![Vorlage für Hyperledger Fabric-Blockchain für Einzelmember](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Bereitstellung

Wählen Sie zum Starten **Hyperledger Fabric Single Member Blockchain** (Hyperledger Fabric – Einzelmember-Blockchain) aus, und klicken Sie auf **Erstellen**, um das Blatt **Grundlagen** im Assistenten zu öffnen.

Während der Vorlagenbereitstellung werden Sie durch die Konfiguration des Netzwerks mit mehreren Knoten geführt. Der Ablauf für die Bereitstellung ist in drei Schritte unterteilt: Grundlagen, Netzwerkkonfiguration und Fabric-Konfiguration.

### <a name="basics"></a>Grundlagen

Geben Sie auf dem Blatt **Grundlagen** Werte für die Standardparameter für jede Bereitstellung an. Beispiele: Abonnement, Ressourcengruppe, grundlegende VM-Eigenschaften.

![Grundlagen](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Parametername| BESCHREIBUNG| Zulässige Werte|Standardwert
---|---|---|---
**Ressourcenpräfix**| Eine Zeichenfolge, die als Grundlage für die Benennung der bereitgestellten Ressourcen verwendet wird.|Max. 6 Zeichen|Nicht verfügbar
**Benutzername des virtuellen Computers**| Der Benutzername des Administrators für jeden bereitgestellten virtuellen Computer dieses Members.|1–64 Zeichen|azureuser
**Authentifizierungstyp**| Die Methode zur Authentifizierung des virtuellen Computers.|Kennwort oder öffentlicher SSH-Schlüssel|Kennwort
**Kennwort (Authentifizierungstyp = Kennwort)**|Das Kennwort für das Administratorkonto jedes bereitgestellten, virtuellen Computers. Das Kennwort muss drei der folgenden Arten von Zeichen umfassen: ein Großbuchstabe, ein Kleinbuchstabe, eine Ziffer und ein Sonderzeichen.<br /><br />Alle VMs haben zunächst dasselbe Kennwort, das nach der Bereitstellung jedoch geändert werden kann.|12–72 Zeichen|Nicht verfügbar
**SSH-Schlüssel (Authentifizierungstyp = öffentlicher Schlüssel)**|Der Secure Shell-Schlüssel für die Remoteanmeldung.||Nicht verfügbar
**Zugriff beschränken nach IP-Adresse**|Einstellung zur Festlegung, ob der Zugriff auf den Clientendpunkt beschränkt ist.|Ja/Nein| Nein 
**Zulässige IP-Adresse oder Subnetz (Zugriff beschränken nach IP-Adresse = Ja)**|Die IP-Adresse oder der Satz von IP-Adressen mit Zugriff auf den Clientendpunkt, wenn die Zugriffssteuerung aktiviert ist.||Nicht verfügbar
**Abonnement** |Das Abonnement für die Bereitstellung.
**Ressourcengruppe** |Die Ressourcengruppe, für die das Konsortiumsnetzwerk bereitgestellt wird.||Nicht verfügbar
**Location** |Die Azure-Region, in der der Netzwerkfußabdruck des ersten Members bereitgestellt wird.

### <a name="network-size-and-performance"></a>Netzwerkgröße und -leistung

Geben Sie als Nächstes unter **Netzwerkgröße und -leistung** Werte für die Größe des Konsortiumsnetzwerks ein. Beispiele: Anzahl von Mitgliedschafts-, Auftraggeber- und Peerknoten. Wählen Sie Infrastrukturoptionen und die Größe des virtuellen Computers aus.

![Netzwerkgröße und -leistung](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Parametername| BESCHREIBUNG| Zulässige Werte|Standardwert
---|---|---|---
**Anzahl der Mitgliedschaftsknoten**|Die Anzahl der Knoten, die den Mitgliedschaftsdienst ausführen. Weitere Informationen zum Mitgliedschaftsdienst finden Sie im Abschnitt zu Sicherheits- & Mitgliedschaftsdiensten in der [Dokumentation](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf) zu Hyperledger.<br /><br />Dieser Wert ist zurzeit auf einen Knoten beschränkt, es ist jedoch geplant, die horizontale Hochskalierung durch Clustering in der nächsten Version zu unterstützen.|1| 1
**Anzahl der Auftraggeberknoten** |Die Anzahl der Knoten, die Transaktionen in einen Block sortieren (organisieren). Weitere Informationen zum Sortierdienst finden Sie in der [Dokumentation](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html) zu Hyperledger.<br /><br />Dieser Wert ist zurzeit auf einen Knoten beschränkt. |1 |1
**Anzahl der Peerknoten**| Knoten, deren Besitzer das Konsortium ist und die Transaktionen ausführen und den Status einer Ledgerkopie verwalten.<br /><br />Weitere Informationen zum Sortierdienst finden Sie in der [Dokumentation](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html) zu Hyperledger.|3| 3–9
**Speicherleistung**|Der Typ des Speichers für die einzelnen bereitgestellten Knoten. Weitere Informationen zum Speicher finden Sie unter [Einführung in Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) und [Storage Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Standard oder Premium|Standard
**Größe des virtuellen Computers** |Die Größe des virtuellen Computers, die für alle Knoten im Netzwerk verwendet wird|Standard A,<br />Standard D,<br />Standard D-v2,<br />Standard F-Serie,<br />Standard DS,<br />und Standard FS|Standard D1_v2

### <a name="fabric-specific-settings"></a>Fabricspezifische Einstellungen

Geben Sie schließlich unter **Fabriceinstellungen** noch fabricspezifische Einstellungen an.

![Fabriceinstellungen](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Parametername| BESCHREIBUNG| Zulässige Werte|Standardwert
---|---|---|---
**Bootstrap-Benutzername**| Der anfängliche autorisierte Benutzer, der im Mitgliedschaftsdienst im bereitgestellten Netzwerk registriert wird.|Max. 9 Zeichen|admin
**Bootstrap-Benutzerkennwort für die Fabriczertifizierungsstelle**|Das Administratorkennwort zur Sicherung des in den Mitgliedschaftsknoten importierten Kontos der Fabriczertifizierungsstelle.<br /><br />Das Kennwort muss Folgendes enthalten: 1 Großbuchstaben, 1 Kleinbuchstaben und 1 Ziffer.|Min. 12 Zeichen|Nicht verfügbar

### <a name="deploy"></a>Bereitstellen

Überprüfen Sie unter **Zusammenfassung** die Eingaben, und führen Sie eine Standardüberprüfung vor der Bereitstellung durch.

![Zusammenfassung](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Lesen Sie rechtliche Hinweise und Datenschutzrichtlinien, und klicken Sie auf **Kauf**, um die Bereitstellung abzuschließen. Abhängig von der Anzahl an bereitgestellten virtuellen Computern dauert die Bereitstellung zwischen wenigen Minuten und längerer Zeit.

### <a name="accessing-nodes"></a>Zugreifen auf Knoten

Nachdem die Bereitstellung abgeschlossen ist, wird eine **Übersicht** angezeigt.

![Bereitstellungen](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Wenn der Bildschirm nicht automatisch angezeigt wird (da Sie eventuell während der Ausführung der Bereitstellung im Verwaltungsportal navigiert haben), können Sie ihn jederzeit auf der Registerkarte „Ressourcengruppen“ in der linken Navigationsleiste finden. Klicken Sie auf den Namen der Ressourcengruppe, den Sie in Schritt 1 eingegeben haben, und wechseln Sie zur Seite **Übersicht**.

In der Übersicht werden alle Ressourcen, die von der Lösungsvorlage bereitgestellt wurden, aufgelistet. Sie können sie beliebig untersuchen. Sie können von diesem Bildschirm aus aber auch auf die _Ausgabeparameter_, die von der Vorlage generiert wurden, zugreifen. Diese Ausgabeparameter enthalten nützliche Informationen für das Verbinden mit Ihrem Hyperledger Fabric-Netzwerk.

Klicken Sie für den Zugriff auf diese Ausgabeparameter zuerst auf dem Blatt „Ressourcengruppe“ auf die Registerkarte **Bereitstellungen**. Der Bereitstellungsverlauf wird angezeigt.

![Bereitstellungsverlauf](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

Klicken Sie im Bereitstellungsverlauf auf die erste Bereitstellung in der Liste, um die Details anzuzeigen.

![Bereitstellungsdetails](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

Die Detailbildschirm zeigt eine Zusammenfassung der Bereitstellung, gefolgt von drei nützlichen Ausgabeparametern:

- Den _API-ENDPOINT_ können Sie verwenden, nachdem Sie eine Anwendung im Netzwerk bereitgestellt haben.
- Das _PREFIX_ wird auch als _Bereitstellungspräfix_ bezeichnet. Es identifiziert Ihre Ressourcen und Ihre Bereitstellung eindeutig. Es wird für Befehlszeilentools verwendet.
- Mit _SSH-TO-FIRST-VM_ erhalten Sie einen vorab assemblierten SSH-Befehl mit den richtigen Parametern, die für eine Verbindung mit dem ersten virtuellen Computer in Ihrem Netzwerk erforderlich sind. Bei Hyperledger Fabric ist dies der Knoten mit der Fabriczertifizierungsstelle.

Sie können für jeden Knoten eine Remoteverbindung mit virtuellen Computern herstellen. Dazu verwenden Sie SSH mit dem angegebenen Administratorbenutzernamen und dem zugehörigen Kennwort/SSH-Schlüssel. Da die Knoten-VMs keine eigenen, öffentlichen IP-Adressen haben, müssen Sie den Lastenausgleich durchlaufen und eine Portnummer angeben. Der SSH-Befehl für den Zugriff auf dem ersten Transaktionsknoten ist die dritte Vorlagenausgabe **SSH-TO-FIRST-VM** (für die Beispielbereitstellung: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Um weitere Transaktionsknoten anzuzeigen, erhöhen Sie die Portnummer jeweils um 1 (Beispiel: Der erste Transaktionsnoten befindet sich an Port 3000, der zweite an 3001, der dritte an 3002 usw.).

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie sich auf die Entwicklung von Anwendungen und Chain Codes für Ihr Blockchain-Konsortiumsnetzwerk mit Hyperledger konzentrieren.
