---
title: Supportrichtlinien für Azure Kubernetes Service (AKS)
description: Hier finden Sie Informationen zu Supportrichtlinien für Azure Kubernetes Service (AKS), zur gemeinsamen Verantwortung sowie zu Features der Vorschau-, Alpha- und Betaversion.
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 04/01/2019
ms.author: jenoller
ms.openlocfilehash: f173fc7c794729eae8c60cceefa88d153800a816
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59501839"
---
# <a name="azure-kubernetes-service-aks-support-policies"></a>Supportrichtlinien für Azure Kubernetes Service (AKS)

Dieser Artikel enthält Details zu Richtlinien für den technischen AKS-Support und zu Einschränkungen sowie andere Details wie Workerknotenverwaltung, verwaltete Komponenten der Steuerungsebene, Open-Source-Komponenten von Drittanbietern und Sicherheits-/Patchverwaltung.

## <a name="service-updates--releases"></a>Dienstupdates und Releases

* Releaseinformationen finden Sie in den [AKS-Versionshinweisen][1].
* Informationen zu Features in der Public Preview-Phase finden Sie unter [AKS Preview Features and Related Projects][2] (AKS-Previewfunktionen und dazugehörige Projekte).

## <a name="what-is-managed"></a>Was bedeutet „verwaltet“?

Im Gegensatz zu grundlegenden IaaS-Cloudkomponenten wie Compute und Netzwerk, die Low-Level-Steuerungen und Anpassungsoptionen für Benutzer verfügbar machen, bietet AKS eine sofort einsatzbereite Kubernetes-Bereitstellung mit den gängigen Konfigurationen und Funktionen, die für Kubernetes erforderlich sind. Kunden, die diesen Dienst nutzen, steht nur eine begrenzte Anzahl von Optionen für Anpassungen, Bereitstellung und andere Aspekte zur Verfügung. Das bedeutet aber auch, dass sich Kunden keine Gedanken machen und die Kubernetes-Cluster nicht direkt verwalten müssen.

Mit AKS erhält der Kunde eine vollständig verwaltete **Steuerungsebene**, die sämtliche Komponenten und Dienste enthält, die erforderlich sind, um Kubernetes-Cluster zu betreiben und für Endbenutzer bereitzustellen. Alle Kubernetes-Komponenten werden von Microsoft verwaltet und betrieben.

Mit der verwalteten **Steuerungsebene** werden die folgenden Komponenten von Microsoft verwaltet und überwacht:

* Kubelet/Kubernetes-API-Server
* etcd oder ein kompatibler Schlüssel-Wert-Speicher (einschließlich Quality of Service, Skalierbarkeit und Runtime)
* DNS-Dienste wie kube-dns oder CoreDNS
* Kubernetes-Proxy/-Netzwerk

