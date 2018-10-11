---
title: Projektmappenvorlage für „Ethereum – Proof-of-Work-Konsortium“
description: Verwenden Sie die Projektmappenvorlage für „Ethereum – Proof-of-Work-Konsortium“, um ein Multi-Member-Netzwerk für das Ethereum-Konsortium bereitzustellen und zu konfigurieren.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 823bea9bac8ff270d5b5c02e3b76a2f7236c9c99
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48241013"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Projektmappenvorlage für „Ethereum – Proof-of-Work-Konsortium“

Mit der Projektmappenvorlage für „Ethereum – Proof-of-Work-Konsortium“ lässt sich ein entsprechendes Multi-Member-Netzwerk mit minimalen Kenntnissen in Azure und Ethereum schneller und einfacher bereitstellen und konfigurieren.

Einige wenige Benutzereingaben und eine Einfachklick-Bereitstellung über das Azure-Portal reichen, damit jedes Mitglied seinen Netzwerk-Fußabdruck über Microsoft Azure Compute, Networking und weltweite Speicherdienste bereitstellen kann. Der Netzwerk-Fußabdruck jedes Mitglieds umfasst eine Reihe von Transaktionsknoten mit Lastenausgleich, über die eine Anwendung oder ein Benutzer interaktiv Transaktionen übermitteln kann, sowie eine Reihe von Miningknoten zum Aufzeichnen von Transaktionen und ein VPN-Gateway. Über einen nachfolgenden Verbindungsschritt werden die Gateways verbunden, um ein vollständig konfiguriertes Multi-Member-Blockchain-Netzwerk zu erstellen.

## <a name="about-blockchain"></a>Über Blockchain

Neuen Mitgliedern der Blockchain-Community bietet diese Projektmappenversion eine ausgezeichnete Möglichkeit, die Technologie auf einfache und konfigurierbare Weise in Azure kennenzulernen. Für den Einstieg empfehlen wir jedoch die einfachere eigenständige Ethereum-Netzwerktopologie mit einer geführten, exemplarischen Vorgehensweise, bevor dann ein Multi-Member-Konsortiumsnetzwerk umgesetzt wird.

### <a name="mining-node-details"></a>Details zu Miningknoten

Knoten, die für das Mining bzw. Akzeptieren von Transaktionen zuständig sind, werden getrennt gehalten. So wird sichergestellt, dass zwei Aktionen nicht um dieselben Ressourcen konkurrieren.

Ein Konsortiumsmitglied kann bis zu fünf Regionen bereitstellen, die mindestens einen Miningknoten enthalten und von einem verwalteten Datenträger unterstützt werden. Mindestens ein Knoten in der Region wird als Startknoten konfiguriert, um die dynamische Erkennbarkeit der Knoten im Netzwerk zu unterstützen. Miningknoten kommunizieren mit anderen Miningknoten, um sich über den Zustand des zugrunde liegenden Distributed Ledger abzustimmen. Es ist nicht erforderlich, dass die Anwendung die Knoten erkennt oder mit ihnen kommuniziert. Durch die Fokussierung auf private Netzwerke sind die Miningknoten vom eingehenden, öffentlichen Internetdatenverkehr isoliert, wodurch eine zweite Schutzebene entsteht. Ausgehender Datenverkehr ist zulässig, jedoch nicht in Richtung Ethereum-Discoveryport.

Alle Knoten verfügen über eine stabile Version des Go Ethereum (Geth)-Clients und werden als Miningknoten konfiguriert. Wenn Sie keinen benutzerdefinierten Genesis-Block angegeben haben, verwenden alle Knoten dieselbe Ethereum-Adresse und dasselbe Schlüsselpaar, das durch das Kennwort des Ethereum-Kontos geschützt ist. Die angegebene Ethereum-Passphrase wird verwendet, um das Standardkonto (Coinbase) für die einzelnen Miningknoten zu generieren. Beim Miningvorgang können Miningknoten Gebühren verursachen, die diesem Konto hinzugerechnet werden.

Die Anzahl der Miningknoten pro Konsortiumsmitglied hängt von der gewünschten Gesamtgröße des Netzwerks und der Hashleistung ab, die den einzelnen Mitgliedern zugeteilt wurde. Je größer das Netzwerk, desto mehr Knoten müssen kompromittiert werden, um einen unfairen Vorteil zu erlangen. Die Vorlage unterstützt bis zu 15 Miningknoten pro Region, die über VM-Skalierungsgruppen bereitgestellt werden.

### <a name="transaction-node-details"></a>Details zu Transaktionsknoten

Ein Konsortiumsmitglied verfügt außerdem über eine Gruppe von Transaktionsknoten mit Lastenausgleich. Diese Knoten sind von außerhalb des virtuellen Netzwerks erreichbar, sodass sie von Anwendungen verwendet werden können, um Transaktionen zu übermitteln oder intelligente Verträge innerhalb des Blockchain-Netzwerks zu erfüllen. Alle Knoten verfügen über eine stabile Version des Go Ethereum (Geth)-Clients und werden so konfiguriert, dass sie eine vollständige Version des Distributed Ledger verwalten. Wenn kein benutzerdefinierter Genesis-Block angegeben wurde, verwenden diese Knoten dasselbe Ethereum-Konto (geschützt durch das Kennwort des Ethereum-Kontos).

Transaktionsknoten werden per Lastenausgleich in einer Verfügbarkeitsgruppe verteilt, um Hochverfügbarkeit sicherzustellen. Die Vorlage unterstützt bis zu fünf Transaktionsknoten, die über VM-Skalierungsgruppen bereitgestellt werden.

