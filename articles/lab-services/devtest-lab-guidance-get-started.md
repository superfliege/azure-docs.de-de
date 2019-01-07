---
title: Erste Schritte mit Azure DevTest Labs
description: Dieser Artikel enthält die primären Szenarien zur Verwendung von Azure DevTest Labs und zwei allgemeine Pfade zur Verwendung des Diensts in Ihrer Organisation.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: spelluru
ms.openlocfilehash: 87baef8ddb5b5d8fc979ba5afb9f9b13cb4fc2ef
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52876535"
---
# <a name="get-started-with-using-azure-devtest-labs"></a>Erste Schritte mit Azure DevTest Labs
Wenn Sie DevTest Labs erkunden möchten, stehen Ihnen zwei allgemeine Pfade zur Verfügung: Proof of Concept und skalierte Bereitstellung. 

Eine **skalierte Bereitstellung** umfasst Wochen/Monate des Überprüfens und Planens mit dem Zweck, DevTest Labs für das gesamte Unternehmen mit Hunderten oder Tausenden von Entwicklern bereitzustellen. 

Eine **Proof of Concept-Bereitstellung** ist auf einen konzentrierten Einsatz eines einzelnen Teams zum Erreichen eines Mehrwerts für die Organisation beschränkt. Obwohl eine skalierte Bereitstellung verlocken kann, versagt dieser Ansatz häufiger als die Proof of Concept-Option. Aus diesem Grund empfehlen wir, klein anzufangen, aus dem ersten Team zu lernen, den gleichen Ansatz mit zwei oder drei zusätzlichen Teams zu wiederholen und dann anhand der erworbenen Kenntnisse eine skalierte Bereitstellung zu planen. Für einen erfolgreichen Proof of Concept empfehlen wir, ein oder zwei Teams auszuwählen, deren Szenarien (Entwicklungsumgebung oder Testumgebung) zu identifizieren, die aktuellen Anwendungsfälle zu dokumentieren und DevTest Labs bereitzustellen. 

## <a name="scenarios"></a>Szenarien
Es gibt drei primäre Szenarien für eine Implementierung von DevTest Labs: 

- Entwicklerdesktops
- Testumgebungen
- Labs/Schulungen/Hackathon

## <a name="developer-desktops"></a>Entwicklerdesktops
Entwickler haben oft andere Anforderungen an Entwicklungscomputer für unterschiedliche Projekte. Mit DevTest Labs können Entwickler auf bedarfsorientierte VMs zugreifen, die für ihre gängigsten Szenarien vorkonfiguriert sind. DevTest Labs bietet folgende Vorteile:

- Organisationen können gemeinsame Entwicklungs- und Testumgebungen bereitstellen, die teamübergreifende Konsistenz sicherstellen.
- Entwickler können ihre Entwicklungscomputer bei Bedarf schnell bereitstellen.
- Entwickler können Ressourcen auf Self-Service-Art bereitstellen, ohne Berechtigungen auf Abonnementebene zu erfordern.
- Die IT-Abteilung oder Administratoren können die Netzwerktopologie vorab definieren, und Entwickler können sie direkt auf einfache und intuitive Weise ohne speziellen Zugriff nutzen.
- Entwickler können ihre Entwicklungscomputer einfach nach Bedarf anpassen.
- Administratoren können die Kosten steuern, indem sie Folgendes sicherstellen:
    - Entwickler können nicht mehr virtuelle Computer erhalten, als sie für die Entwicklung benötigen.
    - Virtuelle Computer werden heruntergefahren, wenn sie nicht in Gebrauch sind.
    - Es kann nur eine Teilmenge der VM-Instanzgrößen für bestimmte Workloads zugelassen werden.

## <a name="test-environments"></a>Testumgebungen
Das Erstellen und Verwalten von Testumgebungen in einem Unternehmen kann mit einem erheblichen Aufwand verbunden sein. Mit DevTest Labs können Testumgebungen einfach erstellt, aktualisiert oder dupliziert werden, sodass Teams bei Bedarf auf eine vollständig konfigurierte Umgebung zugreifen können. In diesem Szenario bietet DevTest Labs die folgenden Vorteile:

- Tester können schnell Windows- und Linux-Umgebungen mit wiederverwendbaren Vorlagen und Artefakten bereitstellen und so die neueste Version ihrer Anwendung testen.
- Tester können ihre Auslastungstests durch Bereitstellen mehrerer Test-Agents zentral hochskalieren.
- Administratoren können das Lab mit Azure DevOps verbinden und dadurch DevOps-Szenarien ermöglichen.
- Administratoren können die Kosten steuern, indem sie Folgendes sicherstellen:
    - Tester können nicht mehr virtuelle Computer erhalten, als sie benötigen.
    - Virtuelle Computer werden heruntergefahren, wenn sie nicht in Gebrauch sind.
    - Es kann nur eine Teilmenge der VM-Instanzgrößen für bestimmte Workloads zugelassen werden.

## <a name="labs-workshops-trainings-and-hackathons"></a>Labs, Workshops, Schulungen und Hackathons  
Ein Lab in Azure DevTest Labs fungiert als idealer Container für vorübergehende Aktivitäten wie Workshops, praktische Übungen, Schulungen oder Hackathons. Mit dem Dienst können Sie in einem erstellten Lab benutzerdefinierte Vorlagen bereitstellen, mit denen jeder Teilnehmer identische und isolierte Umgebungen zu Schulungszwecken erstellen kann. Sie können durch das Anwenden von Richtlinien sicherstellen, dass Schulungsumgebungen für jeden Lab-Benutzer nur bei Bedarf verfügbar sind und genügend Ressourcen – z.B. virtuelle Computer – enthalten, die zur Schulung erforderlich sind. Schließlich können Sie das Lab problemlos für Benutzer freigeben, die dann mit einem Klick darauf zugreifen können. Nach Abschluss der Klasse kann das Lab mit allen zugehörigen Ressourcen ganz einfach gelöscht werden.


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den nächsten Artikel in dieser Reihe: [Zentrales Hochskalieren der DevTest Labs-Bereitstellung](devtest-lab-guidance-scale.md)
