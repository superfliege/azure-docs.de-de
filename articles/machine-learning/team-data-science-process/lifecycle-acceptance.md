---
title: "Kundenakzeptanzphase des Team Data Science-Prozess-Lebenszyklus – Azure | Microsoft-Dokumentation"
description: "Die Ziele, Aufgaben und Projektleistungen für die Kundenakzeptanzphase Ihrer Data Science-Projekte."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: 953f090f775da5e48b2f4ed36550a5624ae4596b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="customer-acceptance"></a>Kundenakzeptanz

In diesem Thema werden die Ziele, Aufgaben und Projektleistungen in Zusammenhang mit der **Kundenakzeptanzphase** des Team Data Science-Prozesses behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

* **Geschäftliche Aspekte**
* **Datenerfassung und -auswertung**
* **Modellierung**
* **Bereitstellung**
* **Kundenakzeptanz**

Hier ist der **Team Data Science-Prozesslebenszyklus** grafisch dargestellt. 

![TDSP-Lebenszyklus 2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Zielsetzung
* **Abschließen der Projektergebnisse**: Vergewissern Sie sich, dass die Pipeline, das Modell und die Bereitstellung in einer Produktionsumgebung die Anforderungen an die Kundenziele erfüllen.

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden zwei Hauptaufgaben durchgeführt:

* **Systemüberprüfung**: Vergewissern Sie sich, dass das bereitgestellte Modell und die Pipeline die Kundenanforderungen erfüllen.
* **Projektübergabe**: Dies ist die Übergabe an die Entität, die das System in der Produktion ausführen soll.

Der Kunde muss überprüfen, ob das System seine geschäftlichen Anforderungen erfüllt und ob die Fragen mit akzeptabler Genauigkeit beantwortet werden, damit das System in der Produktion zur Nutzung durch die Clientanwendung bereitgestellt werden kann. Die gesamte Dokumentation wird abgeschlossen und überprüft. Die Übergabe des Projekts an die Entität, die für den Betrieb zuständig ist, wird vollzogen. Diese Entität kann beispielsweise ein IT-Team bzw. Data Science-Team des Kunden oder ein Agent des Kunden sein, das bzw. der für die Ausführung des Systems in der Produktion zuständig ist. 

## <a name="artifacts"></a>Artefakte
Das Hauptartefakt, das in dieser letzten Phase produziert wird, ist **Exit Report of Project for Customer** (Abschlussbericht des Projekts für den Kunden). Dies ist der technische Bericht mit allen Details des Projekts, die zum Erlernen und Betreiben des Systems hilfreich sind. Im Rahmen des TDSP wird eine Vorlage für einen [Exit Report](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) (Abschlussbericht) bereitgestellt, die unverändert oder nach Anpassung an bestimmte Clientanforderungen verwendet werden kann. 


## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des Team Data Science-Prozess:

* [1. Geschäftliche Aspekte](lifecycle-business-understanding.md)
* [2. Datenerfassung und -auswertung](lifecycle-data.md)
* [3. Modellierung](lifecycle-modeling.md)
* [4. Bereitstellung](lifecycle-deployment.md)
* [5. Kundenakzeptanz](lifecycle-acceptance.md)

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für **bestimmte Szenarien** gezeigt werden, sind ebenfalls verfügbar. Sie sind im Thema [Exemplarische Vorgehensweisen](walkthroughs.md) aufgeführt und mit Miniaturansichtsbeschreibungen verlinkt. Sie zeigen, wie Cloud- und lokale Tools und Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele zum Ausführen der Schritte im Team Data Science-Prozess mit Azure Machine Learning Studio finden Sie im Lernpfad [Mit Azure ML](http://aka.ms/datascienceprocess).