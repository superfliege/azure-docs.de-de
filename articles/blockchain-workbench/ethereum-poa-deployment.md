---
title: Ethereum-PoA-Konsortium
description: Verwenden Sie die Projektmappe für „Ethereum – Proof-of-Authority-Konsortium“, um ein Multi-Member-Netzwerk für das Ethereum-Konsortium bereitzustellen und zu konfigurieren.
services: azure-blockchain
keywords: ''
author: CodyBorn
ms.author: coborn
ms.date: 8/2/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: vamelech
ms.openlocfilehash: fcb35f23be384b3625e4e17e5dc2285b7eeb341a
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39529481"
---
# <a name="ethereum-proof-of-authority-consortium"></a>Ethereum-PoA-Konsortium

[Diese Projektmappe](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) ist dafür ausgelegt, das Bereitstellen, Konfigurieren und Steuern eines Multi-Member-Netzwerks für das Ethereum-Proof-of-Authority-Konsortium zu erleichtern, ohne dass ausführliche Azure- und Ethereum-Kenntnisse vorhanden sein müssen.

Mit einigen wenigen Benutzereingaben und einer Bereitstellung per Klick über das Azure-Portal kann jedes Mitglied einen Netzwerk-Fußabdruck über Microsoft Azure Compute, Networking und weltweite Speicherdienste bereitstellen. Der Netzwerk-Fußabdruck jedes Mitglieds umfasst eine Gruppe von Validierungsknoten mit Lastenausgleich, über die eine Anwendung oder ein Benutzer interagieren kann, um Ethereum-Transaktionen zu übermitteln.

## <a name="concepts"></a>Konzepte

### <a name="terminology"></a>Begriff

-   **Konsens**: Das Synchronisieren von Daten über das verteilte Netzwerk per Blockvalidierung und -erstellung.

-   **Konsortiumsmitglieder**: Eine Entität, die am Konsens im Blockchain-Netzwerk beteiligt ist.

-   **Admin**: Ein Ethereum-Konto, das zum Verwalten der Beteiligung für ein bestimmtes Konsortiumsmitglied verwendet wird.

-   **Validierungselement**: Ein Computer, der einem Ethereum-Konto zugeordnet ist, das im Namen eines Administrators am Konsens beteiligt ist.

### <a name="proof-of-authority"></a>Proof-of-Authority

Neuen Mitgliedern der Blockchain-Community bietet diese Projektmappenversion eine ausgezeichnete Möglichkeit, die Technologie auf einfache und konfigurierbare Weise in Azure kennenzulernen. Proof-of-Work ist ein Sybil-Widerstandsmechanismus, mit dem Berechnungskosten genutzt werden, um eine Selbstregulierung des Netzwerks und eine faire Beteiligung zu ermöglichen. Dies funktioniert sehr gut in anonymen, offenen Blockchain-Netzwerken, in denen der Wettbewerb um die Kryptowährung die Sicherheit im Netzwerk fördert. In privaten Netzwerken oder Konsortiumsnetzwerken hat das zugrunde liegende Ether-Guthaben aber keinen Wert. Ein alternatives Protokoll (Proof-of-Authority) ist besser für berechtigungsbasierte Netzwerke geeignet, bei denen alle Konsensteilnehmer bekannt sind und als vertrauenswürdig angesehen werden. Ohne dass Mining erforderlich ist, ist Proof-of-Authority effizienter, und die Byzantinischer-Fehler-Toleranz ist trotzdem gewährleistet.

### <a name="consortium-governance"></a>Konsortium-Governance

Da Proof-of-Authority auf einer berechtigungsbasierten Liste mit Netzwerkautoritäten basiert, um das Netzwerk frei von Fehlern zu halten, ist Folgendes wichtig: Es muss ein fairer Mechanismus vorhanden sein, der zum Vornehmen von Änderungen an dieser Berechtigungsliste verwendet werden kann. Jede Bereitstellung verfügt über eine Gruppe von Smart Contracts und ein Portal für On-Chain-Governance-Maßnahmen für diese berechtigungsbasierte Liste. Nachdem eine vorgeschlagene Änderung bei den Konsortiumsmitgliedern die Stimmenmehrheit erzielt hat, wird die Änderung durchgeführt. Auf diese Weise können neue Konsensteilnehmer hinzugefügt oder kompromittierte Teilnehmer auf transparente Weise entfernt werden, um für Ehrlichkeit im Netzwerk zu sorgen.

### <a name="admin-account"></a>Administratorkonto

Während dieser Bereitstellung der Proof-of-Authority-Knoten werden Sie zum Angeben einer Ethereum-Administratoradresse aufgefordert. Sie können mehrere unterschiedliche Mechanismen nutzen, um dieses Ethereum-Konto zu generieren und zu schützen. Nachdem diese Adresse dem Netzwerk als Autorität hinzugefügt wurde, können Sie dieses Konto nutzen, um sich am Governance-Prozess zu beteiligen. Dieses Administratorkonto wird auch verwendet, um die Konsensbeteiligung an die Validierungsknoten zu delegieren, die im Rahmen dieser Bereitstellung erstellt werden. Da nur die öffentliche Ethereum-Adresse verwendet wird, kann jeder Administrator seine privaten Schlüssel flexibel so schützen, dass die Vorgaben des gewünschten Sicherheitsmodells eingehalten werden.

### <a name="validator-node"></a>Validierungsknoten