### <a name="log-analytics-details"></a>Details zu Log Analytics

Bei jeder Bereitstellung wird eine neue Log Analytics-Instanz erstellt oder der Beitritt zu einer vorhandenen Instanz vollzogen. Dies ermöglicht die Überwachung verschiedener Leistungsmetriken für einzelne virtuelle Computer, die das bereitgestellte Netzwerk bilden.

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur

### <a name="description"></a>BESCHREIBUNG

Mit dieser Projektmappenvorlage können Multi-Member-Netzwerke für Ethereum-Konsortien für eine oder mehrere Regionen bereitgestellt werden. Das virtuelle Netzwerk ist in den einzelnen Regionen über VNET-Gateways und Verbindungsressourcen mit anderen Regionen in einer Chaintopologie verbunden. Außerdem wird eine Registrierungsstelle bereitgestellt, die die erforderlichen Informationen aller Mining- und Transaktionsknoten enthält, die in den einzelnen Regionen bereitgestellt wurden.

### <a name="standalone-and-consortium-leader-overview"></a>Eigenständige und LEADER-Bereitstellung von Konsortien – Übersicht

![Eigenständige und LEADER-Bereitstellung von Konsortien – Übersicht](./media/ethereum-deployment/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>Beitritt zu Konsortiumsmitgliedern – Übersicht

![Beitritt zu Konsortiumsmitgliedern – Übersicht](./media/ethereum-deployment/member-overview.png)

## <a name="getting-started"></a>Erste Schritte

Für diesen Prozess ist ein Azure-Abonnement erforderlich, das dafür ausgelegt ist, die Bereitstellung mehrerer VM-Skalierungsgruppen und verwalteter Datenträger zu unterstützen. Erstellen Sie ggf. ein kostenloses Azure-Konto, um anzufangen.

Sobald ein Abonnement gesichert ist, wechseln Sie zum Azure-Portal. Wählen Sie **+ Ressource erstellen**, „Marketplace (alle anzeigen)“, und suchen Sie nach **Ethereum – Proof-of-Work-Konsortium**.

Während der Vorlagenbereitstellung werden Sie durch die Konfiguration des Netzwerk-Fußabdrucks für das erste Mitglied geführt. Der Bereitstellungsablauf ist in fünf Schritte unterteilt: Grundlagen, Operations Management Suite, Bereitstellungsregionen, Netzwerkgröße und -leistung sowie Ethereum-Einstellungen.

### <a name="basics"></a>Grundlagen

Geben Sie unter **Grundlagen** Standardparameterwerte für eine beliebige Bereitstellung an, z. B. Abonnement, Ressourcengruppe und grundlegende VM-Eigenschaften.

![Grundlagen](./media/ethereum-deployment/sample-deployment.png)

Parametername|BESCHREIBUNG| Zulässige Werte|Standardwerte
---|---|---|---
Neues Netzwerk erstellen oder vorhandenem Netzwerk beitreten?|Erstellen Sie ein neues Netzwerk, oder treten Sie einem bereits vorhandenen Konsortiumsnetzwerk bei.|Neu erstellen, Vorhandenem beitreten|Create New
Netzwerk bereitstellen, das Teil eines Konsortiums ist?|Bei einem Konsortiumsnetzwerk ist es zulässig, dass zukünftige Bereitstellungen diesem Netzwerk beitreten (wird angezeigt, wenn *Neu erstellen* oben ausgewählt ist).|Eigenständiges Konsortium|Eigenständig
Ressourcenpräfix |Eine Zeichenfolge, die als Basis für Namensressourcen verwendet wird (zwei bis vier alphanumerische Zeichen). Bei einigen Ressourcen ist der Zeichenfolge ein eindeutiger Hash vorangestellt, während ressourcenspezifischen Informationen angefügt werden.|Alphanumerische Zeichen (2 bis 4)|Nicht verfügbar
VM-Benutzername| Der Benutzername des Administrators für jede bereitgestellte VM (nur alphanumerische Zeichen).|1–64 Zeichen |gethadmin
Authentifizierungsart|Die Methode zur Authentifizierung des virtuellen Computers. |Kennwort oder öffentlicher SSH-Schlüssel|Kennwort
Kennwort (Authentifizierungstyp = Kennwort)|Das Kennwort für das Administratorkonto jedes bereitgestellten, virtuellen Computers. Das Kennwort muss 3 der folgenden Zeichen umfassen: 1 Großbuchstabe, 1 Kleinbuchstabe, 1 Ziffer und 1 Sonderzeichen. <br />Alle VMs haben zunächst dasselbe Kennwort, das nach der Bereitstellung jedoch geändert werden kann.|12–72 Zeichen|Nicht verfügbar
SSH-Schlüssel (Authentifizierungstyp = öffentlicher Schlüssel)|Der Secure Shell-Schlüssel für die Remoteanmeldung.|| Nicht verfügbar
Abonnement| Das Abonnement, für das das Konsortiumsnetzwerk bereitgestellt wird.||Nicht verfügbar
Ressourcengruppe| Die Ressourcengruppe, für die das Konsortiumsnetzwerk bereitgestellt wird.||Nicht verfügbar
Standort| Die Azure-Region für die Ressourcengruppe. ||Nicht verfügbar



### <a name="operations-management-suite"></a>Operations Management Suite

Das OMS (Operations Management Suite)-Blatt, über das Sie eine OMS-Ressource für das Netzwerk konfigurieren können. Mittels OMS werden nützliche Metriken und Protokolle aus dem Netzwerk gesammelt und angezeigt. So lassen sich Netzwerkintegrität oder Debugprobleme schnell überprüfen. Das kostenlose OMS-Angebot läuft kontrolliert aus, sobald die Kapazität aufgebraucht ist.

![Erstellen einer neuen OMS](./media/ethereum-deployment/new-oms.png)

Parametername|BESCHREIBUNG| Zulässige Werte|Standardwerte
---|---|---|---
Mit vorhandener OMS verbinden|Erstellen Sie eine neue Log Analytics-Instanz, oder treten Sie einer vorhandenen Instanz bei.|Neu erstellen, Vorhandener beitreten|Neu erstellen, Log Analytics-Standort|Die Region, in der die neue Log Analytics-Instanz bereitgestellt wird (wird angezeigt, wenn *Neu erstellen* ausgewählt ist).
Vorhandene OMS-Arbeitsbereichs-ID|Die Arbeitsbereichs-ID der vorhandenen Instanz (wird angezeigt, wenn *Vorhandener beitreten* ausgewählt ist). OMS-Dienstebene|Wählen Sie die Tarifstufe für die neue Instanz aus. Weitere Informationen finden Sie unter https://azure.microsoft.com/pricing/details/log-analytics/ (wird angezeigt, wenn *Vorhandener beitreten* ausgewählt ist).|Kostenlos, Eigenständig, Pro Knoten|Kostenlos
Vorhandener OMS-Primärschlüssel|Der Primärschlüssel, der für die Verbindung mit der vorhandenen OMS-Instanz verwendet wird (wird angezeigt, wenn *Vorhandener beitreten* ausgewählt ist).

### <a name="deployment-regions"></a>Bereitstellungsregionen

Geben Sie als Nächstes unter **Bereitstellungsregionen** einen Wert für **Anzahl der Regionen** ein, um das Konsortiumsnetzwerk und die Auswahl von Azure-Regionen auf Basis des angegebenen Werts bereitzustellen. Benutzer können in maximal fünf Regionen bereitstellen.

![Bereitstellungsregionen](./media/ethereum-deployment/deployment-regions.png)

Parametername| BESCHREIBUNG| Zulässige Werte |Standardwerte
---|---|---|---
Anzahl der Regionen| Die Anzahl der Regionen, in denen das Konsortiumsnetzwerk bereitgestellt wird.|1, 2, 3, 4, 5| 2
Erste Region| Die erste Region, in der das Konsortiumsnetzwerk bereitgestellt wird.|Alle zulässigen Azure-Regionen| USA (Westen)
Zweite Region |Die zweite Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „2“ ausgewählt wurde).|Alle zulässigen Azure-Regionen| USA (Ost)
Dritte Region| Die dritte Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „3“ ausgewählt wurde).|Alle zulässigen Azure-Regionen| USA (Mitte)
Vierte Region| Die vierte Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „4“ ausgewählt wurde).|Alle zulässigen Azure-Regionen| USA (Ost) 2
Fünfte Region| Die fünfte Region, in der das Konsortiumsnetzwerk bereitgestellt wird (wird nur angezeigt, wenn für die Anzahl der Regionen „5“ ausgewählt wurde).|Alle zulässigen Azure-Regionen| USA, Westen 2

