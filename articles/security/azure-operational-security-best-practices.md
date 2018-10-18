---
title: Bewährte Methoden für Azure Operational Security | Microsoft-Dokumentation
description: Dieser Artikel enthält bewährte Methoden für Azure Operational Security.
services: security
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: terrylan
ms.openlocfilehash: d005dd01de0dff0136c0a4e9775001dbe018228f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035282"
---
# <a name="azure-operational-security-best-practices"></a>Bewährte Methoden für Azure Operational Security
Azure Operational Security bezieht sich auf die Dienste, Steuerelemente und Features, die Benutzern zum Schützen ihrer Daten, Anwendungen und anderen Ressourcen in Azure zur Verfügung stehen. Azure Operational Security basiert auf einem Framework, das Erkenntnisse verwendet, die aus einzigartigen Microsoft-Funktionen stammen, u.a. aus dem [Microsoft Security Development Lifecycle](https://www.microsoft.com/sdl), dem [Microsoft Security Response Center-Programm](https://www.microsoft.com/msrc?rtc=1) und den umfassenden Informationen zur Bedrohungslage hinsichtlich der Sicherheit im Internet.

In diesem Artikel werden die bewährten Methoden für die Sicherheit beschrieben. Diese bewährten Methoden sind aus unseren Erfahrungen mit der Azure-Datenbanksicherheit und den Erfahrungen von Kunden wie Ihnen abgeleitet.

Für jede bewährte Methode wird Folgendes beschrieben:
-   Wobei es bei der bewährten Methode geht
-   Warum Sie die bewährte Methode nutzen sollten
-   Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
- Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den bewährten Methoden für Azure Operational Security basiert auf einer Konsensmeinung und den Möglichkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Zeitpunkt der Erstellung dieses Artikels vorhanden waren. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Überwachen von Speicherdiensten auf unerwartete Änderungen des Verhaltens
Das Diagnostizieren und Behandeln von Problemen kann in einer verteilten Anwendung, die in einer Cloudumgebung gehostet wird, komplexer sein als in herkömmlichen Umgebungen. Anwendungen können in einer PaaS- oder IaaS-Infrastruktur, lokal, auf einem mobilen Gerät oder in einer Kombination dieser Umgebungen bereitgestellt werden. Der Netzwerkdatenverkehr Ihrer Anwendung fließt möglicherweise durch öffentliche und private Netzwerke, und Ihre Anwendung verwendet möglicherweise mehrere Speichertechnologien.

Es wird empfohlen, die von Ihrer Anwendung verwendeten Speicherdienste fortlaufend zu überwachen, um unerwartete Änderungen am Verhalten zu erkennen (z.B. eine langsamere Reaktionszeit). Erfassen Sie mit der Protokollierung umfangreichere Daten, anhand derer Sie ein Problem genauer analysieren können. Die Diagnoseinformationen, die Sie sowohl aus der Überwachung als auch der Protokollierung erhalten, helfen Ihnen, die Ursache des Problems, das bei Ihrer Anwendung auftritt, zu bestimmen. Dann können Sie das Problem behandeln und die entsprechenden Schritte zur Beseitigung bestimmen.

[Azure Storage Analytics](../storage/storage-analytics.md) führt die Protokollierung durch und stellt Metrikdaten für ein Azure-Speicherkonto bereit. Es wird empfohlen, mit diesen Daten Anforderungen zu verfolgen, Verwendungstrends zu analysieren und Probleme mit dem Speicherkonto zu diagnostizieren.

## <a name="prevent-detect-and-respond-to-threats"></a>Verhindern, Erkennen und Reagieren auf Bedrohungen
[Azure Security Center](../security-center/security-center-intro.md) unterstützt Sie durch mehr Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen dabei, Bedrohungen zu erkennen, zu verhindern und zu beheben. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt blieben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

Der Free-Tarif des Security Center bietet Sicherheit nur für Ihre Azure-Ressourcen. Im Standard-Tarif ist auch die Sicherheit für lokale Ressourcen und Cloudressourcen enthalten. Security Center Standard hilft Ihnen, Sicherheitsrisiken zu finden und zu beseitigen, Zugriffs- und Anwendungssteuerungen anzuwenden, um böswillige Aktivitäten zu blockieren, Bedrohungen mithilfe von Analysen und intelligenter Funktionen zu erkennen und bei Angriffen schnell zu reagieren. Sie können Security Center Standard die ersten 60 Tage kostenlos testen. Es wird empfohlen, für Ihr Azure-Abonnements den [Security Center Standard-Tarif](../security-center/security-center-get-started.md) zu verwenden.

Im Security Center können Sie sich den Sicherheitsstatus Ihrer Azure-Ressourcen auf einen Blick ansehen. Erfahren Sie auf einen Blick, ob die erforderlichen Sicherheitskontrollfunktionen implementiert und ordnungsgemäß konfiguriert sind, und ermitteln Sie schnell Ressourcen, die Ihre Aufmerksamkeit erfordern.

## <a name="monitor-end-to-end-scenario-based-network-monitoring"></a>Szenariobasierte End-to-End-Netzwerküberwachung
Kunden erstellen ein End-to-End-Netzwerk in Azure, indem sie verschiedene Netzwerkressourcen wie z.B. virtuelle Netzwerke, ExpressRoute, Application Gateway und Load Balancer miteinander kombinieren. Überwachung steht für jede der Netzwerkressourcen zur Verfügung.

[Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) ist ein regionaler Dienst. Mit diesem Dienst können Sie Bedingungen auf Netzwerkebene in Azure überwachen und diagnostizieren.

Im Folgenden sind bewährte Methoden für die Netzwerküberwachung und verfügbare Tools aufgelistet.

**Bewährte Methode:** Automatisieren der Remotenetzwerküberwachung per Paketerfassung.   
**Beschreibung:** Überwachen und diagnostizieren Sie mithilfe von Network Watcher Netzwerkprobleme, ohne sich bei Ihren VMs anmelden zu müssen. Lösen Sie mithilfe von Warnungen die [Paketerfassung](../network-watcher/network-watcher-alert-triggered-packet-capture.md) aus, und erhalten Sie Zugriff auf Leistungsinformationen in Echtzeit auf Paketebene. Wenn Sie ein Problem feststellen, können Sie dieses detailliert untersuchen, um es besser diagnostizieren zu können.

**Bewährte Methode:** Gewinnen von Einblicken in den Netzwerkdatenverkehr mithilfe von Datenflussprotokollen.   
**Beschreibung:** Lernen Sie mithilfe von Datenflussprotokollen von [Netzwerksicherheitsgruppen](../network-watcher/network-watcher-nsg-flow-logging-overview.md) die Datenverkehrsmuster in Ihrem Netzwerk kennen. Mit den Daten aus den Datenflussprotokollen können Sie Erkenntnisse gewinnen, die Sie zum Erfüllen von Konformitätsanforderungen und zum Überwachen Ihres Netzwerksicherheitsprofils benötigen.

**Bewährte Methode:** Diagnostizieren von VPN-Konnektivitätsproblemen.   
[Beschreibung:](../network-watcher/network-watcher-diagnose-on-premises-connectivity.md) Verwenden Sie Network Watcher, um die **am häufigsten auftretenden VPN-Gateway- und Verbindungsprobleme zu diagnostizieren**. Sie können nicht nur das Problem identifizieren, sondern auch umfangreiche Protokolle verwenden, um das Problem ausführlich zu untersuchen.

## <a name="secure-deployment-by-using-proven-devops-tools"></a>Sichern der Bereitstellung mithilfe von bewährten DevOps-Tools
Mit den folgenden bewährten DevOps-Methoden können Sie sicherstellen, dass Ihr Unternehmen und Team produktiv und effizient arbeiten.

**Bewährte Methode:** Automatisieren der Erstellung und Bereitstellung eines Diensts.   
**Beschreibung:** [Infrastruktur als Code](https://en.wikipedia.org/wiki/Infrastructure_as_Code) sind Techniken und Methoden, die für IT-Experten die tägliche Erstellung und Verwaltung einer modularen Infrastruktur vereinfachen. IT-Experten können damit ihre moderne Serverumgebung so erstellen und verwalten, wie Softwareentwickler Anwendungscode erstellen und verwalten.

Mit dem [Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) können Sie Ihre Anwendungen mit einer deklarativen Vorlage bereitstellen. In einer Vorlage können Sie mehrere Dienste zusammen mit ihren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage, um Ihre Anwendung in jeder Phase des Anwendungslebenszyklus wiederholt bereitzustellen.

**Bewährte Methode:** Automatisches Erstellen und Bereitstellen von Azure-Web-Apps oder -Clouddiensten.   
**Beschreibung:** Sie können Ihre Teamprojekte in Visual Studio Team Services so konfigurieren, dass sie [automatisch erstellt und in Azure-Web-Apps oder -Clouddiensten bereitgestellt](https://www.visualstudio.com/docs/build/overview) werden. VSTS stellt die Binärdateien nach der Durchführung eines Builds in Azure nach jedem Einchecken von Code automatisch bereit. Der Prozess der Paketerstellung entspricht dem Befehl „Packen“ in Visual Studio, und die Veröffentlichungsschritte entsprechen dem Befehl „Veröffentlichen“ in Visual Studio.

**Bewährte Methode:** Automatisieren der Releaseverwaltung.   
**Beschreibung:** [Release Management für Visual Studio](https://msdn.microsoft.com/library/vs/alm/release/overview) ist eine Lösung für die Automatisierung einer mehrstufigen Bereitstellung und die Verwaltung des Releaseprozesses. Erstellen Sie verwaltete Continuous Deployment-Pipelines für schnelle, einfache und häufige Releases. Mit Release Management können Sie den Releaseprozess automatisieren und vordefinierte Genehmigungsworkflows einrichten. Stellen Sie lokal und in der Cloud bereit, und führen Sie nach Bedarf Erweiterungen und Anpassungen durch.

**Bewährte Methode:** Testen der Leistung Ihrer Web-App vor dem Start oder Bereitstellen von Updates für die Produktion.   
**Beschreibung:** Führen Sie cloudbasierte [Auslastungstest](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) durch, und verwenden Sie VSTS für Folgendes:

- Leistungsprobleme in Ihrer Web-App finden.
- Qualität der Bereitstellung verbessern.
- Verfügbarkeit Ihrer App sicherstellen.
- Stellen Sie sicher, dass Ihre App den Datenverkehr für die nächste Produkteinführung oder Marketingkampagne bewältigen kann.

**Bewährte Methode:** Überwachen der Anwendungsleistung.   
**Beschreibung:** [Azure Application Insights](../application-insights/app-insights-overview.md) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Sie können mit Application Insights Ihre Live-Web-App überwachen. Der Dienst erkennt automatisch Leistungsanomalien. Er verfügt über Analysetools, mit denen Sie Probleme diagnostizieren und nachvollziehen können, wie Ihre App von Benutzern verwendet wird. Der Dienst unterstützt Sie bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit Ihrer App.

## <a name="mitigate-and-protect-against-ddos"></a>Abwenden von und Schützen vor DDoS
Bei einem DDoS-Angriff versucht ein Angreifer, Anwendungsressourcen zu erschöpfen. Die Verfügbarkeit der Anwendung und ihre Fähigkeit, legitime Anforderungen zu verarbeiten, sollen beeinträchtigt werden. Diese Angriffe werden komplexer, ihr Ausmaß größer und ihre Auswirkungen gravierender. Jeder Endpunkt, der öffentlich über das Internet erreichbar ist, kann Ziel von DDoS-Angriffen werden.

Konzeptionen für Resilienz gegenüber DDoS-Angriffen setzen Planung und Entwürfe für eine Vielzahl von Fehlerzuständen voraus.

Im Folgenden sind bewährte Methoden zum Erstellen von Anwendungen in Azure aufgelistet, die DDoS-Angriffen standhalten können.

**Bewährte Methode:** Stellen Sie sicher, dass die Sicherheit im gesamten Lebenszyklus einer Anwendung Priorität hat – von Entwurf und Implementierung bis hin zu Bereitstellung und Betrieb. Anwendungen können Fehler enthalten, die einer relativ geringen Anzahl von Anforderungen die Verwendung von vielen Ressourcen erlauben. Dies führt zu einem Dienstausfall.    
**Beschreibung:** Um einen in Microsoft Azure ausgeführten Dienst zu schützen, sollten Sie Ihre Anwendungsarchitektur verstehen und sich auf die [Fünf Säulen der Softwarequalität](https://docs.microsoft.com/azure/architecture/guide/pillars) konzentrieren. Sie sollten Folgendes kennen: das typische Datenverkehrsvolumen, das Modell der Konnektivität zwischen der Anwendung und anderen Anwendungen, sowie die Dienstendpunkte, die dem öffentlichen Internet verfügbar gemacht werden.

Es ist von größter Wichtigkeit sicherzustellen, dass eine Anwendung stabil genug ist, um mit einem Denial-of-Service-Angriff fertig zu werden. Sicherheit und Datenschutz sind direkt in der Azure-Plattform integriert, beginnend mit [Security Development Lifecycle (SDL)](https://www.microsoft.com/en-us/sdl). Die SDL spricht Sicherheit in jeder Entwicklungsphase an und sorgt dafür, dass Azure kontinuierlich aktualisiert wird, um noch sicherer zu sein.

**Bewährte Methode:** Sie sollten Ihre Anwendungen so entwerfen, dass sie [horizontal skaliert](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) werden können, um die Anforderungen einer verstärkten Auslastung zu erfüllen – insbesondere im Falle eines DDoS-Angriffs. Wenn Ihre Anwendung von einer einzelnen Instanz eines Diensts abhängig ist, entsteht dadurch ein Single Point of Failure. Durch Bereitstellen mehrerer Instanzen wird Ihr System stabiler und besser skalierbar.   
**Beschreibung:** Wählen Sie für [Azure App Service](../app-service/app-service-value-prop-what-is.md) einen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) aus, der mehrere Instanzen bietet.

Konfigurieren Sie für Azure Cloud Services alle Rollen so, dass sie [mehrere Instanzen](../cloud-services/cloud-services-choose-me.md) verwenden.

Stellen Sie für [Azure-VMS](../virtual-machines/windows/overview.md) sicher, dass die VM-Architektur mehr als eine VM enthält und dass jede VM zu einer [Verfügbarkeitsgruppe](../virtual-machines/virtual-machines-windows-manage-availability.md) gehört. Sie sollten VM-Skalierungsgruppen für Funktionen zur automatischen Skalierung verwenden.

**Bewährte Methode:** Implementieren Sie Abwehrmaßnahmen schichtweise in eine Anwendung, um die Wahrscheinlichkeit eines erfolgreichen Angriffs zu reduzieren. Implementieren Sie sichere Entwürfe für Ihre Anwendungen mithilfe der integrierten Funktionen der Azure-Plattform.   
**Beschreibung:** Das Angriffsrisiko steigt mit der Größe (Oberfläche) der Anwendung. Sie können den Oberflächenbereich durch Verwendung von Whitelists verringern, um den verfügbar gemachten IP-Adressraum und die Überwachungsports zu schließen, die im Lastenausgleichsmodul ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) und [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)) nicht mehr benötigt werden.

Durch [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) kann die angreifbare Oberfläche ebenfalls reduziert werden. Sie können mit [Diensttags](../virtual-network/security-overview.md#service-tags) und [Anwendungssicherheitsgruppen](../virtual-network/security-overview.md#application-security-groups) das Erstellen von Sicherheitsregeln weniger komplex machen und Netzwerksicherheit als natürliche Erweiterung der Struktur einer Anwendung konfigurieren.

Sie sollten Azure-Dienste nach Möglichkeit in einem [virtuellen Netzwerk](../virtual-network/virtual-networks-overview.md) bereitstellen. Mit dieser Methode können Dienstressourcen über private IP-Adressen kommunizieren. Standardmäßig werden für Datenverkehr von Azure-Diensten aus einem virtuellen Netzwerk öffentliche IP-Adressen als Quell-IP-Adressen verwendet.

Bei Verwendung von [Dienstendpunkten](../virtual-network/virtual-network-service-endpoints-overview.md) wechselt der Dienstdatenverkehr zu privaten Adressen im virtuellen Netzwerk als Quell-IP-Adressen, wenn aus einem virtuellen Netzwerk auf den Azure-Dienst zugegriffen wird.

Häufig werden lokale Ressourcen von Kunden zusammen mit ihren Azure-Ressourcen angegriffen. Wenn Sie die Verbindung einer lokalen Umgebung mit Azure herstellen, machen Sie lokale Ressourcen so wenig wie möglich im öffentlichen Internet verfügbar.

Azure bietet zwei [DDoS-Dienstangebote](../virtual-network/ddos-protection-overview.md), die vor Netzwerkangriffen schützen:

- Basic-Schutz ist standardmäßig und ohne zusätzliche Kosten in der Azure-Plattform integriert. Die Skalierung und Kapazität des global bereitgestellten Azure-Netzwerks bietet Schutz vor häufigen Vermittlungsschichtangriffen durch Always On-Datenverkehrsüberwachung und Risikominderung in Echtzeit. Für den Basic-Schutz sind keine weiteren Konfigurationen oder Anwendungsänderungen von Seiten des Benutzers erforderlich. Mit diesem Schutz können Sie alle Azure-Dienste schützen, u.a. PaaS-Dienste wie Azure DNS.
- Der Standard-Schutz bietet erweiterte DDoS-Abwehrfunktionen gegen Netzwerkangriffe. Es wird automatisch optimiert, um Ihre spezifischen Azure-Ressourcen zu schützen. Der Schutz lässt sich einfach während der Erstellung virtueller Netzwerke aktivieren. Dies ist auch nach der Erstellung möglich und erfordert keine Änderungen der Anwendung oder Ressourcen.

## <a name="next-steps"></a>Nächste Schritte
Weitere bewährte Methoden für die Sicherheit, die Sie beim Entwerfen, Bereitstellen und Verwalten Ihrer Cloudlösungen mithilfe von Azure verwenden können, finden Sie unter [Sicherheit in Azure: bewährte Methoden und Muster](security-best-practices-and-patterns.md).

Die folgenden Ressourcen enthalten allgemeinere Informationen zur Sicherheit in Azure und verwandten Microsoft-Diensten:
* [Blog des Azure-Sicherheitsteams](https://blogs.msdn.microsoft.com/azuresecurity/): Hier finden Sie Informationen über den aktuellen Stand der Azure-Sicherheit.
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx): Hier können Sie Microsoft-Sicherheitsrisiken (z.B. Probleme mit Azure) melden oder eine E-Mail an secure@microsoft.com schreiben.
