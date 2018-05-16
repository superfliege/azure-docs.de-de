---
title: Vergleichen verschiedener Arten von Labs in Azure Lab Services | Microsoft-Dokumentation
description: Hier werden verschiedene Arten von Labs erläutert und verglichen, die Sie mithilfe von Azure Lab Services (ehemals DevTest Labs) erstellen können.
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
ms.date: 04/30/2018
ms.author: spelluru
ms.openlocfilehash: 3b2fd59444f50f4ac5da6bf569cb0247f3a36943
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="compare-managed-and-custom-labs-in-azure-lab-services"></a>Vergleichen verwalteter und benutzerdefinierter Labs in Azure Lab Services
Sie können zwei Arten von Labs erstellen: **verwaltete Labs** mit Azure Lab Services und **benutzerdefinierte Labs** mit Azure DevTest Labs. Wenn Sie lediglich Ihre Lab-Anforderungen eingeben und die Einrichtung und Verwaltung der dafür erforderlichen Infrastruktur dem Dienst überlassen möchten, wählen Sie eines der **verwalteten Labs**. Aktuell können mit Azure Lab Services nur verwaltete Labs vom Typ **Classroom-Lab** erstellt werden. Wenn Sie Ihre eigene Infrastruktur verwalten möchten, erstellen Sie mithilfe von Azure DevTest Labs ein **benutzerdefiniertes Lab**.

Ausführlichere Informationen zu diesen Labs finden Sie in den folgenden Abschnitten. 

## <a name="managed-labs"></a>Verwaltete Labs
Verwaltete Labs bieten verschiedene Arten von Labs für Ihre individuellen Anforderungen. Aktuell unterstützt Azure Lab Services nur verwaltete Labs vom Typ **Classroom-Lab**. Verwaltete Labs zeichnen sich durch ihren geringen Einrichtungsaufwand aus und sind schnell einsatzbereit. Der Dienst übernimmt die gesamte Infrastrukturverwaltung für das Lab – vom Hochfahren der virtuellen Computer über die Fehlerbehandlung bis hin zur Skalierung der Infrastruktur. Wenn Sie ein verwaltetes Lab erstellen möchten, müssen Sie zunächst ein Lab-Konto für Ihre Organisation erstellen. Das Lab-Konto fungiert als zentrales Konto, unter dem alle Labs der Organisation verwaltet werden. 

Wenn Sie Azure-Ressourcen in diesen verwalteten Labs erstellen und verwenden, erstellt und verwaltet der Dienst Ressourcen in internen Microsoft-Abonnements. Sie werden nicht in Ihrem eigenen Azure-Abonnement erstellt. Der Dienst überwacht die Nutzung dieser Ressourcen in internen Microsoft-Abonnements. Diese Nutzung wird dann über Ihr Azure-Abonnement abgerechnet, dem das Lab-Konto angehört.   

Im Anschluss finden Sie einige **Anwendungsfälle für verwaltete Labs**: 

- Stellen Sie den Teilnehmern ein Lab mit virtuellen Computern zur Verfügung, die exakt für ihre Anforderungen konfiguriert sind. Weisen Sie jedem Teilnehmer eine begrenzte Anzahl von VM-Stunden für Hausaufgaben oder persönliche Projekte zu.
- Richten Sie einen Pool mit hochleistungsfähigen virtuellen Computern für rechen- oder grafikintensive Untersuchungen ein. Führen Sie die virtuellen Computer nach Bedarf aus, und bereinigen Sie sie anschließend. 
- Verlagern Sie das physische Computerlabor Ihrer Einrichtung in die Cloud. Skalieren Sie die VM-Anzahl automatisch bis zur maximalen Auslastung und Kostenschwelle, die Sie für das Lab festgelegt haben.  
- Stellen Sie schnell ein Lab mit virtuellen Computern für einen Hackathon bereit. Löschen Sie das Lab mit nur einem Mausklick, wenn Sie es nicht mehr benötigen. 


## <a name="custom-labs"></a>Benutzerdefinierte Labs
In bestimmten Szenarien kann es wünschenswert sein, die gesamte Infrastruktur und Konfiguration selbst innerhalb des eigenen Abonnements zu verwalten. Zu diesem Zweck können Sie über das Azure-Portal mithilfe von Azure DevTest Labs ein benutzerdefiniertes Lab erstellen. Für diese Labs muss kein Lab-Konto erstellt werden. Diese Labs werden nicht im Lab-Konto für die verwalteten Labs angezeigt.  

Im Anschluss finden Sie einige **Anwendungsfälle für benutzerdefinierte Labs**: 

- Stellen Sie schnell ein Lab mit virtuellen Computern für einen Hackathon oder eine Praxiseinheit für eine Konferenz bereit. Löschen Sie das Lab mit nur einem Mausklick, wenn Sie es nicht mehr benötigen. 
- Erstellen Sie einen Pool mit virtuellen Computern, die mit Ihrer Anwendung konfiguriert sind, und ermöglichen Sie Ihrem Team ganz einfach die Verwendung eines virtuellen Computers für die Fehlerbeseitigung.  
- Stellen Sie Entwicklern virtuelle Computer zur Verfügung, die mit allen benötigten Tools konfiguriert sind. Planen Sie das automatische Starten und Herunterfahren, um die Kosten zu minimieren. 
- Erstellen Sie im Rahmen Ihrer Bereitstellung wiederholt ein Lab mit Testcomputern. Testen Sie Ihre neuesten Komponenten, und bereinigen Sie die Testcomputer, wenn Sie sie nicht mehr benötigen. 
- Richten Sie mehrere unterschiedlich konfigurierte virtuelle Computer und mehrere Test-Agents für Skalierungs- und Leistungstests ein. 
- Verwenden Sie ein Lab, das mit der neuesten Version Ihres Produkts konfiguriert ist, um Schulungen für Kunden anzubieten. Geben Sie jedem Kunden ein begrenztes Zeitkontingent für die Lab-Verwendung. 


## <a name="managed-labs-vs-custom-labs"></a>Vergleich zwischen verwalteten und benutzerdefinierten Labs
In der folgenden Tabelle werden die beiden Arten von Labs verglichen, die von Azure Lab Services unterstützt werden: 

| Features | Verwaltete Labs | Benutzerdefinierte Labs |
| -------- | ----------------  | ---------- |
| Verwaltung der Azure-Infrastruktur im Lab |  Automatisch durch den Dienst | Manuelle Verwaltung  |
| Integrierte Resilienz gegen Infrastrukturprobleme | Automatisch durch den Dienst | Manuelle Verwaltung  |
| Abonnementverwaltung | Der Dienst übernimmt die Zuteilung von Ressourcen in Microsoft-Abonnements, die dem Dienst zugrunde liegen. Die Skalierung wird automatisch vom Dienst durchgeführt. | Die Verwaltung muss manuell in Ihrem eigenen Azure-Abonnement durchgeführt werden. Abonnements werden nicht automatisch skaliert. |
| Azure Resource Manager-Bereitstellung innerhalb des Labs | Nicht verfügbar | Verfügbar |

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit der Einrichtung eines Labs mit Azure Lab Services vertraut:

- [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md)
- [Einrichten eines benutzerdefinierten Labs](tutorial-create-custom-lab.md)