### <a name="network-size-and-performance"></a>Netzwerkgröße und -leistung

Geben Sie als Nächstes unter **Netzwerkgröße und -leistung** einen Wert für die Größe des Konsortiumsnetzwerks ein, z. B. die Anzahl und Größe von Mining- und Transaktionsknoten.

![Netzwerkgröße und -leistung](./media/ethereum-deployment/network-size-performance.png)

Parametername |BESCHREIBUNG |Zulässige Werte| Standardwerte
---|---|---|---
Anzahl von Miningknoten|Die Anzahl der Miningknoten, die pro Region bereitgestellt werden.|2–15| 2
Speicherleistung von Miningknoten|Der Typ des verwalteten Datenträgers, der die einzelnen bereitgestellten Miningknoten unterstützt.|Standard oder Premium|Standard
VM-Größe von Miningknoten|Die VM-Größe, die für Miningknoten verwendet wird.|Standard A, <br />Standard D, <br />Standard D-v2, <br />Standard F-Serie, <br />Standard DS, <br />und Standard FS|Standard D1v2
Anzahl der Transaktionsknoten mit Lastenausgleich|Die Anzahl der Transaktionsknoten, die als Teil des Netzwerks bereitgestellt werden.|1–5| 2
Speicherleistung von Transaktionsknoten|Der Typ des verwalteten Datenträgers, der die einzelnen, bereitgestellten Transaktionsknoten unterstützt.|Standard oder Premium|Standard
VM-Größe von Transaktionsknoten|Die VM-Größe, die für Transaktionsknoten verwendet wird.|Standard A, <br />Standard D, <br />Standard D-v2, <br />Standard F-Serie, <br />Standard DS, <br />und Standard FS|Standard D1v2

### <a name="ethereum-settings"></a>Ethereum-Einstellungen

Geben Sie als Nächstes unter **Ethereum-Einstellungen** Ethereum-bezogene Konfigurationseinstellungen wie Netzwerk-ID und Kennwort des Ethereum-Kontos oder den Genesis-Block an.

![Ethereum-Einstellungen](./media/ethereum-deployment/standalone-leader-deployment.png)

