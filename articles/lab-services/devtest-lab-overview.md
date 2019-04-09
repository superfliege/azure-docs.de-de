---
title: Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie DevTest Labs das Erstellen, Verwalten und Überwachen von virtuellen Azure-Computern erleichtern kann.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 599b4325e0eb9aa7f6ccca2626c6d4b14f38149d
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339328"
---
# <a name="about-azure-devtest-labs"></a>Informationen zu Azure DevTest Labs
Azure DevTest Labs ist ein Dienst, der es Entwicklern in einem Team ermöglicht, nach dem Self-Service-Prinzip virtuelle Computer und/oder PaaS-Ressourcen, die sie für Entwicklung, Tests, Training und Demos usw. benötigen, effizient bereitzustellen, ohne auf ständige Genehmigungen für die Tools zu warten, die sie brauchen. 

Das Lab besteht bereits aus vorkonfigurierten Basis-VMs oder Resource Manager-Vorlagen mit allen notwendigen Tools und Programmen, mit denen Entwickler Umgebungen erstellen können. Entwickler können so ihre Umgebungen in wenigen Minuten statt mehreren Stunden oder Tagen erstellen. 

Mit DevTest Labs können Sie die neueste Version Ihrer Anwendung testen, indem Sie die folgenden Aufgaben ausführen:

- Schnelles Bereitstellen von Linux- und Windows-Umgebungen mithilfe wiederverwendbarer Vorlagen und Artefakte
- Einfaches Integrieren Ihrer Bereitstellungspipeline in DevTest Labs, um Umgebungen bedarfsabhängig bereitzustellen
- Zentrales Hochskalieren Ihrer Auslastungstests, indem mehrere Test-Agents bereitgestellt werden, und Erstellen vorab bereitgestellter Umgebungen für Training und Demos

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

## <a name="capabilities"></a>Funktionen
DevTest Labs bietet Entwicklern, die mit virtuellen Computern arbeiten, die folgenden Möglichkeiten:

- Erstellen Sie schnell einen virtuellen Computer in nicht mehr als fünf einfachen Schritten.
- Wählen Sie aus einer übersichtlichen Liste von Basis-VMs, die vom Teamleiter oder der zentralen IT-Abteilung konfiguriert, genehmigt und autorisiert werden.
- Erstellen Sie VMs anhand von vorab erstellten benutzerdefinierten Images, bei denen sämtliche Software und Tools im Image installiert sind. 
- Erstellen Sie VMs anhand von Formeln, die im Wesentlichen aus benutzerdefinierten Images und dem neuesten Build der Software bestehen und die zum Zeitpunkt der Erstellung des virtuellen Computers installiert werden.
- Installieren Sie Artefakte, bei denen es sich um Erweiterungen handelt, die auf der VM nach ihrer Inbetriebnahme bereitgestellt werden.
- Richten Sie Zeitpläne für das automatische Herunter- und Hochfahren auf den VMs ein, damit sie am Ende des Tages heruntergefahren und am nächsten Morgen wieder hochgefahren werden.
- Übernehmen Sie einfach eine vorab erstellte VM, ohne den Prozess der VM-Erstellung durchlaufen zu müssen. 

DevTest Labs bietet Entwicklern, die mit PaaS-Umgebungen arbeiten, die folgenden Möglichkeiten:

- Erstellen Sie schnell in nicht mehr als drei einfachen Schritten PaaS-basierte Azure Resource Manager-Umgebungen.
- Wählen Sie aus einer übersichtlichen Liste von Resource Manager-Vorlagen, die vom Teamleiter oder der zentralen IT-Abteilung konfiguriert, genehmigt und autorisiert werden.
- Richten Sie eine leere Ressourcengruppe (Sandbox) mithilfe einer Resource Manager-Vorlage ein, um die gesamte Azure-Umgebung zu erkunden, während Sie im Kontext des Labs bleiben.
- DevTest Labs bietet die folgenden Vorteile beim Erstellen, Konfigurieren und Verwalten von Entwickler- und Testumgebungen in der Cloud:

Neben einem Self-Service-Modell für Entwickler im Team ermöglicht der Dienst der zentralen IT-Abteilung, die Verschwendung von Ressourcen zu kontrollieren, deren Kosten zu optimieren und das Budget einzuhalten, indem folgende Aufgaben ausgeführt werden: 

