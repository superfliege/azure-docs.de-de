---
title: Informationen zu Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit Lab Services das Erstellen, Verwalten und Schützen von Labs mit virtuellen Computern vereinfachen können, die von Entwicklern, Testern, Lehrkräften, Kursteilnehmern und anderen genutzt werden können.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/13/2018
ms.author: spelluru
ms.openlocfilehash: 2f4bc3bc00d1a803ed678e49df23a78e6b063e50
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957792"
---
# <a name="an-introduction-to-azure-lab-services"></a>Einführung in Azure Lab Services

Mit Azure Lab Services können Sie schnell eine Umgebung für Ihr Team (z.B. Entwicklungsumgebung, Testumgebung, Classroom-Lab-Umgebung) in der Cloud einrichten. Der Besitzer des Labs erstellt ein Lab, stellt virtuelle Windows- oder Linux-Computer bereit, installiert die erforderlichen Programme und Tools und stellt sie Lab-Benutzern zur Verfügung. Lab-Benutzer verbinden sich mit virtuellen Computern (VMs) im Lab und nutzen sie für ihre tägliche Arbeit, kurzzeitige Projekte oder für Übungen im Classroom. Sobald Benutzer die Ressourcen im Lab nutzen, kann ein Lab-Administrator die Kosten und Nutzung mehrerer Labs analysieren und übergreifende Richtlinien festlegen, um die Kosten Ihrer Organisation oder Ihres Teams zu optimieren.

> [!IMPORTANT]
> **Azure DevTest Labs** wird um neue Lab-Typen (Azure Lab Services) erweitert!
>  
> Azure Lab Services ermöglicht Ihnen, verwaltete Labs, wie z.B. Classroom-Labs, zu erstellen. Der Dienst selbst übernimmt die gesamte Infrastrukturverwaltung für ein verwaltetes Lab – vom Hochfahren der VMs über die Fehlerbehandlung bis hin zur Skalierung der Infrastruktur. Verwaltete Labs befinden sich derzeit in der Vorschauphase. Nach Ablauf der Vorschauphase werden die neuen Lab-Typen und bestehenden DevTest Labs unter dem neuen gemeinsamen Namen Azure Lab Services geführt, unter dem alle Lab-Typen weiterentwickelt werden.

## <a name="key-capabilities"></a>Wichtige Funktionen

Azure Lab Services unterstützt Folgendes:

- **Schnelle und flexible Einrichtung eines Labs**. Mithilfe von Azure Lab Services können Lab-Besitzer schnell ein Lab ihren Anforderungen entsprechend einrichten. Der Dienst bietet die Möglichkeit, alle Aufgaben im Zusammenhang mit der Azure-Infrastruktur für verwaltete Labs zu übernehmen oder es den Besitzern von Labs zu überlassen, die Infrastruktur im Rahmen ihres Abonnements selbst zu verwalten und anzupassen. Der Dienst bietet eine integrierte Skalierung und Resilienz von Infrastruktur für Labs, die er für Sie verwaltet.
- **Vereinfachte Umgebung für Lab-Benutzer**. In einem verwalteten Lab, z.B. einem Classroom-Lab, können sich Lab-Benutzer mit einem Registrierungscode registrieren und jederzeit auf das Lab zugreifen, um dessen Ressourcen zu nutzen. In einem Lab, das in DevTest Labs erstellt wurde, kann ein Lab-Besitzer Lab-Benutzern die Berechtigung erteilen, virtuelle Computer zu erstellen und darauf zuzugreifen, Datenträger zu verwalten und wiederzuverwenden und wiederverwendbare Geheimnisse einzurichten.  
- **Kostenoptimierung und -analyse**. Ein Lab-Besitzer kann Lab-Zeitpläne festlegen, gemäß denen virtuelle Computer automatisch herunter- und hochgefahren werden. Der Lab-Besitzer kann in einem Zeitplan die Zeitfenster festlegen, in denen die virtuellen Computer des Labs für Benutzer zugänglich sind, zur Kostenoptimierung Nutzungsrichtlinien lab- oder benutzerbezogen festlegen, um die Kosten zu optimieren, und Nutzungs- und Aktivitätstrends in einem Lab analysieren. Für verwaltete Labs wie z.B. Classroom-Labs steht derzeit eine kleinere Teilmenge von Optimierungs- und Analyseoptionen zur Verfügung.
- **Integrierte Sicherheit**. Ein Lab-Besitzer kann ein privates virtuelles Netzwerk und ein Subnetz für ein Lab einrichten und eine gemeinsame öffentliche IP-Adresse aktivieren. Lab-Benutzer können über virtuelle Netzwerke, die mit ExpressRoute oder Site-to-Site-VPNs konfiguriert sind, auf sichere Weise auf Ressourcen zugreifen. (derzeit in nur DevTest Labs verfügbar)
- **Integration in Ihre Workflows und Tools**. Azure Lab Services ermöglicht Ihnen, die Labs in die Website- und Verwaltungssysteme Ihres Unternehmens zu integrieren. Mithilfe Ihrer CI/CD-Tools (Continuous Integration/Continuous Deployment) können Sie Umgebungen automatisch bereitstellen. (derzeit in nur DevTest Labs verfügbar)

> [!NOTE]
> Azure Lab Services unterstützt zurzeit nur virtuelle Computer, die aus Azure Marketplace-Images erstellt werden. Wenn Sie benutzerdefinierte Images verwenden oder andere PaaS-Ressourcen in einer Laborumgebung erstellen möchten, verwenden Sie DevTest Labs. Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten Images in DevTest Labs](devtest-lab-create-custom-image-from-vm-using-portal.md) und [Erstellen von Laborumgebungen mithilfe von Resource Manager-Vorlagen](devtest-lab-create-environment-from-arm.md).