Parametername |BESCHREIBUNG |Zulässige Werte|Standardwerte
---|---|---|---
ConsortiumMember-ID|Die ID, die jedem Mitglied im Konsortiumsnetzwerk zugeordnet ist. Wird zur Konfiguration von IP-Adressräumen verwendet, um Konflikte zu vermeiden. <br /><br />Die Mitglieds-ID sollte für verschiedene Organisationen im selben Netzwerk eindeutig sein. Eine eindeutige Mitglieds-ID ist erforderlich, auch dieselbe Organisation Bereitstellungen in mehreren Regionen vornimmt.<br /><br />Notieren Sie sich den Wert dieses Parameters, da Sie ihn mit anderen teilnehmenden Mitgliedern teilen müssen.|0–255
Ethereum-Netzwerk-ID|Die Netzwerk-ID für das bereitgestellte Ethereum-Konsortiumsnetzwerk. Jedes Ethereum-Netzwerk verfügt über eine eigene Netzwerk-ID, wobei 1 die ID für das öffentliche Netzwerk ist. Obwohl der Netzwerkzugriff für Miningknoten beschränkt ist, wird eine große Zahl empfohlen, um Konflikte zu vermeiden.|5–999.999.999| 10101010
Benutzerdefinierter Genesis-Block|Ein Genesis-Block kann automatisch generiert werden, oder es wird ein benutzerdefinierter Block angegeben.|Ja/Nein| Nein 
Kennwort des Ethereum-Kontos (Benutzerdefinierter Genesis-Block = Nein)|Das Administratorkennwort zur Sicherung des in jeden Knoten importierten Ethereum-Kontos. Das Kennwort muss Folgendes enthalten: 1 Großbuchstabe, 1 Kleinbuchstabe und 1 Ziffer.|Min. 12 Zeichen|Nicht verfügbar
Passphrase des privaten Ethereum-Schlüssels (Benutzerdefinierter Genesis Block = Nein)|Die Passphrase, die zum Generieren des privaten ECC-Schlüssels verwendet wird, der dem generierten Ethereum-Standardkonto zugeordnet ist. Ein vorab generierter, privater Schlüssel muss nicht explizit übergeben werden.<br /><br />Verwenden Sie eine Passphrase mit einer ausreichenden Zufälligkeitsstufe, um einen starken privaten Schlüssel zu erhalten, der keine Überlappung mit anderen Konsortiumsmitgliedern erzeugt. Die Passphrase muss mindestens Folgendes enthalten: 1 Großbuchstabe, 1 Kleinbuchstabe und 1 Ziffer.<br /><br />Hinweis: Wenn zwei Mitglieder dieselbe Passphrase verwenden, sind die generierten Konten identisch. Eine identische Passphrase ist hilfreich, wenn eine einzelne Organisation eine Bereitstellung über mehrere Regionen implementiert und ein einzelnes Konto (Coinbase) für alle Knoten freigegeben werden soll.|Min. 12 Zeichen|Nicht verfügbar
Genesis-Block (Benutzerdefinierter Genesis Block = Ja)|Die JSON-Zeichenfolge, die den benutzerdefinierten Genesis-Block darstellt. Weitere Informationen zum Format des Genesis-Blocks finden Sie hier unter „Benutzerdefinierte Netzwerke“.<br /><br />Auch wenn Sie einen benutzerdefinierten Genesis-Block angeben, wird ein Ethereum-Konto erstellt. Geben Sie ggf. ein vorab aufgeladenes Ethereum-Konto im Genesis-Block an, um nicht auf das Mining warten zu müssen.|Gültige JSON |Nicht verfügbar
Gemeinsam verwendeter Schlüssel für die Verbindung|Eine gemeinsam verwendeter Schlüssel für die Verbindung zwischen VNET-Gateways.| Min. 12 Zeichen|Nicht verfügbar
URL der Konsortiumsdaten|Die URL, die auf die relevanten Konsortiumkonfigurationsdaten verweist, die durch die Bereitstellung eines anderen Mitglieds zur Verfügung gestellt werden. <br /><br />Diese Informationen werden durch ein bereits verbundenes Mitglied bereitgestellt, das über eine Bereitstellung verfügt. Nachdem Sie das Netzwerk vollständig bereitgestellt haben, befindet sich die URL im Ausgabebereich CONSORTIUM-DATA der Vorlagenbereitstellung.||Nicht verfügbar
VNet-Gateway für die Verbindung|Der Ressourcenpfad des VNet-Gateways, mit dem eine Verbindung hergestellt werden soll.<br />Diese Informationen werden durch ein bereits verbundenes Mitglied bereitgestellt, das über eine Bereitstellung verfügt. Nachdem Sie das Netzwerk vollständig bereitgestellt haben, befindet sich die URL im Ausgabebereich CONSORTIUM_MEMBER_GATEWAY_ID der Vorlagenbereitstellung. Hinweis: Sie müssen die Konsortiumsdaten-URL und die VNet-Gatewayressource desselben Mitglieds verwenden.||Nicht verfügbar
Registrierungsstelle für den Endpunkt der Peerinformationen|Der Endpunkt für Peerinformationen, der von der Bereitstellung eines anderen Mitglieds angegeben wird.|Gültiger Endpunkt des ersten Mitglieds im Konsortium|Nicht verfügbar
Schlüssel der Registrierungsstelle für Peerinformationen|Der primäre Schlüssel für Peerinformationen, der von der Bereitstellung eines anderen Mitglieds angegeben wird.|Gültiger primärer Schlüssel des ersten Mitglieds im Konsortium|Nicht verfügbar

### <a name="summary"></a>Zusammenfassung

Klicken Sie auf das Zusammenfassungsblatt, um die Eingaben zu überprüfen und eine Standardüberprüfung vor der Bereitstellung durchzuführen.

![Zusammenfassung](./media/ethereum-deployment/summary.png)