- Festlegen von Zeitplänen zum automatischen Herunter- und Hochfahren auf virtuellen Computern
- Festlegen von Richtlinien für die Anzahl virtueller Computer, die der Benutzer erstellen kann
- Festlegen von Richtlinien für die Größen der virtuellen Computer und Katalogimages, aus denen Benutzer wählen können
- Nachverfolgen von Kosten und Festlegen von Zielen für das Lab
- Erhalten von Benachrichtigungen bei hohen voraussichtlichen Kosten für das Lab, damit Sie die notwendigen Maßnahmen ergreifen können 

DevTest Labs bietet beim Erstellen, Konfigurieren und Verwalten von Umgebungen in der Cloud die folgenden Vorteile:

## <a name="cost-control-and-governance"></a>Kostenkontrolle und Governance
DevTest Labs erleichtert die Kostenkontrolle, da Sie die folgenden Aufgaben erledigen können:

- Festlegen von Richtlinien für Ihr Lab, z.B. für die Anzahl virtuellen Computer (VMs) pro Benutzer und der VMs pro Lab 
- Festlegen von Richtlinien für das automatische Herunter- und Hochfahren von VMs
- Nachverfolgen von Kosten für VM- und PaaS-Ressourcen, die sich in Ihrem Lab angesammelt haben, sodass Sie Ihr vorgegebenes Budget einhalten können 
- Ermöglichen, dass Entwickler im Kontext des Labs bleiben, sodass sie weder in der zugrundeliegenden Ressourcengruppe noch im Abonnement Ressourcen außerhalb des Labs einsetzen.

## <a name="quickly-get-to-ready-to-test"></a>Schnelles Erreichen der Testphase
DevTest Labs ermöglicht Ihnen, Umgebungen mit allen erforderlichen Elementen, die Ihr Team benötigt, um mit der Entwicklung und dem Testen von Anwendungen zu beginnen, im Voraus zu erstellen und bereitzustellen. Übernehmen Sie einfach die Umgebungen, in denen der letzte funktionsfähige Build Ihrer Anwendung installiert ist, und beginnen Sie sofort mit der Arbeit. Um die Erstellung der Umgebung zusätzlich zu beschleunigen und zu vereinfachen, können Sie alternativ auch Container verwenden.

## <a name="create-once-use-everywhere"></a>Einmal erstellen, überall verwenden
Indem Sie Paas-Umgebungsvorlagen und -Artefakte erfassen und innerhalb Ihres Teams oder Ihrer Organisation freigeben, können Sie problemlos Entwicklungs- und Testumgebungen erstellen – und das alles im Rahmen der Quellcodeverwaltung.

## <a name="worry-free-self-service"></a>Self-Service-Bereitstellung für sorgenfreies Arbeiten
Mit DevTest Labs können Ihre Entwickler und Tester schnell und einfach VM-Ressourcen (IaaS und PaaS) mit wenigen Klicks erstellen, indem sie eine Reihe von für sie vorkonfigurierten Ressourcen nutzen.

## <a name="use-iaas-and-paas-resources"></a>Verwenden von IaaS- und PaaS-Ressourcen 
DevTest Labs ermöglicht Entwicklern auch, PaaS-Ressourcen wie Web-Apps, Service Fabric-Cluster, SharePoint-Farmen usw. innerhalb des Labs mithilfe einer Resource Manager-Vorlage einzurichten. Nutzen Sie die Resource Manager-Vorlagen im Repository für unsere öffentliche Umgebung, oder verbinden Sie das Lab mit Ihrem eigenen Git-Repository, um in Labs die ersten Schritte mit PaaS zu unternehmen. Sie können auch die Kosten für diese Ressourcen nachverfolgen, um innerhalb Ihrer vorgegebenen Budgets zu bleiben. 

## <a name="integrate-with-your-existing-toolchain"></a>Integration in Ihre bestehende Toolkette
Nutzen Sie unsere sofort einsatzbereiten Plug-Ins oder unsere API, um Entwicklungs-/Testumgebungen direkt über Ihr bevorzugtes CI-Tool (Continuous Integration), Ihre bevorzugte integrierte Entwicklungsumgebung (IDE) oder Ihre bevorzugte automatisierte Veröffentlichungspipeline bereitzustellen. Darüber hinaus steht Ihnen unser umfassendes Befehlszeilentool zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln: 

- Weitere Informationen zu DevTest Labs finden Sie unter [DevTest Labs-Konzepte](devtest-lab-concepts.md).
- Eine exemplarische Vorgehensweise mit Schritt-für-Schritt-Anleitung finden Sie unter [Tutorial: Erstellen eines Labs mithilfe von Azure DevTest Labs](tutorial-create-custom-lab.md).