AKS ist keine vollständig verwaltete **Cluster**lösung. Für bestimmte Komponenten (beispielsweise Workerknoten) gibt es Fälle mit **gemeinsamer Verantwortung**, in denen Benutzer bei Aktionen zur Verwaltung des AKS-Clusters tätig werden müssen. Ein Beispiel wäre etwa die Anwendung von Betriebssystem-Sicherheitspatches für Workerknoten.

 **Verwaltet** bedeutet, dass sich Microsoft und das AKS-Team um die Bereitstellung und den Betrieb dieser Dienste kümmern und für deren Verfügbarkeit und Funktionalität verantwortlich sind. **Diese Komponenten können von Kunden nicht geändert werden.** Die Anpassung wurde eingeschränkt, um eine konsistente und skalierbare Benutzererfahrung zu gewährleisten. Eine vollständig anpassbare Lösung finden Sie hier: [AKS-Engine](https://github.com/Azure/aks-engine).

> [!NOTE]
> Es ist wichtig zu verstehen, dass Workerknoten von Azure Kubernetes Service im Azure-Portal als reguläre Azure-IaaS-Ressourcen angezeigt werden, auch wenn diese virtuellen Computer in einer benutzerdefinierten Azure-Ressourcengruppe bereitgestellt werden (mit dem Präfix „MC\\*“). Benutzer können sie ändern und genau wie bei normalen virtuellen Computern eine SSH-Verbindung mit ihnen herstellen. (Sie können allerdings nicht das Basisbetriebssystem-Image ändern, und Änderungen gehen nach einem Update oder Neustart möglicherweise verloren.) Darüber hinaus können Sie andere Azure-Ressourcen anfügen oder sie anderweitig ändern. **Diese Out-of-Band-Verwaltung und -Anpassung kann jedoch dazu führen, dass für den AKS-Cluster kein Support mehr bereitgestellt werden kann. Änderungen an Workerknoten sollten nur unter Anleitung des Microsoft-Supports vorgenommen werden.**

## <a name="what-is-shared-responsibility"></a>Was ist gemeinsame Verantwortung?

Zum Zeitpunkt der Clustererstellung erstellt AKS eine Reihe von Kubernetes-Workerknoten, die vom Kunden definiert werden. Auf diesen Knoten werden wie bereits erwähnt die Kundenworkloads ausgeführt. Kunden sind die Besitzer dieser Workerknoten und können sie anzeigen und ändern.

Da diese Knoten privaten Code ausführen und sensible Daten speichern, hat der Microsoft-Support nur **eingeschränkten Zugriff** auf alle Clusterknoten von Kunden. Der Microsoft-Support kann sich erst nach ausdrücklicher Genehmigung des Kunden bei diesen Knoten anmelden, Befehle für die Knoten ausführen oder Protokolle für die Knoten anzeigen, und er benötigt Unterstützung bei der Ausführung von Debuggingbefehlen unter Anleitung des Supportteams.

Angesichts der Sensibilität dieser Workerknoten schränkt Microsoft die Hintergrundverwaltung dieser Knoten äußerst gewissenhaft ein. Selbst wenn die Kubernetes-Masterknoten, etcd und andere (von Microsoft verwaltete) Komponenten ausfallen sollten, wird Ihre Workload in der Regel weiter ausgeführt. Unvorsichtige Workerknotenänderungen können Daten- und/oder Workloadverluste zur Folge haben und dazu führen, dass für den Cluster kein Support mehr bereitgestellt werden kann.

## <a name="azure-kubernetes-service-support-coverage"></a>Supportabdeckung für Azure Kubernetes Service

**Microsoft bietet technischen Support für Folgendes:**

* Konnektivität mit allen Kubernetes-Komponenten, die vom Kubernetes-Dienst bereitgestellt und unterstützt werden (beispielsweise der API-Server).
* Verwaltung, Betriebszeit, QoS und Vorgänge der Kubernetes-Steuerungsebenendienste (beispielsweise Kubernetes-Masterknoten, API-Server, etcd und kube-dns).
* Der etcd-Support beinhaltet automatisierte, transparente Sicherungen sämtlicher etcd-Daten im 30-Minuten-Takt für die Planung der Notfallwiederherstellung und die Wiederherstellung des Clusterzustands. Diese Sicherungen sind nicht direkt für Kunden/Benutzer verfügbar und dienen zur Gewährleistung der Datenzuverlässigkeit und -konsistenz.
* Alle Integrationspunkte im Azure-Cloudanbietertreiber für Kubernetes – beispielsweise Integrationen in andere Azure-Dienste wie Lastenausgleichsmodule, persistente Volumes und Netzwerk (Kubernetes und Azure CNI).
* Fragen oder Probleme im Zusammenhang mit der Anpassung von Komponenten der Steuerungsebene wie Kubernetes-API-Server, etcd oder kube-dns.
* Probleme im Zusammenhang mit Netzwerkthemen – beispielsweise Azure CNI, Kubenet oder andere Probleme in Verbindung mit Netzwerkzugriff und -funktionalität (DNS-Auflösung, Paketverluste, Routing und Ähnliches).
  * Zu den unterstützten Netzwerkszenarien zählen unter anderem Kubenet (Basic) und erweiterte Netzwerke (Azure CNI) innerhalb des Clusters und den dazugehörigen Komponenten sowie Konnektivität mit anderen Azure-Diensten und -Anwendungen. Außerdem bietet Microsoft Support für Eingangscontroller und Eingangs-/Lastenausgleichskonfiguration, Netzwerkleistung und Wartezeit.

**Microsoft bietet keinen technischen Support für Folgendes:**

* Tipps/Antworten auf Fragen zur Verwendung von Kubernetes – etwa zur Erstellung benutzerdefinierter Eingangscontroller, zu Anwendungsworkloads oder zu Drittanbieter-/OSS-Paketen oder -Tools – sind nicht enthalten.
  * Beratungstickets für AKS-Clusterfunktionen, -Anpassung und -Optimierung – etwa Probleme/Anleitungen im Zusammenhang mit Kubernetes-Vorgängen – sind enthalten.
* Open-Source-Projekte von Drittanbietern, die nicht im Rahmen der Kubernetes-Steuerungsebene bereitgestellt oder mit AKS-Clustern bereitgestellt wurden (etwa Istio, Helm, Envoy und andere).
  * Für Open-Source-Projekte von Drittanbietern wie Helm und Kured wird bestmöglicher Support für Beispiele und Anwendungen bereitgestellt, die in der Microsoft-Dokumentation angegeben sind und bei denen das Open-Source-Tool des Drittanbieters in den Kubernetes-Azure-Cloudanbieter integriert wird, oder bei anderen AKS-spezifischen Fehlern.
* Closed-Source-Software von Drittanbietern – etwa Tools für Sicherheitsscans, Netzwerkgeräte oder Software.
* Für Probleme im Zusammenhang mit mehreren Clouds oder mehreren Anbietern wird kein Support bereitgestellt. Hierzu zählt beispielsweise der Betrieb einer Verbundlösung mit mehreren Public Cloud-Anbietern.
* Spezifische Netzwerkanpassungen, die nicht in der offiziellen [AKS-Dokumentation][3] dokumentiert sind.
  > [!NOTE]
  > Für Probleme und Fehler im Zusammenhang mit Netzwerksicherheitsgruppen wird Support bereitgestellt. Der Support kann beispielsweise Fragen zu NSGs beantworten, die sich nicht aktualisieren lassen, oder zum unerwarteten Verhalten einer NSG oder eines Lastenausgleichs.

## <a name="azure-kubernetes-service-support-coverage-worker-nodes"></a>Supportabdeckung für Azure Kubernetes Service (Workerknoten)

### <a name="microsoft-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>Aufgaben von Microsoft im Zusammenhang mit Workerknoten von Azure Kubernetes Service

Wie im Abschnitt `shared responsibility` erwähnt unterliegen Kubernetes-Workerknoten einem Modell für gemeinsame Verantwortung, bei dem Folgendes gilt:

* Bereitstellung des Basisbetriebssystem-Images mit erforderlichen Ergänzungen (wie etwa Überwachungs- und Netzwerk-Agents)
* Automatische Bereitstellung von Betriebssystempatches auf den Workerknoten
* Automatische Behebung von Problemen mit Komponenten der Kubernetes-Steuerungsebene, die auf den Workerknoten ausgeführt werden. Beispiele:
  * kube-proxy
  * Netzwerktunnel zur Bereitstellung von Kommunikationspfaden zu Kubernetes-Masterkomponenten
  * kubelet
  * Docker-/Moby-Daemon

> [!NOTE]
> Wenn eine Komponente der Steuerungsebene auf einem Workerknoten ausgefallen ist, muss das AKS-Team unter Umständen den gesamten Workerknoten neu starten, um das Problem zu beheben. Dieser Schritt wird im Auftrag des Benutzers und nur dann ausgeführt, wenn eine Kundeneskalation vorliegt (aufgrund des Zugriffs auf die aktive Workload und die Daten des Kunden). Die AKS-Mitarbeiter versuchen bei jedem erforderlichen Neustart, Auswirkungen auf die Anwendung zu vermeiden.

### <a name="customer-responsibilities-for-azure-kubernetes-service-worker-nodes"></a>Aufgaben von Kunden im Zusammenhang mit Workerknoten von Azure Kubernetes Service

**Folgendes wird von Microsoft nicht ausgeführt:**

- Automatisches Neustarten von Workerknoten, um Patches auf der Betriebssystemebene anzuwenden **(aktueller Stand; weitere Informationen unten)**

Betriebssystempatches werden an die Workerknoten übermittelt, es liegt jedoch in der **Verantwortung des Kunden**, die Workerknoten neu zu starten, damit die Änderungen wirksam werden. Dieses automatische Patching beinhaltet freigegebene Bibliotheken, Daemons wie SSHD und andere Komponenten auf der System-/Betriebssystemebene.

Bei Kubernetes-Upgrades **sind Kunden für die Ausführung des Upgrades verantwortlich** (entweder über die Azure-Systemsteuerung oder über die Azure-Befehlszeilenschnittstelle). Dies gilt für Updates zur Verbesserung der Sicherheit oder Funktionalität von Kubernetes.

> [!NOTE]
> Da es sich bei AKS um einen `managed service` handelt, besteht das Endziel für den Dienst darin, jegliche Verantwortung für Patches, Updates, Protokollerfassung und Ähnliches zu beseitigen, um AKS zu einem stärker verwalteten und besser automatisierten Dienst zu machen. Diese Elemente (Neustarten von Knoten, automatisches Patchen) werden in zukünftigen Releases unter Umständen entfernt, wenn wir über bessere Möglichkeiten für die End-to-End-Verwaltung verfügen.

### <a name="security-issues-and-patching"></a>Sicherheitsprobleme und -patches

Es kann Fälle geben (etwa CVEs), in denen ein Sicherheitsrisiko in einer oder mehreren Komponenten des AKS-Diensts gefunden wird. In solchen Szenarien patcht AKS alle betroffenen Cluster, um das Problem zu beheben (sofern möglich), oder lässt den Benutzern eine Upgradeanleitung zukommen.

Für Workerknoten, die von einer solchen Sicherheitslücke betroffen sind, wendet das AKS-Team einen Patch ohne Downtime an, sofern ein solcher für das Problem verfügbar ist, und informiert die Benutzer über die Änderung.

Sollte für einen Sicherheitspatch ein Neustart von Workerknoten erforderlich sein, informiert Microsoft die Kunden über diese Anforderung, und der Kunde muss den Neustart oder das Update ausführen, um den Patch für seinen Cluster zu erhalten. Wenn Benutzer die Patches nicht gemäß den Anweisungen von AKS anwenden, ist ihr Cluster weiterhin anfällig für die Probleme.

### <a name="node-maintenance-and-access"></a>Knotenwartung und -zugriff

Da für Workerknoten die gemeinsame Verantwortung gilt und sie sich im Besitz der Kunden befinden, können sich Kunden bei diesen Workern anmelden und potenziell schädliche Änderungen vornehmen. Hierzu zählen etwa Kernel-Updates, das Entfernen von Paketen und das Installieren neuer Pakete.

Wenn Kunden schädliche Aktionen ausführen oder Komponenten der Steuerungsebene aufgrund von Kundenaktionen offline geschaltet werden oder anderweitig ausfallen, erkennt AKS diesen Ausfall und führt eine automatische Korrektur durch, um den vorherigen funktionsfähigen Zustand des Workerknotens wiederherzustellen.

Kunden können sich zwar bei Workerknoten anmelden und diese ändern, es wird jedoch davon *abgeraten*, da dies dazu führen kann, dass für Ihren Cluster kein Support mehr bereitgestellt wird.

## <a name="network-ports-access-and-network-security-groups"></a>Netzwerkports, Zugriff und Netzwerksicherheitsgruppen

Als verwalteter Dienst gelten für AKS bestimmte Netzwerk- und Konnektivitätsanforderungen. Diese Anforderungen sind weniger flexibel als bei normalen IaaS-Komponenten. Im Gegensatz zu anderen IaaS-Komponenten können bestimmte Vorgänge (etwa die Anpassung von Netzwerksicherheitsgruppen-Regeln, die Blockierung bestimmter Ports, die Erstellung von URL-Whitelists und Ähnliches) dazu führen, dass für Ihren Cluster kein Support mehr bereitgestellt wird (beispielsweise, wenn der ausgehende Port 443 durch Firewallregeln blockiert wird).

> [!NOTE]
> Die vollständige Sperrung des ausgehenden Datenverkehrs Ihres Clusters (etwa durch explizites Whitelisting von Domänen/Ports) ist derzeit kein unterstütztes AKS-Szenario. Die Liste mit den URLs und Ports kann sich ohne vorherige Ankündigung ändern und vom Azure-Support über ein Ticket bereitgestellt werden. Die bereitgestellte Liste gilt nur für Kunden, die bereit sind zu akzeptieren, dass *die Verfügbarkeit des Clusters jederzeit beeinträchtigt werden kann*.

## <a name="alphabeta-kubernetes-features-not-supported"></a>Kubernetes-Features der Alpha-/Betaversion (kein Support)

AKS bietet nur Support für stabile Features im Rahmen des Upstream-Kubernetes-Projekts. Für Alpha-/Betafeatures, die in Upstream-Kubernetes verfügbar sind, wird kein Support bereitgestellt, sofern dies nicht anderweitig kommuniziert und dokumentiert wurde.

Es gibt zwei Fälle, in denen Alpha- oder Betafeatures unter Umständen vor der allgemeinen Verfügbarkeit eingeführt werden:

* Kunden wurden bei einem Treffen mit den Produkt-, Support- oder -Entwicklungsteams von AKS explizit darum gebeten, diese neuen Features auszuprobieren.
* Die Features wurden [über ein Featureflag aktiviert][2] (explizite Aktivierung).

## <a name="preview-features--feature-flags"></a>Previewfunktionen/Featureflags

Für Features und Funktionen, die ausführliche Tests sowie Community- und Benutzerfeedback erfordern, veröffentlicht Microsoft neue Previewfunktionen oder Features hinter einem Featureflag. Diese Features sind als Vorabversion/Beta zu betrachten und werden verfügbar gemacht, um Benutzern Gelegenheit zu geben, diese neuen Features zu testen.

Diese Preview-/Featureflag-Features sind jedoch nicht für den Einsatz in einer Produktionsumgebung gedacht. API-Änderungen, Verhaltensänderungen, Fehlerkorrekturen und andere Änderungen können instabile Cluster und Downtime zur Folge haben. Der Support für diese Features ist auf Fehler-/Problemberichte beschränkt. Aktivieren Sie diese Features nicht in Produktionssystemen oder -abonnements.

> [!NOTE]
> Previewfunktionen werden auf der **Azure-Abonnementebene** aktiviert. Installieren Sie Previewfunktionen nicht in einem Produktionsabonnement, da dies zu einer Änderung des API-Standardverhaltens führen und den regulären Betrieb beeinträchtigen kann.

## <a name="upstream-bugs-and-issues"></a>Upstream-Fehler und -Probleme

Aufgrund der hohen Entwicklungsgeschwindigkeit im Upstream-Kubernetes-Projekt kommt es immer wieder zu Fehlern, die sich nicht durch einen Patch oder eine Problemumgehung innerhalb des AKS-Systems beheben lassen und umfangreichere Patches für Upstream-Projekte (wie etwa Kubernetes, Knoten-/Worker-Betriebssysteme und Kernel) erfordern. Bei Komponenten, die sich in unserem Besitz befinden (beispielsweise der Azure-Cloudanbieter), kümmern sich die AKS-/Azure-Mitarbeiter um die Upstream-Behebung des Problems in der Community.

Sollte ein technisches Supportproblem auf einen oder mehrere Upstream-Fehler zurückzuführen sein, führen die Support- und Entwicklungsteams von AKS folgende Schritte aus:

* Identifizieren und Verknüpfen aller Upstream-Fehler mit unterstützenden Details, die Aufschluss darüber geben, warum sich dadurch Auswirkungen für Ihren Cluster und/oder Ihre Workload ergeben. Kunden erhalten Links zu den erforderlichen Repositorys/Problemen, um auf die Probleme zu achten und zu erfahren, wann ein neues Kubernetes-Release oder ein anderes Release entsprechende Korrekturen enthält.
* Bereitstellen möglicher Problemumgehungen oder Gegenmaßnahmen: Manchmal lässt sich das Problem unter Umständen umgehen. In diesem Fall wird im AKS-Repository ein [bekanntes Problem](https://github.com/Azure/AKS/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue) mit folgenden Informationen dokumentiert:
  * Das Problem (einschließlich eines Links zu Upstream-Fehlern)
  * Die Problemumgehung sowie Details zu einem Upgrade/einer anderen Persistenz der Lösung
  * Grober Zeitplan für die Einbeziehung des Problems auf der Grundlage des Upstream-Releaserhythmus

[1]: https://github.com/Azure/AKS/releases
[2]: https://github.com/Azure/AKS/blob/master/previews.md
[3]: https://docs.microsoft.com/azure/aks/