Lesen Sie rechtliche Hinweise und Datenschutzrichtlinien, und klicken Sie auf **Kauf**, um die Bereitstellung abzuschließen. Wenn die Bereitstellung über mehr als eine Region verfügt oder für ein Konsortiumsnetzwerk gedacht ist, werden die erforderlichen VPN-Gateways von der Vorlage vorab bereitgestellt, um die Netzwerkkonnektivität mit anderen Mitgliedern zu unterstützen. Die Bereitstellung des Gateways kann 45 bis 50 Minuten dauern.

## <a name="post-deployment-sanity-checks"></a>Integritätsprüfungen nach der Bereitstellung

### <a name="administrator-page"></a>Administratorseite

Nachdem die Bereitstellung erfolgreich abgeschlossen und alle Ressourcen bereitgestellt wurden, können Sie die Administratorseite aufrufen, um das Blockchain-Netzwerk anzuzeigen und eine Integritätsprüfung für den Bereitstellungszustand durchzuführen. The URL der Administratorseite entspricht dem DNS-Namen des Lastenausgleichs. Sie befindet sich im Ausgabebereich ADMIN_SITE der Vorlagenbereitstellung.

Um die URL zu finden, wählen Sie die bereitgestellte Ressourcengruppe aus. Wählen Sie dann **Übersicht** aus, und klicken Sie auf den Link unter **Bereitstellungen**, über den die Anzahl der erfolgreichen Bereitstellungen angezeigt wird.

![Übersicht über Ressourcengruppen](./media/ethereum-deployment/resource-overview.png)

Der neue Bildschirm zeigt den Bereitstellungsverlauf. Wählen Sie die erste Bereitstellungsressource (z. B. microsoft-azure-blockchain.azure-blockchain-servi...) aus, und suchen Sie den Bereich **Ausgaben** in der unteren Seitenhälfte. Sie finden die URL der Administratorseite im Ausgabebereich ADMIN_SITE der Vorlagenbereitstellung.

![Ressourcenbereitstellungen](./media/ethereum-deployment/resource-deployments.png)

![Ausgabe der Bereitstellung](./media/ethereum-deployment/deployment-output.png)

Um die Administratorseite aufzurufen, kopieren Sie die Ausgabe **ADMIN-SITE**, und öffnen Sie sie in einer anderen Registerkarte.

Auf der Administratorseite finden Sie eine allgemeine Übersicht der bereitgestellten Topologie (im Abschnitt mit dem Status des Ethereum-Knotens). Die Seite enthält alle Hostnamen des Knotens, die Anzahl der Peers und den zuletzt angezeigten Block. Die Peeranzahl der einzelnen Knoten liegt zwischen dem Minimalwert 1 minus der *Gesamtknotenanzahl* und der konfigurierten maximalen Peeranzahl. Beachten Sie, dass die Anzahl der Konten, die im Netzwerk bereitgestellt werden können, durch die Peeranzahl nicht eingeschränkt werden.
Es kann vorkommen, dass die Peeranzahl schwankt und geringer als (Gesamtzahl der Knoten –1) ist. Diese Abweichung weist nicht zwangsläufig auf fehlerhafte Knoten hin. Vielmehr können geringfügige Abweichungen in der Peeranzahl durch Forks im Ledger verursacht werden. Schließlich können Sie den zuletzt angezeigten Block knotenweise im Netzwerk untersuchen, um festzustellen, ob es Forks oder Verzögerungen im System gibt.

![Knotenstatus](./media/ethereum-deployment/node-status.png)

Der Knotenstatus wird alle 10 Sekunden aktualisiert. Laden Sie die Seite über den Browser oder die Schaltfläche **Neu laden** erneut, um die Ansicht zu aktualisieren.

### <a name="oms-portal"></a>OMS-Portal

Sie können das OMS-Portal suchen, indem Sie den Link in der Bereitstellungsausgabe (OMSPORTALURL) aktivieren oder die OMS-Ressource in der bereitgestellten Ressourcengruppe auswählen.

![OMS-URL](./media/ethereum-deployment/oms-url.png)

![OMS-Ressource](./media/ethereum-deployment/oms-resource.png)

Im Portal wird zunächst eine allgemeine Netzwerkstatistik zu Übersichtszwecken angezeigt.

![OMS-Übersicht](./media/ethereum-deployment/oms-overview.png)

Wenn Sie auf die Übersicht klicken, werden Sie zu einem Portal mit einer knotenbasierten Statistik weitergeleitet.

![Statistik pro Knoten](./media/ethereum-deployment/per-node-stats.png)

### <a name="accessing-nodes"></a>Zugriff auf Knoten

Für Transaktionsknoten können Sie eine Remoteverbindung mit virtuellen Computern herstellen. Dazu verwenden Sie SSH mit dem angegebenen Administratorbenutzernamen und Kennwort/SSH-Schlüssel. Da die Transaktionsknoten-VMs keine eigenen, öffentlichen IP-Adressen haben, müssen Sie den Lastenausgleich durchlaufen und die Portnummer angeben. Welcher SSH-Befehl ausgeführt werden muss, um auf den ersten Transaktionsknoten zuzugreifen, können Sie dem Ausgabeparameter **SSH_TO_FIRST_TX_NODE** der Vorlagenbereitstellung entnehmen (Beispielbereitstellung: ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com). Um weitere Transaktionsknoten anzuzeigen, erhöhen Sie die Portnummer jeweils um 1 (Beispiel: Der erste Transaktionsnoten befindet sich an Port 4000).