## <a name="scenarios"></a>Szenarien

Hier einige der Szenarien, die Azure Lab Services unterstützt:

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Einrichtung eines Computerlabors mit anpassbarer Größe in der Cloud für Ihren Classroom  

- Erstellen Sie ein verwaltetes Classroom-Lab. Sie geben dem Dienst einfach genau das an, was Sie brauchen. Daraufhin wird die Infrastruktur des Labs für Sie erstellt und verwaltet, sodass Sie sich auf das Unterrichten konzentrieren können, ohne auf die technischen Details eines Labs achten zu müssen.
- Stellen Sie den Teilnehmern ein Lab mit virtuellen Computern zur Verfügung, die genau für ihre Anforderungen konfiguriert sind. Weisen Sie jedem Teilnehmer eine begrenzte Anzahl von Stunden für die Nutzung der VMs für die Unterrichtsarbeit zu.  
- Verlagern Sie das physische Computerlabor Ihrer Einrichtung in die Cloud. Skalieren Sie die Anzahl der VMs automatisch nur bis zur maximalen Auslastung und Kostenschwelle, die Sie für das Lab festgelegt haben.
- Löschen Sie das Lab mit einem einzigen Mausklick, sobald Sie fertig sind.

### <a name="use-devtest-labs-for-development-environments"></a>Verwenden von DevTest Labs für Entwicklungsumgebungen

Azure DevTest Labs kann zum Implementieren vieler wichtiger Szenarien verwendet werden. Eines der primären Szenarien stellt aber die Verwendung von DevTest Labs zum Hosten von Entwicklungscomputern für Entwickler dar. In diesem Szenario bietet DevTest Labs folgende Vorteile:

- Ermöglichen, dass Entwickler ihre Entwicklungscomputer nach Bedarf bereitstellen können.
- Bereitstellen von Linux- und Windows-Umgebungen mithilfe wiederverwendbarer Vorlagen und Artefakte.
- Entwickler können ihre Entwicklungscomputer bei Bedarf einfach anpassen.
- Administratoren können die Kosten kontrollieren, indem sie sicherstellen, dass Entwickler nicht mehr VMs erhalten, als sie für die Entwicklung benötigen, und VMs heruntergefahren werden, sobald sie nicht verwendet werden.

Weitere Informationen finden Sie unter [Verwenden von DevTest Labs für die Entwicklung](devtest-lab-developer-lab.md).

### <a name="use-devtest-labs-for-test-environments"></a>Verwenden von DevTest Labs für Testumgebungen

Mit Azure DevTest Labs lassen sich zahlreiche Schlüsselszenarien implementieren. Ein primäres Szenario ist jedoch das Hosten von Computern für Tester. In diesem Szenario bietet DevTest Labs folgende Vorteile:

- Tester können schnell Windows- und Linux-Umgebungen mit wiederverwendbaren Vorlagen und Artefakten bereitstellen und so die neueste Version ihrer Anwendung testen.
- Tester können ihre Auslastungstests durch Bereitstellen mehrerer Test-Agents zentral hochskalieren.
- Administratoren können die Kosten kontrollieren, indem sie sicherstellen, dass Tester nicht mehr VMs erhalten, als sie für Tests benötigen, und VMs heruntergefahren werden, sobald sie nicht verwendet werden.

Weitere Informationen finden Sie unter [Verwenden von DevTest Labs für Tests](devtest-lab-test-env.md).

## <a name="user-profiles"></a>Benutzerprofile

In diesem Artikel werden verschiedene Benutzerprofile in Azure Lab Services beschrieben.

### <a name="lab-account-owner"></a>Lab-Kontobesitzer

Typischerweise fungiert der IT-Administrator der Cloudressourcen des Unternehmens, der das Azure-Abonnement besitzt, als Besitzer eines Lab-Kontos und erledigt die folgenden Aufgaben:

- Einrichten eines Lab-Dienstkontos für Ihre Organisation
- Verwalten und Konfigurieren von Richtlinien in allen Labs
- Erteilen von Berechtigungen an Personen in der Organisation zum Erstellen eines Lab unter dem Lab-Konto

### <a name="lab-creator"></a>Lab-Ersteller

Typischerweise erstellen Benutzer wie ein Entwicklungsleiter/-manager, Lehrer, Host eines Hackathons oder Onlinetrainer Labs unter einem Lab-Konto. Ein Lab-Ersteller führt die folgenden Aufgaben aus:

- Erstellen eines Labs
- Erstellen virtueller Computer im Lab
- Installieren der auf virtuellen Computern benötigten Software
- Angeben, wer auf das Lab zugreifen darf
- Bereitstellen eines Links zum Lab für dessen Benutzer

### <a name="lab-user"></a>Lab-Benutzer

Ein Lab-Benutzer führt die folgenden Aufgaben aus:

- Klicken auf den Registrierungslink, den der Lab-Benutzer vom Lab-Ersteller erhält, um sich beim Lab zu registrieren
- Herstellen einer Verbindung mit einem virtuellen Computer im Lab, der für Entwicklungs-, Test oder Unterrichtszwecke verwendet wird

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit der Einrichtung eines Labs mit Azure Lab Services vertraut:

- [Einrichten eines Classroom-Labs](classroom-labs/tutorial-setup-classroom-lab.md)
- [Einrichten eines Labs](tutorial-create-custom-lab.md)
