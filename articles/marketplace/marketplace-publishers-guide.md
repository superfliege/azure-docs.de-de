---
title: Herausgeberleitfaden für Azure Marketplace und AppSource
description: Schritt-für-Schritt-Anleitung und Veröffentlichung von Prüflisten für neue Herausgeber in Azure Marketplace
services: Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: msmbaldwin
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/18/2018
ms.author: ellacroi
ms.openlocfilehash: f090bcd56377d167dddab1b8f942d473aecb66a2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="azure-marketplace-and-appsource-publisher-guide"></a>Herausgeberleitfaden für Azure Marketplace und AppSource

Willkommen beim Herausgeberleitfaden für [Azure Marketplace](https://azuremarketplace.microsoft.com) und [AppSource](https://appsource.microsoft.com). Dieser Leitfaden soll neue und vorhandene Herausgeber dabei unterstützen, Azure Marketplace- und AppSource-Storefronts zu nutzen, ihre Anwendungen und Dienste zu veröffentlichen und ihr Geschäft im Rahmen der Partnerschaft mit Microsoft auszubauen. 

Wenn Sie diesen Leitfaden gelesen haben, sind Sie mit den folgenden Themen besser vertraut und wissen außerdem, wo Sie weitere Informationen finden:

- Vorteile der Auflistung in einer der Marketplace-Storefronts
- Verwenden der Storefronts
- Welche Storefront ist für Ihre Angebote und Dienste geeignet? 
- Welche Arten von Anwendungen und Dienstangeboten können veröffentlicht werden?
- Wie lauten die technischen und geschäftlichen Anforderungen für die einzelnen Veröffentlichungsoptionen?
- Erstellen einer Vorveröffentlichungs-Objektprüfliste
- Herausgeber werden
- Wo werden Angebote erstellt und veröffentlicht?
- Optimieren einer Auflistung und Verwenden von Ressourcen für die Markteinführung, um die Wirkung zu verbessern
- Zugreifen auf Hilfe und Support

Wenn Sie Fragen zu Azure Marketplace, AppSource oder diesem Leitfaden zur Veröffentlichung haben, können Sie sich unter cloudmarketplace@microsoft.com an das Marketplace-Team wenden. 

## <a name="benefits-of-participating-in-the-marketplace"></a>Vorteile der Teilnahme am Marketplace

Azure Marketplace und AppSource sind die Ausgangspunkte für gemeinsame Markteinführungsaktivitäten mit Microsoft und fördern das Geschäftswachstum. Mit Einführungsaktionen, Nachfragegenerierung und gemeinsamem Verkauf und Marketing können Sie Ihre Marketplace-Auflistungen zum Kernstück Ihres Cloudgeschäftsmoduls machen. Es werden keine Gebühren für Ihre Teilnahme am Marketplace berechnet. Unser Ziel ist, Microsoft-Kunden mit den besten Lösungen und Diensten zu verbinden, die unser Partnerökosystem bietet.

Nutzen Sie Marketplace-Funktionen, um Ihr Geschäftswachstum zu fördern:

- **Generieren Sie Leads und Verkaufschancen.** Betreten Sie neue Märkte mit einem erweiterten Portfolio von Lösungen auf der Microsoft-Cloudplattform. Setzen Sie Marketplace-Angebote für Up- und Cross-Selling ein. 
- **Steigern Sie Geschäftswert und Auftragsvolumen für vorhandene und neue Kunden.** Erhöhen Sie das Auftragsvolumen, und lösen Sie Kundenprobleme durch das Verschieben von Workloads in die Cloud. Verkürzen Sie Verkaufszyklen, und steigern Sie die Rentabilität von Geschäften, indem Sie vollständige Lösungen für spezifische Workloads und Branchenszenarien verkaufen.
- **Verschaffen Sie sich verwertbare Einblicke.** Ihr Erfolg ist unser Erfolg. Erhalten Sie einen Einblick in die Leistung Ihrer Auflistungen über das Cloud-Partnerportal. Halten Sie sich darüber auf dem Laufenden, welche Aktionen ablaufen, welche Leads Sie generiert haben und wie Sie Ihre Kampagnenaktivitäten maximieren können.

>[!NOTE]
>Apps zur Erweiterung von Office ermöglichen Einblicke über den Veröffentlichungsprozess für Office-Apps.

## <a name="azure-marketplace-and-appsource-storefronts"></a>Azure Marketplace- und AppSource-Storefronts

Microsoft stellt zwei unterschiedliche Marketplace-Storefronts bereit, über die Partner Angebote auflisten, Testversionen aktivieren und direkt Transaktionen mit Microsoft-Kunden und dem dazugehörigen Ökosystem durchführen können: [Azure Marketplace](https://azuremarketplace.microsoft.com) und [AppSource](https://appsource.microsoft.com). Mit diesen Storefronts können Kunden Anwendungen und Dienste, mit denen die digitale Transformation beschleunigt werden kann, finden, ausprobieren und erwerben. Sie erleichtern Herausgebern den Ausbau ihres Geschäfts, indem sie ihnen einen erweiterten Zugang zu Kunden und Partnerökosystemen von Microsoft bieten.
 
Die Marketplace-Storefronts sind auf Zielgruppen und Microsoft-Cloudprodukte ausgerichtet, damit Kunden das finden, was sie benötigen. Jede Storefront verfügt über spezielle Veröffentlichungsoptionen, die bewirken sollen, dass sich Ihre Investitionen in die Veröffentlichung noch besser amortisieren. Diese Optionen sind in der folgenden Tabelle zusammengefasst:


|          |Azure Marketplace |AppSource  |
|---------|---------|---------|
|Zielgruppe     |IT-Experten und Entwickler (zu den speziellen Rollen gehören DBAs, SecOps, DevOps usw.)    | Branchenspezifische Entscheidungsträger (zu den speziellen Rollen gehören Beschaffung, Herstellung, Buchhaltung)      |
|Erstellt mit oder zum Erweitern von     |Azure         | Azure, Dynamics 365, Office 365, Power BI, PowerApps       |
|Typen von Lösungen und Diensten     |  Infrastrukturlösungen, Professionelle Dienstleistungen   | Fertige branchenspezifische Cloudanwendungen, Office 365-Add-Ins, Professionelle Dienstleistungen        |
|Veröffentlichungsoptionen     |  Kontaktaufnahme, Beratungsangebot, Testversion, virtueller Computer, Lösungsvorlage, verwaltete App       |  Kostenlose Testversion, Bestellmöglichkeit, Testversion, Kontaktaufnahme, Beratungsangebot      |
|In-App-Erfahrung ermöglicht Benutzern Zugriff auf Apps und Dienste im Rahmen ihrer Anwendung  | Azure-Portal und Azure CLI         | Office 365, Dynamics 365, Power BI, Office-Client-Apps       |

## <a name="using-the-storefronts"></a>Verwenden der Storefronts

Jede Storefront erfüllt bestimmte Kundenanforderungen. Es ist eine Ausrichtung nach Rolle möglich, sodass Sie je nach Kunde jeweils die richtige Lösung oder den richtigen Dienst anbieten können.

Wecken Sie bei IT-Experten und Cloudentwicklern im Azure Marketplace das Interesse, IaaS-, SaaS- und PaaS-Lösungen zu entdecken, zu testen und zu erwerben:


|Kunde  |Azure Marketplace |
|---------|---------|
|**Benötigt zusätzliche Cloudplattformfunktionen zur Erfüllung geschäftlicher und technischer Anforderungen**     |  Bietet ein wachsendes Portfolio mit Ergänzungsanwendungen und -diensten, die für Azure optimiert sind       |
|**Hat Probleme, die passende Anwendung oder den passenden Dienst zu finden**    |  Bietet eine zentrale Anlaufstelle zum Entdecken, Testen und Erwerben von Lösungen und Diensten für Azure        |
|**Benötigt einen skalierbaren Bereitstellungsmechanismus für Drittanbieteranwendungen und -dienste**   | Ermöglicht die Erstellung und Konfiguration skalierbarer Bereitstellungen für Drittanbieteranwendungen und -dienste        |
|**Hat die Vorgabe, dass sich neue Anwendungen und Dienste in vorhandene Lösungen integrieren lassen und mit diesen zusammenarbeiten müssen**  |   Integriert Drittanbieteranwendungen und -dienste problemlos in vorhandene Lösungen in Azure      |

Wecken Sie bei Geschäftsbenutzern das Interesse, in AppSource branchenspezifische SaaS-Anwendungen und Implementierungsdienste zu entdecken, zu testen und abzurufen und so gute Geschäftsergebnisse zu erzielen und die Amortisationszeit zu reduzieren: 


|Kunde  |AppSource  |
|---------|---------|
|**Geschäftslösungen, die mit bereits vom Kunden verwendeten Microsoft-Produkten funktionieren** | Ermöglicht es Kunden, Anwendungen und Dienste von Drittanbietern zu verwenden, um Cloudanwendungen und Technologien von Microsoft zu erweitern.       |
|**Möglichkeit zum einfachen Suchen nach der geeigneten Lösung oder dem passenden Implementierungsdienst**    |   Ist eine zentrale Anlaufstelle zum Entdecken, Testen und Abrufen von Anwendungen und Diensten, Add-Ins und mehr.      |
|**Branchenspezifische Lösungen für die speziellen Geschäftsanforderungen**   | Bietet fertige End-to-End-Branchenlösungen für spezifische Anforderungen in zahlreichen Branchen.     |
|**Apps zur Optimierung der Produktivität, Effizienz und geschäftlichen Einblicke**    | Bietet Apps für verschiedene Branchen, z.B. Kundendienst, HR, Betrieb und viele mehr.        |
| **Erfahrener Implementierungspartner zur Unterstützung bei der Anpassung von Apps auf die spezifische Situation des Kunden** | Stellt einen Katalog mit Angeboten zu Beratungsdiensten für Lösungen basierend auf Dynamics 365, Power BI, PowerApps und Drittanbieter-Apps bereit, damit Geschäftsbenutzer vorhersagbare Ergebnisse erzielen können. |

## <a name="understanding-the-differences-between-storefronts"></a>Grundlegendes zu den Unterschieden zwischen Storefronts

Die Auswahl einer Storefront beginnt mit dem Identifizieren der Zielgruppe für Ihr Angebot. Azure Marketplace ist an den Anforderungen von IT-Experten und Entwicklern ausgerichtet. AppSource ist an den Anforderungen von Geschäftsbenutzern ausgerichtet. Falls sich Ihre Lösung an beide Zielgruppen richtet, müssen Sie für die Auflistung in beiden Storefronts nur eine Veröffentlichung durchführen.
 
Betrachten Sie die zusätzlichen Vorteile der einzelnen Storefronts:

|Storefront-Vorteil  |Azure Marketplace  |AppSource   |
|---------|---------|---------|
|**Abrechnungsflexibilität**    | Für die Optionen zur Abrechnung bei nutzungsbasierter Bezahlung von virtuellen Computern werden Microsoft Enterprise Agreements (EAs) oder Modelle für den Direktverkauf im Internet verwendet. Die Preisoptionen enthalten auch ein Free-Tarif-Abonnement als unbefristet kostenloses Angebot. Außerdem ist ein Abonnement vom Typ „Jetzt testen“ verfügbar, das als kostenloses Angebot für einen begrenzten Zeitraum gilt und anschließend in ein kostenpflichtiges Abonnement umgewandelt wird. Die „Bring-Your-Own-License“-Aktivierung ist eine weitere Option zur Unterstützung von Herausgebern. <br><br>Bei beiden Abrechnungsoptionen werden in Szenarien, bei denen virtuelle Computer mithilfe von Azure-Apps (z.B. Lösungsvorlagen oder verwaltete Apps) bereitgestellt werden, alle bereitgestellten Azure-Ressourcen direkt dem Kunden berechnet. | Bei AppSource kann eine Testversion bereitgestellt werden, aber derzeit keine handelstaugliche Veröffentlichungsoption. Sie können Ihre derzeitige Bestellungs- und Abrechnungsinfrastruktur ohne zusätzliche Investitionen oder Änderungen nutzen.        |
|**Verbindungen mit anderen Partnern**     |Azure Marketplace ermöglicht dem Herausgeber derzeit keine Verknüpfung eines Serviceanbieters oder Lieferpartners mit dem Angebot, aber diese Funktion wird 2018 eingeführt.         |  Unabhängige Softwarehersteller, Systemintegratoren und Anbieter von verwalteten Diensten können zur Erzielung bestimmter Implementierungsszenarien verknüpft werden. Diese Möglichkeit unterstützt den gemeinsamen Verkauf an neue Kunden.      |
|**Automation**     |    Azure Marketplace ermöglicht dem Herausgeber derzeit keine Verknüpfung eines Serviceanbieters oder Lieferpartners mit dem Angebot.     | Nutzen Sie den Vorteil der SaaS-Automatisierung mit Add-In-Bereitstellung. Verwenden Sie Lösungsvorlagen, um die SaaS-basierte Datensammlung und entsprechende Bereitstellungsszenarien zu automatisieren.        |Unabhängige Softwarehersteller, Systemintegratoren und Anbieter verwalteter Dienste können für bestimmte Implementierungsszenarien verknüpft werden, wobei der gemeinsame Verkauf an neue Kunden unterstützt wird.
|**Mehrere Cloudtypen**     |   Veröffentlichen Sie sowohl Lösungen für die öffentliche Cloud als auch lokale Lösungen über Azure Stack, oder veröffentlichen Sie in Azure Government und regionalen Clouds, z.B. Deutschland und China.      |    AppSource bietet derzeit keine Unterstützung für Azure Stack, Azure Government oder regionale Clouds.     |
|**Kontextbezogene Präsentation gegenüber Kunden**     |  Stellen Sie Ihre Lösung im Azure-Portal für die Kontextsuche (virtuelle Computer und Lösungsvorlagen) zur Verfügung.       |  Erreichen Sie über die In-App-Funktionalität für Microsoft-Produkte wie Dynamics 365, Power BI und Office 365 mehr Kunden.    |

## <a name="publishing-options"></a>Veröffentlichungsoptionen

Jede Storefront unterstützt mehrere Veröffentlichungsoptionen und Angebotstypen. Wählen Sie einen Angebotstyp aus, der Ihre Anwendung und Dienstdetails am besten widerspiegelt. Alle Veröffentlichungsoptionen bieten Partnern den gemeinsamen Zugriff auf Leads. 

|**Veröffentlichungsoption**  | **Angebotstyp** | **Storefront**  |
|---------|---------|---------|
|**Liste**    |    Kontaktaufnahme, Beratungsdienste     |  Azure Marketplace, AppSource       |
|**Testen**   |     Kostenlose Testversion, SaaS-Testversion, interaktive Demo, Testversion    |  Azure Marketplace, AppSource       |
|**Transaktion**     |   Virtueller Computer, Lösungsvorlage, verwaltete Anwendung      |    Azure Marketplace     |

### <a name="list"></a>Auflisten

Verwenden Sie die Kontaktaufnahme, wenn eine Beteiligung auf Testversions- oder Transaktionsebene nicht möglich ist. Der Vorteil dieses Ansatzes ist, dass Herausgeber, die eine marktinterne Lösung anbieten, dabei sofort Leads gewinnen und zu Erstabschlüssen führen können. Dies ist der Anfang des Umsatzwachstums. Der Nachteil ist jedoch, dass die Kundenbindung im Vergleich zu anderen Angebotstypen beschränkt ist.

>[!IMPORTANT]
>Die Kundenbindung ist am stärksten bei den Angebotstypen „Testen“ und „Transaktion“. Der Wert von „Kontaktaufnahme“ ist der Lead, den Sie erhalten. Wenn Sie also diesen Auflistungstyp auswählen, stellen Sie sicher, dass Ihr Leadziel konfiguriert ist und Sie darauf vorbereitet sind, den Lead zu maximieren. 

Wenn das Angebot im Wesentlichen aus Dienstleistungen (z.B. Bewertungen, Implementierungen, Workshops) besteht, sollten Sie den Angebotstyp „Beratungsdienste“ verwenden. Umfang, Dauer und Preis des Angebots müssen festgelegt werden, für einen einzelnen Kunden bestimmt sein und vor Ort durchgeführt werden.

### <a name="trial"></a>Testversion

Das Bereitstellen einer Testversion stärkt die Kundenbindung und ist damit eine nachhaltigere Präsentation Ihrer Lösung. Anhand einer Testversion können Kunden Ihre Lösung vor dem Kauf kennenlernen. Eine Testversion steigert Ihre Verkaufschancen in den Storefronts, und die dadurch erzielte Vernetzung mit den Kunden führt zu weiteren und lukrativeren Leads.
 
Alle Testoptionen werden in Ihrer Testumgebung und/oder Ihrem Azure-Abonnement bereitgestellt, anstatt in der Umgebung oder im Azure-Abonnement des Kunden. Tests sollten ohne zusätzliche Käufe vom Kunden durchgeführt werden können und höchstens minimalen zusätzlichen Konfigurationsaufwand zur Durchführung eines einfachen Anwendungsfalls erfordern. Testversionen müssen mindestens für die Dauer des Testzeitraums kostenlosen Support enthalten. Testbenutzer sollten während der Bewertungsphase aufmerksam betreut werden, um optimale Resultate zu erzielen. Herausgeber sollten zum Beobachten und Verwalten von Testbenutzern sowohl Marketplace-Leads als auch ihre eigene In-App-Intelligence nutzen.

Es gibt drei typische Testszenarien:


|**Testoption**  |**Hauptvorteile**  |**Wählen Sie diese Option in den folgenden Fällen:**  |
|---------|---------|---------|
|**Kostenlose Testversion**    |     Ermöglicht es einem Kunden, Ihr Produkt vor dem Kauf auszuprobieren, und später kann automatisch die Umstellung auf die kostenpflichtige Nutzung durchgeführt werden. Außerdem können Kunden Proofs of Concept erstellen und sich mit Microsoft-Vertriebsteams vernetzen. |     Ihre Lösung ist ein virtueller Computer oder eine Lösungsvorlage.<br><br> Ihre Lösung ist ein SaaS-Angebot, und Sie bieten ein mehrinstanzenfähiges SaaS-Produkt an. <br><br>Sie verfügen über eine Umgebung für die erstmalige Ausführung, um Kunden den Einstieg zu erleichtern. <br><br>Sie verfügen nur über einen Mandanten, aber Sie fügen Kunden als Gastbenutzer hinzu.|
**Testversion**     |     Ermöglicht es einem Kunden, Ihr Produkt vor dem Kauf auszuprobieren. Außerdem kann Ihre Lösung anhand einer Anleitung mit einem vorkonfigurierten Setup getestet werden. |   Ihre Lösung ist ein virtueller Computer, eine Lösungsvorlage oder eine SaaS-App mit nur einem Mandanten, oder die Bereitstellung ist kompliziert. <br><br>Sie verfügen nicht über eine Methode, mit der Ihre Testversion in ein kostenpflichtiges Angebot umgewandelt werden kann. |
|**Interaktive Demo**    |  Ermöglicht es Kunden, Ihr Produkt ohne die Komplexität des Setups in Aktion zu sehen.       |    Ihre Lösung erfordert ein komplexes Setup, das in der Testphase schwierig durchzuführen wäre.     |


#### <a name="free-trial"></a>Kostenlose Testversion

Verwenden Sie eine kostenlose Testversion, wenn die Lösung oder App eine kostenlos zu testende, SaaS-basierte Testversion umfasst. Mit dieser Option werden aus interessierten Kunden hochwertige Leads, die Ihr Geschäftswachstum ankurbeln. Kostenlose Testversionen können als Testkonten mit eingeschränkter Nutzung oder begrenzter Dauer angeboten werden. Sie sollten eine Handlungsaufforderung (Call-to-Action) enthalten, um die Umwandlung in die kostenpflichtige Nutzung Ihrer Software zu beschleunigen.

#### <a name="test-drive"></a>Testversion

Verwenden Sie eine Testversion, wenn die Lösung über eine oder mehrere VMs mit IaaS- oder SaaS-Apps bereitgestellt wird. Der Vorteil dieses Ansatzes ist die automatisierte Bereitstellung eines virtuellen Geräts oder der gesamten Lösungsumgebung in einer vom Partner gehosteten „interaktiven Tour“ der Lösung zur Auswertung durch den Kunden, ohne das ihm zusätzliche Kosten entstehen. Der Kunde muss nicht bereits Azure-Kunde sein, um zu hochwertigeren Leads zu führen.

Eine Testversion hat noch weitere Vorteile:

- 27% der Benutzersuchen im Marketplace werden von Benutzern so definiert, dass nur Angebote mit Testversionen angezeigt werden. 
- Aus Angeboten mit Testversion resultieren 38% mehr Leads als aus Angeboten ohne Testversion. 
- 36% der neu gewonnenen Kunden im Marketplace haben eine Testversion genutzt. 
- Mit Testversionen können Microsoft-Verkäufer Ihr Produkt besser verstehen und im Co-Selling verwenden.

#### <a name="interactive-demo"></a>Interaktive Demo

Führen Sie Ihre Kunden mit einer interaktiven Demo durch Ihr Produkt. Der Vorteil dieser Option ist, dass Sie ohne komplizierte Bereitstellung für komplexe Lösungen eine Testversion anbieten können. Mit dieser Option können sich Kunden in der Lösung umsehen. Sie ermöglicht es Herausgebern, Leads zu gewinnen, die zu den ersten Abschlüssen weiterentwickelt werden können, mit denen das Geschäftswachstum beginnt. 

### <a name="transaction"></a>Transaktion

Verwenden Sie im Azure Marketplace eine *VM*, wenn die Lösung als virtuelles Gerät im Abonnement des Kunden bereitgestellt wird. Virtuelle Computer sind über nutzungsbasierte oder BYOL-fähige Lizenzierungsmodelle vollständig handelstauglich. Microsoft hostet die geschäftliche Transaktion und führt für den Herausgeber die Abrechnung mit dem Kunden durch. Die Herausgeber haben den Vorteil, dass sie die bevorzugte Zahlungsmethode des Kunden bei Microsoft einschließlich des Enterprise Agreements nutzen können. 

>[!NOTE]
>Derzeit können die finanziellen Zusagen eines Enterprise Agreements für die Azure-Nutzung virtueller Geräte genutzt werden, aber nicht für die Softwarelizenzgebühren des Herausgebers.

Verwenden Sie eine *Azure-Lösungsvorlage*, wenn für eine Lösung zusätzlich zum virtuellen Gerät noch eine zusätzliche Bereitstellung und Konfigurationsautomatisierung erforderlich ist. Lösungsvorlagen können die Bereitstellung mindestens einer VM-Ressource automatisieren und Netzwerk- und Speicherressourcen bereitstellen. Lösungsvorlagen können auf einzelnen VMs und in gesamten IaaS-basierten Lösungsumgebungen zu Automatisierungsvorteilen führen. Weitere Informationen zum Erstellen von Lösungsvorlagen finden Sie auf [GitHub](https://github.com/MicrosoftDocs/azure-docs).

Verwenden Sie eine *verwaltete Azure-App*, wenn Sie einen virtuellen Computer oder eine vollständig IaaS-basierte Lösung für das Abonnement eines Kunden bereitstellen und der Herausgeber oder Kunde wünscht, dass die Lösung von einem Drittanbieter (z.B. einem SI oder MSP) verwaltet wird. Weitere Informationen zum Erstellen von verwalteten Apps finden Sie unter [Übersicht über verwaltete Azure-Anwendungen](https://docs.microsoft.com/azure/managed-applications/overview). Eine Liste mit häufig gestellten Fragen finden Sie unter [Häufig gestellte Fragen zum Marketplace](https://azure.microsoft.com/marketplace/faq/).

>[!NOTE]
> Verwaltete Apps müssen im gesamten Marketplace bereitgestellt werden können. In Bezug auf die Kundenkommunikation ist zu beachten, dass Sie interessierte Kunden erreichen können, wenn der gemeinsame Zugriff auf Leads aktiviert ist.

### <a name="azure-certified-program"></a>Azure Certified-Programm

Alle im Azure Marketplace veröffentlichten virtuellen Computer werden für das Azure Certified-Programm getestet. Das Programm hat die folgenden Aufgaben:

- Es gibt Kunden die Sicherheit, dass ihr virtueller Computer mit der Azure Platform und dem Marketplace-Verkaufsmodell kompatibel ist.
- Es führt Tests auf die Sicherheitskonformität von Onlineimages, inklusive Viren und Schadsoftware, durch.
- Es ermöglicht Badging auf Angebotsebene, um die Verkaufschancen als überprüfte Lösung bei Microsoft-Unternehmenskunden zu verbessern.

#### <a name="marketplace-commercial-considerations"></a>Kommerzielle Aspekte im Marketplace

Es werden keine Gebühren für Ihre Teilnahme am Marketplace berechnet. Bei der Teilnahme am Marketplace wird bei der Veröffentlichung mit den Optionen Auflisten, Testversion und BYOL-Transaktion kein Umsatzanteil fällig. Weitere Informationen finden Sie in den [Marketplace-Teilnahmerichtlinien](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

#### <a name="pay-as-you-go-and-bring-your-own-license-billing-options"></a>Abrechnungsoptionen „Nutzungsbasierte Zahlung“ und „BYOL“

Wenn Sie eine nutzungsbasierte Transaktion als Veröffentlichungsoption verwenden, wird Ihr nutzungsbasierter Softwarelizenzierungsumsatz zwischen Ihnen und Microsoft im Verhältnis 80%/20% aufgeteilt. Ein einzelnes Produkt kann sowohl nach dem nutzungsbasierten als auch nach dem BYOL-Abrechnungsmodell angeboten werden und damit auf Angebotsebene in separaten SKUs vorliegen. Sie können dies in Ihrem Angebot im Cloud-Partnerportal konfigurieren.

Betrachten Sie das folgende Beispiel.

Wenn Sie die nutzungsbasierte Abrechnung als Option aktivieren, gilt Folgendes:


|Ihre Lizenzkosten   | 1,00 USD pro Stunde        |
|---------|---------|
|Azure-Nutzungskosten (D1/1-Kern)     | 0,14 USD pro Stunde  |
|**Microsoft führt die Abrechnung mit dem Kunden durch**    | **1,14 USD pro Stunde**       |

In diesem Szenario berechnet Microsoft 1,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images.


|**Microsoft berechnet** |**1,14 USD pro Stunde**  |
|---------|---------|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten | 0,80 USD pro Stunde        |
|Microsoft behält 20% Ihrer Lizenzkosten ein    | 0,20 USD pro Stunde        |
|Microsoft behält die Kosten der Azure-Nutzung ein     |   0,14 USD pro Stunde      |

Wenn Sie BYOL als Option aktivieren:

|Ihre Lizenzkosten     | Ausgehandelte und vom Herausgeber in Rechnung gestellte Lizenzgebühren        |
|---------|---------|
|Azure-Nutzungskosten (D1/1-Kern)    | 0,14 USD pro Stunde         |
|**Microsoft führt die Abrechnung mit dem Kunden durch**     | **0,14 USD pro Stunde**        |

In diesem Szenario berechnet Microsoft 0,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images. 

|**Microsoft berechnet**    |   **0,14 USD pro Stunde**      |
|---------|---------|
|Microsoft behält die Kosten der Azure-Nutzung ein     |    0,14 USD pro Stunde     |
|Microsoft behält 0% Ihrer Lizenzkosten ein     |  0,00 USD pro Stunde       |

### <a name="single-billing-and-payment-methods"></a>Einzelabrechnung und Zahlungsmethoden

Ein wesentlicher Vorteil der Verwendung der Veröffentlichungsoption „Transaktion“ ist, dass Microsoft Ihre Lizenzkosten zur gleichen Zeit wie die zugrunde liegende Azure-Nutzung direkt beim Kunden einzeln abrechnen kann. In diesem Szenario rechnet Microsoft in Ihrem Auftrag ab, sodass Sie keine eigene Beschaffungsbeziehung mit dem Kunden erstellen müssen. Dadurch sparen Sie Zeit und Ressourcen, sodass Sie sich auf den Abschluss des Verkaufs konzentrieren können und nicht mit der Abrechnung befassen müssen.

### <a name="enterprise-agreement"></a>Enterprise Agreement

Microsoft-Kunden verwenden gelegentlich ein Enterprise Agreement, um für Microsoft-Produkte, einschließlich Azure-Nutzung, zu zahlen. Diese Zahlungsoption ist für Organisationen gedacht, die Software und Clouddienste für einen Drei-Jahres-Mindestzeitraum lizenzieren möchten. Die Kunden haben die Möglichkeit, die Zahlungen zu verteilen, anstatt eine Vorauszahlung zu leisten. Wenn ein EA-Kunde die nutzungsbasierte Transaktionsauflistung verwendet, folgt die Abrechnung für die Softwarelizenzkosten des Herausgebers dem vierteljährlichen EA-Überschreitungsabrechnungszyklus.

### <a name="monetary-commitment"></a>Finanzielle Verpflichtung 

Jeder Enterprise Agreement-Kunde kann durch eine vorherige, Azure betreffende finanzielle Verpflichtung Azure seinem Enterprise Agreement hinzufügen. Diese Verpflichtung wird im Laufe des Jahres durch die Verwendung beliebiger Kombinationen der verschiedenen Clouddienste verbraucht, die Azure über die globalen Datencenter anbietet.

## <a name="prerequisites-for-marketplace-publishing"></a>Voraussetzungen für die Veröffentlichung im Marketplace

### <a name="prerequisites-for-all-marketplace-publishing-options"></a>Voraussetzungen für alle Marketplace-Veröffentlichungsoptionen


|**Anforderung**  |**Details**  |**Veröffentlichungsoption**  |
|---------|---------|---------|
|**Teilnahmerichtlinien**    | Lesen Sie sich die [Teilnahmerichtlinien](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) für Azure Marketplace durch.       | Auflisten, Testversion, Transaktion        |
|**Integration in Microsoft**    | Für Azure Marketplace-Angebote sollten Microsoft Azure-Diensttypen, z.B. Compute, Netzwerk oder Speicher, genutzt bzw. erweitert werden. Sie sollten an einer vorhandenen Azure Marketplace-Kategorie ausgerichtet sein, z.B. Datenbanken, Sicherheit und Netzwerk. [Die vollständige Liste finden Sie hier](https://azuremarketplace.microsoft.com/marketplace/apps).        | Auflisten, Testversion, Transaktion        |
|**Zielgruppe**    | Azure Marketplace-Angebote müssen sich an IT-Experten, Cloudentwickler oder andere technische Kundenrollen richten.       |  Auflisten, Testversion, Transaktion 
|**Leadverwaltung**    | Um Leads vom Marketplace zu erhalten, müssen Sie Ihr Kundenbeziehungsmanagement (Customer Relationship Management, CRM) (Marketo, Microsoft Dynamics oder Salesforce) für den Empfang von Leaddaten aktivieren.        |   Auflisten, Testversion, Transaktion      |
|**Datenschutz- und Nutzungsrichtlinien**     |   Ihre Datenschutzrichtlinie muss über eine öffentliche URL verfügbar sein. Die Nutzungsbedingungen müssen während der Veröffentlichung als Text eingegeben werden.      |   Auflisten, Testversion, Transaktion      |
|**Unterstützung**     |  Ihr Angebot muss eine öffentlich zugängliche Support-URL enthalten, unter der Kunden Hilfe bekommen können. Für Testversionen muss der Support im Testzeitraum ohne zusätzliche Kosten geleistet werden.       |  Testversion, Transaktion       |

### <a name="prerequisites-specific-to-trial-publishing"></a>Besondere Voraussetzungen für die Veröffentlichung von Testversionen

|**Anforderung**  | **Details**  |**Veröffentlichungsoption**  |
|---------|---------|---------|
|**Kostenloser Testzeitraum und Testversion**     |  Ein Kunde muss Ihre App für begrenzte Zeit kostenlos verwenden können.<br><br>Dies bedeutet, dass der Kunde weder Lizenz- bzw. Abonnementgebühren für Ihr Produkt noch die Kosten des zugrunde liegenden Erstanbieterprodukts oder -diensts von Microsoft zahlen muss. Da alle Testoptionen für das Microsoft-Produktabonnement des Herausgebers bereitgestellt werden, hat er die alleinige Kontrolle über die Optimierung und Verwaltung der Testkosten.<br><br>Sie können eine kostenlose Testversion, eine interaktive Demo oder eine Testversion auswählen. Unabhängig von Ihrer Wahl muss die kostenlose Testversion dem Kunden ausreichend Zeit lassen, die App ohne zusätzliche Kosten zu testen.<br><br>Beginnen Sie mit dem Prozess zur Erstellung einer Testversion, indem Sie sich an cloudmarketplace@microsoft.com wenden. <br><br>Beachten Sie, dass Azure Marketplace-SaaS-Testversionen es Benutzern ermöglichen müssen, sich mit ihren Anmeldeinformationen für Active Directory anzumelden. [Weitere Informationen](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences) |   Testversion      | 
| **Leicht konfigurierbare, schlüsselfertige Lösung**    |  Ihre App muss sich einfach und schnell konfigurieren und einrichten lassen.       |  Testversion       |
|**Verfügbarkeit/Betriebszeit**    |    Ihre SaaS-App oder -Plattform benötigt eine Betriebszeit von mindestens 99,9 %.     |    Testversion     |
|**Azure Active Directory**    |    Ihr Angebot muss die einmalige Azure Active Directory-Verbundanmeldung (SSO) mit aktivierter Zustimmung zulassen.      |  Testversion|

### <a name="prerequisites-specific-to-transaction-publishing"></a>Besondere Voraussetzungen für die Transaktionsveröffentlichung


|**Anforderung**  |**Details** |**Veröffentlichungsoption**  |
|---------|---------|---------|
|**Abrechnung und Messung**    |  Ihr virtueller Computer muss entweder die BYOL- oder die nutzungsbasierte monatliche Abrechnung zulassen.       |    Transaktion    |
|**Azure-kompatibler virtueller Datenträger (Virtual Hard Disk, VHD)**     |   Virtuelle Computer müssen unter [Windows](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation) oder [Linux](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation) erstellt werden.    |   Transaktion      |

### <a name="prerequisites-specific-to-consulting-services-publishing"></a>Besondere Voraussetzungen für die Veröffentlichung von Beratungsdiensten


|**Anforderungen** |**Details**  |**Veröffentlichungsoption**  |
|---------|---------|---------|
|**Merkmale des Dienstangebots**     | Für Ihre Beratungsdienste muss Folgendes gelten: <br>- Angebot mit festem Umfang, fester Dauer und festem Preis (oder kostenlos) <br>- Vorrangige Ausrichtung auf den Pre-Sales-Bereich <br>- Beschränkung auf einen einzelnen Kunden <br>- Durchführung vor Ort        |    Auflisten     |
|**Partneranforderungen für Beratungsdienste**    |   *Nur AppSource*:  <br>- **Dynamics 365 for Customer Engagement**: [Cloud Customer Relationship Management](https://partner.microsoft.com/en-us/membership/cloud-customer-relationship-management-competency)-Kompetenz Silber oder Gold. <br>- **Dynamics 365 for Finance and Operations Enterprise-Edition**: Die Kompetenz Silber oder Gold für [Enterprise Resource Planning](https://partner.microsoft.com/en-us/membership/enterprise-resource-planning-competency) und ein Mindestumsatz von 25.000 USD mit Cloudvorgängen in den letzten 12 Monaten. <br>- **Dynamics 365 for Finance and Operations, Business-Edition**: Tätigkeit als [Cloud-Dienstanbieter (Cloud Services Provider, CSP)](https://partner.microsoft.com/en-us/cloud-solution-provider) oder [Digital Partner of Record (DPOR)](https://partner.microsoft.com/en-us/membership/digital-partner-of-record) für mindestens einen Kunden. <br>- **Power BI**: Erfüllung der Kriterien für [Lösungspartner](file:///C:/Users/ellacroi/Downloads/BI%20Partner%20Program%20Overview%20 & % 20Incentives.pdf). <br>- **PowerApps**: Eine Lösung zur [Vorstellung der Partner](https://powerapps.microsoft.com/en-us/partner-showcase/). |    Auflisten     |

## <a name="using-azure-active-directory-to-enable-trials"></a>Verwenden von Azure Active Directory, um Tests zu ermöglichen
Azure Active Directory ist ein Cloudidentitätsdienst, der die Authentifizierung mit einem Geschäfts- oder Schulkonto von Microsoft mithilfe der branchenüblichen Protokolle OAuth und OpenID Connect ermöglicht. Weitere Informationen zu Azure AD finden Sie auf der [Webseite zum Produkt](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory-features). 

Microsoft authentifiziert alle Marketplace-Benutzer per Azure AD. Wenn ein authentifizierter Benutzer im Marketplace durch Ihr Testversionsangebot klickt und zu Ihrer Testumgebung geleitet wird, können Sie dem Benutzer ohne zusätzlichen Anmeldungsschritt direkt eine Testversion bereitstellen. Das [Token, das Ihre App während der Authentifizierung von Azure AD erhält](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens), enthält wertvolle Benutzerinformationen, die Sie zum Erstellen eines Benutzerkontos in Ihrer App verwenden können. Sie können dann den Ablauf der Bereitstellung automatisieren und die Wahrscheinlichkeit für eine Konvertierung erhöhen. 

Wenn Sie Azure AD zum Ermöglichen der Authentifizierung per Klick für Ihre App oder Testversion verwenden, bewirkt dies Folgendes:

- Benutzerfreundlicher Prozess vom Marketplace zur Testumgebung. 
- Das Gefühl einer „produktinternen Erfahrung“ bleibt erhalten, wenn der Benutzer vom Marketplace zu Ihrer Domäne bzw. Testumgebung umgeleitet wird.
- Geringere Wahrscheinlichkeit des Kundenverlusts bei der Umleitung, weil kein zusätzlicher Anmeldungsschritt vorhanden ist.
- Weniger Bereitstellungshindernisse für den Großteil der Azure AD-Benutzer.

### <a name="certify-your-azure-ad-integration-for-the-marketplace-multitenant-applications"></a>Zertifizieren Ihrer Azure AD-Integration für den Marketplace: Mehrinstanzenfähige Anwendungen

Wenn Sie Azure AD bereits unterstützen:

- Registrieren Sie Ihre Anwendung im Azure-Portal.
- Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um für die Testversion die Nutzung per einfachem Klick zu ermöglichen.
- [Weitere Informationen](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

Wenn die einmalige Azure AD-Verbundanmeldung für Sie neu ist:

- Registrieren Sie Ihre Anwendung im Azure-Portal.
- Entwickeln Sie SSO mit Azure AD mithilfe von [OpenID Connect](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-openid-connect-code) oder [OAuth 2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code).
- Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um für die Testversion die Nutzung per einfachem Klick zu ermöglichen.
- [Weitere Informationen](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified)

### <a name="certify-your-azure-ad-integration-for-the-marketplace-single-tenant-applications"></a>Zertifizieren Ihrer Azure AD-Integration für den Marketplace: Anwendungen mit nur einem Mandanten

Es gibt mehrere Optionen für Anwendungen mit einem Mandanten:

- Hinzufügen von Benutzern als Gastbenutzer zu Ihrem Verzeichnis mit [Azure B2B](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- Manuelles Bereitstellen von Testversionen für Kunden über „Kontaktaufnahme“
- Entwickeln einer Testversion pro Kunde
- Erstellen einer mehrinstanzenfähigen Beispiel-Demo-App mit SSO

## <a name="publishing-processes-by-product-for-office-dynamics-and-power-bi"></a>Veröffentlichungsprozesse nach Produkt für Office, Dynamics und Power BI
Weitere Informationen zu den speziellen Anforderungen für AppSource-Apps, mit denen Office, Dynamics und Power BI erweitert werden, erhalten Sie in der produktspezifischen Dokumentation in diesem Abschnitt. 


|Produkt |Informationen zur Veröffentlichung  |
|---------|---------|
|Office 365     |    Lesen Sie die Informationen zum [Veröffentlichungsprozess und den Richtlinien]( https://docs.microsoft.com/en-us/office/dev/store/submit-to-the-office-store).     |
|Dynamics 365 for Finance and Operations  |   Lesen Sie bei der Erstellung für die Enterprise Edition die Informationen zum [Veröffentlichungsprozess und den Richtlinien](https://docs.microsoft.com/en-us/dynamics365/unified-operations/dev-itpro/lcs-solutions/lcs-solutions-app-source).      |
|Dynamics 365 for Customer Engagement |Lesen Sie die Informationen zum [Veröffentlichungsprozess und den Richtlinien](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/publish-app-appsource). |
|Power BI   |     Lesen Sie die Informationen zum [Veröffentlichungsprozess und den Richtlinien]( https://docs.microsoft.com/en-us/power-bi/developer/office-store).    |
|Cortana Intelligence     |    Lesen Sie die Informationen zu [Cortana in AppSource](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-intelligence-appsource-publishing-guide).     |
|AppSource-Beratungsangebote     |  Lesen Sie die [Informationen zu den Richtlinien, und erhalten Sie Informationen zum Übermitteln Ihres Angebots](https://smp-cdn-prod.azureedge.net/documents/Microsoft%20AppSource%20Partner%20Listing%20Guidelines.pdf).    |



## <a name="cloud-partner-portal-pre-publishing-checklist-for-the-azure-marketplace"></a>Vorveröffentlichungs-Prüfliste im Cloud-Partnerportal für Azure Marketplace

Es ist hilfreich, wenn Sie sich vor Beginn des Veröffentlichungsprozesses mit den erforderlichen Komponenten für die Erstellung eines Angebots vertraut machen. Die folgenden Artefakte sind zum Abschluss des Workflows zum Veröffentlichen eines Angebots im Cloud-Partnerportal erforderlich. 

### <a name="storefront-details"></a>Storefront-Details


|Erforderliches Veröffentlichungsartefakt  |Angebotstyp  |
|---------|---------|
|**Angebotsname (200 Zeichen) und -beschreibung (2.000 Zeichen)**    |  Alle        |
|**MPN-ID (Microsoft Partner Network)**   |  Alle       |
|**Verfügbarkeit in Land/Region**   | Alle        |
|**Dauer des Projekts**     |   Beratungsdienste      |
|**Geeignete Branchen, Kategorien und Suchschlüsselwörter**     |  Alle       |
|**Unternehmenslogos (48 x 48, 216 x 216)**     |  Beratungsdienste       |
|**Produktübersichtsvideo (optional)**  |  Alle       |
|**Screenshots (max. 5, 1.280 x 720)**   |    Alle     |
|**Marketingdokumente (max. 3)**    |  Alle       |
|**Leadzielgruppe**    |   Alle      |

### <a name="contacts"></a>Kontakte


|Erforderliches Veröffentlichungsartefakt  |Angebotstyp  |
|---------|---------|
|**Kontaktinformationen (Support, Engineering, kommerziell)**    |    Alle     |

### <a name="technical-info"></a>Technische Informationen


|Erforderliches Veröffentlichungsartefakt  |Angebotstyp |
|---------|---------|
|**Test-URL**     |  Alle Arten von Testversionsangeboten       |
|**Unterstützte Sprachen**    |   Alle Arten von Testversionsangeboten      |
|**Versionsnummer und Veröffentlichungsdatum der App**    |   Alle Arten von Testversionsangeboten      |
|**Support-URL**    |   Alle Arten von Testversionsangeboten, virtuelle Computer      |
|**Nutzungsbedingungen- und Datenschutzrichtlinien-URL**     |    Alle     |

### <a name="test-drive"></a>Testversion


|Erforderliches Veröffentlichungsartefakt  |Angebotstyp  |
|---------|---------|
|**Beschreibung und Dauer**     |  Nur Testversion       |
|**Benutzerhandbuch**     |   Nur Testversion      |
|**Testversionsvideo (max. 1)**     |  Nur Testversion       |
|**Verfügbarkeit der Testversion in Land/Region**    |   Nur Testversion      |
|**Name der Azure-Ressourcengruppe**   |         |
|**Azure-Abonnement-ID**     |  Nur Testversion       |
|**Azure AD-Mandanten-ID**   |    Nur Testversion     |
|**Azure AD-App-ID**  |  Nur Testversion       |
|**Azure AD-App-Schlüssel**     |   Nur Testversion      |

### <a name="storefrontmarketplace"></a>Storefront/Marketplace


|Erforderliches Veröffentlichungsartefakt  |Angebotstyp  |
|---------|---------|
|**Titel (max. 50 Zeichen)**    |  Transaktion: Virtuelle Computer, Azure-Apps (Lösungsvorlagen und verwaltete Apps)       |
|**Zusammenfassung (max. 200 Zeichen)**    |  Transaktion: Virtuelle Computer, Azure-Apps (Lösungsvorlagen und verwaltete Apps)       |
|**Lange Zusammenfassung (max. 256 Zeichen)**     |   Transaktion: Virtuelle Computer, Azure-Apps (Lösungsvorlagen und verwaltete Apps)      |
|**HTML-basierte Beschreibung (max. 3.000 Zeichen)**    |  Transaktion: Virtuelle Computer, Azure-Apps (Lösungsvorlagen und verwaltete Apps)       |
|**Unternehmenslogos (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)**    |  Transaktion: Virtuelle Computer, Azure-Apps (Lösungsvorlagen und verwaltete Apps)       |

### <a name="sku"></a>SKU


|Erforderliches Veröffentlichungsartefakt  |Angebotstyp  |
|---------|---------|
|**Versionsnummer**     |    Transaktion: Azure-Apps (Lösungsvorlagen und verwaltete Apps)     |
|**Paketdatei mit allen Vorlagendateien und createUIDefinitionFile**   |Transaktion: Azure-Apps (Lösungsvorlagen und verwaltete Apps)         |
|**Betriebssystemdetails**    |   Transaktion: virtuelle Computer      |
|**Verwendete Ports und Protokolle**    |  Transaktion: virtuelle Computer       |
|**Datenträgerversion und SAS-URL für jede verwendete VHD**   |  Transaktion: virtuelle Computer       |

## <a name="becoming-a-publisher"></a>Weg zum Herausgeber

In diesem Abschnitt werden die folgenden Schritte beschrieben:

- Der Weg zum Herausgeber im Azure Marketplace und AppSource.
- Zugreifen auf das Cloud-Partnerportal. Sie nutzen dieses Portal zum Erstellen, Veröffentlichen und Verwalten Ihres Angebots. 

### <a name="process-overview"></a>Prozessübersicht


|Schritt für die Marketplace-Registrierung  |Zeit  |BESCHREIBUNG  |
|---------|---------|---------|
| Registrieren im Microsoft Partner Network | 15 Min. | Herausgeber müssen im Microsoft Partner Network (MPN) registriert sein, um die erste Ebene der Kontoüberprüfung durchführen zu können und zusätzliche Vorteile und Support als Azure Marketplace-Herausgeber zu erhalten. |
|Erstellen einer Microsoft-ID     |   15 Minuten      |   Partner benötigen eine Microsoft-ID. Diese Microsoft-ID wird für den Zugriff auf das Cloud-Partnerportal benötigt.       |
|Übermitteln des Marketplace-Antragsformulars     |  1 - 3 Tage       |  Partner müssen das Antragsformular übermitteln, um den Genehmigungsprozess für den Marketplace zu starten. Nachdem das Formular übermittelt wurde, überprüft das Marketplace-Onboardingteam die Anwendung und die Anforderung.       |
|Registrieren im Developer Center     |    5 - 10 Tage     | Die Registrierung im Microsoft Developer Center ist erforderlich, damit Microsoft überprüfen kann, ob der Partner eine gültige juristische Person mit gültiger Steuernummer in dem Land ist, in dem er registriert ist. Das Developer Center ermöglicht dem Partner, ein registrierter Microsoft-Entwickler zu werden, und gewährt ihm Zugriff auf das Azure-Entwicklerprogramm. <br><br>Beachten Sie Folgendes: Wenn Sie das Antragsformular für den Marketplace nicht ausgefüllt haben, fällt eine Registrierungsgebühr in Höhe von 99 USD an. Füllen Sie das Marketplace-Antragsformular aus, um einen Werbeaktionscode per E-Mail zu erhalten und diese Gebühr zu umgehen.  |
|Anmelden am Cloud-Partnerportal     |  15 Minuten       |   Nachdem der Partner vom Marketplace-Team erfahren hat, dass sein Antrag genehmigt wurde, hat er Zugriff auf das [Cloud-Partnerportal](https://cloudpartner.azure.com/). Der Partner sollte die Microsoft-ID aus dem Antragsformular verwenden, um sich im Cloud-Partnerportal an seinem Herausgeberprofil anzumelden. Nach der Registrierung im Developer Center muss der Partner sein Developer Center-Konto mit seinem Herausgeberprofil im Azure Marketplace verknüpfen, damit er Angebote veröffentlichen kann.      |

#### <a name="create-a-microsoft-id"></a>Erstellen einer Microsoft-ID

Während des gesamten Marketplace-Veröffentlichungsprozesses verwenden Sie eine E-Mail-Adresse, die das Marketplace-Konto identifiziert. Diese E-Mail-Adresse muss als Microsoft-ID registriert werden und wird sowohl für das [Microsoft Developer Center](https://developer.microsoft.com/) als auch das [Cloud-Partnerportal](https://cloudpartner.azure.com/) verwendet. 

Sie sollten für Ihre Azure Marketplace- und AppSource-Angebote nur ein Microsoft-ID-Konto verwenden. Es wird dringend empfohlen, es nicht auch für andere Dienste oder Angebote zu nutzen.

Die ausgewählte E-Mail-Adresse sollte idealerweise zu Ihrer Unternehmensdomäne gehören und von Ihrem IT-Team verwaltet werden. Richtlinien zur Erstellung einer ID finden Sie in den Abschnitten unter [Richtlinien und Leitfäden](#guidelines-and-how-tos). 

#### <a name="register-in-microsoft-partner-network"></a>Registrieren im Microsoft Partner Network 
Wenn Sie zu einem Azure Marketplace- oder AppSource-Herausgeber werden, geht Ihr Unternehmen eine Partnerschaft mit Microsoft ein. Durch die Teilnahme am Microsoft Partner Network (MPN) erhalten Sie Zugang zu einigen grundlegenden Vorteilen, damit Ihr Unternehmen die technischen Lösungen entwickeln und sein Geschäft ausbauen kann (z.B. technischen Support). Wenn Sie als Herausgeber auf dem Marketplace aktiv sind, ist dies auch mit Vorteilen in Bezug auf das Microsoft Partner Network verbunden. Greifen Sie auf die Seite zum [Microsoft Partner Network](https://partner.microsoft.com/en-us/membership/) zu, um die MPN-Registrierung durchzuführen. Sie sollten überprüfen, ob Ihr Unternehmen bereits beim MPN registriert ist. Nach der Registrierung bitten wir Sie um Ihre MPN-ID, damit wir Ihr Konto und Ihr Herausgeberprofil im [Cloud-Partnerportal](https://cloudpartner.azure.com/) überprüfen können. 

#### <a name="submit-the-marketplace-nomination-form"></a>Übermitteln des Marketplace-Antragsformulars
Im Rahmen des Onboarding-Prozesses für den Marketplace müssen Sie ein Antragsformular übermitteln. Das Formular enthält Informationen zu Ihrem Anwendungs- oder Dienstangebot, Ihre Unternehmensinformationen und die Supportebene, die Sie bereitstellen möchten. 

- [Antragsformular für Azure Marketplace](http://aka.ms/listonazuremarketplace)   
- [Antragsformular für AppSource](http://aka.ms/listonappsource)

Nachdem Sie das Formular übermittelt haben, überprüft das Marketplace-Team die Anwendung und die Anforderung. Nach der Überprüfung der Anforderung werden Sie per E-Mail über die nächsten Schritte informiert, die Sie ausführen müssen, um ein genehmigter Partner im Cloud-Partnerportal zu werden.

#### <a name="register-in-the-developer-center"></a>Registrieren im Developer Center

Das [Microsoft Developer Center](https://developer.microsoft.com/) ist zum Veröffentlichen von Anwendungen erforderlich, die über Transaktionsfunktionen verfügen, z.B. virtuelle Computer, Lösungsvorlagen und verwaltete Azure-Apps. Aufgrund dieser Anforderung kann Microsoft die Informationen zu den rechtlichen, steuerlichen und bankbezogenen Entitäten Ihres Unternehmens überprüfen. Die Person, die das Konto registriert, muss ein offizieller Vertreter des Unternehmens sein und ihre persönlichen Informationen zur Identitätsüberprüfung bereitstellen. Diese Person muss eine Microsoft-ID verwenden, die für das Unternehmen freigegeben ist. Das gleiche Konto muss im [Cloud-Partnerportal](https://cloudpartner.azure.com/) verwendet werden. 

>[!IMPORTANT]
>Stellen Sie sicher, das Ihr Unternehmen nicht bereits über ein Microsoft Developer Center-Konto verfügt, bevor Sie versuchen, die Erstellung durchzuführen.

Während des Prozesses erfassen wir Informationen zu Unternehmensadresse, Bankdaten und Steuer. Diese Informationen erhalten Sie üblicherweise über Bank- oder Geschäftskontakte. Außerdem müssen Sie folgende Komponenten des Herausgeberprofils angeben, um die verschiedenen Phasen der Angebotserstellung und -bereitstellung durchlaufen zu können:


|**Herausgeberprofil**  |**Startprofil**  |**Staging**  |**Auflisten und Testen**  |**Transaktion**
|---------|---------|---------|---------|---------|
|**Unternehmensregistrierung**     | Verpflichtend        |  Verpflichtend       | Verpflichtend        |  Verpflichtend       |
|**Steuer-ID**   |    Optional     |    Optional     |  Optional       | Verpflichtend      |
|**Bankkonto**     |   Optional      |    Optional     |  Optional       |  Verpflichtend      |

Eine Schritt-für-Schritt-Beschreibung dieses Prozesses finden Sie in den [Anweisungen zur Registrierung im Developer Center](#instructions-on-how-to-register-in-the-developer-center). 

#### <a name="sign-in-to-the-cloud-partner-portal"></a>Anmelden am Cloud-Partnerportal

Nachdem das Marketplace-Team Ihnen mitgeteilt hat, dass Ihr Antrag genehmigt wurde, und Sie sich beim [Microsoft Partner Network](https://partner.microsoft.com/en-us/membership/) und [Microsoft Developer Center](https://dev.windows.com) (falls erforderlich) registriert haben, wird für Sie ein Konto für den Zugriff auf das [Cloud-Partnerportal](https://cloudpartner.azure.com) erstellt. Anmeldeinformationen für die erstmalige Anmeldung sind in der E-Mail zur Genehmigung des Antrags enthalten. 

Greifen Sie mit Ihrem Marketplace-Konto (Microsoft-ID) auf Ihr Herausgeberprofil zu. Im Cloud-Partnerportal ist der letzte Schritt das Verknüpfen des Microsoft Partner Network- und Developer Center-Kontos (falls erforderlich) mit dem jeweiligen Marketplace-Herausgeberprofil für die Veröffentlichung. Hierzu können Sie im Cloud-Partnerportal in Ihrem Herausgeberprofil die Schaltfläche am unteren Rand des Bildschirms verwenden.

Ausführliche Informationen zum Verwenden des Cloud-Partnerportals erhalten Sie, wenn Sie sich im Portal im Menü [Lernen](https://cloudpartner.azure.com/#Learn) den Abschnitt **Dokumentation** durchlesen. 


## <a name="support"></a>Support

Dies ist die Liste der Supportoptionen für Azure Marketplace:

**Allgemeine Azure Marketplace-Anfragen**
|Supportkanal |BESCHREIBUNG |
|---------|---------|
|E-Mail: cloudmarketplace@microsoft.com     |  Onboarding-Supportverteilerliste. Wird für Onboardinganfragen, die Einrichtung von Discovery-Sitzungen und Architecture Design Sessions mit Partnern verwendet.        |

**Azure Marketplace-Veröffentlichungssupport**

|Supportkanal  |BESCHREIBUNG  |
|---------|---------|
|E-Mail: azurecertified@microsoft.com |   Unterstützung für Azure Marketplace-Veröffentlichungsanwendungen von Partnern. Die Geschäftszeiten sind in „Pacific Time“ angegeben.      |
|E-Mail: AzureMarketOnboard@microsoft.com |   Unterstützung für das Antragsformular und den Prozess zur Azure Marketplace-Lösung. Die Geschäftszeiten sind in „Pacific Time“ angegeben.      |
|E-Mail: Amp-testdrive@microsoft.com |   Onboardingzugriff auf Testversionen. Die Geschäftszeiten sind in „Pacific Time“ angegeben. | 

**Azure Marketplace-Portalunterstützung**

|Supportkanal  |BESCHREIBUNG  |
|---------|---------|
|E-Mail: [Support](https://go.microsoft.com/fwlink/?linkid=844975)    |   Marketplace-Veröffentlichungsportalsupport. Support ist zu jeder Zeit verfügbar (Tag und Nacht).        |

**Technischer Support**


|Supportkanal  |BESCHREIBUNG  |
|---------|---------|
|MSDN-Foren: [Marketplace](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=DataMarket)     | Microsoft Developer Network-Forum.         |
|Stack Overflow: [Azure](https://stackoverflow.com/questions/tagged/azure)     |    Stack Overflow-Umgebung zum Abrufen von Lösungen und Stellen von allgemeinen Fragen zu Azure und zum Marketplace:<ul><li>[Azure Marketplace](https://stackoverflow.com/questions/tagged/azure-marketplace)</li><li>[Azure Resource Manager](https://stackoverflow.com/questions/tagged/azure-resource-manager)</li><li>[Dokumentation zu virtuellen Computern](https://stackoverflow.com/questions/tagged/azure-virtual-machine)</li></ul> |


**Marketingressourcen**

|Supportkanal  |BESCHREIBUNG  |
|---------|---------|
|E-Mail: cosell@microsoft.com    |  Unterstützung für Onboardingprozesse und Fragen im Zusammenhang mit dem Co-Sell-Programm. Befindet sich in der Zone mit „Pacific Time“.        |
|E-Mail: gtm@microsoft.com    |  Unterstützung zu GTM-Vorteilen und Programmfragen. Die Geschäftszeiten sind in „Pacific Time“ angegeben.        |
|E-Mail: CEBrand@Microsoft.com     |  Antworten auf Fragen zur Nutzung für Azure-Logos und zum Branding.       |

## <a name="guidelines-and-how-tos"></a>Richtlinien und Leitfäden

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-an-azure-marketplace-account"></a>Richtlinien zur Erstellung einer Microsoft-ID für die Verwaltung eines Azure Marketplace-Kontos

Beachten Sie beim Erstellen eines Unternehmenskontos diese Richtlinien, falls mehrere Personen auf das Konto zugreifen müssen, indem sie sich mit dem Microsoft-Konto anmelden, das auch zum Eröffnen des Kontos verwendet wurde.

>[!IMPORTANT]
>Wenn Sie den Zugriff auf Ihr Developer Center-Konto für mehrere Benutzer zulassen möchten, empfehlen wir Ihnen, die Verwendung von Azure Active Directory, um den einzelnen Benutzern Rollen zuzuweisen. Diese Benutzer können auf das Konto zugreifen, indem sie sich mit ihren individuellen Azure AD-Anmeldeinformationen anmelden. Weitere Informationen finden Sie unter [Leitfaden zu Microsoft-IDs in einer Azure AD-Verbunddomäne](#guidance-for-microsoft-ids-in-an-azure-ad-federated-domain). Erstellen Sie Ihr Microsoft-Konto mit einer E-Mail-Adresse, die zur Domäne Ihres Unternehmens gehört, aber nicht zu einer Einzelperson. Ein Beispiel ist windowsapps@fabrikam.com.

- Beschränken Sie den Zugriff auf dieses Microsoft-Konto auf die kleinste mögliche Anzahl von Entwicklern.
- Richten Sie eine E-Mail-Verteilerliste für Ihr Unternehmen mit allen Personen ein, die auf das Entwicklerkonto zugreifen müssen, und fügen Sie diese E-Mail-Adresse Ihren Sicherheitsinformationen hinzu. So können alle Mitarbeiter auf der Liste bei Bedarf Sicherheitscodes erhalten, und die Sicherheitsinformationen Ihres Microsoft-Kontos können verwaltet werden. Falls die Einrichtung einer Verteilerliste nicht möglich ist, muss der Besitzer des einzelnen E-Mail-Kontos verfügbar sein, um bei entsprechender Aufforderung auf den Sicherheitscode zuzugreifen und ihn bereitzustellen. (Der Benutzer erhält beispielsweise eine Aufforderung, wenn dem Konto neue Sicherheitsinformationen hinzugefügt werden oder wenn darauf mit einem neuen Gerät zugegriffen werden muss.)
- Fügen Sie eine Telefonnummer hinzu, die keine Durchwahl benötigt, und auf die wichtige Teammitglieder zugreifen können.
- Sorgen Sie generell dafür, dass Entwickler vertrauenswürdige Geräte verwenden, um sich am Entwicklerkonto Ihres Unternehmens anzumelden. Alle wichtigen Teammitglieder sollten auf diese vertrauenswürdigen Geräten zugreifen können. Dieser Zugriff reduziert die Notwendigkeit, Sicherheitscodes senden zu müssen, wenn Personen auf das Konto zugreifen.
- Wenn Sie Zugriff auf das Konto von einem nicht vertrauenswürdigen PC zulassen müssen, begrenzen Sie den Zugriff auf ein Maximum von fünf Entwicklern. Im Idealfall sollten diese Entwickler mit Computern auf das Konto zugreifen, die den gleichen geografischen und Netzwerkstandort haben.
- Prüfen Sie die Sicherheitsinformationen Ihres Unternehmens regelmäßig auf der [Verwaltungsseite](https://account.live.com/proofs/Manage), um sicherzustellen, dass die Angaben aktuell sind.

Auf Ihr Entwicklerkonto sollte in erster Linie mit vertrauenswürdigen PCs zugegriffen werden. Dies ist wichtig, da die Anzahl von Codes, die pro Konto und Woche generiert werden, beschränkt ist. Darüber hinaus ermöglicht die Nutzung von vertrauenswürdigen PCs die nahtloseste Anmeldeerfahrung.

Weitere Informationen zu zusätzlichen Entwicklerkontorichtlinien und zur Sicherheit finden Sie unter [Eröffnen eines Entwicklerkontos](https://docs.microsoft.com/en-us/windows/uwp/publish/opening-a-developer-account).

### <a name="guidance-for-microsoft-ids-in-an-azure-ad-federated-domain"></a>Leitfaden zu Microsoft-IDs in einer Azure AD-Verbunddomäne

Ihr Unternehmenskonto kann mit [Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/) in einen Verbund eingefügt werden. Es wird ein Fehler zurückgegeben, wenn Sie versuchen, eine Microsoft-ID mit der E-Mail-Adresse eines Unternehmens zu erstellen. Wenn Sie eine Fehlermeldung erhalten, erkundigen Sie sich zunächst beim IT-Team, ob dies der Fall ist. Dies ist ein bekanntes Problem, und wir arbeiten an der Lösung. 

Als Problemumgehung empfehlen wir, in der Domäne @outlook.com eine neue E-Mail-Adresse und eine dazugehörige Regel zu erstellen. Folgen Sie diesen Schritten:

1. Navigieren Sie zur [Anmeldeseite](https://signup.live.com/signup), und wählen Sie die Option **Neue E-Mail-Adresse anfordern**.


2. Erstellen Sie die neue E-Mail-Adresse, und geben Sie ein Kennwort ein. In diesem Schritt werden im outlook.com-Dienst eine neue Microsoft-ID und ein E-Mail-Postfach erstellt. Setzen Sie die Registrierung fort, bis das Konto erstellt ist.

   >[!IMPORTANT]
   >Stellen Sie sicher, dass die E-Mail-ID oder Verteilerliste, die Sie zum Registrieren im Developer Center verwenden, als Microsoft-Konto registriert ist. (Wir empfehlen Ihnen die Verwendung einer Verteilerliste, um die Abhängigkeit von Personen zu beseitigen.) Falls nicht, sollten Sie die [Registrierung jetzt durchführen](https://signup.live.com/signup?uaid=e479342fe2824efeb0c3d92c8f961fd3&lic=1).
   >
   >Darüber hinaus kann für die Developer Center-Registrierung *keine E-Mail-ID unter der Microsoft-Unternehmensdomäne verwendet werden*.

3. Nachdem Sie die Microsoft-ID mit diesem Konto erstellt haben, können Sie sich am [Postfach](https://outlook.live.com/owa/) des Kontos anmelden. [Erstellen Sie für die E-Mail eine Weiterleitungsregel](https://support.office.com/en-us/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed), um alle E-Mails des Postfachs auf die E-Mail-Adresse umzustellen, die Sie in Ihrer Domäne für die Verwaltung des Azure Marketplace-Kontos erstellt haben.

Nachdem Sie den letzten Schritt abgeschlossen haben, sendet Outlook alle E-Mails/Nachrichten von der Microsoft-ID an Ihr E-Mail-Konto in Ihrer Domäne. Sie benötigen die @outlook.com-E-Mail-Adresse zum Authentifizieren im Microsoft Developer Center und im Cloud-Partnerportal.

### <a name="instructions-on-how-to-register-in-the-developer-center"></a>Anleitung zur Registrierung im Developer Center

1. Öffnen Sie eine neue Internet Explorer-InPrivate- oder Chrome-Inkognito-Browsersitzung, um sicherzustellen, dass Sie nicht bei einem persönlichen Konto angemeldet sind.
2. Navigieren Sie zum [Developer Center](http://dev.windows.com/registration?accountprogram=azure), um sich als Verkäufer zu registrieren.
3. Führen Sie alle Schritte des Assistenten **Unterstützen Sie uns beim Schutz Ihres Kontos** aus. Damit wird Ihre Identität anhand einer Telefonnummer oder E-Mail-Adresse überprüft.

   ![Kontrollkästchen für Telefoninfo im Assistenten „Unterstützen Sie uns beim Schutz Ihres Kontos“](./media/marketplace-publishers-guide/registerdevcenteremail.png)

4. Wählen Sie im Abschnitt **Registrierungskontoinformationen** im Dropdownmenü „Land/Region“ des Kontos die für Sie zutreffende Option aus.

   ![Kontoinformationen mit Kontrollkästchen für Land/Region](./media/marketplace-publishers-guide/devcenterregistrationaccountinfo.png)
   
   >[!WARNING]
   >Um Ihre Dienste im Azure Marketplace verkaufen zu können, muss die registrierte Organisation in einem der genehmigten Verkäuferländer ansässig sein. Diese Einschränkung hat auszahlungstechnische und steuerliche Gründe. Weitere Informationen finden Sie in den [Marketplace-Teilnahmerichtlinien](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

5. Wählen Sie unter **Kontotyp** den Typ **Unternehmen** aus, und klicken Sie auf die Schaltfläche **Weiter**.

   >[!IMPORTANT]
   >Unter [Kontotypen, Standorte und Gebühren](https://docs.microsoft.com/en-us/windows/uwp/publish/account-types-locations-and-fees) können Sie sich über Kontotypen und die für Sie richtige Kontotypenauswahl informieren.

6. Geben Sie unter **Anzeigename des Herausgebers** den Anzeigenamen ein (normalerweise der Name Ihres Unternehmens).

   >[!TIP]
   >Der im Developer Center eingegebene Anzeigename des Herausgebers wird im Azure Marketplace nicht angezeigt, nachdem Ihr Angebot aufgeführt wird. Sie müssen dieses Feld aber ausfüllen, um den Registrierungsvorgang abzuschließen.

7. Geben Sie unter **Kontaktinformationen** die Informationen für die Kontoüberprüfung ein.

   >[!IMPORTANT]
   >Die angegebenen Kontaktinformationen müssen genau sein. Sie werden während des Überprüfungsvorgangs verwendet, um Ihr Unternehmen im Developer Center zu genehmigen.

8. Geben Sie die Kontaktinformationen für den Genehmiger unter **Genehmiger des Unternehmens** ein. Der Genehmiger ist die Person, die gewährleisten kann, dass Sie zum Erstellen eines Kontos im Developer Center im Namen Ihres Unternehmens berechtigt sind. Wählen Sie **Weiter**, wenn Sie fertig sind.

   ![Seite „Kontoinformationen“ mit hervorgehobenen Abschnitten](./media/marketplace-publishers-guide/devcenterregistrationpayment.png)

9. Geben Sie Ihre Zahlungsinformationen für Ihr Konto ein. Wenn Sie einen Aktionscode haben, der die Kosten der Registrierung abdeckt, können Sie ihn hier eingeben. Geben Sie andernfalls Ihre Kreditkarteninformationen oder PayPal-Informationen für unterstützte Märkte an. Wählen Sie **Weiter**, wenn Sie fertig sind.

   ![Zahlungsinformationen für Developer Center](./media/marketplace-publishers-guide/devcenterregistrationpayment2.png)

10. Überprüfen Sie Ihre Kontoinformationen, und bestätigen Sie, dass alles korrekt ist. Lesen und akzeptieren Sie anschließend die Nutzungsbedingungen der Microsoft Azure Marketplace-Herausgebervereinbarung. Aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie die Bedingungen gelesen und akzeptiert haben.
11. Wählen Sie **Fertig stellen**, um die Registrierung zu bestätigen. Wir senden eine Bestätigungsnachricht an Ihre E-Mail-Adresse.
12. Wenn Sie planen, nur kostenlose Angebote zu veröffentlichen, können Sie **Go to the Cloud Partner Portal** (Zum Cloud-Partnerportal wechseln) wählen.

    Wählen Sie **Kontoinformationen aktualisieren**, wenn Sie planen, kommerzielle Angebote (Transaktion) zu veröffentlichen, z.B. VM-Angebote mit stündlicher Abrechnung. Anschließend müssen Sie die Bank- und Steuerdaten für Ihr Developer Center-Konto eingeben, wie im nächsten Abschnitt beschrieben.

#### <a name="add-bank-and-tax-information"></a>Hinzufügen der Bank- und Steuerinformationen

Wenn Sie Angebote für den Erwerb veröffentlichen möchten, müssen Sie auch Auszahlungs- und Steuerinformationen hinzufügen und zur Validierung im Developer Center absenden. Falls Sie nur kostenlose oder BYOL-Angebote veröffentlichen möchten, müssen Sie diese Informationen nicht hinzufügen. Sie können dieses Informationen später eingeben, es dauert jedoch einige Zeit, bis die Steuerinformationen validiert werden können. Wenn Sie wissen, dass Sie Angebote für den Erwerb veröffentlichen werden, sollten Sie diese Informationen so schnell wie möglich bereitstellen.

>[!IMPORTANT]
>Für kommerzielle Angebote (Transaktion) können Sie Ihre Angebote erst per Pushvorgang in die Produktionsumgebung verschieben, wenn Sie die Bank- und Steuerinformationen angegeben haben.

Fügen Sie die Bankinformationen wie folgt hinzu:
1. Melden Sie mit Ihrem Microsoft-Konto beim Microsoft Developer Center an.
2. Wählen Sie im Menü auf der linken Seite die Option **Auszahlungskonto**. Wählen Sie unter **Zahlungsmethode wählen** die Option **Bankkonto** oder **PayPal**.

   >[!IMPORTANT]
   >Wenn Sie über kommerzielle Angebote verfügen, die Kunden im Marketplace erwerben, wird dieses Konto für die entsprechenden Auszahlungen an Sie verwendet.

3. Geben Sie die Zahlungsinformationen ein, und wählen Sie **Speichern**, wenn alle Angaben stimmen.

   >[!IMPORTANT]
   >Wenn Sie Ihr Auszahlungskonto aktualisieren oder ändern müssen, können Sie die obigen Schritte erneut ausführen und die aktuellen Informationen dabei durch die neuen Informationen ersetzen. Wenn Sie Ihr Zahlungskonto ändern, kann dies dazu führen, dass sich Ihre Zahlungen um maximal einen Zahlungszyklus verzögern. Diese Verzögerung tritt auf, weil wir die Kontoänderung überprüfen müssen. Dies ist der gleiche Vorgang wie bei der ersten Einrichtung des Zahlungskontos. Ihnen wird weiterhin der volle Betrag bezahlt, nachdem Ihr Konto überprüft wurde. Alle Zahlungen, die für den aktuellen Zahlungszyklus fällig sind, werden dem nächsten Zyklus hinzugefügt.

4. Klicken Sie auf **Weiter**. 

Fügen Sie Steuerinformationen wie folgt hinzu:

1. Melden Sie mit Ihrem Microsoft-Konto am [Microsoft Developer Center](https://dev.windows.com) an (falls erforderlich).
2. Wählen Sie im Menü auf der linken Seite die Option **Steuerprofil**.
3. Wählen Sie auf der Seite **Richten Sie Ihr Steuerformular ein** das Land oder die Region aus, in dem bzw. der sich Ihr ständiger Wohnsitz befindet. Wählen Sie anschließend das Land oder die Region aus, dessen bzw. deren erste Staatsbürgerschaft Sie besitzen. Klicken Sie auf **Weiter**.
4. Geben Sie Ihre Steuerdaten ein, und wählen Sie **Weiter**.

Öffnen Sie ein Supportticket, wenn bei der Developer Center-Registrierung Probleme auftreten:

1. Navigieren Sie zur [Supportseite](https://developer.microsoft.com/windows/support).
2. Wählen Sie unter **Contact Us** (Kontakt) die Schaltfläche **Submit an incident** (Incident übermitteln).

   ![Schaltfläche „Submit an incident“ (Incident übermitteln)](./media/marketplace-publishers-guide/devcentersubmitincident.png)

3. Wählen Sie **Hilfe zu Developer Center** als **Problemtyp**, und **Veröffentlichen und Verwalten von Apps** als **Kategorie** aus. Wählen Sie anschließend die Schaltfläche **E-Mail starten**.   
4. Verwenden Sie auf der Anmeldeseite ein beliebiges Microsoft-Konto für die Anmeldung. Falls Sie kein Microsoft-Konto besitzen, können Sie über den Link ein Konto erstellen. 
5. Tragen Sie die Details des Problems ein, und übermitteln Sie das Ticket, indem Sie die Schaltfläche **Übermitteln** wählen.