Beim Proof-of-Authority-Protokoll übernehmen Validierungsknoten die Rolle der herkömmlichen Miningknoten. Jede Validierung verfügt über eine eindeutige Ethereum-Identität, die einer Smart Contract-Berechtigungsliste hinzugefügt wird. Wenn eine Validierung in dieser Liste enthalten ist, kann sie sich am Blockerstellungsprozess beteiligen. Weitere Informationen zu diesem Prozess finden Sie in der Parity-Dokumentation unter [Authority Round consensus](https://wiki.parity.io/Aura) (Authority Round-Konsens). Jedes Konsortiumsmitglied kann zwei oder mehr Validierungsknoten für fünf Regionen bereitstellen, um Georedundanz zu erzielen. Validierungsknoten kommunizieren mit anderen Validierungsknoten, um sich über den Zustand des zugrunde liegenden Distributed Ledger abzustimmen.
Um im Netzwerk die Möglichkeit zur fairen Beteiligung sicherzustellen, ist es für alle Konsortiumsmitglieder untersagt, mehr Validierungen als das erste Mitglied des Netzwerks zu verwenden (wenn das erste Mitglied drei Validierungen bereitstellt, kann jedes Mitglied nur maximal drei Validierungen nutzen).

### <a name="identity-store"></a>Identitätsspeicher

Da jedes Mitglied über mehrere Validierungsknoten verfügt, die gleichzeitig ausgeführt werden, und jeder Knoten über eine berechtigungsbasierte Identität verfügen muss, ist Folgendes wichtig: Die Validierungselemente müssen im Netzwerk auf sichere Weise eine eindeutige aktive Identität beschaffen können. Zu diesem Zweck haben wir einen Identitätsspeicher erstellt, der jeweils im Abonnement eines Mitglieds bereitgestellt wird und in dem die generierten Ethereum-Identitäten enthalten und geschützt sind. Bei der Bereitstellung generiert der Orchestrierungscontainer einen privaten Ethereum-Schlüssel für jedes Validierungselement und speichert ihn in Azure Key Vault. Bevor der Paritätsknoten gestartet wird, wird zuerst eine Lease für eine ungenutzte Identität abgerufen, um sicherzustellen, dass die Identität nicht von einem anderen Knoten übernommen wird. Die Identität wird für den Client bereitgestellt, der sie an die Autorität weitergibt, um mit dem Erstellen von Blöcken zu beginnen. Wenn es auf der Host-VM zu einem Ausfall kommt, wird die Lease der Identität freigegeben, damit ein Ersatzknoten die Identität zu einem späteren Zeitpunkt annehmen kann.

### <a name="bootnode-registrar"></a>Startknoten-Registrierungsstelle

Um eine einfache Verbindungsherstellung zu ermöglichen, hostet jedes Mitglied auf dem [API-Datenendpunkt](#data-api) einen Satz von Verbindungsinformationen. Zu diesen Daten gehört auch eine Liste mit Startknoten, die als Peeringknoten für das beitretende Mitglied bereitgestellt werden. Im Rahmen dieser Daten-API halten wir diese Liste mit Startknoten aktuell.

### <a name="bring-your-own-operator"></a>„Bring Your Own Operator“

Häufig möchte sich ein Konsortiumsmitglied an der Netzwerksteuerung beteiligen, aber nicht seine eigene Infrastruktur betreiben und warten müssen. Im Gegensatz zu herkömmlichen Systemen steht die Nutzung eines einzelnen Betreibers für das gesamte Netzwerk dem dezentralisierten Modell von Blockchain-Systemen entgegen. Anstatt einen zentralisierten zwischengeschalteten Anbieter in Anspruch zu nehmen, der ein Netzwerk betreibt, kann jedes Konsortiumsmitglied die Infrastrukturverwaltung an den Betreiber seiner Wahl delegieren. Dies ermöglich ein Hybridmodell, bei dem jedes Mitglied die Wahl hat, seine eigene Infrastruktur zu betreiben oder den Betrieb an einen Partner zu delegieren. Der Workflow für den delegierten Betrieb lautet wie folgt:

1.  Das **Konsortiumsmitglied** generiert eine Ethereum-Adresse (enthält privaten Schlüssel).

2.  Das **Konsortiumsmitglied** stellt die öffentliche Ethereum-Adresse für den **Betreiber** bereit.

3.  Der **Betreiber** stellt die PoA-Validierungsknoten bereit und konfiguriert sie, indem er die Azure Resource Manager-Lösung verwendet.

4.  Der **Betreiber** stellt den RPC und den Verwaltungsendpunkt für das **Konsortiumsmitglied** bereit.

5.  Das **Konsortiumsmitglied** verwendet seinen privaten Schlüssel, um eine Anforderung zu signieren, über die die vom **Betreiber** bereitgestellten Validierungsknoten akzeptiert werden, um die Beteiligung in seinem Namen zu ermöglichen.

![„Bring Your Own Operator“](./media/ethereum-poa-deployment-guide/bring-you-own-operator.png)

### <a name="azure-monitor"></a>Azure Monitor

Diese Lösung verfügt auch über Azure Monitor zum Nachverfolgen von Knoten- und Netzwerkstatistiken. Anwendungsentwickler haben so Einblick in die zugrunde liegende Blockchain, um Statistiken zur Blockgenerierung nachzuverfolgen. Netzwerkbetreiber können Azure Monitor nutzen, um Netzwerkausfälle anhand von Infrastrukturstatistiken und abfragbaren Protokollen schnell erkennen und verhindern zu können. Ausführlichere Informationen finden Sie unter [Dienstüberwachung](#service-monitoring).

### <a name="deployment-architecture"></a>Bereitstellungsarchitektur

#### <a name="description"></a>BESCHREIBUNG

Mit dieser Lösung kann ein Multi-Member-Netzwerk für Ethereum-Konsortien für eine oder mehrere Regionen bereitgestellt werden. Standardmäßig sind die RPC- und Peeringendpunkte über die öffentliche IP-Adresse zugänglich, um eine vereinfachte Konnektivität über Abonnements und Clouds zu ermöglichen. Wir empfehlen Ihnen, für Zugriffssteuerungen auf Anwendungsebene die [Parity-Verträge zur Berechtigungsvergabe](https://wiki.parity.io/Permissioning) zu nutzen. Wir unterstützen auch Netzwerke, die hinter VPNs mit VNET-Gateways für abonnementübergreifende Konnektivität bereitgestellt werden. Da diese Bereitstellungen komplexer sind, empfehlen wir Ihnen, zuerst mit dem öffentlichen IP-Modell zu beginnen.

#### <a name="consortium-member-overview"></a>Konsortiumsmitglieder – Übersicht

Jede Bereitstellung von Konsortiumsmitgliedern umfasst Folgendes:

-   VM-Skalierungsgruppen (VMSS) für die Ausführung der PoA-Validierungselemente

-   Azure Load Balancer für die Verteilung von RPC-, Peering- und Governance-DApp-Anforderungen

-   Azure Key Vault zum Schützen der Validierungselementidentitäten

-   Azure Storage zum Hosten von persistenten Netzwerkinformationen und Koordinieren des Leasens

-   Azure Monitor zum Aggregieren von Protokollen und Leistungsstatistiken

-   VNET-Gateway (optional) zum Zulassen von VPN-Verbindungen über private VNETs

![Bereitstellungsarchitektur](./media/ethereum-poa-deployment-guide/deployment-architecture.png)

Wir nutzen Docker-Container, um für Zuverlässigkeit und Modularität zu sorgen. Wir verwenden Azure Container Registry, um im Rahmen jeder Bereitstellung Images mit Versionsangabe zu hosten und bereitzustellen. Die Containerimages bestehen aus:

-   Orchestrator

    -   Einmalige Ausführung während der Bereitstellung

    -   Generiert Identitäten und Governance-Verträge

    -   Speichert Identitäten im Identitätsspeicher

-   Parity-Client

    -   Least die Identität vom Identitätsspeicher

    -   Ermittelt Peers und stellt die Verbindung damit her

-   EthStats-Agent

    -   Sammelt lokale Protokolle und Statistiken per RPC und sendet sie per Pushvorgang an Azure Monitor

-   Governance-DApp

    -   Webschnittstelle für die Interaktion mit Governance-Verträgen

## <a name="governance-dapp"></a>Governance-DApp

Das Herzstück von Proof-of-Authority ist die dezentralisierte Governance-Komponente. Die Governance-DApp umfasst vorab bereitgestellte Smart Contracts und eine Webanwendung, die zum Steuern der Autoritäten im Netzwerk verwendet werden.
Die Autoritäten sind in Administratoridentitäten und Validierungsknoten unterteilt.
Administratoren können die Teilnahme am Konsens an eine Reihe von Validierungsknoten delegieren. Außerdem können Administratoren andere Administratoren für das Netzwerk auswählen oder abwählen.

![Governance-DApp](./media/ethereum-poa-deployment-guide/governance-dapp.png)

-   **Dezentralisierte Governance**: Änderungen der Netzwerkautoritäten werden von ausgewählten Administratoren per On-Chain-Abstimmung verwaltet.

-   **Delegierung von Validierungselementen**: Autoritäten können ihre Validierungsknoten verwalten, die für die einzelnen PoA-Bereitstellungen eingerichtet werden.

-   **Überwachbarer Änderungsverlauf**: Jede Änderung wird für die Blockchain aufgezeichnet, um für Transparenz und Überwachbarkeit zu sorgen.

## <a name="how-to-guides"></a>Anleitungen

### <a name="deploy-ethereum-proof-of-authority"></a>Bereitstellen von Ethereum-Proof-of-Authority

Hier ist ein Beispiel für einen Bereitstellungsablauf mit mehreren Parteien angegeben:

1.  Drei Mitglieder generieren mit MetaMask jeweils ein Ethereum-Konto.

2.  *Mitglied A* stellt Ethereum-PoA bereit und gibt seine öffentliche Ethereum-Adresse an.

3.  *Mitglied A* stellt die Konsortiums-URL für *Mitglied B* und *Mitglied C* bereit.

4.  *Mitglied B* und *Mitglied C* stellen Ethereum-PoA bereit und geben ihre öffentliche Ethereum-Adresse und die Konsortiums-URL von *Mitglied A* an.

5.  *Mitglied A* wählt per Abstimmung *Mitglied B* als Administrator aus.

6.  *Mitglied A* und *Mitglied B* wählen beide *Mitglied C* als Administrator aus.

Für diesen Prozess ist ein Azure-Abonnement erforderlich, das dafür ausgelegt ist, die Bereitstellung mehrerer VM-Skalierungsgruppen und verwalteter Datenträger zu unterstützen. [Erstellen Sie ggf. ein kostenloses Azure-Konto](https://azure.microsoft.com/free/), um zu beginnen.

Sobald ein Abonnement gesichert ist, wechseln Sie zum Azure-Portal. Wählen Sie „+“ > „Marketplace“ („Alle anzeigen“), und suchen Sie nach „Ethereum-PoA-Konsortium“.

Im folgenden Abschnitt wird das Konfigurieren des Netzwerk-Fußabdrucks für das erste Mitglied Schritt für Schritt beschrieben. Der Bereitstellungsablauf ist in fünf Schritte unterteilt: Grundlagen, Bereitstellungsregionen, Netzwerkgröße und -leistung, Ethereum-Einstellungen und Azure Monitor.

#### <a name="basics"></a>Grundlagen

Geben Sie unter **Grundlagen** Standardparameterwerte für eine beliebige Bereitstellung an, z.B. Abonnement, Ressourcengruppe und grundlegende VM-Eigenschaften.

Hier ist eine ausführliche Beschreibung der einzelnen Parameter angegeben:

Parametername|BESCHREIBUNG|Zulässige Werte|Standardwerte    
---|---|---|---
Neues Netzwerk erstellen oder vorhandenem Netzwerk beitreten?|Erstellen Sie ein neues Netzwerk, oder treten Sie einem bereits vorhandenen Konsortiumsnetzwerk bei.|Neu erstellen, Vorhandenem beitreten|Create New
E-Mail-Adresse (optional)|Sie erhalten eine E-Mail-Benachrichtigung mit Informationen zu Ihrer Bereitstellung, wenn die Bereitstellung abgeschlossen ist.|Gültige E-Mail-Adresse|Nicht verfügbar
VM-Benutzername|Der Benutzername des Administrators für jede bereitgestellte VM (nur alphanumerische Zeichen).|1 bis 64 Zeichen|Nicht verfügbar
Authentifizierungsart|Die Methode zur Authentifizierung des virtuellen Computers.|Kennwort oder öffentlicher SSH-Schlüssel|Password
Kennwort (Authentifizierungstyp = Kennwort)|Das Kennwort für das Administratorkonto jedes bereitgestellten, virtuellen Computers.  Das Kennwort muss 3 der folgenden Zeichen umfassen: 1 Großbuchstabe, 1 Kleinbuchstabe, 1 Ziffer und 1 Sonderzeichen. Alle VMs haben zunächst dasselbe Kennwort, das nach der Bereitstellung jedoch geändert werden kann.|12 bis 72 Zeichen|Nicht verfügbar
SSH-Schlüssel (Authentifizierungstyp = öffentlicher Schlüssel)|Der Secure Shell-Schlüssel für die Remoteanmeldung.||Nicht verfügbar
Abonnement|Das Abonnement, für das das Konsortiumsnetzwerk bereitgestellt wird.||Nicht verfügbar
Ressourcengruppe|Die Ressourcengruppe, für die das Konsortiumsnetzwerk bereitgestellt wird.||Nicht verfügbar
Standort|Die Azure-Region für die Ressourcengruppe.||Nicht verfügbar

Unten ist ein Beispiel für eine Bereitstellung angegeben: ![Blatt mit Grundlagen](./media/ethereum-poa-deployment-guide/basic-blade.png)

#### <a name="deployment-regions"></a>Bereitstellungsregionen

Geben Sie als Nächstes unter „Bereitstellungsregionen“ einen Wert für die Anzahl von Regionen ein, um das Konsortiumsnetzwerk und die Auswahl von Azure-Regionen auf Basis des angegebenen Werts bereitzustellen. Benutzer können in maximal fünf Regionen Bereitstellungen durchführen. Wir empfehlen Ihnen, die erste Region so auszuwählen, dass sie mit dem Ressourcengruppenstandort aus dem Abschnitt mit den Grundlagen übereinstimmt. Für Entwicklungs- oder Testnetzwerke wird eine einzelne Region pro Mitglied empfohlen. Für die Produktion ist es ratsam, die Bereitstellung in mindestens zwei Regionen durchzuführen, um Hochverfügbarkeit sicherzustellen.

Hier ist eine ausführliche Beschreibung der einzelnen Parameter angegeben:

  Parametername|BESCHREIBUNG|Zulässige Werte|Standardwerte
  ---|---|---|---
  Anzahl der Regionen|Die Anzahl der Regionen, in denen das Konsortiumsnetzwerk bereitgestellt wird.|1, 2, 3, 4, 5|1
  Erste Region|Die erste Region, in der das Konsortiumsnetzwerk bereitgestellt wird.|Alle zulässigen Azure-Regionen|Nicht verfügbar
  Zweite Region|Die zweite Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „2“ ausgewählt wurde).|Alle zulässigen Azure-Regionen|Nicht verfügbar
  Dritte Region|Die dritte Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „3“ ausgewählt wurde).|Alle zulässigen Azure-Regionen|Nicht verfügbar
  Vierte Region|Die vierte Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „4“ ausgewählt wurde).|Alle zulässigen Azure-Regionen|Nicht verfügbar
  Fünfte Region|Die fünfte Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „5“ ausgewählt wurde).|Alle zulässigen Azure-Regionen|Nicht verfügbar

Hier ist ein Beispiel für eine Bereitstellung angegeben: ![Bereitstellungsregionen](./media/ethereum-poa-deployment-guide/deployment-regions.png)

#### <a name="network-size-and-performance"></a>Netzwerkgröße und -leistung 

Geben Sie als Nächstes unter „Netzwerkgröße und -leistung“ einen Wert für die Größe des Konsortiumsnetzwerks ein, z.B. Anzahl und Größe von Validierungsknoten.
Die Speichergröße des Validierungsknotens gibt die potenzielle Größe der Blockchain vor. Dies kann nach der Bereitstellung geändert werden.

Hier ist eine ausführliche Beschreibung der einzelnen Parameter angegeben:

  Parametername|BESCHREIBUNG|Zulässige Werte|Standardwerte
  ---|---|---|---
  Anzahl von Validierungsknoten mit Lastenausgleich|Anzahl von Validierungsknoten, die als Teil des Netzwerks bereitgestellt werden|2 - 15|2
  Speicherleistung von Validierungsknoten|Der Typ des verwalteten Datenträgers, der die einzelnen bereitgestellten Validierungsknoten unterstützt.|Standard oder Premium|Standard
  VM-Größe für Validierungsknoten|Die VM-Größe, die für Validierungsknoten verwendet wird.|Standard A, Standard D, Standard D-v2, Standard F-Serie, Standard DS und Standard FS|Standard D1 v2

Hier ist ein Beispiel für eine Bereitstellung angegeben: ![Netzwerkgröße und-leistung](./media/ethereum-poa-deployment-guide/network-size-and-performance.png)

#### <a name="ethereum-settings"></a>Ethereum-Einstellungen

Geben Sie als Nächstes unter „Ethereum-Einstellungen“ Ethereum-bezogene Konfigurationseinstellungen wie Netzwerk-ID und Kennwort des Ethereum-Kontos oder den Genesis-Block an.

Hier ist eine ausführliche Beschreibung der einzelnen Parameter angegeben:

  Parametername|BESCHREIBUNG|Zulässige Werte|Standardwerte
  ---|---|---|---
ID des Konsortiumsmitglieds|Die ID, die jedem Mitglied im Konsortiumsnetzwerk zugeordnet ist. Wird zur Konfiguration von IP-Adressräumen verwendet, um Konflikte zu vermeiden. Bei einem privaten Netzwerk sollte die Mitglieds-ID für verschiedene Organisationen in demselben Netzwerk eindeutig sein.  Eine eindeutige Mitglieds-ID ist erforderlich, auch dieselbe Organisation Bereitstellungen in mehreren Regionen vornimmt. Notieren Sie sich den Wert dieses Parameters, da Sie ihn mit anderen teilnehmenden Mitgliedern teilen müssen, um Konflikte auszuschließen.|0 - 255|Nicht verfügbar
Netzwerk-ID|Die Netzwerk-ID für das bereitgestellte Ethereum-Konsortiumsnetzwerk.  Jedes Ethereum-Netzwerk verfügt über eine eigene Netzwerk-ID, wobei 1 die ID für das öffentliche Netzwerk ist.|5–999.999.999|10101010
Ethereum-Administratoradresse|Die Adresse des Ethereum-Kontos, die für die Teilnahme am PoA-Governance-Prozess verwendet wird.  Es wird empfohlen, MetaMask zum Generieren einer Ethereum-Adresse zu verwenden.|42 alphanumerische Zeichen, beginnend mit 0x|Nicht verfügbar
Erweiterte Optionen|Erweiterte Optionen für Ethereum-Einstellungen|Aktivieren oder deaktivieren|Deaktivieren
Öffentliche IP-Adresse (Erweiterte Optionen = Aktivieren)|Stellt das Netzwerk hinter einem VNET-Gateway bereit und entfernt den Peeringzugriff. Wenn diese Option ausgewählt ist, müssen alle Mitglieder ein VNET-Gateway nutzen, damit die Verbindung kompatibel ist.|Öffentliche IP-Adresse – Privates VNET|Öffentliche IP-Adresse
Vertrag zu Transaktionsberechtigungen (Erweiterte Optionen = Aktivieren)|Bytecode für den Vertrag zur Berechtigungsvergabe für Transaktionen. Beschränkt die Smart Contract-Bereitstellung und -Ausführung auf eine berechtigungsbasierte Liste mit Ethereum-Konten.|Bytecode für Vertrag|Nicht verfügbar

Hier ist ein Beispiel für eine Bereitstellung angegeben: ![Ethereum-Einstellungen](./media/ethereum-poa-deployment-guide/ethereum-settings.png)

#### <a name="azure-monitor"></a>Azure Monitor

Auf dem Blatt „Azure Monitor“ können Sie eine Azure Monitor-Ressource für Ihr Netzwerk konfigurieren. Mit Azure Monitor werden nützliche Metriken und Protokolle aus dem Netzwerk gesammelt und angezeigt. So lassen sich Netzwerkintegrität oder Debugprobleme schnell überprüfen.

  Parametername|BESCHREIBUNG|Zulässige Werte|Standardwerte
  ---|---|---|---
Überwachung|Option zum Aktivieren von Azure Monitor|Aktivieren oder deaktivieren|Aktivieren
Herstellen einer Verbindung mit einer vorhandenen Log Analytics-Instanz|Erstellen einer neuen Log Analytics-Instanz als Teil von Azure Monitor oder Einbinden einer vorhandenen Instanz|„Neu erstellen“ oder „Vorhandener beitreten“|Neu erstellen
Azure Monitor-Standort (Mit vorhandener Azure Monitor-Instanz verbinden = Neu erstellen)|Region, in der die neue Azure Monitor-Instanz bereitgestellt wird|Alle Azure Monitor-Regionen|Nicht verfügbar
Vorhandene Log Analytics-Arbeitsbereichs-ID (Mit vorhandener Azure Monitor-Instanz verbinden = Vorhandener beitreten)|Arbeitsbereichs-ID der vorhandenen Log Analytics-Instanz||Nicht verfügbar
Vorhandener Log Analytics-Primärschlüssel (Mit vorhandener Azure Monitor-Instanz verbinden = Vorhandener beitreten)|Primärschlüssel, der zum Herstellen einer Verbindung mit der vorhandenen Azure Monitor-Instanz verwendet wird||Nicht verfügbar


Unten ist ein Beispiel für eine Bereitstellung angegeben: ![Azure Monitor](./media/ethereum-poa-deployment-guide/azure-monitor.png)

#### <a name="summary"></a>Zusammenfassung

Klicken Sie auf das Zusammenfassungsblatt, um die Eingaben zu überprüfen und eine Standardüberprüfung vor der Bereitstellung durchzuführen. Vor der Bereitstellung können Sie die Vorlage und die Parameter herunterladen.

Lesen Sie die rechtlichen Hinweise und Datenschutzrichtlinien, und klicken Sie auf „Kauf“, um die Bereitstellung abzuschließen. Wenn die Bereitstellung VNET-Gateways enthält, dauert die Bereitstellung bis zu 45 bis 50 Minuten.

#### <a name="post-deployment"></a>Nach der Bereitstellung

##### <a name="deployment-output"></a>Ausgabe der Bereitstellung 

Nachdem die Bereitstellung abgeschlossen ist, können Sie über die Bestätigungs-E-Mail oder das Azure-Portal auf die erforderlichen Parameter zugreifen. Unter diesen Parametern finden Sie Folgendes:

-   Ethereum-RPC-Endpunkt

-   Governance-Dashboard-URL

-   Azure Monitor-URL

-   Daten-URL

-   VNET-Gateway-Ressourcen-ID (optional)

##### <a name="confirmation-email"></a>Bestätigungs-E-Mail 

Wenn Sie eine E-Mail-Adresse angeben ([Abschnitt „Grundlagen“](#basics)), wird eine E-Mail an die E-Mail-Adresse gesendet, in der die Informationen zur Ausgabe der Bereitstellung enthalten sind.

![E-Mail zur Bereitstellung](./media/ethereum-poa-deployment-guide/deployment-email.png)

##### <a name="portal"></a>Portal

Nachdem die Bereitstellung erfolgreich abgeschlossen wurde und alle Ressourcen bereitgestellt wurden, können Sie die Ausgabeparameter in Ihrer Ressourcengruppe anzeigen.

1.  Navigieren Sie im Portal zu Ihrer Ressourcengruppe.

2.  Navigieren Sie zu *Bereitstellungen*.

3.  Wählen Sie die oberste Bereitstellung mit dem gleichen Namen wie Ihre Ressourcengruppe aus.

4.  Wählen Sie *Ausgaben*.

### <a name="growing-the-consortium"></a>Vergrößern des Konsortiums

Um Ihr Konsortium zu erweitern, müssen Sie zuerst eine Verbindung mit dem physischen Netzwerk herstellen.
Bei Verwendung der Bereitstellung, die auf der öffentlichen IP-Adresse basiert, ist dieser erste Schritt nahtlos. Bei einer Bereitstellung hinter einem VPN helfen Ihnen die Informationen im Abschnitt [Herstellen einer Verbindung für das VNET-Gateway](#connecting-vnet-gateways) weiter, was die Herstellung der Netzwerkverbindung in [Schritt 2](#step-2-new-admin-deployment) betrifft.

#### <a name="step-1-add-the-new-admin"></a>Schritt 1: Hinzufügen des neuen Administrators

1.  Erfassen Sie die öffentliche Ethereum-Adresse des neuen Administrators.

2.  Navigieren Sie zur Governance-DApp, und erstellen Sie einen neuen Administrator mit der Ethereum-Adresse und einem entsprechenden Alias.

3.  Benachrichtigen Sie die anderen vorhandenen Mitglieder der neuen Anforderung, damit sie in Bezug auf die Hinzufügung dieses neuen Administrators abstimmen können.

4.  Nachdem für die Abstimmung 51% erreicht wurden, wird das Mitglied als Administrator hinzugefügt.

![Hinzufügen des Administrators](./media/ethereum-poa-deployment-guide/adding-admin.png)

#### <a name="step-2-new-admin-deployment"></a>Schritt 2: Bereitstellung des neuen Administrators

1.  Stellen Sie für das beitretende Mitglied die folgenden Informationen bereit. Diese Informationen finden Sie in Ihrer E-Mail, die Sie nach der Bereitstellung erhalten, oder in der Ausgabe zur Bereitstellung im Portal.

    -  URL der Konsortiumsdaten

    -  Anzahl von bereitgestellten Knoten

    -  VNET-Gateway-Ressourcen-ID (bei Verwendung von VPN)

2.  Das bereitstellende Mitglied sollte beim Bereitstellen seiner Netzverfügbarkeit [dieselbe Lösung](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) verwenden und Folgendes beachten:

    -  Wählen Sie *Vorhandener beitreten*.

    -  Wählen Sie die gleiche Anzahl von Validierungsknoten wie die anderen Mitglieder des Netzwerks, um eine faire Verteilung sicherzustellen.

    -  Verwenden Sie dieselbe Ethereum-Adresse, die im [vorherigen Schritt](#step-1-add-the-new-admin) angegeben wurde.

    -  Übergeben Sie auf der Registerkarte mit den *Ethereum-Einstellungen* die *URL der Konsortiumsdaten*.

    -  Wählen Sie unter „Erweitert“ die Option *Private VNet* (Privates VNET), wenn der restliche Teil des Netzwerks hinter einem VPN angeordnet ist.

#### <a name="step-3-delegate-validators"></a>Schritt 3: Delegieren von Validierungselementen

Der neue Administrator muss die Validierungselemente autorisieren, um in deren Namen teilnehmen zu können. Dies kann nicht automatisch erfolgen, da die Schlüssel nur vom Mitglied gesteuert werden.\*

_\*Die Knoten des ersten Mitglieds im Netzwerk können automatisch hinzugefügt werden, da wir deren Validierungsknoten vorab in den Genesis-Block im Netzwerk kompilieren können._

Der neue Administrator muss die folgenden Schritte ausführen:

1.  Öffnen der Governance-DApp, die im Rahmen *seiner* Bereitstellung bereitgestellt wurde

2.  Navigieren zur Registerkarte „Validators“ (Validierungselemente)

3.  Auswählen der einzelnen Validierungselemente, die bereitgestellt wurden, und Klicken auf **Hinzufügen**

![Hinzufügen von Validierungselementen](./media/ethereum-poa-deployment-guide/adding-validators.png)

#### <a name="connecting-vnet-gateways"></a>Herstellen einer Verbindung für VNET-Gateways

Bei einem privaten Netzwerk werden die unterschiedlichen Mitglieder über VNET-Gatewayverbindungen verbunden. Bevor ein Mitglied dem Netzwerk beitreten und Transaktionsdatenverkehr anzeigen kann, muss ein vorhandenes Mitglied die Konfiguration auf seinem VPN-Gateway abschließen, um die Verbindung zu akzeptieren. Die Ethereum-Knoten des beitretenden Mitglieds werden also erst ausgeführt, nachdem die Verbindung hergestellt wurde. Es wird empfohlen, redundante Netzwerkverbindungen (Mesh) mit dem Konsortium herzustellen, um das Vorhandensein eines Single Point of Failure unwahrscheinlicher zu machen.

Nachdem das neue Mitglied bereitgestellt wurde, muss das vorhandene Mitglied die bidirektionale Verbindung herstellen, indem es eine VNET-Gatewayverbindung mit dem neuen Mitglied einrichtet. Hierfür benötigt das vorhandene Mitglied Folgendes:

1.  VNET-Gateway-ResourceId des Mitglieds, das die Verbindung herstellt (siehe Ausgabe der Bereitstellung)

2.  Gemeinsam verwendeter Schlüssel für Verbindung

Das vorhandene Mitglied muss das folgende PowerShell-Skript ausführen, um die Verbindung herzustellen. Es wird empfohlen, Azure Cloud Shell zu verwenden (oben rechts in der Navigationsleiste im Portal).

![Cloud Shell](./media/ethereum-poa-deployment-guide/cloud-shell.png)

```bash
$MyGatewayResourceId = "<EXISTING_MEMBER_RESOURCEID>"
$OtherGatewayResourceId = "<NEW_MEMBER_RESOURCEID]"
$ConnectionName = "Leader2Member"
$SharedKey = "<NEW_MEMBER_KEY>"

## $myGatewayResourceId tells me what subscription I am in, what ResourceGroup and the VNetGatewayName
$splitValue = $MyGatewayResourceId.Split('/')
$MySubscriptionid = $splitValue[2]
$MyResourceGroup = $splitValue[4]
$MyGatewayName = $splitValue[8]

## $otherGatewayResourceid tells me what the subscription and VNet GatewayName are
$OtherGatewayName = $OtherGatewayResourceId.Split('/')[8]
$Subscription=Select-AzureRmSubscription -SubscriptionId $MySubscriptionid

## create a PSVirtualNetworkGateway instance for the gateway I want to connect to
$OtherGateway=New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
$OtherGateway.Name = $OtherGatewayName
$OtherGateway.Id = $OtherGatewayResourceId
$OtherGateway.GatewayType = "Vpn"
$OtherGateway.VpnType = "RouteBased"

## get a PSVirtualNetworkGateway instance for my gateway
$MyGateway = Get-AzureRmVirtualNetworkGateway -Name $MyGatewayName -ResourceGroupName $MyResourceGroup

## create the connection
New-AzureRmVirtualNetworkGatewayConnection -Name $ConnectionName -ResourceGroupName $MyResourceGroup -VirtualNetworkGateway1 $MyGateway -VirtualNetworkGateway2 $OtherGateway -Location $MyGateway.Location -ConnectionType Vnet2Vnet -SharedKey $SharedKey -EnableBgp $True
```

### <a name="service-monitoring"></a>Dienstüberwachung

Sie können auf Ihr Azure Monitor-Portal zugreifen, indem Sie entweder dem Link in der E-Mail zur Bereitstellung folgen oder in der Ausgabe der Bereitstellung nach dem Parameter \[OMS\_PORTAL\_URL\] suchen.

Im Portal werden zunächst eine allgemeine Netzwerkstatistik und eine Knotenübersicht angezeigt.

![Überwachungskategorien](./media/ethereum-poa-deployment-guide/monitor-categories.png)

Wenn Sie die **Knotenübersicht** auswählen, gelangen Sie zu einem Portal, in dem Sie Infrastrukturstatistiken pro Knoten anzeigen können.

![Knotenstatistiken](./media/ethereum-poa-deployment-guide/node-stats.png)

Wenn Sie **Netzwerkstatistik** wählen, wird die Ethereum-Netzwerkstatistik angezeigt.

![Netzwerkstatistik](./media/ethereum-poa-deployment-guide/network-stats.png)

#### <a name="sample-log-analytics-queries"></a>Log Analytics-Beispielabfragen

Hinter diesen Dashboards befindet sich eine Reihe von abfragbaren unformatierten Protokollen. Sie können diese unformatierten Protokolle verwenden, um die Dashboards anzupassen, Fehler zu untersuchen oder Schwellenwertwarnungen einzurichten. Hier sind einige Beispielabfragen angegeben, die im Tool für die Protokollsuche ausgeführt werden können:

##### <a name="lists-blocks-that-have-been-reported-by-more-than-one-validator-useful-to-help-find-chain-forks"></a>Listet Blöcke auf, die von mehr als einem Validierungselement gemeldet wurden Dient zum Finden von Kettenverzweigungen

```sql
MinedBlock_CL
| summarize DistinctMiners = dcount(BlockMiner_s) by BlockNumber_d, BlockMiner_s
| where DistinctMiners > 1
```

##### <a name="get-average-peer-count-for-a-specified-validator-node-averaged-over-5-minute-buckets"></a>Ruft die durchschnittliche Peeranzahl für einen angegebenen Validierungsknoten ab (Mittelwert für 5-Minuten-Buckets)

```sql
let PeerCountRegex = @"Syncing with peers: (\d+) active, (\d+) confirmed, (\d+)";
ParityLog_CL
| where Computer == "vl-devn3lgdm-reg1000001"
| project RawData, TimeGenerated
| where RawData matches regex PeerCountRegex
| extend ActivePeers = extract(PeerCountRegex, 1, RawData, typeof(int)) 
| summarize avg(ActivePeers) by bin(TimeGenerated, 5m)
```

### <a name="ssh-access"></a>SSH-Zugriff

Aus Sicherheitsgründen wird der SSH-Portzugriff durch eine Netzwerksicherheitsgruppen-Regel standardmäßig verweigert. Sie müssen diese Regel in \"Zulassen\" ändern, um auf die Instanzen der VM-Skalierungsgruppen im PoA-Netzwerk zugreifen zu können.

1.  Beginnen Sie im Azure-Portal im Abschnitt „Übersicht“ der bereitgestellten Ressourcengruppe.

    ![SSH-Übersicht](./media/ethereum-poa-deployment-guide/ssh-overview.png)

2.  Wählen Sie die Netzwerksicherheitsgruppe aus.

    ![SSH-NSG](./media/ethereum-poa-deployment-guide/ssh-nsg.png)

3.  Wählen Sie die Regel \"allow-ssh\" aus.

    ![ssh-allow](./media/ethereum-poa-deployment-guide/ssh-allow.png)

4.  Ändern Sie \"Aktion\" in „Zulassen“.

    ![SSH – Aktivieren von „Zulassen“](./media/ethereum-poa-deployment-guide/ssh-enable-allow.png)

5.  Klicken Sie auf \"Speichern\". (Es kann einige Minuten dauern, bis die Änderungen wirksam werden.)

Für die Validierungsknoten können Sie jetzt eine Remoteverbindung mit virtuellen Computern herstellen. Dazu verwenden Sie SSH mit dem angegebenen Administratorbenutzernamen und Kennwort/SSH-Schlüssel.
Der SSH-Befehl zum Zugreifen auf den ersten Validierungsknoten ist in der Ausgabe der Vorlagenbereitstellung unter dem Parameter „SSH\_TO\_FIRST\_VL\_NODE\_REGION1“ (für die Beispielbereitstellung: ssh - p 4000 poaadmin\@leader4vb.eastus.cloudapp.azure.com) aufgeführt. Um weitere Transaktionsknoten anzuzeigen, erhöhen Sie die Portnummer jeweils um 1 (Beispiel: Der erste Transaktionsnoten befindet sich an Port 4000).

Wenn Sie die Bereitstellung in mehr als einer Region durchgeführt haben, sollten Sie den obigen Befehl in den DNS-Namen oder die IP-Adresse des Lastenausgleichsmoduls dieser Region ändern. Um den DNS-Namen oder die IP-Adresse der anderen Regionen zu ermitteln, können Sie auf die Ressource mit der Namenskonvention \*\*\*\*\*-lbpip-reg\# zugreifen und die zugehörigen Eigenschaften für den DNS-Namen und die IP-Adresse anzeigen.

### <a name="azure-traffic-manager-load-balancing"></a>Azure Traffic Manager – Lastenausgleich

Mit Azure Traffic Manager lassen sich durch die Weiterleitung des eingehenden Datenverkehrs über mehrere Bereitstellungen in unterschiedlichen Regionen Ausfallzeiten reduzieren und die Reaktionsfähigkeit des PoA-Netzwerks verbessern. Durch integrierte Integritätsprüfungen und das automatische Neuberechnen von Routen wird die Hochverfügbarkeit für RPC-Endpunkte und die Governance-DApp sichergestellt. Dieses Feature ist nützlich, wenn Sie die Bereitstellung in mehreren Regionen durchgeführt haben und für die Produktion bereit sind.

Verwendungsmöglichkeiten für Traffic Manager:

-   Verbessern der Verfügbarkeit des PoA-Netzwerks mit automatischem Failover

-   Steigern Ihrer Reaktionsfähigkeit, indem Endbenutzer an den Azure-Standort mit der geringsten Netzwerklatenz geleitet werden

Wenn Sie sich dafür entscheiden, ein Traffic Manager-Profil zu erstellen, können Sie den DNS-Namen des Profils verwenden, um auf Ihr Netzwerk zuzugreifen. Nachdem dem Netzwerk weitere Konsortiumsmitglieder hinzugefügt wurden, kann Traffic Manager auch genutzt werden, um den Lastenausgleich für die bereitgestellten Validierungselemente zu laden.

#### <a name="creating-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils

Suchen Sie nach der Option \"Traffic Manager-Profil\", und wählen Sie sie aus, nachdem Sie im Azure-Portal auf die Schaltfläche \"Create a resource\" (Ressource erstellen) geklickt haben.

![Suchen nach Azure Traffic Manager](./media/ethereum-poa-deployment-guide/traffic-manager-search.png)

Geben Sie dem Profil einen eindeutigen Namen, und wählen Sie die Ressourcengruppe aus, die während der PoA-Bereitstellung erstellt wurde.

![Erstellen – Traffic Manager](./media/ethereum-poa-deployment-guide/traffic-manager-create.png)

Wählen Sie die Instanz nach der Bereitstellung in der Ressourcengruppe aus. Der DNS-Name für den Zugriff auf Traffic Manager befindet sich auf der Registerkarte „Übersicht“.

![Ermitteln des DNS-Namens für Traffic Manager](./media/ethereum-poa-deployment-guide/traffic-manager-dns.png)

Wählen Sie die Registerkarte „Endpunkte“ aus, und klicken Sie auf die Schaltfläche „Hinzufügen“. Ändern Sie anschließend den Typ der Zielressource in „Öffentliche IP-Adresse“. Wählen Sie anschließend die öffentliche IP-Adresse für den Lastenausgleich der ersten Region aus.

![Routing für Traffic Manager](./media/ethereum-poa-deployment-guide/traffic-manager-routing.png)

Wiederholen Sie dies für jede Region im bereitgestellten Netzwerk. Wenn sich die Endpunkte im Status \"Aktiviert\" befinden, wird dafür automatisch der Lastenausgleich und der Ausgleich nach Region für den DNS-Namen von Traffic Manager durchgeführt. Sie können diesen DNS-Namen jetzt anstelle des Parameters \[CONSORTIUM\_DATA\_URL\] in den anderen Schritten des Dokuments verwenden.

## <a name="data-api"></a>Daten-API

Jedes Konsortiumsmitglied hostet die erforderlichen Informationen, über die andere Personen eine Verbindung mit dem Netzwerk herstellen können. Das vorhandene Mitglied gibt die [CONSORTIUM_DATA_URL] vor der Bereitstellung des Mitglieds an. Bei der Bereitstellung ruft ein beitretendes Mitglied Informationen über die JSON-Schnittstelle am folgenden Endpunkt ab:

`<CONSORTIUM_DATA_URL>/networkinfo`

Die Antwort enthält Informationen, die für den Beitritt von Mitgliedern hilfreich sind (Genesis-Block, ABI für Validierungselementsatz-Vertrag, Startknoten), sowie Informationen, die für das vorhandene Mitglied hilfreich sind (Adressen der Validierungselemente). Wir empfehlen die Verwendung dieser Standardisierung, um das Konsortium auf mehrere Cloudanbieter zu erweitern. Diese API gibt eine Antwort im JSON-Format mit der folgenden Struktur zurück:
```json
{
  "$id": "", 
  "type": "object", 
  "definitions": {}, 
  "$schema": "http://json-schema.org/draft-07/schema#", 
  "properties": {
    "majorVersion": {
      "$id": "/properties/majorVersion", 
      "type": "integer", 
      "title": "This schema’s major version", 
      "default": 0, 
      "examples": [
        0
      ]
    }, 
    "minorVersion": {
      "$id": "/properties/minorVersion", 
      "type": "integer", 
      "title": "This schema’s minor version", 
      "default": 0, 
      "examples": [
        0
      ]
    }, 
    "bootnodes": {
      "$id": "/properties/bootnodes", 
      "type": "array", 
      "items": {
        "$id": "/properties/bootnodes/items", 
        "type": "string", 
        "title": "This member’s bootnodes", 
        "default": "", 
        "examples": [
          "enode://a348586f0fb0516c19de75bf54ca930a08f1594b7202020810b72c5f8d90635189d72d8b96f306f08761d576836a6bfce112cfb6ae6a3330588260f79a3d0ecb@10.1.17.5:30300", 
          "enode://2d8474289af0bb38e3600a7a481734b2ab19d4eaf719f698fe885fb239f5d33faf217a860b170e2763b67c2f18d91c41272de37ac67386f80d1de57a3d58ddf2@10.1.17.4:30300"
        ]
      }
    }, 
    "valSetContract": {
      "$id": "/properties/valSetContract", 
      "type": "string", 
      "title": "The ValidatorSet Contract Source", 
      "default": "", 
      "examples": [
        "pragma solidity 0.4.21;\n\nimport \"./SafeMath.sol\";\nimport \"./Utils.sol\";\n\ncontract ValidatorSet …"
      ]
    }, 
    "adminContract": {
      "$id": "/properties/adminContract", 
      "type": "string", 
      "title": "The AdminSet Contract Source", 
      "default": "", 
      "examples": [
        "pragma solidity 0.4.21;\nimport \"./SafeMath.sol\";\nimport \"./SimpleValidatorSet.sol\";\nimport \"./Admin.sol\";\n\ncontract AdminValidatorSet is SimpleValidatorSet { …"
      ]
    }, 
    "adminContractABI": {
      "$id": "/properties/adminContractABI", 
      "type": "string", 
      "title": "The Admin Contract ABI", 
      "default": "", 
      "examples": [
        "[{\"constant\":false,\"inputs\":[{\"name\":\"proposedAdminAddress\",\"type\":\"address\"},…"
      ]
    }, 
    "paritySpec": {
      "$id": "/properties/paritySpec", 
      "type": "string", 
      "title": "The Parity client spec file", 
      "default": "", 
      "examples": [
        "\n{\n \"name\": \"PoA\",\n \"engine\": {\n \"authorityRound\": {\n \"params\": {\n \"stepDuration\": \"2\",\n \"validators\" : {\n \"safeContract\": \"0x0000000000000000000000000000000000000006\"\n },\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\"\n }\n }\n },\n \"params\": {\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\",\n \"wasmActivationTransition\": \"0x0\"\n },\n \"genesis\": {\n \"seal\": {\n \"authorityRound\": {\n \"step\": \"0x0\",\n \"signature\": \"0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\"\n }\n },\n \"difficulty\": \"0x20000\",\n \"gasLimit\": \"0x2FAF080\"\n },\n \"accounts\": {\n \"0x0000000000000000000000000000000000000001\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ecrecover\", \"pricing\": { \"linear\": { \"base\": 3000, \"word\": 0 } } } },\n \"0x0000000000000000000000000000000000000002\": { \"balance\": \"1\", \"builtin\": { \"name\": \"sha256\", \"pricing\": { \"linear\": { \"base\": 60, \"word\": 12 } } } },\n \"0x0000000000000000000000000000000000000003\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ripemd160\", \"pricing\": { \"linear\": { \"base\": 600, \"word\": 120 } } } },\n \"0x0000000000000000000000000000000000000004\": { \"balance\": \"1\", \"builtin\": { \"name\": \"identity\", \"pricing\": { \"linear\": { \"base\": 15, \"word\": 3 } } } },\n \"0x0000000000000000000000000000000000000006\": { \"balance\": \"0\", \"constructor\" : \"…\" }\n }\n}"
      ]
    }, 
    "errorMessage": {
      "$id": "/properties/errorMessage", 
      "type": "string", 
      "title": "Error message", 
      "default": "", 
      "examples": [
        ""
      ]
    }, 
    "addressList": {
      "$id": "/properties/addressList", 
      "type": "object", 
      "properties": {
        "addresses": {
          "$id": "/properties/addressList/properties/addresses", 
          "type": "array", 
          "items": {
            "$id": "/properties/addressList/properties/addresses/items", 
            "type": "string", 
            "title": "This member’s validator addresses", 
            "default": "", 
            "examples": [
              "0x00a3cff0dccc0ecb6ae0461045e0e467cff4805f", 
              "0x009ce13a7b2532cbd89b2d28cecd75f7cc8c0727"
            ]
          }
        }
      }
    }
  }
}

```
## <a name="development"></a>Entwicklung

### <a name="programmatically-interacting-with-a-smart-contract"></a>Programmgesteuertes Interagieren mit einem Smart Contract

> [!WARNING]
> Senden Sie niemals Ihren privaten Ethereum-Schlüssel über das Netzwerk! Stellen Sie sicher, dass jede Transaktion zuerst lokal signiert wird und die signierte Transaktion über das Netzwerk gesendet wird.

Im folgenden Beispiel verwenden wir *ethereumjs-wallet*, um eine Ethereum-Adresse zu generieren, *ethereumjs-tx*, um lokal zu signieren, und *web3*, um die unformatierte Transaktion an den Ethereum-RPC-Endpunkt zu senden.

Für dieses Beispiel verwenden wir diesen einfachen Hello-World-Smart Contract:

```javascript
pragma solidity ^0.4.11;
contract postBox {
    string message;
    function postMsg(string text) public {
        message = text; 
    }
    function getMsg() public view returns (string) {
        return message;
    }
}
```

In diesem Beispiel wird davon ausgegangen, dass der Vertrag bereits bereitgestellt wurde. Sie können *solc* und *web3* nutzen, um einen Vertrag programmgesteuert bereitzustellen. Installieren Sie zuerst die folgenden Knotenmodule:
```
sudo npm install web3@0.20.2
sudo npm install ethereumjs-tx@1.3.6
sudo npm install ethereumjs-wallet@0.6.1
```
Mit diesem nodeJS-Skript wird Folgendes durchgeführt:

-   Erstellen einer unformatierten Transaktion: postMsg

-   Signieren der Transaktion mit dem generierten privaten Schlüssel

-   Übermitteln der signierten Transaktion an das Ethereum-Netzwerk

```javascript
var ethereumjs = require('ethereumjs-tx')
var wallet = require('ethereumjs-wallet')
var Web3 = require('web3')

// TODO Replace with your contract address
var address = "0xfe53559f5f7a77125039a993e8d5d9c2901edc58";
var abi = [{"constant": false,"inputs": [{"name": "text","type": "string"}],"name": "postMsg","outputs": [],"payable": false,"stateMutability": "nonpayable","type": "function"},{"constant": true,"inputs": [],"name": "getMsg","outputs": [{"name": "","type": "string"}],"payable": false,"stateMutability": "view","type": "function"}];

// Generate a new Ethereum account
var account = wallet.generate();
var accountAddress = account.getAddressString()
var privateKey = account.getPrivateKey();

 // TODO Replace with your RPC endpoint
 var web3 = new Web3(new Web3.providers.HttpProvider(
    "http://testzvdky-dns-reg1.eastus.cloudapp.azure.com:8545"));
 
// Get the current nonce of the account
 web3.eth.getTransactionCount(accountAddress, function (err, nonce) {
   var data = web3.eth.contract(abi).at(address).postMsg.getData("Hello World");
   var rawTx = {
     nonce: nonce,
     gasPrice: '0x00', 
     gasLimit: '0x2FAF080',
     to: address, 
     value: '0x00', 
     data: data
   }
   var tx = new ethereumjs(rawTx);
   
   tx.sign(privateKey);

   var raw = '0x' + tx.serialize().toString('hex');
   web3.eth.sendRawTransaction(raw, function (txErr, transactionHash) {
     console.log("TX Hash: " + transactionHash);
     console.log("Error: " + txErr);
   });
 });
```

### <a name="debug-a-smart-contract-truffle-develop"></a>Debuggen eines Smart Contract (Truffle Develop)

Truffle verfügt über ein lokales Entwicklungsnetzwerk, das zum Debuggen von Smart Contracts verfügbar ist. Das vollständige Tutorial finden Sie [hier](http://truffleframework.com/tutorials/debugging-a-smart-contract).

### <a name="webassembly-wasm-support"></a>Unterstützung von WebAssembly (WASM)

Die WebAssembly-Unterstützung ist für neu bereitgestellte PoA-Netzwerke bereits für Sie aktiviert. Sie ermöglicht die Smart Contract-Entwicklung in jeder Sprache, mit der die Transpilierung für Web-Assembly (Rust, C, C++) möglich ist. Unter den unten angegebenen Links finden Sie weitere Informationen.

-   Parity-Übersicht über WebAssembly: <https://wiki.parity.io/WebAssembly-Home>

-   Tutorial von Parity Tech: <https://github.com/paritytech/pwasm-tutorial>

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="i-notice-a-bunch-of-transactions-on-the-network-that-i-didnt-send-where-are-these-coming-from"></a>Mir sind im Netzwerk einige Transaktionen aufgefallen, die ich nicht gesendet habe. Woher kommen diese Transaktionen?

Es ist unsicher, die [persönliche API](https://web3js.readthedocs.io/en/1.0/web3-eth-personal.html) zu entsperren. Botkonten lauschen auf nicht gesperrte Ethereum-Konten und versuchen, das Guthaben abzuschöpfen. Der Bot nimmt an, dass diese Konten echtes Ether-Guthaben enthalten, und versucht, als Erster an das Guthaben zu gelangen. Sie sollten die persönliche API im Netzwerk nicht aktivieren. Signieren Sie die Transaktionen stattdessen vorab, indem Sie entweder manuell vorgehen und eine Wallet wie MetaMask verwenden, oder die programmgesteuerte Vorgehensweise wählen. Dies ist im Abschnitt „Programmgesteuertes Interagieren mit einem Smart Contract“ beschrieben.

### <a name="how-to-ssh-onto-a-vm"></a>Wie stelle ich eine SSH-Verbindung mit einem virtuellen Computer her?

Aus Sicherheitsgründen sperren wir den SSH-Port. Befolgen Sie [diese Anleitung zum Aktivieren des SSH-Ports](#_Accessing_Nodes_via).

### <a name="how-do-i-set-up-an-audit-member-or-transaction-nodes"></a>Wie richte ich ein Überwachungsmitglied oder einen Transaktionsknoten ein?

Bei Transaktionsknoten handelt es sich um eine Reihe von Parity-Clients, die als Peers im Netzwerk vorhanden sind, aber nicht am Konsens teilnehmen. Diese Knoten können weiterhin verwendet werden, um Ethereum-Transaktionen zu übermitteln und den Smart Contract-Zustand zu lesen.
Dies funktioniert gut als Mechanismus für das Bereitstellen der Überwachbarkeit für Netzwerkregulatoren. Führen Sie hierzu einfach Schritt 2 unter „Vergrößern des Konsortiums“ aus.

### <a name="why-are-metamask-transactions-taking-a-long-time"></a>Warum dauern MetaMask-Transaktionen so lange?

Um sicherzustellen, dass Transaktionen in der richtigen Reihenfolge empfangen werden, verfügt jede Ethereum-Transaktion über einen Nonce für die Inkrementierung. Wenn Sie ein Konto in MetaMask in einem anderen Netzwerk verwendet haben, müssen Sie den Nonce-Wert zurücksetzen. Klicken Sie auf das Symbol „Einstellungen“ (3 Balken) und dann auf „Einstellungen“ und „Konto zurücksetzen“. Der Transaktionsverlauf wird gelöscht, und Sie können die Transaktion erneut übermitteln.

### <a name="are-public-ip-deployments-compatible-with-private-network-deployments"></a>Sind Bereitstellungen mit öffentlicher IP-Adresse mit Bereitstellungen privater Netzwerke kompatibel?

Nein. Für das Peering ist eine bidirektionale Kommunikation erforderlich, sodass das gesamte Netzwerk entweder öffentlich oder privat sein muss.

## <a name="next-steps"></a>Nächste Schritte

Steigen Sie ein, indem Sie die Lösung [Ethereum-Proof-of-Authority-Konsortium](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) verwenden.
