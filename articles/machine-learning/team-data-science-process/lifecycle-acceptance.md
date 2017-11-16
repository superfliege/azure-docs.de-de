---
title: "Kundenakzeptanzphase des Team Data Science-Prozess-Lebenszyklus – Azure | Microsoft-Dokumentation"
description: "Die Ziele, Aufgaben und Projektleistungen für die Kundenakzeptanzphase Ihrer Data Science-Projekte"
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
ms.date: 11/04/2017
ms.author: bradsev;
ms.openlocfilehash: e150b3e7c7e98443264dd5b8aaeda1bfe6047b2c
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2017
---
# <a name="customer-acceptance"></a>Kundenakzeptanz

In diesem Artikel werden die Ziele, Aufgaben und Projektleistungen im Zusammenhang mit der Kundenakzeptanzphase des Team Data Science-Prozesses (TDSP) behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

   1. **Geschäftliche Aspekte**
   2. **Datenerfassung und -auswertung**
   3. **Modellierung**
   4. **Bereitstellung**
   5. **Kundenakzeptanz**

Dies ist eine visuelle Darstellung des TDSP-Lebenszyklus: 

![TDSP-Lebenszyklus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Zielsetzung
**Abschließen der Projektergebnisse:** Vergewissern Sie sich, dass die Pipeline, das Modell und die Bereitstellung in einer Produktionsumgebung die Anforderungen an die Kundenziele erfüllen.

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden zwei Hauptaufgaben durchgeführt:

   * **Systemüberprüfung:** Vergewissern Sie sich, dass das bereitgestellte Modell und die Pipeline die Kundenanforderungen erfüllen.
   * **Projektübergabe:** Übergeben Sie das Projekt an die Entität, die das System in der Produktion ausführen soll.

Der Kunde muss überprüfen, ob das System seine geschäftlichen Anforderungen erfüllt und ob die Fragen mit akzeptabler Genauigkeit beantwortet werden, damit das System in der Produktion zur Nutzung durch die Clientanwendung bereitgestellt werden kann. Die gesamte Dokumentation wird abgeschlossen und überprüft. Die Übergabe des Projekts an die Entität, die für den Betrieb zuständig ist, wird vollzogen. Diese Entität kann beispielsweise ein IT-Team bzw. Data Science-Team des Kunden oder ein Agent des Kunden sein, das bzw. der für die Ausführung des Systems in der Produktion zuständig ist. 

## <a name="artifacts"></a>Artefakte
Das Hauptartefakt, das in dieser letzten Phase produziert wird, ist **Exit report of the project for the customer** (Abschlussbericht des Projekts für den Kunden). Dieser technische Bericht enthält alle Details des Projekts, die für den Betrieb des Systems hilfreich sind. TDSP bietet dazu die Vorlage [Abschlussbericht](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) an. Sie können die Vorlage ohne Änderungen verwenden oder sie an bestimmte Clientanforderungen anpassen. 


## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des TDSP:

   1. [Geschäftliche Aspekte](lifecycle-business-understanding.md)
   2. [Datenerfassung und -auswertung](lifecycle-data.md)
   3. [Modellierung](lifecycle-modeling.md)
   4. [Bereitstellung](lifecycle-deployment.md)
   5. [Kundenakzeptanz](lifecycle-acceptance.md)

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für bestimmte Szenarien gezeigt werden, sind ebenfalls verfügbar. Der Artikel mit [exemplarischen Vorgehensweisen](walkthroughs.md) enthält eine Liste der Szenarien mit Links und Kurzbeschreibungen. Die exemplarischen Vorgehensweisen zeigen auch, wie Cloud- und lokale Tools sowie Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele für die Ausführung der Schritte in TDSPs mit Azure Machine Learning Studio finden Sie unter [Verwenden des TDSP mit Azure Machine Learning](http://aka.ms/datascienceprocess).