Da kein externer Zugriff auf die virtuellen Computer möglich ist, auf denen die Miningknoten ausgeführt werden, müssen Sie einen der Transaktionsknoten verwenden. Nachdem Sie eine SSH-Sitzung für einen Transaktionsknoten erstellt haben, installieren Sie Ihren privaten Schlüssel für den Transaktionsknoten, oder verwenden Sie Ihr Kennwort, um eine SSH-Sitzung in einem beliebigen Miningknoten zu starten.

**Hinweis**

Die Hostnamen können von der Administratorwebsite oder aus dem Azure-Portal abgerufen werden. Im Azure-Portal sind die Hostnamen der Knoten aus der VM-Skalierungsgruppen-Ressource unter **Instanzen** aufgeführt und unterscheiden sich insofern von den tatsächlichen Hostnamen. Beispiel: Der Hostname im Azure-Portal kann **mn-asdfmv-reg1_0** lauten, während der tatsächliche Hostname **mn-asdfmv-reg** ist.

Beispiel: 

Hostname im Azure-Portal| Tatsächlicher Hostname
---|---
mn-ethwvu-reg1_0| mn-ethwvu-reg1000000
mn-ethwvu-reg1_1 |mn-ethwvu-reg1000001
mn-ethwvu-reg1_2 |mn-ethwvu-reg1000002

## <a name="adding-a-new-consortium-member"></a>Hinzufügen eines neuen Konsortiumsmitglieds

### <a name="sharing-data"></a>Freigeben von Daten

Wenn Sie das erste (oder ein verbundenes) Mitglied des Konsortiums sind, benötigen andere Mitglieder einige Informationen von Ihnen, damit sie beitreten und eine Verbindung herstellen können. Dies gilt insbesondere in folgenden Fällen:

1. **Freigegebene Konfigurationsdaten für das Konsortium**: Eine Gruppe von Daten sorgt dafür, die Ethereum-Verbindung zwischen zwei Mitglieder zu orchestrieren. Die erforderlichen Informationen, einschließlich Genesis-Block, Konsortiumsnetzwerk-ID und Startknoten, werden in eine Datei auf den Transaktionsknoten der LEADER- oder einer anderen MEMBER-Bereitstellung geschrieben. Den Speicherort der Datei können Sie dem Ausgabeparameter **CONSORTIUM-DATA** der Vorlagenbereitstellung entnehmen.
2. **Endpunkt für Peerinformationen**: Der Registrierungsstelle für den Endpunkt der Peerinformationen wird dazu verwendet, Informationen von allen Knoten abzurufen, die bereits mit dem Ethereum-Netzwerk der LEADER- oder einer anderen MEMBER-Bereitstellung verbunden sind. Die Datenbank speichert Informationen zu jedem Knoten, der mit dem Netzwerk verbunden ist, z. B. den Hostnamen des Knotens, die private IP-Adresse usw. Die Informationen können Sie dem Ausgabeparameter **PEER_INFO_ENDPOINT** der Vorlagenbereitstellung entnehmen.
3. **Primärer Schlüssel für Peerinformationen**: Der primäre Schlüssel für die Registrierungsstelle von Peerinformationen wird verwendet, um auf den primären Schlüssel für Peerinformationen der LEADER- oder einer anderen MEMBER-Bereitstellung zuzugreifen. Die Informationen können Sie dem Ausgabeparameter **PEER_INFO_PRIMARY_KEY** der Vorlagenbereitstellung entnehmen.


4. **VNET-Gateway**: Jedes Mitglied stellt über ein vorhandenes Mitglied eine Verbindung mit dem gesamten Blockchain-Netzwerk her. Um sich mit VNET zu verbinden, benötigen Sie einen Ressourcenpfad zum VNET-Gateway des Mitglieds, mit dem Sie eine Verbindung herstellen. Die Informationen können Sie dem Ausgabeparameter **CONSORTIUM_MEMBER_GATEWAY_ID** der Vorlagenbereitstellung entnehmen.
5. **Gemeinsam verwendeter Schlüssel**: Ein vorab festgelegter, geheimer Schlüssel zwischen zwei Mitgliedern des Konsortiumsnetzwerks, zwischen denen eine Verbindung hergestellt wird. Entspricht einer alphanumerischen Zeichenfolge (zwischen 1 und 128 Zeichen), die außerhalb des Kontexts der Bereitstellung vereinbart wurde. (Beispiel: **MySharedKeyAbc123**)

### <a name="acceptance-of-new-member"></a>Zulassung neuer Mitglieder

Dieser Schritt sollte ausgeführt werden, nachdem das beitretende Mitglied sein Netzwerk erfolgreich bereitgestellt hat. Bevor ein Mitglied dem Netzwerk beitreten und Transaktionsdatenverkehr anzeigen kann, muss ein vorhandenes Mitglied die Konfiguration auf seinem VPN Gateway abschließen, um die Verbindung zu akzeptieren. Die Ethereum-Knoten des beitretenden Mitglieds werden also erst ausgeführt, nachdem die Verbindung hergestellt wurde. Diese Konfiguration kann über PowerShell oder xPlat CLI erfolgen. Ein PowerShell-Modul und ein xPlat CLI-Skript werden ebenfalls zusammen mit den Konsortiumsdaten auf dem Transaktionsknoten gespeichert. Der Skriptspeicherort ist im Ausgabeparameter **PAIR-GATEWAY-PS-MODULE** bzw. **PAIR-GATEWAY-AZURE-CLISCRIPT** der Bereitstellung angegeben.

**PowerShell/CLI-Setup**

Zusätzliche Informationen zum Einstieg in Azure PowerShell-Cmdlets und Azure xPlat CLI finden Sie in der Azure-Dokumentation.

Die neueste Version der Azure-Cmdlets muss lokal installiert werden. Außerdem muss eine Sitzung geöffnet sein. Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Abonnements bei der Sitzung an.

**PowerShell: Verbindung herstellen**

Laden Sie das PowerShell-Modul herunter, und speichern Sie es lokal. Der Speicherort des PowerShell-Moduls ist im Ausgabeparameter **PAIR-GATEWAY-PS-MODULE** der Vorlagenbereitstellung angegeben.

Falls das Modul noch nicht aktiviert ist, verwenden Sie das **Set-ExecutionPolicy**-Cmdlet, um die Ausführung eines unsignierten Moduls in der lokalen Sitzung zuzulassen.

**Set-ExecutionPolicy Unrestricted CurrentUser**

Melden Sie sich als Nächstes bei dem Azure-Abonnement an, mit dem die LEADER-Bereitstellung vorgenommen wurde:

**Login-AzureRmAccount**

Importieren Sie dann das Modul:

**Import-Module <filepath to downloaded file>**

Führen Sie abschließend die Funktion mithilfe der entsprechenden Eingabe aus:

- **MyGatewayResourceId**: Ressourcenpfad des Gateways. Die Informationen können Sie dem Ausgabeparameter **CONSORTIUM_MEMBER_GATEWAY_ID** der Vorlagenbereitstellung entnehmen.
- **OtherGatewayResourceId**: Ressourcenpfad des Gateways des beitretenden Mitglieds. Dieser wird vom beitretenden Mitglied angegeben und entspricht dem gleichnamigen Ausgabeparameter **CONSORTIUM_MEMBER_GATEWAY_ID** der Vorlagenbereitstellung.
- **ConnectionName**: Ein Name zur Identifizierung dieser Gateway-Verbindung.
- **Gemeinsam verwendeter Schlüssel**: Der vorab festgelegte, geheime Schlüssel zwischen zwei Mitgliedern des Konsortiumsnetzwerks, zwischen denen eine Verbindung hergestellt wird.

**CreateConnection** - MyGatewayResourceId <resource path of your Gateway> -OtherGatewayResourceId <Ressourcenpfad des Gateways des beitretenden Mitglieds> -ConnectionName myConnection -SharedKey "MySharedKeyAbc123"

**xPlat CLI: Verbindung herstellen**

Laden Sie das Azure CLI-Skript herunter, und speichern Sie es lokal. Der Speicherort des Azure CLI-Skripts ist im Parameter **PAIR-GATEWAY-AZURE-CLI-SCRIPT** der Vorlagenbereitstellung angegeben.

Führen Sie das Skript mit dem entsprechenden Eingabewert aus:

- **MyGatewayResourceId**: Ressourcenpfad des Gateways. Die Informationen können Sie dem Ausgabeparameter **CONSORTIUM_MEMBER_GATEWAY_ID** der Vorlagenbereitstellung entnehmen.
- **OtherGatewayResourceId**: Ressourcenpfad des Gateways des beitretenden Mitglieds. Dieser wird vom beitretenden Mitglied angegeben und entspricht dem gleichnamigen Parameter **CONSORTIUM_MEMBER_GATEWAY_ID** der Vorlagenbereitstellung.
- **ConnectionName**: Ein Name zur Identifizierung dieser Gateway-Verbindung.
- **Gemeinsam verwendeter Schlüssel**: Der vorab festgelegte, geheime Schlüssel zwischen zwei Mitgliedern des Konsortiumsnetzwerks, zwischen denen eine Verbindung hergestellt wird.
- **Standort**: Die Azure-Region, in der die Gatewayressource bereitgestellt wird.

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

Die Architektur sieht wie folgt aus, nachdem Sie erfolgreich eine Verbindung zwischen **LEADER**-und **MEMBER**-Bereitstellung konfiguriert haben.

![LEADER- und MEMBER-Architektur](./media/ethereum-deployment/leader-member.png)

## <a name="fund-new-member-account"></a>Aufladen eines neuen Mitgliedskontos

### <a name="generated-genesis-block"></a>Generierter Genesis-Block

Genau wie beim ersten Mitglied wird das Ethereum-Konto mit einer Billion Ether aufgeladen, wenn der Genesis-Block (Benutzerdefinierter Genesis-Block = Nein) von der Bereitstellung generiert wird. Andere Mitglieder verfügen über ein nicht aufgeladenes Konto und müssen so lange auf ihr Ether-Guthaben warten, bis die Miningknoten mit dem Mining von Blöcken beginnen. Um zu vermeiden, dass neue Mitglieder auf ihr Ether-Guthaben warten müssen, müssen Sie die Ethereum-Konten der beitretenden Mitglieder explizit aufladen.

Dazu müssen beitretende Mitglieder das Ethereum-Konto angeben, das auf ihrer Administratorwebsite angezeigt wird. Sie können dann Ihre eigene Administratorwebsite verwenden, um Ether von Ihrem Konto auf das Mitgliedskonto zu überweisen. Geben Sie einfach das angegebene Konto ein.

### <a name="custom-genesis-block"></a>Benutzerdefinierter Genesis-Block

Wenn ein benutzerdefinierter Genesis-Block mit einem bestimmten Ethereum-Konto angegeben wird, können Sie MetaMask oder ein anderes Tool verwenden, um Ether vom angegebenen Konto an das vorab generierte Ethereum-Konto zu überweisen, das auf der Administratorwebsite angezeigt wird. Eine Anleitung zur Verwendung von MetaMask finden Sie unter [Erstellen eines Ethereum-Kontos](#create-ethereum-account).

Wenn ein benutzerdefinierter Genesis-Block ohne Konto angegeben wird bzw. wenn Sie keinen Zugriff auf vorab zugewiesene Konten haben, müssen Sie warten, bis die Miningknoten mit dem Mining beginnen, um Ether-Guthaben für das Konto zu generieren (Coinbase). Wie schnell das Guthaben generiert wird, hängt vom Schwierigkeitsgrad ab, den Sie für den benutzerdefinierten Genesis-Block angegeben haben.

## <a name="create-ethereum-account"></a>Erstellen eines Ethereum-Kontos

Um ein zusätzliches Konto zu erstellen, stehen Ihnen verschiedene Möglichkeiten zur Verfügung. Eine Option ist das MetaMask-Tool, eine Chrome-Erweiterung, die einen Identitätstresor und eine Verbindung mit einem Ethereum-Netzwerk (öffentlich, benutzerdefiniert oder Test) bereitstellt. MetaMask erstellt eine Transaktion, um das Konto im Netzwerk zu registrieren.

Diese Transaktion führt wie jede andere Transaktion zu einem der Transaktionsknoten und wird schließlich wie unten dargestellt per Mining in einen Block übernommen.

![Transaktionsknoten](./media/ethereum-deployment/transaction-node.png)

Um die Erweiterung in Chrome zu installieren, wechseln Sie zu „Anpassen“ und zur Google Chrome-Steuerung (Überlaufschaltfläche) > Weitere Tools > Erweiterungen > Mehr Erweiterungen herunterladen, und suchen Sie nach „MetaMask“.

![MetaMask-Erweiterung](./media/ethereum-deployment/metamask-extension.png)

Öffnen Sie MetaMask, nachdem die Installation abgeschlossen ist, und erstellen Sie einen neuen Tresor. Der Tresor ist standardmäßig mit dem Morden Test Network verbunden. Sie müssen die Standardeinstellung ändern, um stattdessen eine Verbindung mit dem bereitgestellten, privaten Konsortiumsnetzwerk herzustellen (d. h. mit dem vor die Transaktionsknoten geschalteten Lastenausgleich). Rufen Sie von der Vorlagenausgabe aus den verfügbar gemachten Ethereum RPC-Endpunkt an Port 8545 (`ETHEREUM-RPC-ENDPOINT`) ab, und geben Sie ihn wie unten dargestellt in den benutzerdefinierten RPC ein.

![MetaMask-Einstellungen](./media/ethereum-deployment/metamask-settings.png)

Indem Sie den Tresor erstellen, können Sie ein Wallet anlegen, in dem ein Konto enthalten ist. Um weitere Konten zu erstellen, wählen Sie **Konto wechseln** und dann die Schaltfläche **+** aus.

![MetaMask, Konto erstellen](./media/ethereum-deployment/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>Initiieren einer anfänglichen Ether-Zuteilung

Auf der Administratorseite können Sie eine Transaktion erstellen, um Ether-Guthaben vom vorab zugewiesenen Konto auf ein anderes Ethereum-Konto zu überweisen. Diese Ether-Überweisung entspricht einer Transaktion, die an den Transaktionsknoten gesendet und per Mining in einen Block übernommen wird (siehe Abbildung unten).

![Transaktionsknoten](./media/ethereum-deployment/transaction-node-mined.png)

Verwenden Sie das Zwischenablagesymbol im MetaMask-Wallet, um die Adresse des Ethereum-Kontos zu kopieren, an das Sie ein Ether-Guthaben überweisen wollen. Kehren Sie dann zur Administratorseite zurück. Fügen Sie das kopierte Konto in das Eingabefeld ein, um 1.000 Ether vom vorab zugeordneten Ethereum-Konto an das neu erstellte Konto zu überweisen. Klicken Sie auf **Absenden**, und warten Sie, bis die Transaktion per Mining in einem Block übernommen wurde.

![Knotenstatus](./media/ethereum-deployment/node-status2.png)

Nachdem die Transaktion in einen Miningblock commitet wurde, beläuft sich der Kontensaldo Ihres Kontos in MetaMask auf den überwiesenen Betrag von 1.000 Ether.

![MetaMask – Überweisung](./media/ethereum-deployment/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>Überweisen von Ether-Guthaben zwischen Konten

Jetzt ist es so weit, dass Sie Transaktionen im privaten Konsortiumsnetzwerk ausführen können. Die einfachste Transaktion besteht darin, Ether-Guthaben von einem Konto auf ein anderes Konto zu überweisen. Um eine solche Transaktion zu erstellen, verwenden Sie erneut MetaMask. Dabei überweisen Sie einen Betrag von dem ersten, oben verwendeten Konto auf ein zweites Konto.

Klicken Sie in MetaMask unter **Wallet 1** auf „Senden“. Kopieren Sie die Adresse des zweiten erstellten Wallets in das Eingabefeld **Empfängeradresse** und das zu überweisende Ether-Guthaben in das Eingabefeld **Betrag**. Klicken Sie auf **Senden**, um die Transaktion zu akzeptieren.

![MetaMask, Transaktion senden](./media/ethereum-deployment/metamask-send.png)

Nachdem die Transaktion per Mining in einen Block committet wurde, werden die Kontensalden entsprechend angepasst. Hinweis: Dem Saldo von **Wallet 1** werden mehr als 15 Ether abgezogen, da für die Verarbeitung der Transaktion eine Mininggebühr fällig wird.

![Wallet 1](./media/ethereum-deployment/wallet1.png)

![Wallet 2](./media/ethereum-deployment/wallet2.png)

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie sich auf die Entwicklung von Anwendungen und intelligenten Verträgen für Ihr privates Blockchain-Konsortiumsnetzwerk konzentrieren.
